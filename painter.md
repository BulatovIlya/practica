# Практика: Fractal Painter. DIP

## 1. Описание предметной области и сущностей
Данный проект представляет собой оконное приложение для генерации и настройки фрактальных изображений. Основными сущностями являются действие, инкапсулирующие логику обработки событий
через интерфейс IUAction.

## 2. Диаграмма классов (Mermaid)

```mermaid
classDiagram
    IUiAction <|.. ImageSettingsAction : Реализация
    IUiAction <|.. SaveImageAction : Реализация
    IUiAction <|.. PaletteSettingsAction : Реализация

    ImageSettingsAction --> IImageController : Ассоциация
    ImageSettingsAction --> ImageSettings : Ассоциация
    ImageSettingsAction ..> SettingsForm : Зависимость

    SaveImageAction --> IImageController : Ассоциация
    SaveImageAction ..> FilePickerSaveOptions : Зависимость

    PaletteSettingsAction --> Palette : Ассоциация
    PaletteSettingsAction ..> SettingsForm : Зависимость

    class IUiAction {
        <<interface>>
        +MenuCategory Category
        +string Name
        +CanExecute(parameter) bool
        +Execute(parameter) void
    }

    class ImageSettingsAction {
        -IImageController controller
        -ImageSettings settings
        -Func~Window~ getWindow
        +ImageSettingsAction(IImageController, ImageSettings, Func~Window~)
        +Execute(parameter) void
    }

    class SaveImageAction {
        -IImageController controller
        -Func~Window~ getWindow
        +SaveImageAction(IImageController, Func~Window~)
        +Execute(parameter) void
    }

    class PaletteSettingsAction {
        -Palette palette
        -Func~Window~ getWindow
        +PaletteSettingsAction(Palette, Func~Window~)
        +Execute(parameter) void
    }
```
