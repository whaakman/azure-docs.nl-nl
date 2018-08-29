---
title: Integraties van B2B-enterprise - Azure Logic Apps maken | Microsoft Docs
description: B2B-gegevens in Azure Logic Apps met Enterprise Integration Pack ontvangen
services: logic-apps
ms.service: logic-apps
ms.suite: integration
author: divyaswarnkar
ms.author: divswa
ms.reviewer: jonfan, estfan, LADocs
ms.topic: article
ms.assetid: 20fc3722-6f8b-402f-b391-b84e9df6fcff
ms.date: 07/08/2016
ms.openlocfilehash: ad7a29f4a554d599b17576921542b1ac6e403911
ms.sourcegitcommit: 2ad510772e28f5eddd15ba265746c368356244ae
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/28/2018
ms.locfileid: "43127761"
---
# <a name="receive-b2b-data-with-azure-logic-apps-and-enterprise-integration-pack"></a>B2B-gegevens met Azure Logic Apps en Enterprise Integration Pack ontvangen

Nadat u een integratieaccount met partners en overeenkomsten maakt, bent u klaar om te maken van een werkstroom bedrijven (B2B) voor uw logische app met de [Enterprise Integration Pack](logic-apps-enterprise-integration-overview.md).

## <a name="prerequisites"></a>Vereisten

Het gebruik van de AS2 en X12 acties, hebt u een Enterprise Integration Account. Informatie over [over het maken van een Enterprise Integration Account](../logic-apps/logic-apps-enterprise-integration-accounts.md).

## <a name="create-a-logic-app-with-b2b-connectors"></a>Een logische app maken met B2B-connectors

Volg deze stappen voor het maken van een B2B-logische app die gebruikmaakt van de AS2 en X12 acties voor het ontvangen van gegevens van een trading partner:

1. Maak een logische app, klikt u vervolgens [uw app koppelen aan uw integratieaccount](../logic-apps/logic-apps-enterprise-integration-accounts.md).

2. Voeg een **aanvraag: wanneer een HTTP-aanvraag wordt ontvangen** trigger aan uw logische app.

    ![](./media/logic-apps-enterprise-integration-b2b/flatfile-1.png)

3. Om toe te voegen de **AS2-decodering** actie, selecteer **een actie toevoegen**.

    ![](./media/logic-apps-enterprise-integration-b2b/transform-2.png)

4. Als u wilt alle acties in de map die u wilt filteren, voert u het woord **as2** in het zoekvak in.

    ![](./media/logic-apps-enterprise-integration-b2b/b2b-5.png)

5. Selecteer de **AS2 - decodering AS2-bericht** actie.

    ![](./media/logic-apps-enterprise-integration-b2b/b2b-6.png)

6. Voeg de **hoofdtekst** die u wilt gebruiken als invoer. Selecteer in dit voorbeeld wordt de hoofdtekst van de HTTP-aanvraag die de logische app wordt geactiveerd. Of voer een expressie die invoer van de kopteksten in de **HEADERS** veld:

    @triggerOutputs([headers])

7. Voeg de vereiste **Headers** voor AS2, die u in de HTTP-aanvraagheaders vinden kunt. Selecteer de headers van de HTTP-aanvraag die de logische app activeren in dit voorbeeld.

8. Nu de decoderen X12 bericht-actie toevoegen. Selecteer **een actie toevoegen**.

    ![](./media/logic-apps-enterprise-integration-b2b/b2b-9.png)

9. Als u wilt alle acties in de map die u wilt filteren, voert u het woord **x12** in het zoekvak in.

    ![](./media/logic-apps-enterprise-integration-b2b/b2b-10.png)

10. Selecteer de **X12-decoderen X12 bericht** actie.

    ![](./media/logic-apps-enterprise-integration-b2b/b2b-as2message.png)

11. Nu moet u de invoer voor deze actie. Deze invoer is de uitvoer van de vorige AS2-actie.

    De werkelijke berichtinhoud is in een JSON-object en base64-gecodeerd, is dus u een expressie als invoer opgeven moet. 
    Voer de volgende expressie in de **X12 PLATTE bestand bericht te decoderen** invoerveld:
    
    @base64ToString(body('Decode_AS2_message')? ['AS2Message']? ['Inhoud'])

    Voeg stappen voor het decoderen van de X12 gegevens ontvangen van de trading partner en uitvoer van de items in een JSON-object toe. 
    Als u wilt de partner sturen dat de gegevens is ontvangen, kunt u terugsturen van een antwoord met de AS2 bericht toestand melding (MDN) in een HTTP-reactie.

12. Om toe te voegen de **antwoord** actie, kiest u **een actie toevoegen**.

    ![](./media/logic-apps-enterprise-integration-b2b/b2b-14.png)

13. Als u wilt alle acties in de map die u wilt filteren, voert u het woord **antwoord** in het zoekvak in.

    ![](./media/logic-apps-enterprise-integration-b2b/b2b-15.png)

14. Selecteer de **antwoord** actie.

    ![](./media/logic-apps-enterprise-integration-b2b/b2b-16.png)

15. Voor toegang tot de MDN uit de uitvoer van de **decoderen X12 bericht** actie, de reactie instellen **hoofdtekst** veld met deze expressie:

    @base64ToString(body('Decode_AS2_message')? ['OutgoingMdn']? ['Inhoud'])

    ![](./media/logic-apps-enterprise-integration-b2b/b2b-17.png)  

16. Sla uw werk op.

    ![](./media/logic-apps-enterprise-integration-b2b/transform-5.png)  

U bent nu klaar instellen van uw B2B logic app. In een werkelijke toepassing, kunt u voor het opslaan van het gedecodeerde X12 gegevens in een line-of-business (LOB)-app of de gegevensopslag. Als u wilt verbinding maken met uw eigen LOB-apps en het gebruik van deze API's in uw logische app, kunt u verdere acties toevoegen of schrijven van aangepaste API's.

## <a name="features-and-use-cases"></a>Functies en use-cases

* De AS2 en X12 decoderen en coderen acties kunt u gegevens tussen zakelijke partners met behulp van protocollen volgens de industrienorm in logische apps uit te wisselen.
* U kunt voor het uitwisselen van gegevens met handelspartners, AS2 en X12 gebruiken met of zonder elkaar.
* De B2B-acties kunnen u eenvoudig partners en overeenkomsten maken in uw integratie-account en gebruiken in een logische app.
* Wanneer u uw logische app met andere acties uitbreidt, kunt u deze kunt verzenden en ontvangen van gegevens tussen andere apps en services zoals SalesForce.

## <a name="learn-more"></a>Meer informatie
[Meer informatie over het Enterprise Integration Pack](logic-apps-enterprise-integration-overview.md)
