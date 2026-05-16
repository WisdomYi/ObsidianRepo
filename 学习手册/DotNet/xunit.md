## 基础安装
引用：
1.xunit ：xunit核心包。
2.xunit.runner.visualstudio : nuget解释器，支持vs2022以后的版本中的测试管理器。
3.Microsoft.NET.Test.SDK :  运行测试的基础包，如果不安装会导致找不到testhost。

4.方法中加上特性 \[Fact\]。
5.执行vs调试/运行测试。

通常可指定Xunit.Assert类校验。

## 1.测试方法命名规范

• 清晰表达意图：测试方法的名称应明确表达测试的意图，包括被测试的方法名、测试场景和预期行为。例如：

```csharp
  [Fact]
  public void Add_TwoNumbers_ReturnsSum()
  {
      // 测试代码
  }
  ```

这样的命名方式可以让其他开发者快速理解测试的目的。


## 2.使用`[Fact]`和`[Theory]`

• `[Fact]`：用于标记无需参数的测试方法，表示该方法是一个独立的测试用例。

```csharp
  [Fact]
  public void ShouldReturnTrue_WhenConditionIsMet()
  {
      // 测试代码
  }
  ```


• `[Theory]`：用于标记需要参数的测试方法，通常与`[InlineData]`或`[MemberData]`配合使用，以支持数据驱动的测试。

```csharp
  [Theory]
  [InlineData(1, 2, 3)]
  [InlineData(-1, -2, -3)]
  public void Add_TwoNumbers_ReturnsSum(int a, int b, int expected)
  {
      var result = new Calculator().Add(a, b);
      Assert.Equal(expected, result);
  }
  ```



## 3.数据驱动测试

• `[InlineData]`：直接在测试方法上提供测试数据。

```csharp
  [Theory]
  [InlineData(1, 2, 3)]
  [InlineData(0, 0, 0)]
  public void Add_TwoNumbers_ReturnsSum(int a, int b, int expected)
  {
      var result = new Calculator().Add(a, b);
      Assert.Equal(expected, result);
  }
  ```


• `[MemberData]`：从类的静态方法、属性或字段中获取测试数据，适用于复杂的测试数据集合。

```csharp
  public static IEnumerable<object[]> GetTestData()
  {
      yield return new object[] { 1, 2, 3 };
      yield return new object[] { -1, -2, -3 };
  }

  [Theory]
  [MemberData(nameof(GetTestData))]
  public void Add_TwoNumbers_ReturnsSum(int a, int b, int expected)
  {
      var result = new Calculator().Add(a, b);
      Assert.Equal(expected, result);
  }
  ```



## 4.使用`[Trait]`对测试分类

• `[Trait]`：为测试方法添加分类标签，方便筛选和执行特定类型的测试。

```csharp
  [Fact]
  [Trait("Category", "Math")]
  public void Add_TwoNumbers_ReturnsSum()
  {
      // 测试代码
  }
  ```



## 5.忽略测试

• 如果某些测试暂时不需要运行，可以使用`[Fact(Skip = "Reason")]`或`[Theory(Skip = "Reason")]`来跳过它们。

```csharp
  [Fact(Skip = "暂时跳过")]
  public void TemporarilySkipThisTest()
  {
      // 测试代码
  }
  ```



## 6.使用`ITestOutputHelper`输出日志

• 在测试中注入`ITestOutputHelper`，用于输出测试过程中的日志信息。

```csharp
  public class MyTests
  {
      private readonly ITestOutputHelper _output;

      public MyTests(ITestOutputHelper output)
      {
          _output = output;
      }

      [Fact]
      public void TestWithOutput()
      {
          _output.WriteLine("测试开始");
          // 测试代码
          _output.WriteLine("测试结束");
      }
  }
  ```



## 7.避免在测试中编写逻辑

• 测试方法应尽量避免包含复杂的逻辑（如循环、条件语句等），以减少引入错误的可能性。如果需要测试多个数据点，建议使用`[Theory]`和数据驱动的方式。

```csharp
  // 不推荐：在测试中使用循环
  [Fact]
  public void Add_MultipleDataPoints_ReturnsCorrectResults()
  {
      var testData = new List<(int, int, int)>
      {
          (1, 2, 3),
          (0, 0, 0),
          (-1, -2, -3)
      };

      foreach (var (a, b, expected) in testData)
      {
          var result = new Calculator().Add(a, b);
          Assert.Equal(expected, result);
      }
  }
  ```



## 8.使用`IClassFixture`共享测试上下文

• 如果多个测试需要共享相同的初始化逻辑，可以使用`IClassFixture`。它会在测试类的生命周期内创建一次，并在所有测试方法中共享。

```csharp
  public class CalculatorFixture
  {
      public Calculator Calculator { get; } = new Calculator();
  }

  public class CalculatorTests : IClassFixture<CalculatorFixture>
  {
      private readonly Calculator _calculator;

      public CalculatorTests(CalculatorFixture fixture)
      {
          _calculator = fixture.Calculator;
      }

      [Fact]
      public void Add_TwoNumbers_ReturnsSum()
      {
          var result = _calculator.Add(1, 2);
          Assert.Equal(3, result);
      }
  }
  ```



## 9.使用`CollectionBehavior`控制测试执行顺序

• 默认情况下，xUnit 不保证测试的执行顺序。如果需要控制测试的执行顺序，可以使用`CollectionBehavior`。

```csharp
  [CollectionBehavior(DisableTestParallelization = true)]
  public class OrderedTests
  {
      [Fact]
      public void Test1()
      {
          // 测试代码
      }

      [Fact]
      public void Test2()
      {
          // 测试代码
      }
  }
  ```



## 10.测试异常

• 使用`Assert.Throws`或`Record.Exception`来测试方法是否抛出预期的异常。

```csharp
  [Fact]
  public void DivideByZero_ThrowsDivideByZeroException()
  {
      var calculator = new Calculator();
      Assert.Throws<DivideByZeroException>(() => calculator.Divide(1, 0));
  }
  ```



## 11.使用 Mock 框架

• 在测试中，使用 Mock 框架（如 Moq）来模拟依赖项，避免测试与外部系统的耦合。

```csharp
  [Fact]
  public void ProcessOrder_WithValidOrder_SendsEmail()
  {
      var mockEmailService = new Mock<IEmailService>();
      var orderService = new OrderService(mockEmailService.Object);

      orderService.ProcessOrder(new Order());

      mockEmailService.Verify(x => x.SendEmail(It.IsAny<string>()), Times.Once());
  }
  ```



## 12.避免使用`Setup`和`Teardown`

• xUnit 不支持`Setup`和`Teardown`方法。如果需要初始化和清理逻辑，建议使用`IClassFixture`或`IDisposable`。

```csharp
  public class MyTests : IDisposable
  {
      private readonly SomeResource _resource;

      public MyTests()
      {
          _resource = new SomeResource();
      }

      public void Dispose()
      {
          _resource.Dispose();
      }

      [Fact]
      public void TestUsingResource()
      {
          // 使用 _resource
      }
  }
  ```



## 13.使用 Fluent Assertions


• Fluent Assertions 是一个强大的断言库，可以提供更清晰、更易读的断言语法。
以下是基于搜索结果整理的 Fluent Assertions 的使用技巧和最佳实践，帮助你更高效地编写和管理单元测试代码：

引用包：FluentAssertions

### 1.简化断言语法
Fluent Assertions 提供了链式调用的语法，使断言更加直观和易读。例如：

```csharp
// 传统写法
Assert.AreEqual(5, result);

// Fluent Assertions 写法
result.Should().Be(5);
```



### 2.多条件断言
可以在一个语句中连续进行多个断言，使代码更加简洁：

```csharp
string actual = "ABCDEFGHI";
actual.Should().StartWith("AB")
      .And.EndWith("HI")
      .And.Contain("EF")
      .And.HaveLength(9);
```



### 3.提供上下文信息
在断言失败时，Fluent Assertions 会尝试提取被测试对象的名称，并在错误消息中显示。例如：

```csharp
string username = "dennis";
username.Should().Be("jonas");
```

如果断言失败，错误消息会显示：

```
Expected username to be "jonas" with a length of 5, but "dennis" has a length of 6, differs near "den" (index 0).
```



### 4.使用`AssertionScope`批量断言
可以将多个断言放入一个`AssertionScope`中，这样即使其中一个断言失败，其他断言也会继续执行，并在最后抛出一个包含所有失败信息的异常：

```csharp
using (new AssertionScope())
{
    5.Should().Be(10);
    "Actual".Should().Be("Expected");
}
```

如果断言失败，抛出的异常会包含所有失败信息：

```
Expected value to be 10, but found 5 (difference of -5).
Expected string to be "Expected" with a length of 8, but "Actual" has a length of 6, differs near "Act" (index 0).
```



### 5.自定义断言
可以通过扩展方法创建自定义断言，方便在多个测试中复用复杂的断言逻辑。例如：

```csharp
public static class PersonAssertions
{
    [CustomAssertion]
    public static void NotBeDoe(this Person? person, string because = "", params object[] becauseArgs)
    {
        Execute.Assertion.ForCondition(person is not null)
            .BecauseOf(because, becauseArgs)
            .FailWith("Expected person not to be null.");

        Execute.Assertion.ForCondition(!string.IsNullOrEmpty(person!.FirstName))
            .BecauseOf(because, becauseArgs)
            .FailWith("Expected person's first name not to be null or empty.");

        Execute.Assertion.ForCondition(!string.Equals(person.LastName, "doe", StringComparison.OrdinalIgnoreCase))
            .BecauseOf(because, becauseArgs)
            .FailWith("Expected person must not have last name Doe.");
    }
}
```

使用自定义断言：

```csharp
var person = new Person { FirstName = "John", LastName = "Doe" };
person.Should().NotBeDoe();
```



### 6.针对不同数据类型的断言
Fluent Assertions 提供了丰富的断言方法，适用于各种数据类型：


字符串断言

```csharp
string result = "This is an example";
result.Should().Contain("example");
result.Should().StartWith("This");
result.Should().EndWith("example");
result.Should().HaveLength(18);
```



数值断言

```csharp
int number = 10;
number.Should().Be(10);
number.Should().BeGreaterThan(5);
number.Should().BeLessThan(20);
number.Should().BeInRange(1, 15);
```



布尔值断言

```csharp
bool result = true;
result.Should().BeTrue();
result.Should().NotBeFalse();
```



日期时间断言

```csharp
var date = new DateTime(2023, 7, 7, 12, 15, 0, DateTimeKind.Utc);
date.Should().BeAfter(new DateTime(2023, 7, 1));
date.Should().BeBefore(new DateTime(2023, 7, 8));
date.Should().BeSameDateAs(new DateTime(2023, 7, 7));
```



对象断言

```csharp
var author = new Author { Id = 1, FirstName = "Joydip", LastName = "Kanjilal" };
author.Should().NotBeNull();
author.Should().BeOfType<Author>();
author.FirstName.Should().Be("Joydip");
```



### 7.使用`Because`参数
在断言中可以使用`Because`参数提供额外的上下文信息，帮助理解断言失败的原因：

```csharp
int result = 10;
result.Should().Be(5, "because we expect the result to be half of the input value");
```



### 8.测试异常
可以使用 Fluent Assertions 来测试方法是否抛出预期的异常：

```csharp
Action action = () => new Calculator().Divide(1, 0);
action.Should().Throw<DivideByZeroException>();
```



### 9.测试集合
Fluent Assertions 提供了丰富的集合断言方法：

```csharp
var list = new List<int> { 1, 2, 3 };
list.Should().Contain(2);
list.Should().NotContain(4);
list.Should().HaveCount(3);
list.Should().BeInAscendingOrder();
```



### 10.测试空值和非空值
对于可空类型，可以使用以下方法：

```csharp
int? result = null;
result.Should().BeNull();
result.Should().NotHaveValue();

result = 10;
result.Should().HaveValue();
```



### 11.使用扩展方法
Fluent Assertions 支持扩展方法，可以为特定类型添加自定义的断言方法。例如：

```csharp
public static class StringExtensions
{
    public static void BeUpperCased(this string subject, string because = "", params object[] becauseArgs)
    {
        subject.Should().Be(subject.ToUpper(), because, becauseArgs);
    }
}
```

使用扩展方法：

```csharp
string result = "HELLO";
result.BeUpperCased();
```



### 12.与 xUnit 集成
Fluent Assertions 与 xUnit 集成非常简单，只需要在测试项目中安装 Fluent Assertions 包即可：

```bash
dotnet add package FluentAssertions
```

然后在测试方法中使用 Fluent Assertions 的断言方法。


### 13.测试性能
可以使用 Fluent Assertions 测试代码的性能：

```csharp
Action action = () => new Calculator().CalculateSomething();
action.Should().ExecuteFasterThan(100.Milliseconds());
```



### 14.使用`Match`和`NotMatch`
可以使用正则表达式或通配符来测试字符串：

```csharp
string email = "john.doe@example.com";
email.Should().Match("*@*.com");
email.Should().NotMatch("*@*.net");
```



### 15.提供详细的错误信息
Fluent Assertions 会提供详细的错误信息，帮助快速定位问题。例如：

```csharp
string result = "This is an example";
result.Should().Be("This is a test");
```

如果断言失败，错误信息会显示：

```
Expected result to be "This is a test", but "This is an example" differs near "example" (index 16).
```

