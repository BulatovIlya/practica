# Практика: HoMM

## 1. Описание предметной области и сущностей
Игрок выполняет роль главного герия, который перемещается по карте для захвата объектов и ресурсов. Игрок может умерет. 
Каждый игрок имеет свой Id.    
**ICouldbecapturable** — интерфейс объектов, которые можно захватить    
**ICanfightable** — интерфейс объектов, которые защищает армия    
**IKeeptresures** — интерфейс объектов, которые содержат сокровища    
**Player** — класс игрока. Содержит методы проверкуи исхода боя, сбора ресурсов и гибели      
**Army** — класс войск    
**Treasure** — класс ресурсов    
**MapObjectExtensions** - класс, который содержит методы расширения для взаимодействия    
**Interaction** — класс, в котором выполняется логика взаимодействия      
**Dwelling** - класс жилища, которое можно захватить      
**Mine**  — класс шахты, которая имеет охрану, ресурсы и возможность захвата    
**Creeps** - класс монстров, которые содержат сокровища и имеют армию    
**Wolves** - класс волков, м ними можно тоько сразиться    
**ResourcePile** - куча ресурсов, которую можно собрать не сражаясь    

## 2. Диаграмма классов (Mermaid)

```mermaid
classDiagram
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

    class Army
    class Treasure
    class Player {
        +int Id
        +CanBeat(Army army) bool
        +Consume(Treasure treasure) void
        +Die() void
    }

    class MapObjectExtensions {
        <<static>>
        +TryBattle~T~(this T mapObject, Player player) bool
        +TryGiveTreasure~T~(this T mapObject, Player player) void
        +TryCapture~T~(this T mapObject, Player player) void
    }
    class Interaction {
        <<static>>
        +Make~T~(Player player, T mapObject) void
    }

    ICouldbecapturable <|.. Dwelling : Реализация
    ICouldbecapturable <|.. Mine : Реализация
    ICanfightable <|.. Mine : Реализация
    IKeeptresures <|.. Mine : Реализация
    ICanfightable <|.. Creeps : Реализация
    IKeeptresures <|.. Creeps : Реализация
    ICanfightable <|.. Wolves : Реализация
    IKeeptresures <|.. ResourcePile : Реализация

    ICanfightable --> Army : Ассоциация 
    IKeeptresures --> Treasure : Ассоциация 
    Mine --> Army : Ассоциация
    Mine --> Treasure : Ассоциация
    Creeps --> Army : Ассоциация
    Creeps --> Treasure : Ассоциация
    Wolves --> Army : Ассоциация
    ResourcePile --> Treasure : Ассоциация

    
    Interaction ..> Player : Зависимость 
    Interaction ..> MapObjectExtensions : Зависимость 
    
    MapObjectExtensions ..> Player : Зависимость 
    MapObjectExtensions ..> ICanfightable : Зависимость 
    MapObjectExtensions ..> IKeeptresures : Зависимость 
    MapObjectExtensions ..> ICouldbecapturable : Зависимость 
    
    Player ..> Army : Зависимость 
    Player ..> Treasure : Зависимость 
```
