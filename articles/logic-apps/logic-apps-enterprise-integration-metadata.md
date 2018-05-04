---
title: Integratie account artefact metagegevens - Azure Logic Apps beheren | Microsoft Docs
description: Toevoegen of artefact metagegevens ophalen uit integratieaccounts voor Azure Logic Apps
author: padmavc
manager: anneta
editor: ''
services: logic-apps
documentationcenter: ''
ms.assetid: bb7d9432-b697-44db-aa88-bd16ddfad23f
ms.service: logic-apps
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.custom: H1Hack27Feb2017
ms.date: 02/23/2018
ms.author: LADocs; padmavc
ms.openlocfilehash: 8ebca60c21366cddadccfd0456880696457d7777
ms.sourcegitcommit: 6e43006c88d5e1b9461e65a73b8888340077e8a2
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/01/2018
---
# <a name="manage-artifact-metadata-in-integration-accounts-for-logic-apps"></a>De metagegevens van de artefacten in integratieaccounts voor logic apps beheren

U kunt aangepaste metagegevens voor artefacten in integratieaccounts definiëren en ophalen van metagegevens tijdens de runtime voor uw logische app. Bijvoorbeeld, kunt u metagegevens voor artefacten, zoals partners, overeenkomsten, schema's en maps - alle store metagegevens met sleutel-waardeparen. 

## <a name="add-metadata-to-artifacts-in-integration-accounts"></a>Metagegevens toevoegen aan artefacten in integratieaccounts

1. Maak in de Azure-portal een [integratie account](logic-apps-enterprise-integration-create-integration-account.md).

2. Voeg bijvoorbeeld een artefact toe aan uw account integratie, een [partner](logic-apps-enterprise-integration-partners.md), [overeenkomst](logic-apps-enterprise-integration-agreements.md), of [schema](logic-apps-enterprise-integration-schemas.md).

3. Selecteer het artefact, kies **bewerken**, en voer de details van de metagegevens.

   ![Voer de metagegevens](media/logic-apps-enterprise-integration-metadata/image1.png)

## <a name="retrieve-metadata-from-artifacts-for-logic-apps"></a>Ophalen van metagegevens van artefacten voor logic apps

1. Maak in de Azure-portal een [logische app](quickstart-create-first-logic-app-workflow.md).

2. Maak een [koppeling van uw logische app naar uw account integratie](logic-apps-enterprise-integration-create-integration-account.md#link-account). 

3. In Logic App-ontwerper, voegt u een trigger zoals **aanvragen** of **HTTP** aan uw logische app.

4. Kies onder de trigger **nieuwe stap** > **een actie toevoegen**. Zoek naar 'integratie account', zodat u kunt zoeken naar en selecteer vervolgens deze actie: **integratie Account - integratie Account artefact Lookup**

   ![Selecteer integratie Account artefact Lookup](media/logic-apps-enterprise-integration-metadata/image2.png)

5. Selecteer de **artefact Type** en geef de **artefact naam**. Bijvoorbeeld:

   ![Selecteer artefact type en geef de naam van het artefact](media/logic-apps-enterprise-integration-metadata/image3.png)

## <a name="example-retrieve-partner-metadata"></a>Voorbeeld: Ophalen partner metagegevens

Stel dat deze partner heeft deze metagegevens met `routingUrl` details:

![Partner 'routingURL' metagegevens vinden](media/logic-apps-enterprise-integration-metadata/image6.png)

1. Voeg de trigger in uw logische app, een **integratie Account - integratie Account artefact Lookup** actie voor uw partner en een **HTTP** actie, bijvoorbeeld:

   ![Trigger, artefacten opzoeken en HTTP-actie toevoegen aan uw logische app](media/logic-apps-enterprise-integration-metadata/image4.png)

2. Kies voor het ophalen van de URI, op de werkbalk Logic App-ontwerper **codeweergave** voor uw logische app. De definitie van de logische app moet eruitzien als in dit voorbeeld:

   ![Zoeken zoeken](media/logic-apps-enterprise-integration-metadata/image5.png)

## <a name="next-steps"></a>Volgende stappen

* [Meer informatie over de overeenkomsten](logic-apps-enterprise-integration-agreements.md)
