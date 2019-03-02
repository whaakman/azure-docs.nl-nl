---
title: Azure Functions activeren met behulp van webhooks in Azure IoT Central
description: Een functie-app die wordt uitgevoerd elke keer dat een regel wordt geactiveerd in Azure IoT Central maken.
author: viv-liu
ms.author: viviali
ms.date: 02/20/2018
ms.topic: conceptual
ms.service: iot-central
services: iot-central
manager: peterpr
ms.openlocfilehash: 8d638743b3cb07b0d060e754ecb76e74bd02719d
ms.sourcegitcommit: c712cb5c80bed4b5801be214788770b66bf7a009
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/01/2019
ms.locfileid: "57215010"
---
# <a name="trigger-azure-functions-using-webhooks-in-azure-iot-central"></a>Azure Functions activeren met behulp van webhooks in Azure IoT Central

*In dit onderwerp is van toepassing op builders en beheerders.*

Azure Functions gebruiken voor serverloze code uitvoeren op de uitvoer van de webhook van IoT Central regels. U hoeft in te richten van een virtuele machine of een web-app voor het gebruik van Azure Functions publiceren, maar in plaats daarvan kunt u deze code serverlessly uitvoeren. Azure Functions gebruiken voor het transformeren van de nettolading van de webhook voordat deze worden verzonden naar de uiteindelijke bestemming, zoals een SQL-database of Event Grid.

## <a name="prerequisites"></a>Vereisten

Als u nog geen abonnement op Azure hebt, maak dan een [gratis account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) aan voordat u begint.

## <a name="how-to-connect-azure-functions"></a>Verbinding maken tussen Azure Functions

1. [Een nieuwe functie-app maken in Azure Portal](https://ms.portal.azure.com/#create/Microsoft.FunctionApp).

    ![Een nieuwe functie-app maken in Azure Portal](media/howto-trigger-azure-functions/createfunction.png)

2. Vouw uw functie-app en klikt u op de **knop +** naast functies. Als dit de eerste functie in de functie-app is, selecteert u **Aangepaste functie**. U ziet nu de volledige set het functiesjablonen.

    ![Aangepaste functie in de functie-app kiezen](media/howto-trigger-azure-functions/customfunction.png)

3. Typ in het zoekveld **"generic"** en kies vervolgens de gewenste taal voor de algemene webhook-trigger-sjabloon. In dit onderwerp wordt gebruikgemaakt van een C#-functie. 

    ![Generieke webhook-trigger selecteren](media/howto-trigger-azure-functions/genericwebhooktrigger.png)

4. Klik in de nieuwe functie op **</> Functie-URL ophalen**, kopieer de waarde en sla deze op. U gebruikt deze waarde om de webhook te configureren.

    ![De URL van de functie ophalen](media/howto-trigger-azure-functions/getfunctionurl.png)

4. Navigeer in IoT Central, naar de regel die u wilt verbinding maken met uw functie-app.

5. Een webhookactie toevoegen. Voer een **weergavenaam** en plak deze in de functie-URL die u eerder hebt gekopieerd in **URL voor terugbellen**.

    ![De functie-URL invoeren in het veld van de callback-URL](media/howto-trigger-azure-functions/configurewebhook.PNG)

6. De regel niet opslaan. Wanneer de regel wordt geactiveerd, roept de webhook nu de functie-app om uit te voeren. In uw functie-app kunt u **Monitor** om te zien van de functie aanroepen geschiedenis. U kunt App Insights of de klassieke weergave gebruiken om te kijken naar de geschiedenis.

    ![De geschiedenis van de aanroep van de functie bewaken](media/howto-trigger-azure-functions/monitorfunction.PNG)

Voor meer informatie gaat u naar de Azure Functions-artikel over [het maken van een door een algemene webhook geactiveerde functie](https://docs.microsoft.com/azure/azure-functions/functions-create-generic-webhook-triggered-function).

## <a name="next-steps"></a>Volgende stappen
U hebt geleerd hoe u kunt instellen en gebruiken van webhooks, de voorgestelde volgende stap is om te verkennen [het bouwen van werkstromen in Microsoft Flow](howto-add-microsoft-flow.md).
