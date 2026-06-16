# Практика: Генератор отчетов

## 1. Описание предметной области и сущностей
Данный проект представляет собой модуль для автоматической генерации отчётов на основе собранных данных.    
**IReportFormatter** - интерфейс, который определеяет методы для формирования отчёта, изолирует логику вычислений и форматирования отчёта    
**HtmlCompiler** - класс, который реализует интерфейс IReportFormatter и отвечает за генерацию html разметки в отчёте       
**MarkdownCompiler** - класс, который реализует интерфейс IReportFormatter и отвечает за генерацию markdown разметки в отчёте          
**ReportMaker** - класс, который является основным. Объединяет все процессы для создания отчёта    
**MathematicalCalculations** - класс, который объединяет в себе методы для математических вычислений    
**ReportMakerHelper** - класс, который объединяет в себе все методы для создания отчётов    
## 2. Диаграмма классов (Mermaid)

```mermaid
classDiagram
    direction TD

    class IReportFormatter {
        <<interface>>
        +string Caption
        +MakeCaption(string caption) string
        +BeginList() string
        +MakeItem(string valueType, string entry) string
        +EndList() string
    }

    class HtmlCompiler {
        +string Caption
        +MakeCaption(string caption) string
        +BeginList() string
        +EndList() string
        +MakeItem(string valueType, string entry) string
    }

    class MarkdownCompiler {
        +string Caption
        +MakeCaption(string caption) string
        +BeginList() string
        +EndList() string
        +MakeItem(string valueType, string entry) string
    }

    class ReportMaker {
        -IReportFormatter _formatter
        -Func~IEnumerable~double~, object~ _stats
        -string _caption
        +ReportMaker(string caption, IReportFormatter formatter, Func~IEnumerable~double~, object~ stats)
        +MakeReport(IEnumerable~Measurement~ measurements) string
    }

    class Measurement {
        +double Temperature
        +double Humidity
    }

    class MathematicalСalculations {
        <<static>>
        +MeanAndStd(IEnumerable~double~ _date) object
        +Median(IEnumerable~double~ date) object
    }

    class ReportMakerHelper {
        <<static>>
        +MeanAndStdHtmlReport(IEnumerable~Measurement~ data) string
        +MedianMarkdownReport(IEnumerable~Measurement~ data) string
        +MeanAndStdMarkdownReport(IEnumerable~Measurement~ data) string
        +MedianHtmlReport(IEnumerable~Measurement~ data) string
    }

    IReportFormatter <|.. HtmlCompiler : Реализация интерфейса
    IReportFormatter <|.. MarkdownCompiler : Реализация интерфейса
    ReportMaker o-- IReportFormatter : Агрегация
    ReportMaker ..> Measurement : Зависимость
    
    ReportMakerHelper ..> ReportMaker : Зависимость
    ReportMakerHelper ..> HtmlCompiler : Зависимость
    ReportMakerHelper ..> MarkdownCompiler : Зависимость
    ReportMakerHelper ..> MathematicalСalculations : Зависимость
    ReportMakerHelper ..> Measurement : Зависимость
```
