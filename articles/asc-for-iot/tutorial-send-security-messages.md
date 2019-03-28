---
title: Uw beveiligingsberichten verzenden naar ASC voor IoT-Preview | Microsoft Docs
description: Leer hoe u uw beveiligingsberichten verzenden met ASC voor IoT.
services: ascforiot
documentationcenter: na
author: mlottner
manager: barbkess
editor: ''
ms.assetid: c611bb5c-b503-487f-bef4-25d8a243803d
ms.service: ascforiot
ms.devlang: na
ms.topic: tutorial
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/26/2019
ms.author: mlottner
ms.openlocfilehash: 3daef0bfb2b9108e2b3c28ca0f6975a6f5768e92
ms.sourcegitcommit: cf971fe82e9ee70db9209bb196ddf36614d39d10
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/27/2019
ms.locfileid: "58541559"
---
# <a name="tutorial-send-security-messages-sdk"></a>Zelfstudie: Verzenden van beveiligingsberichten SDK

> [!IMPORTANT]
> ASC voor IoT is momenteel in openbare preview.
> Deze preview-versie wordt geleverd zonder een service level agreement, en wordt niet aanbevolen voor productieworkloads. Misschien worden bepaalde functies niet ondersteund of zijn de mogelijkheden ervan beperkt. Zie [Supplemental Terms of Use for Microsoft Azure Previews (Aanvullende gebruiksvoorwaarden voor Microsoft Azure-previews)](https://azure.microsoft.com/support/legal/preview-supplemental-terms/) voor meer informatie.

Deze zelfstudie worden ASC uitgelegd voor analysemogelijkheden van IoT-gegevens bij het kiezen voor het verzamelen en verzenden van uw apparaat beveiligingsberichten zonder gebruik te maken van een ASC voor IoT-agent en wordt uitgelegd hoe u om dit te doen.  

In deze zelfstudie leert u het volgende: 
> [!div class="checklist"]
> * Beveiligingsbericht verzenden API voor gebruikenC#
> * De beveiliging van het verzonden bericht API voor C gebruiken

## <a name="asc-for-iot"></a>ASC voor IoT 

ASC voor IoT kunt verwerken en analyseren van welke aard dan ook van de beveiligingsgegevens van het bericht, zolang de gegevens die worden verzonden, voldoet aan de [ASC voor IoT-schema](https://github.com/Azure/ASC-for-IoT-Schemas). 

## <a name="send-security-messages"></a>Beveiligingsberichten verzenden 

Beveiligingsberichten verzenden zonder de ASC voor IoT-agent, met behulp van de [Azure IoT device-SDK](https://github.com/Azure/azure-iot-sdk-csharp).

Voor het verzenden van het apparaatgegevens van uw apparaten voor verwerking door ASC voor IoT, door een van de volgende API's te gebruiken om berichten voor een correcte routering naar ASC voor IoT verwerkings-pipeline markeren. Berichten die worden verzonden op deze manier wordt verwerkt en weergegeven als beveiligingsinzichten binnen ASC voor IoT binnen zowel IoT-Hub of Azure Security Center. 

Alle gegevens die worden verzonden, zelfs als die zijn gemarkeerd met de juiste header moet ook voldoen aan de [ASC voor IoT-berichtschema](https://github.com/Azure/ASC-for-IoT-Schemas). 

> [!NOTE]
> Berichten die niet voldoen aan het schema worden genegeerd. Zorg ervoor dat u het schema controleren voordat u begint verzenden van gegevens, zoals genegeerd berichten worden momenteel niet opgeslagen. 

### <a name="send-security-message-api"></a>API voor security-bericht verzenden

De **security berichten verzenden** API is momenteel beschikbaar in C en C#.  

#### <a name="c-api"></a>C# API

```cs
private static async Task SendSecurityMessageAsync()
{
    string messageContent = "Security Data";
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