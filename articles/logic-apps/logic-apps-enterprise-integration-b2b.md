---
title: Oplossingen voor B2B - Azure Logic Apps maken | Microsoft Docs
description: Ontvangt gegevens in logische apps met behulp van de B2B-functies in de Enterprise-integratiepakket
services: logic-apps
documentationcenter: .net,nodejs,java
author: msftman
manager: anneta
editor: cgronlun
ms.assetid: 20fc3722-6f8b-402f-b391-b84e9df6fcff
ms.service: logic-apps
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/08/2016
ms.author: LADocs; padmavc
ms.openlocfilehash: 0625787ddcbc0091e70b111f687e25929720ad15
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="receive-data-in-logic-apps-with-the-b2b-features-in-the-enterprise-integration-pack"></a>Ontvangt gegevens in logische apps met de B2B-functies in de Enterprise-integratiepakket

Nadat u een account voor integratie met partners en overeenkomsten hebt gemaakt, bent u klaar voor het maken van een werkstroom bedrijven (B2B) voor uw logische app met de [Enterprise Integration Pack](logic-apps-enterprise-integration-overview.md).

## <a name="prerequisites"></a>Vereisten

Gebruik de AS2 en X12 acties, moet u een Enterprise Integration-Account hebben. Meer informatie over [een Enterprise Integration-Account maken](../logic-apps/logic-apps-enterprise-integration-accounts.md).

## <a name="create-a-logic-app-with-b2b-connectors"></a>Een logische app maken met B2B-connectors

Volg deze stappen voor het maken van een B2B logische app die gebruikmaakt van de AS2 en X12 acties mag gegevens ontvangen van een trading partner:

1. Maak een logische app, klikt u vervolgens [uw app koppelen aan uw account integratie](../logic-apps/logic-apps-enterprise-integration-accounts.md).

2. Voeg een **aanvraag: wanneer een HTTP-aanvraag wordt ontvangen** trigger aan uw logische app.

    ![](./media/logic-apps-enterprise-integration-b2b/flatfile-1.png)

3. Om toe te voegen de **decoderen AS2** actie, selecteer **een actie toevoegen**.

    ![](./media/logic-apps-enterprise-integration-b2b/transform-2.png)

4. Als u wilt alle acties die u wilt filteren, voer dan het woord **as2** in het zoekvak.

    ![](./media/logic-apps-enterprise-integration-b2b/b2b-5.png)

5. Selecteer de **AS2 - decoderen AS2-bericht** in te grijpen.

    ![](./media/logic-apps-enterprise-integration-b2b/b2b-6.png)

6. Voeg de **hoofdtekst** die u wilt gebruiken als invoer. Selecteer de hoofdtekst van de HTTP-aanvraag die de logische app getriggerd in dit voorbeeld. Of typ een expressie die de invoer van de kopteksten in de **HEADERS** veld:

    @triggerOutputs([headers])

7. Voeg de vereiste **Headers** voor AS2, kunt u vinden in de headers van de HTTP-aanvraag. Selecteer in de headers van de HTTP-aanvraag die de logische app activeren in dit voorbeeld.

8. Nu de berichtactie decoderen X12 toevoegen. Selecteer **een actie toevoegen**.

    ![](./media/logic-apps-enterprise-integration-b2b/b2b-9.png)

9. Als u wilt alle acties die u wilt filteren, voer dan het woord **x12** in het zoekvak.

    ![](./media/logic-apps-enterprise-integration-b2b/b2b-10.png)

10. Selecteer de **X12-decoderen X12 bericht** in te grijpen.

    ![](./media/logic-apps-enterprise-integration-b2b/b2b-as2message.png)

11. Nu moet u de invoer voor deze actie. Deze invoer is de uitvoer van de vorige AS2-actie.

    Inhoud van het werkelijke bericht in een JSON-object is en is met base64 gecodeerd, dus u een expressie als invoer opgeven moet. 
    Voer de volgende expressie in de **X12 PLATTE bestand bericht te decoderen** invoerveld:
    
    @base64ToString(body('Decode_AS2_message')? ['AS2Message']? ['Inhoud'])

    Voeg nu de stappen voor het decoderen van de X12 gegevens ontvangen van de trading partner en uitvoer van de items in een JSON-object. 
    Als u wilt de partner waarschuwen dat de gegevens zijn ontvangen, kunt u verzenden weer een antwoord met de AS2 bericht Disposition melding (MDN) in een HTTP-antwoord-actie.

12. Om toe te voegen de **antwoord** actie, kies **een actie toevoegen**.

    ![](./media/logic-apps-enterprise-integration-b2b/b2b-14.png)

13. Als u wilt alle acties die u wilt filteren, voer dan het woord **antwoord** in het zoekvak.

    ![](./media/logic-apps-enterprise-integration-b2b/b2b-15.png)

14. Selecteer de **antwoord** in te grijpen.

    ![](./media/logic-apps-enterprise-integration-b2b/b2b-16.png)

15. Voor toegang tot de MDN uit de uitvoer van de **decoderen X12 bericht** actie, het antwoord ingesteld **hoofdtekst** veld met deze expressie:

    @base64ToString(body('Decode_AS2_message')? ['OutgoingMdn']? ['Inhoud'])

    ![](./media/logic-apps-enterprise-integration-b2b/b2b-17.png)  

16. Sla uw werk op.

    ![](./media/logic-apps-enterprise-integration-b2b/transform-5.png)  

U bent nu klaar uw B2B logische app in te stellen. In een toepassing praktijk wilt u mogelijk de gedecodeerde X12 opslaan gegevens in een line-of-business (LOB)-app of de gegevensopslag. Als u wilt verbinding maken met uw eigen LOB-apps en het gebruik van deze API's in uw logische app, kunt u verdere acties toevoegen of schrijven van aangepaste API's.

## <a name="features-and-use-cases"></a>Functies en gebruiksvoorbeelden

* De AS2 en X12 decoderen en coderen acties kunnen uitwisselen van gegevens tussen handelspartners via standaardprotocollen in logic apps.
* U kunt voor het uitwisselen van gegevens met handelspartners AS2 en X12 gebruiken met of zonder elkaar.
* De acties B2B kunnen u eenvoudig partners en -overeenkomsten maken in uw account integratie en gebruiken in een logische app.
* Wanneer u uw logische app met andere acties uitbreidt, kunt u deze kunt verzenden en ontvangen van gegevens tussen andere apps en services zoals SalesForce.

## <a name="learn-more"></a>Meer informatie
[Meer informatie over het Enterprise-integratiepakket](logic-apps-enterprise-integration-overview.md)
