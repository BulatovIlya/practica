# Практика: HoMM

## 1. Описание предметной области и сущностей
Игрок выполняет роль главного герия, который перемещается по карте для захвата объектов и ресурсов. Игрок может умерет. 
Каждый игрок имеет свой Id.    
**ICouldbecapturable** — интерфейс объектов, которые можно захватить    
**ICanfightable** — интерфейс объектов, которые защищает армия    
**IKeeptresures** — интерфейс объектов, которые содержат сокровища    
**Player** — класс игрока, может сражаться, захватывать и умирать    
**Army** — класс войск    
**Treasure** — класс ресурсов    
**Interaction** — класс, в котором выполняется вся логика    
**Dwelling** - класс жилища, которое можно захватить    
**Mine**  — класс шахты, которая имеет охрану, ресурсы и возможность захвата    
**Creeps** - класс монстров, которые содержат сокровища и имеют армию    
**Wolves** - класс волков, м ними можно тоько сразиться    
**ResourcePile** - куча ресурсов, которую можно собрать не сражаясь    

## 2. Диаграмма классов (Mermaid)

```mermaid
classDiagram
    direction TB

    class Player {
        +int Id
        +CanBeat(Army army) bool
        +Consume(Treasure treasure) void
        +Die() void
    }

    class Army {
    }

    class Treasure {
    }

    class ICouldbecapturable {
        <<interface>>
        +int Owner
    }

    class ICanfightable {
        <<interface>>
        +Army Army
    }

    class IKeeptresures {
        <<interface>>
        +Treasure Treasure
    }

    class Dwelling {
        +int Owner
    }

    class Mine {
        +int Owner
        +Army Army
        +Treasure Treasure
    }

    class Creeps {
        +Army Army
        +Treasure Treasure
    }

    class Wolves {
        +Army Army
    }

    class ResourcePile {
        +Treasure Treasure
    }

    class Interaction {
        <<static>>
        +Make(Player player, object mapObject)$ void
        -IsPlayerDefeated(Player activePlayer, object targetLocation)$ bool
        -ProcessRewardsAndCapture(Player activePlayer, object targetLocation)$ void
    }

    ICouldbecapturable <|.. Dwelling : Реализация интерфейса
    ICouldbecapturable <|.. Mine : Реализация интерфейса
    ICanfightable <|.. Mine : Реализация интерфейса
    IKeeptresures <|.. Mine : Реализация интерфейса
    ICanfightable <|.. Creeps : Реализация интерфейса
    IKeeptresures <|.. Creeps : Реализация интерфейса
    ICanfightable <|.. Wolves : Реализация интерфейса
    IKeeptresures <|.. ResourcePile : Реализация интерфейса


    ICanfightable o-- Army : Агрегация
    IKeeptresures o-- Treasure : Агрегация
    Mine o-- Army : Агрегация
    Mine o-- Treasure : Агрегация
    Creeps o-- Army : Агрегация
    Creeps o-- Treasure : Агрегация
    Wolves o-- Army : Агрегация
    ResourcePile o-- Treasure : Агрегация

    Interaction ..> Player : Зависимость
    Interaction ..> IsPlayerDefeated : Зависимость (проверка боя)
    Interaction ..> ProcessRewardsAndCapture : Зависимость (проверка можно ли захватить или забрать награду)
     
    IsPlayerDefeated ..> ICanfightable : Зависимость
    ProcessRewardsAndCapture ..> IKeeptresures : Зависимость
    ProcessRewardsAndCapture ..> ICouldbecapturable : Зависимость
```
