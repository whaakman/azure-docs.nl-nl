---
title: EDIFACT-berichten - Azure Logic Apps-decodering | Microsoft Docs
description: EDI valideren en het genereren van bevestigingen met de decoder EDIFACT-bericht voor Azure Logic Apps met Enterprise Integration Pack
services: logic-apps
ms.service: logic-apps
ms.suite: integration
author: divyaswarnkar
ms.author: divswa
ms.reviewer: jonfan, estfan, LADocs
ms.topic: article
ms.assetid: 0e61501d-21a2-4419-8c6c-88724d346e81
ms.date: 01/27/2017
ms.openlocfilehash: b101922d15a3f90c29eff51c223d2ea7dc30ddf2
ms.sourcegitcommit: 2ad510772e28f5eddd15ba265746c368356244ae
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/28/2018
ms.locfileid: "43125349"
---
# <a name="decode-edifact-messages-for-azure-logic-apps-with-the-enterprise-integration-pack"></a>EDIFACT-berichten decoderen voor Azure Logic Apps met het Enterprise Integration Pack

Met de EDIFACT-decodering bericht-connector, kunt u valideren EDI en partner-specifieke eigenschappen, uitwisselingen in transacties sets splitsen of hele uitwisselingen behouden en genereren van bevestigingen voor verwerkte transacties. Voor het gebruik van deze connector, moet u de connector toevoegen aan een bestaande trigger in uw logische app.

## <a name="before-you-start"></a>Voordat u begint

Hier volgt de items die u nodig hebt:

* Een Azure-account; u kunt maken een [gratis account](https://azure.microsoft.com/free)
* Een [integratieaccount](logic-apps-enterprise-integration-create-integration-account.md) die al is gedefinieerd en die zijn gekoppeld aan uw Azure-abonnement. Hebt u een integratieaccount om de EDIFACT-decodering bericht-connector te gebruiken. 
* Ten minste twee [partners](logic-apps-enterprise-integration-partners.md) die al zijn gedefinieerd in uw integratieaccount
* Een [EDIFACT-overeenkomst](logic-apps-enterprise-integration-edifact.md) die al gedefinieerd in uw integratieaccount

## <a name="decode-edifact-messages"></a>EDIFACT-berichten decoderen

1. [Maak een logische app](quickstart-create-first-logic-app-workflow.md).

2. De EDIFACT-decodering bericht connector geen triggers, dus moet u een trigger voor het starten van uw logische app, zoals een aanvraag als trigger toevoegen. Een trigger toevoegen in de Logic App Designer en klikt u vervolgens een actie toevoegen aan uw logische app.

3. Typ 'EDIFACT' als filter in het zoekvak. Selecteer **EDIFACT-bericht decoderen**.
   
    ![EDIFACT zoeken](./media/logic-apps-enterprise-integration-edifact-decode/edifactdecodeimage1.png)

3. Als u geen verbindingen voor uw integratieaccount eerder hebt gemaakt, wordt u gevraagd om nu deze verbinding te maken. Naam van de verbinding en selecteer de integratieaccount waarmee u verbinding wilt maken.
   
    ![integratie-account maken](./media/logic-apps-enterprise-integration-edifact-decode/edifactdecodeimage2.png)

    Eigenschappen met een sterretje zijn vereist.

    | Eigenschap | Details |
    | --- | --- |
    | Verbindingsnaam * |Voer een naam voor de verbinding. |
    | Integratie-Account * |Voer een naam voor uw integratie-account. Zorg ervoor dat uw integratie-account en logica-app zich in dezelfde Azure-locatie. |

4. Als u klaar bent voor de verbinding hebt aangemaakt, kiest u **maken**. Uw verbindingsgegevens moeten met het volgende voorbeeld als volgt uitzien:

    ![details van de integratie-account](./media/logic-apps-enterprise-integration-edifact-decode/edifactdecodeimage3.png)  

5. Nadat de verbinding is gemaakt, zoals wordt weergegeven in dit voorbeeld, selecteert u de platte bestandsindeling EDIFACT-bericht moet worden gedecodeerd.

    ![integratie-accountverbinding hebt gemaakt](./media/logic-apps-enterprise-integration-edifact-decode/edifactdecodeimage4.png)  

    Bijvoorbeeld:

    ![Selecteer het EDIFACT-bericht met platte bestanden voor het decoderen van](./media/logic-apps-enterprise-integration-edifact-decode/edifactdecodeimage5.png)  

## <a name="edifact-decoder-details"></a>Details van de EDIFACT-decodeerfunctie

De EDIFACT-decodering connector voert deze taken uit: 

* Valideert de envelop tegen trading partner agreement.
* Oplossing voor de overeenkomst door die overeenkomt met de afzenderkwalificatie van de-id en ontvangerskwalificatie & id.
* Hiermee wordt een knooppunt in meerdere transacties wanneer de uitwisseling heeft meer dan één transactie op basis van de overeenkomst van de instellingen voor configuratie ontvangen.
* De uitwisseling ontleden.
* Valideert EDI en partner-specifieke eigenschappen met inbegrip van:
  * Validatie van de structuur van de envelop uitwisseling
  * Schemavalidatie van de enveloppe op basis van het schema van het besturingselement
  * Schemavalidatie van de gegevenselementen van de transactie-set op basis van de berichtschema
  * EDI-validatie uitgevoerd voor de transactieset gegevenselementen
* Hiermee wordt gecontroleerd dat de uitwisseling, groep en transactie set controlenummers geen dubbele waarden zijn (indien geconfigureerd) 
  * Controleert of het controlenummer voor de uitwisseling tegen eerder ontvangen uitwisselingen. 
  * Controleert of het groepscontrolenummer op basis van andere groep controlenummers in de uitwisseling. 
  * Controleert of dat de transactie controlenummer ingesteld op basis van andere transactie set controlenummers in die groep.
* Hiermee wordt de uitwisseling in transactiesets of behoudt de gehele uitwisseling:
  * Uitwisseling splitsen in transactiereeksen - transactiereeksen onderbreken bij fout: uitwisseling splitsen in transactie wordt ingesteld en wordt elke transactieset geparseerd. 
  De X12 decoderen actie voert alleen deze transactie wordt ingesteld die niet voldoen aan de validatie `badMessages`, en de resterende transacties wordt ingesteld op uitvoer `goodMessages`.
  * Uitwisseling splitsen in transactiereeksen - uitwisseling onderbreken bij fout: uitwisseling splitsen in transactie wordt ingesteld en wordt elke transactieset geparseerd. 
  Als een of meer transactie wordt ingesteld in de uitwisseling mislukt de validatie van de X12 decoderen actie voert alle de transactie wordt ingesteld in dat knooppunt aan `badMessages`.
  * Uitwisseling bewaren-transactiereeksen onderbreken bij fout: de uitwisseling behouden en verwerken van de gehele uitwisseling van de batch. 
  De X12 decoderen actie voert alleen deze transactie wordt ingesteld die niet voldoen aan de validatie `badMessages`, en de resterende transacties wordt ingesteld op uitvoer `goodMessages`.
  * Uitwisseling bewaren-uitwisseling onderbreken bij fout: de uitwisseling behouden en verwerken van de gehele uitwisseling van de batch. 
  Als een of meer transactie wordt ingesteld in de uitwisseling mislukt de validatie van de X12 decoderen actie voert alle de transactie wordt ingesteld in dat knooppunt aan `badMessages`.
* Genereert een technische (beheer) en/of functionele bevestigingen (indien geconfigureerd).
  * De bevestiging van een technische of de ACK CONTRL rapporteert de resultaten van een syntactische controle van de volledige ontvangen uitwisseling.
  * Een functionele bevestiging erkent accepteren of weigeren van een ontvangen uitwisseling of een groep

## <a name="view-swagger-file"></a>Swagger-bestand weergeven
De details Swagger voor de EDIFACT-connector, Zie [EDIFACT](/connectors/edifact/).

## <a name="next-steps"></a>Volgende stappen
[Meer informatie over het Enterprise Integration Pack](logic-apps-enterprise-integration-overview.md "meer informatie over Enterprise Integration Pack") 

