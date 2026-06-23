```mermaid
classDiagram

    %% перечисленя
    class FailureType {
        UnexpectedShutdown = 0
        Overheat = 1
        PowerSurge = 2
        SensorFailure = 3
        MechanicalFault = 4
        SoftwareCrash = 5
        NetworkLoss = 6
        BatteryLow = 7
    }
    
    %% классы
    class Device {
        +int DeviceId
        +string Name
        +Device(int id, string name)
    }
    
    class Failure {
        +int DeviceId
        +FailureType Type
        +DateTime Date
        +Failure(int deviceId, FailureType type, DateTime date)
        +bool IsSerious()
    }
    
    class ReportMaker {
        +List~string~ FindDevicesFailedBeforeDate(DateTime threshold, IEnumerable~Device~ devices, IEnumerable~Failure~ failures)
        +List~string~ FindDevicesFailedBeforeDateObsolete(int day, int month, int year, int[] failureTypes, int[] deviceId, object[][] times, List~Dictionary~string,object~~ devices)
    }
    
    class Common {
        <<статический>>
        +int IsFailureSerious(int failureType)
        +int Earlier(object[] v, int day, int month, int year)
    }
    
    %% связи
    Сбой --> FailureType : имеет тип
    Сбой --> Device : зарегистрирован на (DeviceId)
    ReportMaker ..> Device : фильтрует по ID
    ReportMaker ..> Failure : проверяет критичность
    ReportMaker ..> Common : использует (устаревший)
```
