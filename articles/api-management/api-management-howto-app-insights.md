---
title: Azure API Management te integreren met Azure Application Insights | Microsoft Docs
description: Informatie over het vastleggen en gebeurtenissen van Azure API Management in Azure Application Insights te bekijken.
services: api-management
documentationcenter: ''
author: mikebudzynski
manager: erikre
editor: ''
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/20/2018
ms.author: apimpm
ms.openlocfilehash: 7740da505f7635944536252d60ec2c2039295975
ms.sourcegitcommit: 65b399eb756acde21e4da85862d92d98bf9eba86
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/22/2018
ms.locfileid: "36320581"
---
# <a name="how-to-integrate-azure-api-management-with-azure-application-insights"></a>Azure API Management te integreren met Azure Application Insights

Azure API Management kunt u eenvoudige integratie met Azure Application Insights - service voor een uitbreidbare voor webontwikkelaars maken en beheren van apps op meerdere platforms. Deze handleiding wordt begeleid bij elke stap van deze integratie en beschrijft strategieën voor het beperken van invloed op de prestaties van uw API Management-service-exemplaar.

## <a name="prerequisites"></a>Vereisten

Wilt u deze handleiding, moet u een exemplaar van Azure API Management. Als u niet hebt, voert u de [zelfstudie](get-started-create-service-instance.md) eerste.

## <a name="create-an-azure-application-insights-instance"></a>Azure Application Insights-exemplaar maken

Voordat u Azure Application Insights gebruiken kunt, moet u eerst een exemplaar van de service te maken.

1. Open de **Azure-portal** en navigeer naar **Application Insights**.  
    ![App Insights maken](media/api-management-howto-app-insights/apim-app-insights-instance-1.png)  
2. Klik op **+ Toevoegen**.  
    ![App Insights maken](media/api-management-howto-app-insights/apim-app-insights-instance-2.png)  
3. Vul het formulier. Selecteer **algemene** als de **toepassingstype**.
4. Klik op **Create**.

## <a name="create-a-connection-between-azure-application-insights-and-azure-api-management-service-instance"></a>Een verbinding maken tussen Azure Application Insights en Azure API Management-service-exemplaar

1. Navigeer naar uw **Azure API Management-service-exemplaar** in de **Azure-portal**.
2. Selecteer **Application Insights** in het menu aan de linkerkant.
3. Klik op **+ Toevoegen**.  
    ![App Insights berichtenlogboek](media/api-management-howto-app-insights/apim-app-insights-logger-1.png)  
4. Selecteer de eerder gemaakte **Application Insights** -instantie en geef een korte beschrijving.
5. Klik op **Create**.
6. U hebt een Azure Application Insights berichtenlogboek zojuist hebt gemaakt met een instrumentatiesleutel. Er wordt nu weergegeven in de lijst.  
    ![App Insights berichtenlogboek](media/api-management-howto-app-insights/apim-app-insights-logger-2.png)  

## <a name="enable-application-insights-logging-for-your-api"></a>Application Insights logboekregistratie inschakelen voor uw API

1. Navigeer naar uw **Azure API Management-service-exemplaar** in de **Azure-portal**.
2. Selecteer **API's** in het menu aan de linkerkant.
3. Klik op uw API in dit geval **Demo conferentie API**.
4. Ga naar de **instellingen** tabblad van de bovenste balk.
5. Schuif omlaag naar de **diagnostische logboeken** sectie.  
    ![App Insights berichtenlogboek](media/api-management-howto-app-insights/apim-app-insights-api-1.png)  
6. Controleer de **inschakelen** vak.
7. Selecteer uw gekoppelde berichtenlogboek in de **bestemming** vervolgkeuzelijst.
8. Invoer **100** als **samplingfrequentie (%)** en maatstreepjes de **fouten in het logboek altijd** selectievakje.
9. Invoer **1024** in de **eerste bytes van de tekst van** veld.
10. Klik op **Opslaan**.

| Naam van de instelling                        | Waardetype                        | Beschrijving                                                                                                                                                                                                                                                                                                                                      |
|-------------------------------------|-----------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Inschakelen                              | booleaans                           | Hiermee geeft u op of de registratie van deze API is ingeschakeld.                                                                                                                                                                                                                                                                                                |
| Doel                         | Azure Application Insights-logboek | Hiermee geeft u Azure Application Insights berichtenlogboek moet worden gebruikt                                                                                                                                                                                                                                                                                           |
| Steekproef nemen (%)                        | decimale                           | De waarden van 0 tot 100 (procent). <br/> Hiermee geeft u op welk percentage van aanvragen naar Azure Application Insights worden geregistreerd. 0% steekproeven betekent nul aanvragen dat is geregistreerd, terwijl de steekproeven van 100% betekent dat alle aanvragen in het logboek geregistreerd. <br/> Deze instelling wordt gebruikt voor het beperken van de prestatie van netwerkbestandsscenario van logboekregistratie van aanvragen naar Azure Application Insights (Zie de sectie hieronder). |
| Altijd fouten in het logboek                   | booleaans                           | Als deze instelling is ingeschakeld, alle fouten worden geregistreerd met Azure Application Insights, ongeacht de **steekproeven** instelling.                                                                                                                                                                                                                  |
| Eenvoudige opties: koppen              | lijst                              | Hiermee geeft u de headers die voor aanvragen en antwoorden naar Azure Application Insights worden geregistreerd.  Standaard: geen headers worden geregistreerd.                                                                                                                                                                                                             |
| Eenvoudige opties: Eerste bytes van de tekst  | geheel getal                           | Hiermee geeft u op hoeveel van de eerste bytes van de instantie worden geregistreerd in Azure Application Insights voor aanvragen en antwoorden.  Standaardwaarde: instantie is niet geregistreerd.                                                                                                                                                                                              |
| Geavanceerde opties: Frontend-aanvraag  |                                   | Hiermee geeft u op of en hoe *frontend aanvragen* naar Azure Application Insights worden geregistreerd. *Frontend-aanvraag* is een verzoek binnenkomende naar de Azure API Management-service.                                                                                                                                                                        |
| Geavanceerde opties: Frontend-antwoord |                                   | Hiermee geeft u op of en hoe *frontend antwoorden* naar Azure Application Insights worden geregistreerd. *Frontend-antwoord* is een antwoord van de Azure API Management-service uitgaand.                                                                                                                                                                   |
| Geavanceerde opties: Back-end-aanvraag   |                                   | Hiermee geeft u op of en hoe *back-end aanvragen* naar Azure Application Insights worden geregistreerd. *Back-end-aanvraag* is een aanvraag voor uitgaande van de Azure API Management-service.                                                                                                                                                                        |
| Geavanceerde opties: Back-end-antwoord  |                                   | Hiermee geeft u op of en hoe *back-end-antwoorden* naar Azure Application Insights worden geregistreerd. *Back-end antwoord* is een antwoord binnenkomende naar de Azure API Management-service.                                                                                                                                                                       |

> [!NOTE]
> U kunt voorkomen op verschillende niveaus - logboek met één API of een logboek voor alle API's opgeven.
>  
> Als u beide opgeven:
> + Als ze verschillende voorkomen, worden beide parameters gebruikt (multiplex Logboeken),
> + Als ze zijn de dezelfde voorkomen maar verschillende instellingen hebben en één voor één API (meer gedetailleerd niveau) een voor alle API's wordt overschreven.

## <a name="what-data-is-added-to-azure-application-insights"></a>Welke gegevens worden toegevoegd aan Azure Application Insights

Azure Application Insights ontvangt:

+ *Aanvraag* telemetrie-item, voor elke inkomende aanvraag (*frontend aanvraag*, *frontend antwoord*),
+ *Afhankelijkheid* telemetrie-item, voor elke aanvraag doorgestuurd naar een back-endservice (*back-end-aanvraag*, *back-end antwoord*),
+ *Uitzondering* telemetrie-item, voor elke mislukte aanvragen.

Een mislukte aanvraag is een aanvraag die:

+ is mislukt vanwege een gesloten clientverbinding of
+ geactiveerd een *bij fouten* sectie van het beleid voor API of
+ heeft een antwoord HTTP-status code overeenkomende 4xx of 5xx.

## <a name="performance-implications-and-log-sampling"></a>Gevolgen van de prestaties en logboek steekproeven

> [!WARNING]
> Logboekregistratie van alle gebeurtenissen kan prestaties ernstige gevolgen hebben, afhankelijk van de snelheid van inkomende aanvragen.

Op basis van de interne load tests, veroorzaakt u deze functie inschakelt een 40-50% reductie in doorvoer wanneer aanvraagsnelheid 1000 aanvragen per seconde is overschreden. Azure Application Insights is ontworpen voor gebruik van statistische analyse voor de beoordeling van de prestaties van de toepassing. Het is niet bedoeld als een audit-systeem en is niet geschikt voor het registreren van elke afzonderlijke aanvraag voor grote hoeveelheden API's.

U kunt het aantal aanvragen worden geregistreerd door aan te passen bewerken de **steekproeven** instellen (Zie de bovenstaande stappen). De waarde van 100% betekent dat die alle aanvragen worden geregistreerd, terwijl 0% weerspiegelt helemaal geen logboekregistratie. **Steekproef nemen** helpt te verminderen volume telemetrie, effectief zo wordt voorkomen dat slechtere aanzienlijke prestaties, terwijl u nog steeds uitvoering van de voordelen van logboekregistratie.

Logboekregistratie van kopteksten en de hoofdtekst van aanvragen en antwoorden wordt overgeslagen wordt ook positieve gevolgen hebben op prestatieproblemen te verlichten.

## <a name="next-steps"></a>Volgende stappen

+ Meer informatie over [Azure Application Insights](https://docs.microsoft.com/en-us/azure/application-insights/).
+ Houd rekening met [logboekregistratie met Azure Event Hubs](api-management-howto-log-event-hubs.md).
