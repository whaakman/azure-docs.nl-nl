---
title: Azure Queue storage gebruiken voor het bewaken van taakmeldingen Media Services met .NET | Microsoft Docs
description: Leer hoe u Azure Queue storage gebruiken voor het bewaken van taakmeldingen Media Services. De voorbeeldcode is geschreven in C# en maakt gebruik van de Media Services SDK voor .NET.
services: media-services
documentationcenter: ''
author: juliako
manager: femila
editor: ''
ms.assetid: f535d0b5-f86c-465f-81c6-177f4f490987
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 02/09/2019
ms.author: juliako
ms.openlocfilehash: a863915ceef437957170d87ac4cc452d651a8dab
ms.sourcegitcommit: e69fc381852ce8615ee318b5f77ae7c6123a744c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/11/2019
ms.locfileid: "55994942"
---
# <a name="use-azure-queue-storage-to-monitor-media-services-job-notifications-with-net-legacy"></a>Azure Queue storage gebruiken voor het bewaken van taakmeldingen Media Services met .NET (verouderd)

Wanneer u coderingstaken uitvoert, moet u vaak een manier om bij te houden van de taak wordt uitgevoerd. U kunt Media Services voor het leveren van meldingen configureren [Azure Queue storage](../../storage/storage-dotnet-how-to-use-queues.md). U kunt taakvoortgang controleren met het ophalen van meldingen vanuit de Queue-opslag. 

Berichten die worden geleverd aan Queue storage zijn toegankelijk vanaf overal ter wereld. De berichten opslagarchitectuur van wachtrij is betrouwbare en zeer schaalbare. Queue storage voor berichten polling geniet de voorkeur boven het gebruik van andere methoden.

Een veelvoorkomend scenario voor het luisteren naar Media Services-meldingen is dat als u ontwikkelt een inhoudsbeheersysteem die nodig zijn voor een extra taak na een coderingstaak is voltooid (bijvoorbeeld, voor een trigger die de volgende stap in een werkstroom of om te publiceren inhoud).

Dit artikel leest hoe u meldingen van Queue storage.  

## <a name="considerations"></a>Overwegingen
Overweeg het volgende bij het ontwikkelen van Media Services-toepassingen die gebruikmaken van Queue storage:

* Wachtrijopslag biedt geen garantie van first in first out (FIFO) besteld levering. Zie voor meer informatie, [Azure Queues en Azure Service Bus-wachtrijen vergeleken en Contrasted](https://msdn.microsoft.com/library/azure/hh767287.aspx).
* Queue-opslag is niet een push-service. U moet pollen van de wachtrij.
* U kunt een onbeperkt aantal wachtrijen hebben. Zie voor meer informatie, [REST-API voor Queue-Service](https://docs.microsoft.com/rest/api/storageservices/Queue-Service-REST-API).
* Queue-opslag heeft enkele beperkingen en specificaties rekening mee moet houden. Deze worden beschreven in [Azure Queues en Azure Service Bus-wachtrijen vergeleken en Contrasted](https://docs.microsoft.com/azure/service-bus-messaging/service-bus-azure-and-service-bus-queues-compared-contrasted).

## <a name="net-code-example"></a>.NET-codevoorbeeld

Het voorbeeld in deze sectie doet het volgende:

1. Definieert de **EncodingJobMessage** klasse die is toegewezen aan de notification-berichtindeling. De code gedeserialiseerd berichten ontvangen uit de wachtrij in objecten van de **EncodingJobMessage** type.
2. Laadt de accountgegevens voor Media Services en de opslag van het bestand app.config. Het codevoorbeeld gebruikt deze informatie om te maken de **CloudMediaContext** en **CloudQueue** objecten.
3. Hiermee maakt u de wachtrij die meldingen over de coderingstaak ontvangt.
4. Hiermee maakt u de melding-eindpunt dat is toegewezen aan de wachtrij.
5. Het eindpunt van de melding is gekoppeld aan de taak en de coderingstaak indient. U kunt meerdere notification-eindpunten die zijn gekoppeld aan een taak hebben.
6. Passen **NotificationJobState.FinalStatesOnly** naar de **AddNew** methode. (In dit voorbeeld zijn we alleen geïnteresseerd in de laatste status van de verwerking van een taak.)

        job.JobNotificationSubscriptions.AddNew(NotificationJobState.FinalStatesOnly, _notificationEndPoint);
7. Als u doorgeeft **NotificationJobState.All**, krijgt u de volgende status wijzigingsmeldingen: in de wachtrij, gepland, verwerking en voltooid. Echter is eerder hebt genoteerd, Queue storage geen garantie geordende levering. Op volgorde van berichten, gebruikt u de **Timestamp** eigenschap (gedefinieerd in de **EncodingJobMessage** type in het onderstaande voorbeeld). Er zijn dubbele berichten mogelijk. Als u wilt controleren op duplicaten, gebruiken de **ETag eigenschap** (gedefinieerd op de **EncodingJobMessage** type). Het is ook mogelijk dat sommige wijzigingsmeldingen status ophalen overgeslagen.
8. Wacht tot de taak om de status voltooid door het controleren van de wachtrij elke 10 seconden. Worden berichten verwijderd nadat ze zijn verwerkt.
9. Hiermee verwijdert u de wachtrij en het eindpunt van de melding.

> [!NOTE]
> De aanbevolen manier voor het bewaken van de status van een taak is door te luisteren naar meldingen, zoals wordt weergegeven in het volgende voorbeeld:
>
> U kunt ook u kunt controleren over de status van een taak met behulp van de **IJob.State** eigenschap.  Op een melding over van een taak is voltooid voordat de status kan binnenkomen **IJob** is ingesteld op **voltooid**. De **IJob.State** eigenschap weerspiegelt de nauwkeurige staat met een kleine vertraging.
>
>

### <a name="create-and-configure-a-visual-studio-project"></a>Maak en configureer een Visual Studio-project.

1. Stel uw ontwikkelomgeving in en vul in het bestand app.config de verbindingsinformatie in, zoals beschreven in [Media Services ontwikkelen met .NET](media-services-dotnet-how-to-use.md). 
2. Maak een nieuwe map (map kan zich overal op uw lokale schijf) en kopieer een MP4-bestand dat u wilt coderen en streamen of progressief wilt downloaden. In dit voorbeeld wordt het pad 'C:\Media' gebruikt.
3. Voeg een verwijzing naar de **System.Runtime.Serialization** bibliotheek.

### <a name="code"></a>Code

```csharp
using System;
using System.Linq;
using System.Configuration;
using System.IO;
using System.Threading;
using System.Collections.Generic;
using Microsoft.WindowsAzure.MediaServices.Client;
using Microsoft.WindowsAzure.Storage;
using Microsoft.WindowsAzure.Storage.Queue;
using System.Runtime.Serialization.Json;

namespace JobNotification
{
    public class EncodingJobMessage
    {
        // MessageVersion is used for version control.
        public String MessageVersion { get; set; }

        // Type of the event. Valid values are
        // JobStateChange and NotificationEndpointRegistration.
        public String EventType { get; set; }

        // ETag is used to help the customer detect if
        // the message is a duplicate of another message previously sent.
        public String ETag { get; set; }

        // Time of occurrence of the event.
        public String TimeStamp { get; set; }

        // Collection of values specific to the event.

        // For the JobStateChange event the values are:
        //     JobId - Id of the Job that triggered the notification.
        //     NewState- The new state of the Job. Valid values are:
        //          Scheduled, Processing, Canceling, Cancelled, Error, Finished
        //     OldState- The old state of the Job. Valid values are:
        //          Scheduled, Processing, Canceling, Cancelled, Error, Finished

        // For the NotificationEndpointRegistration event the values are:
        //     NotificationEndpointId- Id of the NotificationEndpoint
        //          that triggered the notification.
        //     State- The state of the Endpoint.
        //          Valid values are: Registered and Unregistered.

        public IDictionary<string, object> Properties { get; set; }
    }

    class Program
    {

        // Read values from the App.config file.
        private static readonly string _AADTenantDomain =
            ConfigurationManager.AppSettings["AMSAADTenantDomain"];
        private static readonly string _RESTAPIEndpoint =
            ConfigurationManager.AppSettings["AMSRESTAPIEndpoint"];
        private static readonly string _AMSClientId =
            ConfigurationManager.AppSettings["AMSClientId"];
        private static readonly string _AMSClientSecret =
            ConfigurationManager.AppSettings["AMSClientSecret"];

        private static readonly string _StorageConnectionString = 
            ConfigurationManager.AppSettings["StorageConnectionString"];

        private static CloudMediaContext _context = null;
        private static CloudQueue _queue = null;
        private static INotificationEndPoint _notificationEndPoint = null;

        private static readonly string _singleInputMp4Path =
            Path.GetFullPath(@"C:\Media\BigBuckBunny.mp4");

        static void Main(string[] args)
        {
            string endPointAddress = Guid.NewGuid().ToString();

            // Create the context.
            AzureAdTokenCredentials tokenCredentials = 
                new AzureAdTokenCredentials(_AADTenantDomain,
                    new AzureAdClientSymmetricKey(_AMSClientId, _AMSClientSecret),
                    AzureEnvironments.AzureCloudEnvironment);

            var tokenProvider = new AzureAdTokenProvider(tokenCredentials);

            _context = new CloudMediaContext(new Uri(_RESTAPIEndpoint), tokenProvider);
            
            // Create the queue that will be receiving the notification messages.
            _queue = CreateQueue(_StorageConnectionString, endPointAddress);

            // Create the notification point that is mapped to the queue.
            _notificationEndPoint =
                    _context.NotificationEndPoints.Create(
                    Guid.NewGuid().ToString(), NotificationEndPointType.AzureQueue, endPointAddress);


            if (_notificationEndPoint != null)
            {
                IJob job = SubmitEncodingJobWithNotificationEndPoint(_singleInputMp4Path);
                WaitForJobToReachedFinishedState(job.Id);
            }

            // Clean up.
            _queue.Delete();
            _notificationEndPoint.Delete();
        }


        static public CloudQueue CreateQueue(string storageAccountConnectionString, string endPointAddress)
        {
            CloudStorageAccount storageAccount = CloudStorageAccount.Parse(storageAccountConnectionString);

            // Create the queue client
            CloudQueueClient queueClient = storageAccount.CreateCloudQueueClient();

            // Retrieve a reference to a queue
            CloudQueue queue = queueClient.GetQueueReference(endPointAddress);

            // Create the queue if it doesn't already exist
            queue.CreateIfNotExists();

            return queue;
        }


        public static IJob SubmitEncodingJobWithNotificationEndPoint(string inputMediaFilePath)
        {
            // Declare a new job.
            IJob job = _context.Jobs.Create("My MP4 to Smooth Streaming encoding job");

            //Create an encrypted asset and upload the mp4.
            IAsset asset = CreateAssetAndUploadSingleFile(AssetCreationOptions.StorageEncrypted,
                inputMediaFilePath);

            // Get a media processor reference, and pass to it the name of the
            // processor to use for the specific task.
            IMediaProcessor processor = GetLatestMediaProcessorByName("Media Encoder Standard");

            // Create a task with the conversion details, using a configuration file.
            ITask task = job.Tasks.AddNew("My encoding Task",
                processor,
                "Adaptive Streaming",
                Microsoft.WindowsAzure.MediaServices.Client.TaskOptions.None);

            // Specify the input asset to be encoded.
            task.InputAssets.Add(asset);

            // Add an output asset to contain the results of the job.
            task.OutputAssets.AddNew("Output asset",
                AssetCreationOptions.None);

            // Add a notification point to the job. You can add multiple notification points.  
            job.JobNotificationSubscriptions.AddNew(NotificationJobState.FinalStatesOnly,
                _notificationEndPoint);

            job.Submit();

            return job;
        }

        public static void WaitForJobToReachedFinishedState(string jobId)
        {
            int expectedState = (int)JobState.Finished;
            int timeOutInSeconds = 600;

            bool jobReachedExpectedState = false;
            DateTime startTime = DateTime.Now;
            int jobState = -1;

            while (!jobReachedExpectedState)
            {
                // Specify how often you want to get messages from the queue.
                Thread.Sleep(TimeSpan.FromSeconds(10));

                foreach (var message in _queue.GetMessages(10))
                {
                    using (Stream stream = new MemoryStream(message.AsBytes))
                    {
                        DataContractJsonSerializerSettings settings = new DataContractJsonSerializerSettings();
                        settings.UseSimpleDictionaryFormat = true;
                        DataContractJsonSerializer ser = new DataContractJsonSerializer(typeof(EncodingJobMessage), settings);
                        EncodingJobMessage encodingJobMsg = (EncodingJobMessage)ser.ReadObject(stream);

                        Console.WriteLine();

                        // Display the message information.
                        Console.WriteLine("EventType: {0}", encodingJobMsg.EventType);
                        Console.WriteLine("MessageVersion: {0}", encodingJobMsg.MessageVersion);
                        Console.WriteLine("ETag: {0}", encodingJobMsg.ETag);
                        Console.WriteLine("TimeStamp: {0}", encodingJobMsg.TimeStamp);
                        foreach (var property in encodingJobMsg.Properties)
                        {
                            Console.WriteLine("    {0}: {1}", property.Key, property.Value);
                        }

                        // We are only interested in messages
                        // where EventType is "JobStateChange".
                        if (encodingJobMsg.EventType == "JobStateChange")
                        {
                            string JobId = (String)encodingJobMsg.Properties.Where(j => j.Key == "JobId").FirstOrDefault().Value;
                            if (JobId == jobId)
                            {
                                string oldJobStateStr = (String)encodingJobMsg.Properties.
                                                            Where(j => j.Key == "OldState").FirstOrDefault().Value;
                                string newJobStateStr = (String)encodingJobMsg.Properties.
                                                            Where(j => j.Key == "NewState").FirstOrDefault().Value;

                                JobState oldJobState = (JobState)Enum.Parse(typeof(JobState), oldJobStateStr);
                                JobState newJobState = (JobState)Enum.Parse(typeof(JobState), newJobStateStr);

                                if (newJobState == (JobState)expectedState)
                                {
                                    Console.WriteLine("job with Id: {0} reached expected state: {1}",
                                        jobId, newJobState);
                                    jobReachedExpectedState = true;
                                    break;
                                }
                            }
                        }
                    }
                    // Delete the message after we've read it.
                    _queue.DeleteMessage(message);
                }

                // Wait until timeout
                TimeSpan timeDiff = DateTime.Now - startTime;
                bool timedOut = (timeDiff.TotalSeconds > timeOutInSeconds);
                if (timedOut)
                {
                    Console.WriteLine(@"Timeout for checking job notification messages,
                                        latest found state ='{0}', wait time = {1} secs",
                        jobState,
                        timeDiff.TotalSeconds);

                    throw new TimeoutException();
                }
            }
        }

        static private IAsset CreateAssetAndUploadSingleFile(AssetCreationOptions assetCreationOptions, string singleFilePath)
        {
            var asset = _context.Assets.Create("UploadSingleFile_" + DateTime.UtcNow.ToString(),
                assetCreationOptions);

            var fileName = Path.GetFileName(singleFilePath);

            var assetFile = asset.AssetFiles.Create(fileName);

            Console.WriteLine("Created assetFile {0}", assetFile.Name);
            Console.WriteLine("Upload {0}", assetFile.Name);

            assetFile.Upload(singleFilePath);
            Console.WriteLine("Done uploading of {0}", assetFile.Name);

            return asset;
        }

        static private IMediaProcessor GetLatestMediaProcessorByName(string mediaProcessorName)
        {
            var processor = _context.MediaProcessors.Where(p => p.Name == mediaProcessorName).
                ToList().OrderBy(p => new Version(p.Version)).LastOrDefault();

            if (processor == null)
                throw new ArgumentException(string.Format("Unknown media processor", mediaProcessorName));

            return processor;
        }
    }
}
```

Het vorige voorbeeld gemaakt in de volgende uitvoer: Uw waarden variëren.

    Created assetFile BigBuckBunny.mp4
    Upload BigBuckBunny.mp4
    Done uploading of BigBuckBunny.mp4

    EventType: NotificationEndPointRegistration
    MessageVersion: 1.0
    ETag: e0238957a9b25bdf3351a88e57978d6a81a84527fad03bc23861dbe28ab293f6
    TimeStamp: 2013-05-14T20:22:37
        NotificationEndPointId: nb:nepid:UUID:d6af9412-2488-45b2-ba1f-6e0ade6dbc27
        State: Registered
        Name: dde957b2-006e-41f2-9869-a978870ac620
        Created: 2013-05-14T20:22:35

    EventType: JobStateChange
    MessageVersion: 1.0
    ETag: 4e381f37c2d844bde06ace650310284d6928b1e50101d82d1b56220cfcb6076c
    TimeStamp: 2013-05-14T20:24:40
        JobId: nb:jid:UUID:526291de-f166-be47-b62a-11ffe6d4be54
        JobName: My MP4 to Smooth Streaming encoding job
        NewState: Finished
        OldState: Processing
        AccountName: westeuropewamsaccount
    job with Id: nb:jid:UUID:526291de-f166-be47-b62a-11ffe6d4be54 reached expected
    State: Finished


## <a name="next-step"></a>Volgende stap
Media Services-leertrajecten bekijken.

[!INCLUDE [media-services-learning-paths-include](../../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Feedback geven
[!INCLUDE [media-services-user-voice-include](../../../includes/media-services-user-voice-include.md)]
