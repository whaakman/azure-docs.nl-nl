---
title: Limieten en configuratie - Azure Logic Apps | Microsoft Docs
description: Service limieten en configuratiewaarden voor Azure Logic Apps
services: logic-apps
author: ecfan
manager: jeconnoc
ms.author: estfan
ms.topic: article
ms.date: 05/30/2018
ms.service: logic-apps
ms.reviewer: klam, LADocs
ms.suite: integration
ms.openlocfilehash: d153fa495c82103460dd8e4e2aae6000e49eb3eb
ms.sourcegitcommit: 068fc623c1bb7fb767919c4882280cad8bc33e3a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/27/2018
ms.locfileid: "39283585"
---
# <a name="limits-and-configuration-information-for-azure-logic-apps"></a>Limieten en configuratie-informatie voor Azure Logic Apps

Dit artikel beschrijft de limieten en configuratiegegevens voor het maken en uitvoeren van geautomatiseerde werkstromen met Azure Logic Apps. Zie voor Microsoft Flow, [limieten en configuratie in Microsoft Flow](https://docs.microsoft.com/flow/limits-and-config).

<a name="definition-limits"></a>

## <a name="definition-limits"></a>Definitielimieten

Hier zijn de limieten voor een definitie van één logische app:

| Naam | Limiet | Opmerkingen | 
| ---- | ----- | ----- | 
| Acties per werkstroom | 500 | U kunt om uit te breiden deze limiet, geneste werkstromen toevoegen als nodig is. |
| Toegestane nestdiepte voor acties voor acties | 8 | U kunt om uit te breiden deze limiet, geneste werkstromen toevoegen als nodig is. | 
| Werkstromen per regio per abonnement | 1000 | | 
| Triggers per werkstroom | 10 | Als u werkt in de weergave van code, niet de ontwerper | 
| Switch bereik gevallen beperken | 25 | | 
| Variabelen per werkstroom | 250 | | 
| Tekens per expressie | 8.192 | | 
| Maximale grootte voor `trackedProperties` | 16.000 tekens | 
| Geef de naam voor `action` of `trigger` | 80 tekens | | 
| Lengte van `description` | 256 tekens | | 
| Maximaal `parameters` | 50 | | 
| Maximaal `outputs` | 10 | | 
||||  

<a name="run-duration-retention-limits"></a>

## <a name="run-duration-and-retention-limits"></a>Limieten voor de duur en retentie uitvoeren

Hier zijn de limieten voor een enkele logische app:

| Naam | Limiet | Opmerkingen | 
|------|-------|-------| 
| Uitvoeringsduur | 90 dagen | Deze limiet, Zie [wijziging uitvoeringsduur](#change-duration). | 
| Bewaarperiode | Begintijd van 90 dagen na het uitvoeren van | Zie voor informatie over het wijzigen van deze limiet op een waarde tussen 7 dagen en 90 dagen [opslag retentie wijzigen](#change-retention). | 
| Minimale vernieuwingsfrequentie | 1 seconde | | 
| Maximale terugkeerpatroon | 500 dagen | | 
|||| 

<a name="change-duration"></a>
<a name="change-retention"></a>

### <a name="change-run-duration-and-storage-retention"></a>Voer de duur en opslag retentie wijzigen

Als u wilt wijzigen van de standaardlimiet voor het tussen 7 dagen en 90 dagen, volg deze stappen. Als u nodig hebt om te gaan dan de maximale limiet [contact op met de Logic Apps-team](mailto://logicappsemail@microsoft.com) voor hulp bij uw vereisten.

1. Kies in de Azure-portal, in het menu van uw logische app **Werkstroominstellingen**. 

2. Onder **Runtime-opties**, uit de **behoud van uitvoeringsgeschiedenis in dagen uitgevoerd** Kies **aangepaste**. 

3. Opgeven of de schuifregelaar voor het aantal dagen dat u wilt.

<a name="looping-debatching-limits"></a>

## <a name="concurrency-looping-and-debatching-limits"></a>Gelijktijdigheid van taken, lussen en debatching limieten

Hier zijn de limieten voor een enkele logische app:

| Naam | Limiet | Opmerkingen | 
| ---- | ----- | ----- | 
| Trigger gelijktijdigheid | 50 | De standaardlimiet is 20. Deze limiet wordt het maximum aantal logic app-exemplaren die kunnen worden uitgevoerd op hetzelfde moment of parallel beschreven. <p><p>De standaardlimiet op een waarde tussen 1 en 50 liggen, Zie [wijziging trigger gelijktijdigheid](../logic-apps/logic-apps-workflow-actions-triggers.md#change-trigger-concurrency) of [exemplaren sequentieel activeren](../logic-apps/logic-apps-workflow-actions-triggers.md#sequential-trigger). | 
| Maximale wachttijd wordt uitgevoerd | 100 | De standaardlimiet is 10. Deze limiet wordt het maximum aantal logic app-exemplaren die worden uitgevoerd wanneer u uw logische app wordt al uitgevoerd voor het maximum aantal gelijktijdige instanties kunt wachten beschreven. <p><p>De standaardlimiet op een waarde tussen 0 en 100 liggen, Zie [wijziging wachten uitvoeringen beperken](../logic-apps/logic-apps-workflow-actions-triggers.md#change-waiting-runs). | 
| Foreach-items | 100.000 | Deze limiet wordt het maximum aantal matrixitems die kan worden verwerkt door een lus 'voor elke' beschreven. <p><p>Voor grotere matrices filteren, kunt u de [queryactie](../connectors/connectors-native-query.md). | 
| Foreach-iteraties | 50 | De standaardlimiet is 20. Deze limiet wordt beschreven voor het maximum aantal 'voor elke' iteraties die kunnen worden uitgevoerd op hetzelfde moment of parallel in een lus. <p><p>De standaardlimiet op een waarde tussen 1 en 50 liggen, Zie [wijzigen 'voor elke' gelijktijdigheid](../logic-apps/logic-apps-workflow-actions-triggers.md#change-for-each-concurrency) of [uitvoeren 'voor elke' wordt uitgevoerd na elkaar](../logic-apps/logic-apps-workflow-actions-triggers.md#sequential-for-each). | 
| SplitOn-items | 100.000 | | 
| UNTIL-iteraties | 5.000 | | 
|||| 

<a name="throughput-limits"></a>

## <a name="throughput-limits"></a>Doorvoerlimieten

Hier zijn de limieten voor een enkele logische app:

| Naam | Limiet | Opmerkingen | 
| ---- | ----- | ----- | 
| Actie: Uitvoeringen per vijf minuten | 300,000 | De standaardlimiet is 100.000. De standaardlimiet Zie [uw logische app uitvoeren in de modus "hoge doorvoer"](../logic-apps/logic-apps-workflow-actions-triggers.md#run-high-throughput-mode), deze bevindt zich in preview. Of u kunt de workload gedistribueerd over meer dan één logische app zo nodig. | 
| Actie: Gelijktijdige uitgaande oproepen | ~2,500 | U kunt Verminder het aantal gelijktijdige aanvragen of Beperk de duur indien nodig. | 
| Runtime-eindpunt: gelijktijdige binnenkomende oproepen | ~1,000 | U kunt Verminder het aantal gelijktijdige aanvragen of Beperk de duur indien nodig. | 
| Runtime-eindpunt: aanroepen per vijf minuten lezen  | 60,000 | U kunt werklast verdelen over meer dan één app zo nodig. | 
| Runtime-eindpunt: aanroepen aanroepen per vijf minuten | 45,000 | U kunt werklast verdelen over meer dan één app zo nodig. | 
| Inhoud doorvoer per vijf minuten | 600 MB | U kunt werklast verdelen over meer dan één app zo nodig. | 
|||| 

Hoger dan deze limieten in de normale verwerking of voer belastingtests uitvoeren die mogelijk verder gaan dan deze limieten [contact op met de Logic Apps-team](mailto://logicappsemail@microsoft.com) voor hulp bij uw vereisten.

<a name="request-limits"></a>

## <a name="http-request-limits"></a>Limieten voor HTTP-aanvraag

Hier zijn de limieten voor een afzonderlijke HTTP-aanvraag of een aanroep van synchrone connector:

#### <a name="timeout"></a>Time-out

Bepaalde bewerkingen connector asynchrone aanroepen of luisteren naar aanvragen van de webhook, zodat de time-out voor deze bewerkingen mogelijk niet langer zijn dan deze limieten. Zie voor meer informatie, de technische details voor de specifieke connector en ook [werkstroomtriggers en acties](../logic-apps/logic-apps-workflow-actions-triggers.md#http-action).

| Naam | Limiet | Opmerkingen | 
| ---- | ----- | ----- | 
| Uitgaande aanvraag | 120 seconden | Voor meer actieve bewerkingen, gebruikt u een [asynchrone polling patroon](../logic-apps/logic-apps-create-api-app.md#async-pattern) of een [totdat-lus](../logic-apps/logic-apps-workflow-actions-triggers.md#until-action). | 
| Synchrone reactie | 120 seconden | Alle stappen in het antwoord moeten voltooien binnen de limiet voor de oorspronkelijke aanvraag voor het ophalen van het antwoord, tenzij u een andere logische app als een geneste werkstroom aanroepen. Zie voor meer informatie, [aanroepen, trigger of nesten van logische apps](../logic-apps/logic-apps-http-endpoint.md). | 
|||| 

#### <a name="message-size"></a>Berichtgrootte

| Naam | Limiet | Opmerkingen | 
| ---- | ----- | ----- | 
| Berichtgrootte | 100 MB | Als tijdelijke oplossing voor deze limiet, Zie [grote berichten met logische groepen te verdelen verwerken](../logic-apps/logic-apps-handle-large-messages.md). Echter sommige connectors en API's mogelijk geen ondersteuning voor logische groepen te verdelen of zelfs de standaardlimiet. | 
| Grootte van het bericht met logische groepen te verdelen | 1 GB | Deze limiet geldt voor acties die systeemeigen ondersteuning voor logische groepen te verdelen of kunnen u inschakelen opdelen in hun runtime-configuratie. Zie voor meer informatie, [grote berichten met logische groepen te verdelen verwerken](../logic-apps/logic-apps-handle-large-messages.md). | 
| Limiet voor evaluatie van expressie | 131.072 tekens | De `@concat()`, `@base64()`, `@string()` expressies mag niet langer zijn dan deze limiet. | 
|||| 

#### <a name="retry-policy"></a>Beleid voor opnieuw proberen

| Naam | Limiet | Opmerkingen | 
| ---- | ----- | ----- | 
| Nieuwe pogingen | 90 | De standaardwaarde is 4. U kunt de standaardinstelling wijzigen met de [parameter van het beleid opnieuw](../logic-apps/logic-apps-workflow-actions-triggers.md). | 
| Maximale vertraging voor opnieuw proberen | 1 dag | U kunt de standaardinstelling wijzigen met de [parameter van het beleid opnieuw](../logic-apps/logic-apps-workflow-actions-triggers.md). | 
| Min vertraging voor opnieuw proberen | 5 seconden | U kunt de standaardinstelling wijzigen met de [parameter van het beleid opnieuw](../logic-apps/logic-apps-workflow-actions-triggers.md). |
|||| 

<a name="custom-connector-limits"></a>

## <a name="custom-connector-limits"></a>Limieten voor de aangepaste connector

Hier zijn de limieten voor aangepaste connectors die u van web-API's maken kunt.

| Naam | Limiet | 
| ---- | ----- | 
| Aantal aangepaste connectors | 1000 per Azure-abonnement | 
| Het aantal aanvragen per minuut voor elke verbinding die tot stand is gebracht door een aangepaste connector | 500 aanvragen per verbinding |
|||| 

<a name="integration-account-limits"></a>

## <a name="integration-account-limits"></a>Limieten voor integratie-account

<a name="artifact-number-limits"></a>

### <a name="artifact-limits-per-integration-account"></a>Artefact limieten per integratie-account

Hier zijn de limieten voor het aantal artefacten voor elke integratie-account. Zie voor meer informatie, [prijzen voor Logic Apps](https://azure.microsoft.com/pricing/details/logic-apps/). 

*Gratis laag*

Gebruik de gratis laag alleen voor experimentele scenario's, niet-productie scenario's. Deze laag beperkt doorvoer en het gebruik, en heeft geen service level agreement (SLA).

| Artefact | Limiet | Opmerkingen | 
|----------|-------|-------| 
| EDI-handelspartners | 25 | | 
| EDI-handelspartners overeenkomsten | 10 | | 
| Kaarten | 25 | | 
| Schema 's | 25 | 
| Assembly's | 10 | | 
| Batchconfiguraties | 5 | 
| Certificaten | 25 | | 
|||| 

*Basic-laag*

| Artefact | Limiet | Opmerkingen | 
|----------|-------|-------| 
| EDI-handelspartners | 2 | | 
| EDI-handelspartners overeenkomsten | 1 | | 
| Kaarten | 500 | | 
| Schema 's | 500 | 
| Assembly's | 25 | | 
| Batchconfiguraties | 1 | | 
| Certificaten | 2 | | 
|||| 

*Standaardlaag*

| Artefact | Limiet | Opmerkingen | 
|----------|-------|-------| 
| EDI-handelspartners | 500 | | 
| EDI-handelspartners overeenkomsten | 500 | | 
| Kaarten | 500 | | 
| Schema 's | 500 | 
| Assembly's | 50 | | 
| Batchconfiguraties | 5 |  
| Certificaten | 50 | | 
|||| 

<a name="artifact-capacity-limits"></a>

### <a name="artifact-capacity-limits"></a>Capaciteitslimieten artefact

| Naam | Limiet | Opmerkingen | 
| ---- | ----- | ----- | 
| Schema | 8 MB | Als u wilt uploaden van bestanden die groter zijn dan 2 MB, gebruikt u de [blob-URI](../logic-apps/logic-apps-enterprise-integration-schemas.md). | 
| Kaart (XSLT-bestand) | 2 MB | | 
| Runtime-eindpunt: aanroepen per vijf minuten lezen | 60,000 | U kunt de workload gedistribueerd over meer dan één account indien nodig. | 
| Runtime-eindpunt: aanroepen aanroepen per vijf minuten | 45,000 | U kunt de workload gedistribueerd over meer dan één account indien nodig. | 
| Runtime-eindpunt: bijhouden van aanroepen per vijf minuten | 45,000 | U kunt de workload gedistribueerd over meer dan één account indien nodig. | 
| Runtime-eindpunt: gelijktijdige aanroepen blokkeren | ~1,000 | U kunt Verminder het aantal gelijktijdige aanvragen of Beperk de duur indien nodig. | 
||||  

<a name="b2b-protocol-limits"></a>

### <a name="b2b-protocol-as2-x12-edifact-message-size"></a>Berichtgrootte voor B2B-protocol (AS2, X12, EDIFACT)

Dit zijn de limieten die voor B2B-protocollen gelden:

| Naam | Limiet | Opmerkingen | 
| ---- | ----- | ----- | 
| AS2 | 50 MB | Is van toepassing op decoderen en coderen | 
| X12 | 50 MB | Is van toepassing op decoderen en coderen | 
| EDIFACT | 50 MB | Is van toepassing op decoderen en coderen | 
|||| 

<a name="configuration"></a>

## <a name="configuration-ip-addresses"></a>Configuratie: IP-adressen

### <a name="azure-logic-apps-service"></a>Azure Logic Apps-service

Alle logische apps in een regio gebruiken dezelfde bereiken van IP-adressen. Ter ondersteuning van de aanroepen die logische apps rechtstreeks met [HTTP](../connectors/connectors-native-http.md), [HTTP + Swagger](../connectors/connectors-native-http-swagger.md), en andere HTTP-aanvragen, de firewallconfiguraties van uw instellen, zodat ze deze binnenkomende en uitgaande IP-adressen bevatten, op basis van op waar uw logische apps bestaan:

| Logic Apps-regio | Uitgaande IP |
|-------------------|-------------|
| Australië - oost | 13.75.149.4, 104.210.91.55, 104.210.90.241 |
| Australië - zuidoost | 13.73.114.207, 13.77.3.139, 13.70.159.205 |
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

| Logic Apps-regio | Inkomende IP |
|-------------------|------------|
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
| Zuid-centraal VS | 13.65.98.39, 13.84.41.46, 13.84.43.45 |
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

Ter ondersteuning van de aanroepen die [connectors](../connectors/apis-list.md) maken, de firewallconfiguraties van uw instellen, zodat ze deze uitgaande IP-adressen bevatten op basis van de regio's waar uw logische apps bestaan.

> [!IMPORTANT]
>
> Als u bestaande configuraties hebt, werkt u deze **zo snel mogelijk vóór 1 September 2018** zodat ze zijn en overeenkomen met de IP-adressen in deze lijst voor de regio's waar uw logische apps bestaan. 

| Logic Apps-regio | Uitgaande IP | 
|-------------------|-------------|  
| Australië - oost | 13.70.72.192 - 13.70.72.207, 13.72.243.10, 40.126.251.213 | 
| Australië - zuidoost | 13.77.50.240 - 13.77.50.255, 13.70.136.174, 40.127.80.34 | 
| Brazilië - zuid | 191.233.203.192 - 191.233.203.207, 104.41.59.51, 191.232.38.129 | 
| Canada - midden | 13.71.170.208 - 13.71.170.223, 13.71.170.224 - 13.71.170.239, 52.237.24.126, 52.233.31.197, 52.228.42.205, 52.228.33.76, 52.228.34.13, 52.233.26.83 | 
| Canada - oost | 40.69.106.240 - 40.69.106.255, 52.242.35.152, 52.229.123.98, 52.229.120.178, 52.229.126.202, 52.229.120.52, 52.229.120.131 | 
| Centraal-India | 104.211.81.192 - 104.211.81.207, 52.172.211.12, 104.211.98.164 | 
| VS - midden | 13.89.171.80 - 13.89.171.95, 52.173.245.164, 40.122.49.51 | 
| Oost-Azië | 13.75.36.64 - 13.75.36.79, 52.175.23.169, 23.99.116.181 | 
| VS - oost | 40.71.11.80 - 40.71.11.95, 40.71.249.205, 191.237.41.52 | 
| VS - oost 2 | 40.70.146.208 - 40.70.146.223, 52.232.188.154, 104.208.233.100 | 
| Japan - oost | 13.78.108.0 - 13.78.108.15, 13.71.153.19, 40.115.186.96 | 
| Japan - west | 40.74.100.224 - 40.74.100.239, 104.215.61.248, 40.74.130.77 | 
| Noord-centraal VS | 52.162.107.160 - 52.162.107.175, 52.162.242.161, 65.52.218.230 | 
| Noord-Europa | 13.69.227.208 - 13.69.227.223, 52.178.150.68, 104.45.93.9 | 
| Zuid-centraal VS | 104.214.19.48 - 104.214.19.63, 13.65.86.57, 104.214.70.191 | 
| Zuid-India | 40.78.194.240 - 40.78.194.255, 13.71.125.22, 104.211.227.225 | 
| Zuidoost-Azië | 13.67.8.240 - 13.67.8.255, 52.187.68.19, 13.76.231.68 | 
| West-centraal VS | 13.71.195.32 - 13.71.195.47, 52.161.102.22, 52.161.27.108, 52.161.30.5, 52.161.29.35, 52.161.26.212, 52.161.128 | 
| West-Europa | 13.69.64.208 - 13.69.64.223, 52.174.88.118, 40.115.50.13 | 
| West-India | 104.211.146.224 - 104.211.146.239, 104.211.189.218, 104.211.161.203 | 
| VS - west | 40.112.243.160 - 40.112.243.175, 104.42.122.49, 104.40.51.248 | 
| VS - west 2 | 13.66.140.128 - 13.66.140.143, 52.183.78.157, 13.66.225.219, 13.66.218.78, 13.66.220.135, 13.66.219.14, 13.66.221.19 | 
| Verenigd Koninkrijk Zuid | 51.140.148.0 - 51.140.148.15, 51.140.80.51 | 
| Verenigd Koninkrijk West | 51.140.211.0 - 51.140.211.15, 51.141.47.105 | 
| | | 

## <a name="next-steps"></a>Volgende stappen  

* Meer informatie over het [uw eerste logische app maken](../logic-apps/quickstart-create-first-logic-app-workflow.md)  
* Meer informatie over [algemene voorbeelden en scenario's](../logic-apps/logic-apps-examples-and-scenarios.md)
