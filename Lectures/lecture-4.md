 # Микропроцессоры
[⬅️ Предыдущая лекция](lecture-3.md)
[📚 Вернуться к оглавлению](../README.md)
[➡️ Следующая лекция](lecture-5.md)

## Содержание
- [Структура процессора](#структура-процессора)
	- [Введение](#введение)
	- [Обозначения](#обозначения)
	- [Устройство управления (УУ)](#устройство-управления-уу)
	- [Арифметико-логическое устройство (АЛУ)](#арифметико-логическое-устройство-алу)
	- [Микропроцессорная память (МПП)](#микропроцессорная-память-мпп)
	- [Интерфейсная система](#интерфейсная-система)
 - [Архитектуры CISC и RISC](#архитектуры-cisc-и-risc)
 - [Параллелизация вычислений](#параллелизация-вычислений)
	- [Конвейеры](#конвейеры)
	- [Суперскалярные процессоры и VLIW](#суперскалярные-процессоры-и-vliw)
	    - [OFFTOP - Легенда о капитане Итанике](#offtop---легенда-о-капитане-итанике)
	- [Многоядерные и многопроцессорные системы](#многоядерные-и-многопроцессорные-системы)
	- [Векторные процессоры](#векторные-процессоры)
 - [Дополнительные материалы](#дополнительные-материалы)

## Структура процессора
### Введение
<img width="644" alt="image" src="https://github.com/user-attachments/assets/c808a3c4-f1a2-4af5-9d80-88aa84aa158b">

Микропроцессор (CPU):
- **CPU** - **C**entral **P**rocessing **U**nit
- Функционально-законченное программно-управляемое устройство обработки информации, выполненное в виде одной или нескольких больших (БИС) или сверхбольших (СБИС) интегральных схем.
- Центральное устройство вычислителя, предназначенное для управления работой блоков машины и для выполнения арифметических и логических операций над информацией

Интегральная схема называется большой, если она содержит более 1000 транзисторов, и сверхбольшой, если больше 10000 (в современных процессоров число транзисторов исчисляется миллиардами).

### Обозначения
- **ЦП** — центральный процессор
- **ГТИ** — генератор тактовых импульсов
- **АЛУ** — арифметико-логическое устройство
- **УУ** — устройство управления
- **МПП** — микропроцессорная память
- **МСП** — математический сопроцессор
- **СШ** — системная шина
- **СА** — сетевой адаптер
- **L1**, **L2**, **L3** — кэщи
- **I** — интерфейсная система
- **РК** — регистр команд
- **КОП** - код операции
- **ПЗУ** — постоянное запоминающее устройство


### Устройство управления (УУ)
«Задача УУ заключается в преобразовании кода операции (КОП) в управляющие сигналы для конкретных аппаратных схем, выполняющих данные операции»(c)

Декодер преобразует код операции в унарный код, чтобы определить куда подавать сигнал для операции (см. [лекцию №3](lecture-3.md#кодер-мультиплексор-демультиплексор))
- Формирует и подает во все блоки машины в нужные моменты времени определенные сигналы управления (управляющие импульсы), обусловленные спецификой выполняемой операции и результатамипредыдущих операций; 
- Формирует адреса ячеек памяти, используемых выполняемой операцией, и передает эти адреса в соответствующие блоки компьютера; 
- Опорную последовательность импульсов получает от ГТИ.

<img width="361" alt="image" src="https://github.com/user-attachments/assets/aa53a515-a6eb-4a32-886b-5bd19b3d8b41">

ПЗУ микропрограмм может быть комбинационным или последовательностным (если используется микрокод)

**Микрокод** – построение сложных машинных команд на основе простых команд.

«Микрокод — это аналог функций в языках программирования высокого уровня. Когда можно на вход подать команду, которая подразумевает выполнение серии более простых команд, требующих память. Это требует уже не комбинационной схемы, а последовательностной, которая будет управлять комбинационными схемами. Примером может служить умножение матриц, сначала вызывается множество умножений, потом результаты многократно сложить. Всем этим надо как-то управлять»


<img width="400" alt="image" src="https://github.com/user-attachments/assets/ac5e8aac-541b-4781-9080-df27466b92bc">

«На вход подается цифровое обозначение выполняемой команды (КОП) получаем множество проводков, каждый из которых говорит, что надо сделать в конкретной операции. Дальше у нас идет модуль микропрограммы, но это не цельное устройство, здесь прописывается, что делается при срабатывании каждого из проводков. Если у нас что-то простое RISC-овское, то это будет много комбинационных схем. Если это микрокод — последовательностная схема, вызывающая комбинационные» (c)

«Множество исходных сигналов, которые непосредственно воздействуют на АЛУ в процессе выполнения команд и будут образовывать шину команд (ШК)»(c)

### Арифметико-логическое устройство (АЛУ)
АЛУ необходима для выполнение арифметических и логических операций над числовой и символьной информацией.

**Математический сопроцессор** – отдельное устройство (может быть в рамках той же схемы или отдельно) для ускорения операций с плавающей запятой.

Операции, которые может выполнять АЛУ
1) Арифметические операции
    - Сложение/вычитание/умножение/деление (реализация рассматривалась в [лекции №3](lecture-3.md##аппаратная-реализация-арифметических-операций)
    - С фиксированной/плавающей запятой
    - Над двоичным/двоично-десятичным/индексным кодом
2) Логические операции
3) Циклические сдвиги
4) Операции над алфавитно-цифровыми полями

<img width="400" alt="image" src="https://github.com/user-attachments/assets/d27f9700-9677-45fb-a916-43112a9559d3">

**Комментарии лектора**:
- В фон-Неймановской архитектуре по одной и той же шине подают и команды, и данные.
- У второго регистра стрелка идет только в одну сторону, так как из него мы только считываем, но не записываем
- Сумматор на схеме приведен к примеру, там может быть любая операция, заданная на шине команд. На лекции элемент обозначался как EXE (от англ. execute)
- Алгоритм простой: данные считываются из первого и второго регистра, выполняется операция, заданная на ШК, результат записывается обратно в первый регистр
- Так как мы работаем с регистрами, а не с последовательным кодом, то сумматор будет параллельный
- Умножение реализуется стандартно методом сложения и сдвига (основан на сумматорах, занимает несколько тактов)
- Аппаратные делители существуют, реализуют деление столбиком, либо в простейшем случае вычитание с подсчетом количества
- Операции могут выполняться как в двоичном, так и в двоично-десятичном (когда каждые четыре бита кодируют одну десятичную цифру)
- Циклические сдвиги и битовые перестановки очень полезны для шифрования
- «Алфавитно-цифровые поля» означает, что мы рассматриваем поля, как какие-то символы
- В данных могут лежать адреса других данных
- Это был общий обзор, что такое процессор «как это понимали в 40-е и 50-е годы» и реальность 60-ых/70-ых
- Эта схема показывает выполнение одной ассемблерной команды

### Микропроцессорная память (МПП)
Набор регистров различной длины для операций высокого быстродействия.

Регистры образуют регистровый файл процессора, это самое быстрое что есть. Регистры, как было сказано, в [лекции №3](lecture-3.md) построены на триггерах. Это так называемая SRAM (об этом в следующей лекции)

**Кэш-память** – дополнительная схема с дополнительными регистрами



Кэши могут быть разных уровней:
- **L1** - по кэшу на каждое ядро, реализованы на кристалле процессора
- **L2** – общий на все ядра, реализован на кристалле 
- **L3** - общий на все ядра, отдельное устройство
  
Порядок объема:
- Регистровый файл ~6 КБ
- L1 ~32 КБ
- L2 ~256КБ
- L3 ~64 МБ

### Интерфейсная система
Интерфейсная система нужна для сопряжения и связи с другими устройствами.

Включает внутренний интерфейс МП, буферные запоминающие регистры, а также схемы управления портами ввода-вывода и системной шиной.

**Интерфейс** (interface) — совокупность средств сопряжения и связи устройств компьютера, обеспечивающая их эффективное взаимодействие.

**Порты ввода-вывода** (I/O ports) — элементы системного интерфейса ПК, через которые МП обменивается информацией с другими устройствами.

## Архитектуры CISC и RISC
**CISC** - **C**omplex **I**nstruction **S**et **I**nstruction.

**RISC** - **R**educed **I**nstruction **S**et **I**nstruction.

Данное разделение было актуальным в 1980-е, когда площадь микросхемы процессора оставалось ограниченной и требовался баланс между числом команд и регистров

CISC использует много сложных команд (20% площади схемы), имеет мало регистров (обычно 8) и использует микрокод. Если в какой-то из внутренних инструкций микрокода возникло исключение, его все равно нужно отработать до конца.

> [!NOTE]
> Всё началось в 1960-х. Поначалу программисты работали с машинным кодом, то есть реально писали нолики и единички. Это быстро всех достало и появился Assembler. Низкоуровневый язык программирования, который позволял писать простые команды типа сложить, скопировать и прочее. Но программировать на Assembler'е тоже было несладко. Потому как приходилось буквально “за ручку” поэтапно описывать процессору каждое его действие.
> Поэтому, если бы вы ужинали с процессором, и попросили передать его вам соль, это выглядело бы так:
> 
> Эй процессор, посмотри в центр стола.
> 
> Видишь соль? Возьми её.
> 
> Теперь посмотри на меня.
> 
> Отдай мне соль. — Ага, спасибо!
> 
> А теперь снова возьми у меня соль.
> 
> Поставь её откуда взял
> 
> Спасибо большое! Продолжай свои дела.
> 
> Кхм… Процессор, видишь перец?
> 
> И так далее....
> 
> В какой-то момент это всё задолбало программистов. И они решили: Хей, а почему бы нам просто не не написать инструкцию «Передай мне соль»? Так и сделали. Набор таких комплексных инструкций назвали CISC.



RISC использует минимум команд и имеет много регистров (обычно 32). При равной тактовой частоту у RISC лучшее быстродействие за счеткраткого выполнения простых команд.

![image](https://github.com/user-attachments/assets/d6e677d0-feeb-4c03-a325-becfbb87bea7)

В условиях роста тактовой частоты количество тактов на команду стало менее критичным.

С увеличением емкости микросхем проблема хранения команд стала менее острой и «RISC»-процессоры тоже стали использовать микрокод.
Микросхемы стали оснащаться большими L1-кэшами.
Сильно увеличивать число регистрового файла смысла нет, так как все равно при исполнении высокоуровневых программ их надо кэшировать при вызове каждой внутренней функции.
Процессоры с регистровыми окнами – регистров много, каждой функции выделяется подмножество («окно»); по мере роста стека данные могут выталкиваться в кэш и подгружаться из него (пример AMD29K – 5 окон на 128 регистрах)

Дальнейшее развитие – использование параллелизма

## Параллелизация вычислений
### Конвейеры
**Конвейер** (pipeline) – способ частичной параллелизации вычислений, когда однотипные цепочки инструкций выполняются пошагово, так что одновременно выполняются несколько инструкций из разных цепочек.

Конвейер может быть недозагруженным (underutilized) – в один момент времени могут выполняться не все инструкции из последовательности.

**Глубокий конвейер** – конвейер на некоторых процессорах с большим числом шагов (>20)

Уровни параллелизма ВС:
1. Конвейерная обработка
2. Суперскалярность
3. Многоядерность
4. Многопроцессорность
5. Массовый параллелизм

Дополнительно:
Векторная обработка(в т.ч. на GPU)

Пример для RISC:
- **IF**(Instruction Fetch) — считывание команды в процессор;
- **ID** (Instruction Decoding) - декодирование команды;
- **EX** (Execution) - выполнение команды
- **MEM** (Memory) – выделение памяти
- **WB** (Write Back) - запись результата

«Эти блоки функционально не зависят друг от друга, что позоляет перейти к концепции конвейера»

<img width="506" alt="image" src="https://github.com/user-attachments/assets/c3ef44a1-2c28-47a3-98c1-360d6ca33e4b">

Пятиступенчатый конвейер 
![image](https://github.com/user-attachments/assets/70f9cc06-f30b-461a-a512-d136342c7da9)

Конвейер может быть недозагруженным (underutilized) – одновременно выполняются не все инструкции последовательности.
При работе конвейера бывают конфликты – ситуации, препятствующие полной загрузке.

**Структурные конфликты** – общая архитектура не позволяет выполнять все инструкции одновременно.

**Конфликты по данным** – для начальных инструкций следующей операции требуется результат предыдущей.

**Конфликты по управлению** – связаны с выбором следующей операции по условному оператору. Для предотвращения используется предсказание ветвлений (branch predictor)

**Пузырек** (bubble) – холостая операция, прогоняемая через конвейер для разрешения конфликта. В ассемблере команда nop или noop (no operation, ничего не делать)

### Суперскалярные процессоры и VLIW
Суперскалярность и параллелизм узлов 
![image](https://github.com/user-attachments/assets/845dad9f-6de7-4f48-9eac-713103a97fb5)
Параллелизовать можно только независимые инструкции

Распределение команд по конвейерам с помощью специальной аппаратной схемы – диспетчера инструкций

На практике осуществляется выполнение до 8 инструкций одновременно. Дальше – слишком сложная логика распределения инструкций

Диспетчер инструкций – аппаратная схема, которая читает инструкции из памяти, определяет возможность распараллеливания, распределяет по узлам для выполнения

Процессоры с точки зрения обработки данных:
1. Скалярные – одна инструкция над юнитом данных
2. Суперскалярные – несколько инструкций, каждая над своим юнитом
3. Векторные – одна инструкция над массивом 

**VLIW** - **V**ery **L**ong **I**nstruction **W**ord.

Если в суперскалярном процессоре распределение инструкций делает специализированное устройство, то во VLIW – компилятор

При компилировании генерируется макрокоманда `bundle`, представляющая список микрокоманд, выполняемых каждым узлом.

Недостаток – сложность компилирования.

Пример «лайтового» варианта VLIW – Intel Itanium (архитектура EPIC) – три команды за такт

#### OFFTOP - Легенда о капитане Итанике

Хотя Itanium нарекли Титаником сразу же после анонса архитектуры 4 октября 1999го, он не был поначалу и вполовину 
так плох, как его реноме. Архитектура VLIW/EPIC смотрелась необычно по сравнению с CISC и манила новыми возможностями.
Мою фантазию будоражили предикатное исполнение, вращающиеся регистры и explicit software pipelining.
    
> [!NOTE]
> Продолжение под спойлером

<details>
<summary>Валерий Черепенников - Глава «Гибель Титаника» из книги «Made at Intel»</summary>

К тому же IA-64 была in-order архитектурой – можно было точно предсказать сколько будет обрабатываться один элемент достаточно 
длинного цикла при условии прогретых кэшей. Для кого как, а для меня эта “иллюзия контроля” почему-то всегда была 
важна. Тогда я еще плохо представлял себе важность software ecosystem для успеха платформы. Да, понимал, что работа
предстоит огромная, но шансы представлялись вполне себе неплохими.
       
Но все же Itanium, как и Титаник, видимо, был проклят с самого начала. Дело в том, что против него играли как
религия (not invented here!) так и политика. А в средневековом государстве – это необоримая сила. “Крестным отцом”
Itanium был Mike Fister, тогдашний глава серверного подразделения Intel. И в начале 2000х между ним и Полом 
Отеллини развернулась борьба за то, кто станет следующим  CEO Intel после Kрейга Баррета. Борьбу эту Captain 
Itanic проиграл и ушел в CEO в Cadence (который, безусловно уважаемая компания, но все же не Intel). Также ко дну 
пошло его детище. А спасать было некому - Отеллини Itanium не жаловал. Уж не знаю вследствие “разборок” начала 
2000х или по каким то другим причинам... К тому же обнаружилась масса других проблем.
    
Индустрия как-то сразу не поверила в Itanium. Портирование софта шло без особого энтузиазма. А Intel не решился на 
большую ставку – Itanium enabling strategy всегда оставляла у меня ощущение какой то недосказанности...
Возможно, расчет был на  x86 compatibility block, но именно он стал больным местом Itanium – энергии потреблял больше 
чем весь остальной процессор и грелся как сволочь. Бинарный транслятор также не выглядел панацеей: преобразование из
CISCа во VLIW является одним из самых сложных (хотя на Эльбрусе как то работает)...
    
Насколько увлекательным являлось написание микрокернелов для Itanium на ассемблере – настолько кошмарным было 
портирование приложений. Компилятор является основным камнем преткновения для архитектуры VLIW/EPIC. Одно из немногих 
исключений, которое я знаю – опять же Эльбрус. Но для того чтобы довести его компилятор до ума потребовалось порядка 
20 лет. Интел столько ждать не захотел...
Ну и последнее – Itanium всегда выпускался с отставанием на шаг по техпроцессу от x86. И в этом трудно не усмотреть
наличие “доброй” политической воли.
    

IA-64 влачила жалкое существование до начала 20х. И лишь в феврале прошлого года  Linus Torvalds сказал “It's dead, 
Jim." Но можно было спокойно сделать это и на 10 лет раньше. И все же у меня осталось от Itanium ощущение “неспетой 
песни”. Да, я не люблю VLIW (я тоже религиозен ?) и мне кажется, что рано или поздно мы бы все равно “уперлись” в его 
ограничения. Но все же стоило пытаться по-честному пройти этот путь ...

</details>
    
«Хардкорные» варианты VLIW (десятки команд за такт) – экзотика (цифровые сигнальные процессоры, «Эльбрус»)

### Многоядерные и многопроцессорные системы 
Разные вычислительные ядра могут параллельно выполнять разные потоки.
При конвейерных и суперскалярных вычислениях в рамках одного ядра распараллеливаются независимые команды в рамках одного потока; многопотоковость реализуется через выделение потокам временных слотов, при завершении которых состояние потока кэшируется.

Кэш L1 отдельный для каждого ядра, а L2 – общий. В многоядерной системе вычислительные ядра реализованы на одном микрочипе, в многопроцессорной – на разных физических платах.

**Массово-параллельная обработка** (massive-parallel processing) - физически разделены не только процессоры, но и память.
На каждом узле самостоятельная операционная система (если есть главный узел, то на остальных может быть сильно урезанная версия)

### Векторные процессоры
Одна команда выполняется над соответствующими элементами из массивов операндов

**Графический процессор** (Graphical Processing Unit, GPU) – изначально предназанчен для обработки выводимой графической информации (массив – пиксели изображения)

Общие вычисления на графическом процессоре (General Purpose computations on GPU, GPGPU) – векторную архитектуру графических процессоров можно использовать для параллельных вычислений (например, для обучения нейросетей)

Пример - CUDA, с 2007.

Видеопамять и шина видеоданных – специализированные схемы для быстрого доступа графического процессора к необходимым данным.

[⬅️ Предыдущая лекция](lecture-3.md)
[📚 Вернуться к оглавлению](../README.md)
[➡️ Следующая лекция](lecture-5.md)

## Дополнительные материалы
- [Презентация](https://github.com/user-attachments/files/18033436/_04_.pdf)
- [Оригинал истории капитана Итаника](https://habr.com/ru/articles/664682/)
- [Про конвейер](https://courses.cs.washington.edu/courses/cse410/10sp/lectures/09-pipelining-1.pdf)
- [МФТИ-шная лекция](https://www.youtube.com/watch?v=LM6TjTI5K_Y&list=PLtWzSku1VHKDOr59H6S8rxhFMwoIXZj0o)
- [CISC/RISC](https://habr.com/ru/companies/selectel/articles/542074/)
- [Еще о конвейере](https://habr.com/ru/articles/182002/)
- [О кэшах](https://habr.com/ru/companies/vdsina/articles/515660/)
- [Основы](https://dzen.ru/a/ZKuC0WuR2WXPQZJr)
- [Первоисточник истории про соль и CISC](https://habr.com/ru/companies/droider/articles/519732/)
