---
title: Aan de slag met de opslag van de wachtrij en Visual Studio verbonden services (webtaak projecten) | Microsoft Docs
description: Hoe u aan de slag met Azure Queue storage in een project webtaak nadat u verbinding met een opslagaccount met Visual Studio hebt verbonden services.
services: storage
documentationcenter: 
author: kraigb
manager: ghogen
editor: 
ms.assetid: 5c3ef267-2a67-44e9-ab4a-1edd7015034f
ms.service: storage
ms.workload: web
ms.tgt_pltfrm: vs-getting-started
ms.devlang: na
ms.topic: article
ms.date: 12/02/2016
ms.author: kraigb
ms.openlocfilehash: efd2f1e471f67396d35f11f2eb1044a8afa469af
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="getting-started-with-azure-queue-storage-and-visual-studio-connected-services-webjob-projects"></a>Aan de slag met Azure Queue storage en Visual Studio verbonden services (webtaak projecten)
[!INCLUDE [storage-try-azure-tools-queues](../../includes/storage-try-azure-tools-queues.md)]

## <a name="overview"></a>Overzicht
Dit artikel wordt beschreven hoe aan de slag met Azure Queue storage in een project voor Visual Studio Azure webtaak nadat u hebt gemaakt of een Azure storage-account waarnaar wordt verwezen door het gebruik van de Visual Studio **verbonden Services toevoegen** in het dialoogvenster. Wanneer u een opslagaccount toevoegt aan een project webtaak met behulp van de Visual Studio **verbonden Services toevoegen** dialoogvenster de juiste Azure Storage NuGet-pakketten zijn geïnstalleerd, de juiste .NET-verwijzingen worden toegevoegd aan het project en verbindingsreeksen voor het opslagaccount worden bijgewerkt in het bestand App.config.  

Dit artikel vindt u C#-codevoorbeelden die laten zien hoe de Azure WebJobs SDK-versie 1.x met de Azure Queue storage-service.

Azure Queue Storage is een service voor de opslag van grote aantallen berichten die via HTTP of HTTPS overal vandaan kunnen worden opgevraagd met geverifieerde aanroepen. Een enkel wachtrijbericht mag maximaal 64 KB groot zijn en een wachtrij kan miljoenen berichten bevatten, tot de totale capaciteitslimiet van een opslagaccount. Zie [aan de slag met Azure Queue Storage met .NET](../storage/queues/storage-dotnet-how-to-use-queues.md) voor meer informatie. Zie voor meer informatie over ASP.NET [ASP.NET](http://www.asp.net).

## <a name="how-to-trigger-a-function-when-a-queue-message-is-received"></a>Het activeren van een functie wanneer er een wachtrijbericht wordt ontvangen
Voor het schrijven van een functie waarmee de WebJobs SDK wordt aangeroepen wanneer een wachtrijbericht wordt ontvangen, gebruik de **QueueTrigger** kenmerk. De kenmerkconstructor tekenreeksparameter een waarmee de naam van de wachtrij om te pollen. Bekijk informatie over het instellen van de wachtrijnaam dynamisch, [het instellen van configuratie-opties](#how-to-set-configuration-options).

### <a name="string-queue-messages"></a>String, Wachtrijberichten
In het volgende voorbeeld bevat de wachtrij een string-bericht, dus **QueueTrigger** wordt toegepast op een tekenreeksparameter gebruikt met de naam **logMessage** die de inhoud van het bericht uit de wachtrij bevat. De functie [een logboekbericht schrijft naar het Dashboard](#how-to-write-logs).

        public static void ProcessQueueMessage([QueueTrigger("logqueue")] string logMessage, TextWriter logger)
        {
            logger.WriteLine(logMessage);
        }

Naast **tekenreeks**, de parameter is mogelijk een bytematrix een **CloudQueueMessage** object of een POCO die u definieert.

### <a name="poco-plain-old-clr-objecthttpenwikipediaorgwikiplainoldclrobject-queue-messages"></a>POCO [(Plain oude CLR-Object](http://en.wikipedia.org/wiki/Plain_Old_CLR_Object)) berichten in de wachtrij
In het volgende voorbeeld bevat de wachtrijbericht JSON voor een **BlobInformation** object waaronder een **BlobName** eigenschap. De SDK deserializes automatisch het object.

        public static void WriteLogPOCO([QueueTrigger("logqueue")] BlobInformation blobInfo, TextWriter logger)
        {
            logger.WriteLine("Queue message refers to blob: " + blobInfo.BlobName);
        }

De SDK gebruikt de [Newtonsoft.Json NuGet-pakket](http://www.nuget.org/packages/Newtonsoft.Json) voor het serialiseren en deserialiseren van berichten. Als u berichten in wachtrij plaatsen in een programma dat geen gebruik maakt van de WebJobs SDK maakt, kunt u code op het volgende voorbeeld voor het maken van een wachtrijbericht POCO die kan worden geparseerd met de SDK schrijven.

        BlobInformation blobInfo = new BlobInformation() { BlobName = "log.txt" };
        var queueMessage = new CloudQueueMessage(JsonConvert.SerializeObject(blobInfo));
        logQueue.AddMessage(queueMessage);

### <a name="async-functions"></a>Async-functies
De volgende async-functie [een logboek wordt geschreven naar het Dashboard](#how-to-write-logs).

        public async static Task ProcessQueueMessageAsync([QueueTrigger("logqueue")] string logMessage, TextWriter logger)
        {
            await logger.WriteLineAsync(logMessage);
        }

Async-functies kunnen duren voordat een [annulering token](http://www.asp.net/mvc/overview/performance/using-asynchronous-methods-in-aspnet-mvc-4#CancelToken), zoals wordt weergegeven in het volgende voorbeeld die een blob kopieert. (Voor een uitleg van de **queueTrigger** tijdelijke aanduiding, Zie de [Blobs](#how-to-read-and-write-blobs-and-tables-while-processing-a-queue-message) sectie.)

        public async static Task ProcessQueueMessageAsyncCancellationToken(
            [QueueTrigger("blobcopyqueue")] string blobName,
            [Blob("textblobs/{queueTrigger}",FileAccess.Read)] Stream blobInput,
            [Blob("textblobs/{queueTrigger}-new",FileAccess.Write)] Stream blobOutput,
            CancellationToken token)
        {
            await blobInput.CopyToAsync(blobOutput, 4096, token);
        }

## <a name="types-the-queuetrigger-attribute-works-with"></a>Het kenmerk QueueTrigger met werkt typen
U kunt **QueueTrigger** met de volgende typen:

* **tekenreeks**
* Een POCO-type dat geserialiseerd als JSON
* **byte]**
* **CloudQueueMessage**

## <a name="polling-algorithm"></a>Polling-algoritme
De SDK implementeert een willekeurige exponentiële back-off-algoritme het effect van niet-actieve wachtrij op de opslagkosten transaction polling beperkt.  Wanneer een bericht is gevonden, de SDK Wacht twee seconden en wordt er gecontroleerd of een ander bericht; Wanneer er geen bericht is gevonden wacht ongeveer vier seconden alvorens het opnieuw proberen. Na meerdere mislukte pogingen om op te halen van een wachtrijbericht blijft de wachttijd verhogen totdat het de maximale wachttijd, die standaard één minuut bereikt. [De maximale wachttijd is configureerbaar](#how-to-set-configuration-options).

## <a name="multiple-instances"></a>Meerdere exemplaren
Als uw web-app wordt uitgevoerd op meerdere exemplaren, een doorlopende webtaken uitvoert op elke machine en elke computer wordt gewacht op triggers en probeert uit te voeren van functies. In sommige gevallen is die dit kan leiden tot sommige functies die twee keer dezelfde gegevens verwerken, dus functies moet idempotent (geschreven zodat meerdere keren aanroepen met de dezelfde invoergegevens geen dubbele resultaten levert).  

## <a name="parallel-execution"></a>Parallelle uitvoering
Als er meerdere functies luistert naar verschillende wachtrijen, wordt de SDK aangeroepen ze parallel wanneer berichten tegelijkertijd worden ontvangen.

Hetzelfde geldt wanneer meerdere berichten worden ontvangen voor een enkele wachtrij. Standaard is de SDK opgehaald van een batch van 16 Wachtrijberichten tegelijk, en voert de functie die parallel worden verwerkt. [De batchgrootte is configureerbaar](#how-to-set-configuration-options). Wanneer het aantal verwerkte opgehaald omlaag naar de helft van de batchgrootte van de, wordt de SDK opgehaald van een andere batch en begint met de verwerking van deze berichten. Daarom is het maximum aantal gelijktijdige berichten worden verwerkt per functie en een half keer de batchgrootte. Deze limiet is van toepassing afzonderlijk op elke functie die een **QueueTrigger** kenmerk. Als u geen parallelle uitvoering voor berichten ontvangen voor een wachtrij wilt, moet u de batchgrootte ingesteld op 1.

## <a name="get-queue-or-queue-message-metadata"></a>Wachtrij of wachtrij bericht metagegevens ophalen
U kunt de volgende berichteigenschappen krijgen door parameters toe te voegen aan de methodehandtekening:

* **DateTimeOffset** expirationTime
* **DateTimeOffset** insertionTime
* **DateTimeOffset** nextVisibleTime
* **tekenreeks** queueTrigger (de berichttekst bevat)
* **tekenreeks** id
* **tekenreeks** popReceipt
* **int** dequeueCount

Als u samenwerken met de Azure storage-API wilt, u kunt ook toevoegen een **CloudStorageAccount** parameter.

Het volgende voorbeeld worden alle deze metagegevens van een logboek INFO geschreven. In het voorbeeld bevatten zowel logMessage als queueTrigger de inhoud van het bericht uit de wachtrij.

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

Hier volgt een voorbeeld-logboekbestanden geschreven door de voorbeeldcode:

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
Een functie die wordt uitgevoerd in een doorlopende webtaak kan accepteren een **CancellationToken** parameter waarmee het besturingssysteem naar de functie waarschuwen wanneer de webtaak wordt beëindigd. U kunt deze melding om ervoor te zorgen dat de functie niet onverwacht beëindigd op een manier die gegevens in een inconsistente status heeft blijven.

Het volgende voorbeeld laat zien hoe om te controleren voor aanstaande webtaak beëindiging in een functie.

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

**Opmerking:** het Dashboard mogelijk niet correct weergegeven status en de uitvoer van de functies die zijn afgesloten.

Zie voor meer informatie [WebJobs correct afsluiten](http://blog.amitapple.com/post/2014/05/webjobs-graceful-shutdown/#.VCt1GXl0wpR).   

## <a name="how-to-create-a-queue-message-while-processing-a-queue-message"></a>Het maken van een wachtrijbericht tijdens het verwerken van een wachtrijbericht
Voor het schrijven van een functie die een nieuwe wachtrijbericht maakt, gebruikt u de **wachtrij** kenmerk. Zoals **QueueTrigger**, u doorgeeft in de naam van de wachtrij als een tekenreeks of kunt u [dynamisch naam van de wachtrij ingesteld](#how-to-set-configuration-options).

### <a name="string-queue-messages"></a>String, Wachtrijberichten
Het volgende niet-async-codevoorbeeld maakt een nieuwe wachtrijbericht in de wachtrij met de naam 'outputqueue' met dezelfde inhoud als het bericht in de wachtrij ontvangen in de wachtrij met de naam 'inputqueue'. (Gebruik voor asynchrone functies **IAsyncCollector<T>**  zoals later in deze sectie.)

        public static void CreateQueueMessage(
            [QueueTrigger("inputqueue")] string queueMessage,
            [Queue("outputqueue")] out string outputQueueMessage )
        {
            outputQueueMessage = queueMessage;
        }

### <a name="poco-plain-old-clr-objecthttpenwikipediaorgwikiplainoldclrobject-queue-messages"></a>POCO [(Plain oude CLR-Object](http://en.wikipedia.org/wiki/Plain_Old_CLR_Object)) berichten in de wachtrij
Voor het maken van een wachtrijbericht met een POCO in plaats van een tekenreeks, geeft u het type POCO als een output-parameter voor de **wachtrij** kenmerkconstructor.

        public static void CreateQueueMessage(
            [QueueTrigger("inputqueue")] BlobInformation blobInfoInput,
            [Queue("outputqueue")] out BlobInformation blobInfoOutput )
        {
            blobInfoOutput = blobInfoInput;
        }

De SDK serialiseert automatisch het object naar JSON. Een wachtrijbericht is altijd gemaakt, zelfs als het object null is.

### <a name="create-multiple-messages-or-in-async-functions"></a>Meerdere berichten maken of in een async-functies
Controleer het parametertype voor de wachtrij voor de uitvoer voor het maken van meerdere berichten **ICollector<T>**  of **IAsyncCollector<T>**, zoals wordt weergegeven in het volgende voorbeeld.

        public static void CreateQueueMessages(
            [QueueTrigger("inputqueue")] string queueMessage,
            [Queue("outputqueue")] ICollector<string> outputQueueMessage,
            TextWriter logger)
        {
            logger.WriteLine("Creating 2 messages in outputqueue");
            outputQueueMessage.Add(queueMessage + "1");
            outputQueueMessage.Add(queueMessage + "2");
        }

Het bericht voor elke wachtrij wordt onmiddellijk gemaakt wanneer de **toevoegen** methode wordt aangeroepen.

### <a name="types-that-the-queue-attribute-works-with"></a>Typen die geschikt is voor het kenmerk wachtrij
U kunt de **wachtrij** kenmerk voor de volgende parameter:

* **uitgaand tekenreeks** (wachtrijbericht maakt als waarde voor de parameter niet null wanneer de functie eindigt)
* **uitgaand byte []** (werkt als **tekenreeks**)
* **uitgaand CloudQueueMessage** (werkt als **tekenreeks**)
* **uitgaand POCO** (een serialiseerbaar type, maakt u een bericht met een null-object als de parameter is null wanneer de functie wordt beëindigd)
* **ICollector**
* **IAsyncCollector**
* **CloudQueue** (voor het maken van berichten handmatig rechtstreeks met de Azure Storage-API)

### <a name="use-webjobs-sdk-attributes-in-the-body-of-a-function"></a>Kenmerken in de hoofdtekst van een functie WebJobs SDK gebruiken
Als u werken in uw functie wilt voordat u een kenmerk WebJobs SDK, zoals **wachtrij**, **Blob**, of **tabel**, kunt u de **IBinder**interface.

Het volgende voorbeeld wordt een bericht invoerwachtrij en maakt u een nieuw bericht met de inhoud van een wachtrij voor de uitvoer. De naam van de uitvoer-wachtrij is ingesteld door de code in de hoofdtekst van de functie.

        public static void CreateQueueMessage(
            [QueueTrigger("inputqueue")] string queueMessage,
            IBinder binder)
        {
            string outputQueueName = "outputqueue" + DateTime.Now.Month.ToString();
            QueueAttribute queueAttribute = new QueueAttribute(outputQueueName);
            CloudQueue outputQueue = binder.Bind<CloudQueue>(queueAttribute);
            outputQueue.AddMessage(new CloudQueueMessage(queueMessage));
        }

De **IBinder** interface kan ook worden gebruikt met de **tabel** en **Blob** kenmerken.

## <a name="how-to-read-and-write-blobs-and-tables-while-processing-a-queue-message"></a>Hoe kunnen lezen en schrijven van blobs, tabellen en tijdens het verwerken van een wachtrijbericht
De **Blob** en **tabel** kenmerken kunnen u blobs en tabellen lezen en schrijven. De voorbeelden in deze sectie zijn van toepassing op blobs. Zie voor codevoorbeelden die laten hoe u zien voor het activeren van processen wanneer BLOB's worden gemaakt of bijgewerkt, [Azure blob storage gebruiken met de WebJobs SDK](https://github.com/Azure/azure-webjobs-sdk/wiki).
<!-- , and for code samples that read and write tables, see [How to use Azure table storage with the WebJobs SDK](../app-service-web/websites-dotnet-webjobs-sdk-storage-tables-how-to.md). -->

### <a name="string-queue-messages-triggering-blob-operations"></a>String, Wachtrijberichten activering van blob-bewerkingen
Voor een wachtrijbericht met een tekenreeks **queueTrigger** is een tijdelijke aanduiding kunt u in de **Blob** van het kenmerk **blobPath** parameter met de inhoud van de Bericht.

Het volgende voorbeeld wordt **stroom** objecten te lezen en schrijven blobs. Bericht uit de wachtrij is de naam van een blob die zich in de container textblobs. Een kopie van de blob met '-nieuwe ' toegevoegd aan de naam wordt gemaakt in dezelfde container.

        public static void ProcessQueueMessage(
            [QueueTrigger("blobcopyqueue")] string blobName,
            [Blob("textblobs/{queueTrigger}",FileAccess.Read)] Stream blobInput,
            [Blob("textblobs/{queueTrigger}-new",FileAccess.Write)] Stream blobOutput)
        {
            blobInput.CopyTo(blobOutput, 4096);
        }

De **Blob** kenmerk constructor heeft een **blobPath** parameter waarmee de container en de blob-naam. Zie voor meer informatie over deze tijdelijke aanduiding [Azure blob storage gebruiken met de WebJobs SDK](https://github.com/Azure/azure-webjobs-sdk/wiki).

Wanneer het kenmerk wordt verfraaid een **stroom** object, een andere constructorparameter geeft u de **FileAccess** modus lezen, schrijven of lezen/schrijven.

Het volgende voorbeeld wordt een **CloudBlockBlob** object verwijderen van een blob. Bericht uit de wachtrij is de naam van de blob.

        public static void DeleteBlob(
            [QueueTrigger("deleteblobqueue")] string blobName,
            [Blob("textblobs/{queueTrigger}")] CloudBlockBlob blobToDelete)
        {
            blobToDelete.Delete();
        }

### <a name="poco-plain-old-clr-objecthttpenwikipediaorgwikiplainoldclrobject-queue-messages"></a>POCO [(Plain oude CLR-Object](http://en.wikipedia.org/wiki/Plain_Old_CLR_Object)) berichten in de wachtrij
Voor een POCO opgeslagen als JSON in bericht in de wachtrij, kunt u tijdelijke aanduidingen die eigenschappen van het object in een naam geven de **wachtrij** van het kenmerk **blobPath** parameter. U kunt ook metagegevens-Eigenschapsnamen wachtrij gebruiken als tijdelijke aanduidingen. Zie [wachtrij of wachtrij bericht metagegevens ophalen](#get-queue-or-queue-message-metadata).

Het volgende voorbeeld wordt een blob kopieert naar een nieuwe blob met een andere extensie. Bericht uit de wachtrij is een **BlobInformation** -object met **BlobName** en **BlobNameWithoutExtension** eigenschappen. De namen van eigenschappen worden gebruikt als tijdelijke aanduidingen in de blobpad voor de **Blob** kenmerken.

        public static void CopyBlobPOCO(
            [QueueTrigger("copyblobqueue")] BlobInformation blobInfo,
            [Blob("textblobs/{BlobName}", FileAccess.Read)] Stream blobInput,
            [Blob("textblobs/{BlobNameWithoutExtension}.txt", FileAccess.Write)] Stream blobOutput)
        {
            blobInput.CopyTo(blobOutput, 4096);
        }

De SDK gebruikt de [Newtonsoft.Json NuGet-pakket](http://www.nuget.org/packages/Newtonsoft.Json) voor het serialiseren en deserialiseren van berichten. Als u berichten in wachtrij plaatsen in een programma dat geen gebruik maakt van de WebJobs SDK maakt, kunt u code op het volgende voorbeeld voor het maken van een wachtrijbericht POCO die kan worden geparseerd met de SDK schrijven.

        BlobInformation blobInfo = new BlobInformation() { BlobName = "boot.log", BlobNameWithoutExtension = "boot" };
        var queueMessage = new CloudQueueMessage(JsonConvert.SerializeObject(blobInfo));
        logQueue.AddMessage(queueMessage);

Als u werken in de functie wilt voor het binden van een blob naar een object, kunt u het kenmerk in de hoofdtekst van de functie, zoals wordt weergegeven in [WebJobs SDK gebruiken kenmerken in de hoofdtekst van een functie](#use-webjobs-sdk-attributes-in-the-body-of-a-function).

### <a name="types-you-can-use-the-blob-attribute-with"></a>U het kenmerk Blob met kunt typen
De **Blob** kenmerk kan worden gebruikt met de volgende typen:

* **Stroom** (lezen of schrijven, opgegeven met behulp van de constructorparameter FileAccess)
* **TextReader**
* **TextWriter**
* **tekenreeks** (gelezen)
* **uitgaand tekenreeks** (schrijven, maakt u een blob alleen als de tekenreeksparameter is een niet-null wanneer de functie retourneert)
* POCO (lezen)
* uitgaand POCO (schrijven; altijd een blob maakt, maakt als null-object als POCO-parameter null is wanneer de functie retourneert)
* **CloudBlobStream** (schrijven)
* **ICloudBlob** (lezen of schrijven)
* **CloudBlockBlob** (lezen of schrijven)
* **CloudPageBlob** (lezen of schrijven)

## <a name="how-to-handle-poison-messages"></a>Hoe verontreinigde berichten worden verwerkt
Berichten waarvan de inhoud zorgt ervoor dat een functie mislukken worden genoemd *berichten poison*. De functie mislukt, bericht uit de wachtrij wordt niet verwijderd als uiteindelijk wordt opgehaald, waardoor de cyclus om te worden herhaald. De SDK kan automatisch de cyclus worden onderbroken na een beperkt aantal iteraties of u kunt dit handmatig doen.

### <a name="automatic-poison-message-handling"></a>Verwerken van verontreinigde berichten automatische
De SDK worden aanroepen van een functie 5 maal een wachtrijbericht te verwerken. Als de vijfde poging is mislukt, wordt het bericht wordt verplaatst naar een poison wachtrij. U kunt zien hoe het configureren van het maximum aantal pogingen in [het instellen van configuratie-opties](#how-to-set-configuration-options).

De naam van de wachtrij verontreinigd *{originalqueuename}*-verontreinigd. U kunt schrijven om een functie verwerken van berichten uit de wachtrij verontreinigd door registratie of het verzenden van een melding dat handmatige aandacht nodig is.

In het volgende voorbeeld de **CopyBlob** functie mislukken wanneer er een wachtrijbericht bevat de naam van een blob die niet bestaat. Wanneer dit gebeurt, wordt het bericht uit de wachtrij copyblobqueue verplaatst naar de wachtrij copyblobqueue poison. De **ProcessPoisonMessage** meldt zich dan verontreinigd bericht.

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

De volgende afbeelding toont console-uitvoer van deze functies bij het verwerken van een poison-bericht.

![De uitvoer van de console voor het verwerken van verontreinigde berichten](./media/vs-storage-webjobs-getting-started-queues/poison.png)

### <a name="manual-poison-message-handling"></a>Verwerken van verontreinigde berichten handmatige
U kunt het aantal keren dat een bericht opgepikt voor verwerking ophalen door het toevoegen van een **int** parameter met de naam **dequeueCount** aan de functie. U kunt controleren van de wachtrij halen telling in functiecode en voer uw eigen verontreinigd bericht verwerkt wanneer meer dan een drempelwaarde, zoals wordt weergegeven in het volgende voorbeeld.

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

## <a name="how-to-set-configuration-options"></a>Het instellen van configuratie-opties
U kunt de **JobHostConfiguration** type in te stellen van de volgende configuratieopties:

* De SDK-verbindingsreeksen in code instellen.
* Configureer **QueueTrigger** instellingen zoals het maximum aantal in wachtrij.
* Wachtrijnamen van de ophalen van configuratie.

### <a name="set-sdk-connection-strings-in-code"></a>SDK-verbindingsreeksen instellen in code
Instellen van de SDK-verbindingsreeksen in code kunt u uw eigen namen van de tekenreeks verbindingen in configuratiebestanden of omgevingsvariabelen gebruiken zoals wordt weergegeven in het volgende voorbeeld.

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

### <a name="configure-queuetrigger--settings"></a>QueueTrigger instellingen configureren
U kunt de volgende instellingen die betrekking hebben op de berichtverwerking wachtrij configureren:

* Het maximum aantal berichten die tegelijkertijd worden opgepikt parallel worden uitgevoerd (de standaardwaarde is 16).
* Het maximum aantal pogingen alvorens een wachtrijbericht naar een poison-wachtrij verzonden (de standaardwaarde is 5).
* De maximale wachttijd voordat opnieuw polling wanneer een wachtrij leeg is (de standaardwaarde is 1 minuut).

Het volgende voorbeeld ziet u hoe deze instellingen te configureren:

        static void Main(string[] args)
        {
            JobHostConfiguration config = new JobHostConfiguration();
            config.Queues.BatchSize = 8;
            config.Queues.MaxDequeueCount = 4;
            config.Queues.MaxPollingInterval = TimeSpan.FromSeconds(15);
            JobHost host = new JobHost(config);
            host.RunAndBlock();
        }

### <a name="set-values-for-webjobs-sdk-constructor-parameters-in-code"></a>Waarden instellen voor de WebJobs SDK constructorparameters in code
Soms wilt u de naam van een wachtrij, een blob-naam of -container opgeven of een tabel naam in de code in plaats van harde-code. Bijvoorbeeld, u wilt mogelijk opgeven de naam van de wachtrij voor **QueueTrigger** in een configuratie-bestand of de omgeving variabele.

U kunt dit doen door doorgeven in een **NameResolver** object toe aan de **JobHostConfiguration** type. U speciale tijdelijke aanduidingen omgeven door procent (%) tekens in constructorparameters WebJobs SDK-kenmerk, opnemen en uw **NameResolver** code bevat de werkelijke waarden moet worden gebruikt in plaats van de tijdelijke aanduidingen.

Stel dat u wilt gebruiken, een wachtrij met de naam logqueuetest in de testomgeving en een benoemde logqueueprod in productie. In plaats van een vastgelegde wachtrijnaam, die u wilt opgeven van de naam van een vermelding in de **appSettings** verzameling die de werkelijke wachtrijnaam zou hebben. Als de **appSettings** sleutel logqueue is, de functie eruit als in het volgende voorbeeld.

        public static void WriteLog([QueueTrigger("%logqueue%")] string logMessage)
        {
            Console.WriteLine(logMessage);
        }

Uw **NameResolver** klasse kan vervolgens worden opgehaald uit de naam van de wachtrij **appSettings** zoals weergegeven in het volgende voorbeeld:

        public class QueueNameResolver : INameResolver
        {
            public string Resolve(string name)
            {
                return ConfigurationManager.AppSettings[name].ToString();
            }
        }

U geeft de **NameResolver** -klasse in voor de **JobHost** object, zoals wordt weergegeven in het volgende voorbeeld.

        static void Main(string[] args)
        {
            JobHostConfiguration config = new JobHostConfiguration();
            config.NameResolver = new QueueNameResolver();
            JobHost host = new JobHost(config);
            host.RunAndBlock();
        }

**Opmerking:** Queue, table en blob-namen zijn opgelost telkens wanneer een functie wordt aangeroepen, maar de namen van de blob-containers zijn opgelost, alleen wanneer de toepassing wordt gestart. U kunt de naam van de blob-container niet wijzigen terwijl de taak wordt uitgevoerd.

## <a name="how-to-trigger-a-function-manually"></a>Het activeren van een functie handmatig
Als u wilt een functie handmatig activeren, gebruiken de **aanroepen** of **CallAsync** methode op de **JobHost** object en de **NoAutomaticTrigger** het kenmerk voor de functie, zoals wordt weergegeven in het volgende voorbeeld.

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

## <a name="how-to-write-logs"></a>Het schrijven van Logboeken
Het Dashboard toont de logboeken op twee plaatsen: de pagina voor de webtaak en op de pagina voor een specifieke webtaak-aanroep.

![Logboeken in webtaak pagina](./media/vs-storage-webjobs-getting-started-queues/dashboardapplogs.png)

![Logboeken op de pagina voor functie-aanroep](./media/vs-storage-webjobs-getting-started-queues/dashboardlogs.png)

De uitvoer van de Console methoden die u in een functie aanroept of in de **Main()** wordt weergegeven op de pagina Dashboard voor de webtaak, niet op de pagina voor het aanroepen van een bepaalde methode. De uitvoer van het object TextWriter die u via een parameter in de methodehandtekening wordt weergegeven op de pagina Dashboard voor een methodeaanroep.

Console-uitvoer kan niet worden gekoppeld aan een bepaalde methodeaanroep omdat de Console één thread, is terwijl veel functies kunnen worden uitgevoerd op hetzelfde moment. Daarom is de SDK kunt u elke functieaanroep met een eigen unieke logboek writer-object.

Schrijven van [logboeken voor tracering van toepassing](../app-service/web-sites-dotnet-troubleshoot-visual-studio.md#logsoverview), gebruik **Console.Out** (maakt logboeken die zijn gemarkeerd als INFO) en **Console.Error** (logboeken die zijn gemarkeerd als fout maakt). Een alternatief is [tracering of TraceSource](http://blogs.msdn.com/b/mcsuksoldev/archive/2014/09/04/adding-trace-to-azure-web-sites-and-web-jobs.aspx), waarmee u uitgebreid, waarschuwing, en kritieke niveaus naast gegevens en de fout. Logboeken voor tracering van toepassing worden weergegeven in de logboekbestanden web app, Azure-tabellen, of Azure blobs, afhankelijk van hoe u uw Azure-web-app configureren. Geldt voor alle Console-uitvoer, weergegeven de meest recente 100 toepassingslogboeken ook in de pagina Dashboard voor de webtaak, niet op de pagina voor een functie-aanroep.

Console-uitvoer wordt weergegeven in het Dashboard alleen als het programma wordt uitgevoerd in een Azure-webtaak niet als het programma lokaal wordt uitgevoerd of in sommige andere omgeving.

U kunt logboekregistratie uitschakelen door de verbindingsreeks Dashboard in te stellen op null. Zie voor meer informatie [het instellen van configuratie-opties](#how-to-set-configuration-options).

Het volgende voorbeeld ziet u schrijven logboeken op verschillende manieren:

        public static void WriteLog(
            [QueueTrigger("logqueue")] string logMessage,
            TextWriter logger)
        {
            Console.WriteLine("Console.Write - " + logMessage);
            Console.Out.WriteLine("Console.Out - " + logMessage);
            Console.Error.WriteLine("Console.Error - " + logMessage);
            logger.WriteLine("TextWriter - " + logMessage);
        }

In de WebJobs SDK-Dashboard de uitvoer van de **TextWriter** object wordt wanneer u naar de pagina voor een bepaalde gaat functie aanroepen en selecteer **wisselknop uitvoer**:

![Het aanroepen van koppeling](./media/vs-storage-webjobs-getting-started-queues/dashboardinvocations.png)

![Logboeken op de pagina voor functie-aanroep](./media/vs-storage-webjobs-getting-started-queues/dashboardlogs.png)

In de WebJobs SDK-Dashboard de meest recente 100 regels van de Console uitvoer weergeven van wanneer u gaat u naar de pagina voor de webtaak (niet voor de functie-aanroep) en selecteer **wisselknop uitvoer**.

![Uitvoer van de in-of uitschakelen](./media/vs-storage-webjobs-getting-started-queues/dashboardapplogs.png)

In een doorlopende webtaak toepassingslogboeken weergegeven in/data/taken/continue/*{webjobname}*/job_log.txt in het bestandssysteem van de web-app.

        [09/26/2014 21:01:13 > 491e54: INFO] Console.Write - Hello world!
        [09/26/2014 21:01:13 > 491e54: ERR ] Console.Error - Hello world!
        [09/26/2014 21:01:13 > 491e54: INFO] Console.Out - Hello world!

In een Azure blob-de toepassing Logboeken eruit als volgt: 2014-09-26T21:01:13,Information,contosoadsnew,491e54,635473620738373502,0,17404,17,Console.Write - Hello world!, 2014-09-26T21:01:13, fout, contosoadsnew, 491e54, 635473620738373502,0,17404,19,console.Error - Hello world!, 2014-09-26T21:01:13,Information,contosoadsnew,491e54,635473620738529920,0,17404,17,Console.Out - Hello world!,

En in een Azure-tabel de **Console.Out** en **Console.Error** logboeken moeten uitzien:

![Logboek van de gegevens in de tabel](./media/vs-storage-webjobs-getting-started-queues/tableinfo.png)

![Foutenlogboek in tabel](./media/vs-storage-webjobs-getting-started-queues/tableerror.png)

## <a name="next-steps"></a>Volgende stappen
In dit artikel is opgegeven codevoorbeelden die laten hoe u veelvoorkomende scenario's zien voor het werken met Azure wachtrijen verwerken. Zie voor meer informatie over het gebruik van Azure WebJobs en de WebJobs SDK [documentatiebronnen Azure WebJobs](http://go.microsoft.com/fwlink/?linkid=390226).

