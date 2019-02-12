---
title: Opvragen van configuratiegegevens bij langlopende bewerkingen | Microsoft Docs
description: In dit onderwerp laat zien hoe pollen van langlopende bewerkingen.
services: media-services
documentationcenter: ''
author: juliako
manager: femila
editor: ''
ms.assetid: 9a68c4b1-6159-42fe-9439-a3661a90ae03
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/09/2019
ms.author: juliako
ms.openlocfilehash: f5098b2691f7c73be5df6b44479082bf25effde7
ms.sourcegitcommit: e69fc381852ce8615ee318b5f77ae7c6123a744c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/11/2019
ms.locfileid: "55998036"
---
# <a name="delivering-live-streaming-with-azure-media-services"></a>Live streamen met Azure mediaservices leveren

## <a name="overview"></a>Overzicht

Microsoft Azure Media Services biedt API's die aanvragen verzenden naar Media Services activiteiten te starten (bijvoorbeeld: maken, starten, stoppen of verwijderen van een kanaal). Deze bewerkingen worden langlopende.

De Media Services .NET SDK biedt API's die de aanvraag verzenden en wacht totdat de bewerking is voltooid (intern, de API's zijn polling voor de bewerking wordt uitgevoerd met bepaalde intervallen). Bijvoorbeeld, wanneer u kanaal aanroepen. Start(), retourneert de methode nadat het kanaal wordt gestart. U kunt ook de asynchrone versie: wachten op een kanaal. StartAsync() (Zie voor meer informatie over het asynchrone patroon op basis van een taak [tik](https://msdn.microsoft.com/library/hh873175\(v=vs.110\).aspx)). API's die regelmatig wordt bevraagd voor de status totdat de bewerking voltooid is en verzenden van een aanvraag opnieuw worden 'methoden polling' genoemd. Deze methoden (met name de asynchrone versie) worden aanbevolen voor interactieve toepassingen en/of stateful services.

Er zijn scenario's waarin een toepassing niet kunt wachten op een langdurige http-aanvraag en wil te pollen voor de bewerkingsvoortgang van de handmatig. Een typisch voorbeeld zou een browser interactie met een stateless webservice: wanneer de browser vraagt om een kanaal te maken, de webservice initieert een langdurige bewerking en de bewerkings-ID geretourneerd naar de browser. De browser kan vervolgens vraagt u de web-service om op te halen van de status van de bewerking op basis van de ID. De Media Services .NET SDK biedt API's die nuttig voor dit scenario zijn. Deze API's worden 'methoden niet polling' genoemd.
De 'niet-polling-methoden' hebben een naamgevingspatroon uit het volgende: Verzenden*OperationName*bewerking (bijvoorbeeld SendCreateOperation). Verzenden*OperationName*bewerking methoden retourneren de **IOperation** object; het geretourneerde object bevat informatie die kan worden gebruikt voor het bijhouden van de bewerking. De verzenden*OperationName*OperationAsync methoden retourneren **taak<IOperation>**.

Op dit moment ondersteunen de volgende klassen niet polling methoden:  **Kanaal**, **streamingendpoint zo**, en **programma**.

Als u wilt laten pollen voor de status van de bewerking, gebruikt u de **GetOperation** methode voor het **OperationBaseCollection** klasse. Gebruik de volgende intervallen om te controleren of de status van de bewerking: voor **kanaal** en **streamingendpoint zo** bewerkingen, gebruikt u 30 seconden; voor **programma** bewerkingen, gebruik van 10 seconden.

## <a name="create-and-configure-a-visual-studio-project"></a>Maak en configureer een Visual Studio-project.

Stel uw ontwikkelomgeving in en vul in het bestand app.config de verbindingsinformatie in, zoals beschreven in [Media Services ontwikkelen met .NET](media-services-dotnet-how-to-use.md).

## <a name="example"></a>Voorbeeld

Het volgende voorbeeld definieert een klasse genaamd **ChannelOperations**. Deze klassedefinitie wordt mogelijk een startpunt voor de klassedefinitie van uw web-service. De volgende voorbeelden gebruiken voor het gemak de niet-asynchrone versies van methoden.

Het voorbeeld ook ziet u hoe de client kan deze klasse.

### <a name="channeloperations-class-definition"></a>Definitie van de klasse ChannelOperations

```csharp
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
        ConfigurationManager.AppSettings["AMSAADTenantDomain"];
    private static readonly string _RESTAPIEndpoint =
        ConfigurationManager.AppSettings["AMSRESTAPIEndpoint"];
    private static readonly string _AMSClientId =
        ConfigurationManager.AppSettings["AMSClientId"];
    private static readonly string _AMSClientSecret =
        ConfigurationManager.AppSettings["AMSClientSecret"];

    // Field for service context.
    private static CloudMediaContext _context = null;

    public ChannelOperations()
    {
        AzureAdTokenCredentials tokenCredentials = 
            new AzureAdTokenCredentials(_AADTenantDomain,
                new AzureAdClientSymmetricKey(_AMSClientId, _AMSClientSecret),
                AzureEnvironments.AzureCloudEnvironment);

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
```

### <a name="the-client-code"></a>De clientcode

```csharp
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
```

## <a name="media-services-learning-paths"></a>Media Services-leertrajecten
[!INCLUDE [media-services-learning-paths-include](../../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Feedback geven
[!INCLUDE [media-services-user-voice-include](../../../includes/media-services-user-voice-include.md)]

