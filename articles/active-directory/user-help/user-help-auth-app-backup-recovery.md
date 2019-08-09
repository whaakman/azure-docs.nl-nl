---
title: Back-ups maken en herstellen met Microsoft Authenticator-app-Azure Active Directory | Microsoft Docs
description: Meer informatie over het maken van een back-up en het herstellen van uw account referenties met behulp van de app Microsoft Authenticator.
services: active-directory
author: eross-msft
manager: daveba
ms.subservice: user-help
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 01/24/2019
ms.author: lizross
ms.reviewer: olhaun
ms.collection: M365-identity-device-management
ms.openlocfilehash: ba99001159277d9d221910cafa4f5165ae2bd812
ms.sourcegitcommit: aa042d4341054f437f3190da7c8a718729eb675e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/09/2019
ms.locfileid: "68880757"
---
# <a name="backup-and-recover-account-credentials-with-the-microsoft-authenticator-app"></a>Back-ups maken en account referenties herstellen met de app Microsoft Authenticator

**Van toepassing op:**

- iOS-apparaten

De Microsoft Authenticator-app maakt een back-up van uw account referenties en gerelateerde app-instellingen, zoals de volg orde van uw accounts, naar de Cloud. Na het maken van een back-up kunt u de app ook gebruiken om uw gegevens op een nieuw apparaat te herstellen, waardoor het voor komen van vergrendelde of het opnieuw maken van accounts.

> [!IMPORTANT]
> U hebt één persoonlijk Microsoft-account en één iCloud-account nodig voor elke opslag locatie voor back-ups. Maar binnen die opslag locatie kunt u een back-up maken van verschillende accounts. U kunt bijvoorbeeld een persoonlijk account, een school account en een account van derden, zoals Facebook, Google, enzovoort, hebben.
>
> Alleen de referenties van uw persoonlijke en derde account worden opgeslagen, inclusief uw gebruikers naam en de account verificatie code die nodig is om uw identiteit te bewijzen. Er worden geen andere gegevens opgeslagen die zijn gekoppeld aan uw accounts, met inbegrip van e-mail berichten of bestanden. We koppelen of delen uw accounts ook niet op enigerlei wijze of met andere producten of services. En ten slotte krijgt de IT-beheerder geen informatie over een van deze accounts.

## <a name="back-up-your-account-credentials"></a>Back-up maken van uw account referenties

Voordat u een back-up van uw referenties kunt maken, moet u het volgende hebben:

- Een persoonlijke [Microsoft-account](https://account.microsoft.com/account) om als uw herstel account te fungeren.

- Een [iCloud-account](https://www.icloud.com/) voor de werkelijke opslag locatie.

Als u zich wilt aanmelden bij beide accounts, zorgt u voor een betere beveiliging van uw back-upgegevens.

### <a name="to-turn-on-cloud-backup"></a>Cloud back-up inschakelen

- Selecteer op uw iOS-apparaat **instellingen**, selecteer **back-up**en schakel vervolgens **iCloud-back-up**in.

    Er wordt een back-up van uw account referenties gemaakt op uw iCloud-account.

    ![scherm iOS-instellingen, waarin de locatie van de iCloud-back-upinstellingen wordt weer gegeven](./media/user-help-auth-app-backup-recovery/backup-and-recovery-turn-on.png)

## <a name="recover-your-account-credentials-on-your-new-device"></a>Uw account referenties herstellen op het nieuwe apparaat

U kunt uw account referenties herstellen vanuit uw iCloud-account met hetzelfde micro soft-herstel account dat u hebt ingesteld bij het maken van een back-up van uw gegevens.

### <a name="to-recover-your-information"></a>Uw gegevens herstellen

1. Open de Microsoft Authenticator-app op uw iOS-apparaat en selecteer vervolgens **herstel starten** onder aan het scherm.

    ![App Microsoft Authenticator, waarin wordt weer gegeven waar u op herstel starten klikt](./media/user-help-auth-app-backup-recovery/backup-and-recovery-begin-recovery.png)

2. Meld u aan bij uw herstel account met dezelfde persoonlijke Microsoft-account die u hebt gebruikt tijdens het back-upproces.

    Uw account referenties worden hersteld naar het nieuwe apparaat.

Nadat u uw herstel hebt voltooid, ziet u mogelijk dat uw persoonlijke Microsoft-account verificatie codes in de Microsoft Authenticator-app verschillen tussen uw oude en nieuwe telefoons. De codes verschillen, omdat elk apparaat een eigen unieke referentie heeft, maar beide geldig zijn en werken tijdens het aanmelden met behulp van de bijbehorende telefoon.

## <a name="recover-additional-accounts-requiring-more-verification"></a>Extra accounts herstellen waarvoor meer verificatie is vereist

Als u push meldingen met uw persoonlijke, werk-of school account gebruikt, ontvangt u een waarschuwing op het scherm met de melding dat u aanvullende verificatie moet opgeven voordat u uw gegevens kunt herstellen. Omdat push meldingen vereisen dat een referentie wordt gebruikt die is gekoppeld aan uw specifieke apparaat en nooit via het netwerk wordt verzonden, moet u uw identiteit bewijzen voordat de referentie op uw apparaat wordt gemaakt.

Voor persoonlijke micro soft-accounts kunt u uw identiteit bewijzen door uw wacht woord in te voeren en een alternatief e-mail adres of telefoon nummer. Voor werk-of school accounts moet u een QR-code scannen die u hebt ontvangen van uw account provider.

### <a name="to-provide-additional-verification-for-personal-accounts"></a>Aanvullende verificatie voor persoonlijke accounts opgeven

1. Selecteer in het scherm **accounts** van de Microsoft Authenticator-app de vervolg keuze pijl naast het account dat u wilt herstellen.

    ![Microsoft Authenticator app, waarbij de beschik bare accounts worden weer gegeven met de bijbehorende vervolg keuzelijst pijlen](./media/user-help-auth-app-backup-recovery/backup-and-recovery-arrow.png)

2. Selecteer **aanmelden om te herstellen**, typ uw wacht woord en bevestig uw e-mail adres of telefoon nummer als aanvullende verificatie.

    ![Microsoft Authenticator-app, zodat u uw aanmeldings gegevens kunt invoeren](./media/user-help-auth-app-backup-recovery/backup-and-recovery-sign-in.png)

### <a name="to-provide-additional-verification-for-work-or-school-accounts"></a>Aanvullende verificatie voor werk-of school accounts opgeven

1. Selecteer in het scherm **accounts** van de Microsoft Authenticator-app de vervolg keuze pijl naast het account dat u wilt herstellen.

    ![Microsoft Authenticator app, waarbij de beschik bare accounts worden weer gegeven met de bijbehorende vervolg keuzelijst pijlen](./media/user-help-auth-app-backup-recovery/backup-and-recovery-additional-accts.png)

2. Selecteer **QR-code scannen om te herstellen**en scan de QR-code.

    ![Microsoft Authenticator app, zodat u uw QR-code kunt scannen](./media/user-help-auth-app-backup-recovery/backup-and-recovery-scan-qr-code.png)

    >[!NOTE]
    >Als dit de eerste keer is dat u de Microsoft Authenticator-app instelt, wordt u mogelijk gevraagd of u de app toegang wilt geven tot uw camera (iOS) of dat de app afbeeldingen en record video (Android) mag maken. U moet **toestaan** inschakelen zodat de verificator-app toegang heeft tot uw camera om een foto van de QR-code in de volgende stap te maken. Als u de camera niet toestaat, kunt u nog steeds de verificator-app instellen, maar u moet de code gegevens hand matig toevoegen. Zie [hand matig een account toevoegen aan de app](user-help-auth-app-add-account-manual.md)voor meer informatie over het hand matig toevoegen van de code.
    >
    >Zie voor meer informatie over het verkrijgen van een QR-code aan [de slag met de Microsoft Authenticator-app](https://docs.microsoft.com/azure/active-directory/user-help/user-help-auth-app-download-install) of [Stel beveiligings informatie in voor het gebruik van een verificator-app](https://docs.microsoft.com/azure/active-directory/user-help/security-info-setup-auth-app), op basis van het feit of uw beheerder beveiligings gegevens heeft ingeschakeld.

## <a name="troubleshooting-backup-and-recovery-problems"></a>Problemen met back-up en herstel oplossen

Er zijn een aantal redenen waarom uw back-up mogelijk niet beschikbaar is:

- **Besturings systemen wijzigen.** Uw back-up wordt opgeslagen in de Cloud-opslag optie van het besturings systeem van uw telefoon. Dit betekent dat de back-up niet beschikbaar is als u schakelt tussen Android en iOS. In dit geval moet u uw account hand matig opnieuw in de app maken.

- **Problemen met het netwerk of het wacht woord.** Zorg ervoor dat u bent verbonden met een netwerk en u bent aangemeld bij uw iCloud-account met behulp van de Apple die u hebt gebruikt op uw laatste iPhone.

- **Verwijderen per ongeluk.** Het is mogelijk dat u uw back-upaccount hebt verwijderd van het vorige apparaat of tijdens het beheren van uw Cloud-opslag account. In dit geval moet u uw account hand matig opnieuw in de app maken.

- **Bestaande Microsoft Authenticator-accounts.** Als u al accounts hebt ingesteld in de app Microsoft Authenticator, kan de app uw back-upaccount niet herstellen. Door herstel te voor komen, zorgt u ervoor dat uw account gegevens niet worden overschreven met verouderde informatie. In dit geval moet u de bestaande account gegevens verwijderen uit de bestaande accounts die in de verificator-app zijn ingesteld voordat u uw back-up kunt herstellen.

## <a name="next-steps"></a>Volgende stappen

Nu u een back-up hebt gemaakt en uw account referenties hebt hersteld naar uw nieuwe apparaat, kunt u de app Microsoft Authenticator blijven gebruiken om uw identiteit te verifiëren. Zie [Aanmelden bij uw accounts met behulp van de app Microsoft Authenticator](user-help-sign-in.md)voor meer informatie.

## <a name="related-topics"></a>Verwante onderwerpen

- [Wat is de Microsoft Authenticator-app?](user-help-auth-app-overview.md)

- [Veelgestelde vragen over Microsoft Authenticator-app](user-help-auth-app-faq.md)

- [Multi-Factor Authentication](https://docs.microsoft.com/azure/multi-factor-authentication/)
