---
title: Azure en Dynamics 365 advies service-aanbieding - definiëren aanbiedingsinstellingen | Microsoft Docs
description: Handleiding voor het definiëren van instellingen in een Azure- of Dynamics 365 advies serviceaanbieding vallen in de Cloud Partner-Portal bieden.
services: Azure, Marketplace, Cloud Partner Portal,
documentationcenter: ''
author: qianw211
manager: Patrick.Butler
editor: ''
ms.assetid: ''
ms.service: marketplace
ms.workload: ''
ms.tgt_pltfrm: ''
ms.devlang: ''
ms.topic: conceptual
ms.date: 11/30/2018
ms.author: pbutlerm
ms.openlocfilehash: 590aa440f35f97e854fa827b0d8db1c3f9211faf
ms.sourcegitcommit: a512360b601ce3d6f0e842a146d37890381893fc
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/11/2019
ms.locfileid: "54231934"
---
# <a name="offer-settings-tab"></a>Tabblad instellingen bieden

Op de **nieuwe aanbieding** scherm, de eerste stap is het maken van de identiteit van de aanbieding. De identiteit van de aanbieding bestaat uit drie onderdelen: **Aanbiedings-ID**, **uitgevers-ID**, en **naam**. Elk van deze onderdelen wordt in de volgende secties beschreven.

![Maken van een nieuwe service-aanbieding advies - tabblad instellingen bieden](media/consultingoffer-settings-tab.png)

### <a name="offer-id"></a>Aanbiedings-id

Deze id is een unieke naam die u maakt wanneer u de aanbieding voor het eerst verzendt. Deze moet bestaan alleen uit kleine letters, alfanumerieke tekens, streepjes of onderstrepingstekens bevatten. De **aanbiedings-ID** zichtbaar is in de URL en de invloed van zoekmachineresultaten. Een voorbeeld is *yourcompanyname_exampleservice*.

Zoals wordt weergegeven in het voorbeeld wordt de **aanbiedings-ID** wordt toegevoegd aan de uitgevers-ID te maken van een unieke id. Deze unieke id wordt weergegeven als een permanente koppeling die kan worden gereserveerd en wordt geïndexeerd door de zoekmachines.

>[!Note]
>Nadat een aanbieding gepubliceerd is, kan de id kan niet worden bijgewerkt.

### <a name="publisher-id"></a>Uitgevers-ID

Deze id is gerelateerd aan uw account. Nadat u zich aanmelden met uw organisatieaccount, uw **uitgevers-ID** wordt weergegeven in de vervolgkeuzelijst.

### <a name="name"></a>Name

Deze tekenreeks wordt weergegeven als de aanbiedingsnaam van de op AppSource of in de Azure Marketplace. De **naam** vak is beperkt tot 50 tekens. De revisor mogelijk om de titel voor het toevoegen van de duur en bieden dat u wilt de aanbiedingsnaam van uw te bewerken.

Het volgende voorbeeld laat zien hoe de aanbiedingsnaam van de is samengesteld. 

![Een nieuwe advies service-aanbieding maken](media/cppsampleconsultingoffer.png)

De aanbiedingsnaam van de bestaat uit vier onderdelen:

-   **Duur:** Gedefinieerd op de **Storefront Details** tabblad van de editor. Duur kan worden uitgedrukt in uren, dagen of weken.
-   **Type van de service:** Gedefinieerd op de **Storefront Details** tabblad van de editor. Typen van de services zijn `Assessment`, `Briefing`, `Implementation`, `Proof of concept`, en `Workshop`.
-   **Voorzetsel:** Door de revisor is ingevoegd.
-   **Naam:** Gedefinieerd op de **instellingen bieden** pagina.

>[!Note]
>De **naam** vak is beperkt tot 50 tekens. De revisor mogelijk om de titel voor het toevoegen van de duur en bieden dat u wilt de aanbiedingsnaam van uw te bewerken.

De volgende lijst bevat enkele goed benoemde aanbieding namen:

-   Essentials voor professionele diensten: 1 uur Briefing
-   Cloudplatform voor migratie: 1 uur Briefing
-   PowerApps en Microsoft Flow: 1 dag-Workshop
-   Azure Machine Learning-Services: 3-wk PoC
-   Brick en klikt u op van een Retailanalyse oplossing: 1 uur Briefing
-   Gebruik uw eigen gegevens: 1 wk-Workshop
-   Cloud-analyse: 3 dagen-Workshop
-   Training voor Power BI: 3 dagen-Workshop
-   Verkoop Management-oplossing: 1 week-implementatie
-   CRM-Snelstartgids: 1 dag-Workshop
-   Dynamics 365 for Sales: 2-daagse evaluatie

Nadat u vult het **instellingen bieden** tabblad, sla uw inzending. De aanbiedingsnaam van de wordt nu weergegeven boven de editor en u vindt deze in **alle biedt**.

## <a name="next-steps"></a>Volgende stappen

Nu kunt u [winkel om details te bepalen of in de Azure Marketplace of AppSource publiceren](./cpp-consulting-service-storefront-details.md).
