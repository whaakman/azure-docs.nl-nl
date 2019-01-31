---
title: 'Zelfstudie:  WHS als back-up voor AD FS in Azure AD Connect, instellen | Microsoft Docs'
description: Demonstreert hoe u synchronisatie van wachtwoordhashes inschakelen als een back-up en voor AD FS.
services: active-directory
documentationcenter: ''
author: billmath
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.topic: article
ms.date: 01/30/2019
ms.component: hybrid
ms.author: billmath
ms.openlocfilehash: 7e7c9ca30659fd1e99989bb77406b653a8ed9e7f
ms.sourcegitcommit: a7331d0cc53805a7d3170c4368862cad0d4f3144
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/30/2019
ms.locfileid: "55303076"
---
# <a name="tutorial--setting-up-phs-as-backup-for-ad-fs-in-azure-ad-connect"></a>Zelfstudie:  WHS als back-up voor AD FS in Azure AD Connect, instellen

De volgende zelfstudie begeleidt u bij het instellen van synchronisatie van wachtwoordhashes als een back-up en failover voor AD FS.  Dit document wordt ook gedemonstreerd hoe u synchronisatie van wachtwoordhashes inschakelen als primaire verificatiemethode, als AD FS is mislukt of niet beschikbaar.

## <a name="prerequisites"></a>Vereisten
In deze zelfstudie is gebaseerd op de [zelfstudie: Een omgeving met één AD-forest naar de cloud federeren](tutorial-federation.md) en een per-vereiste is voordat u deze zelfstudie.  Als u niet in deze zelfstudie hebt voltooid, doen voordat u de stappen in dit document.

## <a name="enable-phs-in-azure-ad-connect"></a>WHS inschakelen in Azure AD Connect
De eerste stap nu dat we hebben een Azure AD Connect-omgeving die van Federatie gebruikmaakt, wordt synchronisatie van wachtwoordhashes inschakelen en Azure AD Connect om te synchroniseren van de hashes toestaan.

Ga als volgt te werk:

1.  Dubbelklik op het pictogram voor Azure AD Connect dat is gemaakt op het bureaublad
2.  Klik op **Configureren**
3.  Selecteer op de pagina extra taken **aanpassen Synchronisatieopties** en klikt u op **volgende**.
4.  Voer de gebruikersnaam en wachtwoord voor de globale beheerder.  Dit account is gemaakt [hier](tutorial-federation.md#create-a-global-administrator-in-azure-ad) in de vorige zelfstudie.
5.  Op de **verbinding maken met uw mappen** scherm, klikt u op **volgende**.
6.  Op de **domein en OE filteren** scherm, klikt u op **volgende**.
7.  Op de **optionele functies** scherm selectievakje **wachtwoord-hashsynchronisatie** en klikt u op **volgende**.
![Selecteren](media/tutorial-phs-backup/backup1.png)</br>
8.  Op de **klaar om te configureren** scherm op **configureren**.
9.  Nadat de configuratie is voltooid, klikt u op **afsluiten**.
10. Dat is alles.  U bent klaar.  Wachtwoord-hashsynchronisatie wordt nu uitgevoerd en kan worden gebruikt als een back-up als AD FS niet beschikbaar is.

## <a name="switch-to-password-hash-synchronization"></a>Schakel over naar wachtwoord-hashsynchronisatie
Nu we wordt laten zien hoe u Schakel over naar wachtwoord-hashsynchronisatie als u wilt.  Ga als volgt te werk:

1. Dubbelklik op het pictogram voor Azure AD Connect dat is gemaakt op het bureaublad
2.  Klik op **Configureren**
3.  Selecteer **aanmelden van gebruikers wijzigen** en klikt u op **volgende**.
![Change](media/tutorial-phs-backup/backup2.png)</br>
4.  Voer de gebruikersnaam en wachtwoord voor de globale beheerder.  Dit account is gemaakt [hier](tutorial-federation.md#create-a-global-administrator-in-azure-ad) in de vorige zelfstudie.
5.  Op de **aanmelden van gebruikers** scherm, selecteer **wachtwoord-Hashsynchronisatie** en zet een vinkje in de **gebruikersaccounts niet converteren** vak.  
6.  Laat de standaardwaarde **eenmalige aanmelding inschakelen** geselecteerd en klik op **volgende**.
7.  Op de **eenmalige aanmelding inschakelen** scherm op **volgende**.
8.  Op de **klaar om te configureren** scherm, klikt u op **configureren**.
9.  Nadat de configuratie is voltooid, klikt u op **afsluiten**.
10. Gebruikers kunnen nu hun wachtwoorden gebruiken om aan te melden bij Azure en Azure services.

## <a name="test-signing-in-with-one-of-our-users"></a>Aanmelden met een van onze gebruikers testen

1.  Blader naar [http://myapps.microsoft.com](http://myapps.microsoft.com)
2. Meld u aan met een gebruikersaccount dat is gemaakt in onze nieuwe tenant.  U moet zich aanmelden met de volgende indeling: (user@domain.onmicrosoft.com). Gebruik hetzelfde wachtwoord die gebruikmaakt van de gebruiker aan te melden bij on-premises.</br>
![Controleer of](media/tutorial-password-hash-sync/verify1.png)</br>

## <a name="next-steps"></a>Volgende stappen


- [Hardware en vereisten](how-to-connect-install-prerequisites.md) 
- [Snelle instellingen](how-to-connect-install-express.md)
- [Synchronisatie van wachtwoord-hashes](how-to-connect-password-hash-synchronization.md)|
