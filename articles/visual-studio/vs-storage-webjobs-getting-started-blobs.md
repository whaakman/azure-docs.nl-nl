---
title: Aan de slag met blob storage en Visual Studio verbonden services (webtaak projecten) | Microsoft Docs
description: Klik hier voor meer informatie over het aan de slag met Blob-opslag in een project webtaak nadat verbinding te maken met een Azure-opslag met Visual Studio services verbonden.
services: storage
documentationcenter: 
author: kraigb
manager: ghogen
editor: 
ms.assetid: 324c9376-0225-4092-9825-5d1bd5550058
ms.service: storage
ms.workload: web
ms.tgt_pltfrm: vs-getting-started
ms.devlang: na
ms.topic: article
ms.date: 12/02/2016
ms.author: kraigb
ms.openlocfilehash: 7d683f950e8847a18f38158a8f8727b1274fc711
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="get-started-with-azure-blob-storage-and-visual-studio-connected-services-webjob-projects"></a>Aan de slag met Azure Blob storage en Visual Studio verbonden services (webtaak projecten)
[!INCLUDE [storage-try-azure-tools-blobs](../../includes/storage-try-azure-tools-blobs.md)]

## <a name="overview"></a>Overzicht
Dit artikel bevat een C#-codevoorbeelden die laten hoe een proces wordt geactiveerd zien wanneer een Azure-blob is gemaakt of bijgewerkt. De code voorbeelden gebruiken de [WebJobs SDK](https://github.com/Azure/azure-webjobs-sdk/wiki) versie 1.x. Wanneer u een opslagaccount toevoegt aan een project webtaak met behulp van de Visual Studio **verbonden Services toevoegen** dialoogvenster het juiste Azure Storage NuGet-pakket is geïnstalleerd, de juiste .NET-verwijzingen worden toegevoegd aan het project en verbindingsreeksen voor het opslagaccount worden bijgewerkt in het bestand App.config.

## <a name="how-to-trigger-a-function-when-a-blob-is-created-or-updated"></a>Het activeren van een functie wanneer een blob is gemaakt of bijgewerkt
Deze sectie wordt beschreven hoe u de **BlobTrigger** kenmerk.

 **Opmerking:** de WebJobs SDK scant logboekbestanden moeten worden gecontroleerd op nieuwe of gewijzigde blobs. Dit proces is inherent langzaam; een functie mogelijk niet ophalen geactiveerd tot enkele minuten of langer nadat de blob is gemaakt.  Als uw toepassing onmiddellijk verwerkt, blobs moet, de aanbevolen methode is het maken van een wachtrijbericht wanneer u de blob maken en gebruiken de **QueueTrigger** kenmerk in plaats van de **BlobTrigger** kenmerk voor de functie die de blob wordt verwerkt.

### <a name="single-placeholder-for-blob-name-with-extension"></a>Één tijdelijke aanduiding voor blob-naam met de extensie
Het volgende codevoorbeeld kopieert tekst blobs die worden weergegeven in de *invoer* container voor de *uitvoer* container:

        public static void CopyBlob([BlobTrigger("input/{name}")] TextReader input,
            [Blob("output/{name}")] out string output)
        {
            output = input.ReadToEnd();
        }

De kenmerkconstructor tekenreeksparameter een waarin de containernaam van de en een tijdelijke aanduiding voor de blob-naam opgegeven. In dit voorbeeld als de naam van een blob *Blob1.txt* wordt gemaakt in de *invoer* -container, de functie maakt een blob met de naam *Blob1.txt* in de *uitvoer* container.

Zoals wordt weergegeven in het volgende codevoorbeeld, kunt u een bestandsnaampatroon opgeven met de tijdelijke aanduiding voor blob:

        public static void CopyBlob([BlobTrigger("input/original-{name}")] TextReader input,
            [Blob("output/copy-{name}")] out string output)
        {
            output = input.ReadToEnd();
        }

Deze code alleen blobs die namen die beginnen met 'oorspronkelijke--' hebt opgehaald. Bijvoorbeeld: *oorspronkelijke Blob1.txt* in de *invoer* container wordt gekopieerd naar *kopie Blob1.txt* in de *uitvoer* container.

Als u een naampatroon opgeven voor blob-namen die accolades hebben met de naam moet, dubbelklikt u accolades gebruiken. Bijvoorbeeld, als u wilt zoeken blobs in de *installatiekopieën* container met namen als volgt:

        {20140101}-soundfile.mp3

Gebruik deze optie voor het patroon:

        images/{{20140101}}-{name}

In het voorbeeld wordt de *naam* aanduidingswaarde zou worden *soundfile.mp3*.

### <a name="separate-blob-name-and-extension-placeholders"></a>Afzonderlijke blob-naam en extensie voor tijdelijke aanduidingen
Het volgende codevoorbeeld de bestandsextensie wijzigt, zoals het gekopieerd blobs die worden weergegeven in de *invoer* container voor de *uitvoer* container. De code registreert de uitbreiding van de *invoer* blob en stelt u de uitbreiding van de *uitvoer* -blob naar *.txt*.

        public static void CopyBlobToTxtFile([BlobTrigger("input/{name}.{ext}")] TextReader input,
            [Blob("output/{name}.txt")] out string output,
            string name,
            string ext,
            TextWriter logger)
        {
            logger.WriteLine("Blob name:" + name);
            logger.WriteLine("Blob extension:" + ext);
            output = input.ReadToEnd();
        }

## <a name="types-that-you-can-bind-to-blobs"></a>Typen die u aan BLOB's koppelen kunt
U kunt de **BlobTrigger** -kenmerk op de volgende typen:

* **tekenreeks**
* **TextReader**
* **Stroom**
* **ICloudBlob**
* **CloudBlockBlob**
* **CloudPageBlob**
* Andere typen gedeserialiseerd door [ICloudBlobStreamBinder](#getting-serialized-blob-content-by-using-icloudblobstreambinder)

Als u samenwerken met het Azure storage-account wilt, kunt u ook toevoegen een **CloudStorageAccount** -parameter voor de methodehandtekening.

## <a name="getting-text-blob-content-by-binding-to-string"></a>Ophalen van blob tekstinhoud door binding naar een tekenreeks
Als tekst blobs worden verwacht, **BlobTrigger** kunnen worden toegepast op een **tekenreeks** parameter. Het volgende codevoorbeeld wordt gebonden een tekst-blob naar een **tekenreeks** parameter met de naam **logMessage**. De functie wordt deze parameter de inhoud van de blob geschreven naar het dashboard WebJobs SDK.

        public static void WriteLog([BlobTrigger("input/{name}")] string logMessage,
            string name,
            TextWriter logger)
        {
             logger.WriteLine("Blob name: {0}", name);
             logger.WriteLine("Content:");
             logger.WriteLine(logMessage);
        }

## <a name="getting-serialized-blob-content-by-using-icloudblobstreambinder"></a>Ophalen van blob-inhoud geserialiseerd met behulp van ICloudBlobStreamBinder
Het volgende codevoorbeeld maakt gebruik van een klasse die implementeert **ICloudBlobStreamBinder** zodat de **BlobTrigger** kenmerk binden van een blob naar de **WebImage** type.

        public static void WaterMark(
            [BlobTrigger("images3/{name}")] WebImage input,
            [Blob("images3-watermarked/{name}")] out WebImage output)
        {
            output = input.AddTextWatermark("WebJobs SDK",
                horizontalAlign: "Center", verticalAlign: "Middle",
                fontSize: 48, opacity: 50);
        }
        public static void Resize(
            [BlobTrigger("images3-watermarked/{name}")] WebImage input,
            [Blob("images3-resized/{name}")] out WebImage output)
        {
            var width = 180;
            var height = Convert.ToInt32(input.Height * 180 / input.Width);
            output = input.Resize(width, height);
        }

De **WebImage** binding code is opgegeven een **WebImageBinder** klasse die is afgeleid van **ICloudBlobStreamBinder**.

        public class WebImageBinder : ICloudBlobStreamBinder<WebImage>
        {
            public Task<WebImage> ReadFromStreamAsync(Stream input,
                System.Threading.CancellationToken cancellationToken)
            {
                return Task.FromResult<WebImage>(new WebImage(input));
            }
            public Task WriteToStreamAsync(WebImage value, Stream output,
                System.Threading.CancellationToken cancellationToken)
            {
                var bytes = value.GetBytes();
                return output.WriteAsync(bytes, 0, bytes.Length, cancellationToken);
            }
        }

## <a name="how-to-handle-poison-blobs"></a>Het verwerken van verontreinigde blobs
Wanneer een **BlobTrigger** functie mislukt, de SDK-aanroepen deze opnieuw als de fout is veroorzaakt door een tijdelijke fout. Als de fout wordt veroorzaakt door de inhoud van de blob, mislukt de functie elke keer dat de blob wordt verwerkt. Standaard de SDK-aanroepen een functie maximaal 5 maal voor een opgegeven blob. Als de vijfde mislukt probeert, wordt een bericht met de SDK toegevoegd aan een wachtrij met de naam *webjobs-blobtrigger-poison*.

Het maximum aantal nieuwe pogingen kan worden geconfigureerd. Dezelfde **MaxDequeueCount** instelling wordt gebruikt voor de verwerking van verontreinigde blob en wachtrij verontreinigd bericht verwerking.

Bericht uit de wachtrij voor verontreinigde blobs is een JSON-object met de volgende eigenschappen:

* FunctionId (in de notatie *{naam van een webtaak}*. Functies. *{Functienaam}*, bijvoorbeeld: WebJob1.Functions.CopyBlob)
* BlobType ('BlockBlob' of 'PageBlob')
* ContainerName
* BlobName
* ETag (een blob-id, bijvoorbeeld: '0x8D1DC6E70A277EF')

In het volgende codevoorbeeld wordt de **CopyBlob** functie heeft code die ervoor zorgt dat mislukken telkens wanneer deze wordt aangeroepen. Nadat de SDK voor het maximum aantal nieuwe pogingen aangeroepen, een bericht in de wachtrij verontreinigd blob gemaakt en dat bericht is verwerkt door de **LogPoisonBlob** functie.

        public static void CopyBlob([BlobTrigger("input/{name}")] TextReader input,
            [Blob("textblobs/output-{name}")] out string output)
        {
            throw new Exception("Exception for testing poison blob handling");
            output = input.ReadToEnd();
        }

        public static void LogPoisonBlob(
        [QueueTrigger("webjobs-blobtrigger-poison")] PoisonBlobMessage message,
            TextWriter logger)
        {
            logger.WriteLine("FunctionId: {0}", message.FunctionId);
            logger.WriteLine("BlobType: {0}", message.BlobType);
            logger.WriteLine("ContainerName: {0}", message.ContainerName);
            logger.WriteLine("BlobName: {0}", message.BlobName);
            logger.WriteLine("ETag: {0}", message.ETag);
        }

De SDK deserializes automatisch het JSON-bericht. Hier volgt de **PoisonBlobMessage** klasse:

        public class PoisonBlobMessage
        {
            public string FunctionId { get; set; }
            public string BlobType { get; set; }
            public string ContainerName { get; set; }
            public string BlobName { get; set; }
            public string ETag { get; set; }
        }

### <a name="blob-polling-algorithm"></a>BLOB polling-algoritme
De WebJobs SDK scant alle containers die zijn opgegeven door **BlobTrigger** kenmerken aan begin van de toepassing. In een grote opslagaccount deze scan kan enige tijd duren, zodat dit mogelijk even voordat nieuwe blobs zijn gevonden en **BlobTrigger** functies worden uitgevoerd.

Om te detecteren nieuwe of gewijzigde blobs na het starten van toepassingsservices, leest de SDK periodiek uit de logboeken van de blob-opslag. De logboeken van de blob zijn gebufferd en alleen fysiek elke 10 minuten weggeschreven of dus, dus er aanzienlijke vertraging optreden nadat een blob wordt gemaakt of voordat de bijbehorende bijgewerkt **BlobTrigger** functie wordt uitgevoerd.

Er is een uitzondering voor blobs die u met behulp van maakt de **Blob** kenmerk. Wanneer de WebJobs SDK een nieuwe blob maakt, wordt de nieuwe blob onmiddellijk doorgegeven aan een overeenkomende **BlobTrigger** functies. Daarom hebt u een keten van blob-invoer en uitvoer, kan de SDK verwerken efficiënt. Maar als u wilt een lage latentie functies voor blobs die zijn gemaakt of bijgewerkt op een andere manier voor het verwerken van uw blob uitgevoerd, wordt u aangeraden **QueueTrigger** plaats **BlobTrigger**.

### <a name="blob-receipts"></a>BLOB ontvangstbevestigingen
De WebJobs SDK zorgt ervoor dat er geen **BlobTrigger** functie wordt meer dan één keer aangeroepen voor de dezelfde nieuwe of bijgewerkte blob. Dit wordt uitgevoerd door het onderhouden van *blob ontvangstbevestigingen* om te bepalen of een versie van de opgegeven blob is verwerkt.

BLOB ontvangstbevestigingen worden opgeslagen in een container met de naam *webjobs-azure-hosts* in de Azure storage-account dat is opgegeven door de verbindingsreeks AzureWebJobsStorage. De ontvangst van een blob heeft de volgende informatie:

* De functie die voor de blob is aangeroepen ('*{naam van een webtaak}*. Functies. *{Functienaam}*', bijvoorbeeld: 'WebJob1.Functions.CopyBlob')
* De containernaam
* Het blobtype ('BlockBlob' of 'PageBlob')
* De blob-naam
* De ETag (een blob-id, bijvoorbeeld: '0x8D1DC6E70A277EF')

Als u afdwingen opnieuw verwerken van een blob wilt, kunt u handmatig de ontvangst van de blob voor blob uit verwijderen de *webjobs-azure-hosts* container.

## <a name="related-topics-covered-by-the-queues-article"></a>Verwante onderwerpen gedekt door het artikel wachtrijen
Scenario's die niet specifiek zijn voor blob-verwerking, Zie voor meer informatie over het afhandelen van de blob-verwerking geactiveerd door een wachtrijbericht of voor WebJobs SDK [Azure queue storage gebruiken met de WebJobs SDK](https://github.com/Azure/azure-webjobs-sdk/wiki).

Verwante onderwerpen in dit artikel omvatten het volgende:

* Async-functies
* Meerdere exemplaren
* Correct afsluiten
* Kenmerken in de hoofdtekst van een functie WebJobs SDK gebruiken
* De SDK-verbindingsreeksen in code instellen.
* Waarden instellen voor de WebJobs SDK constructorparameters in code
* Configureer **MaxDequeueCount** voor het verwerken van verontreinigde blob.
* Een functie handmatig activeren
* Schrijven Logboeken

## <a name="next-steps"></a>Volgende stappen
In dit artikel is opgegeven codevoorbeelden die laten hoe algemene scenario's zien voor het werken met Azure blobs afhandelen. Zie voor meer informatie over het gebruik van Azure WebJobs en de WebJobs SDK [documentatiebronnen Azure WebJobs](http://go.microsoft.com/fwlink/?linkid=390226).

