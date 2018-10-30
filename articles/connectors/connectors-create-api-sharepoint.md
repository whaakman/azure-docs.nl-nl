---
title: Verbinding maken met SharePoint vanuit Azure Logic Apps | Microsoft Docs
description: Automatiseren van taken en werkstromen die bewaken en beheren van resources in SharePoint Online of SharePoint Server on-premises met behulp van Azure Logic Apps
services: logic-apps
ms.service: logic-apps
ms.suite: integration
author: ecfan
ms.author: estfan
ms.reviewer: klam, LADocs
ms.assetid: e0ec3149-507a-409d-8e7b-d5fbded006ce
ms.topic: article
tags: connectors
ms.date: 08/25/2018
ms.openlocfilehash: f076b2e859d334b407fe5c93fcec1bb67cb75e48
ms.sourcegitcommit: fbdfcac863385daa0c4377b92995ab547c51dd4f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/30/2018
ms.locfileid: "50233256"
---
# <a name="monitor-and-manage-sharepoint-resources-with-azure-logic-apps"></a>Controleren en beheren van SharePoint-resources met Azure Logic Apps

Met Azure Logic Apps en de SharePoint-connector, kunt u geautomatiseerde taken en werkstromen die bewaken en beheren van resources, zoals bestanden, mappen, lijsten, artikelen, personen, enzovoort, in SharePoint Online of SharePoint Server on-premises, bijvoorbeeld:

* Monitor als de bestanden of items zijn gemaakt, gewijzigd of verwijderd.
* Maken, ophalen, bijwerken of verwijderen van items.
* Toevoegen, verkrijgen of verwijderen van bijlagen. De inhoud ophalen van bijlagen.
* Maken, kopieert, bijwerken of verwijderen van bestanden. 
* Bestandseigenschappen bijwerken. Haal de inhoud, metagegevens of eigenschappen voor een bestand.
* Lijst of ophalen van mappen.
* Get-lijsten of lijstweergaven.
* Stel de status van de goedkeuring van inhoud.
* Personen die worden omgezet.
* HTTP-aanvragen verzenden naar SharePoint.
* Entiteitswaarden ophalen.

U kunt triggers die te antwoorden krijgen van SharePoint en de uitvoer beschikbaar voor andere acties. U kunt acties in uw logische apps gebruiken voor het uitvoeren van taken in SharePoint. U kunt ook andere acties waarmee de uitvoer van de SharePoint-acties hebben. Bijvoorbeeld, als u regelmatig bestanden uit SharePoint ophalen, kunt u berichten verzenden naar uw team met behulp van de Slack-connector.
Als u geen ervaring met logische apps, raadpleegt u [wat is Azure Logic Apps?](../logic-apps/logic-apps-overview.md)

## <a name="prerequisites"></a>Vereisten

* Een Azure-abonnement. Als u nog geen abonnement op Azure hebt, <a href="https://azure.microsoft.com/free/" target="_blank">registreer u dan nu voor een gratis Azure-account</a>. 

* Uw SharePoint-site-adres en de gebruikersreferenties

  Uw referenties toestaan dat de logische app een verbinding maken en toegang tot uw SharePoint-account. 

* Voordat u logische apps met on-premises systemen, zoals SharePoint-Server verbinden kunt, moet u [installeren en instellen van een on-premises gegevensgateway](../logic-apps/logic-apps-gateway-install.md). Op die manier kunt u opgeven voor het gebruik van de gatewayinstallatie, wanneer u de SharePoint-Server verbinding voor uw logische app maakt.

* Basiskennis over [over het maken van logische apps](../logic-apps/quickstart-create-first-logic-app-workflow.md)

* De logische app waar u toegang tot uw SharePoint-account. Om te beginnen met een SharePoint-trigger, [maken van een lege, logische app](../logic-apps/quickstart-create-first-logic-app-workflow.md). Voor het gebruik van een SharePoint-actie, start u uw logische app met een trigger, zoals een Salesforce-trigger, hebt u een Salesforce-account.

  Bijvoorbeeld, kun u uw logische app met de **wanneer een record wordt gemaakt** Salesforce-trigger. 
  Deze trigger wordt elke keer dat een nieuwe record, zoals een lead in Salesforce wordt gemaakt. 
  U kunt deze trigger met de SharePoint volgen **bestand maken** actie. Op die manier, wanneer de nieuwe record wordt gemaakt, uw logische app maakt een bestand in SharePoint met informatie over deze nieuwe record.

## <a name="connect-to-sharepoint"></a>Verbinding maken met SharePoint

[!INCLUDE [Create connection general intro](../../includes/connectors-create-connection-general-intro.md)]

1. Aanmelden bij de [Azure-portal](https://portal.azure.com), en open uw logische app in Logic App Designer, als het niet al geopend.

1. Typ 'sharepoint' als filter voor lege, logische apps, in het zoekvak. Selecteer de gewenste trigger onder de lijst met triggers. 

   -of-

   Voor bestaande logische apps, onder de laatste stap waarin u wilt toevoegen van een SharePoint-actie, kies **nieuwe stap**. 
   Typ 'sharepoint' als filter in het zoekvak. 
   Selecteer de actie die u wilt onder de lijst met acties.

   Als u wilt toevoegen een actie tussen fasen, de aanwijzer over de pijl tussen fasen. 
   Kies het plusteken (**+**) die wordt weergegeven, en selecteer vervolgens **een actie toevoegen**.

1. Wanneer u wordt gevraagd of u zich aanmeldt, bevatten de noodzakelijke verbindingsgegevens. Als u SharePoint Server, zorg ervoor dat u selecteert **verbinding maken via on-premises gegevensgateway**. Wanneer u klaar bent, kiest u **Maken**.

1. Geef de benodigde informatie voor uw geselecteerde trigger of actie en doorgaan met het ontwikkelen van uw logische app-werkstroom.

## <a name="connector-reference"></a>Connector-verwijzing

Voor technische informatie over triggers en acties limieten die worden beschreven van de connector openapi (voorheen Swagger) beschrijving van de connector controleren [-verwijzingspagina](/connectors/sharepoint/).

## <a name="get-support"></a>Ondersteuning krijgen

* Ga naar het [Azure Logic Apps forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurelogicapps) (Forum voor Azure Logic Apps) als u vragen hebt.
* Als u ideeën voor functies wilt indienen of erop wilt stemmen, gaat u naar de [website voor feedback van Logic Apps-gebruikers](https://aka.ms/logicapps-wish).

## <a name="next-steps"></a>Volgende stappen

* Meer informatie over andere [Logic Apps-connectors](../connectors/apis-list.md)
