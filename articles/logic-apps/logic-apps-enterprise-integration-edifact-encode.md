---
title: Codeer EDIFACT-berichten - Azure Logic Apps | Microsoft Docs
description: EDI valideren en genereren van XML met EDIFACT-berichtencoder voor Azure Logic Apps met Enterprise Integration Pack
services: logic-apps
ms.service: logic-apps
ms.suite: integration
author: divyaswarnkar
ms.author: divswa
ms.reviewer: jonfan, estfan, LADocs
ms.topic: article
ms.assetid: 974ac339-d97a-4715-bc92-62d02281e900
ms.date: 01/27/2017
ms.openlocfilehash: e1c990030a9fa9ad25950ccb24b36b82a8c69f5c
ms.sourcegitcommit: 2ad510772e28f5eddd15ba265746c368356244ae
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/28/2018
ms.locfileid: "43122350"
---
# <a name="encode-edifact-messages-for-azure-logic-apps-with-enterprise-integration-pack"></a>EDIFACT-berichten coderen voor Azure Logic Apps met Enterprise Integration Pack

Met de coderen EDIFACT-bericht-connector, kunt u valideren EDI en partner-specifieke eigenschappen, een XML-document voor elke transactieset genereren en aanvragen van een bevestiging technische, functionele bevestigingen of beide.
Voor het gebruik van deze connector, moet u de connector toevoegen aan een bestaande trigger in uw logische app.

## <a name="before-you-start"></a>Voordat u begint

Hier volgt de items die u nodig hebt:

* Een Azure-account; u kunt maken een [gratis account](https://azure.microsoft.com/free)
* Een [integratieaccount](logic-apps-enterprise-integration-create-integration-account.md) die al is gedefinieerd en die zijn gekoppeld aan uw Azure-abonnement. Hebt u een integratieaccount om de coderen EDIFACT-bericht-connector te gebruiken. 
* Ten minste twee [partners](logic-apps-enterprise-integration-partners.md) die al zijn gedefinieerd in uw integratieaccount
* Een [EDIFACT-overeenkomst](logic-apps-enterprise-integration-edifact.md) die al gedefinieerd in uw integratieaccount

## <a name="encode-edifact-messages"></a>EDIFACT-berichten coderen

1. [Maak een logische app](quickstart-create-first-logic-app-workflow.md).

2. De connector coderen EDIFACT-bericht geen triggers, dus moet u een trigger voor het starten van uw logische app, zoals een aanvraag als trigger toevoegen. Een trigger toevoegen in de Logic App Designer en klikt u vervolgens een actie toevoegen aan uw logische app.

3.  Typ 'EDIFACT' als filter in het zoekvak. Selecteer een **EDIFACT-bericht coderen op Overeenkomstnaam** of **coderen op identiteiten EDIFACT-bericht**.
   
    ![EDIFACT zoeken](media/logic-apps-enterprise-integration-edifact-encode/edifactdecodeimage1.png)  

3. Als u geen verbindingen voor uw integratieaccount eerder hebt gemaakt, wordt u gevraagd om nu deze verbinding te maken. Naam van de verbinding en selecteer de integratieaccount waarmee u verbinding wilt maken.

    ![integratie-accountverbinding maken](media/logic-apps-enterprise-integration-edifact-encode/edifactencodeimage1.png)  

    Eigenschappen met een sterretje zijn vereist.

    | Eigenschap | Details |
    | --- | --- |
    | Verbindingsnaam * |Voer een naam voor de verbinding. |
    | Integratie-Account * |Voer een naam voor uw integratie-account. Zorg ervoor dat uw integratie-account en logica-app zich in dezelfde Azure-locatie. |

5.  Wanneer u klaar bent, moeten uw verbindingsgegevens lijken op het volgende voorbeeld. Kies voor het voltooien van het maken van uw verbinding, **maken**.

    ![integratie-account-verbindingsgegevens](media/logic-apps-enterprise-integration-edifact-encode/edifactencodeimage2.png)

    De verbinding is nu gemaakt.

    ![integratie-accountverbinding hebt gemaakt](media/logic-apps-enterprise-integration-edifact-encode/edifactencodeimage4.png)

#### <a name="encode-edifact-message-by-agreement-name"></a>EDIFACT-bericht coderen op Overeenkomstnaam

Als u ervoor kiest de EDIFACT-berichten coderen op Overeenkomstnaam, opent u de **naam van de EDIFACT-overeenkomst** lijst Typ of Selecteer de naam van de EDIFACT-overeenkomst. Voer in het XML-bericht coderen.

![Voer de naam van de EDIFACT-overeenkomst en XML-bericht coderen](media/logic-apps-enterprise-integration-edifact-encode/edifactencodeimage6.png)

#### <a name="encode-edifact-message-by-identities"></a>EDIFACT-bericht coderen op identiteiten

Als u ervoor kiest de EDIFACT-berichten coderen op identiteiten, voert u de afzender-id, de afzenderkwalificatie van de, ontvanger-id en ontvangerskwalificatie zoals geconfigureerd in het EDIFACT-overeenkomst. Selecteer het XML-bericht coderen.

![Identiteiten voor een afzender en de ontvanger opgeven, selecteert u het XML-bericht coderen](media/logic-apps-enterprise-integration-edifact-encode/edifactencodeimage7.png)

## <a name="edifact-encode-details"></a>Details van EDIFACT-codering

De connector coderen EDIFACT voert deze taken uit: 

* Oplossen van de overeenkomst door die overeenkomt met de afzenderkwalificatie & id en ontvangerskwalificatie en -id
* Serialiseert het EDI-uitwisseling, conversie van XML-gecodeerde berichten naar EDI-transactiesets in de uitwisseling.
* Transactie set-header en aanhangwagen segmenten van toepassing is
* Genereert een controlenummer uitwisseling, een groepscontrolenummer en een controlenummer transactie voor elke uitgaande-uitwisseling
* Vervangt de scheidingstekens in de nettolading
* EDI- en partner-specifieke eigenschappen worden gevalideerd
  * Schemavalidatie van de gegevenselementen van de transactie-set op basis van de berichtschema.
  * EDI-validatie wordt uitgevoerd op elementen transactieset gegevens.
  * Uitgebreide validatie wordt uitgevoerd op de transactieset gegevenselementen
* Genereert een XML-document voor elke transactieset.
* Een bevestiging technische en/of functionele-aanvragen (indien geconfigureerd).
  * Als een bevestiging van technische geeft het bericht CONTRL ontvangst van een knooppunt.
  * Als een functionele bevestiging geeft het bericht CONTRL aan acceptatie- of weigering van de ontvangen uitwisseling, groep of een bericht met een lijst met fouten of niet-ondersteunde functies

## <a name="view-swagger-file"></a>Swagger-bestand weergeven
De details Swagger voor de EDIFACT-connector, Zie [EDIFACT](/connectors/edifact/).

## <a name="next-steps"></a>Volgende stappen
[Meer informatie over het Enterprise Integration Pack](logic-apps-enterprise-integration-overview.md "meer informatie over Enterprise Integration Pack") 

