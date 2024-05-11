SeaMonkey high CPU usage / SeaMonkey жрёт процессор
===================================================

  
2024-04-01, 04:37  
 (EN)   
   
 My computer is working for months, but i have to restart my browser from time to time. I have several browsers, but SeaMonkey (based on Gecko engine) is one of my primary. I had several troubles with it, but most annoying for now is high CPU and RAM usage. Several days of work and VOILA, it takes 5-6 GiB of RAM. Surely, this is not very large amount for modern browser. But the other issue is freezing. In this state browser is not responding for 5+ seconds when I try to press buttons etc.   
   
 Also, I discovered that Task Manager shows full usage of one of the CPU cores. How to find out the reason of high CPU and RAM usage?   
   
 I asked Google and tried to use special page, about:memory . Reports on this page lack details, but that was better than nothing. In addition I discovered strange side effect.   
   
 I pressed "Measure" button, and soon after I found out that CPU usage became zero, and 1 to 2 GiB of memory was freed. So, the problem was solved by "observation" attempt.   
   
 (RU)   
   
 Компьютер у меня не выключается месяцами, но браузер приходится периодически перезапускать. Браузеров у меня несколько, и один из основных -- SeaMonkey (движок Gecko). С ним у меня было несколько страданий, но одно из наиболее докучливых заключалось в том, что через несколько суток работы он не только начинал жрать по 5-6 гб памяти (что совсем немного для современного браузера), но ещё и начинал подтормаживать. Можно было по 5 секунд ждать, пока кнопки будут снова нажиматься, а курсор переставляться.   
   
 Таск менеджер при этом начинал показывать, что одно ядро полностью занято браузером. Но как понять, какая сущность внутри браузера жрёт процессор и память?   
   
 Некоторое гугление навело меня на вкладку about:memory . Отчёт там не очень подробный, но лучше чем ничего. Странным оказалось другое.   
   
 При попытке запроса отчёта (кнопка Measure) у меня не только освободилось значительное количество памяти, но и потребление процессора упало до нуля. Таким образом, в очередной раз было продемонстрировано, что процесс измерения влияет на явление.   
  
<https://diary.ru/~zHz00/p221966316_seamonkey-high-cpu-usage-seamonkey-zhryot-processor.htm>  
  
Теги:  
[[Лайфхак]]  
[[Программы]]  
[[Борьба с техникой]]  
ID: p221966316  


Комментарии: 5
--------------

  


---



|         #         |              Дата              |                     Автор                     |           ID           |
| --- | --- | --- | --- |
| (1/5) | 2024-04-01, 21:26 | anhelmoders | c758122181 |

  
 Ты выходишь на международный уровень?   
 ^c758122181

---



|         #         |              Дата              |                     Автор                     |           ID           |
| --- | --- | --- | --- |
| (2/5) | 2024-04-01, 21:59 | zHz00 | c758122316 |

  
  [anhelmoders](https://anhelmoders.diary.ru "No plans. Only wonders.")  , я счёл, что пользователей SeaMonkey так мало, что хоть кто-то сможет найти этот пост только если он будет на английском.   
 ^c758122316

---



|         #         |              Дата              |                     Автор                     |           ID           |
| --- | --- | --- | --- |
| (3/5) | 2024-04-01, 22:01 | CD\_Eater | c758122329 |

  
 не знаю что такое морская обезьяна, но геккон - это движок, использующийся в огненном лисе   
 то есть, в ФФ тоже есть эта полезная about memory (и эти проблемы с памятью)   
 шпасиба за информацию   
 ^c758122329

---



|         #         |              Дата              |                     Автор                     |           ID           |
| --- | --- | --- | --- |
| (4/5) | 2024-04-02, 12:16 | RetXiRT suiR@ttig@$ | c758125465 |

  
 Собрала операционка все браузеры и говорит:   
 - Кто будет втихаря жрать ресурсы, сразу буду бить по наглой обезьяньей морде!   
 -------------------------------------------------------------------------------------------------------------   
 There's only one explanation to this. I'm God. (c) Supernatural.   
 ^c758125465

---



|         #         |              Дата              |                     Автор                     |           ID           |
| --- | --- | --- | --- |
| (5/5) | 2024-04-02, 17:32 | anhelmoders | c758127246 |

  
  [zHz00](https://zHz00.diary.ru "Untitled")  , предполагала что-то подобное)   
 ^c758127246