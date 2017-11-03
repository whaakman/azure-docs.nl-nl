---
title: 'Logic Apps B2B lijst met fouten en oplossingen: Azure App Service | Microsoft Docs'
description: Logic Apps B2B lijst met fouten en oplossingen
services: logic-apps
documentationcenter: .net,nodejs,java
author: padmavc
manager: anneta
editor: 
ms.assetid: cf44af18-1fe5-41d5-9e06-cc57a968207c
ms.service: logic-apps
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/02/2017
ms.author: LADocs; padmavc
ms.openlocfilehash: 1865d75f1b4c2aa18d5a3130f639572d19563b3e
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="logic-apps-b2b-list-of-errors-and-solutions"></a>Logic Apps B2B lijst met fouten en oplossingen  
In dit artikel helpt u bij het oplossen van fouten die mogelijk in Logic Apps B2B-scenario's en stelt de nodige acties voor het oplossen van deze fouten.


## <a name="agreement-resolution"></a>Omzetting van de overeenkomst

### <a name="no-agreement-found"></a>* Geen overeenkomst gevonden 

|   |   |  
|---|---|
| Foutbeschrijving | Er is geen overeenkomst gevonden met de overeenkomst resolutie Parameters|    
| Gebruikersactie | De overeenkomst moet worden toegevoegd aan het account van de integratie met overeengekomen business identiteiten.</br> De zakelijke identiteit moeten overeenkomen met de bericht-id 's|  
|   |   |

### <a name="-no-agreement-found-with-identities"></a>* Geen overeenkomst gevonden met identiteiten

|   |   | 
|---|---|
| Foutbeschrijving | Er is geen overeenkomst gevonden met identiteiten: 'AS2Identity':: 'Partner1' en 'AS2Identity':: 'Partner3'| 
| Gebruikersactie | Ongeldige AS2-uit of AS2-tot geconfigureerd voor de overeenkomst. </br> Juiste AS2 bericht AS2-uit of AS2-headers of overeenkomst moet worden gezocht AS2-id's in AS2-bericht headers met de configuraties van de overeenkomst |
|   |   |     

## <a name="as2"></a>AS2

### <a name="-missing-as2-message-headers"></a>* AS2 berichtkoppen ontbreekt  

|   |   |  
|---|---|
| Foutbeschrijving| Ongeldige AS2-headers. Een van de ' AS2-aan ' of ' AS2-van ' headers zijn leeg| 
| Gebruikersactie | Een AS2-bericht is ontvangen die niet de AS2 bevat-uit of AS2-te of beide headers. </br> Controleer AS2 bericht AS2-vanaf en AS2-berichtkoppen en juist ze op basis van overeenkomst configuratie |
|  |  | 


### <a name="-missing-as2-message-body-and-headers"></a>* Ontbrekende AS2 berichttekst en -koppen    

|   |   |  
|---|---|
| Foutbeschrijving| Inhoud van de aanvraag is null of leeg | 
| Gebruikersactie | Een AS2-bericht is ontvangen die niet de berichttekst bevat |
|  |  | 

### <a name="-as2-message-decryption-failure"></a>* Ontsleutelingsfout AS2-bericht

|   |   | 
|---|---|
| Foutbeschrijving |  [verwerkt/fout: ontsleuteling is mislukt] | 
| Gebruikersactie | Voeg @base64ToBinary naar AS2Message voordat wordt verzonden naar partner 
```java
            "HTTP": {
                "inputs": {
                    "body": "@base64ToBinary(body('Encode_to_AS2_message')?['AS2Message']?['Content'])",
                    "headers": "@body('Encode_to_AS2_message')?['AS2Message']?['OutboundHeaders']",
                    "method": "POST",
                    "uri": "xxxxx.xxx"
                },
                
``` 

### <a name="-mdn-decryption-failure"></a>* MDN ontsleutelingsfout

|   |   | 
|---|---|
| Foutbeschrijving |  [verwerkt/fout: ontsleuteling is mislukt] | 
| Gebruikersactie | Voeg @base64ToBinary naar MDN voordat wordt verzonden naar partner 
```java
            "Response": {
                "inputs": {
                    "body": "@base64ToBinary(body('Decode_AS2_message')?['OutgoingMDN']?['Content'])",
                    "headers": "@body('Decode_AS2_message')?['OutgoingMDN']?['OutboundHeaders']",
                    "statusCode": 200
                },
                
``` 

### <a name="-missing-signing-certificate"></a>* Handtekeningcertificaat ontbreekt

|   |   |  
|---|---|
| Foutbeschrijving| Het certificaat voor ondertekening is niet geconfigureerd voor AS2 partij. </br> AS2-vanaf: partner1 AS2-te: partner2 | 
| Gebruikersactie | AS2-overeenkomst instellingen configureren met het juiste certificaat voor handtekening |
|  |  | 

## <a name="x12-and-edifact"></a>X12 en EDIFACT

### <a name="-leading-or-trailing-space-found"></a>* Voorloop- of een afsluitende spatie gevonden    
    
|   |   | 
|---|---|
| Foutbeschrijving | Fout opgetreden tijdens het parseren. De transactie Edifact ingesteld met de id ' 123456 'opgenomen in interchange (zonder groep) met id ' 987654', met id 'Partner1' van de afzender, ontvanger-id 'Partner2' wordt onderbroken met de volgende fouten: voorloopspaties afsluitende scheidingsteken gevonden |
| Gebruikersactie | De instellingen van de overeenkomst moet worden geconfigureerd voor het toestaan van voorloopspaties en afsluitende spatie. </br> Instellingen om toe te staan voorloop- en volgspaties ruimte overeenkomst bewerken |
|   |   |

![ruimte](./media/logic-apps-enterprise-integration-b2b-list-errors-solutions/leadingandtrailing.png)

### <a name="-duplicate-check-has-enabled-in-the-agreement"></a>* Dubbele selectievakje is ingeschakeld in de overeenkomst

|   |   | 
|---|---| 
| Foutbeschrijving | Dubbele controle-aantal |
| Gebruikersactie | Deze fout geeft aan dat het ontvangen bericht heeft dubbele besturingselement cijfers. </br> Corrigeer het besturingselementnummer en het bericht te verzenden |
|   |   |

### <a name="-missing-schema-in-the-agreement"></a>* Ontbrekende schema in de overeenkomst

|   |   | 
|---|---| 
| Foutbeschrijving | Fout opgetreden tijdens het parseren. De X12 transactie set met id '564220001' opgenomen in functionele groep met id '56422', in het knooppunt met id '000056422' met afzender-id ' 12345678', ' 87654321-id van de ontvanger' wordt onderbroken met de volgende fouten 'in het bericht heeft een onbekend documenttype en het is niet omgezet naar een van de bestaande schema's zijn geconfigureerd in de overeenkomst' |
| Gebruikersactie | Schema in de overeenkomst-instellingen configureren  |
|   |   |

### <a name="-incorrect-schema-in-the-agreement"></a>* Onjuist schema in de overeenkomst

|   |   | 
|---|---| 
| Foutbeschrijving | Het bericht heeft een onbekend documenttype en is niet omgezet naar een van de bestaande schema's zijn geconfigureerd in de overeenkomst. |
| Gebruikersactie | Juiste schema in de overeenkomst-instellingen configureren  |
|   |   |

## <a name="flat-file"></a>Plat bestand

### <a name="-input-message-with-no-body"></a>* Invoerbericht bij geen instantie

|   |   | 
|---|---|
| Foutbeschrijving | InvalidTemplate. Kan de sjabloontaalexpressies proces in de invoer van de actie 'Flat_File_Decoding' op regel '1' en kolom '1902': ' eigenschap 'inhoud' verwacht een waarde maar kreeg null vereist. Pad '.'. |
| Gebruikersactie | Deze fout geeft aan dat het invoerbericht bevat geen hoofdtekst |
|   |   | 

## <a name="learn-more"></a>Meer informatie
[Meer informatie over het Enterprise-integratiepakket](logic-apps-enterprise-integration-overview.md)