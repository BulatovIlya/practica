# Практика: HoMM

## 1. Описание предметной области и сущностей
Игрок выполняет роль главного героя, который перемещается по карте для захвата объектов и ресурсов. Игрок может умерет. 
Каждый игрок имеет свой Id.    
**IOwnable** — интерфейс объектов, которые могут иметь владельца (можно захватить)      
**ICombatant** — интерфейс объектов, обладающих армией (с ними можно сразиться)       
**ITreasurable** — интерфейс объектов, содержащих внутри себя сокровища/ресурсы     
**Player** — класс игрока. Содержит уникальный идентификатор Id, а также методы для проверки исхода боя, сбора ресурсов и гибели        
**Army** — класс войск        
**Treasure** — класс ресурсов    
**Interaction** — класс, запускающий поочередную цепочку шагов взаимодействия игрока с объектом на карте    
**IInteractionStep** — интерфейс одного шага взаимодействия    
**Dwelling** — класс жилища    
**Mine** — класс шахты. Имеет охрану, приносит ресурсы и может быть захвачена    
**Creeps** — класс нейтральных монстров, которые охраняют сокровища    
**Wolves** — класс волков, с которыми можно только сразиться (не охраняют ресурсы, нельзя захватить)    
**ResourcePile** — куча ресурсов, которую можно собрать без боя    

## 2. Диаграмма классов (Mermaid)

```mermaid
classDiagram
    class IOwnable {
        <<interface>>
        +int Owner
    }
    class ICombatant {
        <<interface>>
        +Army Army
    }
    class ITreasurable {
        <<interface>>
        +Treasure Treasure
    }

    class Dwelling {
        +int Owner
    }
    class Wolves {
        +Army Army
    }
    class ResourcePile {
        +Treasure Treasure
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

    class IInteractionStep {
        <<interface>>
        +Process~T~(Player player, T target) bool
    }
    class CombatStep {
        +Process~T~(Player player, T target) bool
    }
    class TreasureStep {
        +Process~T~(Player player, T target) bool
    }
    class OwnerStep {
        +Process~T~(Player player, T target) bool
    }

    class Interaction {
        <<static>>
        -List~IInteractionStep~ Steps
        +Make~T~(Player player, T mapObject) void
    }

    class Army
    class Treasure
    class Player

    
    IOwnable <|.. Dwelling : 
    ICombatant <|.. Wolves : Реализация интерфейса
    ITreasurable <|.. ResourcePile : Реализация интерфейса
    
    IOwnable <|.. Mine : Реализация интерфейса
    ICombatant <|.. Mine : Реализация интерфейса
    ITreasurable <|.. Mine : Реализация интерфейса
    
    ICombatant <|.. Creeps : Реализация интерфейса
    ITreasurable <|.. Creeps : Реализация интерфейса

    IInteractionStep <|.. CombatStep : Реализация интерфейса
    IInteractionStep <|.. TreasureStep : Реализация интерфейса
    IInteractionStep <|.. OwnerStep : Реализация интерфейса

 
    Interaction *-- IInteractionStep : Композиция 

    ICombatant --> Army : Ассоциация
    ITreasurable --> Treasure : Ассоциация

    Interaction ..> Player : Зависимость
    CombatStep ..> Player : Зависимость
    CombatStep ..> ICombatant : Зависимость
    TreasureStep ..> Player : Зависимость
    TreasureStep ..> ITreasurable : Зависимость
    OwnerStep ..> Player : Зависимость
    OwnerStep ..> IOwnable : Зависимость
```
