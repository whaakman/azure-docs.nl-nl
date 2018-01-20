---
title: Codeer EDIFACT berichten - Azure Logic Apps | Microsoft Docs
description: EDI valideren en genereren van XML met EDIFACT berichtencoder in de Enterprise-integratiepakket voor Azure Logic Apps
services: logic-apps
documentationcenter: .net,nodejs,java
author: padmavc
manager: anneta
editor: 
ms.assetid: 974ac339-d97a-4715-bc92-62d02281e900
ms.service: logic-apps
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/27/2017
ms.author: LADocs; padmavc
ms.openlocfilehash: 799d444632b67788520be8a777ec656076022583
ms.sourcegitcommit: be9a42d7b321304d9a33786ed8e2b9b972a5977e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/19/2018
---
# <a name="encode-edifact-messages-for-azure-logic-apps-with-the-enterprise-integration-pack"></a>EDIFACT-berichten te coderen voor Azure Logic Apps met de Enterprise-integratiepakket

Met de connector van het bericht EDIFACT coderen, kunt u valideren EDI- en partner-specifieke eigenschappen, een XML-document voor elke set transactie genereren en aanvragen van een bevestiging van technische, functionele bevestiging of beide.
Voor het gebruik van deze connector, moet u de connector toevoegen aan een bestaande trigger in uw logische app.

## <a name="before-you-start"></a>Voordat u begint

Hier volgt de items die u nodig:

* Een Azure-account; kunt u een [gratis account](https://azure.microsoft.com/free)
* Een [integratie account](logic-apps-enterprise-integration-create-integration-account.md) die al is gedefinieerd en die zijn gekoppeld aan uw Azure-abonnement. U moet een integratie-account aan het gebruik van de connector coderen EDIFACT-bericht hebben. 
* Ten minste twee [partners](logic-apps-enterprise-integration-partners.md) die al zijn gedefinieerd in uw account integratie
* Een [EDIFACT overeenkomst](logic-apps-enterprise-integration-edifact.md) die al is gedefinieerd in uw account integratie

## <a name="encode-edifact-messages"></a>EDIFACT-berichten te coderen

1. [Een logische app maken](quickstart-create-first-logic-app-workflow.md).

2. Het bericht coderen EDIFACT connector geen triggers, dus moet u een trigger voor het starten van uw logische app, zoals een aanvraag-trigger toevoegen. In de ontwerpfunctie voor Logic App een trigger toevoegen en vervolgens een actie toevoegen aan uw logische app.

3.  Voer in het zoekvak 'EDIFACT' als filter. Selecteer een **coderen EDIFACT bericht met de Overeenkomstnaam** of **coderen EDIFACT-bericht met identiteiten**.
   
    ![EDIFACT zoeken](media/logic-apps-enterprise-integration-edifact-encode/edifactdecodeimage1.png)  

3. Als u geen verbindingen tot je account integratie eerder hebt gemaakt, wordt u gevraagd nu die verbinding maken. Naam van uw verbinding en selecteer de integratie-account waarmee u verbinding wilt maken.

    ![integratie-account verbinding maken](media/logic-apps-enterprise-integration-edifact-encode/edifactencodeimage1.png)  

    Eigenschappen met een sterretje zijn vereist.

    | Eigenschap | Details |
    | --- | --- |
    | Verbindingsnaam * |Voer een naam voor de verbinding. |
    | Integratie Account * |Voer een naam voor uw account integratie. Zorg ervoor dat uw account en logica app voor integratie in dezelfde Azure-locatie. |

5.  Wanneer u bent klaar, de verbindingsdetails van uw moeten er ongeveer uitzien in dit voorbeeld. Voor het voltooien van de verbinding te maken, kies **maken**.

    ![de accountdetails verbinding integratie](media/logic-apps-enterprise-integration-edifact-encode/edifactencodeimage2.png)

    De verbinding wordt nu gemaakt.

    ![integratie-account verbinding is gemaakt](media/logic-apps-enterprise-integration-edifact-encode/edifactencodeimage4.png)

#### <a name="encode-edifact-message-by-agreement-name"></a>EDIFACT-bericht met de Overeenkomstnaam coderen

Als u hebt gekozen voor het coderen van EDIFACT berichten door de naam van de overeenkomst, opent u de **overeenkomst met de naam van EDIFACT** lijst, typ of Selecteer de naam van de overeenkomst EDIFACT. Voer in het XML-bericht voor het coderen.

![Voer de naam van de overeenkomst EDIFACT en XML-bericht voor het coderen van](media/logic-apps-enterprise-integration-edifact-encode/edifactencodeimage6.png)

#### <a name="encode-edifact-message-by-identities"></a>EDIFACT-bericht met identiteiten coderen

Als u kiest voor het coderen van EDIFACT berichten door identiteiten, voert u de afzender-id, kwalificatie afzender, ontvanger-id en ontvanger kwalificatie zoals geconfigureerd in uw EDIFACT-overeenkomst. Selecteer het XML-bericht voor het coderen.

![Geef identiteiten voor een afzender en ontvanger, selecteer van XML-bericht voor het coderen van](media/logic-apps-enterprise-integration-edifact-encode/edifactencodeimage7.png)

## <a name="edifact-encode-details"></a>Details EDIFACT coderen

De connector coderen EDIFACT voert deze taken uit: 

* De overeenkomst oplossen door de afzender kwalificatie & -id en ontvanger kwalificatie en id overeenkomende
* De EDI-uitwisseling, conversie van XML-codering berichten naar EDI transactie sets in de uitwisseling serialiseert.
* Transactie set-header en trailer segmenten van toepassing is
* Genereren van een besturingselement interchange getal, een besturingselement groepsnummer en een getal voor het instellen van transactie voor elke uitgaande uitwisseling
* Vervangt scheidingstekens in de nettolading
* Valideert EDI en partner-specifieke eigenschappen
  * Schemavalidatie van de gegevenselementen van de transactie-set tegen het berichtschema.
  * EDI gevalideerd gegevenselementen transactie-set.
  * Uitgebreide validatie uitgevoerd op van de transactie-set-gegevenselementen
* Genereert een XML-document voor elke set transactie.
* Vraagt om bevestiging van de technische en/of functionele (indien geconfigureerd).
  * Als een technische bevestiging geeft het bericht CONTRL ontvangst van een knooppunt aan.
  * Als een functionele bevestiging geeft het bericht CONTRL aan acceptatie- of afwijzing van de ontvangen interchange, groep of een bericht met een lijst met fouten of niet-ondersteunde functies

## <a name="view-swagger-file"></a>Swagger-bestand weergeven
De Swagger-details voor de connector EDIFACT Zie [EDIFACT](/connectors/edifact/).

## <a name="next-steps"></a>Volgende stappen
[Meer informatie over het Enterprise-integratiepakket](logic-apps-enterprise-integration-overview.md "meer informatie over Enterprise Integration Pack") 

