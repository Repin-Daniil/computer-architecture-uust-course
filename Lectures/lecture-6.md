# Сети
[⬅️ Предыдущая лекция](lecture-5.md)
[📚 Вернуться к оглавлению](../README.md)
[➡️ Консультация](../Exam/consultation.md)


## Содержание
- [Основы](#основы)
 	- [Кодирование](#кодирование)
  - [Характеристики скорости передачи данных](#характеристики-скорости-передачи-данных)
  - [Модуляция](#модуляция)
  - [Режимы передачи](#режимы-передачи)
  - [Разделение доступа](#разделение-доступа)
 - [Дополнительные материалы](#источники)

# Основы
## Кодирование
**Кодирование** — это процесс представление информации в виде кода. Обратное преобразование называется **декодированием**

Цифровое кодирование дискретных данных осуществляется с использованием потенциальных или импульсных кодов. Для представления двоичных нулей и единиц в потенциальных кодах используются разные значения потенциала сигнала, а в импульсных кодах — импульсы разной полярности или перепады потенциала

На рисунке ниже изображено кодирование 1011 двумя способами
![Screenshot 2024-12-22 at 16 18 08](https://github.com/user-attachments/assets/c66e1ce4-7b0e-4b96-8301-564d3bad7a28)

**Потенциальное кодирование**

В случае потенциального кодирования уровень сигнала задается напряжением. Есть уровень сигнала 1 и 0, за один такт передается один бит. Реализовать можно через сдвиговый регистр, работающий на частоте канала связи

**Манчестерское кодирование**
> [!warning]
> В лекции была допущена оговорка, лектор имел в виду «Манчестерское»

Для кодирования используются два уровня сигнала, при этом для представления двоичных единиц и нулей используется переход сигнала в середине каждого битового интервала:
 - двоичной «1» соответствует переход от высокого уровня сигнала к низкому;
 - двоичному «0» – переходом от низкого уровня сигнала к высокому.

В случае последовательности из нескольких единиц или нулей в начале каждого битового интервала происходит дополнительный служебный переход сигнала.
Применяется в технологиях Ethernet и Token Ring.
>[!NOTE]
>Можно делать разные градации и передавать за один такт больше одного бита, как во FLASH-накопителях из [прошлый лекции](lecture-5.md)


## Характеристики скорости передачи данных
Для описания скорости передачи данных  вводятся два понятия

- **Бод** (англ. baud) — единица измерения символьной скорости, количество переключений в секунду. Названа в честь французского инженера Эмиля Бодо. Может быть знакома читателю по работе с ардуинкой в прошлом семестре
- **Бит в секунду** -  без комментариев

**Пример:** Если у нас есть 8 уровней и 1000 бод. По формуле $N = 2^{i}$ получаем, что за один такт можно передать 3 бита. Скорость: 3000 бит/с


## Модуляция
### Определения
- **Модуляцией** называется процесс изменения одного или нескольких параметров высокочастотного несущего колебания по закону низкочастотного информационного сигнала.

- **Модулирующий сигнал** - сигнал, хранящий передаваемую информацию.

- **Несущий сигнал**- сигнал, выполняющий роль переносчика информации.

- **Модулированный сигнал** - сигнал, получающийся после посадки модулирующего сигнала на несущий сигнал.

### Нюансы
О модуляции говорим в том случае, если мы передаем высокочастотный сигнал, при этом передача информации идет на существенно меньшей частоте, то есть медленно изменяющийся сигнал определяет изменение формы более быстрого сигнала 

- Классическим примером исопльзования модуляции может служить радио. Абревиатуры FM/AM обозначают именно это
- Отсюда и происходит название для «модема» - модулятор/демодулятор 
- Можно радделить модуляцию на аналоговую и цифровую. Второй вариант также иногда назыается манипуляцией. В нем аналоговый несущий сигнал модулируется цифровым битовым потоком.
![image](https://github.com/user-attachments/assets/940fda74-8d90-4a7c-8fad-bef160d5f7f0)


### Виды
![image](https://upload.wikimedia.org/wikipedia/commons/a/a4/Amfm3-en-de.gif)



- Амплитудная, AM (amplitude modulation)
- Частотная, FM (frequency modulation)
- Фазовая, PM (phase modulation)
- Квадратурная амплитудная модуляция, QAM (Quadrature Amplitude Modulation). В данном случае меняется все подряд, что позволяет за один бод передавать до 16 бит
>[!NOTE]
>Да, ваша любимая радиостанция из диапазона 108 МГц использует именно частотную модуляцию, недаром в обывательской терминологии оно именуется FM-вещанием

![image](https://github.com/user-attachments/assets/3c5ad35b-4619-48bf-a634-9c028ddc875d)

## Режимы передачи
### Симплексное
//TODO
### Полнодуплексное
//TODO
### Дуплексное
//TODO

## Разделение доступа
### Частотное
У нас есть определенная полоса частот нашего базового сигнала, мы разбиваем ее на узкие полосы и в каждой полосе частот передаем свой сигнал. Применяется как в проводных, так и беспроводных технологиях. Так, например, работает GSM (3g,4g и так далее) и кабельное телевидение: на каждом участке полосы пропускания коаксиального кабеля передается один телеканал. 

Диапазоны обозначают как метровые, дециметровые и так далее. Следует вспомнить формулу из школьной физики $$\nu = \frac{c}{\lambda}$$, где $\nu$ — частота, $c$ — скорость света в среде передачи данных, $\lambda$ — длина волны.

### Временное 
//TODO

### Случайное
//TODO

### Маркерное
//TODO

## Сетевое оборудование
### Свич
Коммутатор (switch) 
//TODO

### Роутер
Маршрутизатор (router)
//TODO

### Репитер
Повторитель (repeater)
//TODO

### Хаб
Концентратор (hub)
//TODO

### Шлюз
Сетевой шлюз (gateway)
//TODO

## Интерфейсы
Разделяют на последовательные и параллельные
Оговаривалось, что будут отправлены доп. материалы
//TODO


## Физические среды передачи
### Витая пара
### Коаксиальный кабель
### Оптоволокно
### Радиоканал

## Ethernet
### Обозначение версий
//TODO
### Структура кадра
//TODO
### Устройства коммутации
//TODO
### Таблица коммутации
//TODO
[⬅️ Предыдущая лекция](lecture-5.md)
[📚 Вернуться к оглавлению](../README.md)
## Источники
- [Кодирование](https://books.ifmo.ru/file/pdf/2275.pdf)
- [Больше о кодировании и модуляции](https://math.gsu.by/wp-content/uploads/courses/networks/r2.2.html)
- [Лекции Валеева](https://github.com/user-attachments/files/18232159/_.pdf)
