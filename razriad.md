# Практика: Контрольный разряд

## 1. Описание предметной области и сущностей
Данный проект представляет собой скрипт, скрипт который валидирует серийные номера.    
**ControlDigitAlgo** - класс, в который отвечает за реализацию алгоритмов валидации    
**Extensions** - класс, отвечает за работу с числами: разбиение на разряды и агрегация    
****
## 2. Диаграмма классов (Mermaid)

```mermaid
classDiagram
    class Extensions {
        <<static>>
        +GetDigits(long number) IEnumerable~int~
        +Sum(IEnumerable~int~ digits, Func~int, int, int~ selector) int
    }

    class ControlDigitAlgo {
        <<static>>
        +Upc(long number) int
        +Isbn10(long number) int
        +Luhn(long number) int
        -TransformLuhn(int digit) int
    }

    ControlDigitAlgo ..> Extensions : Зависимость
```
