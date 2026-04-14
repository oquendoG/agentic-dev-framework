### Definición del Patrón Result

(Usa esta implementación exacta, no la modifiques)

```csharp
public interface IResult<out TValue>
{
    bool IsSuccess { get; }
    bool IsFailed { get; }
    TValue? Value { get; }
    IEnumerable<string> ErrorMessages { get; }
}

public abstract class Result<TValue> : IResult<TValue>
{
    public abstract bool IsSuccess { get; }
    public abstract bool IsFailed { get; }
    public virtual TValue? Value => default;
    public virtual IEnumerable<string> ErrorMessages => [];

    public static Result<TValue> Ok(TValue value) => new Ok<TValue>(value);
    public static Result<TValue> NotFound() => new NotFound<TValue>();
    public static Result<TValue> Error(params IEnumerable<string> errorMessages) => new Error<TValue>(errorMessages);
}

public class Ok<TValue>(TValue value) : Result<TValue>
{
    public override bool IsSuccess => true;
    public override bool IsFailed => false;
    public override TValue Value { get; } = value;
}

public class NotFound<TValue> : Result<TValue>
{
    public override bool IsSuccess => false;
    public override bool IsFailed => true;
}

public class Error<TValue> : Result<TValue>
{
    public override bool IsSuccess => false;
    public override bool IsFailed => true;
    public bool HasErrors => _errorMessages.Count != 0;

    private readonly List<string> _errorMessages = [];

    public override IEnumerable<string> ErrorMessages => _errorMessages;

    public Error(params IEnumerable<string> errorMessages)
    {
        if (errorMessages is not null)
        {
            _errorMessages.AddRange(errorMessages);
        }
    }

    public Error<TValue> AddError(string errorMessage)
    {
        _errorMessages.Add(errorMessage);
        return this;
    }
}

public static class ErrorExtensions
{
    public static string GetAllErrorsAsString<TValue>(this Error<TValue> error, string separator = "; ")
    {
        return string.Join(separator, error.ErrorMessages);
    }
}

```