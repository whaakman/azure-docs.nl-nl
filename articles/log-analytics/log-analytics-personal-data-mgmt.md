---
title: Richtlijnen voor persoonlijke gegevens die zijn opgeslagen in Azure Log Analytics | Microsoft Docs
description: In dit artikel wordt beschreven hoe voor het beheren van persoonlijke gegevens die zijn opgeslagen in Azure Log Analytics en de methoden te identificeren en te verwijderen.
services: log-analytics
documentationcenter: ''
author: mgoedtel
manager: carmonm
editor: ''
ms.assetid: ''
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 05/18/2018
ms.author: magoedte
ms.component: na
ms.openlocfilehash: 3692c83a4991fc67ec176687bd076ab14e4c640d
ms.sourcegitcommit: 5892c4e1fe65282929230abadf617c0be8953fd9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/29/2018
ms.locfileid: "37129367"
---
# <a name="guidance-for-personal-data-stored-in-log-analytics"></a>Richtlijnen voor persoonlijke gegevens die zijn opgeslagen in Log Analytics

Log Analytics is een gegevensarchief waar persoonlijke gegevens zijn waarschijnlijk worden gevonden. In dit artikel wordt beschreven waar in logboekanalyse dergelijke gegevens doorgaans wordt gevonden, evenals de mogelijkheden die voor u beschikbaar zijn voor het afhandelen van dergelijke gegevens.

[!INCLUDE [gdpr-dsr-and-stp-note](../../includes/gdpr-dsr-and-stp-note.md)]

## <a name="strategy-for-personal-data-handling"></a>Strategie voor de verwerking van persoonlijke gegevens

Terwijl het is mogelijk maximaal u en uw bedrijf uiteindelijk bepalen de strategie waarbij u uw persoonlijke, verwerkt gegevens (als op alle), wordt hieronder volgen enkele mogelijke benaderingen. Ze worden weergegeven in de volgorde van voorkeur vanuit een technisch oogpunt van de meeste naar de minste voorkeur:

* Waar mogelijk, stop het verzamelen van, verbergen, weergeven, anoniem te maken of anders passen de gegevens worden verzameld uit wordt beschouwd als 'persoonlijke' te sluiten. Dit is _veel_ de gewenste aanpak, zodat u moet een strategie voor de afhandeling erg kostbaar en indrukwekkende van gegevens maken.
* Indien niet mogelijk is, proberen normaliseren van de gegevens zodat de gevolgen voor het gegevensplatform en de prestaties verminderen. Bijvoorbeeld, in plaats van een expliciete gebruikersnaam aan te melden, maken een lookup-gegevens die wordt correleren van de gebruikersnaam en de bijbehorende gegevens naar een interne ID die vervolgens elders kunnen worden geregistreerd. Op die manier moet een van uw gebruikers vragen om te verwijderen van hun persoonlijke gegevens, is het mogelijk dat alleen verwijderen van de rij in de opzoektabel overeenkomt met de gebruiker voldoende zal zijn. 
* Bouw ten slotte als persoonlijke gegevens moeten worden verzameld, een proces rond het opschonen van API-pad en de bestaande query API-pad om te voldoen aan eventuele verplichtingen die hebt u wellicht rond exporteren en het verwijderen van alle persoonlijke gegevens die zijn gekoppeld aan een gebruiker. 

## <a name="where-to-look-for-private-data-in-log-analytics"></a>Waar wilt zoeken naar persoonlijke gegevens in Log Analytics?

Log Analytics is een flexibele archief tijdens voorschrijven van een schema dat u wilt uw gegevens, kunt u voor het onderdrukken van elk veld met uw eigen waarden. Bovendien mag aangepast schema worden ingenomen. Hierdoor is het onmogelijk om in te spreken precies waar persoonlijke gegevens worden gevonden in uw specifieke werkruimte. De volgende locaties zijn echter goed beginpunt in uw inventaris:

* *IP-adressen*: Log Analytics tal van IP-informatie verzamelt over veel verschillende tabellen. De volgende query ziet u bijvoorbeeld alle tabellen waarbij IPv4-adressen in de afgelopen 24 uur zijn verzameld:
    ```
    search * 
    | where * matches regex @'\b((25[0-5]|2[0-4][0-9]|[01]?[0-9][0-9]?)(\.|$)){4}\b' //RegEx originally provided on https://stackoverflow.com/questions/5284147/validating-ipv4-addresses-with-regexp
    | summarize count() by $table
    ```
* *Gebruikers-id's*: gebruikers-id's zijn gevonden in een grote verscheidenheid aan oplossingen en tabellen. U kunt raadplegen voor een bepaalde gebruikersnaam in de volledige gegevensset met de zoekopdracht:
    ```
    search "[username goes here]"
    ```
Vergeet niet om te zoeken, niet alleen voor leesbare gebruikersnamen, maar ook GUID's die rechtstreeks aan een bepaalde gebruiker terug kunnen worden herleid.
* *Apparaat-id's*: zoals gebruikersnamen, apparaat-id's worden soms ' als privé beschouwd '. Gebruik dezelfde manier zoals hierboven vermeld voor de gebruikers-id's voor het identificeren van tabellen wanneer dit wordt mogelijk een probleem. 
* *Aangepaste gegevens*: Log Analytics kunt u de verzameling in een aantal methoden: aangepaste logboeken en aangepaste velden, de [HTTP Data Collector API](log-analytics-data-collector-api.md) , en aangepaste gegevens verzameld als onderdeel van Logboeken. Al deze waarden zijn vatbaar voor die persoonlijke gegevens en om te controleren of er geen dergelijke gegevens bestaat moeten worden onderzocht.
* *Oplossing opgenomen gegevens*: omdat het mechanisme voor de oplossing een open is, wordt u aangeraden alle tabellen die zijn gegenereerd door oplossingen om de naleving controleren.

## <a name="how-to-export-and-delete-private-data"></a>Het exporteren en persoonlijke gegevens verwijderen

Zoals vermeld in de [strategie voor de verwerking van persoonlijke gegevens](#strategy-for-personal-data-handling) eerder sectie, is het __sterk__ aanbevolen als het mogelijk om uit te schakelen van de verzameling van uw beleid voor gegevensverzameling herstructureren persoonlijke gegevens, obfuscating of het anonymizing of anderszins wijzigen om te verwijderen uit wordt beschouwd als 'persoonlijke'. Verwerking van de gegevens wordt voorste resultaat kosten voor u en uw team te definiëren en een strategie automatiseren, het bouwen van een interface voor uw klanten om te communiceren met hun gegevens via en lopende onderhoudskosten. Verder rekenkundig kostbare voor logboekanalyse en een groot aantal gelijktijdige query is of opschonen API-aanroepen over de potentiële negatieve invloed hebben op alle andere interactie met Log Analytics-functionaliteit. Die gezegd, er zijn immers geldig scenario's waarbij de persoonlijke gegevens moeten worden verzameld. Voor deze aanvragen worden gegevens zoals beschreven in deze sectie behandeld.

[!INCLUDE [gdpr-intro-sentence](../../includes/gdpr-intro-sentence.md)]

### <a name="view-and-export"></a>Weergeven en exporteren

Voor beide weergeven en exporteren van van gegevensaanvragen, de [Query API](https://dev.loganalytics.io/) moet worden gebruikt. Logica in de vorm van de gegevens converteren naar een geschikte beheerpunt om te leveren aan uw gebruikers worden aan u om te implementeren. [Azure Functions](https://azure.microsoft.com/services/functions/) maakt een goede plaats voor het hosten van dergelijke logica.

### <a name="delete"></a>Verwijderen

> [!WARNING]
> Hiermee verwijdert u in logboekanalyse zijn destructief en niet-omkeerbare! Gebruik daarom uiterst voorzichtig in de uitvoering ervan.

We beschikbaar hebt gemaakt als onderdeel van de verwerking van een privacy een *opschonen* API-pad. Dit pad moet spaarzaam worden gebruikt vanwege het risico dat samenhangt met hiertoe de mogelijke invloed op prestaties en de potentie heeft om te leiden tot onjuiste aggregaties all-up, metingen en andere aspecten van uw gegevens logboekanalyse. Zie de [strategie voor de verwerking van persoonlijke gegevens](#strategy-for-personal-data-handling) sectie voor alternatieve methoden voor het afhandelen van persoonlijke gegevens.

Leegmaken is een bijzondere rechten bewerking dat geen app of de gebruiker in Azure (inclusief zelfs de resource-eigenaar) beschikken over de machtigingen om uit te voeren zonder expliciet worden verleend een rol in Azure Resource Manager. Deze rol is _gegevens Purger_ en wees voorzichtig bij moet worden overgedragen omdat de kans dat gegevens verloren gaan. 

Zodra de Azure Resource Manager-rol is toegewezen, zijn twee nieuwe API-paden zijn beschikbaar: 

* [Opschonen boeken] (https://docs.microsoft.com/rest/api/loganalytics/workspaces%202015-03-20/purge) - wordt een object opgeven van parameters van de gegevens te verwijderen en retourneert een verwijzing GUID 
* GET status opschonen - retourneert de aanroep van POST leegmaken als u een 'x-ms-status-location'-header met een URL die u aanroepen kunt om te bepalen van de status van uw API opschonen. Bijvoorbeeld:

    ```
    x-ms-status-location: https://management.azure.com/subscriptions/[SubscriptionId]/resourceGroups/[ResourceGroupName]/providers/Microsoft.OperatonalInsights/workspaces/[WorkspaceName]/operations/purge-[PurgeOperationId]?api-version=2015-03-20
    ```

Terwijl we verwachten dat de meeste opschonen bewerkingen om uit te voeren veel sneller dan onze SERVICEOVEREENKOMST, vanwege een zware gevolgen van het gegevensplatform dat die worden gebruikt door Log Analytics een formele SLA voor de voltooiing van bewerkingen voor opschonen is ingesteld op 30 dagen. 

## <a name="next-steps"></a>Volgende stappen
Zie voor meer informatie over hoe gegevens wordt verzameld, verwerkt en beveiligd, [Log Analytics-gegevensbeveiliging](log-analytics-data-security.md).