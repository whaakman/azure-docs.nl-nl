---
title: Valideren van XML is - Azure Logic Apps | Microsoft Docs
description: XML-validatie met schema's voor Azure Logic Apps en B2B-scenario's met behulp van het Enterprise-integratiepakket
services: logic-apps
documentationcenter: .net,nodejs,java
author: msftman
manager: anneta
editor: cgronlun
ms.assetid: d700588f-2d8a-4c92-93eb-e1e6e250e760
ms.service: logic-apps
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/08/2016
ms.author: LADocs; padmavc
ms.openlocfilehash: 8558efffa354cc4bb93820c837077ee997924c95
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2017
---
# <a name="validate-xml-for-enterprise-integration"></a>XML-validatie voor enterprise-integratie

Vaak in B2B-scenario's, moeten de partners in een overeenkomst ervoor zorgen dat de wisselen ze berichten geldig zijn voordat u kunt beginnen met het verwerken van gegevens. U kunt documenten tegen een vooraf gedefinieerd schema valideren met behulp van het gebruik van de XML-validatie-connector in de Enterprise-integratiepakket.

## <a name="validate-a-document-with-the-xml-validation-connector"></a>Een document met de XML-validatie-connector valideren

1. Een logische app maken en [de app koppelen aan het account integratie](../logic-apps/logic-apps-enterprise-integration-accounts.md "informatie over het koppelen van een integratie-account aan een logische app") is met het schema dat u gebruiken wilt voor het valideren van XML-gegevens.

2. Voeg een **aanvraag: wanneer een HTTP-aanvraag wordt ontvangen** trigger aan uw logische app.

    ![](./media/logic-apps-enterprise-integration-xml/xml-1.png)

3. Om toe te voegen de **XML-validatie** actie, kies **een actie toevoegen**.

4. De acties met de fout die u wilt filteren, voeren *xml* in het zoekvak. Kies **XML-validatie**.

    ![](./media/logic-apps-enterprise-integration-xml/xml-2.png)

5. Als u de XML-inhoud die u wilt valideren, schakelt u **inhoud**.

    ![](./media/logic-apps-enterprise-integration-xml/xml-1-5.png)

6. Selecteer de label body als de inhoud die u wilt valideren.

    ![](./media/logic-apps-enterprise-integration-xml/xml-3.png)

7. Om op te geven van het schema dat u gebruiken wilt voor het valideren van de vorige *inhoud* invoeren, kies **SCHEMANAAM**.

    ![](./media/logic-apps-enterprise-integration-xml/xml-4.png)

8. Sla uw werk  

    ![](./media/logic-apps-enterprise-integration-xml/xml-5.png)

Nu bent u klaar met het instellen van uw validatie-connector. In een toepassing werkelijkheid is het raadzaam de gevalideerde gegevens opslaan in een line-of-business (LOB)-app zoals SalesForce. De uitvoer van de gevalideerde om naar te verzenden Salesforce, moet u een actie toevoegen.

Als u wilt testen uw actie validatie, maak een aanvraag met het HTTP-eindpunt.

## <a name="next-steps"></a>Volgende stappen
[Meer informatie over het Enterprise-integratiepakket](../logic-apps/logic-apps-enterprise-integration-overview.md "meer informatie over Enterprise Integration Pack")   

