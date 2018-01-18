---
title: 'Azure AD Connect: Uw installatietype selecteren | Microsoft Docs'
description: In dit onderwerp leert u hoe u selecteert het installatietype om te gebruiken voor Azure AD Connect
services: active-directory
documentationcenter: 
author: billmath
manager: mtillman
editor: 
ms.assetid: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/12/2017
ms.author: billmath
ms.openlocfilehash: 9b7bcd5856ec94367edd586c28c781ac9569a792
ms.sourcegitcommit: f1c1789f2f2502d683afaf5a2f46cc548c0dea50
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/18/2018
---
# <a name="select-which-installation-type-to-use-for-azure-ad-connect"></a>Selecteer welk installatietype moet worden gebruikt voor Azure AD Connect
Azure AD Connect heeft twee installatietypen voor nieuwe installatie: Express en aangepast. Dit onderwerp helpt u om te bepalen welke optie u moet gebruiken tijdens de installatie.

## <a name="express"></a>Express
Express is de meest voorkomende optie en wordt gebruikt door ongeveer 90% van alle nieuwe installaties. Het is ontworpen voor een configuratie die geschikt is voor de meest voorkomende scenario's voor de klant.

Er wordt vanuit gegaan:

- U hebt een enkele Active Directory lokale forest.
- U hebt een enterprise-beheerdersaccount dat u voor de installatie gebruiken kunt.
- U hebt minder dan 100.000 objecten in uw lokale Active Directory.

U krijgt:

- [Wachtwoordsynchronisatie](active-directory-aadconnectsync-implement-password-synchronization.md) on-premises naar Azure AD voor eenmalige aanmelding.
- Een configuratie die wordt gesynchroniseerd [gebruikers, groepen, contactpersonen en Windows 10-computers](active-directory-aadconnectsync-understanding-default-configuration.md).
- Synchronisatie van alle in aanmerking komende objecten in alle domeinen en alle OE's.
- [Automatische upgrade](active-directory-aadconnect-feature-automatic-upgrade.md) is ingeschakeld om ervoor te zorgen dat u altijd de meest recente versie gebruiken.

Opties voor waar u Express nog steeds kunt gebruiken:

- Als u niet synchroniseren van alle OE's wilt, u kunt nog steeds gebruiken Express en op de laatste pagina selectie ** Start het synchronisatieproces... ***. Vervolgens de installatiewizard opnieuw uitvoeren en wijzigen van de organisatie-eenheden in [configuratieopties](active-directory-aadconnectsync-installation-wizard.md#customize-synchronization-options) en geplande synchronisatie inschakelen.
- Wilt u een van de functies in Azure AD Premium, zoals wachtwoord terugschrijven inschakelen. Ga eerst via snelle ophalen van de initiële installatie is voltooid. Vervolgens de installatiewizard opnieuw uitvoeren en wijzigen de [configuratieopties](active-directory-aadconnectsync-installation-wizard.md#customize-synchronization-options).

## <a name="custom"></a>Aangepast telefoonnummer
Het aangepaste pad kunt veel meer opties dan express. Het moet worden gebruikt in alle gevallen waarin de configuratie wordt beschreven in de vorige sectie voor snelle niet representatief is voor uw organisatie.

Gebruiken wanneer:

- U hebt geen toegang tot een enterprise-beheerdersaccount in Active Directory.
- U hebt meer dan één forest of u van plan bent om te synchroniseren van meer dan één forest in de toekomst.
- U kunt domeinen hebt in uw forest niet bereikbaar is vanaf de Connect-server.
- U van plan bent voor gebruikersaanmelding federation of Pass through-verificatie te gebruiken.
- U hebt meer dan 100.000 objecten en moet een volledige SQL Server gebruiken.
- U wilt filteren op basis van de groep en niet alleen domein of filteren op basis van organisatie-eenheid gebruiken.

## <a name="upgrade-from-dirsync"></a>Upgraden van DirSync
Als u momenteel DirSync gebruikt, volgt u de stappen in [Upgrade van DirSync](active-directory-aadconnect-dirsync-upgrade-get-started.md) uw bestaande configuratie bijwerken. Er zijn twee verschillende upgrade-opties:

- In-place upgrade uit naar Connect installeert op dezelfde server.
- Parallelle implementatie Connect installeren op een nieuwe server terwijl de bestaande DirSync-server nog steeds operationeel is.

## <a name="upgrade-from-azure-ad-sync"></a>Upgrade uitvoeren vanaf Azure AD Sync
Als u Azure AD Sync momenteel gebruikt vervolgens u voert de [dezelfde stappen](active-directory-aadconnect-upgrade-previous-version.md) als bij de upgrade van één versie van Connect naar een nieuwere. Er zijn twee verschillende upgrade-opties:

- In-place upgrade uit naar Connect installeert op dezelfde server.
- Swing-migratie naar Connect op een nieuwe server installeren terwijl u de bestaande Azure AD Sync-server is nog steeds operationeel.

## <a name="migrate-from-fim2010-or-mim2016"></a>Migreren van FIM 2010 wachtwoordherstelextensies of MIM2016
Als u momenteel Forefront Identity Manager 2010 of Microsoft Identity Manager 2016 met de Azure AD-Connector gebruikt, is de enige optie een migratie. Volg de stappen [swing migratie](active-directory-aadconnect-upgrade-previous-version.md#swing-migration). In de stappen, kunt u een melding van Azure AD Sync vervangen door FIM 2010 wachtwoordherstelextensies/MIM2016.

## <a name="next-steps"></a>Volgende stappen
Afhankelijk van de optie die u hebt geselecteerd om te gebruiken, gebruikt u de tabel van inhoud naar links in uw artikel met de gedetailleerde stappen te vinden.
