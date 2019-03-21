---
title: 'Azure AD Connect-synchronisatie:  De Azure AD Connect Sync-serviceaccount wijzigen | Microsoft Docs'
description: Dit document onderwerp beschrijft de versleutelingssleutel en hoe u deze afbreken nadat het wachtwoord is gewijzigd.
services: active-directory
keywords: Azure AD sync-serviceaccount en het wachtwoord
documentationcenter: ''
author: billmath
manager: daveba
editor: ''
ms.assetid: 76b19162-8b16-4960-9e22-bd64e6675ecc
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 10/31/2018
ms.subservice: hybrid
ms.author: billmath
ms.collection: M365-identity-device-management
ms.openlocfilehash: 15d0d537a23e21eeda3b284e7ec706cde2b443e7
ms.sourcegitcommit: 2d0fb4f3fc8086d61e2d8e506d5c2b930ba525a7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/18/2019
ms.locfileid: "58014072"
---
# <a name="changing-the-azure-ad-connect-sync-service-account-password"></a>Het wachtwoord voor de Azure AD Connect sync-serviceaccount wijzigen
Als u het wachtwoord voor de Azure AD Connect sync-serviceaccount wijzigen, zich de Synchronization Service niet kunnen starten goed totdat u hebt de versleutelingssleutel afgebroken en het wachtwoord voor de Azure AD Connect sync-serviceaccount opnieuw geïnitialiseerd. 

Azure AD Connect, als onderdeel van de Synchronisatieservices maakt gebruik van een coderingssleutel voor het opslaan van de wachtwoorden van de AD DS en Azure AD-service-accounts.  Deze accounts worden versleuteld voordat ze worden opgeslagen in de database. 

De versleutelingssleutel die wordt gebruikt is beveiligd met [Windows DPAPI (Data Protection)](https://msdn.microsoft.com/library/ms995355.aspx). DPAPI beveiligt de versleuteling sleutel met behulp van de **wachtwoord van het Azure AD Connect sync-serviceaccount**. 

Als u wilt wijzigen van het wachtwoord van serviceaccount kunt u de procedures in [wordt de versleutelingssleutel van de Azure AD Connect Sync afgebroken](#abandoning-the-azure-ad-connect-sync-encryption-key) om dit te doen.  Deze procedures moeten ook worden gebruikt als u wilt afbreken van de versleutelingssleutel voor een bepaalde reden.

## <a name="issues-that-arise-from-changing-the-password"></a>Problemen die ontstaan bij het wijzigen van het wachtwoord
Er zijn twee dingen die worden uitgevoerd moeten wanneer u het wachtwoord van serviceaccount wijzigt.

Eerst moet u het wachtwoord bij de Windows Service Control Manager te wijzigen.  Totdat dit probleem opgelost is, ziet u volgende fouten:


- Als u de synchronisatieservice niet starten in Windows Service Control Manager probeert, ontvangt u de fout '**Windows kan de Microsoft Azure AD Sync-service niet starten op de lokale Computer**'. **Fout 1069: De service is niet gestart vanwege een fout met aanmelding.** "
- In Windows-Logboeken, het logboek voor systeemgebeurtenissen bevat een fout opgetreden bij het **gebeurtenis-ID 7038** en het bericht '**de ADSync-service zich niet aanmelden bij het huidige ingestelde wachtwoord vanwege de volgende fout is: De gebruikersnaam of wachtwoord is onjuist.** "

Ten tweede onder bepaalde omstandigheden, kan als het wachtwoord is bijgewerkt, de synchronisatieservice niet meer ophalen de versleutelingssleutel via DPAPI. De synchronisatieservice kan zonder de versleutelingssleutel, de wachtwoorden die vereist zijn voor het synchroniseren van on-premises AD en Azure AD niet ontsleutelen.
U ziet fouten, zoals:

- Onder Windows Service Control Manager als u probeert om de synchronisatie-Service te starten en deze kan niet van de versleutelingssleutel ophalen, worden deze mislukt met fout "<strong>Windows kan de Microsoft Azure AD-synchronisatie niet starten op de lokale Computer. Controleer het gebeurtenislogboek van systeem voor meer informatie. Als dit een niet-Microsoft-service, neem contact op met de leverancier van de service en Raadpleeg servicespecifieke foutcode-21451857952</strong>. "
- In Windows-Logboeken, het logboek voor toepassingsgebeurtenissen bevat een fout opgetreden bij het **gebeurtenis-ID 6028** en het volgende foutbericht *"de versleutelingssleutel van de server kan niet worden geopend."*

Volg de procedures in om ervoor te zorgen dat u geen deze fouten ontvangt, [wordt de versleutelingssleutel van de Azure AD Connect Sync afgebroken](#abandoning-the-azure-ad-connect-sync-encryption-key) bij het wijzigen van het wachtwoord.
 
## <a name="abandoning-the-azure-ad-connect-sync-encryption-key"></a>De versleutelingssleutel van de Azure AD Connect Sync wordt afgebroken
>[!IMPORTANT]
>De volgende procedures zijn alleen van toepassing op Azure AD Connect versie 1.1.443.0 of ouder bent.

Gebruik de volgende procedures om de versleutelingssleutel af te breken.

### <a name="what-to-do-if-you-need-to-abandon-the-encryption-key"></a>Wat te doen als u wilt afbreken van de versleutelingssleutel

Als u afbreken van de versleutelingssleutel wilt, gebruikt u de volgende procedures om dit te doen.

1. [De synchronisatieservice stoppen](#stop-the-synchronization-service)

1. [De bestaande versleutelingssleutel afbreken](#abandon-the-existing-encryption-key)

2. [Geef het wachtwoord van het AD DS-account](#provide-the-password-of-the-ad-ds-account)

3. [Het wachtwoord van de Azure AD sync-account opnieuw initialiseren](#reinitialize-the-password-of-the-azure-ad-sync-account)

4. [De synchronisatieservice niet starten](#start-the-synchronization-service)

#### <a name="stop-the-synchronization-service"></a>De synchronisatieservice stoppen
U kunt eerst de in de Windows Service Control Manager-service stoppen.  Zorg ervoor dat de service wordt niet uitgevoerd wanneer wordt geprobeerd om deze te stoppen.  Als dit het geval is, wacht u totdat deze is voltooid en deze vervolgens te stoppen.


1. Ga naar Windows Service Control Manager (START → Services).
2. Selecteer **Microsoft Azure AD Sync** en klik op stoppen.

#### <a name="abandon-the-existing-encryption-key"></a>De bestaande versleutelingssleutel afbreken
De bestaande versleutelingssleutel afbreken zodanig dat de nieuwe versleutelingssleutel kan worden gemaakt:

1. Meld u aan bij uw Azure AD Connect-Server aan als beheerder.

2. Een nieuwe PowerShell-sessie starten.

3. Navigeer naar de map: `$env:Program Files\Microsoft Azure AD Sync\bin\`

4. Voer de opdracht uit: `./miiskmu.exe /a`

![Azure AD Connect Sync Encryption Key-hulpprogramma](./media/how-to-connect-sync-change-serviceacct-pass/key5.png)

#### <a name="provide-the-password-of-the-ad-ds-account"></a>Geef het wachtwoord van het AD DS-account
Als de bestaande wachtwoorden worden opgeslagen in de database kunnen niet meer worden ontsleuteld, moet u de Synchronization Service voorzien van het wachtwoord van het AD DS-account. De synchronisatieservice versleutelt met behulp van de nieuwe versleutelingssleutel voor wachtwoorden:

1. Start de Synchronization Service Manager (→-synchronisatieservice START).
</br>![Sync Service Manager](./media/how-to-connect-sync-change-serviceacct-pass/startmenu.png)  
2. Ga naar de **Connectors** tabblad.
3. Selecteer de **AD-Connector** die overeenkomt met uw on-premises AD. Als u meer dan één AD-connector hebt, herhaalt u de volgende stappen uit voor elk van deze.
4. Onder **acties**, selecteer **eigenschappen**.
5. Selecteer in het pop-updialoogvenster **verbinding maken met Active Directory-Forest**:
6. Voer het wachtwoord van de AD DS-account in de **wachtwoord** tekstvak. Als u het wachtwoord niet weet, moet u deze instellen op een bekende waarde voordat u deze stap uitvoert.
7. Klik op **OK** het nieuwe wachtwoord opslaan en sluiten van het pop-updialoogvenster.
![Azure AD Connect Sync Encryption Key-hulpprogramma](./media/how-to-connect-sync-change-serviceacct-pass/key6.png)

#### <a name="reinitialize-the-password-of-the-azure-ad-sync-account"></a>Het wachtwoord van de Azure AD sync-account opnieuw initialiseren
U kan niet direct het wachtwoord van de Azure AD-serviceaccount opgeven voor de synchronisatieservice. In plaats daarvan moet u het gebruik van de cmdlet **toevoegen ADSyncAADServiceAccount** om opnieuw te initialiseren van de Azure AD-serviceaccount. De cmdlet de accountwachtwoord opnieuw instellen en is beschikbaar voor de Synchronization Service:

1. Start een nieuwe PowerShell-sessie op de Azure AD Connect-server.
2. Voer cmdlet `Add-ADSyncAADServiceAccount`.
3. In het pop-updialoogvenster bieden u de Azure AD-hoofdbeheerder referenties voor uw Azure AD-tenant.
![Azure AD Connect Sync Encryption Key-hulpprogramma](./media/how-to-connect-sync-change-serviceacct-pass/key7.png)
4. Als deze geslaagd is, ziet u de PowerShell-opdrachtprompt.

#### <a name="start-the-synchronization-service"></a>De synchronisatieservice niet starten
Nu dat de Synchronization Service toegang tot de versleutelingssleutel en de wachtwoorden die nodig is heeft, kunt u de service opnieuw in de Windows Service Control Manager:


1. Ga naar Windows Service Control Manager (START → Services).
2. Selecteer **Microsoft Azure AD Sync** en klik op opnieuw starten.

## <a name="next-steps"></a>Volgende stappen
**Overzichtsonderwerpen**

* [Azure AD Connect-synchronisatie: Begrijpen en aanpassen van synchronisatie](how-to-connect-sync-whatis.md)

* [Uw on-premises identiteiten integreren met Azure Active Directory](whatis-hybrid-identity.md)
