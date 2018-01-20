---
title: Decoderen EDIFACT berichten - Azure Logic Apps | Microsoft Docs
description: EDI valideren en genereren van bevestigingen met de decoder EDIFACT-bericht in de Enterprise-integratiepakket voor Azure Logic Apps
services: logic-apps
documentationcenter: .net,nodejs,java
author: padmavc
manager: anneta
editor: 
ms.assetid: 0e61501d-21a2-4419-8c6c-88724d346e81
ms.service: logic-apps
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/27/2017
ms.author: LADocs; padmavc
ms.openlocfilehash: 41bd0291f3ff8f5dd61a21d1ba4ecb3b60a3cbfb
ms.sourcegitcommit: be9a42d7b321304d9a33786ed8e2b9b972a5977e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/19/2018
---
# <a name="decode-edifact-messages-for-azure-logic-apps-with-the-enterprise-integration-pack"></a>EDIFACT-berichten worden gedecodeerd voor Azure Logic Apps met de Enterprise-integratiepakket

Met de connector van het bericht EDIFACT decoderen, kunt u valideren EDI- en partner-specifieke eigenschappen, uitgewisseld in transacties sets splitsen of volledige uitgewisseld behouden en genereren van bevestigingen voor verwerkte transacties. Voor het gebruik van deze connector, moet u de connector toevoegen aan een bestaande trigger in uw logische app.

## <a name="before-you-start"></a>Voordat u begint

Hier volgt de items die u nodig:

* Een Azure-account; kunt u een [gratis account](https://azure.microsoft.com/free)
* Een [integratie account](logic-apps-enterprise-integration-create-integration-account.md) die al is gedefinieerd en die zijn gekoppeld aan uw Azure-abonnement. U moet een integratie-account aan het gebruik van de connector decoderen EDIFACT-bericht hebben. 
* Ten minste twee [partners](logic-apps-enterprise-integration-partners.md) die al zijn gedefinieerd in uw account integratie
* Een [EDIFACT overeenkomst](logic-apps-enterprise-integration-edifact.md) die al is gedefinieerd in uw account integratie

## <a name="decode-edifact-messages"></a>EDIFACT-berichten worden gedecodeerd

1. [Een logische app maken](quickstart-create-first-logic-app-workflow.md).

2. Het bericht decoderen EDIFACT connector geen triggers, dus moet u een trigger voor het starten van uw logische app, zoals een aanvraag-trigger toevoegen. In de ontwerpfunctie voor Logic App een trigger toevoegen en vervolgens een actie toevoegen aan uw logische app.

3. Voer in het zoekvak 'EDIFACT' als filter. Selecteer **decoderen EDIFACT bericht**.
   
    ![EDIFACT zoeken](./media/logic-apps-enterprise-integration-edifact-decode/edifactdecodeimage1.png)

3. Als u geen verbindingen tot je account integratie eerder hebt gemaakt, wordt u gevraagd nu die verbinding maken. Naam van uw verbinding en selecteer de integratie-account waarmee u verbinding wilt maken.
   
    ![integratie-account maken](./media/logic-apps-enterprise-integration-edifact-decode/edifactdecodeimage2.png)

    Eigenschappen met een sterretje zijn vereist.

    | Eigenschap | Details |
    | --- | --- |
    | Verbindingsnaam * |Voer een naam voor de verbinding. |
    | Integratie Account * |Voer een naam voor uw account integratie. Zorg ervoor dat uw account en logica app voor integratie in dezelfde Azure-locatie. |

4. Wanneer u bent klaar om te maken van uw verbinding, kies **maken**. De verbindingsdetails moeten er ongeveer als in dit voorbeeld:

    ![de accountdetails integratie](./media/logic-apps-enterprise-integration-edifact-decode/edifactdecodeimage3.png)  

5. Nadat de verbinding is gemaakt, zoals in dit voorbeeld, selecteert u het bericht EDIFACT plat bestand decoderen.

    ![integratie-account verbinding is gemaakt](./media/logic-apps-enterprise-integration-edifact-decode/edifactdecodeimage4.png)  

    Bijvoorbeeld:

    ![Selecteer EDIFACT plat bestand-bericht voor het decoderen van](./media/logic-apps-enterprise-integration-edifact-decode/edifactdecodeimage5.png)  

## <a name="edifact-decoder-details"></a>EDIFACT decoder details

De connector decoderen EDIFACT voert deze taken uit: 

* Valideert de envelop tegen trading partner agreement.
* Oplossing voor de overeenkomst door die overeenkomt met de kwalificatie afzender-id en ontvanger kwalificatie & -id.
* Een knooppunt in meerdere transacties splitst wanneer het knooppunt heeft meer dan één transactie op basis van de overeenkomst instellingen configuratie ontvangen.
* De uitwisseling ontleed.
* Valideert EDI en partner-specifieke eigenschappen, waaronder:
  * Validatie van de structuur van de envelop interchange
  * Schemavalidatie van de enveloppe ten opzichte van het besturingselement schema
  * Schemavalidatie van de gegevenselementen van de transactie-set tegen het berichtschema
  * EDI-validatie wordt uitgevoerd op transactie-set gegevenselementen
* Verifieert dat de DIF, groep en transactie set besturingselement getallen niet identiek zijn (indien geconfigureerd) 
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
* Genereert een technische (beheer) en/of functionele bevestiging (indien geconfigureerd).
  * Een technische bevestigings- of de CONTRL ACK rapporteert de resultaten van een syntactische controle van de volledige ontvangen uitwisseling.
  * Een functionele bevestiging erkent accepteren of weigeren van een ontvangen interchange of een groep

## <a name="view-swagger-file"></a>Swagger-bestand weergeven
De Swagger-details voor de connector EDIFACT Zie [EDIFACT](/connectors/edifact/).

## <a name="next-steps"></a>Volgende stappen
[Meer informatie over het Enterprise-integratiepakket](logic-apps-enterprise-integration-overview.md "meer informatie over Enterprise Integration Pack") 

