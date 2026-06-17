# Практика: TaxiOrder

## 1. Описание предметной области и сущностей
Данный проект представляет собой реализацию API заказа такси.    
**TaxiOrder** - главный класс, который управляет всем жизненным циклом заказа. Содержит в себе данные о клиенте, маршруте, водителе и статусе поездки    
**Driver** - класс, который предстиавляет сущность водителя в системе. Имеет свой Id    
**Car** - класс, который описывает автомобиль(цвет, модель, номер)    
**PersonName** - класс, который обрабатывает ФИО клиентов    
**Address** - класс, который хранит в себе параметры точек маршрута    
## 2. Диаграмма классов (Mermaid)

```mermaid
classDiagram

    ValueType_Car <|-- Car :  Наследование
    Entity_int <|-- Driver : Наследование
    Entity_int <|-- TaxiOrder : Наследование
    OrderMilestone <|-- CreationMilestone : Наследование
    OrderMilestone <|-- AssignmentMilestone : Наследование
    OrderMilestone <|-- ProgressMilestone : Наследование
    OrderMilestone <|-- CompletionMilestone : Наследование
    OrderMilestone <|-- CancellationMilestone : Наследование

    ITaxiApi_TaxiOrder <|.. TaxiApi : Реализация интерфейса


    TaxiOrder *-- OrderMilestone : Композиция 

    Driver o-- Car : Агрегация 
    Driver o-- PersonName : Агрегация 
    TaxiOrder o-- Driver : Агрегация 
    TaxiOrder o-- PersonName : Агрегация 
    TaxiOrder o-- Address : Агрегация 

    TaxiApi --> DriversRepository : Ассоциация


    DriversRepository ..> Driver : Зависимость
    TaxiApi ..> TaxiOrder : Зависимость
    TaxiApi ..> PersonName : Зависимость
    TaxiApi ..> Address : Зависимость


    class Car {
        +String Color
        +String Model
        +String PlateNumber
        +Car(String, String, String)
    }

    class Driver {
        +PersonName Name
        +Car Car
        +Driver(int, PersonName, Car)
    }

    class DriversRepository {
        +GetDriver(int driverId) Driver
    }

    class OrderMilestone {
        +DateTime MilestoneTime
        #OrderMilestone(DateTime)
    }

    class TaxiOrder {
        -List~OrderMilestone~ _timelineLog
        +PersonName ClientName
        +Address Start
        +Address Destination
        +Driver Driver
        +TaxiOrderStatus Status
        +DateTime CreationTime
        +DateTime DriverAssignmentTime
        +DateTime StartRideTime
        +DateTime FinishRideTime
        +DateTime CancelTime
        +TaxiOrder(int, PersonName, Address, DateTime)
        +UpdateDestination(Address)
        +AssignDriver(Driver, DateTime)
        +UnassignDriver()
        +StartRide(DateTime)
        +FinishRide(DateTime)
        +Cancel(DateTime)
        -ValidatePhase(TaxiOrderStatus[])
        -ExtractTimeOf~TMilestone~() DateTime
    }

    class TaxiApi {
        -DriversRepository _provider
        -Func~DateTime~ _timer
        -int[] _identityCell
        +TaxiApi(DriversRepository, Func~DateTime~)
        +CreateOrderWithoutDestination(string, string, string, string) TaxiOrder
        +UpdateDestination(TaxiOrder, string, string)
        +AssignDriver(TaxiOrder, int)
        +UnassignDriver(TaxiOrder)
        +Cancel(TaxiOrder)
        +StartRide(TaxiOrder)
        +FinishRide(TaxiOrder)
        +GetDriverFullInfo(TaxiOrder) string
        +GetShortOrderInfo(TaxiOrder) string
    }

    class ValueType_Car["ValueType&lt;Car&gt;"]
    class Entity_int["Entity&lt;int&gt;"]
    class ITaxiApi_TaxiOrder["ITaxiApi&lt;TaxiOrder&gt;"]
    <<interface>> ITaxiApi_TaxiOrder
    <<abstract>> OrderMilestone
```
