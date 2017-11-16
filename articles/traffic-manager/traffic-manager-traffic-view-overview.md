---
title: Verkeer weergeven in Azure Traffic Manager | Microsoft Docs
description: Inleiding tot Traffic Manager verkeer weergeven
services: traffic-manager
documentationcenter: traffic-manager
author: KumudD
manager: timlt
editor: 
tags: 
ms.assetid: 
ms.service: traffic-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: 
ms.workload: infrastructure
ms.date: 11/11/2017
ms.author: kumud
ms.custom: 
ms.openlocfilehash: 6b4378cb293824702dd52dcdeb86619f957b83ea
ms.sourcegitcommit: afc78e4fdef08e4ef75e3456fdfe3709d3c3680b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/16/2017
---
# <a name="traffic-manager-traffic-view"></a>Traffic Manager verkeer weergeven

>[!NOTE]
>De functie verkeer weergeven in Traffic Manager kan is openbare preview-versie en geen hetzelfde niveau van beschikbaarheid en betrouwbaarheid zoals functies die in het algemeen beschikbaarheid release. De functie wordt niet ondersteund, kan hebben beperkte mogelijkheden en mogelijk niet beschikbaar in alle Azure-locaties. Voor de meest recente meldingen op beschikbaarheid en de status van deze functie, Controleer de [Azure Traffic Manager-updates](https://azure.microsoft.com/updates/?product=traffic-manager) pagina.

Traffic Manager biedt dat u met DNS-niveau routering zodat uw eindgebruikers worden omgeleid naar orde eindpunten op basis van de routeringsmethode opgegeven toen u het profiel hebt gemaakt. Traffic Manager biedt verkeer weergave een weergave van uw gebruikersbases (op een DNS-omzetter granulatie niveau) en hun verkeer-patroon. Wanneer u verkeer weergave inschakelt, wordt deze informatie om u te bieden met bruikbare insights verwerkt. 

U kunt met behulp van verkeer weergeven:
- begrijpen waarin uw gebruikersbases bevinden (maximaal een lokale DNS-omzetter niveau granulatie).
- de hoeveelheid verkeer (waargenomen als DNS-query's verwerkt door Azure Traffic Manager) weergeven die afkomstig zijn van deze gebieden.
- Verkrijg inzicht in wat de representatieve latentie door deze gebruikers ondervonden is.
- diepgaand in de specifieke verkeerspatronen van elk van deze gebruikersbases naar Azure-regio's waar u eindpunten hebt. 

Bijvoorbeeld, kunt u verkeer weergave te begrijpen welke regio's hebben een groot aantal verkeer maar lijden onder blokkering van de hogere latenties. Vervolgens kunt u deze gegevens naar uw footprint uitbreiding naar de nieuwe Azure-regio's zodanig plannen dat deze gebruikers kunnen een lagere latentie ervaring hebben.

## <a name="how-traffic-view-works"></a>De werking van verkeer weergeven

Verkeer weergave werkt met Traffic Manager kijken naar de binnenkomende query's ontvangen in de afgelopen zeven dagen op basis van een profiel dat u deze functie is ingeschakeld. Verkeer weergave geëxtraheerd uit de binnenkomende gegevens van de query's het bron-IP van de DNS-resolver die wordt gebruikt als een representatie van de locatie van de gebruikers. Deze worden vervolgens gegroepeerd in een DNS-resolver niveau samenvattingen base regio's voor een gebruiker maken met behulp van de geografische informatie van IP-adressen bijgehouden door Traffic Manager. Azure-regio's waarop de query is gerouteerd en Hiermee wordt een overzicht van de processtroom verkeer voor gebruikers van deze gebieden kijkt Traffic Manager vervolgens.  
In de volgende stap Traffic Manager de gebruiker base regio toewijzing van de Azure-regio met het netwerk intelligence latentie tabellen houdt voor verschillende eindgebruiker netwerken om te begrijpen van de gemiddelde latentie waarmee gebruikers van deze gebieden correleert wanneer verbinding maken met Azure-regio's. Al deze berekeningen vervolgens worden gecombineerd met een niveau van lokale DNS-omzetter IP voordat het is aan u gepresenteerd. U kunt de gegevens op verschillende manieren verbruiken.

>[!NOTE]
>De latentie beschreven in de weergave van verkeer een representatieve latentie tussen de eindgebruiker en de Azure-regio's waarmee ze was verbonden, en niet de DNS-lookup-latentie.

## <a name="visual-overview"></a>Visual-overzicht

Wanneer u navigeren naar de **verkeer weergave** sectie op de pagina Traffic Manager krijgt u een geografische kaart met een overlay van verkeer weergave inzicht. De kaart bevat informatie over de gebruikersgroep en de eindpunten voor uw Traffic Manager-profiel.

### <a name="user-base-information"></a>Basis van gebruikersgegevens

Voor de lokale DNS-resolvers voor welke locatie informatie beschikbaar is, worden ze weergegeven in de kaart. De kleur van de DNS-resolver geeft de gemiddelde latentie ervaren door eindgebruikers die deze DNS-resolver voor de Traffic Manager-query's gebruikt.

Als u de muisaanwijzer op een DNS-omzetter locatie in de kaart, wordt deze weergegeven:
- het IP-adres van de DNS-resolver
- het volume van de DNS-query-verkeer door Traffic Manager gezien vanaf het
- de eindpunten op welk verkeer van de DNS resolver is gerouteerd, als een lijn tussen het eindpunt en de DNS-resolver 
- de gemiddelde latentie van die locatie naar het eindpunt, weergegeven als de kleur van de regel die deze te verbinden

### <a name="endpoint-information"></a>Informatie over endpoint

Azure-regio's waarin de eindpunten zich bevinden, worden weergegeven als blauwe punten op de kaart. Klik op een willekeurig eindpunt om te zien van de verschillende vestigingen (op basis van de DNS-resolver gebruikt) van waar het verkeer is omgeleid naar dat eindpunt. De verbindingen worden weergegeven als een lijn tussen het eindpunt en de locatie van DNS-omzetter en volgens de representatieve latentie tussen die paar zijn gekleurd. Bovendien ziet u de naam van het eindpunt, de Azure-regio waarin deze wordt uitgevoerd en het totale volume van aanvragen die zijn omgeleid naar dit Traffic Manager-profiel.


## <a name="tabular-listing-and-raw-data-download"></a>In de lijst in tabelvorm en onbewerkte gegevens downloaden

U kunt de weergave van verkeer gegevens weergeven in tabelvorm in Azure-portal. Er is een vermelding voor elk IP-adres van DNS-omzetter / eindpunt paar waarin de geografische locatie van de DNS-resolver (indien beschikbaar), naam van de Azure-regio waarin het eindpunt zich bevindt, het volume aan aanvragen die zijn gekoppeld aan deze DNS-omzetter en de vertegenwoordiger Latentie die zijn gekoppeld aan eindgebruikers die DNS gebruiken (indien beschikbaar). U kunt ook het verkeer weergavegegevens downloaden als een CSV-bestand dat kan worden gebruikt als onderdeel van een werkstroom analytics van uw keuze.

## <a name="billing"></a>Facturering

Wanneer u verkeer weergave gebruikt, kunt u wordt gefactureerd op basis van het aantal gegevenspunten gebruikt voor het maken van de inzichten die zijn gepresenteerd. Het type van het enige gegevens die wordt gebruikt, is momenteel de query's ontvangen op basis van uw Traffic Manager-profiel. Voor meer informatie over de prijzen, gaat u naar de [Traffic Manager pagina met prijzen](https://azure.microsoft.com/pricing/details/traffic-manager/).


## <a name="next-steps"></a>Volgende stappen

- Meer informatie over [de werking van Traffic Manager](traffic-manager-overview.md)
- Meer informatie over de [verkeersroutering methoden](traffic-manager-routing-methods.md) ondersteund door Traffic Manager
- Meer informatie over hoe [een Traffic Manager-profiel maken](traffic-manager-create-profile.md)

