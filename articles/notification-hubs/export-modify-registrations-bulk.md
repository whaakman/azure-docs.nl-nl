---
title: Exporteren en importeren van Azure Notification Hubs-registraties in bulk | Microsoft Docs
description: Informatie over het gebruik van Notification Hubs bulksgewijs ondersteuning om uit te voeren van een groot aantal bewerkingen voor een notification hub, of voor het exporteren van alle registraties.
services: notification-hubs
author: jwargo
manager: patniko
editor: spelluru
ms.service: notification-hubs
ms.workload: mobile
ms.tgt_pltfrm: ''
ms.devlang: ''
ms.topic: article
ms.date: 03/18/2019
ms.author: jowargo
ms.openlocfilehash: a86c3bd85f9d611787a41754f49ee2475ba33a9a
ms.sourcegitcommit: 5839af386c5a2ad46aaaeb90a13065ef94e61e74
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/18/2019
ms.locfileid: "58175775"
---
# <a name="export-and-import-azure-notification-hubs-registrations-in-bulk"></a>Exporteren en importeren van Azure Notification Hubs-registraties in bulk
Er zijn scenario's waarin deze is vereist om te maken of wijzigen van grote aantallen registraties in een notification hub. Sommige van deze scenario's zijn tag updates batch berekeningen te volgen of migreert van een bestaande push-implementatie voor het gebruik van Notification Hubs.

In dit artikel wordt uitgelegd hoe om uit te voeren van een groot aantal bewerkingen voor een notification hub en om alle registraties in bulk exporteren.

## <a name="high-level-flow"></a>Op hoog niveau stroom
Batch-ondersteuning is ontworpen ter ondersteuning van langlopende taken met betrekking tot miljoenen registraties. Ondersteuning voor batch gebruikt voor het bereiken van deze schaal, Azure Storage voor het opslaan van taakdetails en uitvoer. Voor de bulk-update-bewerkingen is de gebruiker vereist voor het maken van een bestand in een blob-container, waarvan de inhoud de lijst met bewerkingen voor het bijwerken van registraties is. Bij het starten van de taak, biedt de gebruiker een URL naar de blob, samen met een URL naar een map met de uitvoer (ook in een blob-container). Nadat de taak is gestart, kan de gebruiker de status controleren door het opvragen van een URL-locatie die is opgegeven tijdens het starten van de taak. Een specifieke taak kan alleen bewerkingen van een bepaald soort uitvoeren (gemaakt, bijgewerkt of verwijderd). Exportbewerkingen worden analogously uitgevoerd.

## <a name="import"></a>Importeren

### <a name="set-up"></a>Instellen
In deze sectie wordt ervan uitgegaan dat u hebt de volgende entiteiten:

- Een ingerichte notification hub.
- Een Azure Storage-blob-container.
- Verwijzingen naar de Azure Storage en Azure Service Bus NuGet-pakketten.

### <a name="create-input-file-and-store-it-in-a-blob"></a>Invoerbestand maken en op te slaan in een blob
Een bestand voor invoer bevat een lijst met rapporten die zijn geserialiseerd in XML, één per rij. Met de Azure SDK, ziet in het volgende voorbeeld u hoe de registraties serialiseren en te uploaden naar blob-container.

```csharp
private static void SerializeToBlob(CloudBlobContainer container, RegistrationDescription[] descriptions)
{
    StringBuilder builder = new StringBuilder();
    foreach (var registrationDescription in descriptions)
    {
        builder.AppendLine(RegistrationDescription.Serialize());
    }

    var inputBlob = container.GetBlockBlobReference(INPUT_FILE_NAME);
    using (MemoryStream stream = new MemoryStream(Encoding.UTF8.GetBytes(builder.ToString())))
    {
        inputBlob.UploadFromStream(stream);
    }
}
```

> [!IMPORTANT]
> De bovenstaande code serialiseert de registraties in het geheugen en uploadt vervolgens de gehele stream naar een blob. Als u een bestand met meer dan een paar megabytes hebt geüpload, raadpleegt u de Azure blob-richtlijnen over het uitvoeren van deze stappen; bijvoorbeeld, [blok-blobs](/rest/api/storageservices/Understanding-Block-Blobs--Append-Blobs--and-Page-Blobs).

### <a name="create-url-tokens"></a>URL-tokens aanmaken
Zodra uw invoerbestand is geüpload, genereert u de URL's om te bieden aan uw notification hub voor zowel het invoerbestand als de uitvoermap. U kunt twee andere blob-containers gebruiken voor invoer en uitvoer.

```csharp
static Uri GetOutputDirectoryUrl(CloudBlobContainer container)
{
    SharedAccessBlobPolicy sasConstraints = new SharedAccessBlobPolicy
    {
        SharedAccessExpiryTime = DateTime.UtcNow.AddDays(1),
        Permissions = SharedAccessBlobPermissions.Write | SharedAccessBlobPermissions.List | SharedAccessBlobPermissions.Read
    };

    string sasContainerToken = container.GetSharedAccessSignature(sasConstraints);
    return new Uri(container.Uri + sasContainerToken);
}

static Uri GetInputFileUrl(CloudBlobContainer container, string filePath)
{
    SharedAccessBlobPolicy sasConstraints = new SharedAccessBlobPolicy
    {
        SharedAccessExpiryTime = DateTime.UtcNow.AddDays(1),
        Permissions = SharedAccessBlobPermissions.Read
    };
    string sasToken = container.GetBlockBlobReference(filePath).GetSharedAccessSignature(sasConstraints);
    return new Uri(container.Uri + "/" + filePath + sasToken);
}
```

### <a name="submit-the-job"></a>De taak verzenden
Met de twee invoer- en URL's, kunt u nu de batch-taak starten.

```csharp
NotificationHubClient client = NotificationHubClient.CreateClientFromConnectionString(CONNECTION_STRING, HUB_NAME);
var createTask = client.SubmitNotificationHubJobAsync(
new NotificationHubJob {
        JobType = NotificationHubJobType.ImportCreateRegistrations,
        OutputContainerUri = outputContainerSasUri,
        ImportFileUri = inputFileSasUri
    }
);
createTask.Wait();

var job = createTask.Result;
long i = 10;
while (i > 0 && job.Status != NotificationHubJobStatus.Completed)
{
    var getJobTask = client.GetNotificationHubJobAsync(job.JobId);
    getJobTask.Wait();
    job = getJobTask.Result;
    Thread.Sleep(1000);
    i--;
}
```

Naast de invoer- en URL's in dit voorbeeld wordt een `NotificationHubJob` object met een `JobType` object, dit kan een van de volgende typen:

- `ImportCreateRegistrations`
- `ImportUpdateRegistrations`
- `ImportDeleteRegistrations`

Zodra de aanroep is voltooid, de taak door de notification hub wordt voortgezet en u kunt met de aanroep naar de status ervan controleren [GetNotificationHubJobAsync](/dotnet/api/microsoft.azure.notificationhubs.notificationhubclient.getnotificationhubjobasync?view=azure-dotnet).

Na het voltooien van de taak, kunt u de resultaten controleren door te kijken naar de volgende bestanden in uw map met de uitvoer:

- `/<hub>/<jobid>/Failed.txt`
- `/<hub>/<jobid>/Output.txt`

Deze bestanden bevatten de lijst van geslaagde en mislukte bewerkingen van de batch. De bestandsindeling is `.cvs`, in elke rij het regelnummer van het oorspronkelijke bestand voor invoer en de uitvoer van de bewerking (meestal de registratie van nieuwe of bijgewerkte beschrijving heeft).

### <a name="full-sample-code"></a>Volledig voorbeeld van code
De volgende voorbeeldcode importeert registraties in een notification hub.

```csharp
using Microsoft.ServiceBus.Notifications;
using Microsoft.WindowsAzure.Storage;
using Microsoft.WindowsAzure.Storage.Blob;
using System;
using System.Collections.Generic;
using System.Globalization;
using System.IO;
using System.Linq;
using System.Runtime.Serialization;
using System.Text;
using System.Threading;
using System.Threading.Tasks;
using System.Xml;

namespace ConsoleApplication1
{
    class Program
    {
        private static string CONNECTION_STRING = "---";
        private static string HUB_NAME = "---";
        private static string INPUT_FILE_NAME = "CreateFile.txt";
        private static string STORAGE_ACCOUNT = "---";
        private static string STORAGE_PASSWORD = "---";
        private static StorageUri STORAGE_ENDPOINT = new StorageUri(new Uri("---"));

        static void Main(string[] args)
        {
            var descriptions = new[]
            {
                new MpnsRegistrationDescription(@"http://dm2.notify.live.net/throttledthirdparty/01.00/12G9Ed13dLb5RbCii5fWzpFpAgAAAAADAQAAAAQUZm52OkJCMjg1QTg1QkZDMkUxREQFBlVTTkMwMQ"),
                new MpnsRegistrationDescription(@"http://dm2.notify.live.net/throttledthirdparty/01.00/12G9Ed13dLb5RbCii5fWzpFpAgAAAAADAQAAAAQUZm52OkJCMjg1QTg1QkZDMjUxREQFBlVTTkMwMQ"),
                new MpnsRegistrationDescription(@"http://dm2.notify.live.net/throttledthirdparty/01.00/12G9Ed13dLb5RbCii5fWzpFpAgAAAAADAQAAAAQUZm52OkJCMjg1QTg1QkZDMhUxREQFBlVTTkMwMQ"),
                new MpnsRegistrationDescription(@"http://dm2.notify.live.net/throttledthirdparty/01.00/12G9Ed13dLb5RbCii5fWzpFpAgAAAAADAQAAAAQUZm52OkJCMjg1QTg1QkZDMdUxREQFBlVTTkMwMQ"),
            };

            //write to blob store to create an input file
            var blobClient = new CloudBlobClient(STORAGE_ENDPOINT, new Microsoft.WindowsAzure.Storage.Auth.StorageCredentials(STORAGE_ACCOUNT, STORAGE_PASSWORD));
            var container = blobClient.GetContainerReference("testjobs");
            container.CreateIfNotExists();

            SerializeToBlob(container, descriptions);

            // TODO then create Sas
            var outputContainerSasUri = GetOutputDirectoryUrl(container);
            var inputFileSasUri = GetInputFileUrl(container, INPUT_FILE_NAME);


            //Lets import this file
            NotificationHubClient client = NotificationHubClient.CreateClientFromConnectionString(CONNECTION_STRING, HUB_NAME);
            var createTask = client.SubmitNotificationHubJobAsync(
                new NotificationHubJob {
                    JobType = NotificationHubJobType.ImportCreateRegistrations,
                    OutputContainerUri = outputContainerSasUri,
                    ImportFileUri = inputFileSasUri
                }
            );
            createTask.Wait();

            var job = createTask.Result;
            long i = 10;
            while (i > 0 && job.Status != NotificationHubJobStatus.Completed)
            {
                var getJobTask = client.GetNotificationHubJobAsync(job.JobId);
                getJobTask.Wait();
                job = getJobTask.Result;
                Thread.Sleep(1000);
                i--;
            }
        }

        private static void SerializeToBlob(CloudBlobContainer container, RegistrationDescription[] descriptions)
        {
            StringBuilder builder = new StringBuilder();
            foreach (var registrationDescription in descriptions)
            {
                builder.AppendLine(RegistrationDescription.Serialize());
            }

            var inputBlob = container.GetBlockBlobReference(INPUT_FILE_NAME);
            using (MemoryStream stream = new MemoryStream(Encoding.UTF8.GetBytes(builder.ToString())))
            {
                inputBlob.UploadFromStream(stream);
            }
        }

        static Uri GetOutputDirectoryUrl(CloudBlobContainer container)
        {
            //Set the expiry time and permissions for the container.
            //In this case no start time is specified, so the shared access signature becomes valid immediately.
            SharedAccessBlobPolicy sasConstraints = new SharedAccessBlobPolicy
            {
                SharedAccessExpiryTime = DateTime.UtcNow.AddHours(4),
                Permissions = SharedAccessBlobPermissions.Write | SharedAccessBlobPermissions.List | SharedAccessBlobPermissions.Read
            };

            //Generate the shared access signature on the container, setting the constraints directly on the signature.
            string sasContainerToken = container.GetSharedAccessSignature(sasConstraints);

            //Return the URI string for the container, including the SAS token.
            return new Uri(container.Uri + sasContainerToken);
        }

        static Uri GetInputFileUrl(CloudBlobContainer container, string filePath)
        {
            //Set the expiry time and permissions for the container.
            //In this case no start time is specified, so the shared access signature becomes valid immediately.
            SharedAccessBlobPolicy sasConstraints = new SharedAccessBlobPolicy
            {
                SharedAccessExpiryTime = DateTime.UtcNow.AddHours(4),
                Permissions = SharedAccessBlobPermissions.Read
            };

            //Generate the shared access signature on the container, setting the constraints directly on the signature.
            string sasToken = container.GetBlockBlobReference(filePath).GetSharedAccessSignature(sasConstraints);

            //Return the URI string for the container, including the SAS token.
            return new Uri(container.Uri + "/" + filePath + sasToken);
        }

        static string GetJobPath(string namespaceName, string notificationHubPath, string jobId)
        {
            return string.Format(CultureInfo.InvariantCulture, @"{0}//{1}/{2}/", namespaceName, notificationHubPath, jobId);
        }
    }
}
```

## <a name="export"></a>Exporteren
Exporteren van de registratie is vergelijkbaar met het importeren, met de volgende verschillen:

- U hoeft alleen de URL van de uitvoer.
- U maakt een NotificationHubJob van het type ExportRegistrations.

### <a name="sample-code-snippet"></a>Voorbeeld-codefragment
Hier volgt een voorbeeld-codefragment voor het exporteren van registraties in Java:

```java
// submit an export job
NotificationHubJob job = new NotificationHubJob();
job.setJobType(NotificationHubJobType.ExportRegistrations);
job.setOutputContainerUri("container uri with SAS signature");
job = hub.submitNotificationHubJob(job);

// wait until the job is done
while(true){
    Thread.sleep(1000);
    job = hub.getNotificationHubJob(job.getJobId());
    if(job.getJobStatus() == NotificationHubJobStatus.Completed)
        break;
}

```

## <a name="next-steps"></a>Volgende stappen
Zie de volgende artikelen voor meer informatie over rapporten:

- [Registratiebeheer](notification-hubs-push-notification-registration-management.md)
- [Labels voor registraties](notification-hubs-tags-segment-push-message.md)
- [Sjabloonregistraties](notification-hubs-templates-cross-platform-push-messages.md)
