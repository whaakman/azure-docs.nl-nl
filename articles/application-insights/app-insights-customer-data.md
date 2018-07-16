---
title: Richtlijnen voor persoonlijke gegevens die zijn opgeslagen in Azure Application Insights | Microsoft Docs
description: In dit artikel wordt beschreven hoe u persoonlijke gegevens die zijn opgeslagen in Azure Application Insights en de methoden om te bepalen en verwijdert u deze beheren.
services: application-insights
documentationcenter: ''
author: mrbullwinkle
manager: carmonm
ms.assetid: ea2a28ed-4cd9-4006-bd5a-d4c76f4ec20b
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: conceptual
ms.date: 05/18/2018
ms.reviewer: Evgeny.Ternovsky
ms.author: mbullwin
ms.openlocfilehash: a59b57c546f18a7d91160f2ae7282af82fc42160
ms.sourcegitcommit: 7208bfe8878f83d5ec92e54e2f1222ffd41bf931
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/14/2018
ms.locfileid: "39044710"
---
# <a name="guidance-for-personal-data-stored-in-application-insights"></a>Richtlijnen voor persoonlijke gegevens die zijn opgeslagen in Application Insights

Application Insights is een gegevensarchief daar persoonlijke gegevens kunnen worden gevonden. In dit artikel wordt beschreven waar in Application Insights deze gegevens doorgaans wordt gevonden, evenals de mogelijkheden die voor u beschikbaar zijn voor het afhandelen van deze gegevens.

[!INCLUDE [GDPR-related guidance](../../includes/gdpr-dsr-and-stp-note.md)]

## <a name="strategy-for-personal-data-handling"></a>Strategie voor de verwerking van persoonlijke gegevens

Terwijl deze worden tot u en uw bedrijf uiteindelijk bepalen de strategie waarbij u uw persoonlijke afhandelt gegevens (indien helemaal), wordt hier volgen enkele mogelijke strategieën. Ze worden weergegeven in de volgorde van voorkeur vanuit een technisch oogpunt van de meeste te minste voorkeur:
* Waar mogelijk, verzameling van stoppen, onleesbaar maakt, anoniem maken of andere wijze aanpassen, de gegevens worden verzameld als u wilt uitsluiten van de gebeurtenis wordt beschouwd als _persoonlijke_. Deze methode, is de aanpak van voorkeur, zodat u niet hoeft te maken van een strategie voor de afhandeling kostbare en meer impact van gegevens.
* Indien niet mogelijk is, kunt u proberen te normaliseren van de gegevens voor de op het data platform en de prestaties worden verminderd. Bijvoorbeeld, in plaats van de logboekregistratie van een expliciete gebruikers-ID maakt u een zoekopdracht op gegevens die wordt correleren van de gebruikersnaam en de bijbehorende gegevens die vervolgens ergens anders kunnen worden geregistreerd in een interne id. Op die manier als een van uw gebruikers vragen om te verwijderen van hun persoonlijke gegevens, is het mogelijk dat alleen verwijderen van de rij in de opzoektabel overeenkomt met de gebruiker voldoende zal zijn. 
* Als persoonlijke gegevens moeten worden verzameld, bouw ten slotte een proces om het opschonen van API-pad en de bestaande query API pad om te voldoen aan de verplichtingen die u mogelijk om exporteren en verwijderen van persoonlijke gegevens die zijn gekoppeld aan een gebruiker.

## <a name="where-to-look-for-private-data-in-application-insights"></a>Waar om te zoeken naar persoonlijke gegevens in Application Insights?

Application Insights is een volledig flexibele opslag, die tijdens het voorschrijven van een schema dat u wilt uw gegevens, kunt u elk veld met uw eigen waarden. Daarom is het niet mogelijk om in te spreken precies waar persoonlijke gegevens opgenomen in uw specifieke toepassing. De volgende locaties zijn echter goed startpunt in uw inventaris:

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

Het is __sterk__ aanbevolen, wanneer mogelijk, te herstructureren van uw beleid voor gegevensverzameling voor het verzamelen van persoonlijke gegevens, obfuscating of anoniem, uitschakelen of anderszins aanpassen om te verwijderen uit de gebeurtenis wordt beschouwd als 'persoonlijke'. Verwerking van de gegevens nadat deze is verzameld, zal leiden tot kosten bij u en uw team te definiëren en het automatiseren van een strategie, het bouwen van een interface voor uw klanten om te communiceren met hun gegevens via en constant onderhoudskosten. Bovendien is het rekenintensief kostbare voor Application Insights en een groot aantal gelijktijdige query of opschonen van API-aanroepen hebben de potentiële negatieve invloed hebben op alle andere interactie met Application Insights-functionaliteit. Dat gezegd, er zijn inderdaad enkele geldige scenario's waar de persoonlijke gegevens moeten worden verzameld. Voor deze gevallen moet de gegevens worden verwerkt zoals beschreven in deze sectie.

[!INCLUDE [GDPR-related guidance](../../includes/gdpr-intro-sentence.md)]

### <a name="view-and-export"></a>Weergeven en exporteren

Voor beide weergeven en exporteren van aanvragen voor gegevens, de [Query-API](https://dev.applicationinsights.io/quickstart) moet worden gebruikt. Logica voor het converteren van de vorm van de gegevens naar een geschikt om te leveren aan uw gebruikers worden aan u om te implementeren. [Azure Functions](https://azure.microsoft.com/services/functions/) is een goed uitgangspunt voor het hosten van dergelijke logica.

### <a name="delete"></a>Verwijderen

> [!WARNING]
> Verwijderingen in Application Insights zijn schadelijke en niet-omkeerbare! Gebruik zeer voorzichtig te werk in de uitvoering ervan.

We hebben beschikbaar gesteld als onderdeel van een privacy-verwerking van een pad van de API 'wissen' artikel. Dit pad moet spaarzaam worden gebruikt vanwege het risico dat samenhangt met in dat geval, de mogelijke invloed op de prestaties en het potentieel scheeftrekken gehele aggregaties, metingen en andere aspecten van uw Application Insights-gegevens. Zie de [strategie voor de verwerking van persoonlijke gegevens](#strategy-for-personal-data-handling) sectie hierboven voor alternatieve methoden voor het verwerken van persoonlijke gegevens.

Opschonen van Logboeken is een zeer bevoegde bewerking dat er geen app of gebruiker in Azure (met inbegrip van zelfs de resource-eigenaar) hebben machtigingen om uit te voeren zonder expliciet worden verleend een rol in Azure Resource Manager. Deze rol is _gegevens Purger_ en moet zorgvuldig worden overgedragen omdat de mogelijkheden voor preventie van gegevensverlies.

Nadat de Azure Resource Manager-rol is toegewezen, wordt twee nieuwe API-paden zijn-documentatie voor ontwikkelaars beschikbaar, volledige en aanroepen van de vorm die zijn gekoppeld:

* [Opschonen na](https://docs.microsoft.com/rest/api/application-insights/components/purge) - wordt een object op te geven parameters van de gegevens te verwijderen en retourneert een verwijzing GUID
* OPHALEN leegmaken status - de POST-aanroep voor opschonen van een 'x-ms-status-location'-header die bevat een URL die u aanroepen kunt om te bepalen van de status van uw API opschonen wordt geretourneerd. Bijvoorbeeld:
   ```
   x-ms-status-location: https://management.azure.com/subscriptions/[SubscriptionId]/resourceGroups/[ResourceGroupName]/providers/microsoft.insights/components/[ComponentName]/operations/purge-[PurgeOperationId]?api-version=2015-05-01
   ```

> [!IMPORTANT]
>  Hoewel de meeste opschonen bewerkingen mogelijk veel sneller dan de SLA, vanwege de zware impact op het data-platform dat door Application Insights, voltooid **een formele SLA voor de voltooiing van leegmaken is ingesteld op 30 dagen**.

## <a name="next-steps"></a>Volgende stappen
Zie voor meer informatie over hoe data wordt verzameld, verwerkt en beveiligd, [Application Insights-gegevensbeveiliging](app-insights-data-retention-privacy.md).