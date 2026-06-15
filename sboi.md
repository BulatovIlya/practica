# Практика: Сбои

## 1. Описание предметной области и сущностей
Данная система предназначена для анализа сбоев на различных устройств и формированию отчётов по критическим сбоям.    
**Failure** - класс, который содержит в себе тип сбоя, устройство и дату. Сам определелят, насколько сбой критичен с помощью метода IsSerious    
**Device** - класс, который содержит id и название устройства    
**FailureType:** - перечисление типов сбоя, которое классифицирует виды для дальнейший фильтрации ()   
**ReportMaker** - класс, который отвечает за фильтрацию серьезных сбоев до определённой даты и составление списка сломавшихся устройств    

## 2. Диаграмма классов (Mermaid)

```mermaid
classDiagram
    class FailureType {
        <<enumeration>>
        UnexpectedShutdown = 0
        ShortNonResponding = 1
        HardwareFailures = 2
        ConnectionProblems = 3
    }

    class Failure {
        +FailureType Type
        +int DeviceId
        +DateTime Date
        +bool IsSerious
        +Failure()
        +Failure(FailureType type, int deviceId, DateTime date)
    }

    class Device {
        +int Id
        +string Name
        +Device()
        +Device(int id, string name)
    }

    class ReportMaker {
        -ReportMaker()
        +FindDevicesFailedBeforeDate(DateTime beforeDate, List~Failure~ failures, List~Device~ devices) List~string~$
        +FindDevicesFailedBeforeDateObsolete(int day, int month, int year, int[] failureTypes, int[] deviceId, object[][] times, List~Dictionary~string, object~~ devices) List~string~$
        -Convert(List~Dictionary~string, object~~ devices) List~Device~$
    }

    Failure --> FailureType : Ассоциация (классифицирует по типу)
    ReportMaker ..> Failure : Зависимость (анализирует сбои)
    ReportMaker ..> Device : Зависимость (анализирует устройства)
```


