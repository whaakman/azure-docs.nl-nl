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
ms.date: 04/25/2019
ms.subservice: hybrid
ms.author: billmath
ms.collection: M365-identity-device-management
ms.openlocfilehash: 3e5ad7badfa44a006fd7e71d3b0e42ee95ac698d
ms.sourcegitcommit: 2028fc790f1d265dc96cf12d1ee9f1437955ad87
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/30/2019
ms.locfileid: "64918995"
---
# <a name="tutorial--setting-up-phs-as-backup-for-ad-fs-in-azure-ad-connect"></a>Zelfstudie:  WHS instellen als back-up voor AD FS in Azure AD Connect

De volgende zelfstudie begeleidt u bij het instellen van wachtwoord-hashsynchronisatie als een back-up en failover voor AD FS.  In dit document wordt ook gedemonstreerd hoe u wachtwoord-hashsynchronisatie als primaire verificatiemethode inschakelt, als AD FS is mislukt of niet beschikbaar is.

>[!NOTE] 
>Hoewel deze stappen worden doorgaans uitgevoerd tijdens de EMS- of onderbreking situaties, is het raadzaam dat u deze stappen testen en uw procedures controleren voordat er een storing optreedt.

>[!NOTE]
>In het geval dat u geen toegang tot Azure AD Connect-server hebt of de server heeft geen toegang tot het internet, kunt u contact met [Microsoft Support](https://support.microsoft.com/en-us/contactus/) om te helpen met de wijzigingen aan de kant van de Azure AD.

## <a name="prerequisites"></a>Vereisten
Deze zelfstudie is gebaseerd op de [Zelfstudie: Een enkele AD-forestomgeving federeren in de cloud](tutorial-federation.md) en deze moet u hebben gevolgd voordat u aan deze zelfstudie begint.  Als u deze zelfstudie niet hebt voltooid, moet u dit doen voordat u de stappen in dit document gaat uitvoeren.

>[!IMPORTANT]
>Voordat u overschakelt naar WHS moet u een back-up van uw AD FS-omgeving maken.  Dit kan worden gedaan met behulp van de [AD FS Rapid Restore-hulpprogramma](https://docs.microsoft.com/windows-server/identity/ad-fs/operations/ad-fs-rapid-restore-tool#how-to-use-the-tool).

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

> [!IMPORTANT]
> Let erop dat het duurt even voordat het wachtwoord-hashes met synchroniseren met Azure AD.  Dit betekent dat het duurt 3 uur voor de synchronisatie is voltooid en voordat u kunt beginnen verifiëren met behulp van de wachtwoord-hashes.

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

## <a name="switch-back-to-federation"></a>Ga terug naar federation
Nu we wordt laten zien hoe u wilt terugkeren naar federation.  U doet dit door het volgende doen:

1.  Dubbelklik op het pictogram voor Azure AD Connect dat op het bureaublad is gemaakt
2.  Klik op **Configureren**
3.  Selecteer **Gebruikersaanmelding wijzigen** en klik op **Volgende**.
4.  Voer de gebruikersnaam en het wachtwoord voor uw globale beheerder in.  Dit is het account dat is gemaakt [hier](tutorial-federation.md#create-a-global-administrator-in-azure-ad) in de vorige zelfstudie.
5.  Op de **aanmelden van gebruikers** scherm, selecteer **Federatie met AD FS** en klikt u op **volgende**.  
6. Voer op de pagina Referenties van domeinadministrator de gebruikersnaam en het wachtwoord voor contoso\Administrator in en klik op **Volgende.**
7. Klik op het scherm voor AD FS-farm **volgende**.
8. Op de **Azure AD-domein** scherm, selecteert u het domein in de vervolgkeuzelijst en klik op **volgende**.
9. Klik in het scherm **Gereed om te configureren** op **Configureren**.
10. Nadat de configuratie is voltooid, klikt u op **volgende**.
![Configureren](media/tutorial-phs-backup/backup4.png)</br>
11. Op de **federatieve connectiviteit verifiëren** scherm, klikt u op **controleren**.  U wilt configureren van DNS-records (A- en AAAA-records toevoegen) om dit te voltooien.
![Verifiëren](media/tutorial-phs-backup/backup5.png)</br>
12. Klik op **afsluiten**.

## <a name="reset-the-ad-fs-and-azure-trust"></a>AD FS en Azure-vertrouwensrelatie opnieuw instellen
We moeten nu opnieuw instellen van de vertrouwensrelatie tussen AD FS en Azure.

1.  Dubbelklik op het pictogram voor Azure AD Connect dat op het bureaublad is gemaakt
2.  Klik op **Configureren**
3.  Selecteer **beheren Federation** en klikt u op **volgende**.
4.  Selecteer **opnieuw instellen van Azure AD-vertrouwensrelatie** en klikt u op **volgende**.
![Reset](media/tutorial-phs-backup/backup6.png)</br>
5.  Op de **verbinding maken met Azure AD** scherm de gebruikersnaam en wachtwoord invoeren voor de globale beheerder.
6.  Op de **verbinding maken met AD FS** scherm contoso\Administrator gebruikersnaam en wachtwoord invoeren en op **volgende.**
7.  Op de **certificaten** scherm, klikt u op **volgende**.

## <a name="test-signing-in-with-one-of-our-users"></a>Aanmelden testen met een van onze gebruikers

1.  Ga naar [https://myapps.microsoft.com](https://myapps.microsoft.com)
2. Meld u aan met een gebruikersaccount dat is gemaakt in onze nieuwe tenant.  U moet zich aanmelden met de volgende indeling: (user@domain.onmicrosoft.com). Gebruik het wachtwoord waarmee de gebruiker zich on-premises aanmeldt.
![Verifiëren](media/tutorial-password-hash-sync/verify1.png)

U hebt nu een omgeving met een hybride identiteit ingesteld die u kunt gebruiken voor testdoeleinden en om bekend te raken met wat Azure te bieden heeft.

## <a name="next-steps"></a>Volgende stappen


- [Hardware en vereisten](how-to-connect-install-prerequisites.md) 
- [Snelle instellingen](how-to-connect-install-express.md)
- [Synchronisatie van wachtwoord-hashes](how-to-connect-password-hash-synchronization.md)
