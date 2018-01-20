---
title: Integratie account artefact metagegevens - Azure Logic Apps beheren | Microsoft Docs
description: Toevoegen of artefact metagegevens ophalen uit integratieaccounts voor Azure Logic Apps
author: padmavc
manager: anneta
editor: 
services: logic-apps
documentationcenter: 
ms.assetid: bb7d9432-b697-44db-aa88-bd16ddfad23f
ms.service: logic-apps
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.custom: H1Hack27Feb2017
ms.date: 11/21/2016
ms.author: LADocs; padmavc
ms.openlocfilehash: 5eebae624ea024f2ff8c1fa4764027c05220a15f
ms.sourcegitcommit: be9a42d7b321304d9a33786ed8e2b9b972a5977e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/19/2018
---
# <a name="manage-artifact-metadata-in-integration-accounts-for-logic-apps"></a>De metagegevens van de artefacten in integratieaccounts voor logic apps beheren

U kunt aangepaste metagegevens voor artefacten in integratieaccounts definiëren en ophalen van metagegevens tijdens de runtime voor uw logische app. Bijvoorbeeld, kunt u metagegevens voor artefacten zoals partners, overeenkomsten, schema's en maps - alle sleutelwaarde-paren met metagegevens opslaan. Op dit moment artefacten metagegevens via de gebruikersinterface kunnen niet worden gemaakt, maar u kunt REST-API's gebruiken voor het maken van metagegevens. Om toe te voegen metagegevens wanneer u maakt of selecteert u een partner, een overeenkomst of een schema in de Azure-portal, kies **bewerken**. Voor het ophalen van metagegevens van het artefact in logic apps, kunt u de functie Integratie Account artefact opzoeken.

## <a name="add-metadata-to-artifacts-in-integration-accounts"></a>Metagegevens toevoegen aan artefacten in integratieaccounts

1. Maak een [integratie account](logic-apps-enterprise-integration-create-integration-account.md).

2. Voeg bijvoorbeeld een artefact toe aan uw account integratie, een [partner](logic-apps-enterprise-integration-partners.md#how-to-create-a-partner), [overeenkomst](logic-apps-enterprise-integration-agreements.md#how-to-create-agreements), of [schema](logic-apps-enterprise-integration-schemas.md).

3.  Selecteer het artefact, kies **bewerken**, en voer de details van de metagegevens.

    ![Voer de metagegevens](media/logic-apps-enterprise-integration-metadata/image1.png)

## <a name="retrieve-metadata-from-artifacts-for-logic-apps"></a>Ophalen van metagegevens van artefacten voor logic apps

1. Maak een [logische app](quickstart-create-first-logic-app-workflow.md).

2. Maak een [koppeling van uw logische app naar uw account integratie](logic-apps-enterprise-integration-create-integration-account.md#link-an-integration-account-to-a-logic-app). 

3. In Logic App-ontwerper, voegt u een trigger zoals *aanvragen* of *HTTP* aan uw logische app.

4.  Kies **doornemen** > **een actie toevoegen**. Zoeken naar *integratie* zodat u kunt zoeken naar en selecteer vervolgens **integratie Account - integratie Account artefact Lookup**.

    ![Selecteer integratie Account artefact Lookup](media/logic-apps-enterprise-integration-metadata/image2.png)

5. Selecteer de **artefact Type**, en geef de **artefact naam**.

    ![Selecteer artefact type en geef de naam van het artefact](media/logic-apps-enterprise-integration-metadata/image3.png)

## <a name="example-retrieve-partner-metadata"></a>Voorbeeld: Ophalen partner metagegevens

Partner metagegevens bevat deze `routingUrl` details:

![Partner 'routingURL' metagegevens vinden](media/logic-apps-enterprise-integration-metadata/image6.png)

1. Voeg de trigger in uw logische app, een **integratie Account - integratie Account artefact Lookup** actie voor uw partner en een **HTTP**.

    ![Trigger, artefacten opzoeken en 'HTTP' toevoegen aan uw logische app](media/logic-apps-enterprise-integration-metadata/image4.png)

2. Voor het ophalen van de URI, gaat u naar de codeweergave voor uw logische app. De definitie van de logische app moet eruitzien als in dit voorbeeld:

    ![Zoeken zoeken](media/logic-apps-enterprise-integration-metadata/image5.png)


## <a name="next-steps"></a>Volgende stappen
* [Meer informatie over de overeenkomsten](logic-apps-enterprise-integration-agreements.md "meer informatie over enterprise integration-overeenkomsten")  
