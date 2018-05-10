---
title: Limieten en -configuratie - Azure Logic Apps | Microsoft Docs
description: Service-limieten en configuratiewaarden voor Azure Logic Apps
services: logic-apps
documentationcenter: ''
author: jeffhollan
manager: anneta
editor: ''
ms.assetid: 75b52eeb-23a7-47dd-a42f-1351c6dfebdc
ms.service: logic-apps
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/25/2017
ms.author: LADocs; jehollan
ms.openlocfilehash: 524a2dc7a1a5ae4f0747af03d1b9e69d512f0f00
ms.sourcegitcommit: d98d99567d0383bb8d7cbe2d767ec15ebf2daeb2
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/10/2018
---
# <a name="limits-and-configuration-information-for-azure-logic-apps"></a>Limieten en configuratie-informatie voor Azure Logic Apps

Dit artikel worden de limieten en de configuratie-informatie voor het maken en uitvoeren van geautomatiseerde werkstromen met Azure Logic Apps. Zie voor Microsoft-Flow [en -configuratie in de Microsoft-Flow](https://docs.microsoft.com/flow/limits-and-config).

<a name="definition-limits"></a>

## <a name="definition-limits"></a>Definitie-limieten

Hier volgen de limieten voor een definitie van één logische app:

| Naam | Limiet | Opmerkingen | 
| ---- | ----- | ----- | 
| Acties per werkstroom | 500 | Om deze limiet uitbreidt, kunt u geneste werkstromen toevoegen, indien nodig. |
| Diepte van het nesten voor acties toegestaan | 8 | Om deze limiet uitbreidt, kunt u geneste werkstromen toevoegen, indien nodig. | 
| Werkstromen per regio per abonnement | 1000 | | 
| Triggers per werkstroom | 10 | Als u werkt in de codeweergave niet de designer | 
| Switch bereik gevallen limiet | 25 | | 
| Variabelen voor de per werkstroom | 250 | | 
| Tekens per expressie | 8.192 | | 
| Maximale grootte voor `trackedProperties` | 16.000 tekens | 
| Geef een naam voor `action` of `trigger` | 80 tekens | | 
| Lengte van `description` | 256 tekens | | 
| Maximum `parameters` | 50 | | 
| Maximum `outputs` | 10 | | 
||||  

<a name="run-duration-retention-limits"></a>

## <a name="run-duration-and-retention-limits"></a>Limieten voor de duur en retentie uitvoeren

Hier volgen de limieten voor een enkele logische-app uitvoeren:

| Naam | Limiet | 
| ---- | ----- | 
| Duur uitvoering | 90 dagen | 
| Bewaren van opslag | Begintijd van 90 dagen van de uitvoeren | 
| Minimale terugkeerpatroon | 1 seconde </br>Voor logic apps met een App Service-Plan: 15 seconden | 
| Maximale terugkeerpatroon | 500 dagen | 
||| 

Overschrijden voor uitvoering duur of handhaving van opslag in uw flow normale verwerking [Neem contact op met het team van Logic Apps](mailto://logicappsemail@microsoft.com) voor hulp bij uw behoeften.

<a name="looping-debatching-limits"></a>

## <a name="looping-and-debatching-limits"></a>Lussen en limieten debatching

Hier volgen de limieten voor een enkele logische-app uitvoeren:

| Naam | Limiet | Opmerkingen | 
| ---- | ----- | ----- | 
| Pas iteraties | 5.000 | | 
| ForEach-items | 100,000 | U kunt de [query actie](../connectors/connectors-native-query.md) voor het filteren van grotere matrices indien nodig. | 
| ForEach parallelle uitvoering | 50 | De standaardwaarde is 20. <p>Om in te stellen van een specifiek niveau van parallelle uitvoering in een lus ForEach de `runtimeConfiguration` eigenschap in de `foreach` in te grijpen. <p>Als u wilt een lus ForEach sequentieel worden uitgevoerd, stelt de `operationOptions` eigenschap in op 'Sequentiële' in de `foreach` actie. | 
| SplitOn items | 100,000 | | 
|||| 

<a name="throughput-limits"></a>

## <a name="throughput-limits"></a>Doorvoerlimieten

Hier volgen de limieten voor een enkele logische-app uitvoeren:

| Naam | Limiet | Opmerkingen | 
| ----- | ----- | ----- | 
| Acties uitvoeringen per 5 minuten | 100,000 | Als u wilt de limiet voor 300.000 verhogen, kunt u een logische app uitvoeren in `High Throughput` modus. Voor het configureren van de modus hoge doorvoer, onder de `runtimeConfiguration` ingesteld van de resource werkstroom de `operationOptions` eigenschap `OptimizedForHighThroughput`. <p>**Opmerking**: hoge doorvoersnelheid modus bevindt zich in preview. U kunt ook een werkbelasting distribueren voor meerdere apps indien nodig. | 
| Acties gelijktijdige uitgaande oproepen | ~2,500 | Verminder het aantal gelijktijdige aanvragen of verkorten indien nodig. | 
| Runtime-eindpunt: gelijktijdige binnenkomende oproepen | ~1,000 | Verminder het aantal gelijktijdige aanvragen of verkorten indien nodig. | 
| Runtime-eindpunt: aanroepen per 5 minuten lezen  | 60,000 | Werkbelasting kan worden verdelen over meerdere apps naar behoefte. | 
| Runtime-eindpunt: aanroepen aanroepen per 5 minuten| 45,000 |Werkbelasting kan worden verdelen over meerdere apps naar behoefte. | 
|||| 

Aan deze beperkingen in de normale verwerking of voer load testen die mogelijk groter is dan deze limiet overschrijden [Neem contact op met het team van Logic Apps](mailto://logicappsemail@microsoft.com) voor hulp bij uw behoeften.

<a name="request-limits"></a>

## <a name="http-request-limits"></a>Limieten voor HTTP-aanvraag

Hier volgen de limieten voor een één HTTP-aanvraag of een aanroep van synchrone connector:

#### <a name="timeout"></a>Time-out

Bepaalde bewerkingen connector asynchrone aanroepen of luisteren naar webhook-aanvragen, zodat de time-out voor deze bewerkingen mogelijk niet langer zijn dan deze limieten. Zie voor meer informatie, de technische details voor de specifieke connector en ook [werkstroom triggers en acties](../logic-apps/logic-apps-workflow-actions-triggers.md#http-action).

| Naam | Limiet | Opmerkingen | 
| ---- | ----- | ----- | 
| Uitgaande aanvraag | 120 seconden | Gebruik voor actieve bewerkingen langer, een [asynchrone polling patroon](../logic-apps/logic-apps-create-api-app.md#async-pattern) of een [tot lus](../logic-apps/logic-apps-workflow-actions-triggers.md#until-action). | 
| Synchrone antwoord | 120 seconden | Alle stappen in het antwoord moeten voltooien binnen de limiet voor de oorspronkelijke aanvraag voor het ophalen van het antwoord, tenzij u een andere logische app als een geneste werkstroom aanroepen. Zie voor meer informatie [aanroepen, trigger of nesten van logische apps](../logic-apps/logic-apps-http-endpoint.md). | 
|||| 

#### <a name="message-size"></a>Berichtgrootte

| Naam | Limiet | Opmerkingen | 
| ---- | ----- | ----- | 
| Berichtgrootte | 100 MB | Sommige connectors en API's mogelijk geen ondersteuning voor 100 MB. | 
| Expressie evaluatie limiet | 131.072 tekens | De `@concat()`, `@base64()`, `@string()` expressies mag niet langer zijn dan deze limiet. | 
|||| 

#### <a name="retry-policy"></a>Beleid voor opnieuw proberen

| Naam | Limiet | Opmerkingen | 
| ---- | ----- | ----- | 
| Nieuwe pogingen | 90 | De standaardwaarde is 4. U kunt de standaardinstelling wijzigen met de [parameter van het beleid opnieuw](../logic-apps/logic-apps-workflow-actions-triggers.md). | 
| Maximale tijd tussen elke poging | 1 dag | U kunt de standaardinstelling wijzigen met de [parameter van het beleid opnieuw](../logic-apps/logic-apps-workflow-actions-triggers.md). | 
| Tijd tussen elke poging min | 5 seconden | U kunt de standaardinstelling wijzigen met de [parameter van het beleid opnieuw](../logic-apps/logic-apps-workflow-actions-triggers.md). |
|||| 

<a name="custom-connector-limits"></a>

## <a name="custom-connector-limits"></a>Aangepaste connector limieten

Hier volgen de limieten voor het aangepaste connectors die u van web-API's maken kunt.

| Naam | Limiet | 
| ---- | ----- | 
| Aantal aangepaste connectors | 1000 per Azure-abonnement | 
| Het aantal aanvragen per minuut voor elke verbinding die tot stand is gebracht door een aangepaste connector | 500 aanvragen per verbinding |
|||| 

<a name="integration-account-limits"></a>

## <a name="integration-account-limits"></a>Limieten van integratie

<a name="artifact-number-limits"></a>

### <a name="artifact-limits-per-integration-account"></a>Artefacten limieten per integratie-account

Hier volgen de grenzen van het aantal artefacten voor elk account integratie.

*Gratis prijscategorie*

| Naam | Limiet | Opmerkingen | 
| ---- | ----- | ----- | 
| Overeenkomsten | 10 | | 
| Andere typen artefact | 25 | Artefacten typen zijn partners, schema's, certificaten en maps. Elk type mogelijk tot het maximum aantal van artefacten. | 
|||| 

*Standard-prijscategorie*

| Naam | Limiet | Opmerkingen | 
| ---- | ----- | ----- | 
| Elk type artefact | 500 | Artefacten typen zijn overeenkomsten, partners, schema's, certificaten en maps. Elk type mogelijk tot het maximum aantal van artefacten. | 
|||| 

<a name="artifact-capacity-limits"></a>

### <a name="artifact-capacity-limits"></a>Capaciteitslimieten artefact

| Naam | Limiet | Opmerkingen | 
| ---- | ----- | ----- | 
| Schema | 8 MB | Uploaden van bestanden die groter is dan 2 MB gebruiken de [blob-URI](../logic-apps/logic-apps-enterprise-integration-schemas.md). | 
| Kaart (XSLT-bestand) | 2 MB | | 
| Runtime-eindpunt: aanroepen per 5 minuten lezen | 60,000 | U kunt de werklast verdelen over meerdere accounts zo nodig. | 
| Runtime-eindpunt: aanroepen aanroepen per 5 minuten | 45,000 | U kunt de werklast verdelen over meerdere accounts zo nodig. | 
| Runtime-eindpunt: bijhouden aanroepen per 5 minuten | 45,000 | U kunt de werklast verdelen over meerdere accounts zo nodig. | 
| Runtime-eindpunt: blokkeren gelijktijdige aanroepen | ~1,000 | U kunt Verminder het aantal gelijktijdige aanvragen of verkorten indien nodig. | 
||||  

<a name="b2b-protocol-limits"></a>

### <a name="b2b-protocol-as2-x12-edifact-message-size"></a>Berichtgrootte B2B-protocol (AS2, X12, EDIFACT)

Hier volgen de grenzen die betrekking hebben op B2B-protocollen:

| Naam | Limiet | Opmerkingen | 
| ---- | ----- | ----- | 
| AS2 | 50 MB | Van toepassing op decoderen en coderen | 
| X12 | 50 MB | Van toepassing op decoderen en coderen | 
| EDIFACT | 50 MB | Van toepassing op decoderen en coderen | 
|||| 

<a name="configuration"></a>

## <a name="configuration-ip-addresses"></a>Configuratie: IP-adressen

### <a name="azure-logic-apps-service"></a>Azure Logic Apps-service

Alle logische apps in een regio gebruiken dezelfde bereik van IP-adressen.
Het aanroepen van logic apps rechtstreeks met kunt [HTTP](../connectors/connectors-native-http.md), [HTTP + Swagger](../connectors/connectors-native-http-swagger.md) of andere HTTP-aanvragen afkomstig zijn van IP-adressen in deze lijst. 

| Logic Apps regio | Uitgaande IP |
|-------------------|-------------|
| Australië | 13.73.114.207, 13.77.3.139, 13.70.159.205 |
| Australië - oost | 13.75.149.4, 104.210.91.55, 104.210.90.241 |
| Brazilië - zuid | 191.235.82.221, 191.235.91.7, 191.234.182.26 |
| Canada - midden | 52.233.29.92, 52.228.39.241, 52.228.39.244 |
| Canada - oost | 52.232.128.155, 52.229.120.45, 52.229.126.25 |
| Centraal-India | 52.172.154.168, 52.172.186.159, 52.172.185.79 |
| VS - midden | 13.67.236.125, 104.208.25.27, 40.122.170.198 |
| Oost-Azië | 13.75.94.173, 40.83.127.19, 52.175.33.254 |
| VS - oost | 13.92.98.111, 40.121.91.41, 40.114.82.191 |
| VS - oost 2 | 40.84.30.147, 104.208.155.200, 104.208.158.174 |
| Japan - oost | 13.71.158.3, 13.73.4.207, 13.71.158.120 |
| Japan - west | 40.74.140.4, 104.214.137.243, 138.91.26.45 |
| Noord-centraal VS | 168.62.248.37, 157.55.210.61, 157.55.212.238 |
| Noord-Europa | 40.113.12.95, 52.178.165.215, 52.178.166.21 |
| Zuid-centraal VS | 104.210.144.48, 13.65.82.17, 13.66.52.232 |
| Zuid-India | 52.172.50.24, 52.172.55.231, 52.172.52.0 |
| Zuidoost-Azië | 13.76.133.155, 52.163.228.93, 52.163.230.166 |
| West-centraal VS | 52.161.27.190, 52.161.18.218, 52.161.9.108 |
| West-Europa | 40.68.222.65, 40.68.209.23, 13.95.147.65 |
| West-India | 104.211.164.80, 104.211.162.205, 104.211.164.136 |
| VS - west | 52.160.92.112, 40.118.244.241, 40.118.241.243 |
| VS - west 2 | 13.66.210.167, 52.183.30.169, 52.183.29.132 |
| Verenigd Koninkrijk Zuid | 51.140.74.14, 51.140.73.85, 51.140.78.44 |
| Verenigd Koninkrijk West | 51.141.54.185, 51.141.45.238, 51.141.47.136 |
| | |

| Logic Apps regio | Inkomende IP |
|-------------------|-------------|
| Australië - oost | 3.75.153.66, 104.210.89.222, 104.210.89.244 |
| Australië - zuidoost | 13.73.115.153, 40.115.78.70, 40.115.78.237 |
| Brazilië - zuid | 191.235.86.199, 191.235.95.229, 191.235.94.220 |
| Canada - midden | 13.88.249.209, 52.233.30.218, 52.233.29.79 |
| Canada - oost | 52.232.129.143, 52.229.125.57, 52.232.133.109 |
| Centraal-India | 52.172.157.194, 52.172.184.192, 52.172.191.194 |
| VS - midden | 13.67.236.76, 40.77.111.254, 40.77.31.87 |
| Oost-Azië | 168.63.200.173, 13.75.89.159, 23.97.68.172 |
| VS - oost | 137.135.106.54, 40.117.99.79, 40.117.100.228 |
| VS - oost 2 | 40.84.25.234, 40.79.44.7, 40.84.59.136 |
| Japan - oost | 13.71.146.140, 13.78.84.187, 13.78.62.130 |
| Japan - west | 40.74.140.173, 40.74.81.13, 40.74.85.215 |
| Noord-centraal VS | 168.62.249.81, 157.56.12.202, 65.52.211.164 |
| Noord-Europa | 13.79.173.49, 52.169.218.253, 52.169.220.174 |
| Zuid-centraal VS | 52.172.9.47, 52.172.49.43, 52.172.51.140 |
| Zuid-India | 52.172.9.47, 52.172.49.43, 52.172.51.140 |
| Zuidoost-Azië | 52.163.93.214, 52.187.65.81, 52.187.65.155 |
| West-centraal VS | 52.161.26.172, 52.161.8.128, 52.161.19.82 |
| West-Europa | 13.95.155.53, 52.174.54.218, 52.174.49.6 |
| West-India | 104.211.164.112, 104.211.165.81, 104.211.164.25 |
| VS - west | 52.160.90.237, 138.91.188.137, 13.91.252.184 |
| VS - west 2 | 13.66.224.169, 52.183.30.10, 52.183.39.67 |
| Verenigd Koninkrijk Zuid | 51.140.79.109, 51.140.78.71, 51.140.84.39 |
| Verenigd Koninkrijk West | 51.141.48.98, 51.141.51.145, 51.141.53.164 |
| | |

### <a name="connectors"></a>Connectors

De oproepen die [connectors](../connectors/apis-list.md) moet afkomstig zijn van de IP-adressen in deze lijst.

| Logic Apps regio | Uitgaande IP |
|-------------------|-------------|
| Australië - oost | 40.126.251.213 |
| Australië - zuidoost | 40.127.80.34 |
| Brazilië - zuid | 191.232.38.129 |
| Canada - midden | 52.233.31.197, 52.228.42.205, 52.228.33.76, 52.228.34.13 |
| Canada - oost | 52.229.123.98, 52.229.120.178, 52.229.126.202, 52.229.120.52 |
| Centraal-India | 104.211.98.164 |
| VS - midden | 40.122.49.51 |
| Oost-Azië | 23.99.116.181 |
| VS - oost | 191.237.41.52 |
| VS - oost 2 | 104.208.233.100 |
| Japan - oost | 40.115.186.96 |
| Japan - west | 40.74.130.77 |
| Noord-centraal VS | 65.52.218.230 |
| Noord-Europa | 104.45.93.9 |
| Zuid-centraal VS | 104.214.70.191 |
| Zuid-India | 104.211.227.225 |
| Zuidoost-Azië | 13.76.231.68 |
| West-Europa | 40.115.50.13 |
| West-India | 104.211.161.203 |
| VS - west | 104.40.51.248 |
| Verenigd Koninkrijk Zuid | 51.140.80.51 |
| Verenigd Koninkrijk West | 51.141.47.105 |
| | | 

## <a name="next-steps"></a>Volgende stappen  

* [Een logische app maken](../logic-apps/quickstart-create-first-logic-app-workflow.md)  
* [Algemene voorbeelden en scenario 's](../logic-apps/logic-apps-examples-and-scenarios.md)
* [Video: Automatiseren van bedrijfsprocessen met Logic Apps](http://channel9.msdn.com/Events/Build/2016/T694) 
* [Video: Uw systemen integreren met Logic Apps](http://channel9.msdn.com/Events/Build/2016/P462)
