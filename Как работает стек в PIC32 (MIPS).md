Как работает стек в PIC32 (MIPS)?
=================================

  
2018-05-24, 23:59  
 По-видимому кроме STM32 (ARM) теперь я буду работать ещё и с PIC32 (MIPS). Стал разбираться, что к чему. И что-то оказалось, что в MIPS всё не как у людей. Про статусное слово процессора ничего не видно. Про стек ничего не видно. Что происходит вообще?   
   
 Со статусным словом я потом разобрался -- оно хранится в сопроцессоре! И в нём нету флагов результатов арифметической операции. Но ладно, команды ветвления и без этого работают, проверяя результат напрямую.   
   
 А вот со стеком... в общем, стека в MIPS действительно нет. Программист, если хочет, может реализовать его в виде программной эмуляции. Ясное дело, что стек всем нужен, поэтому компиляторы Си автоматически делают эту эмуляцию. Выглядит это примерно так:   
   
 Каждый раз, когда нужен push:   
   
 1. Вычесть из регистра, который назначен регистром стека, четыре.   
 2. Положить по адресу в этом регистре нужное число.   
   
 Каждый раз, когда нужен pop:   
   
 1. Забрать по адресу из регистра число.   
 2. Прибавить к регистру четыре.   
   
 ААА!   
   
 Из-за этого оверхеда один товарищ, который давно с pic32 работает, старается локальные переменные вынести по максимому в глобальные. Типа для скорости. Потому что локальные переменные хранятся на стеке. Хотя так ли велика потеря?   
   
 
>   **UPD.**  Потеря такова.   
>    
>  1. Согласно документации, пуш и поп в STM32 (ARM) занимают 1+N тактов, где N -- число сохраняемых регистров (они задаются списком). См, например, Cortex M-4 r0p0 Technical Reference Manual, Issue B, p. 3-6, Table 3-1.   
>  2. Судя по описанию работы конвейера в PIC32, все команды кроме команд умножения, деления и FPU выполняются за один такт. Хотя мне не удалось найти, где про это написано явно.   
>    
>  Таким образом, оверхед при сохранении/загрузке одного регистра одинаковый (без учёта конвейерной оптимизации). Если регистров несколько, то у STM32 (ARM) небольшое преимущество, однако:   
>  1. В зависимости от реализации конвейера в конкретном PIC32 а также получившегося кода общее время выполнения при той же частоте у PIC32 может оказаться даже меньше.   
>  2. Далеко не вся работа со стеком заключается в использовании пуш и поп. Если локальная переменная хранится в стеке, то обращение к ней будет просто load/store командой, которая занимает 2 такта в STM32 (ARM) и, по-видимому, 1 такт в PIC32. Кроме того, работа с переменными в стеке в таком случае не будет отличаться от работы с глобальными переменными (по времени).   
>  3. При таком большом количестве регистров общего назначения появляется возможность размещать часть локальных переменных в регистрах процессора, а не в стеке. И у PIC32 возможности тут шире, т.к. регистров больше.   
>    
>  Таким образом, отказ от локальных переменных в пользу глобальных для ускорения работы программы необоснован. 

   
   
 Вот я не знаю, может быть, я что-то не понимаю в архитектурах, но почему нельзя было сделать встроенные пуш и поп? Что-что, говорите? Потому что это RISC? Так ARM тоже RISC. И там есть отличные пуш и поп.   
  
<https://diary.ru/~zHz00/p215518798_kak-rabotaet-stek-v-pic32-mips.htm>  
  
Теги:  
[[Крик души]]  
[[Программирование]]  
[[Говнокод]]  
[[Борьба с техникой]]  
ID: p215518798  


(Комментариев нет)
------------------