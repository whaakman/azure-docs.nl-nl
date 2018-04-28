---
title: Back-up en herstellen van gegevens met Microsoft Authenticator - Azure | Microsoft-documenten
description: Informatie over het back-up en herstellen van de referenties van uw account met behulp van de Microsoft Authenticator-app.
services: multi-factor-authentication
documentationcenter: ''
author: eross-msft
manager: mtillman
ms.service: multi-factor-authentication
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/28/2018
ms.author: lizross
ms.reviewer: olhaun
ms.custom: end-user
ms.openlocfilehash: 0b76d2debb9814188e5551252e52bd0dae38eb50
ms.sourcegitcommit: fa493b66552af11260db48d89e3ddfcdcb5e3152
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/23/2018
---
# <a name="backup-and-recover-account-credentials-with-the-microsoft-authenticator-app"></a>Accountreferenties back-up en herstellen met de Microsoft Authenticator-app
**Van toepassing op:**

- iOS-apparaten

De Microsoft Authenticator-app back-ups van uw accountreferenties en verwante appinstellingen, zoals de volgorde van uw accounts, naar de cloud. Na de back-up, kunt u ook de app gebruiken voor het herstellen van uw informatie op een nieuw apparaat mogelijk vermijden ophalen vergrendeld out- of accounts opnieuw hoeft te.

>[!IMPORTANT]
> Moet u een persoonlijk Microsoft-account en een iCloud-account voor elke back-upopslag locatie. Maar in dat opslaglocatie back-up van diverse accounts. U kunt bijvoorbeeld een persoonlijke account, een schoolaccount en een account van derden zoals Facebook, Google, enzovoort.<br><br>De referenties van uw account zijn opgeslagen, waaronder uw gebruikersnaam en de verificatiecode 8 cijfers account dat u nodig hebt om uw identiteit met de Microsoft Authenticator-app te bewijzen. Andere gegevens die zijn gekoppeld aan uw accounts, met inbegrip van bestanden of e-mailberichten niet worden opgeslagen. We niet ook koppelen of delen van uw accounts op elke manier of met een ander product of service. En ten slotte uw IT-beheerder geen informatie over elk van deze accounts komen.

## <a name="back-up-your-account-credentials"></a>Back-up van referenties voor uw account
Voordat u kunt back-up van uw referenties moet hebben beide:

- Een persoonlijke [Microsoft-account](https://account.microsoft.com/account) om te fungeren als uw account voor herstel.

- Een [iCloud-account](https://www.icloud.com/) voor de werkelijke opslaglocatie. 

Biedt een betere beveiliging voor uw back-upgegevens waarin u zich aan te melden bij beide accounts samen.

**Cloud back-up inschakelen**
-   Selecteer op uw iOS-apparaat **instellingen**, selecteer **back-**, en schakelt u **automatische back-up**.

    Referenties voor uw account worden back-up naar uw iCloud-account.

    ![iOS-upinstellingen weergegeven met de locatie van de automatische back-instellingen](./media/authenticator-app-backup-and-recovery/backup-and-recovery-turn-on.png)

## <a name="recover-your-account-credentials-on-your-new-device"></a>De referenties van uw account op het nieuwe apparaat herstellen
U kunt uw accountreferenties herstellen vanaf uw iCloud-account met dezelfde Microsoft herstel account die u instelt wanneer u een back-up van uw gegevens.

**Uw gegevens herstellen**
1.  Open de Microsoft Authenticator-app op uw iOS-apparaat en selecteer **herstel gestart** van de onderkant van het scherm.

    ![Microsoft Authenticator-app die toont waar u klikt u op de Begin-herstel](./media/authenticator-app-backup-and-recovery/backup-and-recovery-begin-recovery.png)

2.  Meld u aan bij uw account herstellen met behulp van dezelfde persoonlijke Microsoft-account die u hebt gebruikt tijdens de back-upproces.

    Referenties voor uw account worden hersteld naar het nieuwe apparaat.

Nadat u het herstel van uw, zult u merken dat uw persoonlijke Microsoft-account verificatiecodes in de Microsoft Authenticator-app zijn de verschillen tussen uw oude en nieuwe telefoons. De codes zijn verschillend, omdat elk apparaat een eigen unieke referentie heeft, maar beide zijn ongeldig en tijdens het aanmelden met behulp van de gekoppelde telefoon.

## <a name="recover-additional-accounts-requiring-more-verification"></a>Herstellen van extra accounts meer verificatie vereisen
Als u pushmeldingen met uw persoonlijke, werk of schoolaccounts gebruikt, krijgt u een waarschuwing op het scherm die zegt moet u aanvullende verificatie opgeven voordat u uw gegevens kunt herstellen. Omdat pushmeldingen via een referentie die is gekoppeld aan uw specifieke apparaat en nooit verzonden via het netwerk vereisen, moet u uw identiteit bewijzen voordat de referentie wordt gemaakt op uw apparaat.

Voor persoonlijke Microsoft-accounts kunt u uw identiteit bewijzen door te voeren van uw wachtwoord samen met een alternatief e-mailadres of telefoonnummer nummer. Voor werk of school-accounts, moet u een QR-code die u van uw accountprovider scannen.

**Aanvullende verificatie voor persoonlijke accounts opgeven**
1.  In de **Accounts** scherm van de Microsoft Authenticator-app, selecteer de vervolgkeuzelijst pijl naast het account dat u wilt herstellen.

    ![Microsoft Authenticator-app met de beschikbare accounts met hun bijbehorende pijlen](./media/authenticator-app-backup-and-recovery/backup-and-recovery-arrow.png)

2.  Selecteer **aanmelden bij het herstellen**, typ uw wachtwoord en Bevestig het nummer van uw e-mailadres of telefoonnummer als aanvullende verificatie.

    ![Microsoft Authenticator-app, zodat u kunt de gegevens van uw aanmelding invoeren](./media/authenticator-app-backup-and-recovery/backup-and-recovery-sign-in.png)

**Aanvullende verificatie voor werk of school accounts opgeven**
1.  In de **Accounts** scherm van de Microsoft Authenticator-app, selecteer de vervolgkeuzelijst pijl naast het account dat u wilt herstellen.

    ![Microsoft Authenticator-app met de beschikbare accounts met hun bijbehorende pijlen](./media/authenticator-app-backup-and-recovery/backup-and-recovery-additonal-accts.png)

2.  Selecteer **Scan de QR-code voor het herstellen van**, en vervolgens scan de QR-code die is geleverd door uw beheerder.

    ![Microsoft Authenticator-app, zodat u scan de QR-code](./media/authenticator-app-backup-and-recovery/backup-and-recovery-scan-qr-code.png)

## <a name="troubleshooting-backup-and-recovery-problems"></a>Het oplossen van problemen met de back-up en herstel
Er zijn enkele redenen waarom de back-up mogelijk niet beschikbaar:

-   **Het wijzigen van besturingssystemen.** Uw back-up wordt opgeslagen in de cloud opslagoptie geleverd door uw telefoon-besturingssysteem, wat betekent dat de back-up is niet beschikbaar dat als u tussen Android en iOS schakelen. In dit geval moet u uw account in de app handmatig opnieuw maken.

-   **Problemen met het netwerk of het wachtwoord.** Zorg ervoor dat u bent verbonden met een netwerk en aangemeld bij uw iCloud-account met behulp van de dezelfde Apple-id worden u op uw laatste iPhone gebruikt.

-   **Per ongeluk verwijderen.** Het is mogelijk dat u uw back-account verwijderd van het vorige apparaat of bij het beheren van uw cloud storage-account. In dit geval moet u uw account in de app handmatig opnieuw maken.

-   **Bestaande Microsoft Authenticator accounts.** Als u al in de Microsoft Authenticator-app hebt ingesteld, kunnen de app voor het herstellen van uw back-up-accounts niet mogelijk. Voorkomen herstel zorgt ervoor dat uw accountgegevens door verouderde gegevens worden niet overschreven. In dit geval moet u alle bestaande gegevens van het account verwijderen uit de bestaande accounts in je verificator-app instellen voordat u uw back-up kunt herstellen.

## <a name="next-steps"></a>Volgende stappen
Nu dat u hebt een back-up en referenties van uw account op het nieuwe apparaat kunt herstellen, kunt u blijven gebruiken van de Microsoft Authenticator-app om uw identiteit te verifiëren.

## <a name="related-topics"></a>Verwante onderwerpen
- [Aan de slag met de Microsoft Authenticator-app](microsoft-authenticator-app-how-to.md)  

- [Aanmelden via uw telefoon](microsoft-authenticator-app-phone-signin-faq.md)

- [Veelgestelde vragen over Microsoft Authenticator-app](microsoft-authenticator-app-faq.md)

- [Multi-Factor Authentication](https://docs.microsoft.com/en-us/azure/multi-factor-authentication/)