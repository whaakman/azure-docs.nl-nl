---
title: Codeer X12 berichten - Azure Logic Apps | Microsoft Docs
description: EDI valideren en converteren XML-gecodeerde berichten met X12 bericht-codering in Azure Logic Apps met Enterprise Integration Pack
services: logic-apps
ms.service: logic-apps
ms.suite: integration
author: divyaswarnkar
ms.author: divswa
ms.reviewer: jonfan, estfan, LADocs
ms.topic: article
ms.assetid: a01e9ca9-816b-479e-ab11-4a984f10f62d
ms.date: 01/27/2017
ms.openlocfilehash: 3ed5cb61fef5f07913f11c4e4df309d720d5b901
ms.sourcegitcommit: 2ad510772e28f5eddd15ba265746c368356244ae
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/28/2018
ms.locfileid: "43123502"
---
# <a name="encode-x12-messages-in-azure-logic-apps-with-enterprise-integration-pack"></a>Codeer X12 berichten in Azure Logic Apps met Enterprise Integration Pack

Met de connector van het bericht coderen X12, kunt u valideren EDI en partner-specifieke eigenschappen, berichten XML-indeling converteren naar EDI-transactiesets in de uitwisseling en aanvragen van een bevestiging technische, functionele bevestigingen of beide.
Voor het gebruik van deze connector, moet u de connector toevoegen aan een bestaande trigger in uw logische app.

## <a name="before-you-start"></a>Voordat u begint

Hier volgt de items die u nodig hebt:

* Een Azure-account; u kunt maken een [gratis account](https://azure.microsoft.com/free)
* Een [integratieaccount](logic-apps-enterprise-integration-create-integration-account.md) die al is gedefinieerd en die zijn gekoppeld aan uw Azure-abonnement. Hebt u een integratieaccount om de coderen X12 bericht-connector te gebruiken.
* Ten minste twee [partners](logic-apps-enterprise-integration-partners.md) die al zijn gedefinieerd in uw integratieaccount
* Een [X12 overeenkomst](logic-apps-enterprise-integration-x12.md) die al gedefinieerd in uw integratieaccount

## <a name="encode-x12-messages"></a>Codeer X12 berichten

1. [Maak een logische app](quickstart-create-first-logic-app-workflow.md).

2. Het bericht coderen X12 connector geen triggers, dus moet u een trigger voor het starten van uw logische app, zoals een aanvraag als trigger toevoegen. Een trigger toevoegen in de Logic App Designer en klikt u vervolgens een actie toevoegen aan uw logische app.

3.  Voer in het zoekvak 'x12' voor uw filter. Selecteer **X12-coderen naar X12 bericht met de naam van de overeenkomst** of **X12-coderen naar X12 bericht op identiteiten**.
   
    ![Zoek naar "x12"](./media/logic-apps-enterprise-integration-x12-encode/x12decodeimage1.png) 

3. Als u geen verbindingen voor uw integratieaccount eerder hebt gemaakt, wordt u gevraagd om nu deze verbinding te maken. Naam van de verbinding en selecteer de integratieaccount waarmee u verbinding wilt maken. 
   
    ![de verbinding van de integratie-account](./media/logic-apps-enterprise-integration-x12-encode/x12encodeimage1.png)

    Eigenschappen met een sterretje zijn vereist.

    | Eigenschap | Details |
    | --- | --- |
    | Verbindingsnaam * |Voer een naam voor de verbinding. |
    | Integratie-Account * |Voer een naam voor uw integratie-account. Zorg ervoor dat uw integratie-account en logica-app zich in dezelfde Azure-locatie. |

5.  Wanneer u klaar bent, moeten uw verbindingsgegevens lijken op het volgende voorbeeld. Kies voor het voltooien van het maken van uw verbinding, **maken**.

    ![integratie-accountverbinding hebt gemaakt](./media/logic-apps-enterprise-integration-x12-encode/x12encodeimage2.png)

    De verbinding is nu gemaakt.

    ![integratie-account-verbindingsgegevens](./media/logic-apps-enterprise-integration-x12-encode/x12encodeimage3.png) 

#### <a name="encode-x12-messages-by-agreement-name"></a>Codeer X12 berichten door de naam van de overeenkomst

Als u wilt coderen X12 berichten door de naam van de overeenkomst, open de **naam van X12 overeenkomst** lijst, invoeren of selecteert u uw bestaande X12 overeenkomst. Voer in het XML-bericht coderen.

![Voer X12 naam van de overeenkomst en XML-bericht coderen](./media/logic-apps-enterprise-integration-x12-encode/x12encodeimage4.png)

#### <a name="encode-x12-messages-by-identities"></a>Codeer X12 berichten op identiteiten

Als u wilt coderen X12 berichten op identiteiten, voer de afzender-id, de afzenderkwalificatie van de, ontvanger-id en ontvangerskwalificatie zoals geconfigureerd in uw X12 overeenkomst. Selecteer het XML-bericht coderen.
   
![Identiteiten voor een afzender en de ontvanger opgeven, selecteert u het XML-bericht coderen](./media/logic-apps-enterprise-integration-x12-encode/x12encodeimage5.png) 

## <a name="x12-encode-details"></a>X12 details coderen

De X12 coderen connector voert deze taken uit:

* De omzetting van de overeenkomst door die overeenkomt met de contexteigenschappen afzender en de ontvanger.
* Serialiseert het EDI-uitwisseling, conversie van XML-gecodeerde berichten naar EDI-transactiesets in de uitwisseling.
* Transactie set-header en aanhangwagen segmenten van toepassing is
* Genereert een controlenummer uitwisseling, een groepscontrolenummer en een controlenummer transactie voor elke uitgaande-uitwisseling
* Vervangt de scheidingstekens in de nettolading
* EDI- en partner-specifieke eigenschappen worden gevalideerd
  * Schemavalidatie van de gegevenselementen van transactie is ingesteld op basis van het bericht Schema
  * EDI-validatie wordt uitgevoerd op elementen transactieset gegevens.
  * Uitgebreide validatie wordt uitgevoerd op de transactieset gegevenselementen
* Een bevestiging technische en/of functionele-aanvragen (indien geconfigureerd).
  * Een technische bevestiging genereert als gevolg van validatie van de header. De technische bevestiging meldt de status van de verwerking van een uitwisseling-header en aanhangwagen door de ontvanger adres
  * Een functionele bevestiging genereert als gevolg van de hoofdtekst van de validatie. De functionele bevestiging meldt een fout opgetreden tijdens het verwerken van het document ontvangen

## <a name="view-the-swagger"></a>De swagger weergeven
Zie de [details swagger](/connectors/x12/). 

## <a name="next-steps"></a>Volgende stappen
[Meer informatie over het Enterprise Integration Pack](logic-apps-enterprise-integration-overview.md "meer informatie over Enterprise Integration Pack") 

