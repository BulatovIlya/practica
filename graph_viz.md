# Практика: GraphViz

## 1. Описание предметной области и сущностей
Данный проект представляет собой библиотеку для построения графов, написанную с помощью Fluent Api    
**DotGraphBuilder** - класс, который отвечает за инизиализацию структуры графа. Является точкой входа для добавления элементов графа и генерации DOT    
**NodeHandle** и **EdgeHandle** - классы, который предоставляют доступ к объектам графа    
**AttrBase<T>** - класс, который отвечает за логику хранения и установки атрибутов    
**NodeAttrs** и **EdgeAttrs** - классы, которые содержат расширяющие методы    
## 2. Диаграмма классов (Mermaid)

```mermaid
classDiagram
    class AttrBase~T~ {
        <<abstract>>
        #Dictionary~string, string~ Attributes
        +Color(string v) T
        +FontSize(int v) T
        +Label(string v) T
    }

    class NodeAttrs {
        +Shape(NodeShape s) NodeAttrs
    }

    class EdgeAttrs {
        +Weight(double v) EdgeAttrs
    }

    class DotGraphBuilder {
        -Graph graph
        +DirectedGraph(string name) DotGraphBuilder
        +UndirectedGraph(string name) DotGraphBuilder
        +AddNode(string name) NodeHandle
        +AddEdge(string src, string dst) EdgeHandle
        +Build() string
    }

    class NodeHandle {
        -DotGraphBuilder builder
        -GraphNode node
        +With(Action~NodeAttrs~ configure) DotGraphBuilder
        +AddNode(string name) NodeHandle
        +AddEdge(string src, string dst) EdgeHandle
        +Build() string
    }

    class EdgeHandle {
        -DotGraphBuilder builder
        -GraphEdge edge
        +With(Action~EdgeAttrs~ configure) DotGraphBuilder
        +AddNode(string name) NodeHandle
        +AddEdge(string src, string dst) EdgeHandle
        +Build() string
    }

    AttrBase <|-- NodeAttrs : Наследование
    AttrBase <|-- EdgeAttrs : Наследование
    
    DotGraphBuilder --> NodeHandle : Ассоциация
    DotGraphBuilder --> EdgeHandle : Ассоциация
    
    NodeHandle --> DotGraphBuilder : Ассоциация
    EdgeHandle --> DotGraphBuilder : Ассоциация
    
    NodeHandle ..> NodeAttrs : Зависимость
    EdgeHandle ..> EdgeAttrs : Зависимость

```
