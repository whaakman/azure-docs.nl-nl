---
title: Polling langlopende bewerkingen | Microsoft Docs
description: Dit onderwerp wordt beschreven hoe pollen langlopende bewerkingen.
services: media-services
documentationcenter: 
author: juliako
manager: cfowler
editor: 
ms.assetid: 9a68c4b1-6159-42fe-9439-a3661a90ae03
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/18/2017
ms.author: juliako
ms.openlocfilehash: 7123a2d44d3b7c332afe30fb0fcea88ca29e313a
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/29/2017
---
# <a name="delivering-live-streaming-with-azure-media-services"></a>Live streamen met Azure mediaservices leveren

## <a name="overview"></a>Overzicht

Microsoft Azure Media Services biedt API's die aanvragen verzenden naar de Media Services activiteiten te starten (bijvoorbeeld: maken, starten, stoppen of verwijderen van een kanaal). Deze bewerkingen zijn langlopend.

Media Services .NET SDK biedt API's die de aanvraag verzendt en wacht totdat de bewerking is voltooid (intern maakt de API's zijn gedelegeerd voor het bewerkingsvoortgang op bepaalde tijden). Bijvoorbeeld bij het aanroepen van kanaal. Start(), de methode retourneert nadat het kanaal is gestart. U kunt ook de asynchrone versie: wachten op een kanaal. StartAsync() (Zie voor meer informatie over de taakgebaseerde asynchrone patroon [tik](https://msdn.microsoft.com/library/hh873175\(v=vs.110\).aspx)). API's die een aanvraag verzendt en vervolgens poll-frequentie voor de status totdat de bewerking voltooid is, worden 'polling methoden' genoemd. Deze methoden (met name de Async-versie) worden aanbevolen voor interactieve toepassingen en/of stateful services.

Er zijn scenario's waarbij een toepassing niet kan wachten op een langdurige HTTP-verzoek en wil poll-frequentie voor de voortgang van de bewerking handmatig. Een typisch voorbeeld zou een browser interactie met een stateless webservice: als de browser aanvraagt een kanaal te maken, de webservice initieert een langdurige bewerking en retourneert de bewerkings-ID in de browser. De browser kan vervolgens vraagt u de web-service om op te halen van de status van de bewerking op basis van de-ID. Media Services .NET SDK biedt API's die handig voor dit scenario zijn. Deze API's worden 'niet polling methoden' genoemd.
'Niet polling methoden' hebben de volgende naamgevingspatroon: verzenden*OperationName*bewerking (bijvoorbeeld SendCreateOperation). Verzenden*OperationName*bewerking methoden retourneren de **IOperation** object; het geretourneerde object bevat informatie die kan worden gebruikt voor het bijhouden van de bewerking. De verzenden*OperationName*OperationAsync methoden retourneren **taak<IOperation>**.

Op dit moment wordt de volgende klassen ondersteuning voor methoden niet polling: **kanaal**, **StreamingEndpoint**, en **programma**.

Om te controleren voor de status van de bewerking, gebruiken de **GetOperation** methode op de **OperationBaseCollection** klasse. Gebruik de volgende intervallen om te controleren van de bewerkingsstatus: voor **kanaal** en **StreamingEndpoint** bewerkingen, gebruikt u 30 seconden; voor **programma** bewerkingen, gebruik van 10 seconden.

## <a name="create-and-configure-a-visual-studio-project"></a>Maak en configureer een Visual Studio-project.

Stel uw ontwikkelomgeving in en vul in het bestand app.config de verbindingsinformatie in, zoals beschreven in [Media Services ontwikkelen met .NET](media-services-dotnet-how-to-use.md).

## <a name="example"></a>Voorbeeld

Het volgende voorbeeld definieert een klasse met de naam **ChannelOperations**. De definitie van deze klasse kan een startpunt voor de klassedefinitie van uw web-service zijn. Voor eenvoud gebruik de volgende voorbeelden de versies niet asynchrone methoden.

Het voorbeeld ziet ook hoe de client deze klasse zou kunnen gebruiken.

### <a name="channeloperations-class-definition"></a>De klassendefinitie ChannelOperations

    using Microsoft.WindowsAzure.MediaServices.Client;
    using System;
    using System.Collections.Generic;
    using System.Configuration;
    using System.Net;

    /// <summary> 
    /// The ChannelOperations class only implements 
    /// the Channel’s creation operation. 
    /// </summary> 
    public class ChannelOperations
    {
        // Read values from the App.config file.
        private static readonly string _AADTenantDomain =
            ConfigurationManager.AppSettings["AADTenantDomain"];
        private static readonly string _RESTAPIEndpoint =
            ConfigurationManager.AppSettings["MediaServiceRESTAPIEndpoint"];

        // Field for service context.
        private static CloudMediaContext _context = null;

        public ChannelOperations()
        {
            var tokenCredentials = new AzureAdTokenCredentials(_AADTenantDomain, AzureEnvironments.AzureCloudEnvironment);
            var tokenProvider = new AzureAdTokenProvider(tokenCredentials);

            _context = new CloudMediaContext(new Uri(_RESTAPIEndpoint), tokenProvider);
        }

        /// <summary>  
        /// Initiates the creation of a new channel.  
        /// </summary>  
        /// <param name="channelName">Name to be given to the new channel</param>  
        /// <returns>  
        /// Operation Id for the long running operation being executed by Media Services. 
        /// Use this operation Id to poll for the channel creation status. 
        /// </returns> 
        public string StartChannelCreation(string channelName)
        {
            var operation = _context.Channels.SendCreateOperation(
                new ChannelCreationOptions
                {
                    Name = channelName,
                    Input = CreateChannelInput(),
                    Preview = CreateChannelPreview(),
                    Output = CreateChannelOutput()
                });

            return operation.Id;
        }

        /// <summary> 
        /// Checks if the operation has been completed. 
        /// If the operation succeeded, the created channel Id is returned in the out parameter.
        /// </summary> 
        /// <param name="operationId">The operation Id.</param> 
        /// <param name="channel">
        /// If the operation succeeded, 
        /// the created channel Id is returned in the out parameter.</param>
        /// <returns>Returns false if the operation is still in progress; otherwise, true.</returns> 
        public bool IsCompleted(string operationId, out string channelId)
        {
            IOperation operation = _context.Operations.GetOperation(operationId);
            bool completed = false;

            channelId = null;

            switch (operation.State)
            {
                case OperationState.Failed:
                    // Handle the failure. 
                    // For example, throw an exception. 
                    // Use the following information in the exception: operationId, operation.ErrorMessage.
                    break;
                case OperationState.Succeeded:
                    completed = true;
                    channelId = operation.TargetEntityId;
                    break;
                case OperationState.InProgress:
                    completed = false;
                    break;
            }
            return completed;
        }

        private static ChannelInput CreateChannelInput()
        {
            return new ChannelInput
            {
                StreamingProtocol = StreamingProtocol.RTMP,
                AccessControl = new ChannelAccessControl
                {
                    IPAllowList = new List<IPRange>
                        {
                            new IPRange
                            {
                                Name = "TestChannelInput001",
                                Address = IPAddress.Parse("0.0.0.0"),
                                SubnetPrefixLength = 0
                            }
                        }
                }
            };
        }

        private static ChannelPreview CreateChannelPreview()
        {
            return new ChannelPreview
            {
                AccessControl = new ChannelAccessControl
                {
                    IPAllowList = new List<IPRange>
                        {
                            new IPRange
                            {
                                Name = "TestChannelPreview001",
                                Address = IPAddress.Parse("0.0.0.0"),
                                SubnetPrefixLength = 0
                            }
                        }
                }
            };
        }

        private static ChannelOutput CreateChannelOutput()
        {
            return new ChannelOutput
            {
                Hls = new ChannelOutputHls { FragmentsPerSegment = 1 }
            };
        }
    }

### <a name="the-client-code"></a>De clientcode
    ChannelOperations channelOperations = new ChannelOperations();
    string opId = channelOperations.StartChannelCreation("MyChannel001");

    string channelId = null;
    bool isCompleted = false;

    while (isCompleted == false)
    {
        System.Threading.Thread.Sleep(TimeSpan.FromSeconds(30));
        isCompleted = channelOperations.IsCompleted(opId, out channelId);
    }

    // If we got here, we should have the newly created channel id.
    Console.WriteLine(channelId);



## <a name="media-services-learning-paths"></a>Media Services-leertrajecten
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Feedback geven
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

