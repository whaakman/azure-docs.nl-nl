---
title: Beheren van metagegevens van integratie-account artefacten - Azure Logic Apps | Microsoft Docs
description: Toevoegen of artefact metagegevens ophalen van integratieaccounts in Azure Logic Apps met Enterprise Integration Pack
services: logic-apps
ms.service: logic-apps
ms.suite: integration
author: divyaswarnkar
ms.author: divswa
ms.reviewer: jonfan, estfan, LADocs
ms.topic: article
ms.assetid: bb7d9432-b697-44db-aa88-bd16ddfad23f
ms.date: 01/17/2019
ms.openlocfilehash: 2a5066a2dcb74051088b2fc67b5089e95ae4dd3f
ms.sourcegitcommit: 82cdc26615829df3c57ee230d99eecfa1c4ba459
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/19/2019
ms.locfileid: "54410350"
---
# <a name="manage-artifact-metadata-in-integration-accounts-with-azure-logic-apps-and-enterprise-integration-pack"></a>Metagegevens van artefacten in integratieaccounts met Azure Logic Apps en Enterprise Integration Pack beheren

U kunt aangepaste metagegevens voor artefacten in integratieaccounts definiëren en deze metagegevens ophalen tijdens runtime voor uw logische app te gebruiken. Bijvoorbeeld, kunt u opgeven metagegevens voor artefacten, zoals partners, overeenkomsten, schema's en kaarten - metagegevens voor alle opslaan met behulp van sleutel / waarde-paren. 

## <a name="prerequisites"></a>Vereisten

* Een Azure-abonnement. Als u nog geen abonnement hebt, <a href="https://azure.microsoft.com/free/" target="_blank">meld u dan aan voor een gratis Azure-account</a>.

* Een eenvoudige [integratieaccount](../logic-apps/logic-apps-enterprise-integration-create-integration-account.md) waarvoor de artefacten waar u wilt toevoegen van metagegevens, bijvoorbeeld: 

  * [Partner](logic-apps-enterprise-integration-partners.md)
  * [Agreement](logic-apps-enterprise-integration-agreements.md)
  * [schema](logic-apps-enterprise-integration-schemas.md)
  * [Kaart](logic-apps-enterprise-integration-maps.md)

* Een logische app die gekoppeld aan de metagegevens van de integratie-account en artefact dat u wilt gebruiken. Als uw logische app niet is al gekoppeld, krijgt u informatie [logische apps koppelen aan integratieaccounts](logic-apps-enterprise-integration-create-integration-account.md#link-account). 

  Als u een logische app nog niet hebt, krijgt u informatie [over het maken van logische apps](../logic-apps/quickstart-create-first-logic-app-workflow.md). 
  Voeg de trigger en acties die u gebruiken wilt voor het beheren van metagegevens van artefacten. Of, als u wilt alleen dingen uitproberen, zoals een trigger toevoegen **aanvragen** of **HTTP** aan uw logische app.

## <a name="add-metadata-to-artifacts"></a>Metagegevens voor artefacten toevoegen

1. Gebruik de referenties van uw Azure-account om u aan melden bij het <a href="https://portal.azure.com" target="_blank">Azure Portal</a>. Zoek en open uw integratieaccount.

1. Selecteer het artefact waar u wilt toevoegen, metagegevens en kies **bewerken**. Voer de details van de metagegevens voor dit artefact, bijvoorbeeld:

   ![Metagegevens invoeren](media/logic-apps-enterprise-integration-metadata/add-partner-metadata.png)

1. Als u klaar bent, kiest u **Done**.

1. U kunt deze metagegevens bekijken in de definitie van JavaScript Object Notation (JSON) voor het integratieaccount **bewerken as JSON** zodat die JSON-editor wordt geopend: 

   ![JSON voor de metagegevens van de partner](media/logic-apps-enterprise-integration-metadata/partner-metadata.png)

## <a name="get-artifact-metadata"></a>Ophalen van metagegevens van artefacten

1. Open de logische app die gekoppeld aan de integratieaccount dat u wilt dat in de Azure-portal. 

1. In de Logic App Designer, als u de stap voor het ophalen van metagegevens in de laatste actie of trigger in de werkstroom toevoegt, kiest u **nieuwe stap** > **een actie toevoegen**. 

1. Voer in het zoekvak ' integratieaccount'. Kies **Alle** onder het zoekvak. Selecteer deze actie uit de lijst met acties: **Integratie van zoeken naar een artefact - integratie-Account**

   ![Selecteer "Integratie zoeken naar een artefact"](media/logic-apps-enterprise-integration-metadata/integration-account-artifact-lookup.png)

1. Geef deze informatie voor het artefact die u wilt zoeken:

   | Eigenschap | Vereist | Waarde | Description | 
   |----------|---------|-------|-------------| 
   | **Artefacttype** | Ja | **Schema**, **kaart**, **Partner**, **overeenkomst**, of een aangepast type | Het type voor het artefact dat u wilt | 
   | **Naam van het assemblyartefact** | Ja | <*naam van het assemblyartefact*> | De naam van het artefact dat u wilt | 
   ||| 

   Stel bijvoorbeeld dat u wilt ophalen van de metagegevens voor een trading partner artefact:

   ![Artefacttype selecteert en voer de naam van het assemblyartefact](media/logic-apps-enterprise-integration-metadata/artifact-lookup-information.png)

1. Voeg de actie die u wilt gebruiken voor het verwerken van metagegevens, bijvoorbeeld:

   1. Onder de **zoeken naar een artefact integratie** actie, kiest u **volgende stap**, en selecteer **een actie toevoegen**. 

   1. Voer in het zoekvak 'http'. Kies onder het zoekvak **dient te worden**, en selecteer deze actie: **HTTP - HTTP**

      ![HTTP-actie toevoegen](media/logic-apps-enterprise-integration-metadata/http-action.png)

   1. Geef informatie voor de metagegevens van artefacten die u wilt beheren. 

      Stel bijvoorbeeld dat u wilt de `routingUrl` metagegevens die eerder in dit onderwerp wordt toegevoegd. Hier volgen de eigenschapswaarden die u kunt opgeven: 

      | Eigenschap | Vereist | Waarde | Description | 
      |----------|----------|-------|-------------| 
      | **Methode** | Ja | <*operation-to-run*> | De HTTP-bewerking uit te voeren op het artefact. Deze actie HTTP gebruikt bijvoorbeeld de **ophalen** methode. | 
      | **URI** | Ja | <*metadata-location*> | Toegang krijgen tot de `routingUrl` metagegevens waarde van het artefact die u hebt opgehaald, kunt u een expressie, bijvoorbeeld: <p>`@{outputs('Integration_Account_Artifact_Lookup')['properties']['metadata']['routingUrl']}` | 
      | **Headers** | Nee | <*header-values*> | Een koptekst wordt de uitvoer van de trigger die u wilt doorgeven aan de HTTP-actie. Bijvoorbeeld, om door te geven van de trigger `headers` eigenschapswaarde: u kunt een expressie, bijvoorbeeld: <p>`@triggeroutputs()['headers']` | 
      | **Hoofdtekst** | Nee | <*body-content*> | Alle andere inhoud die u wilt doorgeven aan de HTTP-actie `body` eigenschap. In dit voorbeeld geeft van het artefact `properties` waarden in de HTTP-actie: <p>1. Klik in de **hoofdtekst** eigenschap, zodat de lijst met dynamische inhoud wordt weergegeven. Als er geen eigenschappen wordt weergegeven, kiest u **meer**. <br>2. Uit de lijst met dynamische inhoud, onder **zoeken naar een artefact integratie**, selecteer **eigenschappen**. | 
      |||| 

      Bijvoorbeeld:

      ![Geef waarden en expressies voor HTTP-actie](media/logic-apps-enterprise-integration-metadata/add-http-action-values.png)

   1. Om te controleren of de informatie die u hebt opgegeven voor de HTTP-actie, JSON-definitie van uw logische app weergeven. Kies op de werkbalk Logic App Designer **codeweergave** , zodat de JSON-definitie van de app wordt weergegeven, bijvoorbeeld:

      ![JSON-definitie van logische app](media/logic-apps-enterprise-integration-metadata/finished-logic-app-definition.png)

      Nadat u naar de ontwerper van logische App overschakelt, eventuele expressies die u hebt gebruikt, worden weergegeven omgezet, bijvoorbeeld:

      ![Opgelost expressies in Logic App Designer](media/logic-apps-enterprise-integration-metadata/resolved-expressions.png)

## <a name="next-steps"></a>Volgende stappen

* [Meer informatie over de overeenkomsten](logic-apps-enterprise-integration-agreements.md)
