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
    direction RL

    class FailureType {
        <<enum>>
        UnexpectedShutdown
        Freeze
        HardwareMalfunction
        ConnectionProblems
    }

    class Device {
        +int Id 
        +string? Name 
        +Device()
        +СhecktValidDevice() bool
    }

    class Failure {
        +FailureType Type 
        +int DeviceId 
        +DateTime Date 
        +bool IsSerious$
        +Failure()
        +WasBefore(DateTime deadline) bool
        +IsInCurrentYear() bool
    }

    class ReportMaker {
        +FindDevicesFailedBeforeDateObsolete(int day, int month, int year, int[] failureTypes, int[] deviceId, object[][] times, List~Dictionary~string, object~~ devices)$ List~string~
        +FindDevicesFailedBeforeDate(DateTime deadline, IReadOnlyCollection~Failure~ registry, IReadOnlyCollection~Device~ inventory)$ List~string~
        +GetRecentCriticalDeviceIds(IReadOnlyCollection~Failure~ registry)$ List~int~
        -TransformData(List~Dictionary~string, object~~ rawData)$ List~Device~
    }

    ReportMaker ..> Device : Зависимость (анализирует устройства)
    Failure --> FailureType : Ассоциация (классифицирует по типу)
    ReportMaker ..> Failure : Зависимость (анализирует сбои)
    
```


