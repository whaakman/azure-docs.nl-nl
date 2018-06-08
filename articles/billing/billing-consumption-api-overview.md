---
title: Overzicht van Azure-verbruik API | Microsoft Docs
description: Informatie over hoe Azure-verbruik-API's u programmatische toegang geven aan de kosten en gebruiksgegevens voor uw Azure-resources.
services: billing
documentationcenter: ''
author: Erikre
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
ms.author: erikre
ms.openlocfilehash: becc6d6e86fd292cfbc919f1eee36a4f5edb711c
ms.sourcegitcommit: 3c3488fb16a3c3287c3e1cd11435174711e92126
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/07/2018
ms.locfileid: "34851276"
---
# <a name="azure-consumption-api-overview"></a>Azure-verbruik API-overzicht 

De Azure-verbruik API's bieden u programmatische toegang tot de kosten en gebruiksgegevens voor uw Azure-resources. Deze API's ondersteuning momenteel alleen voor Enterprise inschrijvingen en Web Direct abonnementen (met een paar uitzonderingen). De API's worden voortdurend bijgewerkt ter ondersteuning van andere soorten Azure-abonnementen.

Azure-verbruik-API's bieden toegang tot:
- Enterprise- en Web Direct klanten 
    - Gebruiksdetails 
    - Marketplace-kosten 
    - Reservering aanbevelingen 
    - Details van de reservering 
    - Samenvattingen van de reservering 
- Enterprise-klanten 
    - Prijslijst 
    - Budgetten 
    - Saldo 's 

## <a name="usage-details-api"></a>Details van gebruik API

De informatie over het gebruik Details-API gebruiken om op te halen van kosten en gebruiksgegevens voor alle Azure 1e resources van derden. Informatie is in de vorm van detail gebruiksrecords die momenteel worden verzonden, één keer per meter per bron per dag. Informatie kan worden gebruikt voor het toevoegen van de kosten voor alle resources of kosten onderzoeken / gebruik op de specifieke resources. 

De API bevat:

-   **Niveau verbruiksgegevens meten** - Zie gegevens, met inbegrip van gebruik van de meter die de kosten, kosten en welke Azure-resource kosten heeft betrekking op. Details van alle gebruiksrecords worden toegewezen aan een dagelijkse bucket.
-   **Azure op rollen gebaseerd toegangsbeheer** -configureren toegangsbeleid op de [Azure-portal](https://portal.azure.com), wordt de [Azure CLI](https://docs.microsoft.com/azure/role-based-access-control/role-assignments-cli) of [Azure PowerShell-cmdlets](https://docs.microsoft.com/powershell/azure/overview) aangeven welke gebruikers of toepassingen kunnen toegang krijgen tot gegevens over het gebruik van het abonnement. Aanroepfuncties moeten standaard tokens van Azure Active Directory voor verificatie gebruiken. De aanroeper toevoegen aan de lezer facturering, een lezer, een eigenaar of een bijdrager rol voor toegang tot de gebruiksgegevens voor een specifieke Azure-abonnement. 
-   **Filteren** -Trim uw API-resultaat niet actief is ingesteld op een kleiner aantal gebruiksrecords van detail met behulp van de volgende filters:
    - Gebruik end / gebruik starten
    - Resourcegroep
    - Resourcenaam
-   **Gegevensaggregatie** -gebruik OData expressies toepassen op statistische informatie over het gebruik door tags of filteren van eigenschappen
-   **Gebruik voor andere aanbiedingstypen** -details van gebruiksgegevens is momenteel beschikbaar voor klanten met Enterprise- en Web Direct.

Zie voor meer informatie, de technische specificaties voor de [gebruik Details API](https://docs.microsoft.com/rest/api/consumption/usagedetails).

## <a name="marketplace-charges-api"></a>Marketplace-kosten API

Gebruik de Marketplace-kosten-API om kosten en gebruiksgegevens op alle Marketplace-bronnen (Azure 3e partij aanbiedingen). Deze gegevens kunnen worden gebruikt voor kosten optellen over alle Marketplace-resources of kosten onderzoeken / gebruik op de specifieke resources. 

De API bevat:

-   **Niveau verbruiksgegevens meten** - Zie gegevens, met inbegrip van marketplace gebruik kosten, de meter die de kosten en welke bronnen de kosten heeft betrekking op. Details van alle gebruiksrecords worden toegewezen aan een dagelijkse bucket.
-   **Azure op rollen gebaseerd toegangsbeheer** -configureren toegangsbeleid op de [Azure-portal](https://portal.azure.com), wordt de [Azure CLI](https://docs.microsoft.com/azure/role-based-access-control/role-assignments-cli) of [Azure PowerShell-cmdlets](https://docs.microsoft.com/powershell/azure/overview) aangeven welke gebruikers of toepassingen kunnen toegang krijgen tot gegevens over het gebruik van het abonnement. Aanroepfuncties moeten standaard tokens van Azure Active Directory voor verificatie gebruiken. De aanroeper toevoegen aan de lezer facturering, een lezer, een eigenaar of een bijdrager rol voor toegang tot de gebruiksgegevens voor een specifieke Azure-abonnement. 
-   **Filteren** -Trim uw API-resultaat niet actief is ingesteld op een kleiner aantal marketplace registreert met behulp van de volgende filters:
    - Gebruik begin / gebruik beëindigen
    - Resourcegroep
    - Resourcenaam
-   **Gebruik voor andere aanbiedingstypen** -Marketplace-informatie is momenteel beschikbaar voor klanten met Enterprise- en Web Direct.

Zie voor meer informatie, de technische specificaties voor de [Marketplace kosten API](https://docs.microsoft.com/rest/api/consumption/marketplaces).

## <a name="balances-api"></a>Tegoeden API

Enterprise-klanten kunnen de API compromis tussen de gebruiken om op te halen per maand samenvatting van informatie over tegoeden, nieuwe aankopen Azure Marketplace kosten, aanpassingen en overschrijding kosten. U kunt deze informatie voor de huidige factureringsperiode of een punt in het verleden. Ondernemingen kunnen deze gegevens gebruiken om uit te voeren van een vergelijking met handmatig berekende samenvatting kosten. Deze API biedt geen resource-specifieke informatie en een samengevoegde weergave van de kosten. 

De API bevat:

-   **Azure op rollen gebaseerd toegangsbeheer** -configureren toegangsbeleid op de [Azure-portal](https://portal.azure.com), wordt de [Azure CLI](https://docs.microsoft.com/azure/role-based-access-control/role-assignments-cli) of [Azure PowerShell-cmdlets](https://docs.microsoft.com/powershell/azure/overview) aangeven welke gebruikers of toepassingen kunnen toegang krijgen tot gegevens over het gebruik van het abonnement. Aanroepfuncties moeten standaard tokens van Azure Active Directory voor verificatie gebruiken. De aanroeper toevoegen aan de lezer facturering, een lezer, een eigenaar of een bijdrager rol voor toegang tot de gebruiksgegevens voor een specifieke Azure-abonnement. 
-   **Alleen Enterprise-klanten** voor deze API is alleen beschikbaar EA klanten. 
    - Klanten moeten ondernemingsadministrator-machtigingen hebben voor deze API niet aanroepen 

Zie voor meer informatie, de technische specificaties voor de [tegoeden API](https://docs.microsoft.com/rest/api/consumption/getbalancesbybillingaccount).

## <a name="budgets-api"></a>Budgetten API

Enterprise-klanten kunnen deze API gebruiken om kosten of gebruik budgetten voor resources, resourcegroepen of facturering meters maken. Zodra deze informatie is bepaald, kan waarschuwingen worden geconfigureerd om te melden wanneer de gebruiker gedefinieerde budgetdrempels zijn overschreden. 

De API bevat:

-   **Azure op rollen gebaseerd toegangsbeheer** -configureren toegangsbeleid op de [Azure-portal](https://portal.azure.com), wordt de [Azure CLI](https://docs.microsoft.com/azure/role-based-access-control/role-assignments-cli) of [Azure PowerShell-cmdlets](https://docs.microsoft.com/powershell/azure/overview) aangeven welke gebruikers of toepassingen kunnen toegang krijgen tot gegevens over het gebruik van het abonnement. Aanroepfuncties moeten standaard tokens van Azure Active Directory voor verificatie gebruiken. De aanroeper toevoegen aan de lezer facturering, een lezer, een eigenaar of een bijdrager rol voor toegang tot de gebruiksgegevens voor een specifieke Azure-abonnement. 
-   **Alleen Enterprise-klanten** -voor deze API is alleen beschikbaar EA klanten.
-   **Configureerbare meldingen** -gebruiker (s) wilt worden gewaarschuwd als het budget wordt omzeild opgeven.
-   **Informatie over het gebruik of kosten op basis van budgetten** -Maak uw budget op basis van verbruik of kosten naar behoefte van uw scenario.
-   **Filteren** -uw budget dat u wilt een subset van resources met behulp van de volgende configureerbare filters filteren
    - Resourcegroep
    - Resourcenaam
    - Naar gebruik
-   **Configureerbare budget perioden** -opgeven hoe vaak het budget opnieuw moet worden ingesteld en hoe lang het budget is geldig voor.

Zie voor meer informatie, de technische specificaties voor de [budgetten API](https://docs.microsoft.com/rest/api/consumption/budgets).

## <a name="reservation-recommendations-api"></a>Reservering aanbevelingen API

Deze API gebruiken om aanbevelingen voor het aanschaffen van een VM gereserveerd exemplaren. Aanbevelingen zijn ontworpen om te kunnen klanten voor het analyseren van verwachte kostenbesparingen en inkoop bedragen. 

De API bevat:

-   **Azure op rollen gebaseerd toegangsbeheer** -configureren toegangsbeleid op de [Azure-portal](https://portal.azure.com), wordt de [Azure CLI](https://docs.microsoft.com/azure/role-based-access-control/role-assignments-cli) of [Azure PowerShell-cmdlets](https://docs.microsoft.com/powershell/azure/overview) aangeven welke gebruikers of toepassingen kunnen toegang krijgen tot gegevens over het gebruik van het abonnement. Aanroepfuncties moeten standaard tokens van Azure Active Directory voor verificatie gebruiken. De aanroeper toevoegen aan de lezer facturering, een lezer, een eigenaar of een bijdrager rol voor toegang tot de gebruiksgegevens voor een specifieke Azure-abonnement. 
-   **Filteren** -de aanbeveling van de resultaten met behulp van de volgende filters aanpassen:
    - Bereik
    - Lookback periode
-   **Reserveringsinformatie voor andere aanbiedingstypen** -reserveringsinformatie is momenteel beschikbaar voor klanten met Enterprise- en Web Direct.

Zie voor meer informatie, de technische specificaties voor de [reservering aanbevelingen API](https://docs.microsoft.com/rest/api/consumption/reservationrecommendations).

## <a name="reservation-details-api"></a>Details van de reservering API

Gebruik de API-reservering Details voor informatie over eerder VM reserveringen aangeschaft zoals hoeveel verbruik is gereserveerd versus hoeveel wordt daadwerkelijk gebruikt. Ziet u gegevens op een niveau details per VM. 

De API bevat:

-   **Azure op rollen gebaseerd toegangsbeheer** -configureren toegangsbeleid op de [Azure-portal](https://portal.azure.com), wordt de [Azure CLI](https://docs.microsoft.com/azure/role-based-access-control/role-assignments-cli) of [Azure PowerShell-cmdlets](https://docs.microsoft.com/powershell/azure/overview) aangeven welke gebruikers of toepassingen kunnen toegang krijgen tot gegevens over het gebruik van het abonnement. Aanroepfuncties moeten standaard tokens van Azure Active Directory voor verificatie gebruiken. De aanroeper toevoegen aan de lezer facturering, een lezer, een eigenaar of een bijdrager rol voor toegang tot de gebruiksgegevens voor een specifieke Azure-abonnement. 
-   **Filteren** -Trim uw API-resultaat niet actief is ingesteld op een kleiner aantal reserveringen met behulp van de volgende filter:
    - Datumbereik
-   **Reserveringsinformatie voor andere aanbiedingstypen** -reserveringsinformatie is momenteel beschikbaar voor klanten met Enterprise- en Web Direct.

Zie voor meer informatie, de technische specificaties voor de [reservering Details API](https://docs.microsoft.com/rest/api/consumption/reservationsdetails).

## <a name="reservation-summaries-api"></a>Reservering samenvattingen API

Gebruik deze API voor statistische informatie over eerder VM reserveringen aangeschaft zoals hoeveel verbruik versus hoeveel daadwerkelijk wordt gebruikt de is gereserveerd. 

De API bevat:

-   **Azure op rollen gebaseerd toegangsbeheer** -configureren toegangsbeleid op de [Azure-portal](https://portal.azure.com), wordt de [Azure CLI](https://docs.microsoft.com/azure/role-based-access-control/role-assignments-cli) of [Azure PowerShell-cmdlets](https://docs.microsoft.com/powershell/azure/overview) aangeven welke gebruikers of toepassingen kunnen toegang krijgen tot gegevens over het gebruik van het abonnement. Aanroepfuncties moeten standaard tokens van Azure Active Directory voor verificatie gebruiken. De aanroeper toevoegen aan de lezer facturering, een lezer, een eigenaar of een bijdrager rol voor toegang tot de gebruiksgegevens voor een specifieke Azure-abonnement. 
-   **Filteren** -aanpassen van de resultaten bij het gebruik van de dagelijks interval met het volgende filter:
    - Gebruiksdatum
-   **Reserveringsinformatie voor andere aanbiedingstypen** -reserveringsinformatie is momenteel beschikbaar voor klanten met Enterprise- en Web Direct.
-   **Dagelijks of maandelijks aggregaties** – aanroepfuncties kunnen opgeven of ze hun samenvattingsgegevens van de reservering in de gebruikerssegmentatie dagelijks of maandelijks wilt.

Zie voor meer informatie, de technische specificaties voor de [reservering samenvattingen API](https://docs.microsoft.com/rest/api/consumption/reservationssummaries).

## <a name="price-sheet-api"></a>Prijs blad API
Enterprise-klanten kunt u deze API gebruiken voor het ophalen van de aangepaste prijscategorie voor alle meters. Ondernemingen kunnen deze gebruiken in combinatie met details en marktplaatsen gebruik gebruiksinformatie kosten berekeningen met behulp van gebruiks- en marketplace-gegevens. 

De API bevat:

-   **Azure op rollen gebaseerd toegangsbeheer** -configureren toegangsbeleid op de [Azure-portal](https://portal.azure.com), wordt de [Azure CLI](https://docs.microsoft.com/azure/role-based-access-control/role-assignments-cli) of [Azure PowerShell-cmdlets](https://docs.microsoft.com/powershell/azure/overview) aangeven welke gebruikers of toepassingen kunnen toegang krijgen tot gegevens over het gebruik van het abonnement. Aanroepfuncties moeten standaard tokens van Azure Active Directory voor verificatie gebruiken. De aanroeper toevoegen aan de lezer facturering, een lezer, een eigenaar of een bijdrager rol voor toegang tot de gebruiksgegevens voor een specifieke Azure-abonnement. 
-   **Alleen Enterprise-klanten** -voor deze API is alleen beschikbaar EA klanten. Directe klanten op het web moeten de RateCard-API gebruiken voor het ophalen prijzen. 

Zie voor meer informatie, de technische specificaties voor de [Price Sheet API](https://docs.microsoft.com/rest/api/consumption/pricesheet).

## <a name="scenarios"></a>Scenario's

Hier volgen enkele van de scenario's die mogelijk via het verbruik van API's worden gemaakt:

-   **Afstemming factuur** -heeft Microsoft kosten in rekening gebracht mij de juiste tijd?  Wat is mijn factuur en kan ik berekenen zelf?
-   **Cross-kosten** - nu dat ik hoeveel ik ben in rekening worden gebracht weet, wie in mijn organisatie nodig heeft om te betalen?
-   **Optimalisatie kosten** -ik weet hoeveel ik hebt gebracht... hoe vind ik meer uit het geld dat ik in Azure uitgaven ben?
-   **Kosten bijhouden** -ik wil weergeven hoeveel ik ben uitgaven en gebruik van Azure gedurende een bepaalde periode. Wat zijn de trends? Hoe kan ik doen beter?
-   **Azure besteden tijdens de maand** -hoeveel is mijn huidige maand tot datum de besteden? Heb ik nodig om alle aanpassingen in mijn uitgaven en/of informatie over het gebruik van Azure? Wanneer tijdens de maand ben ik verbruikt Azure de meeste?
-   **Stel waarschuwingen** : ik wil op basis van bronnen verbruik instellen of monetaire op basis van waarschuwingen op basis van een budget.

## <a name="next-steps"></a>Volgende stappen

- Zie voor meer informatie over het gebruik van Azure Billing-API's om programmatisch inzicht verkrijgen in uw Azure-gebruik [API-overzicht van Azure Billing](billing-usage-rate-card-overview.md).



