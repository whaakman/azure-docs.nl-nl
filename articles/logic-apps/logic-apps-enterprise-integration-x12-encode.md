---
title: Codeer X12 berichten - Azure Logic Apps | Microsoft Docs
description: EDI valideren en converteren XML-codering berichten met X12 bericht codering in de Enterprise-integratiepakket voor Azure Logic Apps
services: logic-apps
documentationcenter: .net,nodejs,java
author: padmavc
manager: jeconnoc
editor: ''
ms.assetid: a01e9ca9-816b-479e-ab11-4a984f10f62d
ms.service: logic-apps
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/27/2017
ms.author: LADocs; padmavc
ms.openlocfilehash: ec350560f4aca4e9a0b4f00892b690012c9f08dd
ms.sourcegitcommit: 6f6d073930203ec977f5c283358a19a2f39872af
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/11/2018
ms.locfileid: "35299004"
---
# <a name="encode-x12-messages-for-azure-logic-apps-with-the-enterprise-integration-pack"></a>Codeer X12 berichten voor Azure Logic Apps met de Enterprise-integratiepakket

Met de connector van het bericht coderen X12, kunt u valideren EDI- en partner-specifieke eigenschappen, berichten XML-indeling converteren naar EDI transactie sets in de uitwisseling en aanvragen van een bevestiging van technische, functionele bevestiging of beide.
Voor het gebruik van deze connector, moet u de connector toevoegen aan een bestaande trigger in uw logische app.

## <a name="before-you-start"></a>Voordat u begint

Hier volgt de items die u nodig:

* Een Azure-account; kunt u een [gratis account](https://azure.microsoft.com/free)
* Een [integratie account](logic-apps-enterprise-integration-create-integration-account.md) die al is gedefinieerd en die zijn gekoppeld aan uw Azure-abonnement. U moet een integratie-account aan het gebruik van de connector van coderen X12 bericht hebben.
* Ten minste twee [partners](logic-apps-enterprise-integration-partners.md) die al zijn gedefinieerd in uw account integratie
* Een [X12 overeenkomst](logic-apps-enterprise-integration-x12.md) die al is gedefinieerd in uw account integratie

## <a name="encode-x12-messages"></a>Codeer X12 berichten

1. [Een logische app maken](quickstart-create-first-logic-app-workflow.md).

2. Het bericht coderen X12 connector geen triggers, dus moet u een trigger voor het starten van uw logische app, zoals een aanvraag-trigger toevoegen. In de ontwerpfunctie voor Logic App een trigger toevoegen en vervolgens een actie toevoegen aan uw logische app.

3.  Voer in het zoekvak 'x12' voor uw filter. Selecteer een **X12-coderen naar X12 bericht door de naam van de overeenkomst** of **X12-coderen naar X12 bericht door identiteiten**.
   
    ![Zoek naar 'x12'](./media/logic-apps-enterprise-integration-x12-encode/x12decodeimage1.png) 

3. Als u geen verbindingen tot je account integratie eerder hebt gemaakt, wordt u gevraagd nu die verbinding maken. Naam van uw verbinding en selecteer de integratie-account waarmee u verbinding wilt maken. 
   
    ![integratie-account verbinding](./media/logic-apps-enterprise-integration-x12-encode/x12encodeimage1.png)

    Eigenschappen met een sterretje zijn vereist.

    | Eigenschap | Details |
    | --- | --- |
    | Verbindingsnaam * |Voer een naam voor de verbinding. |
    | Integratie Account * |Voer een naam voor uw account integratie. Zorg ervoor dat uw account en logica app voor integratie in dezelfde Azure-locatie. |

5.  Wanneer u bent klaar, de verbindingsdetails van uw moeten er ongeveer uitzien in dit voorbeeld. Voor het voltooien van de verbinding te maken, kies **maken**.

    ![integratie-account verbinding is gemaakt](./media/logic-apps-enterprise-integration-x12-encode/x12encodeimage2.png)

    De verbinding wordt nu gemaakt.

    ![de accountdetails verbinding integratie](./media/logic-apps-enterprise-integration-x12-encode/x12encodeimage3.png) 

#### <a name="encode-x12-messages-by-agreement-name"></a>Codeer X12 berichten door de naam van de overeenkomst

Als u ervoor hebt gekozen voor het coderen van X12 berichten door de naam van de overeenkomst, open de **naam van X12 overeenkomst** lijst Typ of Selecteer uw bestaande X12 overeenkomst. Voer in het XML-bericht voor het coderen.

![Voer X12 naam van de overeenkomst en de XML-bericht voor het coderen van](./media/logic-apps-enterprise-integration-x12-encode/x12encodeimage4.png)

#### <a name="encode-x12-messages-by-identities"></a>Codeer X12 berichten door identiteiten

Als u kiest voor het coderen van X12 berichten door identiteiten, voer de afzender-id, kwalificatie afzender, ontvanger-id en ontvanger kwalificatie zoals geconfigureerd in uw x12-overeenkomst(en) overeenkomst. Selecteer het XML-bericht voor het coderen.
   
![Geef identiteiten voor een afzender en ontvanger, selecteer van XML-bericht voor het coderen van](./media/logic-apps-enterprise-integration-x12-encode/x12encodeimage5.png) 

## <a name="x12-encode-details"></a>X12 details coderen

De X12 coderen connector voert deze taken uit:

* De resolutie van de overeenkomst door overeenkomende zender en ontvanger context-eigenschappen.
* De EDI-uitwisseling, conversie van XML-codering berichten naar EDI transactie sets in de uitwisseling serialiseert.
* Transactie set-header en trailer segmenten van toepassing is
* Genereren van een besturingselement interchange getal, een besturingselement groepsnummer en een getal voor het instellen van transactie voor elke uitgaande uitwisseling
* Vervangt scheidingstekens in de nettolading
* Valideert EDI en partner-specifieke eigenschappen
  * Schemavalidatie van de gegevenselementen van de transactie-set tegen het bericht Schema
  * EDI gevalideerd gegevenselementen transactie-set.
  * Uitgebreide validatie uitgevoerd op van de transactie-set-gegevenselementen
* Vraagt om bevestiging van de technische en/of functionele (indien geconfigureerd).
  * Een technische bevestiging genereert als gevolg van header-validatie. De bevestiging van de technische meldt de status van de verwerking van een DIF-header en trailer door de ontvanger adres
  * Een functionele bevestiging genereert als gevolg van validatie van de hoofdtekst. De functionele bevestiging rapporten elke fout opgetreden tijdens het verwerken van het document ontvangen

## <a name="view-the-swagger"></a>De swagger weergeven
Zie de [swagger details](/connectors/x12/). 

## <a name="next-steps"></a>Volgende stappen
[Meer informatie over het Enterprise-integratiepakket](logic-apps-enterprise-integration-overview.md "meer informatie over Enterprise Integration Pack") 

