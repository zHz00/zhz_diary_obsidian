CEdit 3d-look / Трёхмерный CEdit
=================================

   
 2011-11-15, 12:31   
   *How to make 3D-look for dynamically created CEdit?   
 Как сделать трёхмерный CEdit, когда он создаётся динамически?*    
   
 Read in English a little below.   
 В соответствии с тем, что я писал  [несколько постов назад](Untitled%20[008])  , стараюсь донести до других частичку своего опыта. То, на что я убил несколько часов. Ввиду особой важности (куча народа ищет ответ по интернетам), привожу опыт сразу на двух языках. Сначала на английском.   
   
 EN:   
  [(read in English)](https://zHz00.diary.ru/p169267469.htm?index=1#linkmore169267469m1)      
 Before you begin reading, you need to know, that every control in Windows is a window.   
   
 I was using MFC (waratte mo ii yo) and I had to make some CEdit edit-fields at runtime, create it dynamically. I wrote:   
 
```
dlg\_data[ix].m\_edtField->Create(  
	(bVis?WS\_VISIBLE:0)|ES\_NUMBER|ES\_RIGHT,  
	rRect,  
	this,  
	dlg\_data[ix].m\_nedtFieldID);
```
   
 where:   
 ix -- loop variable;   
 dlg\_data[ix] -- structure with some data;   
 dlg\_data[ix].m\_edtField -- pointer to CEdit control;   
 bVis -- boolean, true if CEdit must be created visible;   
 rRect -- calculated coordinates of control in CRect;   
 this -- pointer to class-variable, derieved from CWnd;   
 dlg\_data[ix].m\_nedtFieldID -- ID for control, generated at runtime.   
   
 This sentence, as I thought, created CEdit for me, but it was without border, like this:   
  ![](pics/c9dbcdf42b88.png)    
 I read some documentation and decided to use WS\_BORDER, so I modified sentense:   
 
```
dlg\_data[ix].m\_edtField->Create(  
	(bVis?WS\_VISIBLE:0)|ES\_NUMBER|ES\_RIGHT|WS\_BORDER,  
	rRect,  
	this,  
	dlg\_data[ix].m\_nedtFieldID);
```
   
 The border appeared, but it was plain, like this:   
  ![](pics/489fc6e21a76.png)    
 I wanted 3D-looking border. I read a little more, and understood, that I need to set the so-called "extended window style". The value must be WS\_EX\_CLIENTEDGE. I replaced WS\_BORDER with this:   
 
```
dlg\_data[ix].m\_edtField->Create(  
	(bVis?WS\_VISIBLE:0)|ES\_NUMBER|ES\_RIGHT|WS\_EX\_CLIENTEDGE,  
	rRect,  
	this,  
	dlg\_data[ix].m\_nedtFieldID);
```
   
   
 This had no effect. It was really stupid decision, as I figured out later. I continued to read. I switched to Google and forums.   
   
 I learned, that extended window style can be set only in special parameter by using CerateEx(...), not usual Create(...). So I wrote:   
 
```
dlg\_data[ix].m\_edtField->CreateEx(  
	WS\_EX\_CLIENTEDGE,  
	"WCLASS\_EDIT1",  
	"PREFIX\_EDIT1",  
	(bVis?WS\_VISIBLE:0)|ES\_NUMBER|ES\_RIGHT,  
	rRect,  
	this,  
	dlg\_data[ix].m\_nedtFieldID);
```
   
 This obviously won't work, because I didn't register WCLASS\_EDIT1 windows class. I tried to register it, then I tried NULL and then I tried the standard "edit" class (I must begin with it =( ):   
 
```
dlg\_data[ix].m\_edtField->CreateEx(  
	WS\_EX\_CLIENTEDGE,  
	"edit",  
	"PREFIX\_EDIT1",  
	(bVis?WS\_VISIBLE:0)|ES\_NUMBER|ES\_RIGHT,  
	rRect,  
	this,  
	dlg\_data[ix].m\_nedtFieldID);
```
   
   
 The result was the same: window creation failed. GetLastError returned 0x00000579 (0x0579, 0x579, 1041): "Invalid menu handle". Why? I have no menu in this dialog!   
   
 Then I thought that control's parent window is not my dialog and tried to set the parent window explicitly (using SetParent(...)). But I was wrong, the parent window was correct.   
   
 I gave up with CreateEx, and returned to usual Create, because I learned the great function ModifyStyleEx. This function modifies extended style on-the-fly! I decided to use it. So I wrote:   
 
```
dlg\_data[ix].m\_edtField->Create(  
	(bVis?WS\_VISIBLE:0)|ES\_NUMBER|ES\_RIGHT,  
	rRect,  
	this,  
	dlg\_data[ix].m\_nedtFieldID);  
dlg\_data[ix].m\_edtField->ModifyStyleEx(0,WS\_EX\_CLIENTEDGE);
```
   
   
 This compiled and ran normally, but the control has no border. Why? I explicitly set the border. I continued to read.   
   
  **I learned, that border style cannot be modified with ModifyStyleEx. It can be set only on creation. The reason is unknown.**    
   
 So I MUST use CreateEx(...). But windows creation fails. Why? I read forums a little more and thank godness! I found the reason. Only in one place this was described.   
   
  **CreateEx need explicitly set window style WS\_CHILD. The reason is unknown.**    
 Notice, that usual Create can be successfully called without WS\_CHILD. So, the final version was:   
   
 dlg\_data[ix].m\_edtField->CreateEx(   
 WS\_EX\_CLIENTEDGE,   
 "edit",   
 "PREFIX\_EDIT1",   
 (bVis?WS\_VISIBLE:0)|ES\_NUMBER|ES\_RIGHT|WS\_CHILD,   
 rRect,   
 this,   
 dlg\_data[ix].m\_nedtFieldID);   
   
 This works:   
  ![](pics/fd8385a631cb.png)    
   
     
 RU:   
  [(читать на русском)](https://zHz00.diary.ru/p169267469.htm?index=2#linkmore169267469m2)      
 Перед началом чтения убедитесь, что вы знаете, что каждый контрол (элемент управления) в винде -- это тоже окно, как и то, на чём он лежит.   
   
 Вы будете смеяться, но мне приходится использовать MFC (2011 год на дворе!). Задача была в том, чтобы создать на лету несколько CEdit. Я написал:   
 
```
dlg\_data[ix].m\_edtField->Create(  
	(bVis?WS\_VISIBLE:0)|ES\_NUMBER|ES\_RIGHT,  
	rRect,  
	this,  
	dlg\_data[ix].m\_nedtFieldID);
```
   
 где:   
 ix -- переменная цикла;   
 dlg\_data[ix] -- структура с данными динамических контролов;   
 dlg\_data[ix].m\_edtField -- указатель на CEdit;   
 bVis -- флаг видимости CEdit, если истина, создаём видимым;   
 rRect -- расчитанные координаты контрола, запиханные в CRect;   
 this -- указатель на переменную-класс, класс наследник CWnd;   
 dlg\_data[ix].m\_nedtFieldID -- ID контрола, сгенерированный специально для этого.   
   
 Как я и думал, эта строчка вывела CEdit, но без рамки, вот так:   
  ![](pics/c9dbcdf42b88.png)    
 Я почитал документацию и решил использовать WS\_BORDER, получилось вот что:   
 
```
dlg\_data[ix].m\_edtField->Create(  
	(bVis?WS\_VISIBLE:0)|ES\_NUMBER|ES\_RIGHT|WS\_BORDER,  
	rRect,  
	this,  
	dlg\_data[ix].m\_nedtFieldID);
```
   
   
 Рамка появилась, но была плоской, вот такой:   
  ![](pics/489fc6e21a76.png)    
 Я же хотел обычную трёхмерную. Я почитал ещё немного и понял, что мне нужно установить так называемый "расширенный стиль окна" ("extended window style"). Установить его надо в WS\_EX\_CLIENTEDGE. Я заменил WS\_BORDER:   
 
```
dlg\_data[ix].m\_edtField->Create(  
	(bVis?WS\_VISIBLE:0)|ES\_NUMBER|ES\_RIGHT|WS\_EX\_CLIENTEDGE,  
	rRect,  
	this,  
	dlg\_data[ix].m\_nedtFieldID);
```
   
   
 Ничего не изменилось. Делать так было глупо, как я понял позднее. Я продолжил чтение, переключившись с документаци на гугл и форумы.   
   
 Я узнал, что расширенный стиль окна может быть установлен, но не в том же параметре, где обычный стиль, а в другом, специальном и использовать надо CerateEx(...), а не обычный Create(...). Так что я написал:   
 
```
dlg\_data[ix].m\_edtField->CreateEx(  
	WS\_EX\_CLIENTEDGE,  
	"WCLASS\_EDIT1",  
	"PREFIX\_EDIT1",  
	(bVis?WS\_VISIBLE:0)|ES\_NUMBER|ES\_RIGHT,  
	rRect,  
	this,  
	dlg\_data[ix].m\_nedtFieldID);
```
   
 Очевидно, это не заработало, ведь я не зарегистрировал класс окна WCLASS\_EDIT1. Я попробовал его зарегистрировать, потом написать NULL, а потом я попробовал стандартный класс окна "edit" (с этого надо было начинать =( ):   
 
```
dlg\_data[ix].m\_edtField->CreateEx(  
	WS\_EX\_CLIENTEDGE,  
	"edit",  
	"PREFIX\_EDIT1",  
	(bVis?WS\_VISIBLE:0)|ES\_NUMBER|ES\_RIGHT,  
	rRect,  
	this,  
	dlg\_data[ix].m\_nedtFieldID);
```
   
   
 Результат был тот же: окно не создавалось. GetLastError вовзращал 0x00000579 (0x0579, 0x579, 1041): "Неверный дескриптор меню" ("Invalid menu handle"). С какого хрена? У меня в диалоге нет меню!   
   
 Потом я подумал, может быть родительское окно у контрола установлено неверно? Я попробовал назначить его явно с помощью SetParent(...). Я ошибался, родительское окно было установлено верно.   
   
 Я плюнул на CreateEx, и вернулся к обычному Create, ведь я узнал о замечательной функции ModifyStyleEx. Она позволяет менять расширенный стиль прямо на лету! Я решил использовать её. Я написал:   
 
```
dlg\_data[ix].m\_edtField->Create(  
	(bVis?WS\_VISIBLE:0)|ES\_NUMBER|ES\_RIGHT,  
	rRect,  
	this,  
	dlg\_data[ix].m\_nedtFieldID);  
dlg\_data[ix].m\_edtField->ModifyStyleEx(0,WS\_EX\_CLIENTEDGE);
```
   
   
 Это компилировалось и работало без ошибок, однако у контрола рамки по-прежнему не было. Почему? Я же явно указал стиль рамки. Я продолжил чтение.   
   
  **Я выяснил, что стиль рамки по неизвестной причине не может быть изменён на лету с помощью ModifyStyleEx. Его можно установить только при создании с помощью CreateEx.**    
   
 Таким образом, мне ПРИДЁТСЯ использовать CreateEx(...). Но создать окно не удаётся. Почему? Я ещё немного почитал форумы и слава богам выяснил причину. Правда написано это было только в одном месте.   
   
  **CreateEx трубует явного указания обычного стиля как WS\_CHILD. Почему -- неизвестно.**    
 Заметьте, что обычный Create может быть успешно вызван и без WS\_CHILD. Итак, финальная версия:   
   
 
```
dlg\_data[ix].m\_edtField->CreateEx(  
	WS\_EX\_CLIENTEDGE,  
	"edit",  
	"PREFIX\_EDIT1",  
	(bVis?WS\_VISIBLE:0)|ES\_NUMBER|ES\_RIGHT|WS\_CHILD,  
	rRect,  
	this,  
	dlg\_data[ix].m\_nedtFieldID);
```
   
   
 Это работает:   
  ![](pics/fd8385a631cb.png)    
     
    
 <https://diary.ru/~zHz00/p169267469_cedit-3d-look-tryohmernyj-cedit.htm>   
   
 Теги:   
 [[Программирование]]   
 [[Борьба с техникой]]   
 [[Статьи]]   
 ID: p169267469