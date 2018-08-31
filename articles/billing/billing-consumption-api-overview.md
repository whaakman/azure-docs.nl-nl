---
title: Azure-verbruik API-overzicht | Microsoft Docs
description: Meer informatie over hoe Azure-verbruik API's bieden u programmatische toegang tot de kosten en gebruiksgegevens voor uw Azure-resources.
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
ms.openlocfilehash: 88b7909e78f8bd36ce456eee60587acbbb94b6cd
ms.sourcegitcommit: f94f84b870035140722e70cab29562e7990d35a3
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/30/2018
ms.locfileid: "43286745"
---
# <a name="azure-consumption-api-overview"></a>Azure-verbruik API-overzicht 

De Azure-verbruik API's bieden u programmatische toegang tot de kosten en gebruiksgegevens voor uw Azure-resources. Deze API's ondersteunen momenteel alleen Enterprise-inschrijvingen en Web Direct-abonnementen (met een paar uitzonderingen). De API's worden voortdurend bijgewerkt ter ondersteuning van andere typen Azure-abonnementen.

Azure-verbruik-API's bieden toegang tot:
- Enterprise- en Web Direct-klanten 
    - Gebruiksdetails 
    - Marketplace-kosten 
    - Reservering-aanbevelingen 
    - Details van de reservering 
    - Samenvattingen van de reservering 
- Enterprise-klanten 
    - Prijslijst 
    - Budgetten 
    - Tegoeden 

## <a name="usage-details-api"></a>Informatie over het gebruik API

De gebruik gegevens-API gebruiken om de kosten in rekening gebracht en gebruiksgegevens ophalen voor alle Azure 1e partij resources. Informatie is in de vorm van detail gebruiksrecords die momenteel worden uitgezonden, één keer per meter per resource per dag. Gegevens kunnen worden gebruikt om toevoegen om de kosten voor alle resources of kosten onderzoeken / gebruik op een specifieke resource (s). 

De API omvat:

-   **Meten van het niveau van verbruiksgegevens** - gegevens weergeven met inbegrip van gebruik kosten, het verzenden van de kosten in rekening gebracht, meter en welke Azure-resource die de kosten in rekening gebracht heeft betrekking op. Details van alle gebruiksrecords worden toegewezen aan een dagelijkse bucket.
-   **Azure Role-based Access Control** -configureren toegangsbeleid op de [Azure-portal](https://portal.azure.com), wordt de [Azure CLI](https://docs.microsoft.com/azure/role-based-access-control/role-assignments-cli) of [Azure PowerShell-cmdlets](https://docs.microsoft.com/powershell/azure/overview) om op te geven welke gebruikers of toepassingen kunnen toegang krijgen tot gegevens over het gebruik van het abonnement. Aanroepers moeten standaard Azure Active Directory-tokens gebruiken voor verificatie. De oproepende functie toevoegen aan de factureren voor lezer, lezer, eigenaar, Inzender rol of toegang krijgen tot gegevens over gebruik voor een specifieke Azure-abonnement. 
-   **Filteren** -Verwijder uw API-resultatenset omlaag naar een kleiner aantal gebruiksrecords van gegevens met behulp van de volgende filters:
    - Gebruik end / gebruik start
    - Resourcegroep
    - Resourcenaam
-   **De gegevens worden verzameld** -gebruik OData expressies van toepassing op statistische informatie over het gebruik op label of filteren van eigenschappen
-   **Gebruik voor andere typen** -details van gebruiksgegevens is momenteel beschikbaar voor Enterprise- en Web Direct-klanten.

Zie voor meer informatie, de technische specificatie voor de [Usage Details API](https://docs.microsoft.com/rest/api/consumption/usagedetails).

## <a name="marketplace-charges-api"></a>Marketplace-kosten API

De Marketplace-kosten in rekening gebracht-API gebruiken om de kosten in rekening gebracht en gebruiksgegevens ophalen voor alle Marketplace-resources (Azure 3e partij aanbiedingen). Deze gegevens kan worden gebruikt om de kosten oplopen voor alle Marketplace-resources of kosten onderzoeken / gebruik op een specifieke resource (s). 

De API omvat:

-   **Meten van het niveau van verbruiksgegevens** - gegevens weergeven met inbegrip van marketplace gebruik kosten, het verzenden van de kosten in rekening gebracht, meter en welke resources de kosten in rekening gebracht heeft betrekking op. Details van alle gebruiksrecords worden toegewezen aan een dagelijkse bucket.
-   **Azure Role-based Access Control** -configureren toegangsbeleid op de [Azure-portal](https://portal.azure.com), wordt de [Azure CLI](https://docs.microsoft.com/azure/role-based-access-control/role-assignments-cli) of [Azure PowerShell-cmdlets](https://docs.microsoft.com/powershell/azure/overview) om op te geven welke gebruikers of toepassingen kunnen toegang krijgen tot gegevens over het gebruik van het abonnement. Aanroepers moeten standaard Azure Active Directory-tokens gebruiken voor verificatie. De oproepende functie toevoegen aan de factureren voor lezer, lezer, eigenaar, Inzender rol of toegang krijgen tot gegevens over gebruik voor een specifieke Azure-abonnement. 
-   **Filteren** -Verwijder uw API-resultatenset omlaag naar een kleiner aantal marketplace-records met behulp van de volgende filters:
    - Gebruik start / gebruik beëindigen
    - Resourcegroep
    - Resourcenaam
-   **Gebruik voor andere typen** -Marketplace-gegevens is momenteel beschikbaar voor Enterprise- en Web Direct-klanten.

Zie voor meer informatie, de technische specificatie voor de [Marketplace-kosten API](https://docs.microsoft.com/rest/api/consumption/marketplaces).

## <a name="balances-api"></a>Saldi met een API

Enterprise-klanten kunnen de API saldi gebruiken om op te halen een maandelijkse samenvatting van informatie over de saldi, aankopen in de nieuwe Azure Marketplace servicekosten, aanpassingen en overschrijdingskosten. U kunt deze informatie voor de huidige factureringsperiode of een punt in het verleden. Ondernemingen kunnen deze gegevens gebruiken om uit te voeren van een vergelijking met handmatig berekende samenvatting kosten in rekening gebracht. Deze API biedt geen resource-specifieke informatie en een samengevoegde weergave van de kosten. 

De API omvat:

-   **Azure Role-based Access Control** -configureren toegangsbeleid op de [Azure-portal](https://portal.azure.com), wordt de [Azure CLI](https://docs.microsoft.com/azure/role-based-access-control/role-assignments-cli) of [Azure PowerShell-cmdlets](https://docs.microsoft.com/powershell/azure/overview) om op te geven welke gebruikers of toepassingen kunnen toegang krijgen tot gegevens over het gebruik van het abonnement. Aanroepers moeten standaard Azure Active Directory-tokens gebruiken voor verificatie. De oproepende functie toevoegen aan de factureren voor lezer, lezer, eigenaar, Inzender rol of toegang krijgen tot gegevens over gebruik voor een specifieke Azure-abonnement. 
-   **Alleen Enterprise-klanten** deze API is alleen beschikbaar EA-klanten. 
    - Klanten moeten beschikken over machtigingen van de Enterprise-beheerder deze API aan te roepen 

Zie voor meer informatie, de technische specificatie voor de [-saldi met een API](https://docs.microsoft.com/rest/api/consumption/balances).

## <a name="budgets-api"></a>Budgetten API

Enterprise-klanten kunnen deze API gebruiken om de kosten of gebruik budgetten voor resources, resourcegroepen of factureringsmeters maken. Zodra deze informatie is bepaald, kan waarschuwingen worden geconfigureerd om te melden wanneer de gebruiker gedefinieerde budgetdrempelwaarden worden overschreden. 

De API omvat:

-   **Azure Role-based Access Control** -configureren toegangsbeleid op de [Azure-portal](https://portal.azure.com), wordt de [Azure CLI](https://docs.microsoft.com/azure/role-based-access-control/role-assignments-cli) of [Azure PowerShell-cmdlets](https://docs.microsoft.com/powershell/azure/overview) om op te geven welke gebruikers of toepassingen kunnen toegang krijgen tot gegevens over het gebruik van het abonnement. Aanroepers moeten standaard Azure Active Directory-tokens gebruiken voor verificatie. De oproepende functie toevoegen aan de factureren voor lezer, lezer, eigenaar, Inzender rol of toegang krijgen tot gegevens over gebruik voor een specifieke Azure-abonnement. 
-   **Alleen Enterprise-klanten** -deze API is alleen beschikbaar EA-klanten.
-   **Configureerbare meldingen** -gebruiker (s) wilt worden gewaarschuwd als het budget teruggezet opgeven.
-   **Gebruik of kosten op basis van budgetten** -maken van uw budget op basis van verbruik of kosten als nodig is voor uw scenario.
-   **Filteren** -Filter uw budget tot een kleinere subset van resources met behulp van de volgende filters kunnen worden geconfigureerd
    - Resourcegroep
    - Resourcenaam
    - Naar gebruik
-   **Configureerbare budget perioden** -opgeven hoe vaak het budget opnieuw moet worden ingesteld en hoe lang het budget is geldig voor.

Zie voor meer informatie, de technische specificatie voor de [budgetten API](https://docs.microsoft.com/rest/api/consumption/budgets).

## <a name="reservation-recommendations-api"></a>Reservering Recommendations-API

Gebruik deze API aanbevelingen ophalen voor het kopen van gereserveerde VM-instanties. Aanbevelingen zijn ontworpen om te kunnen klanten voor het analyseren van verwachte kosten te besparen en inkoop bedragen. 

De API omvat:

-   **Azure Role-based Access Control** -configureren toegangsbeleid op de [Azure-portal](https://portal.azure.com), wordt de [Azure CLI](https://docs.microsoft.com/azure/role-based-access-control/role-assignments-cli) of [Azure PowerShell-cmdlets](https://docs.microsoft.com/powershell/azure/overview) om op te geven welke gebruikers of toepassingen kunnen toegang krijgen tot gegevens over het gebruik van het abonnement. Aanroepers moeten standaard Azure Active Directory-tokens gebruiken voor verificatie. De oproepende functie toevoegen aan de factureren voor lezer, lezer, eigenaar, Inzender rol of toegang krijgen tot gegevens over gebruik voor een specifieke Azure-abonnement. 
-   **Filteren** -uw aanbeveling Resultaten met behulp van de volgende filters aanpassen:
    - Bereik
    - Lookback periode
-   **Reservering van gegevens voor verschillende typen** -informatie van de reservering is momenteel beschikbaar voor Enterprise- en Web Direct-klanten.

Zie voor meer informatie, de technische specificatie voor de [reservering Recommendations-API](https://docs.microsoft.com/rest/api/consumption/reservationrecommendations).

## <a name="reservation-details-api"></a>Details van de reservering API

Gebruik de API van de reservering Details voor informatie over eerder VM reserveringen aangeschafte, zoals hoeveel verbruik is gereserveerd en hoeveel wordt daadwerkelijk gebruikt. U kunt zien dat gegevens op een niveau details per VM. 

De API omvat:

-   **Azure Role-based Access Control** -configureren toegangsbeleid op de [Azure-portal](https://portal.azure.com), wordt de [Azure CLI](https://docs.microsoft.com/azure/role-based-access-control/role-assignments-cli) of [Azure PowerShell-cmdlets](https://docs.microsoft.com/powershell/azure/overview) om op te geven welke gebruikers of toepassingen kunnen toegang krijgen tot gegevens over het gebruik van het abonnement. Aanroepers moeten standaard Azure Active Directory-tokens gebruiken voor verificatie. De oproepende functie toevoegen aan de factureren voor lezer, lezer, eigenaar, Inzender rol of toegang krijgen tot gegevens over gebruik voor een specifieke Azure-abonnement. 
-   **Filteren** -Verwijder uw API-resultatenset omlaag naar een kleiner aantal reserveringen met behulp van de volgende filters:
    - Datumbereik
-   **Reservering van gegevens voor verschillende typen** -informatie van de reservering is momenteel beschikbaar voor Enterprise- en Web Direct-klanten.

Zie voor meer informatie, de technische specificatie voor de [reservering Details API](https://docs.microsoft.com/rest/api/consumption/reservationsdetails).

## <a name="reservation-summaries-api"></a>Samenvattingen van de reservering API

Gebruik deze API voor statistische gegevens over eerder VM reserveringen aangeschafte, zoals hoeveel verbruik versus hoeveel daadwerkelijk wordt gebruikt in het aggregaat is gereserveerd. 

De API omvat:

-   **Azure Role-based Access Control** -configureren toegangsbeleid op de [Azure-portal](https://portal.azure.com), wordt de [Azure CLI](https://docs.microsoft.com/azure/role-based-access-control/role-assignments-cli) of [Azure PowerShell-cmdlets](https://docs.microsoft.com/powershell/azure/overview) om op te geven welke gebruikers of toepassingen kunnen toegang krijgen tot gegevens over het gebruik van het abonnement. Aanroepers moeten standaard Azure Active Directory-tokens gebruiken voor verificatie. De oproepende functie toevoegen aan de factureren voor lezer, lezer, eigenaar, Inzender rol of toegang krijgen tot gegevens over gebruik voor een specifieke Azure-abonnement. 
-   **Filteren** -resultaten van uw op maat kunt bij het gebruik van de dagelijks interval met het volgende filter:
    - Gebruiksdatum
-   **Reservering van gegevens voor verschillende typen** -informatie van de reservering is momenteel beschikbaar voor Enterprise- en Web Direct-klanten.
-   **Dagelijks of maandelijks aggregaties** – aanroepers kunnen aangeven of ze hun gegevens van de reservering samenvatting in het dagelijkse of maandelijkse tijdsinterval.

Zie voor meer informatie, de technische specificatie voor de [reservering samenvattingen API](https://docs.microsoft.com/rest/api/consumption/reservationssummaries).

## <a name="price-sheet-api"></a>Prijs blad API
Zakelijke klant kan deze API gebruiken voor het ophalen van de aangepaste prijzen voor alle meters. Ondernemingen kunnen dit gebruiken in combinatie met gebruik van gegevens en marktplaatsen gebruiksinformatie om uit te voeren van de berekening van de kosten met behulp van gebruiks-en marketplace. 

De API omvat:

-   **Azure Role-based Access Control** -configureren toegangsbeleid op de [Azure-portal](https://portal.azure.com), wordt de [Azure CLI](https://docs.microsoft.com/azure/role-based-access-control/role-assignments-cli) of [Azure PowerShell-cmdlets](https://docs.microsoft.com/powershell/azure/overview) om op te geven welke gebruikers of toepassingen kunnen toegang krijgen tot gegevens over het gebruik van het abonnement. Aanroepers moeten standaard Azure Active Directory-tokens gebruiken voor verificatie. De oproepende functie toevoegen aan de factureren voor lezer, lezer, eigenaar, Inzender rol of toegang krijgen tot gegevens over gebruik voor een specifieke Azure-abonnement. 
-   **Alleen Enterprise-klanten** -deze API is alleen beschikbaar EA-klanten. Web Direct-klanten moeten de RateCard API gebruiken om de prijzen. 

Zie voor meer informatie, de technische specificatie voor de [prijs blad API](https://docs.microsoft.com/rest/api/consumption/pricesheet).

## <a name="scenarios"></a>Scenario's

Hier volgen enkele van de scenario's die mogelijk zijn via het verbruik van API's worden gemaakt:

-   **Factuur afstemming** -heeft Microsoft kosten in rekening gebracht mij de juiste hoeveelheid?  Wat is mijn factuur en kan ik berekenen zelf?
-   **Cross-kosten in rekening gebracht** - nu dat ik hoeveel ik ben wordt in rekening gebracht weten, wie in mijn organisatie nodig heeft om te betalen?
-   **Kosten van optimalisatie** -weet ik hoeveel ik hebt gebracht... hoe vind ik meer uit het geld dat ik besteed aan Azure?
-   **Kosten bijhouden** -ik wil zien hoeveel ik ben uitgaven en met behulp van Azure na verloop van tijd. Wat zijn de trends? Hoe kan ik doen beter?
-   **Azure-uitgaven gedurende de maand** -hoeveel is mijn huidige maand tot heden de uitgaven? Moet ik breng de wijzigingen in mijn uitgaven en/of gebruik van Azure? Wanneer tijdens de maand ben ik verbruikt Azure het meest?
-   **Waarschuwingen instellen** : ik wil graag om in te stellen op basis van een resource verbruik of monetaire gebaseerde waarschuwingen op basis van een budget.

## <a name="next-steps"></a>Volgende stappen

- Zie voor meer informatie over het gebruik van Azure facturerings-API's voor het programmatisch Krijg inzicht in uw Azure-gebruik [Azure Billing API-overzicht](billing-usage-rate-card-overview.md).



