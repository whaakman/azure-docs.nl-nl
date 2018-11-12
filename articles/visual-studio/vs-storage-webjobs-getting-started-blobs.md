---
title: Aan de slag met blob storage en Visual Studio verbonden services (webtaak projecten) | Microsoft Docs
description: Aan de slag met behulp van Blob-opslag in een project webtaak nadat u verbinding met een Azure-opslag met behulp van Visual Studio verbonden services.
services: storage
author: ghogen
manager: douge
ms.assetid: 324c9376-0225-4092-9825-5d1bd5550058
ms.prod: visual-studio-dev15
ms.technology: vs-azure
ms.custom: vs-azure
ms.workload: azure-vs
ms.topic: conceptual
ms.date: 12/02/2016
ms.author: ghogen
ms.openlocfilehash: 5a7c16e6ac565d1660fee02cb7df178344b195e7
ms.sourcegitcommit: da3459aca32dcdbf6a63ae9186d2ad2ca2295893
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/07/2018
ms.locfileid: "51254397"
---
# <a name="get-started-with-azure-blob-storage-and-visual-studio-connected-services-webjob-projects"></a>Aan de slag met Azure Blob storage en Visual Studio verbonden services (webtaak projecten)
[!INCLUDE [storage-try-azure-tools-blobs](../../includes/storage-try-azure-tools-blobs.md)]

## <a name="overview"></a>Overzicht
Dit artikel bevat een C#-codevoorbeelden die laten hoe een proces wordt geactiveerd zien wanneer een Azure-blob wordt gemaakt of bijgewerkt. De code-voorbeelden gebruiken de [WebJobs SDK](https://github.com/Azure/azure-webjobs-sdk/wiki) versie 1.x. Wanneer u een opslagaccount toevoegt aan een WebJob-project met behulp van de Visual Studio **Connected Services toevoegen** dialoogvenster, de juiste Azure Storage NuGet-pakket is geïnstalleerd, de juiste .NET-verwijzingen worden toegevoegd aan het project en verbindingsreeksen voor de storage-account worden bijgewerkt in het bestand App.config.

## <a name="how-to-trigger-a-function-when-a-blob-is-created-or-updated"></a>Hoe u een functie wordt geactiveerd wanneer een blob wordt gemaakt of bijgewerkt
In deze sectie ziet u hoe u de **BlobTrigger** kenmerk.

 **Opmerking:** de WebJobs SDK scant logboekbestanden om te bekijken voor nieuwe of gewijzigde blobs. Dit proces is inherent traag; een functie mogelijk niet ophalen geactiveerd totdat enkele minuten of langer nadat de blob is gemaakt.  Als uw toepassing nodig heeft om blobs te verwerken onmiddellijk, de aanbevolen methode is het maken van een wachtrijbericht wanneer u de blob maken en gebruiken de **QueueTrigger** kenmerk in plaats van de **BlobTrigger** kenmerk van de functie waarmee de blob wordt verwerkt.

### <a name="single-placeholder-for-blob-name-with-extension"></a>Enkele tijdelijke aanduiding voor de blobnaam van de met de extensie
Het volgende codevoorbeeld kopieert tekst blobs die worden weergegeven in de *invoer* container naar de *uitvoer* container:

        public static void CopyBlob([BlobTrigger("input/{name}")] TextReader input,
            [Blob("output/{name}")] out string output)
        {
            output = input.ReadToEnd();
        }

De kenmerk-constructor tekenreeksparameter een die Hiermee geeft u de containernaam van de en een tijdelijke aanduiding voor de blob-naam. In dit voorbeeld als een blob met de naam *Blob1.txt* wordt gemaakt in de *invoer* container, een blob met de naam wordt gemaakt van de functie *Blob1.txt* in de *uitvoer* container.

U kunt een patroon voor de met tijdelijke aanduiding voor blob opgeven zoals wordt weergegeven in het volgende codevoorbeeld:

        public static void CopyBlob([BlobTrigger("input/original-{name}")] TextReader input,
            [Blob("output/copy-{name}")] out string output)
        {
            output = input.ReadToEnd();
        }

Deze code wordt alleen blobs die namen die beginnen met 'oorspronkelijke--' hebt gekopieerd. Bijvoorbeeld, *oorspronkelijke Blob1.txt* in de *invoer* container wordt gekopieerd naar *kopie Blob1.txt* in de *uitvoer* container.

Als u nodig hebt om op te geven van een naampatroon voor blob-namen die accolades in de naam hebben, dubbelklikt u de accolades. Bijvoorbeeld, als u wilt zoeken blobs in de *installatiekopieën* container met namen als volgt:

        {20140101}-soundfile.mp3

Gebruik deze optie voor het patroon:

        images/{{20140101}}-{name}

In het voorbeeld wordt de *naam* aanduidingswaarde zou worden *soundfile.mp3*.

### <a name="separate-blob-name-and-extension-placeholders"></a>Aparte blob-naam en extensie tijdelijke aanduidingen
Het volgende codevoorbeeld de bestandsextensie wijzigt, worden gekopieerd blobs die worden weergegeven in de *invoer* container naar de *uitvoer* container. De code registreert de uitbreiding van de *invoer* blob en stelt u de uitbreiding van de *uitvoer* -blob naar *.txt*.

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

## <a name="types-that-you-can-bind-to-blobs"></a>Typen die u met blobs verbinden kunt
U kunt de **BlobTrigger** kenmerk voor de volgende typen:

* **Tekenreeks**
* **TextReader**
* **Stream**
* **ICloudBlob**
* **CloudBlockBlob**
* **CloudPageBlob**
* Andere typen gedeserialiseerd door [ICloudBlobStreamBinder](#getting-serialized-blob-content-by-using-icloudblobstreambinder)

Als u werken rechtstreeks met de Azure storage-account wilt, kunt u ook toevoegen een **CloudStorageAccount** parameter aan de methodehandtekening.

## <a name="getting-text-blob-content-by-binding-to-string"></a>Ophalen van blob tekstinhoud binding met de tekenreeks
Als u tekst blobs worden verwacht, **BlobTrigger** kan worden toegepast op een **tekenreeks** parameter. Het volgende codevoorbeeld wordt gebonden een tekst-blob naar een **tekenreeks** parameter met de naam **logMessage**. De functie maakt gebruik van deze parameter de inhoud van de blob schrijven naar het dashboard van de WebJobs SDK.

        public static void WriteLog([BlobTrigger("input/{name}")] string logMessage,
            string name,
            TextWriter logger)
        {
             logger.WriteLine("Blob name: {0}", name);
             logger.WriteLine("Content:");
             logger.WriteLine(logMessage);
        }

## <a name="getting-serialized-blob-content-by-using-icloudblobstreambinder"></a>Blobinhoud ophalen geserialiseerd met behulp van ICloudBlobStreamBinder
Het volgende codevoorbeeld maakt gebruik van een klasse die **ICloudBlobStreamBinder** om in te schakelen de **BlobTrigger** kenmerk voor het binden van een blob naar de **WebImage** type.

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

De **WebImage** bindingcode vindt u in een **WebImageBinder** klasse die is afgeleid van **ICloudBlobStreamBinder**.

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

## <a name="how-to-handle-poison-blobs"></a>Beheer van onverwerkbare blobs verwerken
Wanneer een **BlobTrigger** functie mislukt, de SDK-aanroepen van het opnieuw als de fout is veroorzaakt door een tijdelijke fout. Als de fout wordt veroorzaakt door de inhoud van de blob, mislukt de functie telkens wanneer er wordt geprobeerd om te verwerken van de blob. Standaard roept de SDK een functie maximaal 5 keer voor een bepaalde blob. Als het vijfde mislukt probeert, de SDK een bericht toevoegt aan een wachtrij met de naam *webjobs-blobtrigger-poison*.

Het maximum aantal nieuwe pogingen worden geconfigureerd. Dezelfde **MaxDequeueCount** instelling wordt gebruikt voor het beheer van onverwerkbare blob verwerking en de afhandeling van wachtrij voor beheer van Onverwerkbare berichten.

Bericht uit de wachtrij voor beheer van onverwerkbare blobs is een JSON-object met de volgende eigenschappen:

* FunctionId (in de indeling *{naam van een webtaak}*. Functies. *{Functienaam}*, bijvoorbeeld: WebJob1.Functions.CopyBlob)
* BlobType ("BlockBlob" of "PageBlob")
* ContainerName
* BlobName
* ETag (een blob versie-id, bijvoorbeeld: "0x8D1DC6E70A277EF")

In het volgende codevoorbeeld wordt de **CopyBlob** functie heeft code die ervoor zorgt deze dat niet telkens wanneer deze wordt aangeroepen. Nadat de SDK deze voor het maximum aantal nieuwe pogingen roept, een bericht op het beheer van onverwerkbare blob-wachtrij wordt gemaakt en dat bericht is verwerkt door de **LogPoisonBlob** functie.

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

De SDK gedeserialiseerd automatisch het JSON-bericht. Hier volgt de **PoisonBlobMessage** klasse:

        public class PoisonBlobMessage
        {
            public string FunctionId { get; set; }
            public string BlobType { get; set; }
            public string ContainerName { get; set; }
            public string BlobName { get; set; }
            public string ETag { get; set; }
        }

### <a name="blob-polling-algorithm"></a>BLOB polling-algoritme
De WebJobs SDK scant alle containers die zijn opgegeven door **BlobTrigger** kenmerken aan begin van de toepassing. In een grote opslagaccount deze scan kan enige tijd duren, zodat het mogelijk even voordat nieuwe blobs worden gevonden en **BlobTrigger** functies worden uitgevoerd.

Voor het detecteren van nieuwe of gewijzigde blobs na de start van toepassing, leest de SDK periodiek uit de logboeken van de blob-opslag. De logboeken van de blob in een buffer opgeslagen en alleen fysiek elke 10 minuten geschreven of dus, dus er mogelijk aanzienlijke vertraging optreden nadat een blob wordt gemaakt of voordat de bijbehorende bijgewerkt **BlobTrigger** functie wordt uitgevoerd.

Er is een uitzondering voor blobs die u met behulp van maakt de **Blob** kenmerk. Wanneer de WebJobs SDK een nieuwe blob maakt, wordt de nieuwe blob onmiddellijk doorgegeven aan alle overeenkomende **BlobTrigger** functies. Daarom hebt u een keten van blob-invoer en uitvoer, kan de SDK verwerken deze efficiënt. Maar als u lage latentie uitvoeren van uw blob verwerking functions voor blobs die zijn gemaakt of bijgewerkt op een andere manier wilt, wordt u aangeraden **QueueTrigger** in plaats van **BlobTrigger**.

### <a name="blob-receipts"></a>BLOB-ontvangsten
De WebJobs SDK zorgt ervoor dat er geen **BlobTrigger** functie wordt meerdere keren aangeroepen voor de dezelfde nieuwe of bijgewerkte blob. Dit gebeurt door het onderhouden van *blob-ontvangsten* om te bepalen als een bepaalde blob-versie is verwerkt.

BLOB ontvangstbevestigingen worden opgeslagen in een container met de naam *azure-webtaken-hosts* in de Azure storage-account dat is opgegeven door de verbindingstekenreeks AzureWebJobsStorage. De ontvangst van een blob bevat de volgende informatie:

* De functie is aangeroepen voor de blob ("*{naam van een webtaak}*. Functies. *{Functienaam}*', bijvoorbeeld: "WebJob1.Functions.CopyBlob")
* De containernaam van de
* Het blobtype ("BlockBlob" of "PageBlob")
* Naam van de blob
* De ETag (een blob versie-id, bijvoorbeeld: "0x8D1DC6E70A277EF")

Als u afdwingen dat opnieuw verwerken van een blob wilt, kunt u handmatig de ontvangst van de blob voor die blob uit verwijderen de *azure-webtaken-hosts* container.

## <a name="related-topics-covered-by-the-queues-article"></a>Verwante onderwerpen wordt gedekt door het artikel wachtrijen
Scenario's die niet specifiek zijn voor blob-verwerking, Zie voor informatie over het afhandelen van de blob-verwerking geactiveerd door een wachtrijbericht of voor WebJobs SDK [Azure queue storage gebruiken met de WebJobs SDK](https://github.com/Azure/azure-webjobs-sdk/wiki).

Verwante onderwerpen in dit artikel besproken omvatten het volgende:

* Async-functies
* Meerdere exemplaren
* Correct afsluiten
* Kenmerken in de hoofdtekst van de functie WebJobs SDK gebruiken
* De SDK-verbindingsreeksen instellen in code.
* Stel waarden voor WebJobs SDK parameters van constructor in code
* Configureer **MaxDequeueCount** voor het beheer van onverwerkbare blob verwerken.
* Een functie handmatig activeren
* Schrijven Logboeken

## <a name="next-steps"></a>Volgende stappen
In dit artikel is codevoorbeelden die laten hoe u algemene scenario's zien voor het werken met Azure-blobs worden opgegeven. Zie voor meer informatie over het gebruik van Azure WebJobs en de WebJobs SDK [documentatiebronnen voor Azure WebJobs](https://go.microsoft.com/fwlink/?linkid=390226).

