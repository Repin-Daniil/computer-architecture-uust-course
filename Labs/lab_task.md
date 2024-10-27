### **Задание на ЛР №1**
**Требования к программе**
Нужен графический интерфейс, просто в консоль нельзя
Можно и нужно использовать просто готовую библиотеку
Вводится диапазон ip адресов (начальный адрес и конечный)
Выводить 4 параметра, описанных в задании

**Задание**
Необходимо разработать приложение:
 1. Ввод диапазона IP-адресов.
 2. На основании парсинга этого диапазона рассчитать следующие параметры сети: 
	 - Адрес сети
	 - broadcast адрес
	 - MAC-адрес вашего узла сети
	 - Маску сети

Алгоритм поиска маски на C#:
```csharp
IPAddress beginIP - начальный ip-адрес
IPAddress endIP - конечный ip-адрес
--------------------------------
var begin = beginIP.GetAddressBytes();
var end = endIP.GetAddressBytes();
byte[] mask = new byte[4];
--------------------------------

bool edge = false;
 for(int i=0; i<4; i++)
  for(byte b=128; b>=1; b/=2){
    if(!edge && (begin[i]&b)==end[i]&b)) {
      mask[i]|=b;
    }
    else{
     edge=true;
     mask[i]=(byte)(mask[i]&~b);
    }
}
```
### **Задание на ЛР №2**
##### Введение
Клиент-серверная архитектура является одним из наиболее распространенных подходов к построению распределенных систем. В данной архитектуре существует два типа узлов: сервер и клиент. Сервер предоставляет определенные ресурсы или услуги, а клиент обращается к серверу для получения доступа к этим ресурсам или услугам.

Одним из примеров клиент-серверной архитектуры является чат-система, в которой сервер отвечает за обработку и передачу сообщений клиентам. 

В данной лабораторной работе предлагается реализовать простой чат клиент-сервер, используя сокеты протокола TCP/IP и язык программирования (C#, C++, Java, Python, и т.д.). Должна быть возможность запуска нескольких приложений-клиентов.
##### Цели и задачи
**Цель**:
- Изучить процесс создания клиент-серверного приложения, обеспечивающего обмен сообщениями в реальном времени через механизм сокетов.
**Задачи**:
- Создание серверной части приложения, отвечающей за обработку подключений клиентов и передачу сообщений(TCP-пакетов).
- Создание клиентской части приложения, предоставляющей интерфейс для обмена сообщениями с сервером.
##### Указания
В C# для работы с протоколом TCP/IP предоставляется класс `TcpClient`, который обеспечивает подключение к серверу и передачу данных между клиентом и сервером. Для создания сервера используется класс `TcpListener`, который прослушивает входящие подключения от клиентов и создает новые экземпляры `TcpClient` для обработки каждого подключения.

Чат-сервер реализуется в виде консольного приложения, которое создает экземпляр класса `TcpListener` и начинает прослушивание входящих подключений на заданном IP-адресе и порту. При получении нового подключения сервер создает новый экземпляр `TcpClient` для обработки подключения и запускает новый поток для обработки входящих сообщений от клиента.
 
**Серверная часть**:
Для создания серверной части используется пространство имён `System.Net.Sockets`, предоставляющее классы для работы с сетевыми соединениями. В качестве основного класса, отвечающего за обработку подключений, был выбран `TcpListener`.При запуске сервера создаётся экземпляр `TcpListener`, настроенный на прослушивание входящих подключений на порту. Затем сервер переходит в бесконечный цикл ожидания новых подключений с помощью метода `AcceptTcpClient()` Каждое новое подключение обрабатывается в отдельном потоке, создаваемом с помощью класса `Thread`.
 
**Клиентская часть:**
Использует пространство имён `System.Net.Sockets` для работы с сетевыми соединениями. Для отправки сообщений используется метод `SendMessage()`, который отправляет сообщение на сервер в виде байтового массива. Для получения сообщений реализуется отдельный поток, который непрерывно ожидает входящих сообщений с помощью метода `Read() `класса `NetworkStream.`

### **Задание на ЛР №3** 
Необходимо добавить авторизацию для клиентского приложения из ЛР№2 в виде `Логин`, `Пароль`.
Можно использовать любую БД для этого (`MariaDB`, `PostgreSQL`, `MongoDB`, `MS Access`, можно хоть чтение из текстового файла)

### **Задание на ЛР №4**
В лабораторной работе №4 вам необходимо написать простой анализатор сетевого траффика (сниффер) с парсингом IP, TCP, UDP и DNS пакетов.

Для захвата пакетов, мы используем raw(сырой) сокет и привязываем его к IP-адресу. После установки некоторых параметров для сокета, вызываем метод IOControl. Обратите внимание, что IOControl аналогичен методу Winsock2WSAIoctl. IOControlCode.ReceiveAll означает, что захватываться будут абсолютно все пакеты как входящие так и исходящие.

```csharp
//Для захвата пакетов сокетом
//он должен иметь тип raw
//с протоколом IP
mainSocket = newSocket(AddressFamily.InterNetwork, SocketType.Raw,
                       ProtocolType.IP);
 
// Привязываем сокет к выбранному IP
mainSocket.Bind(newIPEndPoint(IPAddress.Parse(cmbInterfaces.Text),0));
 
//Устанавливаем опции у сокета
mainSocket.SetSocketOption(SocketOptionLevel.IP,  //Принимать только IP пакеты
                           SocketOptionName.HeaderIncluded, //Включать заголовок
                           true);                           
 
byte[] byTrue = newbyte[4]{1, 0, 0, 0};
byte[] byOut = newbyte[4];
 
//Socket.IOControl это аналог метода WSAIoctl в Winsock 2
mainSocket.IOControl(IOControlCode.ReceiveAll,  //SIO_RCVALL of Winsock
                     byTrue, byOut);
 
//Начинаем приём асинхронный приём пакетов
mainSocket.BeginReceive(byteData, 0, byteData.Length, SocketFlags.None,
                        newAsyncCallback(OnReceive), null);
 
public classIPHeader 
{ 
  //Поля IP заголовка
  private byte byVersionAndHeaderLength; // Восемь бит для версии 
                                         // и длины 
  private byte byDifferentiatedServices; // Восемь бит для дифференцированного 
                                         // сервиса
  private ushort usTotalLength;          // 16 бит для общей длины 
  private ushort usIdentification;       // 16 бит для идентификатора
  private ushort usFlagsAndOffset;       // 16 бит для флагов, фрагментов 
                                         // смещения 
  private byte byTTL;                    // 8 бит для TTL (Time To Live) 
  private byte byProtocol;               // 8 бит для базового протокола
  private short sChecksum;               // 16 бит для контрольной суммы 
                                         //  заголовка 
  private uint uiSourceIPAddress;        // 32 бита для адреса источника IP 
  private uint uiDestinationIPAddress;   // 32 бита для IP назначения 
 
  //Конец полей IP заголовка   
  private byte byHeaderLength;             //Длина заголовка
  private byte[] byIPData = new byte[4096]; //Данные в дейтаграмме
  public IPHeader(byte[] byBuffer, int nReceived)
  {
    try
    {
    //Создаём MemoryStream для принимаемых данных
    MemoryStream memoryStream = newMemoryStream(byBuffer, 0, nReceived);
 
    //Далее создаем BinaryReader для чтения MemoryStream
    BinaryReader binaryReader = newBinaryReader(memoryStream);
 
    //Первые 8 бит содержат верисю и длину заголовка
    //считываем 8 бит = 1 байт
    byVersionAndHeaderLength = binaryReader.ReadByte();
 
    //Следующие 8 бит содержат дифф. сервис
    byDifferentiatedServices = binaryReader.ReadByte();
 
    //Следующие 8 бит содержат общую длину дейтаграммы
    usTotalLength = 
             (ushort) IPAddress.NetworkToHostOrder(binaryReader.ReadInt16());
 
    //16 байт для идентификатора
    usIdentification = 
              (ushort)IPAddress.NetworkToHostOrder(binaryReader.ReadInt16());
 
    //8 бит для флагов, фрагментов, смещений
    usFlagsAndOffset = 
              (ushort)IPAddress.NetworkToHostOrder(binaryReader.ReadInt16());
 
    //8 бит для TTL
    byTTL = binaryReader.ReadByte();
 
    //8 бит для базового протокола
    byProtocol = binaryReader.ReadByte();
 
    //16 бит для контрольной суммы
    sChecksum = IPAddress.NetworkToHostOrder(binaryReader.ReadInt16());
 
    //32 бита для IP источника
    uiSourceIPAddress = (uint)(binaryReader.ReadInt32());
 
    //32 бита IP назначения
    uiDestinationIPAddress = (uint)(binaryReader.ReadInt32());
 
    //Высчитываем длину заголовка
    byHeaderLength = byVersionAndHeaderLength;
 
    //Последние 4 бита в версии и длине заголовка содержат длину заголовка

    //выполняем простые арифметические операции для их извлечения
    byHeaderLength <<= 4;
    byHeaderLength >>= 4;
 
    //Умножаем на 4 чтобы получить точную длину заголовка
    byHeaderLength *= 4;
 
    //Копируем данные (которые содержат информацию в соответствии с типом 
    //основного протокола) в другой массив
    Array.Copy(byBuffer, 
               byHeaderLength, //копируем с конца заголовка
               byIPData, 0, usTotalLength - byHeaderLength);
    }
    catch (Exception ex)
    {
        MessageBox.Show(ex.Message, "MJsniff", MessageBoxButtons.OK, 
                        MessageBoxIcon.Error);
    }
 }

}
```

IP датаграммы инкапсулируются TCP и UDP пакетами. Они также содержатся в  данных, передаваемых по протоколам прикладного уровня, таких как DNS, HTTP, FTP, SMTP, SIP и т.д. Таким образом, пакет TCP содержит в себе дейтаграммы IP, например:

| IP-заголовок | TCP-заголовок<br> | Данные |
| ------------ | ----------------- | ------ |

Итак, первое, что мы должны сделать, это проанализировать IP-заголовок. Для этого создан урезанный класс `classIPHeader`. Комментарии описывают что и как происходит.

```csharp
 
public classIPHeader 
{ 
  //Поля IP заголовка
  private byte byVersionAndHeaderLength; // Восемь бит для версии 
                                         // и длины 
  private byte byDifferentiatedServices; // Восемь бит для дифференцированного 
                                         // сервиса
  private ushort usTotalLength;          // 16 бит для общей длины 
  private ushort usIdentification;       // 16 бит для идентификатора
  private ushort usFlagsAndOffset;       // 16 бит для флагов, фрагментов 
                                         // смещения 
  private byte byTTL;                    // 8 бит для TTL (Time To Live) 
  private byte byProtocol;               // 8 бит для базового протокола
  private short sChecksum;               // 16 бит для контрольной суммы 
                                         //  заголовка 
  private uint uiSourceIPAddress;        // 32 бита для адреса источника IP 
  private uint uiDestinationIPAddress;   // 32 бита для IP назначения 
 
  //Конец полей IP заголовка   
  private byte byHeaderLength;             //Длина заголовка
  private byte[] byIPData = new byte[4096]; //Данные в дейтаграмме
  public IPHeader(byte[] byBuffer, int nReceived)
  {
    try
    {
    //Создаём MemoryStream для принимаемых данных
    MemoryStream memoryStream = newMemoryStream(byBuffer, 0, nReceived);
 
    //Далее создаем BinaryReader для чтения MemoryStream
    BinaryReader binaryReader = newBinaryReader(memoryStream);
 
    //Первые 8 бит содержат верисю и длину заголовка
    //считываем 8 бит = 1 байт
    byVersionAndHeaderLength = binaryReader.ReadByte();
 
    //Следующие 8 бит содержат дифф. сервис
    byDifferentiatedServices = binaryReader.ReadByte();
 
    //Следующие 8 бит содержат общую длину дейтаграммы
    usTotalLength = 
             (ushort) IPAddress.NetworkToHostOrder(binaryReader.ReadInt16());
 
    //16 байт для идентификатора
    usIdentification = 
              (ushort)IPAddress.NetworkToHostOrder(binaryReader.ReadInt16());
 
    //8 бит для флагов, фрагментов, смещений
    usFlagsAndOffset = 
              (ushort)IPAddress.NetworkToHostOrder(binaryReader.ReadInt16());
 
    //8 бит для TTL
    byTTL = binaryReader.ReadByte();
 
    //8 бит для базового протокола
    byProtocol = binaryReader.ReadByte();
 
    //16 бит для контрольной суммы
    sChecksum = IPAddress.NetworkToHostOrder(binaryReader.ReadInt16());
 
    //32 бита для IP источника
    uiSourceIPAddress = (uint)(binaryReader.ReadInt32());
 
    //32 бита IP назначения
    uiDestinationIPAddress = (uint)(binaryReader.ReadInt32());
 
    //Высчитываем длину заголовка
    byHeaderLength = byVersionAndHeaderLength;
 
    //Последние 4 бита в версии и длине заголовка содержат длину заголовка
    //выполняем простые арифметические операции для их извлечения
    byHeaderLength <<= 4;
    byHeaderLength >>= 4;
 
    //Умножаем на 4 чтобы получить точную длину заголовка
    byHeaderLength *= 4;
 
    //Копируем данные (которые содержат информацию в соответствии с типом 
    //основного протокола) в другой массив
    Array.Copy(byBuffer, 
               byHeaderLength, //копируем с конца заголовка
               byIPData, 0, usTotalLength - byHeaderLength);
    }
    catch (Exception ex)
    {
        MessageBox.Show(ex.Message, "MJsniff", MessageBoxButtons.OK, 
                        MessageBoxIcon.Error);
    }
 }
 
}
```
**Пример:**
![](example.png)