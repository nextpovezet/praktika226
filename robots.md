```mermaid
classDiagram
    %% Интерфейсы команд
    class IMoveCommand {
        <<Интерфес>>
        +Destination: Point
    }

    class IShooterMoveCommand {
        <<Интерфей>>
        +ShouldHide: bool
    }

    IMoveCommand <|-- IShooterMoveCommand : наследует

    %% Интерфейсы робота
    class IRobotAI~out TCommand~ {
        <<Интерфейс>>
        +GetCommand() TCommand
    }

    class IDevice~in TCommand~ {
        <<Интерфес>>
        +ExecuteCommand(TCommand) string
    }

    %% Реализации AI
    class ShooterAI {
        -int counter
        +GetCommand() IShooterMoveCommand
    }

    class BuilderAI {
        -int counter
        +GetCommand() IMoveCommand
    }

    ShooterAI ..|> IRobotAI~IShooterMoveCommand~ : реализует
    BuilderAI ..|> IRobotAI~IMoveCommand~ : реализует

    %% Реализации устройств
    class Mover {
        +ExecuteCommand(IMoveCommand) string
    }

    class ShooterMover {
        +ExecuteCommand(IShooterMoveCommand) string
    }

    Mover ..|> IDevice~IMoveCommand~ : реализует
    ShooterMover ..|> IDevice~IShooterMoveCommand~ : реализует

    %% Обобщ
    class Robot~TCommand~ {
        -IRobotAI~TCommand~ ai
        -IDevice~TCommand~ device
        +Start(int steps) IEnumerable~string~
    }

    Robot~TCommand~ --> IRobotAI~TCommand~ : использует
    Robot~TCommand~ --> IDevice~TCommand~ : использует

    %% Стат фабрика
    class Robot {
        <<static>>
        +Create~TCommand~(IRobotAI~TCommand~, IDevice~TCommand~) Robot~TCommand~
    }

    %% Вспомог генераторы 
    class ShooterCommand {
        <<static>>
        +ForCounter(int) IShooterMoveCommand
    }

    class BuilderCommand {
        <<static>>
        +ForCounter(int) IMoveCommand
    }

    ShooterAI --> ShooterCommand : использует
    BuilderAI --> BuilderCommand : использует

    %% завис
    Mover --> IMoveCommand : параметр
    ShooterMover --> IShooterMoveCommand : параметр
```
