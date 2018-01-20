---
title: Limieten en -configuratie - Azure Logic Apps | Microsoft Docs
description: Service-limieten en configuratiewaarden voor Azure Logic Apps
services: logic-apps
documentationcenter: .net,nodejs,java
author: jeffhollan
manager: anneta
editor: 
ms.assetid: 75b52eeb-23a7-47dd-a42f-1351c6dfebdc
ms.service: logic-apps
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/25/2017
ms.author: LADocs; jehollan
ms.openlocfilehash: 22d0ee242d18d73d1d5825567fd61638fd22cc68
ms.sourcegitcommit: be9a42d7b321304d9a33786ed8e2b9b972a5977e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/19/2018
---
# <a name="logic-apps-limits-and-configuration"></a>Logic Apps en -configuratie

Dit onderwerp beschrijft de huidige limieten en de configuratie-informatie voor Azure Logic Apps.

## <a name="limits"></a>Limieten

### <a name="http-request-limits"></a>Limieten voor HTTP-aanvraag

Deze beperkingen gelden voor één HTTP-aanvraag of een aanroep van de connector.

#### <a name="timeout"></a>Time-out

| Naam | Limiet | Opmerkingen | 
| ---- | ----- | ----- | 
| Time-out van de aanvraag | 120 seconden | Een [asynchrone patroon](../logic-apps/logic-apps-create-api-app.md) of [tot lus](logic-apps-loops-and-scopes.md) kunt compenseren indien nodig |
|||| 

#### <a name="message-size"></a>Berichtgrootte

| Naam | Limiet | Opmerkingen | 
| ---- | ----- | ----- | 
| Berichtgrootte | 100 MB | Sommige connectors en API's mogelijk geen ondersteuning voor 100 MB. | 
| Expressie evaluatie limiet | 131.072 tekens | `@concat()`, `@base64()`, `string` mag niet langer zijn dan deze limiet. | 
|||| 

#### <a name="retry-policy"></a>Beleid voor opnieuw proberen

| Naam | Limiet | Opmerkingen | 
| ---- | ----- | ----- | 
| Nieuwe pogingen | 90 | De standaardwaarde is 4. U kunt configureren met de [parameter van het beleid opnieuw](../logic-apps/logic-apps-workflow-actions-triggers.md). | 
| Maximale tijd tussen elke poging | 1 dag | U kunt configureren met de [parameter van het beleid opnieuw](../logic-apps/logic-apps-workflow-actions-triggers.md). | 
| Tijd tussen elke poging min | 5 per seconde | U kunt configureren met de [parameter van het beleid opnieuw](../logic-apps/logic-apps-workflow-actions-triggers.md). |
|||| 

### <a name="run-duration-and-retention"></a>Voer duur en retentie

Deze beperkingen gelden voor een enkele logische-app uitvoeren.

| Naam | Limiet | 
| ---- | ----- | 
| Duur uitvoering | 90 dagen | 
| Bewaren van opslag | Begintijd van 90 dagen van de uitvoeren | 
| Terugkeerpatroon min | 1 seconde </br>Voor logic apps met een App Service-Plan: 15 seconden | 
| Maximale terugkeerpatroon | 500 dagen | 
||| 

Overschrijden voor uitvoering duur of handhaving van opslag in uw flow normale verwerking [contact met ons opnemen](mailto://logicappsemail@microsoft.com) zodat we bij uw behoeften helpen kan.

### <a name="looping-and-debatching-limits"></a>Lussen en limieten debatching

Deze beperkingen gelden voor een enkele logische-app uitvoeren.

| Naam | Limiet | Opmerkingen | 
| ---- | ----- | ----- | 
| ForEach-items | 100,000 | U kunt de [query actie](../connectors/connectors-native-query.md) voor het filteren van grotere matrices indien nodig. | 
| Pas iteraties | 5,000 | | 
| SplitOn items | 100,000 | | 
| ForEach parallelle uitvoering | 50 | De standaardwaarde is 20. <p>Om in te stellen van een specifiek niveau van parallelle uitvoering in een lus ForEach de `runtimeConfiguration` eigenschap in de `foreach` in te grijpen. <p>Als u wilt een lus ForEach sequentieel worden uitgevoerd, stelt de `operationOptions` eigenschap in op 'Sequentiële' in de `foreach` actie. | 
|||| 

### <a name="throughput-limits"></a>Doorvoerlimieten

Deze beperkingen gelden voor een enkele logic app-exemplaar.

| Naam | Limiet | Opmerkingen | 
| ---- | ----- | ----- | 
| Acties uitvoeringen per 5 minuten | 100,000 | Werkbelasting kan worden verdelen over meerdere apps naar behoefte. | 
| Acties gelijktijdige uitgaande oproepen | ~2,500 | Verminder het aantal gelijktijdige aanvragen of verkorten indien nodig. | 
| Runtime-eindpunt: gelijktijdige binnenkomende oproepen | ~1,000 | Verminder het aantal gelijktijdige aanvragen of verkorten indien nodig. | 
| Runtime-eindpunt: aanroepen per 5 minuten lezen | 60,000 | Werkbelasting kan worden verdelen over meerdere apps naar behoefte. | 
| Runtime-eindpunt: aanroepen aanroepen per 5 minuten | 45,000 | Werkbelasting kan worden verdelen over meerdere apps naar behoefte. | 
|||| 

Aan deze beperkingen in de normale verwerking of voer load testen die mogelijk groter is dan deze limiet overschrijden [contact met ons opnemen](mailto://logicappsemail@microsoft.com) zodat we bij uw behoeften helpen kan.

### <a name="logic-app-definition-limits"></a>Limieten voor Logic app-definitie

Deze beperkingen gelden voor een definitie van één logic Apps.

| Naam | Limiet | Opmerkingen | 
| ---- | ----- | ----- | 
| Acties per werkstroom | 500 | Om deze limiet uitbreidt, kunt u geneste werkstromen toevoegen, indien nodig. |
| Diepte van het nesten actie toegestaan | 8 | Om deze limiet uitbreidt, kunt u geneste werkstromen toevoegen, indien nodig. | 
| Werkstromen per regio per abonnement | 1000 | | 
| Triggers per werkstroom | 10 | | 
| Switch bereik gevallen limiet | 25 | | 
| Aantal variabelen per werkstroom | 250 | | 
| Maximum aantal tekens per expressie | 8.192 | | 
| Maximale `trackedProperties` grootte in tekens | 16,000 | 
| `action`/`trigger`limiet voor de naam | 80 | | 
| `description`lengtelimiet | 256 | | 
| `parameters`limiet | 50 | | 
| `outputs`limiet | 10 | | 
|||| 

<a name="custom-connector-limits"></a>

### <a name="custom-connector-limits"></a>Aangepaste connector limieten

Deze beperkingen gelden voor aangepaste connectors die u van web-API's maken kunt.

| Naam | Limiet | 
| ---- | ----- | 
| Het aantal aangepaste connectors dat u kunt maken | 1000 per Azure-abonnement | 
| Het aantal aanvragen per minuut voor elke verbinding die tot stand is gebracht door een aangepaste connector | 500 aanvragen voor elke verbinding die tot stand is gebracht door de connector |
||| 

### <a name="integration-account-limits"></a>Limieten van integratie

Deze beperkingen gelden voor artefacten die u aan een account voor de integratie toevoegen kunt.

| Naam | Limiet | Opmerkingen | 
| ---- | ----- | ----- | 
| Schema | 8 MB | U kunt [blob-URI](../logic-apps/logic-apps-enterprise-integration-schemas.md) voor het uploaden van bestanden groter dan 2 MB. | 
| Kaart (XSLT-bestand) | 2 MB | | 
| Runtime-eindpunt: aanroepen per 5 minuten lezen | 60,000 | De werkbelasting kan worden verdelen over meerdere accounts, indien nodig. | 
| Runtime-eindpunt: aanroepen aanroepen per 5 minuten | 45,000 | De werkbelasting kan worden verdelen over meerdere accounts, indien nodig. | 
| Runtime-eindpunt: bijhouden aanroepen per 5 minuten | 45,000 | De werkbelasting kan worden verdelen over meerdere accounts, indien nodig. | 
| Runtime-eindpunt: blokkeren gelijktijdige aanroepen | ~1,000 | Verminder het aantal gelijktijdige aanvragen of verkorten indien nodig. | 
|||| 

Deze beperkingen gelden voor het aantal artefacten die u aan een account voor de integratie toevoegen kunt.

#### <a name="free-pricing-tier"></a>Gratis prijscategorie

| Naam | Limiet | Opmerkingen | 
| ---- | ----- | ----- | 
| Overeenkomsten | 10 | | 
| Andere typen artefact | 25 |Artefacten typen zijn partners, schema's, certificaten en maps. Elk type mogelijk tot het maximum aantal van artefacten. | 
|||| 

#### <a name="standard-pricing-tier"></a>Standard-prijscategorie

| Naam | Limiet | Opmerkingen | 
| ---- | ----- | ----- | 
| Elk type artefact | 500 | Artefacten typen zijn overeenkomsten, partners, schema's, certificaten en maps. Elk type mogelijk tot het maximum aantal van artefacten. | 
|||| 

### <a name="b2b-protocols-as2-x12-edifact-message-size"></a>Berichtgrootte B2B-protocollen (AS2, X12, EDIFACT)

Deze beperkingen gelden voor B2B-protocollen.

| Naam | Limiet | Opmerkingen | 
| ---- | ----- | ----- | 
| AS2 | 50 MB | Van toepassing op decoderen en coderen | 
| X12 | 50 MB | Van toepassing op decoderen en coderen | 
| EDIFACT | 50 MB | Van toepassing op decoderen en coderen | 
|||| 

<a name="configuration"></a>

## <a name="configuration-ip-addresses"></a>Configuratie: IP-adressen

### <a name="logic-apps-service"></a>Logic Apps-service

De oproepen die een logische app rechtstreeks, dat wil zeggen maakt, via [HTTP](../connectors/connectors-native-http.md) of [HTTP + Swagger](../connectors/connectors-native-http-swagger.md) of andere HTTP-aanvragen afkomstig zijn van IP-adressen in deze lijst.

|Logic Apps regio|Uitgaande IP|
|-----------------|-----------|
|Australië - oost|13.75.149.4, 104.210.91.55, 104.210.90.241|
|Australië - zuidoost|13.73.114.207, 13.77.3.139, 13.70.159.205|
|Brazilië - zuid|191.235.82.221, 191.235.91.7, 191.234.182.26|
|Canada - midden|52.233.29.92, 52.228.39.241, 52.228.39.244|
|Canada - oost|52.232.128.155, 52.229.120.45, 52.229.126.25|
|Centraal-India|52.172.154.168, 52.172.186.159, 52.172.185.79|
|VS - midden|13.67.236.125, 104.208.25.27, 40.122.170.198|
|Oost-Azië|13.75.94.173, 40.83.127.19, 52.175.33.254|
|VS - oost|13.92.98.111, 40.121.91.41, 40.114.82.191|
|VS - oost 2|40.84.30.147, 104.208.155.200, 104.208.158.174|
|Japan - oost|13.71.158.3, 13.73.4.207, 13.71.158.120|
|Japan - west|40.74.140.4, 104.214.137.243, 138.91.26.45|
|Noord-centraal VS|168.62.248.37, 157.55.210.61, 157.55.212.238|
|Noord-Europa|40.113.12.95, 52.178.165.215, 52.178.166.21|
|Zuid-centraal VS|104.210.144.48, 13.65.82.17, 13.66.52.232|
|Zuidoost-Azië|13.76.133.155, 52.163.228.93, 52.163.230.166|
|Zuid-India|52.172.50.24, 52.172.55.231, 52.172.52.0|
|West-centraal VS|52.161.27.190, 52.161.18.218, 52.161.9.108|
|West-Europa|40.68.222.65, 40.68.209.23, 13.95.147.65|
|West-India|104.211.164.80, 104.211.162.205, 104.211.164.136|
|VS - west|52.160.92.112, 40.118.244.241, 40.118.241.243|
|VS - west 2|13.66.210.167, 52.183.30.169, 52.183.29.132|
|Verenigd Koninkrijk Zuid|51.140.74.14, 51.140.73.85, 51.140.78.44|
|Verenigd Koninkrijk West|51.141.54.185, 51.141.45.238, 51.141.47.136|
| | |

### <a name="connectors"></a>Connectors

De oproepen die [connectors](../connectors/apis-list.md) moet afkomstig zijn van de IP-adressen in deze lijst.

|Logic Apps regio|Uitgaande IP|
|-----------------|-----------|
|Australië - oost|40.126.251.213|
|Australië - zuidoost|40.127.80.34|
|Brazilië - zuid|191.232.38.129|
|Canada - midden|52.233.31.197, 52.228.42.205, 52.228.33.76, 52.228.34.13|
|Canada - oost|52.229.123.98, 52.229.120.178, 52.229.126.202, 52.229.120.52|
|Centraal-India|104.211.98.164|
|VS - midden|40.122.49.51|
|Oost-Azië|23.99.116.181|
|VS - oost|191.237.41.52|
|VS - oost 2|104.208.233.100|
|Japan - oost|40.115.186.96|
|Japan - west|40.74.130.77|
|Noord-centraal VS|65.52.218.230|
|Noord-Europa|104.45.93.9|
|Zuid-centraal VS|104.214.70.191|
|Zuidoost-Azië|13.76.231.68|
|Zuid-India|104.211.227.225|
|West-Europa|40.115.50.13|
|West-India|104.211.161.203|
|VS - west|104.40.51.248|
|Verenigd Koninkrijk Zuid|51.140.80.51|
|Verenigd Koninkrijk West|51.141.47.105|
| | | 

## <a name="next-steps"></a>Volgende stappen  

* [Een logische app maken](../logic-apps/quickstart-create-first-logic-app-workflow.md)  
* [Algemene voorbeelden en scenario 's](../logic-apps/logic-apps-examples-and-scenarios.md)
* [Video: Automatiseren van bedrijfsprocessen met Logic Apps](http://channel9.msdn.com/Events/Build/2016/T694) 
* [Video: Uw systemen integreren met Logic Apps](http://channel9.msdn.com/Events/Build/2016/P462)
