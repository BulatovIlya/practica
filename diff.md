# Практика: Дифференцирование

## 1. Описание предметной области и сущностей
Данный код предназначен для дифферинцирования математических функций.    
**Algebra** - класс, который принимает лямбда выражения, извлекает из него переменную дифференцирования и запускает процесс обхода деревьев    
**DifferentiationVisitor** - класс, который занимается рекурсивным обходом узлов дерева выражений    
## 2. Диаграмма классов (Mermaid)

```mermaid
classDiagram
    class ExpressionVisitor {
        <<System.Linq.Expressions>>
        +Visit(Expression node) Expression
    }

    class ParameterExpression {
        <<System.Linq.Expressions>>
    }

    class Algebra {
        <<static>>
        +Differentiate(Expression function) Expression
    }

    class DifferentiationVisitor {
        -ParameterExpression _parameter
        -MethodInfo SinMethod
        -MethodInfo CosMethod
        +DifferentiationVisitor(ParameterExpression parameter)
        +Visit(Expression node) Expression
        -DifferentiateAdd(BinaryExpression node) Expression
        -DifferentiateMultiply(BinaryExpression node) Expression
        -DifferentiateCall(MethodCallExpression node) Expression
    }

 
    ExpressionVisitor <|-- DifferentiationVisitor : Наследование 

    Algebra ..> DifferentiationVisitor : Зависимость 

    DifferentiationVisitor o-- ParameterExpression : Агрегация 
```
