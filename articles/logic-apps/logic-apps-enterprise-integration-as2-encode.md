---
title: Coderen van berichten in AS2 - Azure Logic Apps | Microsoft Docs
description: Het gebruik van het coderingsprogramma AS2 in de Enterprise-integratiepakket voor Azure Logic Apps
services: logic-apps
documentationcenter: .net,nodejs,java
author: padmavc
manager: anneta
editor: 
ms.assetid: 332fb9e3-576c-4683-bd10-d177a0ebe9a3
ms.service: logic-apps
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/27/2017
ms.author: LADocs; padmavc
ms.openlocfilehash: 7889bf9e4e02143b6bb4c797531afa54f8647ce5
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2017
---
# <a name="encode-as2-messages-for-azure-logic-apps-with-the-enterprise-integration-pack"></a>AS2-berichten te coderen voor Azure Logic Apps met de Enterprise-integratiepakket

Om vast te stellen op beveiliging en betrouwbaarheid bij het verzenden van berichten, de coderen AS2-bericht-connector te gebruiken. Deze connector biedt digitale ondertekening, versleuteling en bevestigingen via bericht Disposition meldingen (MDN), zodat de ook ondersteuning voor niet-afwijzing.

## <a name="before-you-start"></a>Voordat u begint

Hier volgt de items die u nodig:

* Een Azure-account; kunt u een [gratis account](https://azure.microsoft.com/free)
* Een [integratie account](logic-apps-enterprise-integration-create-integration-account.md) die al is gedefinieerd en die zijn gekoppeld aan uw Azure-abonnement. U moet een integratie-account aan het gebruik van de connector coderen AS2-bericht hebben.
* Ten minste twee [partners](logic-apps-enterprise-integration-partners.md) die al zijn gedefinieerd in uw account integratie
* Een [AS2-overeenkomst](logic-apps-enterprise-integration-as2.md) die al is gedefinieerd in uw account integratie

## <a name="encode-as2-messages"></a>AS2-berichten te coderen

1. [Een logische app maken](logic-apps-create-a-logic-app.md).

2. De connector coderen AS2-bericht geen triggers, dus moet u een trigger voor het starten van uw logische app, zoals een aanvraag-trigger toevoegen. In de ontwerpfunctie voor Logic App een trigger toevoegen en vervolgens een actie toevoegen aan uw logische app.

3.  Voer in het zoekvak 'AS2' voor uw filter. Selecteer **AS2 - coderen AS2-bericht**.
   
    ![Zoek naar 'AS2'](./media/logic-apps-enterprise-integration-as2-encode/as2decodeimage1.png)

4. Als u geen verbindingen tot je account integratie eerder hebt gemaakt, wordt u gevraagd nu die verbinding maken. Naam van uw verbinding en selecteer de integratie-account waarmee u verbinding wilt maken. 
   
    ![Maak verbinding met de integratie-account](./media/logic-apps-enterprise-integration-as2-encode/as2encodeimage1.png)  

    Eigenschappen met een sterretje zijn vereist.

    | Eigenschap | Details |
    | --- | --- |
    | Verbindingsnaam * |Voer een naam voor de verbinding. |
    | Integratie Account * |Voer een naam voor uw account integratie. Zorg ervoor dat uw account en logica app voor integratie in dezelfde Azure-locatie. |

5.  Wanneer u bent klaar, de verbindingsdetails van uw moeten er ongeveer uitzien in dit voorbeeld. Voor het voltooien van de verbinding te maken, kies **maken**.
   
    ![de verbindingsdetails integratie](./media/logic-apps-enterprise-integration-as2-encode/as2encodeimage2.png)

6. Nadat de verbinding is gemaakt, zoals in dit voorbeeld, vindt u informatie voor **AS2-van**, **AS2-aan-id's** zoals geconfigureerd in de overeenkomst en **hoofdtekst**, die is de nettolading van het bericht.
   
    ![verplichte velden bieden](./media/logic-apps-enterprise-integration-as2-encode/as2encodeimage3.png)

## <a name="as2-encoder-details"></a>AS2-encoder details

Het coderen AS2-connector voert deze taken uit: 

* AS2/HTTP-headers van toepassing is
* Tekenen uitgaande berichten (indien geconfigureerd)
* Versleutelt uitgaande berichten (indien geconfigureerd)
* Comprimeren van het bericht (indien geconfigureerd)

## <a name="try-this-sample"></a>Probeer dit voorbeeld

Als u wilt proberen een volledig operationeel logic app en voorbeeld AS2-scenario implementeren, Zie de [AS2 logic app-sjabloon en het scenario](https://azure.microsoft.com/documentation/templates/201-logic-app-as2-send-receive/).

## <a name="view-the-swagger"></a>De swagger weergeven
Zie de [swagger details](/connectors/as2/). 

## <a name="next-steps"></a>Volgende stappen
[Meer informatie over het Enterprise-integratiepakket](logic-apps-enterprise-integration-overview.md "meer informatie over Enterprise Integration Pack") 

