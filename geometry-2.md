# Практика: Геометрия-2

## 1. Описание предметной области и сущностей
Эта программа представляет собой конструктор, который работает с объёмными фигурами. Он расчитывает размеры и упаковывает эти
объекты    
**body** - класс-родитель, который задаёт логику для всех трехмерных объектов. Содержит метод для посетителя        
**IVisitor<out TResult>** - интерфейс, который содержит в себе все геометрические типы, которые представлены в коде    
**Ball, RectangularCuboid, Cylinder, CompoundBody** - классы геометрических фигур, в которых содержаться их свойства      
**BoundingBoxVisitor** - класс, который вычисляет ограничивающий параллелепипед       
**BoxifyVisitor** - класс, который превращает сложные тела в упрощенные коробки    


## 2. Диаграмма классов (Mermaid)

```mermaid
classDiagram
    class Body {
        <<abstract>>
        +Vector3 Position
        +Accept(IVisitor visitor)* TResult
    }

    class Ball {
        +double Radius
        +Accept(IVisitor visitor) TResult
    }

    class RectangularCuboid {
        +double SizeX
        +double SizeY
        +double SizeZ
        +Accept(IVisitor visitor) TResult
    }

    class Cylinder {
        +double SizeZ
        +double Radius
        +Accept(IVisitor visitor) TResult
    }

    class CompoundBody {
        +IReadOnlyList~Body~ Parts
        +Accept(IVisitor visitor) TResult
    }

    class IVisitor~TResult~ {
        <<interface>>
        +Visit(Ball ball) TResult
        +Visit(RectangularCuboid cuboid) TResult
        +Visit(Cylinder cylinder) TResult
        +Visit(CompoundBody compoundBody) TResult
    }

    class BoundingBoxVisitor {
        +Visit(Ball ball) RectangularCuboid
        +Visit(RectangularCuboid cuboid) RectangularCuboid
        +Visit(Cylinder cylinder) RectangularCuboid
        +Visit(CompoundBody compoundBody) RectangularCuboid
    }

    class Extremums {
        <<struct>>
        +double MinX
        +double MaxX
        +double MinY
        +double MaxY
        +double MinZ
        +double MaxZ
    }

    class BoxifyVisitor {
        -BoundingBoxVisitor _internalEvaluator
        +Visit(Ball ball) Body
        +Visit(RectangularCuboid cuboid) Body
        +Visit(Cylinder cylinder) Body
        +Visit(CompoundBody compoundBody) Body
    }

    Body<|--Ball : Наследование 
    Body<|--RectangularCuboid : Наследование 
    Body<|--Cylinder : Наследование 
    Body<|--CompoundBody : Наследование 

    IVisitor<|..BoundingBoxVisitor : Реализация интерфейса
    IVisitor<|..BoxifyVisitor : Реализация интерфейса

    BoundingBoxVisitor*--Extremums : Композиция

    CompoundBodyo--Body : Агрегация

    BoxifyVisitor-->BoundingBoxVisitor : Ассоциация 

    Body..>IVisitor : Зависимость
    BoundingBoxVisitor..>RectangularCuboid : Зависимость
    BoxifyVisitor..>Body : Зависимость
```
