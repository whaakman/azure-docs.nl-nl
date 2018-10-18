---
title: Azure Media Services-gebeurtenissen met Event Grid met behulp van portal controleren | Microsoft Docs
description: Dit artikel leest hoe u zich kunt abonneren op Event Grid om te controleren van Azure Media Services-gebeurtenissen.
services: media-services
documentationcenter: na
author: Juliako
manager: femila
editor: ''
tags: ''
keywords: azure media services, stream, uitzenden, live, offline
ms.service: media-services
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: multiple
ms.workload: media
ms.date: 10/15/2018
ms.author: juliako
ms.openlocfilehash: 4e6527bf115f327635a0b0fe187094dafb320598
ms.sourcegitcommit: 3a7c1688d1f64ff7f1e68ec4bb799ba8a29a04a8
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/17/2018
ms.locfileid: "49381040"
---
# <a name="create-and-monitor-media-services-events-with-event-grid-using-the-azure-portal"></a>Maken en bewaken van Media Services-gebeurtenissen met Event Grid met Azure portal

Azure Event Grid is een gebeurtenisservice voor de cloud. In dit artikel gebruikt u de Azure-portal om u te abonneren op gebeurtenissen voor Azure Media Services-account. Vervolgens kunt activeren u gebeurtenissen om het resultaat weer te. Normaal gesproken verzendt u gebeurtenissen naar een eindpunt dat de gebeurtenisgegevens verwerkt en vervolgens in actie komt. In dit artikel hebben verzenden we gebeurtenissen naar een web-app die worden verzameld en worden de berichten weergegeven.

Wanneer u klaar bent, ziet u dat de gebeurtenisgegevens naar de web-app zijn verzonden.

## <a name="prerequisites"></a>Vereisten 

* Een actief Azure-abonnement hebben.
* Maak een nieuw Media Services-account zoals beschreven in deze [snelstart](create-account-cli-quickstart.md).

## <a name="create-a-message-endpoint"></a>Het eindpunt van een bericht maken

Voordat u zich abonneert op de gebeurtenissen voor het Media Services-account, moet u gaan we het eindpunt voor het gebeurtenisbericht maken. Het eindpunt onderneemt normaal gesproken actie op basis van de gebeurtenisgegevens. In dit artikel, implementeert u een [vooraf gemaakte web-app](https://github.com/Azure-Samples/azure-event-grid-viewer) waarmee de gebeurtenisberichten worden weergegeven. De geïmplementeerde oplossing omvat een App Service-plan, een App Service-web-app en broncode van GitHub.

1. Selecteer **Implementeren in Azure** om de oplossing voor uw abonnement te implementeren. Geef in Azure Portal waarden op voor de parameters.

   <a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure-Samples%2Fazure-event-grid-viewer%2Fmaster%2Fazuredeploy.json" target="_blank"><img src="http://azuredeploy.net/deploybutton.png"/></a>

1. De implementatie kan enkele minuten duren. Controleer of uw web-app wordt uitgevoerd nadat de implementatie is voltooid. Navigeer in een webbrowser naar: `https://<your-site-name>.azurewebsites.net`

Als u overschakelt naar de site "Azure Event Grid-Viewer", ziet u dat nog geen gebeurtenissen heeft.
   
[!INCLUDE [event-grid-register-provider-portal.md](../../../includes/event-grid-register-provider-portal.md)]

## <a name="subscribe-to-media-services-events"></a>Abonneren op gebeurtenissen voor Media Services

U abonneert u op een onderwerp om Event Grid te laten weten welke gebeurtenissen u wilt traceren en waar de gebeurtenissen naartoe moeten worden gestuurd.

1. In de portal, selecteert u uw Media Services-account en selecteer **gebeurtenissen**.
1. Gebruik een webhook voor het eindpunt als u gebeurtenissen naar uw viewer-app wilt verzenden. 

   ![Webhook selecteren](./media/monitor-events-portal/select-web-hook.png)

1. Het gebeurtenisabonnement wordt vooraf ingevuld met de waarden voor uw Media Services-account. 
1. Selecteer 'Webhook' voor de **Eindpunttype**.
1. In dit onderwerp laten we de **abonneren op alle gebeurtenistypen** gecontroleerd. U kunt echter het en filteren op specifieke gebeurtenistypen uitschakelen. 
1. Klik op de **selecteert u een eindpunt** koppeling.

    Voor het webhookeindpunt geeft u de URL van uw web-app op en voegt u `api/updates` toe aan de URL van de startpagina. 

1. Druk op **Bevestig de selectie van**.
1. Kies **Maken**.
1. Geef het abonnement een beschrijvende naam.

   ![Logboeken selecteren](./media/monitor-events-portal/create-subscription.png)

1. Bekijk opnieuw uw web-app en u zult zien dat er een validatiegebeurtenis voor een abonnement naartoe is verzonden. 

    Via Event Grid wordt de validatiegebeurtenis verzonden zodat het eindpunt kan controleren of de gebeurtenisgegevens in aanmerking komen om ontvangen te worden. Het eindpunt is om in te stellen `validationResponse` naar `validationCode`. Zie voor meer informatie, [Event Grid-beveiliging en verificatie](../../event-grid/security-authentication.md). U kunt de code van de web-app om te zien hoe het abonnement gevalideerd weergeven.

Nu gaan we trigger gebeurtenissen om te zien hoe het bericht aan uw eindpunt wordt gedistribueerd in Event Grid.

## <a name="send-an-event-to-your-endpoint"></a>Een gebeurtenis verzenden naar het eindpunt

U kunt gebeurtenissen voor het Media Services-account door het uitvoeren van een coderingstaak activeren. U kunt volgen [in deze snelstartgids](stream-files-dotnet-quickstart.md) voor het coderen van een bestand en beginnen met het verzenden van gebeurtenissen. Als u geabonneerd op alle gebeurtenissen, ziet u een scherm dat vergelijkbaar is met het volgende:

> [!TIP]
> Selecteer het oogpictogram om de gebeurtenisgegevens uit te breiden. Vernieuw de pagina, niet als u wilt weergeven van alle gebeurtenissen.

![Een abonnementgebeurtenis weergeven](./media/monitor-events-portal/view-subscription-event.png)

## <a name="next-steps"></a>Volgende stappen

[Uploaden, coderen en streamen](stream-files-tutorial-with-api.md)
