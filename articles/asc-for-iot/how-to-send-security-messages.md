---
title: Uw beveiligingsberichten verzenden naar Azure Security Center voor IoT-Preview | Microsoft Docs
description: Leer hoe u uw Azure Security Center gebruiken voor IoT security-berichten te verzenden.
services: ascforiot
documentationcenter: na
author: mlottner
manager: barbkess
editor: ''
ms.assetid: c611bb5c-b503-487f-bef4-25d8a243803d
ms.service: ascforiot
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/26/2019
ms.author: mlottner
ms.openlocfilehash: 2e8fc3e2cf85fb06d37347870e3b2b9c82f1aa96
ms.sourcegitcommit: 563f8240f045620b13f9a9a3ebfe0ff10d6787a2
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/01/2019
ms.locfileid: "58754776"
---
# <a name="send-security-messages-sdk"></a>Verzenden van beveiligingsberichten SDK

> [!IMPORTANT]
> Azure Security Center voor IoT is momenteel in openbare preview.
> Deze preview-versie wordt geleverd zonder een service level agreement, en wordt niet aanbevolen voor productieworkloads. Misschien worden bepaalde functies niet ondersteund of zijn de mogelijkheden ervan beperkt. Zie [Supplemental Terms of Use for Microsoft Azure Previews (Aanvullende gebruiksvoorwaarden voor Microsoft Azure-previews)](https://azure.microsoft.com/support/legal/preview-supplemental-terms/) voor meer informatie.

In deze gebruiksaanwijzing wordt uitgelegd van Azure Security Center (ASC) voor servicecapaciteiten IoT wanneer u kiest voor het verzamelen en verzenden van uw apparaat beveiligingsberichten zonder gebruik te maken van een ASC voor IoT-agent en wordt uitgelegd hoe u om dit te doen.  

In deze handleiding leert u het volgende: 
> [!div class="checklist"]
> * Beveiligingsbericht verzenden API voor gebruikenC#
> * De beveiliging van het verzonden bericht API voor C gebruiken

## <a name="asc-for-iot-capabilities"></a>ASC voor IoT-capaciteiten

ASC voor IoT kunt verwerken en analyseren van welke aard dan ook van de beveiligingsgegevens van het bericht, zolang de gegevens die worden verzonden, voldoet aan de [ASC voor IoT-schema](https://aka.ms/iot-security-schemas) en het bericht is ingesteld als een weergegeven.

## <a name="security-message"></a>Beveiligingsbericht

ASC voor IoT definieert een weergegeven met behulp van de volgende criteria:
- Als het bericht is verzonden met Azure IoT C /C# SDK
- Als het bericht voldoet aan de [security berichtschema](https://aka.ms/iot-security-schemas)
- Als het bericht is ingesteld als een weergegeven voordat deze worden verzonden

Elk beveiligingsbericht omvat de metagegevens van de afzender zoals `AgentId`, `AgentVersion`, `MessageSchemaVersion` en een lijst van beveiligingsgebeurtenissen.
Het schema wordt gedefinieerd voor de geldig en vereiste eigenschappen van het bericht met inbegrip van de typen gebeurtenissen.

[!NOTE]
> Berichten die niet voldoen aan het schema worden genegeerd. Zorg ervoor dat u het schema controleren voordat u begint verzenden van gegevens, zoals genegeerd berichten worden momenteel niet opgeslagen. 
> Berichten die niet zijn ingesteld als een bericht met de Azure IoT C /C# SDK niet worden doorgestuurd naar de ASC voor IoT-pijplijn

## <a name="valid-message-example"></a>Voorbeeld van een ongeldig bericht

Het volgende voorbeeld ziet u een geldige beveiligingsobject bericht. Het voorbeeld bevat de metagegevens van het bericht en een `ProcessCreate` beveiligingsgebeurtenis.

Nadat deze is ingesteld als een bericht en verzonden, dit bericht wordt verwerkt door ASC voor IoT.

```json
"AgentVersion": "0.0.1",
"AgentId": "e89dc5f5-feac-4c3e-87e2-93c16f010c25",
"MessageSchemaVersion": "1.0",
"Events": [
    {
        "EventType": "Security",
        "Category": "Triggered",
        "Name": "ProcessCreate",
        "IsEmpty": false,
        "PayloadSchemaVersion": "1.0",
        "Id": "21a2db0b-44fe-42e9-9cff-bbb2d8fdf874",
        "TimestampLocal": "2019-01-27 15:48:52Z",
        "TimestampUTC": "2019-01-27 13:48:52Z",
        "Payload":
            [
                {
                    "Executable": "/usr/bin/echo",
                    "ProcessId": 11750,
                    "ParentProcessId": 1593,
                    "UserName": "nginx",
                    "CommandLine": "./backup .htaccess"
                }
            ]
    }
]
```

## <a name="send-security-messages"></a>Beveiligingsberichten verzenden 

Beveiligingsberichten verzenden zonder de ASC voor IoT-agent, met behulp van de [Azure IoT C# apparaat-SDK](https://github.com/Azure/azure-iot-sdk-csharp) of [apparaat Azure IoT C SDK](https://github.com/Azure/azure-iot-sdk-c).

Voor het verzenden van het apparaatgegevens van uw apparaten voor verwerking door ASC voor IoT, door een van de volgende API's te gebruiken om berichten voor een correcte routering naar ASC voor IoT verwerkings-pipeline markeren. Berichten die worden verzonden op deze manier wordt verwerkt en weergegeven als beveiligingsinzichten binnen ASC voor IoT binnen zowel IoT-Hub of Azure Security Center. 

Alle gegevens die worden verzonden, zelfs als die zijn gemarkeerd met de juiste header moet ook voldoen aan de [ASC voor IoT-berichtschema](https://aka.ms/iot-security-schemas). 

### <a name="send-security-message-api"></a>API voor security-bericht verzenden

De **security berichten verzenden** API is momenteel beschikbaar in C en C#.  

#### <a name="c-api"></a>C# API

```cs

private static async Task SendSecurityMessageAsync(string messageContent)
{
    ModuleClient client = ModuleClient.CreateFromConnectionString("<connection_string>");
    Message  securityMessage = new Message(Encoding.UTF8.GetBytes(messageContent));
    securityMessage.SetAsSecurityMessage();
    await client.SendEventAsync(securityMessage);
}
```

#### <a name="c-api"></a>C API

```c
bool SendMessageAsync(IoTHubAdapter* iotHubAdapter, const void* data, size_t dataSize) {
 
    bool success = true;
    IOTHUB_MESSAGE_HANDLE messageHandle = NULL;
 
    messageHandle = IoTHubMessage_CreateFromByteArray(data, dataSize);
 
    if (messageHandle == NULL) {
        success = false;
        goto cleanup;
    }
 
    if (IoTHubMessage_SetAsSecurityMessage(messageHandle) != IOTHUB_MESSAGE_OK) {
        success = false;
        goto cleanup;
    }
 
    if (IoTHubModuleClient_SendEventAsync(iotHubAdapter->moduleHandle, messageHandle, SendConfirmCallback, iotHubAdapter) != IOTHUB_CLIENT_OK) {
        success = false;
        goto cleanup;
    }
 
cleanup:
    if (messageHandle != NULL) {
        IoTHubMessage_Destroy(messageHandle);
    }
 
    return success;
}
 
static void SendConfirmCallback(IOTHUB_CLIENT_CONFIRMATION_RESULT result, void* userContextCallback) {
    if (userContextCallback == NULL) {
        //error handling
        return;
    }
 
    if (result != IOTHUB_CLIENT_CONFIRMATION_OK){
        //error handling
    }
}
```

## <a name="next-steps"></a>Volgende stappen
- Lees de ASC voor IoT-service [overzicht](overview.md)
- Meer informatie over ASC voor IoT [architectuur](architecture.md)
- Schakel de [service](quickstart-onboard-iot-hub.md)
- Lees de [Veelgestelde vragen](resources-frequently-asked-questions.md)
- Leer hoe u toegang tot [onbewerkte beveiligingsgegevens](how-to-security-data-access.md)
- Inzicht in [aanbevelingen](concept-recommendations.md)
- Inzicht in [waarschuwingen](concept-security-alerts.md)
