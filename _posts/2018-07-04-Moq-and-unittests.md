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
    - [Raise a event](#raise-a-event)
    - [A object to Json.](#a-object-to-json)

## To call the real method's on a object

Sometimes you want to call the real implementation of a methode but mock some others.
You can do this with `callbase = true ` and make the other methodes virtual.

```csharp
var brockerUtilsMock = new Mock<BrockerUtils>(){ CallBase = true};
```

## AutoMock && AutoFact

When testing method's with a lot of dependency's injected, it can be a lot of repeating work to mock them all.
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

## Raise a event

To raise a event for a mocked object you can use 

```csharp
// Raising a custom event which does not adhere to the EventHandler pattern
// Raise passing the custom arguments expected by the event delegate
    mock.Raise(foo => foo.MyEvent += null, 25, true);
```

## A object to Json.

Some times you want to rerun a senario wit input collected while debugging. You can create a test for this by writing the input object to json en read it in the unit test. ```Newtonsoft.Json.JsonConvert.SerializeObject(Object)``` 
