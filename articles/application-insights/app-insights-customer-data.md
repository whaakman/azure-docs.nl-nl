---
title: Richtlijnen voor persoonlijke gegevens die zijn opgeslagen in Azure Application Insights | Microsoft Docs
description: In dit artikel wordt beschreven hoe voor het beheren van persoonlijke gegevens die zijn opgeslagen in Azure Application Insights en de methoden te identificeren en te verwijderen.
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
ms.openlocfilehash: 95e421278b46015e761764792e11dec0351b9785
ms.sourcegitcommit: 6f6d073930203ec977f5c283358a19a2f39872af
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/11/2018
ms.locfileid: "35294418"
---
# <a name="guidance-for-personal-data-stored-in-application-insights"></a>Richtlijnen voor persoonlijke gegevens die zijn opgeslagen in de Application Insights

Application Insights is een gegevensarchief waar persoonlijke gegevens zijn waarschijnlijk worden gevonden. Dit artikel wordt beschreven waar in Application Insights deze gegevens doorgaans wordt gevonden, evenals de mogelijkheden die voor u beschikbaar zijn voor het afhandelen van deze gegevens.

[!INCLUDE [GDPR-related guidance](../../includes/gdpr-dsr-and-stp-note.md)]

## <a name="strategy-for-personal-data-handling"></a>Strategie voor de verwerking van persoonlijke gegevens

Terwijl het is mogelijk maximaal u en uw bedrijf uiteindelijk bepalen de strategie waarbij u uw persoonlijke, verwerkt gegevens (als op alle), wordt hieronder volgen enkele mogelijke benaderingen. Ze worden weergegeven in de volgorde van voorkeur vanuit een technisch oogpunt van de meeste naar de minste voorkeur:
* Waar mogelijk, stop het verzamelen van, verbergen, weergeven, anoniem te maken of anders passen de gegevens worden verzameld als u wilt uitsluiten wordt beschouwd als _persoonlijke_. Deze methode is de gewenste aanpak, zodat u moet een strategie voor de afhandeling kostbaar en indrukwekkende van gegevens maken.
* Indien niet mogelijk is, proberen normaliseren van de gegevens zodat de gevolgen voor het gegevensplatform en de prestaties verminderen. Bijvoorbeeld, in plaats van een expliciete gebruikersnaam aan te melden, maken een zoekopdracht op gegevens die wordt correleren van de gebruikersnaam en de bijbehorende gegevens naar een interne ID die vervolgens elders kunnen worden geregistreerd. Op die manier als een van uw gebruikers gevraagd om te verwijderen van hun persoonlijke gegevens, is het mogelijk dat alleen verwijderen van de rij in de opzoektabel overeenkomt met de gebruiker voldoende. 
* Bouw ten slotte als persoonlijke gegevens moeten worden verzameld, een proces rond het opschonen van API-pad en de bestaande query API-pad om te voldoen aan eventuele verplichtingen die hebt u wellicht rond exporteren en het verwijderen van alle persoonlijke gegevens die zijn gekoppeld aan een gebruiker.

## <a name="where-to-look-for-private-data-in-application-insights"></a>Waar wilt zoeken naar persoonlijke gegevens in Application Insights?

Application Insights is een volledig flexibele archief tijdens voorschrijven van een schema dat u wilt uw gegevens, kunt u voor het onderdrukken van elk veld met uw eigen waarden. Hierdoor is het onmogelijk om in te spreken precies waar persoonlijke gegevens worden gevonden in uw specifieke toepassing. De volgende locaties zijn echter goed beginpunt in uw inventaris:

* *IP-adressen*: terwijl Application Insights wordt standaard verbergen, weergeven alle velden in de IP-adressen op '0.0.0.0', is het een redelijk algemene patroon voor het onderdrukken van deze waarde met het werkelijke gebruikers-IP-sessiegegevens beheren. De onderstaande Analytics query kan worden gebruikt om een tabel met waarden in de kolom IP-adres dan '0.0.0.0' in de afgelopen 24 uur te vinden:
    ```
    search client_IP != "0.0.0.0"
    | where timestamp > ago(1d)
    | summarize numNonObfuscatedIPs_24h = count() by $table
    ```
* *Gebruikers-id's*: standaard Application Insights willekeurig gegenereerde id wordt gebruikt voor de gebruiker en voor sessie bijhouden. Het is echter algemene voor een overzicht van deze velden voor het opslaan van een ID meer relevant zijn voor de toepassing genegeerd. Bijvoorbeeld: gebruikersnamen, AAD-GUID's, enzovoort. Deze id's zijn vaak wordt beschouwd als moet in bereik als persoonlijke gegevens, en daarom moet worden verwerkt op de juiste wijze. Onze aanbeveling is altijd proberen te verbergen, weergeven of anoniem deze id te maken. Dit zijn enkele velden waar deze waarden zich normaal gesproken session_Id, user_Id, user_AuthenticatedId, user_AccountId, evenals customDimensions.
* *Aangepaste gegevens*: Application Insights kunt u een verzameling aangepaste dimensies toevoegen aan een ander gegevenstype. Deze dimensies kunnen worden *eventuele* gegevens. Gebruik de volgende query voor het identificeren van eventuele aangepaste dimensies in de afgelopen 24 uur verzameld:
    ```
    search * 
    | where isnotempty(customDimensions)
    | where timestamp > ago(1d)
    | project $table, timestamp, name, customDimensions 
    ```
* *Gegevens in het geheugen en in transit*: Application Insights uitzonderingen, aanvragen, afhankelijkheidsaanroepen en traceringen worden bijgehouden. Persoonlijke gegevens kunt vaak op de code en HTTP-aanroep niveau worden verzameld. Controleer de uitzonderingen, aanvragen, afhankelijkheden en traceringen tabellen om te identificeren van dergelijke gegevens. Gebruik [telemetrie initalisatiefuncties](https://docs.microsoft.com/azure/application-insights/app-insights-api-filtering-sampling) waar mogelijk te verbergen, deze gegevens weergeven.
* *Momentopname maken van het foutopsporingsprogramma opnamen*: de [momentopname foutopsporingsprogramma](https://docs.microsoft.com/azure/application-insights/app-insights-snapshot-debugger) functie in Application Insights kunt u foutopsporing-momentopnamen verzamelen als een uitzondering is opgetreden op het exemplaar van de productie van uw toepassing. Momentopnamen wordt de volledige stack-trace leiden tot de uitzonderingen, evenals de waarden voor lokale variabelen bij elke stap in de stack weer. Deze functie is helaas niet toegestaan voor selectieve verwijderen van punten van de module of programmatische toegang tot gegevens in de momentopname. Als de standaardfrequentie van de momentopname-bewaartermijn voldoet niet aan de nalevingsvereisten, is de aanbeveling daarom de functie uitschakelen.

## <a name="how-to-export-and-delete-private-data"></a>Het exporteren en persoonlijke gegevens verwijderen

Het is __sterk__ aanbevolen wanneer mogelijk te herstructureren van uw beleid voor gegevensverzameling voor het verzamelen van persoonlijke gegevens, obfuscating of anonymizing, uitschakelen of anderszins wijzigen om het te verwijderen uit wordt beschouwd als 'Privé'. Verwerken van de gegevens nadat deze is verzameld, leidt ertoe dat bij u en uw team te definiëren en een strategie automatiseren, het bouwen van een interface voor uw klanten om te communiceren met hun gegevens via, en er onderhoudskosten. Verder rekenkundig kostbare voor Application Insights en een groot aantal gelijktijdige query is of opschonen API-aanroepen over de potentiële negatieve invloed hebben op alle andere interactie met Application Insights-functionaliteit. Die gezegd, er zijn immers geldig scenario's waarbij de persoonlijke gegevens moeten worden verzameld. Voor deze aanvragen worden gegevens zoals beschreven in deze sectie behandeld.

[!INCLUDE [GDPR-related guidance](../../includes/gdpr-intro-sentence.md)]

### <a name="view-and-export"></a>Weergeven en exporteren

Voor beide weergeven en exporteren van van gegevensaanvragen, de [Query API](https://dev.applicationinsights.io/quickstart) moet worden gebruikt. Logica in de vorm van de gegevens converteren naar een geschikte beheerpunt om te leveren aan uw gebruikers worden aan u om te implementeren. [Azure Functions](https://azure.microsoft.com/services/functions/) is een goed uitgangspunt voor het hosten van dergelijke logica.

### <a name="delete"></a>Verwijderen

> [!WARNING]
> Hiermee verwijdert u in de Application Insights zijn destructief en niet-omkeerbare! Gebruik daarom uiterst voorzichtig in de uitvoering ervan.

We hebben beschikbaar gesteld als onderdeel van een privacy-verwerking van een API 'verwijderen' pad artikel. Dit pad moet spaarzaam worden gebruikt vanwege het risico dat samenhangt met hiertoe de mogelijke invloed op prestaties en de potentie heeft om te leiden tot onjuiste aggregaties all-up, metingen en andere aspecten van uw Application Insights-gegevens. Zie de [strategie voor de verwerking van persoonlijke gegevens](#strategy-for-personal-data-handling) sectie hierboven voor alternatieve methoden voor het afhandelen van persoonlijke gegevens.

Leegmaken is een bijzondere rechten bewerking dat geen app of de gebruiker in Azure (inclusief zelfs de resource-eigenaar) beschikken over de machtigingen om uit te voeren zonder expliciet worden verleend een rol in Azure Resource Manager. Deze rol is _gegevens Purger_ en moet zorgvuldig worden overgedragen omdat de mogelijkheden voor verlies van gegevens.

Zodra de Azure Resource Manager-rol is toegewezen, worden de twee nieuwe API-paden beschikbaar zijn, volledige ontwikkeldocumentatie zijn en roept u shape gekoppeld:

* [POST opschonen](https://docs.microsoft.com/rest/api/application-insights/components/purge) - wordt een object opgeven van parameters van de gegevens te verwijderen en retourneert een verwijzing GUID
* GET status opschonen - retourneert de aanroep van POST leegmaken als u een 'x-ms-status-location'-header met een URL die u aanroepen kunt om te bepalen van de status van uw API opschonen. Bijvoorbeeld:
   ```
   x-ms-status-location: https://management.azure.com/subscriptions/[SubscriptionId]/resourceGroups/[ResourceGroupName]/providers/microsoft.insights/components/[ComponentName]/operations/purge-[PurgeOperationId]?api-version=2015-05-01
   ```

Hoewel de meeste opschonen bewerkingen mogelijk veel sneller dan de SLA, vanwege een zware gevolgen van het gegevensplatform dat die worden gebruikt door de Application Insights voltooid een formele SLA voor de voltooiing van opschonen operations is ingesteld op 30 dagen.

## <a name="next-steps"></a>Volgende stappen
Zie voor meer informatie over hoe gegevens wordt verzameld, verwerkt en beveiligd, [Application Insights-gegevensbeveiliging](app-insights-data-retention-privacy.md).