# Single Responsability Principle: Clean And Maintainable Code

A class, module, or function has only one responsibility and it has to do it well. That means it should have one and only reason to change, so that makes it maintainable. When a class, module, or function has more than one responsibility, it becomes:

- **Difficult to understand:** When a class or module has multiple responsibilities, it become complex and harder to understand.
- **Difficult to maintain:** If a class or module has multiple responsibilities and one of this responsibilities has to be changed, it can affect the others responsibilities as well. This make the maintenance more difficult and increase likehood of introducting bugs into the code.
- **Difficult to reuse:** When a class or module has multiple responsibilities, it is harder to reuse it in others parts of coe. This can lead to code duplication and increase maintenance costs.

**To illustrate the SRP-Single Responsibility Principle, let's take an example that violate the SRP:**
```c#
public class Calculator
{
    public int Add(int x, int y)
    {
        int result = x + y;

        using (var writer = new StreamWriter("result.txt"))
        {
            writer.Write(result);
        }

        return result;
    }

    public int Subtract(int x, int y)
    {
        int result = x - y;

        using (var writer = new StreamWriter("result.txt"))
        {
            writer.Write(result);
        }

        return result;
    }

    public int Multiply(int x, int y)
    {
        int result = x * y;

        using (var writer = new StreamWriter("result.txt"))
        {
            writer.Write(result);
        }

        return result;
    }

    public int Divide(int x, int y)
    {
        if (y == 0)
        {
            throw new ArgumentException("Division by zero");
        }

        int result = x / y;

        using (var writer = new StreamWriter("result.txt"))
        {
            writer.Write(result);
        }

        return result;
    }
}

```
In this example the Calculator class performs arithmetic operations and save the result in a file. By doing that, we violate the SRP principle because it has two responsibilities: performing arithmetic operations and saving the result into a file.

One problem that this implementation can cause is the code duplication, for example if we need to save the result to a different file format or local, we would need to duplicate the code that writes to the file in each method.

An other problem is that the **Calculator** is limited to extend or modify, for example if we want to add a new feature we can can not do it without modifying the existing code. This can lead the code harder to maintain.

**Implementing the Calculator class on respecting the Single Responsibility Principle:**
```c#
// Interface for classes that perform arithmetic operations
public interface ICalculator
{
    int Add(int x, int y);
    int Subtract(int x, int y);
    int Multiply(int x, int y);
    int Divide(int x, int y);
}

// Implementation of ICalculator
public class Calculator : ICalculator
{
    public int Add(int x, int y)
    {
        return x + y;
    }

    public int Subtract(int x, int y)
    {
        return x - y;
    }

    public int Multiply(int x, int y)
    {
        return x * y;
    }

    public int Divide(int x, int y)
    {
        if (y == 0)
        {
            throw new ArgumentException("Division by zero");
        }

        return x / y;
    }
}

// Interface for classes that save results to a file
public interface IResultSaver
{
    void SaveToFile(int result, string filePath);
}

// Implementation of IResultSaver
public class ResultSaver : IResultSaver
{
    public void SaveToFile(int result, string filePath)
    {
        using (var writer = new StreamWriter(filePath))
        {
            writer.Write(result);
        }
    }
}

```
In this implementation, we have separated the responsibilities of performing arithmetic operations and saving results to a file into two separate classes: **Calculator** and **ResultSaver**. The **Calculator** class implements the **ICalculator** interface, which defines the methods for performing arithmetic operations. The **ResultSaver** class implements the **IResultSaver** interface, which defines the method for saving results to a file.

By separating these responsibilities, we can ensure that each class has only one reason to change, making our code more maintainable and easier to test. We can also use these classes independently of each other, and even reuse them in different contexts if needed.



