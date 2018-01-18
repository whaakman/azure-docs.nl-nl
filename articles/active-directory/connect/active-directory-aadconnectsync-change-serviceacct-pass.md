---
title: 'Azure AD Connect-synchronisatie: het Azure AD Connect-synchronisatie-serviceaccount wijzigen | Microsoft Docs'
description: Dit document onderwerp beschrijft de versleutelingssleutel en hoe deze Breek deze af nadat het wachtwoord is gewijzigd.
services: active-directory
keywords: Azure AD sync-serviceaccount, wachtwoord
documentationcenter: 
author: billmath
manager: mtillman
editor: 
ms.assetid: 76b19162-8b16-4960-9e22-bd64e6675ecc
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/12/2017
ms.author: billmath
ms.openlocfilehash: c22dd91f236454c774c00371bff668da245d45c6
ms.sourcegitcommit: f1c1789f2f2502d683afaf5a2f46cc548c0dea50
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/18/2018
---
# <a name="changing-the-azure-ad-connect-sync-service-account-password"></a>Het wijzigen van het wachtwoord voor de Azure AD Connect sync-serviceaccount
Als u het wachtwoord voor de Azure AD Connect sync-serviceaccount wijzigen, zich de Synchronization Service niet kunnen starten correct totdat u hebt de versleutelingssleutel afgebroken en het wachtwoord voor de Azure AD Connect sync-serviceaccount opnieuw geïnitialiseerd. 

Azure AD Connect als onderdeel van de Synchronisatieservices maakt gebruik van een coderingssleutel voor het opslaan van de wachtwoorden van de AD DS en Azure AD-serviceaccounts.  Deze accounts worden versleuteld voordat ze worden opgeslagen in de database. 

De versleutelingssleutel die wordt gebruikt, is beveiligd met [Windows Data Protection (DPAPI)](https://msdn.microsoft.com/library/ms995355.aspx). DPAPI beveiligt de versleuteling sleutel met behulp van de **wachtwoord van de Azure AD Connect sync-serviceaccount**. 

Als u wilt wijzigen van het wachtwoord van serviceaccount kunt u de procedures in [opgegeven van de versleutelingssleutel van de Azure AD Connect-synchronisatie](#abandoning-the-azure-ad-connect-sync-encryption-key) om dit te bereiken.  Deze procedures moeten ook worden gebruikt als u moet de versleutelingssleutel voor een bepaalde reden afbreken.

##<a name="issues-that-arise-from-changing-the-password"></a>Problemen die ontstaan bij het wijzigen van het wachtwoord
Er zijn twee dingen die worden uitgevoerd moeten wanneer u het wachtwoord van serviceaccount wijzigt.

Eerst moet u het wachtwoord onder Windows Service Control Manager wijzigen.  Totdat dit probleem opgelost is ziet u volgende fouten:


- Als u de synchronisatie-Service starten in Windows Service Control Manager probeert, ontvangt u de fout '**Windows kan de Microsoft Azure AD Sync-service niet starten op de lokale Computer**'. **Fout 1069: De service is niet gestart vanwege een aanmeldingsfout.** "
- Onder Windows Logboeken, het logboek voor systeemgebeurtenissen bevat een fout opgetreden bij **gebeurtenis-ID 7038** en het bericht '**de ADSync-service kan niet worden aangemeld bij het huidige ingestelde wachtwoord vanwege de volgende fout is: de gebruikersnaam of wachtwoord is onjuist.**'

Ten tweede onder bepaalde omstandigheden, kunnen als het wachtwoord wordt bijgewerkt, de synchronisatieservice niet meer ophalen de versleutelingssleutel via DPAPI. Zonder de versleutelingssleutel decoderen de synchronisatieservice niet van de wachtwoorden vereist voor het synchroniseren van on-premises AD en Azure AD.
U ziet bijvoorbeeld fouten:

- Onder Windows Service Control Manager als u probeert te starten van de synchronisatieservice en de versleutelingssleutel, kan niet worden opgehaald mislukt met fout "** Windows kan de Microsoft Azure AD-synchronisatie niet starten op de lokale Computer. Bekijk het gebeurtenislogboek van systeem voor meer informatie. Als dit een niet-Microsoft-service, neem contact op met de leverancier van de service en Raadpleeg servicespecifieke foutcode **-21451857952 ***. "
- Onder Windows Logboeken, het logboek voor toepassingsgebeurtenissen bevat een fout opgetreden bij **gebeurtenis-ID 6028** en foutbericht *'**de versleutelingssleutel van de server kan niet worden geopend.**'*

Volg de procedures in om ervoor te zorgen dat er geen deze fouten, [opgegeven van de versleutelingssleutel van de Azure AD Connect-synchronisatie](#abandoning-the-azure-ad-connect-sync-encryption-key) wanneer het wachtwoord wijzigt.
 
## <a name="abandoning-the-azure-ad-connect-sync-encryption-key"></a>De versleutelingssleutel van de Azure AD Connect-synchronisatie wordt afgebroken
>[!IMPORTANT]
>De volgende procedures zijn alleen van toepassing op Azure AD Connect build 1.1.443.0 of ouder.

Gebruik de volgende procedures om de versleutelingssleutel af te breken.

### <a name="what-to-do-if-you-need-to-abandon-the-encryption-key"></a>Wat te doen als u wilt afbreken van de versleutelingssleutel

Als u afbreken van de versleutelingssleutel wilt, gebruikt u de volgende procedures om dit te bereiken.

1. [De bestaande versleutelingssleutel afbreken](#abandon-the-existing-encryption-key)

2. [Geef het wachtwoord voor de AD DS-account](#provide-the-password-of-the-ad-ds-account)

3. [Initialiseren van het wachtwoord van het Azure AD sync-serviceaccount](#reinitialize-the-password-of-the-azure-ad-sync-account)

4. [De synchronisatieservice starten](#start-the-synchronization-service)

#### <a name="abandon-the-existing-encryption-key"></a>De bestaande versleutelingssleutel afbreken
De bestaande versleutelingssleutel afbreken zodat deze nieuwe versleutelingssleutel kan worden gemaakt:

1. Meld u aan bij uw Azure AD Connect-Server als beheerder.

2. Start een nieuwe PowerShell-sessie.

3. Navigeer naar de map:`$env:Program Files\Microsoft Azure AD Sync\bin\`

4. Voer de opdracht:`./miiskmu.exe /a`

![Azure AD Connect Sync Encryption Key-hulpprogramma](media/active-directory-aadconnectsync-encryption-key/key5.png)

#### <a name="provide-the-password-of-the-ad-ds-account"></a>Geef het wachtwoord voor de AD DS-account
Als de bestaande wachtwoorden worden opgeslagen in de database kunnen niet meer worden ontsleuteld, moet u de Synchronization Service voorzien van het wachtwoord van de AD DS-account. De synchronisatieservice versleutelt de met de nieuwe versleutelingssleutel wachtwoorden:

1. Synchronization Service Manager (START → Synchronization-Service) starten.
</br>![Sync Service Manager](./media/active-directory-aadconnectsync-service-manager-ui/startmenu.png)  
2. Ga naar de **Connectors** tabblad.
3. Selecteer de **AD-Connector** die overeenkomt met uw on-premises AD. Als u meer dan één AD-connector hebt, herhaalt u de volgende stappen uit voor elk van deze.
4. Onder **acties**, selecteer **eigenschappen**.
5. Selecteer in het pop-updialoogvenster **verbinding maken met Active Directory-Forest**:
6. Geef het wachtwoord van de AD DS-account in de **wachtwoord** textbox. Als u het wachtwoord niet weet, moet u deze instellen op een bekende waarde voordat u deze stap uitvoert.
7. Klik op **OK** naar het nieuwe wachtwoord opslaan en sluiten van een pop-upvenster.
![Azure AD Connect Sync Encryption Key-hulpprogramma](media/active-directory-aadconnectsync-encryption-key/key6.png)

#### <a name="reinitialize-the-password-of-the-azure-ad-sync-account"></a>Initialiseren van het wachtwoord van het Azure AD sync-serviceaccount
U kan niet rechtstreeks Geef het wachtwoord van de Azure AD-serviceaccount voor de synchronisatieservice. In plaats daarvan moet u de cmdlet gebruiken **toevoegen ADSyncAADServiceAccount** opnieuw initialiseren van het Azure AD-serviceaccount. De cmdlet het accountwachtwoord opnieuw instellen en is beschikbaar voor de synchronisatieservice:

1. Start een nieuwe PowerShell-sessie op de Azure AD Connect-server.
2. Voer cmdlet `Add-ADSyncAADServiceAccount`.
3. Geef de globale beheerder Azure AD-referenties voor uw Azure AD-tenant in het pop-upvenster.
![Azure AD Connect Sync Encryption Key-hulpprogramma](media/active-directory-aadconnectsync-encryption-key/key7.png)
4. Als dat lukt, ziet u de PowerShell-opdrachtprompt.

#### <a name="start-the-synchronization-service"></a>De synchronisatieservice starten
Nu dat de synchronisatieservice toegang tot de versleutelingssleutel en de wachtwoorden die nodig is heeft, kunt u de service opnieuw in de Windows Service Control Manager:


1. Ga naar Windows Service Control Manager (START → Services).
2. Selecteer **Microsoft Azure AD Sync** en klik op opnieuw starten.

## <a name="next-steps"></a>Volgende stappen
**Overzichtsonderwerpen**

* [Azure AD Connect-synchronisatie: inzicht en synchronisatie aanpassen](active-directory-aadconnectsync-whatis.md)

* [Uw on-premises identiteiten integreren met Azure Active Directory](active-directory-aadconnect.md)
