---
title: Decoderen van berichten in AS2 - Azure Logic Apps | Microsoft Docs
description: Het gebruik van de decoder AS2 in de Enterprise-integratiepakket voor Azure Logic Apps
services: logic-apps
documentationcenter: .net,nodejs,java
author: padmavc
manager: anneta
editor: 
ms.assetid: cf44af18-1fe5-41d5-9e06-cc57a968207c
ms.service: logic-apps
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/27/2016
ms.author: LADocs; padmavc
ms.openlocfilehash: a7920b2509fe368c6f7d55e17fe0bf0020c4562c
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2017
---
# <a name="decode-as2-messages-for-azure-logic-apps-with-the-enterprise-integration-pack"></a>AS2-berichten worden gedecodeerd voor Azure Logic Apps met de Enterprise-integratiepakket 

Om vast te stellen op beveiliging en betrouwbaarheid bij het verzenden van berichten, de decoderen AS2-bericht-connector te gebruiken. Deze connector biedt digitale ondertekening, ontsleuteling en bevestigingen via bericht Disposition meldingen (MDN).

## <a name="before-you-start"></a>Voordat u begint

Hier volgt de items die u nodig:

* Een Azure-account; kunt u een [gratis account](https://azure.microsoft.com/free)
* Een [integratie account](logic-apps-enterprise-integration-create-integration-account.md) die al is gedefinieerd en die zijn gekoppeld aan uw Azure-abonnement. U moet een integratie-account aan het gebruik van de connector decoderen AS2-bericht hebben.
* Ten minste twee [partners](logic-apps-enterprise-integration-partners.md) die al zijn gedefinieerd in uw account integratie
* Een [AS2-overeenkomst](logic-apps-enterprise-integration-as2.md) die al is gedefinieerd in uw account integratie

## <a name="decode-as2-messages"></a>AS2-berichten worden gedecodeerd

1. [Een logische app maken](../logic-apps/logic-apps-create-a-logic-app.md).

2. De connector decoderen AS2-bericht geen triggers, dus moet u een trigger voor het starten van uw logische app, zoals een aanvraag-trigger toevoegen. In de ontwerpfunctie voor Logic App een trigger toevoegen en vervolgens een actie toevoegen aan uw logische app.

3.  Voer in het zoekvak 'AS2' voor uw filter. Selecteer **AS2 - decoderen AS2-bericht**.
   
    ![Zoek naar 'AS2'](media/logic-apps-enterprise-integration-as2-decode/as2decodeimage1.png)

4. Als u geen verbindingen tot je account integratie eerder hebt gemaakt, wordt u gevraagd nu die verbinding maken. Naam van uw verbinding en selecteer de integratie-account waarmee u verbinding wilt maken.
   
    ![Integratie verbinding maken](media/logic-apps-enterprise-integration-as2-decode/as2decodeimage2.png)

    Eigenschappen met een sterretje zijn vereist.

    | Eigenschap | Details |
    | --- | --- |
    | Verbindingsnaam * |Voer een naam voor de verbinding. |
    | Integratie Account * |Voer een naam voor uw account integratie. Zorg ervoor dat uw account en logica app voor integratie in dezelfde Azure-locatie. |

5.  Wanneer u bent klaar, de verbindingsdetails van uw moeten er ongeveer uitzien in dit voorbeeld. Voor het voltooien van de verbinding te maken, kies **maken**.

    ![de verbindingsdetails integratie](media/logic-apps-enterprise-integration-as2-decode/as2decodeimage3.png)

6. Nadat de verbinding is gemaakt, zoals in dit voorbeeld, selecteer **hoofdtekst** en **Headers** van de uitvoer van de aanvraag.
   
    ![integratie van verbinding is gemaakt](media/logic-apps-enterprise-integration-as2-decode/as2decodeimage4.png) 

    Bijvoorbeeld:

    ![Selecteer hoofdtekst en Headers van de uitvoer van de aanvraag](media/logic-apps-enterprise-integration-as2-decode/as2decodeimage5.png) 

## <a name="as2-decoder-details"></a>AS2-decoder details

Het decoderen AS2-connector voert deze taken uit: 

* AS2/HTTP-headers worden verwerkt
* Verifieert de handtekening (indien geconfigureerd)
* De berichten ontsleutelt (indien geconfigureerd)
* Het bericht decomprimeert (indien geconfigureerd)
* Voor overeenstemming zorgt met een ontvangen MDN met het oorspronkelijke uitgaand bericht
* Updates en records in de database onweerlegbaarheid correleert
* Records voor statusrapportage AS2 schrijft
* De inhoud van het uitvoer-nettolading is base64-codering
* Bepaalt of een MDN vereist is, en of de MDN synchrone moet of asynchrone op basis van configuratie in AS2-overeenkomst
* Genereert een synchrone of asynchrone MDN (op basis van overeenkomst configuraties)
* De eigenschappen en correlatietokens ingesteld op de MDN

## <a name="try-this-sample"></a>Probeer dit voorbeeld

Als u wilt proberen een volledig operationeel logic app en voorbeeld AS2-scenario implementeren, Zie de [AS2 logic app-sjabloon en het scenario](https://azure.microsoft.com/documentation/templates/201-logic-app-as2-send-receive/).

## <a name="view-the-swagger"></a>De swagger weergeven
Zie de [swagger details](/connectors/as2/). 

## <a name="next-steps"></a>Volgende stappen
[Meer informatie over het Enterprise-integratiepakket](logic-apps-enterprise-integration-overview.md) 

