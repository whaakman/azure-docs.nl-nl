---
title: 'Azure AD Connect: Selecteer het installatietype | Microsoft Docs'
description: In dit onderwerp leert u hoe u het installatietype voor Azure AD Connect selecteren
services: active-directory
documentationcenter: ''
author: billmath
manager: daveba
editor: ''
ms.assetid: ''
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 07/12/2017
ms.subservice: hybrid
ms.author: billmath
ms.openlocfilehash: d0092c67675ab3ab7c13185f4e7180621232884b
ms.sourcegitcommit: 5978d82c619762ac05b19668379a37a40ba5755b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/31/2019
ms.locfileid: "55489101"
---
# <a name="select-which-installation-type-to-use-for-azure-ad-connect"></a>Selecteer het installatietype voor Azure AD Connect
Azure AD Connect heeft twee installatietypen voor nieuwe installatie: Express en aangepast. Dit onderwerp helpt u om te bepalen welke optie voor het gebruik tijdens de installatie.

## <a name="express"></a>Express
Express is de meest voorkomende optie en wordt gebruikt door ongeveer 90% van alle nieuwe installaties. Het is ontworpen voor een configuratie die geschikt is voor de meest voorkomende scenario's voor de klant.

Hierbij wordt ervan uitgegaan:

- U hebt een enkele Active Directory on-premises forest.
- U hebt een enterprise-administrator-account die kunt u voor de installatie.
- U hebt minder dan 100.000 objecten in uw on-premises Active Directory.

U krijgt:

- [Wachtwoord-hashsynchronisatie](how-to-connect-password-hash-synchronization.md) van on-premises naar Azure AD voor eenmalige aanmelding.
- Een configuratie die wordt gesynchroniseerd [gebruikers, groepen, contactpersonen en Windows 10-computers](concept-azure-ad-connect-sync-default-configuration.md).
- Synchronisatie van alle in aanmerking komende objecten in alle domeinen en alle OE's.
- [Automatische upgrade](how-to-connect-install-automatic-upgrade.md) om ervoor te zorgen dat u altijd de meest recente beschikbare versie is ingeschakeld.

Opties voor waar u nog steeds Express kunt gebruiken:

- Als u niet synchroniseren van alle OE's wilt, kunt u nog steeds Express gebruikt en op de laatste pagina, schakel ** Start het synchronisatieproces... ***. De installatiewizard opnieuw uitvoeren en wijzigen van de organisatie-eenheden in [configuratieopties](how-to-connect-installation-wizard.md#customize-synchronization-options) en geplande synchronisatie in te schakelen.
- Wilt u een van de nieuwe functies in Azure AD Premium, zoals het terugschrijven van wachtwoorden inschakelen. Ga eerst via express om op te halen van de eerste installatie is voltooid. Voer de installatiewizard opnieuw uit en wijzig de [configuratieopties](how-to-connect-installation-wizard.md#customize-synchronization-options).

## <a name="custom"></a>Aangepast telefoonnummer
Het aangepaste pad kunt veel meer opties dan express. Het moet worden gebruikt in alle gevallen waarin de configuratie die wordt beschreven in de vorige sectie voor snelle niet representatief zijn voor uw organisatie.

Gebruiken wanneer:

- U hebt geen toegang tot een enterprise-beheerdersaccount in Active Directory.
- U hebt meer dan één forest of u van plan bent om te synchroniseren van meer dan één forest in de toekomst.
- U hebt domeinen in uw forest niet bereikbaar is vanaf de Connect-server.
- U van plan bent federation of Pass through-verificatie voor aanmelden van gebruikers te gebruiken.
- U hebt meer dan 100.000 objecten en moet een volledige SQL Server gebruiken.
- U wilt gebruiken op basis van een groep filteren en niet alleen domein of OE filteren.

## <a name="upgrade-from-dirsync"></a>Upgraden van DirSync
Als u momenteel van DirSync gebruikmaakt, volgt u de stappen in [Upgrade van DirSync](how-to-dirsync-upgrade-get-started.md) uw bestaande configuratie bijwerken. Er zijn twee verschillende upgrade-opties:

- In-place upgrade naar het Connect installeert op dezelfde server.
- Parallelle implementatie Connect installeren op een nieuwe server terwijl de bestaande DirSync-server nog steeds operationeel is.

## <a name="upgrade-from-azure-ad-sync"></a>Een upgrade uitvoeren voor Azure AD Sync
Als u Azure AD Sync momenteel gebruikt, dan kunt u volgen de [dezelfde stappen](how-to-upgrade-previous-version.md) als bij de upgrade van een verbinding maken met versie naar een nieuwere. Er zijn twee verschillende upgrade-opties:

- In-place upgrade naar het Connect installeert op dezelfde server.
- Swingmigratie Connect op een nieuwe server installeren tijdens de bestaande Azure AD Sync-server nog steeds operationeel is.

## <a name="migrate-from-fim2010-or-mim2016"></a>Migreren van FIM 2010 wachtwoordherstelextensies of MIM2016
Als u momenteel van Forefront Identity Manager 2010 of Microsoft Identity Manager 2016 met de Azure AD-Connector gebruikmaakt, is de enige mogelijkheid een migratie. Volg de stappen [swingmigratie](how-to-upgrade-previous-version.md#swing-migration). In de stappen, kunt u een melding van Azure AD Sync vervangen door FIM 2010 wachtwoordherstelextensies/MIM2016.

## <a name="next-steps"></a>Volgende stappen
Afhankelijk van de optie die u hebt geselecteerd om te gebruiken, gebruikt u de inhoudsopgave aan de linkerkant te vinden van uw artikel met gedetailleerde stappen.
