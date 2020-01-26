# ASP.NET Core 日志模型

[参考](https://mp.weixin.qq.com/s/Qux9eByEhpwH3DFNkKE_Qg)

## 三大核心对象

### Logger

独立的日志记录工具：

```csharp
public interface ILogger {
    // 方便日志追踪，可以制定一个区间范围内的日志同义标识
    IDisposable BeginScope<TState>(TState state);
    // 当前日志等级是否支持
    bool IsEnabled(LogLevel logLevel);
    // 记录日志
    void Log<TState>(LogLevel logLevel, EventId eventId, TState state, 
    Exception exception, Func<TState, Exception, string> formatter);
}
```

### LoggerProvider

日志写入目的地的最终实现者：

```csharp
public interface ILoggerProvider : IDisposable
{
  ILogger CreateLogger(string categoryName);
}
```

### LoggerFactory

创建 `Provider`，其 `CreateLogger` 方法创建的 `Logger` 作为所有 `Provider` 创建的 `Logger` 的统一入口（载体？）

```cs
public interface ILoggerFactory : IDisposable
{
  void AddProvider(ILoggerProvider provider);
  ILogger CreateLogger(string categoryName);
}
```

## 日志等级

`Logger` 的 `Log` 方法上需要传入 `LogLevel`，因此每条记录的日志都必须有日志等级标记。枚举值为：

```cs
public enum LogLevel
{
  Trace = 0,
  Debug = 1,
  Information = 2,
  Warning = 3,
  Error = 4,
  Critical = 5,
  None = 6
}
```

枚举等级越高（越严重），值越大。