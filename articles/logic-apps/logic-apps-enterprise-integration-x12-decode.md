---
title: Decoderen X12 berichten - Azure Logic Apps | Microsoft Docs
description: EDI valideren en genereren van bevestigingen met de X12 decoder bericht in de Enterprise-integratiepakket voor Azure Logic Apps
services: logic-apps
documentationcenter: .net,nodejs,java
author: padmavc
manager: anneta
editor: 
ms.assetid: 4fd48d2d-2008-4080-b6a1-8ae183b48131
ms.service: logic-apps
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/27/2017
ms.author: LADocs; padmavc
ms.openlocfilehash: bc2e5c2b351fb87cb763459a9e24368a422ada1b
ms.sourcegitcommit: be9a42d7b321304d9a33786ed8e2b9b972a5977e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/19/2018
---
# <a name="decode-x12-messages-for-azure-logic-apps-with-the-enterprise-integration-pack"></a>Decoderen X12 berichten voor Azure Logic Apps met de Enterprise-integratiepakket

Met de connector van het bericht decoderen X12 kunt u de envelop tegen een handelspartnerovereenkomst valideren, valideren EDI- en partner-specifieke eigenschappen uitgewisseld in transacties sets splitsen of volledige uitgewisseld behouden en genereren van bevestigingen voor transacties verwerkt. Voor het gebruik van deze connector, moet u de connector toevoegen aan een bestaande trigger in uw logische app.

## <a name="before-you-start"></a>Voordat u begint

Hier volgt de items die u nodig:

* Een Azure-account; kunt u een [gratis account](https://azure.microsoft.com/free)
* Een [integratie account](logic-apps-enterprise-integration-create-integration-account.md) die al is gedefinieerd en die zijn gekoppeld aan uw Azure-abonnement. U moet een account integratie gebruik van het bericht decoderen X12 connector hebben.
* Ten minste twee [partners](logic-apps-enterprise-integration-partners.md) die al zijn gedefinieerd in uw account integratie
* Een [X12 overeenkomst](logic-apps-enterprise-integration-x12.md) die al is gedefinieerd in uw account integratie

## <a name="decode-x12-messages"></a>Decoderen X12 berichten

1. [Een logische app maken](quickstart-create-first-logic-app-workflow.md).

2. Het bericht decoderen X12 connector geen triggers, dus moet u een trigger voor het starten van uw logische app, zoals een aanvraag-trigger toevoegen. In de ontwerpfunctie voor Logic App een trigger toevoegen en vervolgens een actie toevoegen aan uw logische app.

3.  Voer in het zoekvak 'x12' voor uw filter. Selecteer **X12-decoderen X12 bericht**.
   
    ![Zoek naar 'x12'](media/logic-apps-enterprise-integration-x12-decode/x12decodeimage1.png)  

3. Als u geen verbindingen tot je account integratie eerder hebt gemaakt, wordt u gevraagd nu die verbinding maken. Naam van uw verbinding en selecteer de integratie-account waarmee u verbinding wilt maken. 

    ![Bieden integratie Verbindingsdetails voor account](media/logic-apps-enterprise-integration-x12-decode/x12decodeimage4.png)

    Eigenschappen met een sterretje zijn vereist.

    | Eigenschap | Details |
    | --- | --- |
    | Verbindingsnaam * |Voer een naam voor de verbinding. |
    | Integratie Account * |Voer een naam voor uw account integratie. Zorg ervoor dat uw account en logica app voor integratie in dezelfde Azure-locatie. |

5.  Wanneer u bent klaar, de verbindingsdetails van uw moeten er ongeveer uitzien in dit voorbeeld. Voor het voltooien van de verbinding te maken, kies **maken**.
   
    ![de accountdetails verbinding integratie](media/logic-apps-enterprise-integration-x12-decode/x12decodeimage5.png) 

6. Nadat de verbinding is gemaakt, zoals in dit voorbeeld, selecteer de X12 plat bestand-bericht te decoderen.

    ![integratie-account verbinding is gemaakt](media/logic-apps-enterprise-integration-x12-decode/x12decodeimage6.png) 

    Bijvoorbeeld:

    ![Selecteer X12 platte bericht voor het decoderen van bestand](media/logic-apps-enterprise-integration-x12-decode/x12decodeimage7.png) 

   > [!NOTE]
   > De inhoud van het werkelijke bericht of nettolading voor de bericht-matrix, goede of minder, is met base64 gecodeerd. U moet dus een expressie die deze inhoud verwerkt opgeven.
   > Hier volgt een voorbeeld waarin de inhoud als XML-bestand dat u invoeren kunt in de codeweergave of verwerkt met behulp van de opbouwfunctie voor in de ontwerpfunctie.
   > ``` json
   > "content": "@xml(base64ToBinary(item()?['Payload']))"
   > ```
   > ![Voorbeeld van de inhoud](media/logic-apps-enterprise-integration-x12-decode/content-example.png)
   >


## <a name="x12-decode-details"></a>X12 decoderen details

De X12 decoderen connector voert deze taken uit:

* Valideert de envelop tegen trading partner agreement
* Valideert EDI en partner-specifieke eigenschappen
  * Structurele EDI-validatie en uitgebreide schemavalidatie
  * Validatie van de structuur van de DIF-envelope.
  * Schemavalidatie van de enveloppe ten opzichte van het schema van het besturingselement.
  * Schemavalidatie van de gegevenselementen van de transactie-set tegen het berichtschema.
  * EDI-validatie wordt uitgevoerd op transactie-set gegevenselementen 
* Controleert of de DIF, groep en transactie set besturingselement getallen zijn niet identiek
  * Controleert het nummer van het besturingselement interchange tegen eerder ontvangen uitgewisseld.
  * Controleert of het besturingselement groepsnummer tegen een andere groep besturingselement nummers in het knooppunt.
  * Controleert dat de transactie controle-aantal ingesteld op basis van andere cijfers voor het instellen van transactie in die groep.
* De uitwisseling in transactie sets splitst of behoudt de gehele uitwisseling:
  * Gesplitste Interchange als transaction sets - transactie sets onderbreken bij fout: splitsingen interchange in transactie wordt ingesteld en wordt elke transactie set geparseerd. 
  De X12 decoderen actie levert alleen deze transactie wordt ingesteld dat de validatie mislukt `badMessages`, en de resterende transacties wordt ingesteld op uitgangen `goodMessages`.
  * Gesplitste Interchange als transaction sets - uitwisseling onderbreken bij fout: splitsingen interchange in transactie wordt ingesteld en wordt elke transactie set geparseerd. 
  Als een of meer transactie wordt ingesteld in het knooppunt de validatie niet doorstaan, de X12 decoderen actie levert de transactie wordt ingesteld in die interchange naar `badMessages`.
  * Behouden DIF - transactie sets onderbreken bij fout: de uitwisseling behouden en de gehele uitwisseling van de batch worden verwerkt. 
  De X12 decoderen actie levert alleen deze transactie wordt ingesteld dat de validatie mislukt `badMessages`, en de resterende transacties wordt ingesteld op uitgangen `goodMessages`.
  * Behouden DIF - uitwisseling onderbreken bij fout: de uitwisseling behouden en de gehele uitwisseling van de batch worden verwerkt. 
  Als een of meer transactie wordt ingesteld in het knooppunt de validatie niet doorstaan, de X12 decoderen actie levert de transactie wordt ingesteld in die interchange naar `badMessages`. 
* Genereert een bevestiging technische en/of functionele (indien geconfigureerd).
  * Een technische bevestiging genereert als gevolg van header-validatie. De bevestiging van de technische meldt de status van de verwerking van een DIF-header en trailer door de ontvanger adres.
  * Een functionele bevestiging genereert als gevolg van validatie van de hoofdtekst. De functionele bevestiging rapporten elke fout opgetreden tijdens het verwerken van het document ontvangen

## <a name="view-the-swagger"></a>De swagger weergeven
Zie de [swagger details](/connectors/x12/). 

## <a name="next-steps"></a>Volgende stappen
[Meer informatie over het Enterprise-integratiepakket](../logic-apps/logic-apps-enterprise-integration-overview.md "meer informatie over Enterprise Integration Pack") 

