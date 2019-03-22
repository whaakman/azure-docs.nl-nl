---
title: 'Zelfstudie:  Wachtwoord-hashsynchronisatie instellen als back-up voor AD FS in Azure AD Connect | Microsoft Docs'
description: Laat u zien hoe u wachtwoord-hashsynchronisatie als een back-up en voor AD FS inschakelt.
services: active-directory
documentationcenter: ''
author: billmath
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.topic: tutorial
ms.date: 01/30/2019
ms.subservice: hybrid
ms.author: billmath
ms.collection: M365-identity-device-management
ms.openlocfilehash: 2281bc451a5acf9e4e634a124161a3e8b0734deb
ms.sourcegitcommit: 5839af386c5a2ad46aaaeb90a13065ef94e61e74
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/19/2019
ms.locfileid: "58090505"
---
# <a name="tutorial--setting-up-phs-as-backup-for-ad-fs-in-azure-ad-connect"></a>Zelfstudie:  WHS instellen als back-up voor AD FS in Azure AD Connect

De volgende zelfstudie begeleidt u bij het instellen van wachtwoord-hashsynchronisatie als een back-up en failover voor AD FS.  In dit document wordt ook gedemonstreerd hoe u wachtwoord-hashsynchronisatie als primaire verificatiemethode inschakelt, als AD FS is mislukt of niet beschikbaar is.

## <a name="prerequisites"></a>Vereisten
Deze zelfstudie is gebaseerd op de [Zelfstudie: Een enkele AD-forestomgeving federeren in de cloud](tutorial-federation.md) en deze moet u hebben gevolgd voordat u aan deze zelfstudie begint.  Als u deze zelfstudie niet hebt voltooid, moet u dit doen voordat u de stappen in dit document gaat uitvoeren.

## <a name="enable-phs-in-azure-ad-connect"></a>WHS inschakelen in Azure AD Connect
Nu we een Azure AD Connect-omgeving die gebruikmaakt van federatie hebben, bestaat de eerste stap uit het inschakelen van wachtwoord-hashsynchronisatie en Azure AD Connect in staat tellen om de hashes te synchroniseren.

Ga als volgt te werk:

1.  Dubbelklik op het pictogram voor Azure AD Connect dat op het bureaublad is gemaakt
2.  Klik op **Configureren**
3.  Selecteer op de pagina Extra taken de optie **Synchronisatieopties aanpassen** en klik op **Volgende**.
4.  Voer de gebruikersnaam en het wachtwoord voor uw globale beheerder in.  Dit account is [hier](tutorial-federation.md#create-a-global-administrator-in-azure-ad) gemaakt in de vorige zelfstudie.
5.  Klik in het scherm **Verbinding maken met uw directory’s** op **Volgende**.
6.  Klik in het scherm **Domein- en OE-filters** op **Volgende**.
7.  Schakel in het scherm **Optionele functies** het selectievakje **Wachtwoord-hashsynchronisatie** in en klik op **Volgende**.
![Selecteren](media/tutorial-phs-backup/backup1.png)</br>
8.  Klik in het scherm **Gereed om te configureren** op **Configureren**.
9.  Klik zodra de configuratie is voltooid op **Afsluiten**.
10. Dat is alles.  U bent klaar.  Wachtwoord-hashsynchronisatie wordt nu uitgevoerd en kan worden gebruikt als een back-up als AD FS niet beschikbaar is.

## <a name="switch-to-password-hash-synchronization"></a>Overschakelen naar wachtwoord-hashsynchronisatie
We laten u nu zien hoe u overschakelt naar wachtwoord-hashsynchronisatie. Voordat u begint, moet u overwegen onder welke omstandigheden u gaat overschakelen. Schakel niet over voor tijdelijke redenen, zoals netwerkstoringen, een klein probleem met AD FS of een probleem dat betrekking heeft op een subset van uw gebruikers. Als u besluit te maken over te schakelen omdat het oplossen van het probleem te lang duurt, doet u het volgende:

1. Dubbelklik op het pictogram voor Azure AD Connect dat op het bureaublad is gemaakt
2.  Klik op **Configureren**
3.  Selecteer **Gebruikersaanmelding wijzigen** en klik op **Volgende**.
![Wijzigen](media/tutorial-phs-backup/backup2.png)</br>
4.  Voer de gebruikersnaam en het wachtwoord voor uw globale beheerder in.  Dit account is [hier](tutorial-federation.md#create-a-global-administrator-in-azure-ad) gemaakt in de vorige zelfstudie.
5.  Selecteer in het scherm **Aanmelding door gebruiker** de optie **Wachtwoord-hashsynchronisatie** en plaats een vinkje in het vak **Gebruikersaccounts niet converteren**.  
6.  Laat de standaardwaarde **Eenmalige aanmelding inschakelen** geselecteerd en klik op **Volgende**.
7.  Klik in het scherm **Eenmalige aanmelding** inschakelen op **Volgende**.
8.  Klik in het scherm **Gereed om te configureren** op **Configureren**.
9.  Zodra de configuratie is voltooid, klikt u op **Afsluiten**.
10. Gebruikers kunnen nu hun wachtwoord gebruiken om zich aan te melden bij Azure en Azure-services.

## <a name="test-signing-in-with-one-of-our-users"></a>Aanmelden testen met een van onze gebruikers

1. Blader naar [https://myapps.microsoft.com](https://myapps.microsoft.com)
2. Meld u aan met een gebruikersaccount dat is gemaakt in onze nieuwe tenant.  U moet zich aanmelden met de volgende indeling: (user@domain.onmicrosoft.com). Gebruik hetzelfde wachtwoord dat de gebruiker gebruikt om zich on-premises aan te melden.</br>
   ![Verifiëren](media/tutorial-password-hash-sync/verify1.png)</br>

## <a name="next-steps"></a>Volgende stappen


- [Hardware en vereisten](how-to-connect-install-prerequisites.md) 
- [Snelle instellingen](how-to-connect-install-express.md)
- [Synchronisatie van wachtwoord-hashes](how-to-connect-password-hash-synchronization.md)|
