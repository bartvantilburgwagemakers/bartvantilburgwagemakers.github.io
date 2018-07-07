---
title: UnitTests with moq in .Net
layout: post
date:   2018-07-04 20:00:01 -0600
categories: c#, .Net, Moq
tags: c# .net unitTests
---

# {{title}}

- [{{title}}](#title)
    - [To call the real method's on a object](#to-call-the-real-methodes-on-a-object)
    - [AutoMock && AutoFact](#automock--autofact)

## To call the real method's on a object

Somtimes you want to call the real implementation of a methode but mock some others.
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