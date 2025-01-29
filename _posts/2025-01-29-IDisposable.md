---
title: c# Dispose and GC
layout: post
date:   2025-01-29 20:00:01 -0600
categories: c#, .Net
tags: c# .Net
---

# {{title}}


Het gebruik van `Dispose` en `using` in C# is belangrijk voor het beheer van ongebruikte resources en het verbeteren van de prestaties van je applicatie. Hier is een uitleg over waarom en hoe de garbage collector hiermee omgaat:

### Het belang van de Garbage Collector in C#

De garbage collector (GC) in .NET speelt een cruciale rol in het beheer van het geheugen voor beheerde objecten. Wanneer een object niet meer wordt gebruikt, markeert de GC het als "verzamelbaar" en ruimt het op tijdens een van zijn verzamelingen. Echter, de GC weet niet hoe om te gaan met niet-beheerde resources. Daarom is het belangrijk om de Dispose-methode te implementeren en aan te roepen voor objecten die niet-beheerde resources gebruiken. 

### Waarom Dispose gebruiken?

In C# wordt de Dispose-methode gebruikt om ongebruikte resources vrij te geven, zoals bestandshandvatten, databaseverbindingen en netwerkverbindingen. Dit is vooral belangrijk voor niet-beheerde resources die niet automatisch door de garbage collector worden opgeruimd.
Wanneer je een object niet meer nodig hebt, kun je de Dispose-methode aanroepen om deze resources expliciet vrij te geven. Dit helpt om geheugenlekken te voorkomen en zorgt ervoor dat resources tijdig worden vrijgegeven.](<### Waarom Dispose gebruiken?

In C# wordt de `Dispose`-methode gebruikt om ongebruikte resources vrij te geven, zoals bestandshandvatten, databaseverbindingen en netwerkverbindingen. Dit is vooral belangrijk voor niet-beheerde resources die niet automatisch door de garbage collector worden opgeruimd[1](https://www.c-sharpcorner.com/article/dispose-and-finalize-methods-in-c-sharp/ "Dispose() and Finalize() Methods in C# - C# Corner").

Wanneer je een object niet meer nodig hebt, kun je de `Dispose`-methode aanroepen om deze resources expliciet vrij te geven. Dit helpt om geheugenlekken te voorkomen en zorgt ervoor dat resources tijdig worden vrijgegeven.>)

### Waarom using gebruiken?

De `using`-instructie zorgt ervoor dat een object dat `IDisposable` implementeert, correct wordt opgeruimd zodra het buiten scope gaat. Dit betekent dat de `Dispose`-methode automatisch wordt aangeroepen, zelfs als er een uitzondering optreedt binnen het `using`-blok[2](https://learn.microsoft.com/en-us/dotnet/csharp/language-reference/statements/using "using statement - ensure the correct use of disposable objects - C# ...").

```CSharp
using (var reader = new StreamReader("file.txt"))
{
    // Gebruik het reader-object
}
// Hier wordt reader.Dispose() automatisch aangeroepen
```

### Hoe gaat de garbage collector hiermee om?

De garbage collector (GC) in .NET beheert het geheugen voor beheerde objecten. Wanneer een object niet meer wordt gebruikt, markeert de GC het als "verzamelbaar" en ruimt het op tijdens een van zijn verzamelingen. Echter, de GC weet niet hoe om te gaan met niet-beheerde resources. Daarom is het belangrijk om de `Dispose`-methode te implementeren en aan te roepen voor objecten die niet-beheerde resources gebruiken. De `Dispose`-methode kan ook `GC.SuppressFinalize(this)` aanroepen om te voorkomen dat de GC de finalizer van het object aanroept, wat de prestaties kan verbeteren[1](https://www.c-sharpcorner.com/article/dispose-and-finalize-methods-in-c-sharp/ "Dispose() and Finalize() Methods in C# - C# Corner").

```CSharp
public class MyResource : IDisposable
{

    private bool disposed = false;
    public void Dispose()
    {
        Dispose(true); GC.SuppressFinalize(this);
    }

    protected virtual void Dispose(bool disposing)
    {
        if (!disposed)
        {
            if (disposing)
            {                // Beheer de vrijgave van beheerde resources  

            }            // Beheer de vrijgave van niet-beheerde resources  
            disposed = true;
        }
    }

    ~MyResource() { 
        Dispose(false);
     }

}
```

Door `Dispose` en `using` te gebruiken, zorg je ervoor dat resources efficiënt worden beheerd en dat je applicatie beter presteert en minder kans heeft op geheugenlekken.



### Voorbeelden van niet-beheerde resources

1. **Bestandshandvatten**: Wanneer je een bestand opent, wordt een bestandshandvat gecreëerd. Dit handvat moet worden vrijgegeven zodra je klaar bent met het bestand.
2. **Databaseverbindingen**: Verbindingen met databases zoals SQL Server of Oracle moeten worden gesloten om resources vrij te geven.
3. **Netwerkverbindingen**: Sockets en andere netwerkverbindingen moeten worden afgesloten om netwerkresources vrij te geven.
4. **GDI+ objecten**: Grafische objecten zoals penselen, pennen en afbeeldingen in Windows moeten worden vrijgegeven om geheugenlekken te voorkomen.
5. **Geheugen dat is toegewezen met `Marshal.AllocHGlobal`**: Dit geheugen moet handmatig worden vrijgegeven met `Marshal.FreeHGlobal`.

### Hoe dit de GC in de war brengt

De garbage collector in .NET beheert alleen **beheerde** objecten, zoals strings en arrays. Niet-beheerde resources vallen buiten het bereik van de GC. Als je niet-beheerde resources niet expliciet vrijgeeft, kunnen ze geheugenlekken veroorzaken en de prestaties van je applicatie negatief beïnvloeden.

Hier is een voorbeeld van hoe niet-beheerde resources de GC kunnen beïnvloeden:

```csharp
public class FileManager : IDisposable
{
    private IntPtr fileHandle;
    private bool disposed = false;

    public FileManager(string filePath)
    {
        fileHandle = CreateFile(filePath, ...); // Aanroepen van een niet-beheerde API
    }

    public void Dispose()
    {
        Dispose(true);
        GC.SuppressFinalize(this);
    }

    protected virtual void Dispose(bool disposing)
    {
        if (!disposed)
        {
            if (disposing)
            {
                // Beheer de vrijgave van beheerde resources
            }
            // Beheer de vrijgave van niet-beheerde resources
            CloseHandle(fileHandle);
            fileHandle = IntPtr.Zero;
            disposed = true;
        }
    }

    ~FileManager()
    {
        Dispose(false);
    }
}
```

In dit voorbeeld wordt een bestandshandvat (`fileHandle`) toegewezen via een niet-beheerde API. De `Dispose`-methode zorgt ervoor dat dit handvat correct wordt vrijgegeven. Als `Dispose` niet wordt aangeroepen, blijft het bestandshandvat in het geheugen, zelfs nadat het `FileManager`-object door de GC is opgeruimd. Dit kan leiden tot geheugenlekken en uitputting van systeemresources


Door `Dispose` en `using` te gebruiken, zorg je ervoor dat niet-beheerde resources tijdig worden vrijgegeven, wat de stabiliteit en prestaties van je applicatie verbetert.


Objecten die niet goe worden opgeruimt kunnen blijven hangen in momory zonder dat ze ooit worden opgeruimt. En dan kunnen ze ook nog dingen hebben gelock en in gebruik hebben 


[IDisposable Best Practices for C# Developers | Integrated Learning Experience](https://app.pluralsight.com/ilx/video-courses/58f4916e-10df-4cb8-bd58-a2b291fb9c4b/9543e4d3-a058-4b42-a22e-77df55202e7a/f323ca9e-7b5b-4b77-ba48-bbff57196536)

voorbeelde van unmanged code (buiten de eigen app )
![[../assets/unmangedObjects.png]]

# hoe using eigelijk werkt 

dit 
```csharp
static void Main()
{
    using (var obj = new Custom())
    {
        obj.Method();
    }
}

```
wordt door de compiler het zelfde behandeld als dit : 
```csharp 
static void Main()
{
    var obj = new Custom();

    try
    {
        obj.Method();
    }
    finally
    {
        obj.Dispose();
    }
}

```


# demo movie resource exsoing 
https://app.pluralsight.com/ilx/video-courses/58f4916e-10df-4cb8-bd58-a2b291fb9c4b/9543e4d3-a058-4b42-a22e-77df55202e7a/1f193b80-4a32-453d-84c8-b633c757394a


(https://learn.microsoft.com/en-us/dotnet/csharp/language-reference/statements/using "using statement - ensure the correct use of disposable objects - C# ..."): [using statement - ensure the correct use of disposable objects - C# reference | Microsoft Learn](https://learn.microsoft.com/en-us/dotnet/csharp/language-reference/statements/using)[1](https://www.c-sharpcorner.com/article/dispose-and-finalize-methods-in-c-sharp/ "Dispose() and Finalize() Methods in C# - C# Corner"): [Dispose() and Finalize() Methods in C# - C# Corner](https://www.c-sharpcorner.com/article/dispose-and-finalize-methods-in-c-sharp/)

[Niet-beheerde typen - C# reference | Microsoft Learn](https://learn.microsoft.com/nl-nl/dotnet/csharp/language-reference/builtin-types/unmanaged-types) 

 [C# and Resource Management: Best Practices for Handling Unmanaged Resources](https://lucshelton.com/blog/c-and-resource-management-best-practices-for-handling-unmanaged-resources/)
