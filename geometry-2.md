```mermaid
classDiagram
    %% абстракц
    class Body {
        <<абстракц>>
        +Vector3 Position
        +Accept(visitor: IVisitor) Body
    }

    %% кон. фигуры
    class Ball {
        +double Radius
        +Accept(visitor: IVisitor) Body
    }

    class RectangularCuboid {
        +double SizeX
        +double SizeY
        +double SizeZ
        +Accept(visitor: IVisitor) Body
    }

    class Cylinder {
        +double SizeZ
        +double Radius
        +Accept(visitor: IVisitor) Body
    }

    class CompoundBody {
        +IReadOnlyList~Body~ Parts
        +Accept(visitor: IVisitor) Body
    }

    %% интерфейс посетителя
    class IVisitor {
        <<interface>>
        +Visit(Ball) Body
        +Visit(RectangularCuboid) Body
        +Visit(Cylinder) Body
        +Visit(CompoundBody) Body
    }

    %% Реализации посетителей
    class BoundingBoxVisitor {
        +Visit(Ball) Body
        +Visit(RectangularCuboid) Body
        +Visit(Cylinder) Body
        +Visit(CompoundBody) Body
    }

    class BoxifyVisitor {
        +Visit(Ball) Body
        +Visit(RectangularCuboid) Body
        +Visit(Cylinder) Body
        +Visit(CompoundBody) Body
    }

    %% Наследование
    Body <|-- Ball
    Body <|-- RectangularCuboid
    Body <|-- Cylinder
    Body <|-- CompoundBody

    %% Реализация интерфейса
    IVisitor <|.. BoundingBoxVisitor
    IVisitor <|.. BoxifyVisitor

    %% Связи
    Body --> IVisitor : «принимает» посетителя
    CompoundBody --> Body : содержит Part
    BoundingBoxVisitor ..> RectangularCuboid : создаёт (для Ball, Cylinder, CompoundBody)
    BoxifyVisitor ..> CompoundBody : преобразует каждую часть
```
