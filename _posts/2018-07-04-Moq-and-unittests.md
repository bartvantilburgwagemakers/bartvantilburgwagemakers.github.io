---
title: UnitTests with moq in .Net
layout: post
date:   2018-07-04 20:00:01 -0600
categories: c#, .Net, Moq
tags: c# .net unitTests
---

# {{title}}

- [{{title}}](#title)
    - [To call the real methodes on a object](#to-call-the-real-methodes-on-a-object)
    - [AutoMock && AutoFact](#automock--autofact)
    - [Map the ConsoleOutPut to a StringWriter So we can assert that](#map-the-consoleoutput-to-a-stringwriter-so-we-can-assert-that)

## To call the real methodes on a object

Somtimes you want to call the real implementation of a methode but mock some others.
You can do this with `callbase = true ` and make the other methodes virtual.

```csharp
var brockerUtilsMock = new Mock<BrockerUtils>(){ CallBase = true};
```

## AutoMock && AutoFact

When testing methodes with a lot of dependency's injected, it can be a lot of repeating work to mock them all.
To auto mock these you can use autofac + moq to auto resolve and mock some dependencies.

```csharp
 using (var mock = AutoMock.GetLoose())
            {
                mock.Provide<ISchedulerProvider>(new TestSchedulers());
                mock.Provide<IEventHub>(new Logic.Eventing.EventHub());
                var hostedAppRegistry = mock.Create<HostedAppRegistry>();
                Assert.IsFalse(hostedAppRegistry.Apps.Any());
            }
```

## Map the ConsoleOutPut to a StringWriter So we can assert that

```csharp
using (var @out = new StringWriter())
using (var @in = new StringReader(input))
{
    Console.SetOut(@out);
    Console.SetIn(@in);

    Assert.Equal("expected", @out.ToString);
}
```