---
title: X12 decoderen berichten - Azure Logic Apps | Microsoft Docs
description: EDI valideren en het genereren van bevestigingen met X12 bericht decoder in Azure Logic Apps met Enterprise Integration Pack
services: logic-apps
ms.service: logic-apps
ms.suite: integration
author: divyaswarnkar
ms.author: divswa
ms.reviewer: jonfan, estfan, LADocs
ms.topic: article
ms.assetid: 4fd48d2d-2008-4080-b6a1-8ae183b48131
ms.date: 01/27/2017
ms.openlocfilehash: e3d2a458c2cece5e3f01fdb9e3d403b3fb78dd2b
ms.sourcegitcommit: 2ad510772e28f5eddd15ba265746c368356244ae
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/28/2018
ms.locfileid: "43121642"
---
# <a name="decode-x12-messages-in-azure-logic-apps-with-enterprise-integration-pack"></a>X12 decoderen berichten in Azure Logic Apps met Enterprise Integration Pack

Met de connector van het bericht decoderen X12, kunt u valideren van de envelop tegen een handelspartnerovereenkomst, valideren EDI en partner-specifieke eigenschappen uitwisselingen in transacties sets splitsen of hele uitwisselingen behouden en genereren van bevestigingen voor verwerkte transacties. Voor het gebruik van deze connector, moet u de connector toevoegen aan een bestaande trigger in uw logische app.

## <a name="before-you-start"></a>Voordat u begint

Hier volgt de items die u nodig hebt:

* Een Azure-account; u kunt maken een [gratis account](https://azure.microsoft.com/free)
* Een [integratieaccount](logic-apps-enterprise-integration-create-integration-account.md) die al is gedefinieerd en die zijn gekoppeld aan uw Azure-abonnement. Hebt u een integratieaccount om het bericht decoderen X12 connector te gebruiken.
* Ten minste twee [partners](logic-apps-enterprise-integration-partners.md) die al zijn gedefinieerd in uw integratieaccount
* Een [X12 overeenkomst](logic-apps-enterprise-integration-x12.md) die al gedefinieerd in uw integratieaccount

## <a name="decode-x12-messages"></a>X12 decoderen berichten

1. [Maak een logische app](quickstart-create-first-logic-app-workflow.md).

2. Het bericht decoderen X12 connector geen triggers, dus moet u een trigger voor het starten van uw logische app, zoals een aanvraag als trigger toevoegen. Een trigger toevoegen in de Logic App Designer en klikt u vervolgens een actie toevoegen aan uw logische app.

3.  Voer in het zoekvak 'x12' voor uw filter. Selecteer **X12-decoderen X12 bericht**.
   
    ![Zoek naar "x12"](media/logic-apps-enterprise-integration-x12-decode/x12decodeimage1.png)  

3. Als u geen verbindingen voor uw integratieaccount eerder hebt gemaakt, wordt u gevraagd om nu deze verbinding te maken. Naam van de verbinding en selecteer de integratieaccount waarmee u verbinding wilt maken. 

    ![Geef de integratie van accountgegevens voor de verbinding](media/logic-apps-enterprise-integration-x12-decode/x12decodeimage4.png)

    Eigenschappen met een sterretje zijn vereist.

    | Eigenschap | Details |
    | --- | --- |
    | Verbindingsnaam * |Voer een naam voor de verbinding. |
    | Integratie-Account * |Voer een naam voor uw integratie-account. Zorg ervoor dat uw integratie-account en logica-app zich in dezelfde Azure-locatie. |

5.  Wanneer u klaar bent, moeten uw verbindingsgegevens lijken op het volgende voorbeeld. Kies voor het voltooien van het maken van uw verbinding, **maken**.
   
    ![integratie-account-verbindingsgegevens](media/logic-apps-enterprise-integration-x12-decode/x12decodeimage5.png) 

6. Nadat de verbinding is gemaakt, zoals wordt weergegeven in dit voorbeeld, selecteer de X12 plat bestand bericht moet worden gedecodeerd.

    ![integratie-accountverbinding hebt gemaakt](media/logic-apps-enterprise-integration-x12-decode/x12decodeimage6.png) 

    Bijvoorbeeld:

    ![Selecteer X12 platte bericht voor het decoderen van bestand](media/logic-apps-enterprise-integration-x12-decode/x12decodeimage7.png) 

   > [!NOTE]
   > De inhoud van het werkelijke bericht of de nettolading voor de bericht-matrix, goed of slecht, is base64-gecodeerd. Daarom moet u een expressie die deze inhoud verwerkt.
   > Hier volgt een voorbeeld waarin de inhoud als XML-bestand dat u opgeven kunt in de codeweergave of verwerkt met behulp van de opbouwfunctie voor expressies in de ontwerpfunctie.
   > ``` json
   > "content": "@xml(base64ToBinary(item()?['Payload']))"
   > ```
   > ![Voorbeeld van de inhoud](media/logic-apps-enterprise-integration-x12-decode/content-example.png)
   >


## <a name="x12-decode-details"></a>X12 details decoderen

De X12 decoderen connector voert deze taken uit:

* Valideert de envelop tegen trading partner agreement
* EDI- en partner-specifieke eigenschappen worden gevalideerd
  * Structurele EDI-validatie, en uitgebreide schemavalidatie
  * Validatie van de structuur van de enveloppe uitwisseling.
  * Schemavalidatie van de enveloppe op basis van het schema van het besturingselement.
  * Schemavalidatie van de gegevenselementen van de transactie-set op basis van de berichtschema.
  * EDI-validatie uitgevoerd voor de transactieset gegevenselementen 
* Controleert of de uitwisseling, groep en transactie set controlenummers zijn niet identiek
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
* Genereert een bevestiging technische en/of functionele (indien geconfigureerd).
  * Een technische bevestiging genereert als gevolg van validatie van de header. De technische bevestiging meldt de status van de verwerking van een uitwisseling-header en aanhangwagen door de ontvanger van het adres.
  * Een functionele bevestiging genereert als gevolg van de hoofdtekst van de validatie. De functionele bevestiging meldt een fout opgetreden tijdens het verwerken van het document ontvangen

## <a name="view-the-swagger"></a>De swagger weergeven
Zie de [details swagger](/connectors/x12/). 

## <a name="next-steps"></a>Volgende stappen
[Meer informatie over het Enterprise Integration Pack](../logic-apps/logic-apps-enterprise-integration-overview.md "meer informatie over Enterprise Integration Pack") 

