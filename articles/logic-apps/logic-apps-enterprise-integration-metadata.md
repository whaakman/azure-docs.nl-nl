---
title: Beheren van metagegevens van integratie-account artefacten - Azure Logic Apps | Microsoft Docs
description: Toevoegen of ophalen van metagegevens van artefacten van integratieaccounts in Azure Logic Apps met Enterprise Integration Pack
services: logic-apps
ms.service: logic-apps
ms.suite: integration
author: divyaswarnkar
ms.author: divswa
ms.reviewer: jonfan, estfan, LADocs
ms.topic: article
ms.assetid: bb7d9432-b697-44db-aa88-bd16ddfad23f
ms.date: 02/23/2018
ms.openlocfilehash: 537014c2780fe94cfb35806759f8bcbd974c4c95
ms.sourcegitcommit: 2ad510772e28f5eddd15ba265746c368356244ae
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/28/2018
ms.locfileid: "43128800"
---
# <a name="manage-artifact-metadata-from-integration-accounts-in-azure-logic-apps-with-enterprise-integration-pack"></a>Metagegevens van artefacten beheren van integratieaccounts in Azure Logic Apps met Enterprise Integration Pack

U kunt aangepaste metagegevens voor artefacten in integratieaccounts definiëren en ophalen van metagegevens tijdens runtime voor uw logische app. Bijvoorbeeld, kunt u metagegevens voor artefacten, zoals partners, overeenkomsten, schema's en kaarten - metagegevens voor alle opslaan met behulp van sleutel / waarde-paren. 

## <a name="add-metadata-to-artifacts-in-integration-accounts"></a>Metagegevens toevoegen aan artefacten in integratieaccounts

1. In de Azure-portal, maakt u een [integratieaccount](logic-apps-enterprise-integration-create-integration-account.md).

2. Voeg bijvoorbeeld een artefact toe aan uw integratie-account, een [partner](logic-apps-enterprise-integration-partners.md), [overeenkomst](logic-apps-enterprise-integration-agreements.md), of [schema](logic-apps-enterprise-integration-schemas.md).

3. Selecteer het artefact, kies **bewerken**, en voer de details van de metagegevens.

   ![Metagegevens invoeren](media/logic-apps-enterprise-integration-metadata/image1.png)

## <a name="retrieve-metadata-from-artifacts-for-logic-apps"></a>Ophalen van metagegevens van artefacten voor logische apps

1. In de Azure-portal, maakt u een [logische app](quickstart-create-first-logic-app-workflow.md).

2. Maak een [koppeling van uw logische app naar uw integratieaccount](logic-apps-enterprise-integration-create-integration-account.md#link-account). 

3. Voeg een trigger, zoals in Logic App Designer **aanvragen** of **HTTP** aan uw logische app.

4. Kies onder de trigger **nieuwe stap** > **een actie toevoegen**. Zoeken naar 'integratieaccount', zodat u kunt zoeken en vervolgens selecteert u deze actie: **Integratieaccount - zoeken naar een artefact-integratie**

   ![Zoeken naar een artefact integratie selecteren](media/logic-apps-enterprise-integration-metadata/image2.png)

5. Selecteer de **Artefacttype** en geef de **Artefactnaam**. Bijvoorbeeld:

   ![Artefacttype selecteren en geef de naam van het assemblyartefact](media/logic-apps-enterprise-integration-metadata/image3.png)

## <a name="example-retrieve-partner-metadata"></a>Voorbeeld: Ophalen partner metagegevens

Stel dat deze partner heeft deze metagegevens met `routingUrl` details:

![Partner "routingURL" metagegevens zoeken](media/logic-apps-enterprise-integration-metadata/image6.png)

1. Voeg de trigger in uw logische app, een **Integratieaccount - zoeken naar een artefact integratie** actie voor uw partner, en een **HTTP** actie, bijvoorbeeld:

   ![Trigger, zoeken naar een artefact en HTTP-actie toevoegen aan uw logische app](media/logic-apps-enterprise-integration-metadata/image4.png)

2. Als u wilt ophalen van de URI, op de werkbalk van de ontwerper van logische App, kies **codeweergave** voor uw logische app. De definitie van uw logische app moet eruitzien als in dit voorbeeld:

   ![Search opzoeken](media/logic-apps-enterprise-integration-metadata/image5.png)

## <a name="next-steps"></a>Volgende stappen

* [Meer informatie over de overeenkomsten](logic-apps-enterprise-integration-agreements.md)
