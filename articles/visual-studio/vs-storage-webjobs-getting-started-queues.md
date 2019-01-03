---
title: Aan de slag met queue storage en Visual Studio verbonden services (webtaak projecten) | Microsoft Docs
description: Aan de slag met Azure Queue storage in een project webtaak nadat u verbinding met een opslagaccount met behulp van Visual Studio verbonden services.
services: storage
author: ghogen
manager: douge
ms.assetid: 5c3ef267-2a67-44e9-ab4a-1edd7015034f
ms.prod: visual-studio-dev15
ms.technology: vs-azure
ms.custom: vs-azure
ms.workload: azure-vs
ms.topic: article
ms.date: 12/02/2016
ms.author: ghogen
ms.openlocfilehash: 74aea3ad4c3dda8abc69275ad4d683fbcf485ccc
ms.sourcegitcommit: 549070d281bb2b5bf282bc7d46f6feab337ef248
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/21/2018
ms.locfileid: "53722903"
---
# <a name="getting-started-with-azure-queue-storage-and-visual-studio-connected-services-webjob-projects"></a>Aan de slag met Azure Queue storage en Visual Studio verbonden services (webtaak projecten)
[!INCLUDE [storage-try-azure-tools-queues](../../includes/storage-try-azure-tools-queues.md)]

## <a name="overview"></a>Overzicht
Dit artikel wordt beschreven hoe aan de slag met Azure Queue storage in een Azure-webtaak voor Visual Studio-project nadat u hebt gemaakt of een Azure storage-account waarnaar wordt verwezen door het gebruik van de Visual Studio **Connected Services toevoegen** in het dialoogvenster. Wanneer u een opslagaccount toevoegt aan een WebJob-project met behulp van de Visual Studio **Connected Services toevoegen** dialoogvenster, de juiste Azure Storage NuGet-pakketten zijn geïnstalleerd, de juiste .NET-verwijzingen worden toegevoegd aan het project en verbindingsreeksen voor de storage-account worden bijgewerkt in het bestand App.config.  

In dit artikel vindt u C#-codevoorbeelden die laten zien hoe de Azure WebJobs SDK-versie 1.x met de Azure Queue storage-service.

Azure Queue Storage is een service voor de opslag van grote aantallen berichten die via HTTP of HTTPS overal vandaan kunnen worden opgevraagd met geverifieerde aanroepen. Een enkel wachtrijbericht mag maximaal 64 KB groot zijn en een wachtrij kan miljoenen berichten bevatten, tot de totale capaciteitslimiet van een opslagaccount. Zie [aan de slag met Azure Queue Storage met .NET](../storage/queues/storage-dotnet-how-to-use-queues.md) voor meer informatie. Zie voor meer informatie over ASP.NET [ASP.NET](http://www.asp.net).

## <a name="how-to-trigger-a-function-when-a-queue-message-is-received"></a>Hoe u kunt een functie activeren wanneer er een wachtrijbericht is ontvangen
Voor het schrijven van een functie waarmee de WebJobs SDK wordt aangeroepen wanneer er een wachtrijbericht is ontvangen, gebruiken de **QueueTrigger** kenmerk. De constructor kenmerk heeft een queryreeks-parameter met de naam van de wachtrij te peilen. Bekijk voor meer informatie over het instellen van de naam van de wachtrij dynamisch, [over het instellen van configuratie-opties](#how-to-set-configuration-options).

### <a name="string-queue-messages"></a>Tekenreeks-berichtenwachtrij-berichten
In het volgende voorbeeld bevat de wachtrij een Tekenreeksbericht dus **QueueTrigger** wordt toegepast op een queryreeks-parameter met de naam **logMessage** die de inhoud van het wachtrijbericht bevat. De functie [een logboekbericht schrijft naar het Dashboard](#how-to-write-logs).

```csharp
public static void ProcessQueueMessage([QueueTrigger("logqueue")] string logMessage, TextWriter logger)
{
    logger.WriteLine(logMessage);
}
```

Naast **tekenreeks**, de parameter een matrix van bytes, kan niet een **CloudQueueMessage** object of een POCO die u definieert.

### <a name="poco-plain-old-clr-objecthttpenwikipediaorgwikiplainoldclrobject-queue-messages"></a>POCO [(Plain oude CLR-Object](http://en.wikipedia.org/wiki/Plain_Old_CLR_Object)) berichten in de wachtrij
In het volgende voorbeeld bevat het wachtrijbericht JSON voor een **BlobInformation** object waarin een **BlobName** eigenschap. De SDK gedeserialiseerd automatisch het object.

```csharp
public static void WriteLogPOCO([QueueTrigger("logqueue")] BlobInformation blobInfo, TextWriter logger)
{
    logger.WriteLine("Queue message refers to blob: " + blobInfo.BlobName);
}
```

De SDK gebruikt de [NuGet-pakket Newtonsoft.Json](http://www.nuget.org/packages/Newtonsoft.Json) te serialiseren en deserialiseren van berichten. Als u berichten in wachtrij plaatsen in een programma dat geen gebruik maakt van de WebJobs SDK maakt, kunt u code als in het volgende voorbeeld om een wachtrijbericht POCO die kan worden geparseerd met de SDK te maken.

```csharp
BlobInformation blobInfo = new BlobInformation() { BlobName = "log.txt" };
var queueMessage = new CloudQueueMessage(JsonConvert.SerializeObject(blobInfo));
logQueue.AddMessage(queueMessage);
```

### <a name="async-functions"></a>Async-functies
De volgende asynchrone functie [een logboek geschreven naar het Dashboard](#how-to-write-logs).

```csharp
public async static Task ProcessQueueMessageAsync([QueueTrigger("logqueue")] string logMessage, TextWriter logger)
{
    await logger.WriteLineAsync(logMessage);
}
```

Async-functies kunnen duren voordat een [annulering token](http://www.asp.net/mvc/overview/performance/using-asynchronous-methods-in-aspnet-mvc-4#CancelToken), zoals weergegeven in het volgende voorbeeld wordt een blob gekopieerd. (Voor een uitleg van de **queueTrigger** tijdelijke aanduiding, Zie de [Blobs](#how-to-read-and-write-blobs-and-tables-while-processing-a-queue-message) sectie.)

```csharp
public async static Task ProcessQueueMessageAsyncCancellationToken(
    [QueueTrigger("blobcopyqueue")] string blobName,
    [Blob("textblobs/{queueTrigger}",FileAccess.Read)] Stream blobInput,
    [Blob("textblobs/{queueTrigger}-new",FileAccess.Write)] Stream blobOutput,
    CancellationToken token)
{
    await blobInput.CopyToAsync(blobOutput, 4096, token);
}
```

## <a name="types-the-queuetrigger-attribute-works-with"></a>Het kenmerk QueueTrigger met werkt typen
U kunt **QueueTrigger** met de volgende typen:

* **Tekenreeks**
* Een POCO-type dat is geserialiseerd als JSON
* **byte[]**
* **CloudQueueMessage**

## <a name="polling-algorithm"></a>Polling-algoritme
De SDK implementeert een willekeurig exponentieel uitstel algoritme voor het effect van niet-actieve wachtrij polling op de opslagkosten van de transactie wordt beperkt.  Wanneer een bericht wordt gevonden, de SDK Wacht twee seconden en controleert vervolgens of er een andere message; Wacht ongeveer vier seconden voordat u probeert het opnieuw wanneer er geen bericht is gevonden. Na opeenvolgende mislukte pogingen om een wachtrijbericht, blijft de wachttijd vergroten totdat de maximale wachttijd, die standaard één minuut is bereikt. [De maximale wachttijd is configureerbaar](#how-to-set-configuration-options).

## <a name="multiple-instances"></a>Meerdere exemplaren
Als uw web-app wordt uitgevoerd op meerdere exemplaren, een doorlopende webtaken wordt uitgevoerd op elke computer elke machine wordt gewacht op triggers en probeert uit te voeren van functies. In sommige scenario's die dit kan leiden tot sommige functies die dezelfde gegevens tweemaal verwerken, dus functies moet idempotent zijn (geschreven zodat ze herhaaldelijk aanroepen met de dezelfde invoergegevens geen dubbele resultaten levert).  

## <a name="parallel-execution"></a>Parallelle uitvoering
Als u meerdere functies luisteren naar verschillende wachtrijen hebt, wordt de SDK parallel aanroepen wanneer berichten tegelijkertijd worden ontvangen.

Hetzelfde geldt wanneer meerdere berichten worden ontvangen voor één wachtrij. Standaard wordt met de SDK opgehaald van een batch van 16-berichtenwachtrij-berichten op een tijdstip en voert de functie die parallel worden verwerkt. [De batchgrootte is configureerbaar](#how-to-set-configuration-options). Wanneer het aantal dat wordt verwerkt, omlaag aan de helft van de batchgrootte ontvangt, wordt de SDK opgehaald van een andere batch en start deze berichten worden verwerkt. Daarom is het maximum aantal gelijktijdige berichten worden verwerkt per functie en een halve keer de batchgrootte. Deze limiet geldt afzonderlijk voor elke functie heeft een **QueueTrigger** kenmerk. Als u niet dat parallelle uitvoering voor berichten in een wachtrij ontvangen wilt, moet u de batchgrootte instellen op 1.

## <a name="get-queue-or-queue-message-metadata"></a>Wachtrij of metagegevens in de wachtrij bericht ophalen
Door parameters toe te voegen aan de methodehandtekening krijgt u de eigenschappen van de volgende berichten:

* **DateTimeOffset** expirationTime
* **DateTimeOffset** insertionTime
* **DateTimeOffset** nextVisibleTime
* **tekenreeks** queueTrigger (bevat tekst)
* **tekenreeks** id
* **tekenreeks** popReceipt
* **int** dequeueCount

Als u werken rechtstreeks met de Azure storage-API wilt, u kunt ook toevoegen een **CloudStorageAccount** parameter.

Het volgende voorbeeld schrijft al deze metagegevens in een logboek van de toepassing gegevens. In het voorbeeld bevatten logMessage zowel queueTrigger van de inhoud van het bericht uit de wachtrij.

```csharp
public static void WriteLog([QueueTrigger("logqueue")] string logMessage,
    DateTimeOffset expirationTime,
    DateTimeOffset insertionTime,
    DateTimeOffset nextVisibleTime,
    string id,
    string popReceipt,
    int dequeueCount,
    string queueTrigger,
    CloudStorageAccount cloudStorageAccount,
    TextWriter logger)
{
    logger.WriteLine(
        "logMessage={0}\n" +
        "expirationTime={1}\ninsertionTime={2}\n" +
        "nextVisibleTime={3}\n" +
        "id={4}\npopReceipt={5}\ndequeueCount={6}\n" +
        "queue endpoint={7} queueTrigger={8}",
        logMessage, expirationTime,
        insertionTime,
        nextVisibleTime, id,
        popReceipt, dequeueCount,
        cloudStorageAccount.QueueEndpoint,
        queueTrigger);
}
```

Hier volgt een voorbeeldlogboek geschreven door de voorbeeldcode:

        logMessage=Hello world!
        expirationTime=10/14/2014 10:31:04 PM +00:00
        insertionTime=10/7/2014 10:31:04 PM +00:00
        nextVisibleTime=10/7/2014 10:41:23 PM +00:00
        id=262e49cd-26d3-4303-ae88-33baf8796d91
        popReceipt=AgAAAAMAAAAAAAAAfc9H0n/izwE=
        dequeueCount=1
        queue endpoint=https://contosoads.queue.core.windows.net/
        queueTrigger=Hello world!

## <a name="graceful-shutdown"></a>Correct afsluiten
Een functie die wordt uitgevoerd in een doorlopende webtaak accepteert een **CancellationToken** parameter waarmee het besturingssysteem naar de functie een melding wanneer de webtaak wordt beëindigd. U kunt deze melding kunt gebruiken om te controleren of dat de functie niet onverwacht wordt beëindigd op een manier die blijven de gegevens in een inconsistente status.

Het volgende voorbeeld laat zien hoe om te controleren voor beëindiging van een aanstaande WebJob in een functie.

```csharp
public static void GracefulShutdownDemo(
            [QueueTrigger("inputqueue")] string inputText,
            TextWriter logger,
            CancellationToken token)
{
    for (int i = 0; i < 100; i++)
    {
        if (token.IsCancellationRequested)
        {
            logger.WriteLine("Function was cancelled at iteration {0}", i);
            break;
        }
        Thread.Sleep(1000);
        logger.WriteLine("Normal processing for queue message={0}", inputText);
    }
}
```

**Opmerking:** Het Dashboard mogelijk niet correct weergegeven en de uitvoer van de functies die zijn afgesloten.

Zie voor meer informatie, [WebJobs correct afsluiten](http://blog.amitapple.com/post/2014/05/webjobs-graceful-shutdown/#.VCt1GXl0wpR).   

## <a name="how-to-create-a-queue-message-while-processing-a-queue-message"></a>Over het maken van een wachtrijbericht tijdens het verwerken van een wachtrijbericht
Een functie waarmee een nieuw wachtrijbericht worden geschreven, gebruikt u de **wachtrij** kenmerk. Zoals **QueueTrigger**, geeft u de naam van de wachtrij als een tekenreeks of u kunt [stelt de naam van de wachtrij dynamisch](#how-to-set-configuration-options).

### <a name="string-queue-messages"></a>Tekenreeks-berichtenwachtrij-berichten
Het volgende niet-async-codevoorbeeld maakt een nieuw wachtrijbericht in de wachtrij met de naam 'outputqueue' met dezelfde inhoud als de wachtrijbericht wordt ontvangen in de wachtrij met de naam 'inputqueue'. (Voor asynchrone functies gebruiken **IAsyncCollector<T>**  zoals later in deze sectie.)

```csharp
public static void CreateQueueMessage(
    [QueueTrigger("inputqueue")] string queueMessage,
    [Queue("outputqueue")] out string outputQueueMessage )
{
    outputQueueMessage = queueMessage;
}
```

### <a name="poco-plain-old-clr-objecthttpenwikipediaorgwikiplainoldclrobject-queue-messages"></a>POCO [(Plain oude CLR-Object](http://en.wikipedia.org/wiki/Plain_Old_CLR_Object)) berichten in de wachtrij
Voor het maken van een wachtrijbericht met een POCO in plaats van een tekenreeks, geeft u het type POCO als een output-parameter op de **wachtrij** kenmerkconstructie.

```csharp
public static void CreateQueueMessage(
    [QueueTrigger("inputqueue")] BlobInformation blobInfoInput,
    [Queue("outputqueue")] out BlobInformation blobInfoOutput )
{
    blobInfoOutput = blobInfoInput;
}
```

De SDK serialiseert automatisch het object naar JSON. Een wachtrijbericht is altijd gemaakt, zelfs als het object null is.

### <a name="create-multiple-messages-or-in-async-functions"></a>Meerdere berichten maken of in de asynchrone functies
Zorg voor het maken van meerdere berichten, het parametertype voor de uitvoerwachtrij **ICollector<T>**  of **IAsyncCollector<T>**, zoals wordt weergegeven in het volgende voorbeeld.

```csharp
public static void CreateQueueMessages(
    [QueueTrigger("inputqueue")] string queueMessage,
    [Queue("outputqueue")] ICollector<string> outputQueueMessage,
    TextWriter logger)
{
    logger.WriteLine("Creating 2 messages in outputqueue");
    outputQueueMessage.Add(queueMessage + "1");
    outputQueueMessage.Add(queueMessage + "2");
}
```

Het bericht voor elke wachtrij wordt onmiddellijk gemaakt wanneer de **toevoegen** methode wordt aangeroepen.

### <a name="types-that-the-queue-attribute-works-with"></a>Typen die geschikt is voor het wachtrij-kenmerk
U kunt de **wachtrij** kenmerk voor de volgende parameter:

* **uit de tekenreeks** (wachtrijbericht maakt als waarde voor de parameter niet gelijk zijn aan nul is wanneer de functie wordt beëindigd)
* **uit byte []** (werkt als **tekenreeks**)
* **uit CloudQueueMessage** (werkt als **tekenreeks**)
* **uit POCO** (een serialiseerbaar type, maakt u een bericht met een null-object als de parameter is null wanneer de functie wordt beëindigd)
* **ICollector**
* **IAsyncCollector**
* **CloudQueue** (voor het maken van berichten handmatig rechtstreeks met de API van Azure Storage)

### <a name="use-webjobs-sdk-attributes-in-the-body-of-a-function"></a>Kenmerken in de hoofdtekst van de functie WebJobs SDK gebruiken
Als u ook werken in uw functie wilt voordat u met behulp van een WebJobs SDK-kenmerk zoals **wachtrij**, **Blob**, of **tabel**, kunt u de **IBinder**interface.

Het volgende voorbeeld wordt een bericht invoerwachtrij en maakt u een nieuw bericht met de inhoud van een uitvoerwachtrij. De naam van de uitvoer-wachtrij wordt ingesteld door de code in de hoofdtekst van de functie.

```csharp
public static void CreateQueueMessage(
    [QueueTrigger("inputqueue")] string queueMessage,
    IBinder binder)
{
    string outputQueueName = "outputqueue" + DateTime.Now.Month.ToString();
    QueueAttribute queueAttribute = new QueueAttribute(outputQueueName);
    CloudQueue outputQueue = binder.Bind<CloudQueue>(queueAttribute);
    outputQueue.AddMessage(new CloudQueueMessage(queueMessage));
}
```

De **IBinder** interface kan ook worden gebruikt met de **tabel** en **Blob** kenmerken.

## <a name="how-to-read-and-write-blobs-and-tables-while-processing-a-queue-message"></a>Het lezen en schrijven van blobs en tabellen tijdens het verwerken van een wachtrijbericht
De **Blob** en **tabel** kenmerken kunnen u lezen en schrijven van blobs en tabellen. De voorbeelden in deze sectie zijn van toepassing op blobs. Zie voor voorbeelden van code die laten hoe u processen activeren zien wanneer blobs worden gemaakt of bijgewerkt, [Azure blob storage gebruiken met de WebJobs SDK](https://github.com/Azure/azure-webjobs-sdk/wiki).
<!-- , and for code samples that read and write tables, see [How to use Azure table storage with the WebJobs SDK](../app-service-web/websites-dotnet-webjobs-sdk-storage-tables-how-to.md). -->

### <a name="string-queue-messages-triggering-blob-operations"></a>Tekenreeks Wachtrijberichten activeren van de blob-bewerkingen
Voor een wachtrijbericht met een tekenreeks, **queueTrigger** is een tijdelijke aanduiding die u kunt gebruiken in de **Blob** van kenmerk **blobPath** parameter met de inhoud van de Bericht.

Het volgende voorbeeld wordt **Stream** objecten om te lezen en schrijven van blobs. Bericht uit de wachtrij is de naam van een blob zich in de container textblobs. Een kopie van de blob met '-nieuwe ' toegevoegd aan de naam in dezelfde container is gemaakt.

```csharp
public static void ProcessQueueMessage(
    [QueueTrigger("blobcopyqueue")] string blobName,
    [Blob("textblobs/{queueTrigger}",FileAccess.Read)] Stream blobInput,
    [Blob("textblobs/{queueTrigger}-new",FileAccess.Write)] Stream blobOutput)
{
    blobInput.CopyTo(blobOutput, 4096);
}
```

De **Blob** kenmerk constructor duurt een **blobPath** parameter waarmee de naam van de container- en blobnaam. Zie voor meer informatie over deze tijdelijke aanduiding [Azure blob storage gebruiken met de WebJobs SDK](https://github.com/Azure/azure-webjobs-sdk/wiki).

Wanneer het kenmerk wordt verfraaid een **Stream** -object, een andere constructorparameter geeft u de **FileAccess** modus als lezen, schrijven of lezen/schrijven.

Het volgende voorbeeld wordt een **CloudBlockBlob** object verwijderen van een blob. Bericht uit de wachtrij is de naam van de blob.

```csharp
public static void DeleteBlob(
    [QueueTrigger("deleteblobqueue")] string blobName,
    [Blob("textblobs/{queueTrigger}")] CloudBlockBlob blobToDelete)
{
    blobToDelete.Delete();
}
```

### <a name="poco-plain-old-clr-objecthttpenwikipediaorgwikiplainoldclrobject-queue-messages"></a>POCO [(Plain oude CLR-Object](http://en.wikipedia.org/wiki/Plain_Old_CLR_Object)) berichten in de wachtrij
Voor een POCO opgeslagen als JSON in het wachtrijbericht, kunt u tijdelijke aanduidingen die de naam van de eigenschappen van het object in de **wachtrij** van kenmerk **blobPath** parameter. U kunt wachtrijnamen metagegevens eigenschap ook gebruiken als tijdelijke aanduidingen. Zie [wachtrij of metagegevens in de wachtrij bericht](#get-queue-or-queue-message-metadata).

Het volgende voorbeeld wordt een blob gekopieerd naar een nieuwe blob met een andere extensie. Bericht uit de wachtrij is een **BlobInformation** -object met **BlobName** en **BlobNameWithoutExtension** eigenschappen. De namen van eigenschappen worden gebruikt als tijdelijke aanduidingen in de blobpad voor de **Blob** kenmerken.

```csharp
public static void CopyBlobPOCO(
    [QueueTrigger("copyblobqueue")] BlobInformation blobInfo,
    [Blob("textblobs/{BlobName}", FileAccess.Read)] Stream blobInput,
    [Blob("textblobs/{BlobNameWithoutExtension}.txt", FileAccess.Write)] Stream blobOutput)
{
    blobInput.CopyTo(blobOutput, 4096);
}
```

De SDK gebruikt de [NuGet-pakket Newtonsoft.Json](http://www.nuget.org/packages/Newtonsoft.Json) te serialiseren en deserialiseren van berichten. Als u berichten in wachtrij plaatsen in een programma dat geen gebruik maakt van de WebJobs SDK maakt, kunt u code als in het volgende voorbeeld om een wachtrijbericht POCO die kan worden geparseerd met de SDK te maken.

```csharp
BlobInformation blobInfo = new BlobInformation() { BlobName = "boot.log", BlobNameWithoutExtension = "boot" };
var queueMessage = new CloudQueueMessage(JsonConvert.SerializeObject(blobInfo));
logQueue.AddMessage(queueMessage);
```

Als u werken in de functie wilt voor het binden van een blob aan een object, kunt u het kenmerk in de hoofdtekst van de functie, zoals wordt weergegeven in [kenmerken van de WebJobs SDK gebruiken in de hoofdtekst van een functie](#use-webjobs-sdk-attributes-in-the-body-of-a-function).

### <a name="types-you-can-use-the-blob-attribute-with"></a>U kunt het Blob-kenmerk met de typen
De **Blob** kenmerk kan worden gebruikt met de volgende typen:

* **Stream** (lezen of schrijven, opgegeven met behulp van de constructorparameter FileAccess)
* **TextReader**
* **TextWriter**
* **tekenreeks** (lezen)
* **uit de tekenreeks** (schrijven, maakt u een blob alleen als de queryreeks-parameter niet gelijk zijn aan nul is wanneer de functie geeft als resultaat)
* POCO (lezen)
* uit POCO (schrijven; altijd wordt een blob gemaakt, wordt als null-object worden gemaakt als POCO-parameter null is wanneer de functie geeft als resultaat)
* **CloudBlobStream** (schrijven)
* **ICloudBlob** (lezen en schrijven)
* **CloudBlockBlob** (lezen en schrijven)
* **CloudPageBlob** (lezen en schrijven)

## <a name="how-to-handle-poison-messages"></a>Hoe beheer van Onverwerkbare berichten worden verwerkt
Berichten waarvan de inhoud zorgt ervoor een functie dat mislukken heten *Onverwerkbare berichten*. Als de functie is mislukt, wordt het wachtrijbericht niet worden verwijderd, en uiteindelijk wordt opgehaald, waardoor de cyclus en worden herhaald. De SDK kan automatisch de cyclus worden onderbroken na een beperkt aantal herhalingen of u kunt dit handmatig doen.

### <a name="automatic-poison-message-handling"></a>Automatisch beheer van Onverwerkbare berichten verwerken
De SDK roept een functie maximaal 5 keer een wachtrijbericht te verwerken. Als de vijfde poging mislukt, wordt het bericht verplaatst naar een onverwerkbare wachtrij. U kunt zien hoe het configureren van het maximum aantal nieuwe pogingen in [over het instellen van configuratie-opties](#how-to-set-configuration-options).

De naam van het beheer van onverwerkbare wachtrij *{originalqueuename}*-onverwerkbare. U kunt schrijven om een functie voor het verwerken van berichten uit de wachtrij onverwerkbare door registratie of een melding verzonden dat handmatige aandacht is vereist.

In het volgende voorbeeld de **CopyBlob** functie mislukken wanneer er een wachtrijbericht bevat de naam van een blob die niet bestaat. Wanneer dit gebeurt, wordt het bericht uit de wachtrij copyblobqueue verplaatst naar de wachtrij copyblobqueue poison. De **ProcessPoisonMessage** meldt zich vervolgens aan het beheer van Onverwerkbare berichten.

```csharp
public static void CopyBlob(
    [QueueTrigger("copyblobqueue")] string blobName,
    [Blob("textblobs/{queueTrigger}", FileAccess.Read)] Stream blobInput,
    [Blob("textblobs/{queueTrigger}-new", FileAccess.Write)] Stream blobOutput)
{
    blobInput.CopyTo(blobOutput, 4096);
}

public static void ProcessPoisonMessage(
    [QueueTrigger("copyblobqueue-poison")] string blobName, TextWriter logger)
{
    logger.WriteLine("Failed to copy blob, name=" + blobName);
}
```

De volgende afbeelding ziet u console-uitvoer van deze functies wanneer een beheer van Onverwerkbare berichten worden verwerkt.

![Console-uitvoer voor het beheer van Onverwerkbare berichten afhandelen](./media/vs-storage-webjobs-getting-started-queues/poison.png)

### <a name="manual-poison-message-handling"></a>Handmatige beheer van Onverwerkbare berichten verwerken
U kunt het aantal keren dat een bericht is opgehaald voor de verwerking van krijgen door het toevoegen van een **int** parameter met de naam **dequeueCount** naar uw functie. Vervolgens kunt u het aantal uit de wachtrij verwijderen in de functiecode controleren en uitvoeren van uw eigen beheer van Onverwerkbare berichten verwerken wanneer het aantal groter is dan een drempelwaarde, zoals wordt weergegeven in het volgende voorbeeld.

```csharp
public static void CopyBlob(
    [QueueTrigger("copyblobqueue")] string blobName, int dequeueCount,
    [Blob("textblobs/{queueTrigger}", FileAccess.Read)] Stream blobInput,
    [Blob("textblobs/{queueTrigger}-new", FileAccess.Write)] Stream blobOutput,
    TextWriter logger)
{
    if (dequeueCount > 3)
    {
        logger.WriteLine("Failed to copy blob, name=" + blobName);
    }
    else
    {
        blobInput.CopyTo(blobOutput, 4096);
    }
}
```

## <a name="how-to-set-configuration-options"></a>Over het instellen van configuratie-opties
U kunt de **JobHostConfiguration** type om in te stellen van de volgende configuratieopties:

* De SDK-verbindingsreeksen instellen in code.
* Configureer **QueueTrigger** -instellingen, zoals het maximum aantal uit de wachtrij verwijderen.
* Wachtrijnamen van de ophalen uit de configuratie van.

### <a name="set-sdk-connection-strings-in-code"></a>SDK-verbindingsreeksen instellen in code
De SDK-verbindingsreeksen instellen in code kunt u uw eigen namen van de tekenreeks verbindingen in configuratiebestanden of omgevingsvariabelen, zoals wordt weergegeven in het volgende voorbeeld.

```csharp
static void Main(string[] args)
{
    var _storageConn = ConfigurationManager
        .ConnectionStrings["MyStorageConnection"].ConnectionString;

    var _dashboardConn = ConfigurationManager
        .ConnectionStrings["MyDashboardConnection"].ConnectionString;

    var _serviceBusConn = ConfigurationManager
        .ConnectionStrings["MyServiceBusConnection"].ConnectionString;

    JobHostConfiguration config = new JobHostConfiguration();
    config.StorageConnectionString = _storageConn;
    config.DashboardConnectionString = _dashboardConn;
    config.ServiceBusConnectionString = _serviceBusConn;
    JobHost host = new JobHost(config);
    host.RunAndBlock();
}
```

### <a name="configure-queuetrigger--settings"></a>QueueTrigger-instellingen configureren
U kunt de volgende instellingen die betrekking hebben op de verwerking van de wachtrij berichten configureren:

* Het maximum aantal Wachtrijberichten die tegelijkertijd worden doorgevoerd parallel worden uitgevoerd (de standaardinstelling is 16).
* Het maximum aantal pogingen alvorens een wachtrijbericht is verzonden naar een onverwerkbare wachtrij (de standaardwaarde is 5).
* De maximale wachttijd voor navragen opnieuw wanneer een wachtrij is leeg (de standaardwaarde is 1 minuut).

Het volgende voorbeeld laat zien hoe deze instellingen configureren:

```csharp
static void Main(string[] args)
{
    JobHostConfiguration config = new JobHostConfiguration();
    config.Queues.BatchSize = 8;
    config.Queues.MaxDequeueCount = 4;
    config.Queues.MaxPollingInterval = TimeSpan.FromSeconds(15);
    JobHost host = new JobHost(config);
    host.RunAndBlock();
}
```

### <a name="set-values-for-webjobs-sdk-constructor-parameters-in-code"></a>Stel waarden voor WebJobs SDK parameters van constructor in code
Soms wilt u de naam van een wachtrij, een blob-naam of container opgeven of een tabel in de code in plaats van programmeren Geef deze de naam. Bijvoorbeeld, u misschien wilt opgeven van de naam van de wachtrij voor **QueueTrigger** in een configuratie-bestand of de omgeving variabele.

U kunt dit doen door te geven in een **NameResolver** object toe aan de **JobHostConfiguration** type. U speciale tijdelijke aanduidingen tussen procent (%) zich in de WebJobs SDK kenmerk constructor parameters bevatten en uw **NameResolver** code Hiermee geeft u de werkelijke waarden moet worden gebruikt in plaats van de tijdelijke aanduidingen.

Stel bijvoorbeeld dat u wilt gebruiken van een wachtrij met de naam logqueuetest in de testomgeving en één benoemde logqueueprod in productie. In plaats van een vastgelegde wachtrijnaam die u wilt opgeven van de naam van een vermelding in de **appSettings** verzameling die de naam van de werkelijke wachtrij zou hebben. Als de **appSettings** sleutel logqueue is, de functie kan eruitzien als in het volgende voorbeeld.

```csharp
public static void WriteLog([QueueTrigger("%logqueue%")] string logMessage)
{
    Console.WriteLine(logMessage);
}
```

Uw **NameResolver** klasse kan vervolgens de naam van de wachtrij van ophalen **appSettings** zoals wordt weergegeven in het volgende voorbeeld:

```csharp
public class QueueNameResolver : INameResolver
{
    public string Resolve(string name)
    {
        return ConfigurationManager.AppSettings[name].ToString();
    }
}
```

Geeft u de **NameResolver** in klasse aan de **JobHost** object, zoals wordt weergegeven in het volgende voorbeeld.

```csharp
static void Main(string[] args)
{
    JobHostConfiguration config = new JobHostConfiguration();
    config.NameResolver = new QueueNameResolver();
    JobHost host = new JobHost(config);
    host.RunAndBlock();
}
```

**Opmerking:** Queue, table en blob-namen worden opgelost, telkens wanneer een functie is aangeroepen, maar namen van de blob-containers worden opgelost, alleen wanneer de toepassing wordt gestart. U kunt de naam van de blob-container niet wijzigen terwijl de taak wordt uitgevoerd.

## <a name="how-to-trigger-a-function-manually"></a>Hoe u een functie handmatig activeren
Een functie als handmatig wilt activeren, gebruikt u de **aanroepen** of **CallAsync** methode voor het **JobHost** object en de **NoAutomaticTrigger** kenmerk van de functie, zoals wordt weergegeven in het volgende voorbeeld.

```csharp
public class Program
{
    static void Main(string[] args)
    {
        JobHost host = new JobHost();
        host.Call(typeof(Program).GetMethod("CreateQueueMessage"), new { value = "Hello world!" });
    }

    [NoAutomaticTrigger]
    public static void CreateQueueMessage(
        TextWriter logger,
        string value,
        [Queue("outputqueue")] out string message)
    {
        message = value;
        logger.WriteLine("Creating queue message: ", message);
    }
}
```

## <a name="how-to-write-logs"></a>Over het schrijven van Logboeken
Het Dashboard ziet u Logboeken op twee plaatsen: de pagina voor de webtaak en op de pagina voor een specifieke WebJob-aanroep.

![Op de pagina WebJob-Logboeken](./media/vs-storage-webjobs-getting-started-queues/dashboardapplogs.png)

![Logboeken in de pagina functie-aanroep](./media/vs-storage-webjobs-getting-started-queues/dashboardlogs.png)

Uitvoer van de Console-methoden die u in een functie aanroepen of in de **Main()** wordt weergegeven in de Dashboard-pagina voor de webtaak, niet op de pagina voor de methodeaanroep van een bepaalde. Uitvoer van het object TextWriter die u via een parameter in de methodehandtekening wordt weergegeven op de pagina Dashboard voor een methodeaanroep.

Console-uitvoer kan niet worden gekoppeld aan een bepaalde methodeaanroep omdat de-Console één thread, is terwijl veel taakfuncties kunnen worden uitgevoerd op hetzelfde moment. Daarom is de SDK biedt elke functieaanroepen met een eigen unieke log writer-object.

Het schrijven van [tracering toepassingslogboeken](../app-service/troubleshoot-dotnet-visual-studio.md#logsoverview), gebruiken **Console.Out** (wordt gemaakt van de logboeken die zijn gemarkeerd als INFO) en **Console.Error** (logboeken die zijn gemarkeerd als fout maakt). Een alternatief is het gebruik van [Trace- of TraceSource](https://blogs.msdn.com/b/mcsuksoldev/archive/2014/09/04/adding-trace-to-azure-web-sites-and-web-jobs.aspx)en waarmee u uitgebreid, waarschuwing, kritiek niveaus, naast informatie en de fout. Tracering van toepassingslogboeken worden weergegeven in de logboekbestanden web-app, Azure-tabellen, of, afhankelijk van hoe u uw Azure-web-app configureren voor Azure blobs. Geldt voor alle Console-uitvoer, weergegeven de meest recente 100 toepassingslogboeken ook in de Dashboard-pagina voor de webtaak, niet op de pagina voor een functie-aanroep.

Console-uitvoer wordt weergegeven in het Dashboard alleen als het programma wordt uitgevoerd in een Azure-webtaak niet als het programma lokaal wordt uitgevoerd of in een andere omgeving.

U kunt logboekregistratie uitschakelen door de verbindingsreeks van het Dashboard in te stellen op null. Zie voor meer informatie, [over het instellen van configuratie-opties](#how-to-set-configuration-options).

Het volgende voorbeeld ziet u verschillende manieren om te schrijven Logboeken:

```csharp
public static void WriteLog(
    [QueueTrigger("logqueue")] string logMessage,
    TextWriter logger)
{
    Console.WriteLine("Console.Write - " + logMessage);
    Console.Out.WriteLine("Console.Out - " + logMessage);
    Console.Error.WriteLine("Console.Error - " + logMessage);
    logger.WriteLine("TextWriter - " + logMessage);
}
```

In het WebJobs-SDK-Dashboard, de uitvoer van de **TextWriter** object wordt wanneer u gaat u naar de pagina voor een bepaalde functie aanroepen en selecteer **in-/ uitschakelen uitvoer**:

![Aanroepen van koppeling](./media/vs-storage-webjobs-getting-started-queues/dashboardinvocations.png)

![Logboeken in de pagina functie-aanroep](./media/vs-storage-webjobs-getting-started-queues/dashboardlogs.png)

In het WebJobs-SDK-Dashboard, de meest recente 100 regels van de Console-uitvoer weergeven van wanneer u gaat u naar de pagina voor de webtaak (niet voor de functie-aanroep) en selecteer **in-/ uitschakelen uitvoer**.

![Uitvoer van de in-/ uitschakelen](./media/vs-storage-webjobs-getting-started-queues/dashboardapplogs.png)

In een doorlopende webtaak toepassingslogboeken weergegeven in/data/jobs/continue/*{webjobname}*/job_log.txt in het bestandssysteem van de web-app.

        [09/26/2014 21:01:13 > 491e54: INFO] Console.Write - Hello world!
        [09/26/2014 21:01:13 > 491e54: ERR ] Console.Error - Hello world!
        [09/26/2014 21:01:13 > 491e54: INFO] Console.Out - Hello world!

In een Azure blob-de toepassing Logboeken eruit als volgt: 2014-09-26T21:01:13,Information,contosoadsnew,491e54,635473620738373502,0,17404,17,Console.Write - Hallo wereld!, 2014-09-26T21:01:13,Error,contosoadsnew,491e54,635473620738373502,0,17404,19,Console.Error - Hallo wereld!, 2014-09-26T21 : 01:13,Information,contosoadsnew,491e54,635473620738529920,0,17404,17,Console.Out - Hallo wereld!,

En in een Azure-tabel de **Console.Out** en **Console.Error** Logboeken als volgt uitzien:

![Logboek van de informatie in de tabel](./media/vs-storage-webjobs-getting-started-queues/tableinfo.png)

![Foutenlogboek in tabel](./media/vs-storage-webjobs-getting-started-queues/tableerror.png)

## <a name="next-steps"></a>Volgende stappen
In dit artikel is codevoorbeelden die laten hoe u algemene scenario's zien voor het werken met Azure-wachtrijen worden opgegeven. Zie voor meer informatie over het gebruik van Azure WebJobs en de WebJobs SDK [documentatiebronnen voor Azure WebJobs](https://go.microsoft.com/fwlink/?linkid=390226).

