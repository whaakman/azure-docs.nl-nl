---
title: Back-up en herstellen met de Microsoft Authenticator-app - Azure Active Directory | Microsoft Docs
description: Leer hoe u back-up en herstellen van de referenties van uw account met behulp van de Microsoft Authenticator-app.
services: active-directory
author: eross-msft
manager: mtillman
ms.component: user-help
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 03/28/2018
ms.author: lizross
ms.reviewer: olhaun
ms.openlocfilehash: 39ec7c979294860967deb3307f5d87112b762257
ms.sourcegitcommit: cc4fdd6f0f12b44c244abc7f6bc4b181a2d05302
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/25/2018
ms.locfileid: "47106874"
---
# <a name="backup-and-recover-account-credentials-with-the-microsoft-authenticator-app"></a>Back-up en herstel van accountreferenties met de Microsoft Authenticator-app

**Van toepassing op:**

- iOS-apparaten

De Microsoft Authenticator-app back-ups van uw accountreferenties en de bijbehorende appinstellingen, zoals de volgorde van uw accounts, naar de cloud. Na de back-up, kunt u de app ook gebruiken om te herstellen van uw gegevens op een nieuw apparaat mogelijk vermijden ophalen vergrendeld out- of accounts opnieuw hoeft te.

>[!IMPORTANT]
> U moet een persoonlijk Microsoft-account en een iCloud-account voor elke back-upopslag-locatie. Maar binnen die opslaglocatie back-up van diverse accounts. U kunt bijvoorbeeld een persoonlijk account, een school-account en een account van derden, zoals Facebook, Google, enzovoort.<br><br>Alleen de referenties van uw persoonlijke en 3rd derden account zijn opgeslagen, waaronder de naam van de gebruiker en de verificatiecode van account die vereist zijn om uw identiteit te bewijzen. We geen andere gegevens die zijn gekoppeld aan uw accounts, met inbegrip van e-mailberichten of bestanden op te slaan. We geen ook koppelen of delen van uw accounts op geen enkele manier of met een ander product of service. En ten slotte uw IT-beheerder geen informatie over elk van deze accounts wordt niet ophalen.

## <a name="back-up-your-account-credentials"></a>Back-up van uw accountreferenties
Voordat u kunt back-up van uw referenties moet hebben:

- Een persoonlijke [Microsoft-account](https://account.microsoft.com/account) om te fungeren als uw herstelaccount.

- Een [iCloud-account](https://www.icloud.com/) voor de werkelijke opslaglocatie. 

Vereist dat u zich aanmeldt bij beide accounts samen biedt een betere beveiliging voor uw back-upgegevens.

**Cloud back-up inschakelen**
-   Selecteer op uw iOS-apparaat, **instellingen**, selecteer **back-up**, en schakelt u **automatische back-ups**.

    Referenties voor uw back-ups naar uw iCloud-account.

    ![iOS-instellingenscherm weergegeven met de locatie van de automatische back-instellingen](./media/microsoft-authenticator-app-backup-and-recovery/backup-and-recovery-turn-on.png)

## <a name="recover-your-account-credentials-on-your-new-device"></a>De referenties van uw account op het nieuwe apparaat herstellen
U kunt referenties voor uw account herstellen vanaf uw iCloud-account met hetzelfde Microsoft herstel dat die u hebt ingesteld toen u back-ups van uw gegevens.

### <a name="to-recover-your-information"></a>Uw gegevens worden hersteld
1.  Open de Microsoft Authenticator-app op uw iOS-apparaat, en selecteer **herstel starten** vanaf de onderkant van het scherm.

    ![Microsoft Authenticator-app, die laat zien waar Begin recovery klikken](./media/microsoft-authenticator-app-backup-and-recovery/backup-and-recovery-begin-recovery.png)

2.  Meld u aan bij uw herstelaccount, met behulp van hetzelfde persoonlijke Microsoft-account die u hebt gebruikt tijdens de back-upproces.

    Referenties voor uw account worden hersteld naar het nieuwe apparaat.

Nadat u uw recovery, zult u merken dat uw persoonlijke Microsoft-account verificatiecodes in de Microsoft Authenticator-app tussen de oude en nieuwe telefoons verschillen. De codes zijn verschillend, omdat elk apparaat een eigen unieke referentie heeft, maar beide zijn geldig en tijdens het aanmelden met behulp van de gekoppelde telefoon.

## <a name="recover-additional-accounts-requiring-more-verification"></a>Herstellen van extra accounts aanvullende verificatie vereisen
Als u pushmeldingen kunt verzenden met uw persoonlijke, werk of schoolaccounts gebruiken, krijgt u een waarschuwing op het scherm dat zegt moet u aanvullende verificatie opgeven voordat u uw gegevens kunt herstellen. Omdat pushmeldingen te verzenden met behulp van een referentie die is gekoppeld aan uw specifieke apparaat en nooit worden verzonden via het netwerk is vereist, moet u uw identiteit bewijzen voordat de referentie is gemaakt op uw apparaat.

Voor persoonlijke Microsoft-accounts kunt u uw identiteit bewijst door in te voeren van uw wachtwoord, samen met een alternatieve e-mailadres of telefoonnummer aantal. Voor werk of school-accounts, moet u een QR-code dat u van uw accountprovider scannen.

### <a name="to-provide-additional-verification-for-personal-accounts"></a>Aanvullende verificatie voor persoonlijke accounts opgeven
1.  In de **Accounts** scherm van de Microsoft Authenticator-app, selecteer de vervolgkeuzelijst pijl naast het account dat u wilt herstellen.

    ![Microsoft Authenticator-app met de beschikbare accounts met hun bijbehorende vervolgkeuzelijstpijlen](./media/microsoft-authenticator-app-backup-and-recovery/backup-and-recovery-arrow.png)

2.  Selecteer **aanmelden om te herstellen**, typ uw wachtwoord en bevestigt u uw e-mailadres of telefoonnummer-nummer als aanvullende verificatie.

    ![Microsoft Authenticator-app, zodat u kunt uw aanmeldinformatie invoeren](./media/microsoft-authenticator-app-backup-and-recovery/backup-and-recovery-sign-in.png)

### <a name="to-provide-additional-verification-for-work-or-school-accounts"></a>Aanvullende verificatie voor werk-of schoolaccount opgeven
1.  In de **Accounts** scherm van de Microsoft Authenticator-app, selecteer de vervolgkeuzelijst pijl naast het account dat u wilt herstellen.

    ![Microsoft Authenticator-app met de beschikbare accounts met hun bijbehorende vervolgkeuzelijstpijlen](./media/microsoft-authenticator-app-backup-and-recovery/backup-and-recovery-additonal-accts.png)

2.  Selecteer **scannen QR-code te herstellen**, en vervolgens scan de QR-code die is geleverd door uw beheerder.

    ![Microsoft Authenticator-app, zodat u uw QR-code scannen](./media/microsoft-authenticator-app-backup-and-recovery/backup-and-recovery-scan-qr-code.png)

    >[!NOTE]
    >Zie voor meer informatie over het registreren van een QR-code de [toevoegen sectie van de Get-accounts aan de slag met de Microsoft Authenticator-app](https://docs.microsoft.com/azure/active-directory/user-help/microsoft-authenticator-app-how-to#add-accounts-to-the-app) artikel.

## <a name="troubleshooting-backup-and-recovery-problems"></a>Het oplossen van problemen met back-up en herstel
Er zijn enkele redenen waarom de back-up mogelijk niet beschikbaar:

-   **Het wijzigen van besturingssystemen.** Uw back-up wordt opgeslagen in de cloudopslagoptie geleverd door het besturingssysteem van uw telefoon, wat betekent dat de back-up is niet beschikbaar dat als u tussen Android en iOS schakelen. In dit geval moet u uw account in de app handmatig opnieuw maken.

-   **Problemen met het netwerk of het wachtwoord.** Zorg ervoor dat u bent verbonden met een netwerk en aangemeld bij uw iCloud-account met behulp van de dezelfde Apple-id die u op uw laatste iPhone wordt gebruikt.

-   **Onbedoeld verwijderen.** Het is mogelijk dat u uw back-account verwijderd van het vorige apparaat of bij het beheren van uw cloudopslagaccount. In dit geval moet u uw account in de app handmatig opnieuw maken.

-   **Bestaande accounts van de Microsoft Authenticator.** Als u al in de Microsoft Authenticator-app hebt ingesteld, kunnen de app om te herstellen van uw back-up-accounts niet mogelijk. Voorkomen dat herstel zorgt ervoor dat uw accountgegevens worden niet met verouderde gegevens overschreven. In dit geval moet u alle bestaande gegevens verwijderen uit de bestaande accounts in de Authenticator-app instellen voordat u kunt uw back-up herstellen.

## <a name="next-steps"></a>Volgende stappen
Nu dat u hebt een back-up en referenties van uw account op het nieuwe apparaat kunt herstellen, kunt u echter ook doorgaan met de Microsoft Authenticator-app om uw identiteit te verifiëren.

## <a name="related-topics"></a>Verwante onderwerpen
- [Aan de slag gaan met de Microsoft Authenticator-app](microsoft-authenticator-app-how-to.md)  

- [Veelgestelde vragen over Microsoft Authenticator-app](microsoft-authenticator-app-faq.md)

- [Multi-Factor Authentication](https://docs.microsoft.com/azure/multi-factor-authentication/)
