---
title: Verkeersweergave in Azure Traffic Manager | Microsoft Docs
description: Inleiding tot Traffic Manager Traffic View
services: traffic-manager
documentationcenter: traffic-manager
author: KumudD
manager: jeconnoc
ms.service: traffic-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: ''
ms.workload: infrastructure
ms.date: 03/16/2018
ms.author: kumud
ms.custom: ''
ms.openlocfilehash: d1800fea2212628e7647b5250efa33ebb97957f9
ms.sourcegitcommit: 0f54b9dbcf82346417ad69cbef266bc7804a5f0e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/26/2018
ms.locfileid: "50138069"
---
# <a name="traffic-manager-traffic-view"></a>Traffic Manager Traffic View

Traffic Manager kunt dat u met de DNS-niveau routering zodat uw eindgebruikers worden doorgestuurd naar de eindpunten in orde op basis van de routeringsmethode opgegeven tijdens het maken van het profiel. Traffic View biedt Traffic Manager een weergave van uw klantenbestand (op het niveau van een DNS-resolver granulariteit) en hun gebruikspatroon verkeer. Wanneer u de weergave verkeer inschakelen, wordt deze informatie om te voorzien van inzichten verwerkt. 

Met behulp van Traffic View, kunt u het volgende doen:
- inzicht in waar uw klantenbestand zich bevindt (tot een lokale DNS-niveau resolver).
- de hoeveelheid verkeer (in acht genomen als DNS-query's van Azure Traffic Manager verwerkt) weergeven die afkomstig zijn van deze regio's.
- Krijg inzicht in wat de representatieve latentie die wordt ervaren door deze gebruikers is.
- dieper ingaan op de specifieke verkeerspatronen van elk van deze klantenbestanden naar Azure-regio's waar u eindpunten hebt. 

Bijvoorbeeld, kunt u Traffic View te begrijpen welke regio's hebben een groot aantal verkeer maar last van hogere latentie. U kunt deze informatie vervolgens gebruiken voor het plannen van uw footprint uitbreiding van nieuwe Azure-regio's, zodat deze gebruikers een lagere latentie-ervaring hebben.

## <a name="how-traffic-view-works"></a>De werking van Verkeersweergave

Traffic View werkt met Traffic Manager kijken naar de inkomende query's ontvangen in de afgelopen zeven dagen op basis van een profiel dat u deze functie is ingeschakeld. Traffic View geëxtraheerd uit de binnenkomende gegevens van de query's de bron-IP-adres van de DNS-resolver die wordt gebruikt als een weergave van de locatie van de gebruikers. Deze worden vervolgens samen gegroepeerd op een op het niveau resolver DNS-gebruiker basis regio's maken met behulp van de geografische informatie van IP-adressen van Traffic Manager beheerd. Vervolgens kijkt Traffic Manager naar de Azure-regio's waarop de query is doorgestuurd en bouwt een overzicht van de stroom verkeer voor gebruikers van deze regio's.  
In de volgende stap, Traffic Manager overeenkomt met de basis regio van de gebruiker de toewijzing van de Azure-regio met het netwerk intelligence latentie tabellen wordt bijgehouden voor verschillende eindgebruikers netwerken om te begrijpen van de gemiddelde latentie die wordt ervaren gebruikers van deze regio's als verbinding maken met Azure-regio's. Deze berekeningen vervolgens worden gecombineerd met een niveau van lokale DNS-resolver IP voordat deze aan u gepresenteerd. U kunt de gegevens op verschillende manieren gebruiken.

>[!NOTE]
>De latentie die wordt beschreven in de Traffic View is een representatieve latentie tussen de eindgebruiker en de Azure-regio's waarnaar ze met verbonden was en is niet de DNS-lookup-latentie. Traffic View maakt een schatting van de inspanningen van de latentie tussen de lokale DNS-resolver en de Azure-regio die de query is doorgestuurd naar, als er onvoldoende gegevens beschikbaar en de latentie vervolgens geretourneerd worden niet null zijn. 

## <a name="visual-overview"></a>Visueel overzicht

Wanneer u gaat u naar de **Traffic View** sectie van de pagina Traffic Manager, krijgt u een geografische kaart met een overlay van inzichten verkeer weergeven. De kaart bevat informatie over de gebruikers- en eindpunten voor uw Traffic Manager-profiel.

### <a name="user-base-information"></a>Basisgegevens van gebruiker

Voor deze lokale DNS-resolvers voor welke locatie informatie beschikbaar is, worden ze weergegeven in de kaart. De kleur van de DNS-resolver geeft aan dat de gemiddelde latentie van eindgebruikers die deze DNS-resolver voor de Traffic Manager-query's gebruikt.

Als u de muisaanwijzer over een DNS-resolver locatie op de kaart, wordt deze weergegeven:
- het IP-adres van de DNS-resolver
- het volume van de DNS-query-verkeer door Traffic Manager van het gezien
- de eindpunten voor hoe het verkeer van de DNS-server naamomzetting is doorgestuurd, als een lijn tussen het eindpunt en de DNS-resolver 
- de gemiddelde latentie van die locatie naar het eindpunt, weergegeven als de kleur van de regel verbindt deze met

### <a name="endpoint-information"></a>Informatie over endpoint

Azure-regio's waarin de eindpunten zich bevinden worden als de blauwe stippen op de kaart weergegeven. Als uw eindpunt extern is en heeft een Azure-regio aan is toegewezen, wordt het weergegeven aan de bovenkant van de kaart. Klik op elk willekeurig eindpunt om te zien van de verschillende locaties (op basis van de DNS-resolver gebruikt) vanaf waar verkeer is omgeleid naar dit eindpunt. De verbindingen worden weergegeven als een lijn tussen het eindpunt en de locatie van de DNS-resolver en worden gekleurd volgens de representatieve latentie tussen deze twee. Bovendien ziet u de naam van het eindpunt, de Azure-regio waarin deze wordt uitgevoerd en het totale volume van aanvragen die zijn doorgestuurd naar dit Traffic Manager-profiel.


## <a name="tabular-listing-and-raw-data-download"></a>Lijst in tabelvorm en op onbewerkte gegevens downloaden

U kunt de Traffic View-gegevens weergeven in tabelvorm in Azure portal. Er is een vermelding voor elke DNS-resolver-IP-adres / eindpunt koppelen die ziet u het IP-adres van de DNS-resolver, de naam en de geografische locatie van de Azure-regio's in die het eindpunt zich bevindt (indien beschikbaar), het aantal aanvragen dat is gekoppeld aan deze DNS-resolver dit eindpunt, en de representatieve latentie die is gekoppeld aan eindgebruikers met behulp van deze DNS (indien beschikbaar). U kunt ook de Traffic View-gegevens als een CSV-bestand dat kan worden gebruikt als onderdeel van een analytics-werkstroom van uw keuze downloaden.

## <a name="billing"></a>Billing

Wanneer u de weergave verkeer gebruikt, kunt u wordt gefactureerd op basis van het aantal gegevenspunten dat wordt gebruikt om te maken van de inzichten die zijn gepresenteerd. Op dit moment is het enige type gegevenspunt gebruikt de query's ontvangen op basis van uw Traffic Manager-profiel. Voor meer informatie over de prijzen, gaat u naar de [Traffic Manager-pagina met prijzen](https://azure.microsoft.com/pricing/details/traffic-manager/).


## <a name="next-steps"></a>Volgende stappen

- Informatie over [hoe Traffic Manager werkt](traffic-manager-overview.md)
- Meer informatie over de [routeringsmethoden voor verkeer](traffic-manager-routing-methods.md) ondersteund door Traffic Manager
- Meer informatie over het [een Traffic Manager-profiel maken](traffic-manager-create-profile.md)

