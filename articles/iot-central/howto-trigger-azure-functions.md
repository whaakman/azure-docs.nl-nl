---
title: Azure Functions activeren met behulp van webhooks in Azure IoT Central
description: Een functie-app die wordt uitgevoerd elke keer dat een regel wordt geactiveerd in Azure IoT Central maken.
author: viv-liu
ms.author: viviali
ms.date: 09/06/2018
ms.topic: conceptual
ms.service: iot-central
services: iot-central
manager: peterpr
ms.openlocfilehash: 7b14dc4eeec1ab543c407b1bb1cf8a5ce46f3ecb
ms.sourcegitcommit: af9cb4c4d9aaa1fbe4901af4fc3e49ef2c4e8d5e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/11/2018
ms.locfileid: "44356494"
---
# <a name="trigger-azure-functions-using-webhooks-in-azure-iot-central"></a>Azure Functions activeren met behulp van webhooks in Azure IoT Central

Azure Functions gebruiken voor serverloze code uitvoeren op de uitvoer van de webhook van IoT Central regels. U hoeft in te richten van een virtuele machine of een web-app voor het gebruik van Azure Functions publiceren, maar in plaats daarvan kunt u deze code serverlessly uitvoeren. Azure Functions gebruiken voor het transformeren van de nettolading van de webhook voordat deze worden verzonden naar de uiteindelijke bestemming, zoals een SQL-database of Event Grid. 

## <a name="prerequisites"></a>Vereiste onderdelen
+ Als u nog geen abonnement op Azure hebt, maak dan een [gratis account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) aan voordat u begint.

## <a name="how-to-connect-azure-functions"></a>Verbinding maken tussen Azure Functions

1. [Maak een nieuwe functie-app in Azure Portal. ](https://ms.portal.azure.com/#create/Microsoft.FunctionApp).
2. Vouw uw functie-app en klikt u op de knop naast functies +. Als deze functie het eerste item in uw functie-app is, selecteert u aangepaste functie. U ziet nu de volledige set het functiesjablonen.
3. Algemene Typ in het zoekveld en kies vervolgens de gewenste taal voor de algemene webhook-trigger-sjabloon. In dit onderwerp wordt gebruikgemaakt van een C#-functie. 
4. Klik in de nieuwe functie op **</> Functie-URL ophalen**, kopieer de waarde en sla deze op. U gebruikt deze waarde om de webhook te configureren. 
4. In IoT Central, vindt u de regel die u wilt verbinding maken met uw functie-app. 
5. Een webhookactie toevoegen. Voer een **weergavenaam** en plak deze in de functie-URL die u eerder hebt gekopieerd.
6. De regel niet opslaan. Wanneer de regel wordt geactiveerd, wordt de webhook nu aanroepen in de functie-app om uit te voeren. In uw functie-app ziet u de logboeken en Zie telkens wanneer de functie wordt geactiveerd.

Voor meer informatie gaat u naar de Azure Functions-artikel over [het maken van een door een algemene webhook geactiveerde functie](https://docs.microsoft.com/azure/azure-functions/functions-create-generic-webhook-triggered-function). 

## <a name="next-steps"></a>Volgende stappen
U hebt geleerd hoe u kunt instellen en gebruiken van webhooks, de voorgestelde volgende stap is om te verkennen [het bouwen van werkstromen in Microsoft Flow](howto-add-microsoft-flow.md).
