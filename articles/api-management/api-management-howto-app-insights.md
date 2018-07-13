---
title: Hoe u Azure API Management te integreren met Azure Application Insights | Microsoft Docs
description: Leer hoe u aan te melden en gebeurtenissen van Azure API Management in Azure Application Insights weergeven.
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
ms.openlocfilehash: 8546b1228c8d8f213cb87692144e8d1d31a949d8
ms.sourcegitcommit: e0a678acb0dc928e5c5edde3ca04e6854eb05ea6
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/13/2018
ms.locfileid: "39001800"
---
# <a name="how-to-integrate-azure-api-management-with-azure-application-insights"></a>Hoe u Azure API Management te integreren met Azure Application Insights

Azure API Management kunnen zo eenvoudig worden geïntegreerd met Azure Application Insights - een uitbreidbare service voor webontwikkelaars bouwen en beheren van apps op meerdere platforms. Deze handleiding helpt bij elke stap van deze integratie en beschrijft strategieën voor het verminderen van invloed op de prestaties van uw API Management service-exemplaar.

## <a name="prerequisites"></a>Vereisten

Als u wilt volgen van deze handleiding, moet u een Azure API Management-exemplaar. Als u niet hebt, voert u de [zelfstudie](get-started-create-service-instance.md) eerste.

## <a name="create-an-azure-application-insights-instance"></a>Azure Application Insights-exemplaar maken

Voordat u Azure Application Insights gebruiken kunt, moet u eerst een exemplaar van de service te maken.

1. Open de **Azure-portal** en navigeer naar **Application Insights**.  
    ![App Insights maken](media/api-management-howto-app-insights/apim-app-insights-instance-1.png)  
2. Klik op **+ Toevoegen**.  
    ![App Insights maken](media/api-management-howto-app-insights/apim-app-insights-instance-2.png)  
3. Vul het formulier. Selecteer **algemene** als de **toepassingstype**.
4. Klik op **Create**.

## <a name="create-a-connection-between-azure-application-insights-and-azure-api-management-service-instance"></a>Maak een verbinding tussen Azure Application Insights en Azure API Management service-exemplaar

1. Navigeer naar uw **Azure API Management service-exemplaar** in de **Azure-portal**.
2. Selecteer **Application Insights** in het menu aan de linkerkant.
3. Klik op **+ Toevoegen**.  
    ![App Insights logger](media/api-management-howto-app-insights/apim-app-insights-logger-1.png)  
4. Selecteer het eerder gemaakte **Application Insights** exemplaar en geef een korte beschrijving.
5. Klik op **Create**.
6. U hebt een Azure Application Insights-logger zojuist hebt gemaakt met een instrumentatiesleutel. Er wordt nu weergegeven in de lijst.  
    ![App Insights logger](media/api-management-howto-app-insights/apim-app-insights-logger-2.png)  

> [!NOTE]
> Achter de scène, een [Logger](https://docs.microsoft.com/en-us/rest/api/apimanagement/logger/createorupdate) entiteit is gemaakt in uw exemplaar van API Management, met de Instrumentatiesleutel van de Application Insights-exemplaar.

## <a name="enable-application-insights-logging-for-your-api"></a>Application Insights logboekregistratie inschakelen voor uw API

1. Navigeer naar uw **Azure API Management service-exemplaar** in de **Azure-portal**.
2. Selecteer **API's** in het menu aan de linkerkant.
3. Klik op uw API, in dit geval **Demo Conference API**.
4. Ga naar de **instellingen** tabblad in de bovenste balk.
5. Schuif omlaag naar de **diagnoselogboeken** sectie.  
    ![App Insights logger](media/api-management-howto-app-insights/apim-app-insights-api-1.png)  
6. Controleer de **inschakelen** vak.
7. Selecteer uw gekoppelde logger in de **bestemming** vervolgkeuzelijst.
8. Invoer **100** als **steekproeven (%)** en vink het **fouten in het altijd** selectievakje.
9. Invoer **1024** in de **eerste bytes van de hoofdtekst van** veld.
10. Klik op **Opslaan**.

> [!NOTE]
> Achter de scène, een [diagnostische](https://docs.microsoft.com/en-us/rest/api/apimanagement/diagnostic/createorupdate) entiteit met de naam 'Application Insights' wordt gemaakt op de API-niveau.

| De naam van instelling                        | Waardetype                        | Beschrijving                                                                                                                                                                                                                                                                                                                                      |
|-------------------------------------|-----------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Inschakelen                              | booleaans                           | Hiermee geeft u op of de registratie van deze API is ingeschakeld.                                                                                                                                                                                                                                                                                                |
| Doel                         | Azure Application Insights-logboek | Hiermee geeft u de Azure Application Insights logger moet worden gebruikt                                                                                                                                                                                                                                                                                           |
| Steekproeven (%)                        | decimaal                           | De waarden tussen 0 en 100 (procent). <br/> Hiermee geeft u op welk percentage van de aanvragen worden geregistreerd voor Azure Application Insights. 0% steekproeven betekent nul aanvragen die zijn vastgelegd, terwijl steekproeven van 100% betekent alle aanvragen die zijn geregistreerd dat. <br/> Deze instelling wordt gebruikt voor het verminderen van de gevolgen voor de prestaties van logboekregistratie van aanvragen naar Azure Application Insights (Zie de sectie hieronder). |
| Altijd fouten in het logboek                   | booleaans                           | Als deze instelling is ingeschakeld, alle fouten worden geregistreerd voor Azure Application Insights, ongeacht de **steekproeven** instelling.                                                                                                                                                                                                                  |
| Basisopties: Headers              | lijst                              | Hiermee geeft u de headers die worden geregistreerd voor Azure Application Insights voor aanvragen en antwoorden.  Standaard: geen headers worden geregistreerd.                                                                                                                                                                                                             |
| Eenvoudige opties: Eerste bytes van de hoofdtekst  | geheel getal                           | Hiermee geeft u op hoeveel eerste bytes van de instantie worden geregistreerd in Azure Application Insights voor aanvragen en antwoorden.  Standaard: hoofdtekst is niet geregistreerd.                                                                                                                                                                                              |
| Geavanceerde opties: Frontend-aanvraag  |                                   | Hiermee geeft u op of en hoe *frontend aanvragen* naar Azure Application Insights worden geregistreerd. *Frontend-aanvraag* is een aanvraag binnenkomende naar de Azure API Management-service.                                                                                                                                                                        |
| Geavanceerde opties: Frontend antwoord |                                   | Hiermee geeft u op of en hoe *frontend antwoorden* naar Azure Application Insights worden geregistreerd. *Frontend-antwoord* is een antwoord uitgaande van de Azure API Management-service.                                                                                                                                                                   |
| Geavanceerde opties: Back-end-aanvraag   |                                   | Hiermee geeft u op of en hoe *back-end aanvragen* naar Azure Application Insights worden geregistreerd. *Back-end-aanvraag* is een aanvraag van uitgaande van de Azure API Management-service.                                                                                                                                                                        |
| Geavanceerde opties: Back-end-antwoord  |                                   | Hiermee geeft u op of en hoe *back-end antwoorden* naar Azure Application Insights worden geregistreerd. *Back-end antwoord* is een antwoord binnenkomende naar de Azure API Management-service.                                                                                                                                                                       |

> [!NOTE]
> U kunt u kunt op verschillende niveaus - één API logger of een logger voor alle API's.
>  
> Als u beide opgeeft:
> + Als ze verschillende kunt zijn, worden beide providers gebruikt (multiplexing Logboeken),
> + Als ze de dezelfde kunt hebben, maar verschillende instellingen en één voor één API (meer gedetailleerd niveau) één voor alle API's wordt overschreven.

## <a name="what-data-is-added-to-azure-application-insights"></a>Welke gegevens worden toegevoegd aan Azure Application Insights

Azure Application Insights ontvangt:

+ *Aanvraag* telemetrie-item voor elke inkomende aanvraag (*frontend aanvraag*, *frontend antwoord*),
+ *Afhankelijkheid* telemetrie-item voor elke aanvraag doorgestuurd naar een back-endservice (*back-end-aanvraag*, *back-end antwoord*),
+ *Uitzondering* telemetrie-item voor elke mislukte aanvragen.

Een mislukte aanvragen is een aanvraag die:

+ is mislukt vanwege een gesloten clientverbinding, of
+ geactiveerd een *bij fout* sectie van de API-beleid, of
+ heeft een antwoord HTTP-status overeenkomende 4xx of 5xx.

## <a name="performance-implications-and-log-sampling"></a>Gevolgen voor de prestaties en log steekproeven

> [!WARNING]
> Logboekregistratie van alle gebeurtenissen kan ernstige prestaties gevolgen hebben, afhankelijk van de aanvragen frequentie van binnenkomst.

Op basis van de interne load tests, veroorzaakt inschakelen van deze functie een 40-50% reductie in doorvoer wanneer snelheid van aanvragen voor 1000 aanvragen per seconde is overschreden. Azure Application Insights is ontworpen om statistische analyses gebruiken voor het beoordelen van de prestaties van de toepassing. Het is niet bedoeld als een systeem controlebeleid en is niet geschikt voor het registreren van elke afzonderlijke aanvraag voor grote volumes API's.

U kunt het aantal aanvragen dat is vastgelegd door aan te passen bewerken de **steekproeven** instellen (Zie de bovenstaande stappen). De waarde 100% betekent dat die alle aanvragen worden vastgelegd, terwijl 0% weerspiegelt helemaal geen logboekregistratie. **Sampling** helpt te verminderen de hoeveelheid telemetrie, effectief zo wordt voorkomen dat aanzienlijk slechtere prestaties, terwijl u nog steeds uitvoering van de voordelen van logboekregistratie.

Logboekregistratie van headers en hoofdtekst van aanvragen en antwoorden wordt overgeslagen is ook positieve invloed op prestatieproblemen te verlichten.

## <a name="next-steps"></a>Volgende stappen

+ Meer informatie over [Azure Application Insights](https://docs.microsoft.com/en-us/azure/application-insights/).
+ Houd rekening met [logboekregistratie met Azure Event Hubs](api-management-howto-log-event-hubs.md).
