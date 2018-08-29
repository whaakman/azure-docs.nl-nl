---
title: XML-validatie voor B2B-bedrijfsintegratie - Azure Logic Apps | Microsoft Docs
description: XML valideren met schema's voor B2B-oplossingen in Azure Logic Apps met Enterprise Integration Pack
services: logic-apps
ms.service: logic-apps
ms.suite: integration
author: divyaswarnkar
ms.author: divswa
ms.reviewer: jonfan, estfan, LADocs
ms.topic: article
ms.assetid: d700588f-2d8a-4c92-93eb-e1e6e250e760
ms.date: 07/08/2016
ms.openlocfilehash: e25f4e633b8cf9030bb0ce395f093f630db59f38
ms.sourcegitcommit: 2ad510772e28f5eddd15ba265746c368356244ae
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/28/2018
ms.locfileid: "43121110"
---
# <a name="validate-xml-for-b2b-enterprise-integration-in-azure-logic-apps-with-enterprise-integration-pack"></a>XML-validatie voor B2B-bedrijfsintegratie in Azure Logic Apps met Enterprise Integration Pack

Vaak in B2B-scenario's moeten de partners in een overeenkomst ervoor zorgen dat de wisselen ze berichten geldig zijn voordat de verwerking van gegevens kunt beginnen. U kunt documenten op basis van een vooraf gedefinieerd schema valideren met behulp van het gebruik van de XML-validatie-connector in de Enterprise Integration Pack.

## <a name="validate-a-document-with-the-xml-validation-connector"></a>Een document met de XML-validatie-connector valideren

1. Een logische app maken en [de app een koppeling naar het integratieaccount](../logic-apps/logic-apps-enterprise-integration-accounts.md "Leer hoe u een integratieaccount koppelt aan een logische app") waarvoor het schema dat u gebruiken wilt voor het valideren van XML-gegevens.

2. Voeg een **aanvraag: wanneer een HTTP-aanvraag wordt ontvangen** trigger aan uw logische app.

    ![](./media/logic-apps-enterprise-integration-xml-validation/xml-1.png)

3. Om toe te voegen de **XML-validatie** actie, kiest u **een actie toevoegen**.

4. Als u wilt alle acties in de map die u wilt filteren, voer *xml* in het zoekvak in. Kies **XML-validatie**.

    ![](./media/logic-apps-enterprise-integration-xml-validation/xml-2.png)

5. Als u de XML-inhoud die u wilt valideren, schakelt u **inhoud**.

    ![](./media/logic-apps-enterprise-integration-xml-validation/xml-1-5.png)

6. Selecteer de body >-tag als de inhoud die u wilt valideren.

    ![](./media/logic-apps-enterprise-integration-xml-validation/xml-3.png)

7. Om op te geven van het schema dat u gebruiken wilt voor het valideren van de vorige *inhoud* invoer, kiest u **SCHEMANAAM**.

    ![](./media/logic-apps-enterprise-integration-xml-validation/xml-4.png)

8. Sla uw werk  

    ![](./media/logic-apps-enterprise-integration-xml-validation/xml-5.png)

U bent nu klaar met het instellen van uw connector validatie. In een werkelijke toepassing, is het raadzaam om op te slaan van de gevalideerde gegevens in een line-of-business (LOB)-app, zoals SalesForce. Voor het verzenden van de gevalideerde uitvoer met Salesforce, moet u een actie toevoegen.

Als u wilt uw actie validatie testen, moet u een aanvraag indienen voor de HTTP-eindpunt.

## <a name="next-steps"></a>Volgende stappen
[Meer informatie over het Enterprise Integration Pack](../logic-apps/logic-apps-enterprise-integration-overview.md "meer informatie over Enterprise Integration Pack")   

