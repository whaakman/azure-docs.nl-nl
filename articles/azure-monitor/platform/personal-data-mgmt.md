---
title: Richtlijnen voor persoonlijke gegevens die zijn opgeslagen in Azure Log Analytics | Microsoft Docs
description: In dit artikel wordt beschreven hoe u persoonlijke gegevens die zijn opgeslagen in Azure Log Analytics en de methoden om te bepalen en verwijdert u deze beheren.
services: log-analytics
documentationcenter: ''
author: mgoedtel
manager: carmonm
editor: ''
ms.assetid: ''
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 05/18/2018
ms.author: magoedte
ms.component: ''
ms.openlocfilehash: be14b560eb48adc2fcf0ad0a1cf7fe27792a402a
ms.sourcegitcommit: 2469b30e00cbb25efd98e696b7dbf51253767a05
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/06/2018
ms.locfileid: "53002488"
---
# <a name="guidance-for-personal-data-stored-in-log-analytics-and-application-insights"></a>Richtlijnen voor persoonlijke gegevens die zijn opgeslagen in Log Analytics en Application Insights

Log Analytics is een gegevensarchief daar persoonlijke gegevens kunnen worden gevonden. Application Insights worden de gegevens opslaat in een Log Analytics-partitie. In dit artikel wordt uitgelegd waar in Log Analytics en Application Insights dergelijke gegevens doorgaans wordt gevonden, evenals de mogelijkheden die voor u beschikbaar zijn voor het afhandelen van dergelijke gegevens.

> [!NOTE]
> Voor de doeleinden van dit artikel _logboekgegevens_ verwijst naar gegevens die worden verzonden naar Log Analytics-werkruimte, terwijl _toepassingsgegevens_ verwijst naar gegevens die zijn verzameld door Application Insights.

[!INCLUDE [gdpr-dsr-and-stp-note](../../../includes/gdpr-dsr-and-stp-note.md)]

## <a name="strategy-for-personal-data-handling"></a>Strategie voor de verwerking van persoonlijke gegevens

Terwijl deze worden tot u en uw bedrijf uiteindelijk bepalen de strategie waarbij u uw persoonlijke afhandelt gegevens (indien helemaal), wordt hier volgen enkele mogelijke strategieën. Ze worden weergegeven in de volgorde van voorkeur vanuit een technisch oogpunt van de meeste te minste voorkeur:

* Waar mogelijk, verzameling van stoppen, onleesbaar maakt, anoniem maken of andere wijze aanpassen, de gegevens worden verzameld uit de gebeurtenis wordt beschouwd als 'private' te sluiten. Dit is _veruit_ de aanpak van voorkeur, zodat u niet hoeft te maken van een strategie voor de afhandeling zeer kostbaar en meer impact van gegevens.
* Indien niet mogelijk is, kunt u proberen te normaliseren van de gegevens voor de op het data platform en de prestaties worden verminderd. Maak bijvoorbeeld een lookup-gegevens die wordt correleren van de gebruikersnaam en de bijbehorende gegevens die vervolgens ergens anders kunnen worden geregistreerd in een interne id in plaats van de logboekregistratie van een expliciete gebruikers-ID. Op die manier moet een van uw gebruikers vragen om te verwijderen van hun persoonlijke gegevens, is het mogelijk dat alleen verwijderen van de rij in de opzoektabel overeenkomt met de gebruiker voldoende zal zijn. 
* Als persoonlijke gegevens moeten worden verzameld, bouw ten slotte een proces om het opschonen van API-pad en de bestaande query API pad om te voldoen aan de verplichtingen die u mogelijk om exporteren en verwijderen van persoonlijke gegevens die zijn gekoppeld aan een gebruiker. 

## <a name="where-to-look-for-private-data-in-log-analytics"></a>Waar om te zoeken naar persoonlijke gegevens in Log Analytics?

Log Analytics is een flexibele opslag, die tijdens het voorschrijven van een schema dat u wilt uw gegevens, kunt u elk veld met uw eigen waarden. Een aangepaste schema kan bovendien worden opgenomen. Daarom is het niet mogelijk om in te spreken precies waar persoonlijke gegevens worden gevonden in uw specifieke werkruimte. De volgende locaties zijn echter goed startpunt in uw inventaris:

### <a name="log-data"></a>Logboekgegevens

* *IP-adressen*: Log Analytics verzamelt een verscheidenheid aan IP-informatie in veel verschillende tabellen. De volgende query ziet u bijvoorbeeld alle tabellen waarbij IPv4-adressen in de afgelopen 24 uur zijn verzameld:
    ```
    search * 
    | where * matches regex @'\b((25[0-5]|2[0-4][0-9]|[01]?[0-9][0-9]?)(\.|$)){4}\b' //RegEx originally provided on https://stackoverflow.com/questions/5284147/validating-ipv4-addresses-with-regexp
    | summarize count() by $table
    ```
* *Gebruikers-id's*: gebruikers-id's vindt u in een grote verscheidenheid aan oplossingen en tabellen. U kunt zoeken naar een specifieke gebruikersnaam in uw hele gegevensset met de zoekopdracht:
    ```
    search "[username goes here]"
    ```
Houd er rekening mee om te zoeken, niet alleen voor mensen leesbare gebruikersnamen, maar ook GUID's die kunnen rechtstreeks worden gerelateerd aan een bepaalde gebruiker!
* *Apparaat-id's*: zoals gebruikers-id's, apparaat-id's worden soms ' als privé beschouwd '. Zoals hierboven vermeld voor de gebruikers-id's dezelfde benadering gebruiken om te identificeren van tabellen wanneer dit wordt mogelijk een probleem. 
* *Aangepaste gegevens*: Log Analytics kunt u de verzameling in een aantal methoden: aangepaste logboeken en aangepaste velden, de [HTTP Data Collector API](../../azure-monitor/platform/data-collector-api.md) , en aangepaste gegevens worden verzameld als onderdeel van gebeurtenislogboeken van systeem. Al deze waarden zijn vatbaar voor die persoonlijke gegevens bevatten en moet worden onderzocht om te controleren of er dergelijke gegevens bestaat.
* *Oplossing vastgelegde gegevens*: omdat het mechanisme voor oplossing een open is, wordt aanbevolen alle tabellen die zijn gegenereerd door oplossingen om de naleving controleren.

### <a name="application-data"></a>Toepassingsgegevens

* *IP-adressen*: terwijl Application Insights wordt standaard, onleesbaar maakt alle velden van IP-adres op '0.0.0.0', is het een vrij algemeen patroon voor de onderdrukking van deze waarde met de werkelijke gebruikers-IP-adres om sessiegegevens te beheren. De onderstaande Analytics-query kan worden gebruikt om een tabel die de waarden in de kolom IP-adres dan "0.0.0.0" in de afgelopen 24 uur bevat te vinden:
    ```
    search client_IP != "0.0.0.0"
    | where timestamp > ago(1d)
    | summarize numNonObfuscatedIPs_24h = count() by $table
    ```
* *Gebruikers-id's*: standaard, Application Insights willekeurig gegenereerde id wordt gebruikt voor de gebruiker en voor sessie bijhouden. Het is echter gebruikelijk om te zien van deze velden worden genegeerd voor het opslaan van een meer relevant zijn voor de toepassing-ID. Bijvoorbeeld: gebruikersnamen, AAD-GUID's, enzovoort. Deze id's zijn vaak beschouwd als binnen de regeling vallen als persoonlijke gegevens, en daarom moet worden verwerkt op de juiste wijze. Onze aanbeveling is altijd proberen te verbergen, weergeven of anoniem maken van deze id. Dit zijn enkele velden waarin deze waarden worden vaak gevonden type session_Id, user_Id, user_AuthenticatedId, user_AccountId, evenals customDimensions.
* *Aangepaste gegevens*: Application Insights kunt u een set aangepaste dimensies toevoegen aan elk gegevenstype. Deze dimensies kunnen worden *eventuele* gegevens. Gebruik de volgende query uit om te identificeren die worden verzameld in de afgelopen 24 uur aangepaste dimensies:
    ```
    search * 
    | where isnotempty(customDimensions)
    | where timestamp > ago(1d)
    | project $table, timestamp, name, customDimensions 
    ```
* *Gegevens in het geheugen en in-transit*: Application Insights wilt bijhouden, uitzonderingen, aanvragen, afhankelijkheidsaanroepen en traceringen. Persoonlijke gegevens kunnen vaak worden verzameld op de code en het niveau van de HTTP-aanroep. Bekijk de uitzonderingen, aanvragen, afhankelijkheden en traceringen tabellen voor het identificeren van dergelijke gegevens. Gebruik [telemetrie initializers](https://docs.microsoft.com/azure/application-insights/app-insights-api-filtering-sampling) waar mogelijk op deze gegevens, onleesbaar maakt.
* *Snapshot Debugger opnamen*: de [Snapshot Debugger](https://docs.microsoft.com/azure/application-insights/app-insights-snapshot-debugger) functie in Application Insights kunt u voor het verzamelen van momentopnamen voor foutopsporing, telkens wanneer een uitzondering is opgetreden op de productie-instantie van uw toepassing. Momentopnamen wordt de volledige stack-trace leidt tot de uitzonderingen, evenals de waarden voor lokale variabelen bij elke stap in de stack weergegeven. Deze functie is helaas niet toegestaan voor selectieve verwijdering van uitlijnpunten of programmatische toegang tot gegevens in de momentopname. Als de retentie-frequentie van de standaard-momentopnamen niet voldoet aan uw vereisten voor naleving, moet de aanbeveling is daarom de functie uitschakelen.

## <a name="how-to-export-and-delete-private-data"></a>Het exporteren en verwijderen van persoonlijke gegevens

Zoals vermeld in de [strategie voor de verwerking van persoonlijke gegevens](#strategy-for-personal-data-handling) eerdere sectie, is het __sterk__ aanbevolen als het mogelijk om te herstructureren van uw beleid voor het uitschakelen van de verzameling van verzameling persoonlijke gegevens, obfuscating of deze anoniem of anderszins wijzigen om te verwijderen uit de gebeurtenis wordt beschouwd als 'persoonlijke'. Verwerken van de gegevens worden voorste leiden tot kosten bij u en uw team te definiëren en het automatiseren van een strategie, het bouwen van een interface voor uw klanten om te communiceren met hun gegevens via en constant onderhoudskosten. Bovendien is het rekenintensief kostbare voor Log Analytics en Application Insights en een groot aantal gelijktijdige query of opschonen van API-aanroepen hebben de potentiële negatieve invloed hebben op alle interactie met Log Analytics-functionaliteit. Dat gezegd, er zijn inderdaad enkele geldige scenario's waar de persoonlijke gegevens moeten worden verzameld. Voor deze gevallen moet de gegevens worden verwerkt zoals beschreven in deze sectie.

[!INCLUDE [gdpr-intro-sentence](../../../includes/gdpr-intro-sentence.md)]

### <a name="view-and-export"></a>Weergeven en exporteren

Voor beide weergeven en exporteren van aanvragen voor gegevens, de [Log Analytics-query API](https://dev.loganalytics.io/) of de [Application Insights query API](https://dev.applicationinsights.io/quickstart) moet worden gebruikt. Logica voor het converteren van de vorm van de gegevens naar een geschikt om te leveren aan uw gebruikers worden aan u om te implementeren. [Azure Functions](https://azure.microsoft.com/services/functions/) maakt een mooie plek voor het hosten van dergelijke logica.

### <a name="delete"></a>Verwijderen

> [!WARNING]
> Verwijderingen in Log Analytics zijn schadelijke en niet-omkeerbare! Gebruik zeer voorzichtig te werk in de uitvoering ervan.

We hebben aangebracht beschikbaar als onderdeel van de verwerking van een privacy een *opschonen* API-pad. Dit pad moet spaarzaam worden gebruikt vanwege het risico dat samenhangt met in dat geval, de mogelijke invloed op de prestaties en het potentieel scheeftrekken gehele aggregaties, metingen en andere aspecten van uw Log Analytics-gegevens. Zie de [strategie voor de verwerking van persoonlijke gegevens](#strategy-for-personal-data-handling) sectie voor alternatieve methoden voor het verwerken van persoonlijke gegevens.

Opschonen van Logboeken is een zeer bevoegde bewerking dat er geen app of gebruiker in Azure (met inbegrip van zelfs de resource-eigenaar) hebben machtigingen om uit te voeren zonder expliciet worden verleend een rol in Azure Resource Manager. Deze rol is _gegevens Purger_ en moet voorzichtig worden overgedragen vanwege de mogelijkheden voor preventie van gegevensverlies. 

Nadat de Azure Resource Manager-rol is toegewezen, zijn twee nieuwe API-paden zijn beschikbaar: 

#### <a name="log-data"></a>Logboekgegevens

* [Opschonen na](https://docs.microsoft.com/rest/api/loganalytics/workspaces%202015-03-20/purge) - wordt een object op te geven parameters van de gegevens te verwijderen en retourneert een verwijzing GUID 
* OPHALEN leegmaken status - de POST-aanroep voor opschonen van een 'x-ms-status-location'-header die bevat een URL die u aanroepen kunt om te bepalen van de status van uw API opschonen wordt geretourneerd. Bijvoorbeeld:

    ```
    x-ms-status-location: https://management.azure.com/subscriptions/[SubscriptionId]/resourceGroups/[ResourceGroupName]/providers/Microsoft.OperatonalInsights/workspaces/[WorkspaceName]/operations/purge-[PurgeOperationId]?api-version=2015-03-20
    ```

> [!IMPORTANT]
>  Terwijl we verwachten dat de meeste opschonen bewerkingen om uit te voeren veel sneller dan onze SLA, vanwege de zware impact op het data-platform dat door Log Analytics, **een formele SLA voor de voltooiing van leegmaken is ingesteld op 30 dagen**. 

#### <a name="application-data"></a>Toepassingsgegevens

* [Opschonen na](https://docs.microsoft.com/rest/api/application-insights/components/purge) - wordt een object op te geven parameters van de gegevens te verwijderen en retourneert een verwijzing GUID
* OPHALEN leegmaken status - de POST-aanroep voor opschonen van een 'x-ms-status-location'-header die bevat een URL die u aanroepen kunt om te bepalen van de status van uw API opschonen wordt geretourneerd. Bijvoorbeeld:

   ```
   x-ms-status-location: https://management.azure.com/subscriptions/[SubscriptionId]/resourceGroups/[ResourceGroupName]/providers/microsoft.insights/components/[ComponentName]/operations/purge-[PurgeOperationId]?api-version=2015-05-01
   ```

> [!IMPORTANT]
>  Hoewel de meeste opschonen bewerkingen mogelijk veel sneller dan de SLA, vanwege de zware impact op het data-platform dat door Application Insights, voltooid **een formele SLA voor de voltooiing van leegmaken is ingesteld op 30 dagen**.

## <a name="next-steps"></a>Volgende stappen
- Zie voor meer informatie over hoe Log Analytics-gegevens wordt verzameld, verwerkt en beveiligd, [Log Analytics-gegevensbeveiliging](../../azure-monitor/platform/data-security.md).
- Zie voor meer informatie over hoe Application Insights-gegevens wordt verzameld, verwerkt en beveiligd, [Application Insights-gegevensbeveiliging](../../application-insights/app-insights-data-retention-privacy.md).
