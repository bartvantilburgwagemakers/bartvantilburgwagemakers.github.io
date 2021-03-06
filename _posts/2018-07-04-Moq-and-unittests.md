---
title: UnitTests
layout: post
date:   2018-07-04 20:00:01 -0600
categories: c#, .Net, Moq
tags: c# .net unitTests
---

# {{title}}

  - [{{title}}](#title)
  - [Moq examples](#moq-examples)
  - [To call the real method's on a object](#to-call-the-real-methods-on-a-object) 
  - [AutoMock && AutoFact](#automock--autofact) 
  - [Raise a event](#raise-a-event) 
  - [A object to Json](#a-object-to-json) 
  - [Map the ConsoleOutPut to a StringWriter So we can assert that](#map-the-consoleoutput-to-a-stringwriter-so-we-can-assert-that) 
  - [FluentBuilder](#fluentbuilder) 
  - [Testing of a HttpClient](#testing-of-a-httpclient) 
  - [Internals visible to](#internals-visible-to) 

It's always recommended to keep your tests small example 16 lines and readably. And the cyclomatic complexity at 1.

## Moq examples

Set a get only prop

```csharp
 instance.SetupGet(o => o.IsTerminated).Returns(true);
 ```

Make a mocked object cast-able to a other interface use:

```csharp
instance.As<IDisposable>();
```

## To call the real method's on a object

Sometimes you want to call the real implementation of a method but mock some others.
You can do this with `callbase = true` and make the other methods virtual and public.
Or internal with the internals viable to Moq [more info here](##Internals-visible-to)

```csharp
var UtilsMock = new Mock<MyClass>(){ CallBase = true};
```

## AutoMock && AutoFac

When testing method's with a lot of dependency's injected, it can be a lot of repeating work to mock them all.
To auto mock these you can use autofac + moq to auto resolve and mock some dependencies.
Make sure you are using the interface type that's you constructors are using for the provide option.

```csharp
 using (var mock = AutoMock.GetLoose())
            {
                mock.Provide<ISchedulerProvider>(new TestSchedulers());
                mock.Provide<IEventHub>(new Logic.Eventing.EventHub());
                var id = Guid.NewGuid();
                var instance = mock.Create<Instance>(new TypedParameter(typeof(Guid), Guid.NewGuid(),(new NamedParameter("id", id)));
                var registry = mock.Create<Registry>();
                Assert.IsFalse(Registry.Apps.Any());
            }
```

## Raise a event

To raise a event for a mocked object you can use

```csharp
// Raising a custom event which does not adhere to the EventHandler pattern
// Raise passing the custom arguments expected by the event delegate
    mock.Raise(foo => foo.MyEvent += null, 25, true);
```

## A object to Json

Some times you want to rerun a scenario wit input collected while debugging. You can create a test for this by writing the input object to json en read it in the unit test. ```Newtonsoft.Json.JsonConvert.SerializeObject(Object)```

## Map the ConsoleOutPut to a StringWriter So we can assert that

```csharp
using (var @out = new StringWriter())
using (var @in = new StringReader(input))
{
    Console.SetOut(@out);
    Console.SetIn(@in);
    Trace.Listeners.Add(new TextWriterTraceListener(Console.Out)); // if trace's aren't forwarded to the console

    Assert.Equal("expected", @out.ToString);
}
```

## FluentBuilder

To prevent very big test for the setup and prevent the creating of many different factory methods to create the objects.
It's nicer to make use of the [fluent builder pattern](https://martinfowler.com/bliki/FluentInterface.html) and keep this test class internal so you can only use them inside the tests.

An other way to prevent a lot of setup code the easy breaks is to make use of library's like autoMock or [autoFixture](https://github.com/AutoFixture/AutoFixture).

## Testing of a HttpClient

It has no interface so mocking will be difficult. You can overwrite the httpClientHandler that you use to create a new HttpClient.

class to test

```csharp
  internal virtual HttpClientHandler CreateClientHandler()
        {
            return new HttpClientHandler { UseDefaultCredentials = true, PreAuthenticate = true };
        }
```

test class
The using is there to prevent CA2000: Dispose objects before losing scope.

```csharp
 public class MockHandler : HttpClientHandler
    {
        protected override Task<HttpResponseMessage> SendAsync(HttpRequestMessage request, CancellationToken cancellationToken)
        {
            Task<HttpResponseMessage> result = null;
            using (var errorResponse = new HttpResponseMessage(HttpStatusCode.InternalServerError))
            {
                var completionSource = new TaskCompletionSource<HttpResponseMessage>();
                completionSource.SetResult(errorResponse);
                result = completionSource.Task;
            }

            return result;
        }
    }

    public class ProviderTest : Provider
    {
        internal override HttpClientHandler CreateClientHandler()
        {
            return new MockHandler();
        }
    }
```

## Internals visible to

 The `InternalsVisibleToAttribute` can be used when you have the source code to make internal props and method visible to a specific assembly.

[A way for when you can not use internals visible to](https://www.strathweb.com/2018/10/no-internalvisibleto-no-problem-bypassing-c-visibility-rules-with-roslyn/)

example for making it visable to the MOQ framework:

```[assembly: InternalsVisibleTo("DynamicProxyGenAssembly2")]```


## XUnit

One of the biggest advantages of Xunit instead of MsTest or NUnit is the isolation 
xUnit.net creates a new instance of the test class for every test that is run.
So any code which is placed into the constructor of the test class will be run for every single test.

Another advantage is the way of writing. That looks more like the normal way of writing C# code with a constructor instead of [TestInit] see [this post for more info](https://jamesnewkirk.typepad.com/posts/2007/09/why-you-should-.html)

### Xunit throw 

A nice notation for asserting a throwe in XUnit is: 

```csharp
    void Act() => sut.AddKindOfCupboard(null, stringBuilder);

    Assert.Throws<System.ArgumentNullException>(Act);
```

