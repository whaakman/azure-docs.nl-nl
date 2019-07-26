---
title: Overzicht van Azure-verbruiks-API | Microsoft Docs
description: Meer informatie over hoe Azure-verbruiks-Api's u programmatisch toegang bieden tot kosten-en gebruiks gegevens voor uw Azure-resources.
services: billing
documentationcenter: ''
author: bandersmsft
manager: dougeby
editor: ''
tags: billing
ms.assetid: 68825e85-de64-466d-b11a-8baffde836b5
ms.service: billing
ms.devlang: na
ms.topic: reference
ms.tgt_pltfrm: na
ms.workload: billing
ms.date: 6/07/2018
ms.author: banders
ms.openlocfilehash: 2a2922daef5e75af41252119f9a467fae976bcda
ms.sourcegitcommit: a874064e903f845d755abffdb5eac4868b390de7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/24/2019
ms.locfileid: "68443262"
---
# <a name="azure-consumption-api-overview"></a>Overzicht van Azure-verbruiks-API 

De API's voor Azure-gebruiksgegevens bieden u programmatische toegang tot de kosten en gebruiksgegevens van uw Azure-resources. Deze Api's ondersteunen momenteel alleen Enter prise-inschrijvingen en Web direct-abonnementen (met enkele uitzonde ringen). De Api's worden voortdurend bijgewerkt ter ondersteuning van andere typen Azure-abonnementen.

API's voor Azure-gebruiksgegevens bieden toegang tot:
- Enterprise- en Web Direct-klanten 
    - Gebruiksgegevens 
    - Marketplace-kosten 
    - Aanbevelingen voor reserveringen 
    - Reserveringsdetails 
    - Reserveringssamenvattingen 
- Alleen voor Enterprise-klanten 
    - Prijzenoverzicht 
    - Budgetten 
    - Tegoeden 

## <a name="usage-details-api"></a>Gebruiks Details-API

Gebruik de API gebruiks gegevens om kosten en gebruiks gegevens voor alle resources van de eerste partij van Azure te verkrijgen. De informatie is in de vorm van gebruiks gegevens records die momenteel eenmaal per meter per resource per dag worden verzonden. Informatie kan worden gebruikt om de kosten voor alle resources toe te voegen of om de kosten/het gebruik van een specifieke resource (s) te onderzoeken. 

De API omvat:

-   **Verbruiks gegevens op meter niveau** : Zie gegevens, inclusief gebruiks kosten, de meter die de kosten afgeven en de Azure-resource waarop de kosten van toepassing zijn. Alle detail records voor gebruik worden toegewezen aan een dagelijkse Bucket.
-   Op **rollen gebaseerd Azure Access Control** : Configureer toegangs beleid op de [Azure Portal](https://portal.azure.com), de [Azure cli](https://docs.microsoft.com/azure/role-based-access-control/role-assignments-cli) -of [Azure PowerShell-cmdlets](https://docs.microsoft.com/powershell/azure/overview) om op te geven welke gebruikers of toepassingen toegang tot de gebruiks gegevens van het abonnement kunnen krijgen. Aanroepers moeten standaard Azure Active Directory-tokens gebruiken voor verificatie. De oproepende functie toevoegen aan de factureren voor lezer, lezer, eigenaar, Inzender rol of toegang krijgen tot gegevens over gebruik voor een specifieke Azure-abonnement. 
-   **Filters** : Hiermee knipt u het resultaat van de API naar een kleinere set detail records met de volgende filters:
    - Begin/gebruiks datum van gebruik
    - Resourcegroep
    - Resourcenaam
-   **Gegevens aggregatie** : OData gebruiken om expressies toe te passen op statistische gebruiks gegevens per label of filter eigenschappen
-   **Gebruik voor verschillende aanbiedings typen** : de informatie over gebruiks gegevens is momenteel beschikbaar voor zakelijke en Web direct-klanten.

Zie de technische specificatie voor de [gebruiks gegevens-API](https://docs.microsoft.com/rest/api/consumption/usagedetails)voor meer informatie.

## <a name="marketplace-charges-api"></a>API voor Marketplace-kosten

Gebruik de API voor Marketplace-kosten om kosten-en gebruiks gegevens voor alle Marketplace-resources te ontvangen (aanbiedingen van derden van Azure). Deze gegevens kunnen worden gebruikt voor het toevoegen van kosten voor alle resources in Marketplace of het onderzoeken van kosten/gebruik op specifieke resources. 

De API omvat:

-   **Verbruiks gegevens op meter niveau** : Zie gegevens, inclusief de kosten voor Marketplace-gebruik, de meter die de kosten uitstraalt en de resource waarop de kosten van toepassing zijn. Alle detail records voor gebruik worden toegewezen aan een dagelijkse Bucket.
-   Op **rollen gebaseerd Azure Access Control** : Configureer toegangs beleid op de [Azure Portal](https://portal.azure.com), de [Azure cli](https://docs.microsoft.com/azure/role-based-access-control/role-assignments-cli) -of [Azure PowerShell-cmdlets](https://docs.microsoft.com/powershell/azure/overview) om op te geven welke gebruikers of toepassingen toegang tot de gebruiks gegevens van het abonnement kunnen krijgen. Aanroepers moeten standaard Azure Active Directory-tokens gebruiken voor verificatie. De oproepende functie toevoegen aan de factureren voor lezer, lezer, eigenaar, Inzender rol of toegang krijgen tot gegevens over gebruik voor een specifieke Azure-abonnement. 
-   **Filters** : Knip het API-resultaat in op een kleinere set marktplaats records met de volgende filters:
    - Begin/gebruiks eindpunt van gebruik
    - Resourcegroep
    - Resourcenaam
-   **Gebruik voor verschillende aanbiedings typen** : marketplace-informatie is momenteel beschikbaar voor zakelijke en Web direct-klanten.

Zie de technische specificatie van de API voor Marketplace- [kosten](https://docs.microsoft.com/rest/api/consumption/marketplaces)voor meer informatie.

## <a name="balances-api"></a>Balancs-API

Zakelijke klanten kunnen de balances API gebruiken om een maandelijks overzicht te krijgen van de informatie over saldi, nieuwe aankopen, Azure Marketplace-service kosten, aanpassingen en overschrijding kosten. U kunt deze informatie ophalen voor de huidige facturerings periode of een periode in het verleden. Ondernemingen kunnen deze gegevens gebruiken om een vergelijking met hand matig berekende samenvattings kosten uit te voeren. Deze API biedt geen specifieke informatie over resources en een geaggregeerde weer gave van kosten. 

De API omvat:

-   Op **rollen gebaseerd Azure Access Control** : Configureer toegangs beleid op de [Azure Portal](https://portal.azure.com), de [Azure cli](https://docs.microsoft.com/azure/role-based-access-control/role-assignments-cli) -of [Azure PowerShell-cmdlets](https://docs.microsoft.com/powershell/azure/overview) om op te geven welke gebruikers of toepassingen toegang tot de gebruiks gegevens van het abonnement kunnen krijgen. Aanroepers moeten standaard Azure Active Directory-tokens gebruiken voor verificatie. De oproepende functie toevoegen aan de factureren voor lezer, lezer, eigenaar, Inzender rol of toegang krijgen tot gegevens over gebruik voor een specifieke Azure-abonnement. 
-   **Alleen Enter prise-klanten** Deze API is alleen beschikbaar voor EA-klanten. 
    - Klanten moeten beschikken over beheerders machtigingen voor ondernemingen om deze API aan te roepen 

Zie de technische specificatie voor de [Balancing API](https://docs.microsoft.com/rest/api/consumption/balances)voor meer informatie.

## <a name="budgets-api"></a>API voor budgetten

Enter prise-klanten kunnen deze API gebruiken om kosten-of gebruiks budgetten te maken voor resources, resource groepen of facturerings meters. Zodra deze informatie is vastgesteld, kunnen waarschuwingen worden geconfigureerd om te worden gewaarschuwd wanneer door de gebruiker gedefinieerde budget drempels worden overschreden. 

De API omvat:

-   Op **rollen gebaseerd Azure Access Control** : Configureer toegangs beleid op de [Azure Portal](https://portal.azure.com), de [Azure cli](https://docs.microsoft.com/azure/role-based-access-control/role-assignments-cli) -of [Azure PowerShell-cmdlets](https://docs.microsoft.com/powershell/azure/overview) om op te geven welke gebruikers of toepassingen toegang tot de gebruiks gegevens van het abonnement kunnen krijgen. Aanroepers moeten standaard Azure Active Directory-tokens gebruiken voor verificatie. De oproepende functie toevoegen aan de factureren voor lezer, lezer, eigenaar, Inzender rol of toegang krijgen tot gegevens over gebruik voor een specifieke Azure-abonnement. 
-   **Alleen Enter prise-klanten** : deze API is alleen beschikbaar voor EA-klanten.
-   **Configureer bare meldingen** : Geef de gebruiker (s) op die moeten worden gewaarschuwd wanneer het budget wordt omgestuurd.
-   **Gebruik of op kosten gebaseerde budgetten** : Maak uw budget op basis van het verbruik of de kosten die nodig zijn voor uw scenario.
-   **Filteren** : uw budget filteren op een kleinere subset resources met behulp van de volgende Configureer bare filters
    - Resourcegroep
    - Resourcenaam
    - Naar gebruik
-   **Configureer bare budget tijds perioden** : Geef op hoe vaak het budget opnieuw moet worden ingesteld en hoe lang het budget geldig is.

Zie de technische specificatie van de [API budgets](https://docs.microsoft.com/rest/api/consumption/budgets)voor meer informatie.

## <a name="reservation-recommendations-api"></a>API voor reserverings aanbevelingen

Gebruik deze API om aanbevelingen op te halen voor het kopen van gereserveerde VM-instanties. Aanbevelingen zijn bedoeld om klanten de verwachte kosten besparingen en aankoop bedragen te analyseren. 

De API omvat:

-   Op **rollen gebaseerd Azure Access Control** : Configureer toegangs beleid op de [Azure Portal](https://portal.azure.com), de [Azure cli](https://docs.microsoft.com/azure/role-based-access-control/role-assignments-cli) -of [Azure PowerShell-cmdlets](https://docs.microsoft.com/powershell/azure/overview) om op te geven welke gebruikers of toepassingen toegang tot de gebruiks gegevens van het abonnement kunnen krijgen. Aanroepers moeten standaard Azure Active Directory-tokens gebruiken voor verificatie. De oproepende functie toevoegen aan de factureren voor lezer, lezer, eigenaar, Inzender rol of toegang krijgen tot gegevens over gebruik voor een specifieke Azure-abonnement. 
-   **Filteren** : uw aanbevelings resultaten op basis van de volgende filters aanpassen.
    - Scope
    - Lookback periode
-   **Reserverings gegevens voor verschillende aanbiedings typen** : er is momenteel reserverings informatie beschikbaar voor Enter prise-en Web direct-klanten.

Zie de technische specificatie van de API reserverings [aanbevelingen](https://docs.microsoft.com/rest/api/consumption/reservationrecommendations)voor meer informatie.

## <a name="reservation-details-api"></a>Reserverings details-API

Gebruik de reserverings details API om informatie over eerder aangeschafte VM-reserve ringen te bekijken, zoals hoeveel verbruik is gereserveerd en hoeveel er daad werkelijk wordt gebruikt. U kunt gegevens per VM-niveau weer geven. 

De API omvat:

-   Op **rollen gebaseerd Azure Access Control** : Configureer toegangs beleid op de [Azure Portal](https://portal.azure.com), de [Azure cli](https://docs.microsoft.com/azure/role-based-access-control/role-assignments-cli) -of [Azure PowerShell-cmdlets](https://docs.microsoft.com/powershell/azure/overview) om op te geven welke gebruikers of toepassingen toegang tot de gebruiks gegevens van het abonnement kunnen krijgen. Aanroepers moeten standaard Azure Active Directory-tokens gebruiken voor verificatie. De oproepende functie toevoegen aan de factureren voor lezer, lezer, eigenaar, Inzender rol of toegang krijgen tot gegevens over gebruik voor een specifieke Azure-abonnement. 
-   **Filters** : Knip het API-resultaat in met een kleinere set reserve ringen met behulp van het volgende filter:
    - Datumbereik
-   **Reserverings gegevens voor verschillende aanbiedings typen** : er is momenteel reserverings informatie beschikbaar voor Enter prise-en Web direct-klanten.

Zie de technische specificatie voor de API reserverings [Details](https://docs.microsoft.com/rest/api/consumption/reservationsdetails)voor meer informatie.

## <a name="reservation-summaries-api"></a>API-reserverings overzichten

Gebruik deze API om geaggregeerde informatie over eerder aangeschafte VM-reserve ringen te bekijken, zoals hoeveel verbruik is gereserveerd en hoeveel er daad werkelijk wordt gebruikt in de samen stelling. 

De API omvat:

-   Op **rollen gebaseerd Azure Access Control** : Configureer toegangs beleid op de [Azure Portal](https://portal.azure.com), de [Azure cli](https://docs.microsoft.com/azure/role-based-access-control/role-assignments-cli) -of [Azure PowerShell-cmdlets](https://docs.microsoft.com/powershell/azure/overview) om op te geven welke gebruikers of toepassingen toegang tot de gebruiks gegevens van het abonnement kunnen krijgen. Aanroepers moeten standaard Azure Active Directory-tokens gebruiken voor verificatie. De oproepende functie toevoegen aan de factureren voor lezer, lezer, eigenaar, Inzender rol of toegang krijgen tot gegevens over gebruik voor een specifieke Azure-abonnement. 
-   **Filteren** : uw resultaten op basis van de dagelijkse korrel met het volgende filter aanpassen:
    - Gebruiksdatum
-   **Reserverings gegevens voor verschillende aanbiedings typen** : er is momenteel reserverings informatie beschikbaar voor Enter prise-en Web direct-klanten.
-   **Dagelijkse of maandelijkse aggregaties** : bellers kunnen opgeven of ze de reserverings samenvattings gegevens in het dagelijks of maandelijks graan moeten hebben.

Zie de technische specificatie voor de API reserverings [overzichten](https://docs.microsoft.com/rest/api/consumption/reservationssummaries)voor meer informatie.

## <a name="price-sheet-api"></a>Prijs lijst-API
De Enter prise-klant kan deze API gebruiken om de aangepaste prijzen voor alle meters op te halen. Ondernemingen kunnen dit gebruiken in combi natie met gebruiks gegevens en informatie over het gebruik van marketplaces om kosten berekeningen uit te voeren met gebruik-en Marketplace-gegevens. 

De API omvat:

-   Op **rollen gebaseerd Azure Access Control** : Configureer toegangs beleid op de [Azure Portal](https://portal.azure.com), de [Azure cli](https://docs.microsoft.com/azure/role-based-access-control/role-assignments-cli) -of [Azure PowerShell-cmdlets](https://docs.microsoft.com/powershell/azure/overview) om op te geven welke gebruikers of toepassingen toegang tot de gebruiks gegevens van het abonnement kunnen krijgen. Aanroepers moeten standaard Azure Active Directory-tokens gebruiken voor verificatie. De oproepende functie toevoegen aan de factureren voor lezer, lezer, eigenaar, Inzender rol of toegang krijgen tot gegevens over gebruik voor een specifieke Azure-abonnement. 
-   **Alleen Enter prise-klanten** : deze API is alleen beschikbaar voor EA-klanten. Web direct-klanten moeten de RateCard-API gebruiken om prijzen te verkrijgen. 

Zie de technische specificatie voor de [prijs lijst-API](https://docs.microsoft.com/rest/api/consumption/pricesheet)voor meer informatie.

## <a name="scenarios"></a>Scenario's

Hier volgen enkele van de scenario's die mogelijk zijn gemaakt via de Api's voor verbruik:

-   **Factuur afstemming** : heeft micro soft het juiste bedrag in rekening gebracht?  Wat is mijn factuur en kan ik deze zelf berekenen?
-   **Meerdere kosten** : nu weet ik hoeveel er kosten in rekening worden gebracht, wie in mijn organisatie moet betalen?
-   **Kosten optimalisatie** -ik weet hoeveel er in rekening wordt gebracht... Hoe kan ik meer profiteren van het geld dat ik in azure heb?
-   **Kosten bijhouden** : Ik wil weten hoeveel ik besteed aan het gebruik van Azure in de loop van de tijd. Wat zijn de trends? Hoe kan ik beter?
-   **Azure-uitgaven gedurende de maand** : Hoeveel is de huidige maand van de dag? Moet ik aanpassingen aanbrengen in mijn uitgaven en/of gebruik van Azure? Wanneer kan ik in de maand de meeste van Azure gebruiken?
-   **Waarschuwingen instellen** : Ik wil op resource gebaseerd verbruik of op basis van een budget gebaseerde waarschuwingen instellen.

## <a name="next-steps"></a>Volgende stappen

- Zie [overzicht van Azure billing API](billing-usage-rate-card-overview.md)voor informatie over het gebruik van Azure billing-API's om programmatisch inzicht te krijgen in uw Azure-gebruik.



