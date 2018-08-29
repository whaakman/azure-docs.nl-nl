---
title: Fouten en oplossingen voor B2B-scenario's - Azure Logic Apps | Microsoft Docs
description: Zoeken naar fouten en oplossingen voor B2B-scenario's in Azure Logic Apps
services: logic-apps
ms.service: logic-apps
ms.suite: integration
author: divyaswarnkar
ms.author: divswa
ms.reviewer: jonfan, estfan, LADocs
ms.topic: article
ms.date: 06/02/2017
ms.openlocfilehash: 11fbec81e88eec6c7daa9136eb5421387b79d71c
ms.sourcegitcommit: 2ad510772e28f5eddd15ba265746c368356244ae
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/28/2018
ms.locfileid: "43124331"
---
# <a name="b2b-errors-and-solutions-for-azure-logic-apps"></a>B2B-fouten en oplossingen voor Azure Logic Apps

Dit artikel helpt bij het oplossen van fouten die mogelijk in Logic Apps B2B-scenario's en stelt de nodige acties voor het corrigeren van deze fouten.

## <a name="agreement-resolution"></a>Omzetting van de overeenkomst

### <a name="no-agreement-found"></a>Er is geen overeenkomst gevonden 

|   |   |  
|---|---|
| Foutbeschrijving | Er is geen overeenkomst gevonden met de overeenkomst resolutie Parameters. | 
| Actie van de gebruiker | De overeenkomst moet worden toegevoegd aan het integratieaccount met de overeengekomen bedrijfsidentiteiten. </br>De bedrijfsidentiteiten moeten overeenkomen met het invoer bericht-id's. |  
|   |   |

### <a name="no-agreement-found-with-identities"></a>Er is geen overeenkomst gevonden met identiteiten

|   |   | 
|---|---|
| Foutbeschrijving | Er is geen overeenkomst gevonden met identiteiten: 'AS2Identity':: 'Partner1' en 'AS2Identity':: 'Partner3' | 
| Actie van de gebruiker | Ongeldige AS2-uit of AS2-naar geconfigureerd voor de overeenkomst. </br>Corrigeer het AS2-bericht "AS2-uit" of "AS2-naar" headers of de overeenkomst zodat deze overeenkomen met de AS2-id's in de berichtkoppen AS2 met configuraties van de overeenkomst. |
|   |   |     

## <a name="as2"></a>AS2

### <a name="missing-as2-message-headers"></a>Ontbrekende headers voor AS2-bericht  

|   |   |  
|---|---|
| Foutbeschrijving | Ongeldige AS2-headers. Een van de ' AS2-naar ' of ' AS2-uit "headers is leeg. | 
| Actie van de gebruiker | Een AS2-bericht is ontvangen dat geen AS2-uit of AS2-naar of beide headers. </br> Controleren van de AS2-bericht AS2-uit en AS2-headers en juiste ze op basis van overeenkomst configuratie. |
|  |  | 

### <a name="missing-as2-message-body-and-headers"></a>Ontbrekende hoofdtekst van de AS2-bericht en headers    

|   |   |  
|---|---|
| Foutbeschrijving | De aanvraaginhoud is null of leeg. | 
| Actie van de gebruiker | Een AS2-bericht is ontvangen dat geen hoofdtekst van het bericht. |
|  |  | 

### <a name="as2-message-decryption-failure"></a>Ontsleutelingsfout AS2-bericht

|   |   | 
|---|---|
| Foutbeschrijving |  [verwerkt/fout: het ontsleutelen is mislukt] | 
| Actie van de gebruiker | Voeg @base64ToBinary naar AS2Message voordat ze worden verzonden naar partner. |
|||

Bijvoorbeeld:

```json
"HTTP": {
   "inputs": {
   "body": "@base64ToBinary(body('Encode_to_AS2_message')?['AS2Message']?['Content'])",
   "headers": "@body('Encode_to_AS2_message')?['AS2Message']?['OutboundHeaders']",
   "method": "POST",
   "uri": "xxxxx.xxx"
},
``` 

### <a name="mdn-decryption-failure"></a>MDN ontsleutelen mislukt

|   |   | 
|---|---|
| Foutbeschrijving |  [verwerkt/fout: het ontsleutelen is mislukt] | 
| Actie van de gebruiker | Voeg @base64ToBinary naar MDN voordat ze worden verzonden naar partner. | 
|||

Bijvoorbeeld:

```json
"Response": {
   "inputs": {
   "body": "@base64ToBinary(body('Decode_AS2_message')?['OutgoingMDN']?['Content'])",
   "headers": "@body('Decode_AS2_message')?['OutgoingMDN']?['OutboundHeaders']",
   "statusCode": 200
},               
``` 

### <a name="missing-signing-certificate"></a>Ontbrekend certificaat voor ondertekening

|   |   |  
|---|---|
| Foutbeschrijving| Het certificaat voor ondertekening is niet geconfigureerd voor AS2-partij. </br>AS2-uit: partner1 AS2-naar: partner2 | 
| Actie van de gebruiker | AS2-overeenkomst instellingen configureren met het juiste certificaat voor ondertekening. |
|  |  | 

## <a name="x12-and-edifact"></a>X12 en EDIFACT

### <a name="leading-or-trailing-space-found"></a>Er is een voorloop- of volgspatie gevonden    
    
|   |   | 
|---|---|
| Foutbeschrijving | Er is een fout opgetreden tijdens het parseren. De EDIFACT-transactieset instellen met de ID ' 123456 'opgenomen in de uitwisseling (zonder groep) met ID ' 987654', met afzender-ID 'Partner1', 'Partner2' van de ontvanger-ID is geblokkeerd met de volgende fouten: <p>"Toonaangevende Trailing scheidingsteken gevonden" |
| Actie van de gebruiker | De instellingen van de overeenkomst moet worden geconfigureerd om toe te staan voorloopspaties en afsluitende spatie. </br>De instellingen van de overeenkomst om toe te staan voorloop- en volgspaties ruimte bewerken. |
|   |   |

![ruimte](./media/logic-apps-enterprise-integration-b2b-list-errors-solutions/leadingandtrailing.png)

### <a name="duplicate-check-has-enabled-in-the-agreement"></a>Dubbele selectievakje is ingeschakeld in de overeenkomst

|   |   | 
|---|---| 
| Foutbeschrijving | Dubbel controlenummer |
| Actie van de gebruiker | Deze fout geeft aan dat het ontvangen bericht heeft dubbele controlenummers. </br>Corrigeer het besturingselementnummer en het bericht te verzenden. |
|   |   |

### <a name="missing-schema-in-the-agreement"></a>Ontbrekende schema in de overeenkomst

|   |   | 
|---|---| 
| Foutbeschrijving | Er is een fout opgetreden tijdens het parseren. De X12 transactieset met ID '564220001' die zijn opgenomen in de functiegroep met ID '56422', in de uitwisseling met ID '000056422', met afzender-ID ' 12345678', ontvanger-ID ' 87654321' is geblokkeerd met de volgende fouten: <p>'Het bericht heeft een onbekend documenttype en is niet omgezet in een van de bestaande schema's geconfigureerd in de overeenkomst' |
| Actie van de gebruiker | Schema in de overeenkomst-instellingen configureren.  |
|   |   |

### <a name="incorrect-schema-in-the-agreement"></a>Onjuist schema in de overeenkomst

|   |   | 
|---|---| 
| Foutbeschrijving | Het bericht bevat een onbekend documenttype en is niet omgezet in een van de bestaande schema's die in de overeenkomst zijn geconfigureerd. |
| Actie van de gebruiker | Juiste schema in de overeenkomst-instellingen configureren. |
|   |   |

## <a name="flat-file"></a>Plat bestand

### <a name="input-message-with-no-body"></a>Invoerbericht met geen hoofdtekst

|   |   | 
|---|---|
| Foutbeschrijving | InvalidTemplate. Kan de sjabloontaalexpressies proces in actie-invoer voor 'Flat_File_Decoding' op de regel '1' en '1902' kolom: ' vereiste eigenschap 'inhoud' wordt verwacht dat een waarde, maar is null. Pad '.'. |
| Actie van de gebruiker | Deze fout geeft aan dat het invoerbericht een instantie niet bevat. |
|   |   | 

