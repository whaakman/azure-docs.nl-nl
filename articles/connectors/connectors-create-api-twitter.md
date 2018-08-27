---
title: Verbinding maken met Twitter vanuit Azure Logic Apps | Microsoft Docs
description: Automatiseren van taken en werkstromen die bewaken en beheren van tweets, plus gegevens over Volgers, uw gevolgde gebruikers, andere gebruikers, tijdlijnen en meer uit uw Twitter-account met behulp van Azure Logic Apps ophalen
services: logic-apps
ms.service: logic-apps
ms.suite: integration
author: ecfan
ms.author: estfan
ms.reviewer: klam, LADocs
ms.assetid: 8bce2183-544d-4668-a2dc-9a62c152d9fa
ms.topic: article
tags: connectors
ms.date: 08/25/2018
ms.openlocfilehash: eea70d979a69a4855b6eeb892d1705ecadaa8434
ms.sourcegitcommit: ebb460ed4f1331feb56052ea84509c2d5e9bd65c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/24/2018
ms.locfileid: "42918642"
---
# <a name="monitor-and-manage-twitter-by-using-azure-logic-apps"></a>Controleren en beheren van Twitter met behulp van Azure Logic Apps

Met Azure Logic Apps en de Twitter-connector, kunt u geautomatiseerde taken en werkstromen die bewaken en beheren van gegevens die u geïnteresseerd in Twitter, zoals bent een tweet, volgers, gebruikers en gebruikers, tijdlijnen en meer, samen met andere acties, bijvoorbeeld gevolgd:

* Controleren, posten en tweets zoeken.
* Gegevens, zoals Volgers, gevolgde gebruikers en tijdlijnen ophalen.

U kunt triggers die te antwoorden krijgen van uw Twitter-account en de uitvoer beschikbaar voor andere acties. U kunt acties die taken met uw Twitter-account uitvoeren gebruiken. U kunt ook andere acties waarmee de uitvoer van de Twitter-acties hebben. Wanneer een nieuwe tweet met een specifieke hashtag wordt weergegeven, kunt u bijvoorbeeld berichten met de Slack-connector verzenden. Als u geen ervaring met logische apps, raadpleegt u [wat is Azure Logic Apps?](../logic-apps/logic-apps-overview.md)

## <a name="prerequisites"></a>Vereiste onderdelen

* Een Azure-abonnement. Als u nog geen abonnement op Azure hebt, <a href="https://azure.microsoft.com/free/" target="_blank">registreer u dan nu voor een gratis Azure-account</a>. 

* Uw Twitter-account en de gebruikersreferenties

   Uw referenties toestaan dat de logische app een verbinding maken en toegang tot uw Twitter-account.

* Basiskennis over [over het maken van logische apps](../logic-apps/quickstart-create-first-logic-app-workflow.md)

* De logische app waar u toegang tot uw Twitter-account. Om te beginnen met een Twitter-trigger, [maken van een lege, logische app](../logic-apps/quickstart-create-first-logic-app-workflow.md). Voor het gebruik van een Twitter-actie beginnen uw logische app met een andere trigger, bijvoorbeeld, de **terugkeerpatroon** trigger.

## <a name="connect-to-twitter"></a>Verbinding maken met Twitter

[!INCLUDE [Create connection general intro](../../includes/connectors-create-connection-general-intro.md)]

1. Aanmelden bij de [Azure-portal](https://portal.azure.com), en open uw logische app in Logic App Designer, als het niet al geopend.

1. Kies een pad: 

   * Typ 'twitter' als filter voor lege, logische apps, in het zoekvak. 
   Selecteer de gewenste trigger onder de lijst met triggers. 

     -of-

   * Voor bestaande logische apps: 
   
     * Kies onder de laatste stap waar u een actie toevoegen, **nieuwe stap**. 

       -of-

     * Tussen de stappen waar u een actie toevoegen, de aanwijzer over de pijl tussen fasen. 
     Kies het plusteken (**+**) die wordt weergegeven, en selecteer vervolgens **een actie toevoegen**.
     
       Typ 'twitter' als filter in het zoekvak. 
       Selecteer de actie die u wilt onder de lijst met acties.

1. Als u wordt gevraagd of u aanmelden bij Twitter, meld u nu, zodat u toegang voor uw logische app verlenen kunt.

1. Geef de benodigde informatie voor uw geselecteerde trigger of actie en doorgaan met het ontwikkelen van uw logische app-werkstroom.

## <a name="examples"></a>Voorbeelden

### <a name="twitter-trigger-when-a-new-tweet-is-posted"></a>Twitter-trigger: wanneer een nieuwe tweet wordt geplaatst

Deze trigger start een werkstroom voor logische Apps wanneer de trigger wordt een nieuwe tweet bijvoorbeeld gedetecteerd met de hashtag, #Seattle. Wanneer deze tweets worden gevonden, kunt u bijvoorbeeld een bestand met de tweets inhoud toevoegen aan opslag, zoals een Dropbox-account met behulp van de Dropbox-connector. 

U kunt eventueel een voorwaarde die in aanmerking komende tweets afkomstig van gebruikers met ten minste een opgegeven aantal Volgers zijn moeten opnemen.

**Voorbeeld van de onderneming**: U kunt deze trigger gebruiken om te controleren van tweets over uw bedrijf en de tweets inhoud uploaden naar een SQL-database.

### <a name="twitter-action-post-a-tweet"></a>Twitter-actie: een tweet plaatsen

Deze actie een tweet geplaatst, maar u kunt de actie instellen zodat de tweet de inhoud van de tweets die door de eerder beschreven trigger is gevonden bevat. 

## <a name="connector-reference"></a>Connector-verwijzing

Voor technische informatie over triggers en acties limieten die worden beschreven van de connector openapi (voorheen Swagger) beschrijving van de connector controleren [-verwijzingspagina](/connectors/twitterconnector/).

## <a name="get-support"></a>Ondersteuning vragen

* Ga naar het [Azure Logic Apps forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurelogicapps) (Forum voor Azure Logic Apps) als u vragen hebt.
* Als u ideeën voor functies wilt indienen of erop wilt stemmen, gaat u naar de [website voor feedback van Logic Apps-gebruikers](http://aka.ms/logicapps-wish).

## <a name="next-steps"></a>Volgende stappen

* Meer informatie over andere [Logic Apps-connectors](../connectors/apis-list.md)
