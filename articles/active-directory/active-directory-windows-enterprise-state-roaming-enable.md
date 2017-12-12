---
title: Enterprise State Roaming in Azure Active Directory inschakelen | Microsoft Docs
description: Veelgestelde vragen over Enterprise State Roaming-instellingen in Windows-apparaten. Enterprise State Roaming biedt gebruikers een uniforme ervaring via hun Windows-apparaten en minder tijd nodig voor het configureren van een nieuw apparaat.
services: active-directory
keywords: Enterprise-status tijdens roaming windows cloud, het inschakelen van enterprise state roaming
documentationcenter: 
author: tanning
manager: mtillman
editor: curtand
ms.assetid: f71d66fd-7f9e-45eb-9cfe-5d989870f8a4
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/03/2017
ms.author: markvi
ms.openlocfilehash: 3a8714ddbda10d8e1b4a8de35711101f4c8a0106
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/11/2017
---
# <a name="enable-enterprise-state-roaming-in-azure-active-directory"></a>Enterprise state roaming inschakelen in Azure Active Directory
Enterprise State Roaming is beschikbaar voor elke organisatie met een Azure AD Premium of Enterprise Mobility + Security (EMS)-licentie. Zie voor meer informatie over het verkrijgen van een Azure AD-abonnement de [productpagina Azure AD](https://azure.microsoft.com/services/active-directory).

Wanneer u Enterprise State Roaming inschakelt, is uw organisatie automatisch een gratis, beperkt gebruik licentie verleend voor Azure Rights Management. Dit gratis abonnement is beperkt tot het versleutelen en ontsleutelen van de instellingen van de onderneming en toepassingsgegevens gesynchroniseerd door Enterprise State Roaming. U moet hebben [een betaald abonnement](https://azure.microsoft.com/pricing/details/active-directory/) gebruik van de volledige functionaliteit van Azure Rights Management.

## <a name="to-enable-enterprise-state-roaming"></a>Enterprise State Roaming inschakelen

1. Aanmelden bij [Azure AD-beheercentrum](https://aad.portal.azure.com/).

2. Selecteer **Azure Active Directory** &gt; **apparaten** &gt; **apparaatinstellingen**.

3. Selecteer **gebruikers kunnen instellingen en app-gegevens synchroniseren via apparaten**. Zie voor meer informatie [apparaatinstellingen configureren](https://docs.microsoft.com/azure/active-directory/device-management-azure-portal).
  
  ![afbeelding van Apparaatinstelling met het label van gebruikers kan instellingen en app-gegevens synchroniseren via apparaten](./media/active-directory-windows-enterprise-state-roaming-enable/device-settings.png)
  
Voor een apparaat met Windows 10 Enterprise State Roaming service gebruiken, moet het apparaat met een Azure AD-identiteit verifiëren. Identiteit van de gebruiker primaire aanmelden is hun Azure AD-identiteit voor apparaten die zijn gekoppeld aan Azure AD, zodat er geen aanvullende configuratie vereist. Voor apparaten die gebruikmaken van lokale Active Directory, de IT-beheerder moet [de domein-apparaten verbinden met Azure AD voor Windows 10 optreedt](active-directory-azureadjoin-devices-group-policy.md).

## <a name="data-storage"></a>Opslag van gegevens
Enterprise State Roaming gegevens wordt gehost in een of meer [Azure-regio's](https://azure.microsoft.com/regions/) dat beste zijn uitgelijnd met de waarde land/regio in de Azure Active Directory-exemplaar. Enterprise State Roaming gegevens is gepartitioneerd op basis van drie belangrijke geografische regio's: Noord-Amerika, EMEA en APAC. Gegevens voor de tenant Enterprise State Roaming lokaal bevindt met de geografische regio en wordt niet gerepliceerd tussen regio's.  Bijvoorbeeld:
Land/regio-waarde | de gegevens heeft gehost
---------------------|-------------------------
Een land EMEA zoals 'Frankrijk' of 'Zambia' | een of van de Azure-regio's binnen Europa 
Een Noord-Amerika land zoals 'Verenigde Staten' of 'Canada' | een of meer van de Azure-regio's binnen de Verenigde Staten
Een land APAC zoals 'Australië' of 'Nieuw-Zeeland' | een of meer van de Azure-regio's binnen Azië
Zuid-Amerika en Antarctica regio 's | een of meer Azure-regio's binnen de Verenigde Staten

De land/regio-waarde is ingesteld als onderdeel van het maakproces van de Azure AD-directory en vervolgens kan niet worden gewijzigd. Als u meer informatie op uw locatie voor de gegevensopslag moet, het bestand een ticket met [ondersteuning van Azure](https://azure.microsoft.com/support/options/).

## <a name="view-per-user-device-sync-status"></a>De apparaatstatus synchronisatie per gebruiker weergeven
Volg deze stappen om een statusrapport per gebruiker apparaat synchronisatie weer te geven.

1. Aanmelden bij [Azure AD-beheercentrum](https://aad.portal.azure.com/).

2. Selecteer **Azure Active Directory** &gt; **gebruikers en groepen** &gt; **alle gebruikers**.

3. Selecteer de gebruiker en selecteer vervolgens **apparaten**.

4. Onder **weergeven**, selecteer **apparaten synchroniseren instellingen en app-gegevens** om weer te geven van de synchronisatiestatus.
  
  ![afbeelding van gegevens van de apparaatinstellingen synchronisatie](./media/active-directory-windows-enterprise-state-roaming-enable/sync-status.png)
  
5. Als er apparaten synchroniseren voor deze gebruiker, ziet u de apparaten als hier weergegeven.
  
  ![afbeelding van de kolomgegevens apparaat synchroniseren](./media/active-directory-windows-enterprise-state-roaming-enable/device-status-row.png)

## <a name="data-retention"></a>Bewaartijd van gegevens
Gegevens die zijn gesynchroniseerd met Azure met behulp van Enterprise State Roaming moeten worden bewaard totdat deze handmatig wordt verwijderd of totdat is vastgesteld dat de betrokken gegevens verlopen. 

### <a name="explicit-deletion"></a>Expliciet verwijderen
Expliciet verwijderen is wanneer een Azure-beheerder worden verwijderd van een gebruiker of een map of anders expliciet aanvragen dat gegevens worden verwijderd.

* **Verwijderen van een gebruiker**: wanneer een gebruiker wordt verwijderd in Azure AD, wordt het gebruikersaccount zwervende na 90-180 dagen verwijderd. 
* **Verwijderen van een Directory**: verwijderen van een volledige map in Azure AD is een directe bewerking. Alle instellingsgegevens die zijn gekoppeld aan die directory na 90 tot 180 dagen wordt verwijderd. 
* **Op aanvraag verwijdering**: als de beheerder van Azure AD wil Verwijder handmatig de gegevens of instellingsgegevens voor een specifieke gebruiker, de beheerder een ticket met kunt indienen [ondersteuning van Azure](https://azure.microsoft.com/support/). 

### <a name="stale-data-deletion"></a>Verouderde gegevens verwijderen
Gegevens die niet is geopend voor één jaar ('de bewaarperiode'), worden behandeld als verouderd en kan worden verwijderd uit Azure. De bewaarperiode kan worden gewijzigd maar niet minder dan 90 dagen. Verouderde gegevens mogelijk een specifieke set van Windows-toepassing of alle instellingen voor een gebruiker. Bijvoorbeeld:

* Als u apparaten geen toegang tot een verzameling bepaalde instellingen (bijvoorbeeld een toepassing wordt verwijderd van het apparaat of een instellingengroep zoals 'Thema' is uitgeschakeld voor alle apparaten van een gebruiker), en vervolgens die verzameling verlopen na de bewaarperiode is en kan worden verwijderd . 
* Als een gebruiker heeft de synchronisatie van instellingen op alle apparaten zijn uitgeschakeld, klikt u vervolgens geen van de instellingsgegevens wordt geopend en alle instellingsgegevens voor die gebruiker wordt zijn verouderd en kan worden verwijderd na de bewaarperiode. 
* Als de Azure AD-directory-beheerder uitgeschakeld Enterprise State Roaming voor de hele map, worden alle gebruikers wordt in die map stopt met het synchroniseren van de instellingen en alle instellingsgegevens voor alle gebruikers worden verlopen en kan worden verwijderd na de bewaarperiode. 

### <a name="deleted-data-recovery"></a>Verwijderde gegevens te herstellen
Het bewaarbeleid voor gegevens kan niet worden geconfigureerd. Nadat de gegevens worden definitief verwijderd, is het niet hersteld. De voor instellingsgegevens is echter alleen vanaf Azure, niet van de apparaten van eindgebruikers verwijderd. Als een apparaat later opnieuw verbinding mee met de service Enterprise State Roaming maakt, worden de instellingen opnieuw gesynchroniseerd en opgeslagen in Azure.

## <a name="related-topics"></a>Verwante onderwerpen
* [Overzicht van Enterprise State Roaming](active-directory-windows-enterprise-state-roaming-overview.md)
* [Instellingen en veelgestelde vragen voor dataroaming](active-directory-windows-enterprise-state-roaming-faqs.md)
* [Instellingen voor beleid en MDM voor synchronisatie van instellingen voor groep](active-directory-windows-enterprise-state-roaming-group-policy-settings.md)
* [Windows 10 zwervende naslaginformatie](active-directory-windows-enterprise-state-roaming-windows-settings-reference.md)
* [Problemen oplossen](active-directory-windows-enterprise-state-roaming-troubleshooting.md)
