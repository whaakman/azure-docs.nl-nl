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
ms.date: 09/19/2017
ms.author: kumud
ms.custom: 
ms.openlocfilehash: 0a07ff578c6afeedc6f3806d52bfe5aef6945c04
ms.sourcegitcommit: bd0d3ae20773fc87b19dd7f9542f3960211495f9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/18/2017
---
# <a name="traffic-manager-traffic-view"></a>Traffic Manager verkeer weergeven

>[!NOTE]
>De functie verkeer weergeven in Traffic Manager kan is openbare preview-versie en geen hetzelfde niveau van beschikbaarheid en betrouwbaarheid zoals functies die in het algemeen beschikbaarheid release. De functie wordt niet ondersteund, kan hebben beperkte mogelijkheden en mogelijk niet beschikbaar in alle Azure-locaties. Voor de meest recente meldingen op beschikbaarheid en de status van deze functie, Controleer de [Azure Traffic Manager-updates](https://azure.microsoft.com/updates/?product=traffic-manager) pagina.

Traffic Manager biedt u DNS-niveau routering zodat uw eindgebruikers worden omgeleid naar orde eindpunten op basis van de routeringsmethode voor die u is ingesteld wanneer het profiel is gemaakt. Dit biedt een weergave van uw gebruikersbases (op een DNS-omzetter granulatie niveau) en hun patroon verkeer Traffic Manager. Wanneer u verkeer weergave inschakelt, wordt deze informatie om u te bieden met bruikbare insights verwerkt. 

U kunt met behulp van verkeer weergeven:
- begrijpen waarin uw gebruikersbases bevinden (maximaal een lokale DNS-omzetter niveau granulatie).
- de hoeveelheid verkeer (waargenomen als DNS-query's verwerkt door Azure Traffic Manager) weergeven die afkomstig zijn van deze gebieden.
-  Verkrijg inzicht in wat de representatieve latentie door deze gebruikers ondervonden is.
- diepgaand in de specifieke verkeerspatronen van elk van deze gebruikersbases naar Azure-regio's waar u eindpunten hebt. 

Bijvoorbeeld, kunt u verkeer weergave te begrijpen welke regio's hebben een groot aantal verkeer maar lijden onder blokkering van de hogere latenties. Vervolgens kunt u deze gegevens naar uw footprint uitbreiding naar de nieuwe Azure-regio's zodanig plannen dat deze gebruikers kunnen een lagere latentie ervaring hebben.

## <a name="how-traffic-view-works"></a>De werking van verkeer weergeven

Verkeer weergave werkt met Traffic Manager kijken naar de binnenkomende query's ontvangen in de afgelopen zeven dagen op basis van een profiel dat u deze functie is ingeschakeld. Met deze informatie wordt het bron-IP van de DNS-resolver dat vervolgens wordt gebruikt als een representatie van de locatie van de gebruikers. Deze worden vervolgens gegroepeerd in een DNS-resolver niveau samenvattingen base regio's voor een gebruiker maken met behulp van de geografische informatie van IP-adressen bijgehouden door Traffic Manager. Azure-regio's waarop de query is gerouteerd en Hiermee wordt een overzicht van de processtroom verkeer voor gebruikers van deze gebieden kijkt Traffic Manager vervolgens.
In de volgende stap Traffic Manager de gebruiker base regio toewijzing van de Azure-regio met het netwerk intelligence latentie tabellen houdt voor verschillende eindgebruiker netwerken om te begrijpen van de gemiddelde latentie waarmee gebruikers van deze gebieden correleert wanneer verbinding maken met Azure-regio's. Al deze berekeningen vervolgens zijn tabelindeling op een niveau van lokale DNS-omzetter IP voordat het is aan u gepresenteerd. U kunt deze informatie in een werkstroom analytics van uw keuze verwerken, kunt u deze gegevens als een CSV-bestand downloaden.
Wanneer u verkeer weergave gebruikt, kunt u wordt gefactureerd op basis van het aantal gegevenspunten gebruikt voor het maken van de inzichten die zijn gepresenteerd. Het type van het enige gegevens die wordt gebruikt, is momenteel de query's ontvangen op basis van uw Traffic Manager-profiel. Voor meer informatie over de prijzen, gaat u naar de [Traffic Manager pagina met prijzen](https://azure.microsoft.com/pricing/details/traffic-manager/).


## <a name="next-steps"></a>Volgende stappen

- Meer informatie over [de werking van Traffic Manager](traffic-manager-overview.md)
- Meer informatie over de [verkeersroutering methoden](traffic-manager-routing-methods.md) ondersteund door Traffic Manager
- Meer informatie over hoe [een Traffic Manager-profiel maken](traffic-manager-create-profile.md)

