---
title: Uw niet-Microsoft-accounts toevoegen aan de Microsoft Authenticator-app - Azure Active Directory | Microsoft Docs
description: Hoe u uw niet-Microsoft-accounts, zoals voor Google, Facebook of GitHub toevoegen aan de Microsoft Authenticator-app voor tweeledige verificatie.
services: active-directory
author: eross-msft
manager: daveba
ms.service: active-directory
ms.workload: identity
ms.subservice: user-help
ms.topic: conceptual
ms.date: 01/24/2019
ms.author: lizross
ms.reviewer: olhaun
ms.collection: M365-identity-device-management
ms.openlocfilehash: 714a78c436b7071717e2df12c2a65999920f8c63
ms.sourcegitcommit: 301128ea7d883d432720c64238b0d28ebe9aed59
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/13/2019
ms.locfileid: "56192129"
---
# <a name="add-your-non-microsoft-accounts"></a>Uw niet-Microsoft-accounts toevoegen
Uw niet-Microsoft-accounts, zoals voor Google, Facebook of GitHub toevoegen aan de Microsoft Authenticator-app voor tweeledige verificatie. De Microsoft Authenticator-app werkt met elke app die gebruikmaakt van tweeledige verificatie en alle accounts die ondersteuning biedt voor de standaarden op basis van tijd eenmalig wachtwoord (mobiele TOTP).

>[!Important]
>Voordat u uw account toevoegen kunt, moet u download en installeer de Microsoft Authenticator-app. Als u dat nog niet hebt gedaan, volgt u de stappen in de [downloaden en installeren van de app](user-help-auth-app-download-install.md) artikel.

## <a name="add-personal-accounts"></a>Persoonlijke accounts toevoegen.
Voor al uw persoonlijke accounts moet u over het algemeen:

1. Aanmelden bij uw account en schakelt u tweeledige verificatie met behulp van uw apparaat of uw PC.

2. Het account toevoegt aan de Microsoft Authenticator-app. U mogelijk gevraagd een QR-code scannen als onderdeel van dit proces.

Bieden we u hier het proces voor uw accounts Facebook, Google, GitHub en Amazon, maar dit proces is hetzelfde voor alle andere Apps, zoals Instagram, Netflix of Adobe.

## <a name="add-your-google-account"></a>Uw Google-account toevoegen
Uw Google-account toevoegen door het inschakelen van tweeledige verificatie en het account vervolgens toe te voegen aan de app.

### <a name="turn-on-two-factor-verification"></a>Tweeledige verificatie inschakelen

1. Op uw PC, gaat u naar https://myaccount.google.com/signinoptions/two-step-verification/enroll-welcome, selecteer **aan de slag**, en vervolgens uw identiteit te verifiëren.

2. Volg de stappen op de pagina verificatie in twee stappen voor uw persoonlijke Google-account inschakelen.

### <a name="add-your-google-account-to-the-app"></a>Uw Google-account toevoegen aan de app

1. Op de pagina Google op uw PC, gaat u naar de **instellen van alternatieve tweede stap** sectie **instellen** uit de **Authenticator-app** sectie.

2. Op de **codes ophalen uit de Authenticator-app** pagina, selecteert u **Android** of **iPhone** op basis van het telefoontype van uw en selecteer vervolgens **volgende**.

    Krijgt u een QR-code die u automatisch uw account te koppelen aan de Microsoft Authenticator-app kunt gebruiken. Sluit dit venster niet.

3. Open de Microsoft Authenticator-app, selecteer **account toevoegen** uit de **aanpassen en controle** pictogram in de rechterbovenhoek, en selecteer vervolgens **andere account (Google, Facebook, enz.)** .

4. De camera van uw apparaat gebruiken om te scannen van de QR-code uit de **Authenticator instellen** pagina op uw PC.

    >[!Note]
    >Als uw camera niet goed werkt, kunt u de QR-code en de URL handmatig invoeren.

5. Controleer de **Accounts** pagina van de Microsoft Authenticator-app op uw apparaat, om te controleren of gegevens over uw account wordt rechts en er is een bijbehorende 6-cijferige verificatiecode.

    Voor extra beveiliging verandert de verificatiecode in die elke 30 seconden zo wordt voorkomen dat iemand met een code meerdere keren.

6. Selecteer **volgende** op de **Authenticator instellen** pagina op uw PC, typt u de verificatiecode van zes cijfers beschikbaar in de app voor uw Google-account en selecteer vervolgens **controleren**.

7. Uw account is geverifieerd en kunt u selecteren **gedaan** sluiten de **Authenticator instellen** pagina.

    >[!NOTE]
    >Voor meer informatie over tweeledige verificatie en uw Google-account, Zie [inschakelen van verificatie in 2 stappen](https://support.google.com/accounts/answer/185839) en [meer informatie over verificatie in 2 stappen](https://www.google.com/landing/2step/help.html).

## <a name="add-your-facebook-account"></a>Uw Facebook-account toevoegen
Uw Facebook-account toevoegen door het inschakelen van tweeledige verificatie en het account vervolgens toe te voegen aan de app.

### <a name="turn-on-two-factor-verification"></a>Tweeledige verificatie inschakelen

1. Open op uw PC, Facebook, selecteert u de vervolgkeuzelijst in de rechterbovenhoek en vervolgens gaat u naar **instellingen** > **beveiliging en meld u aan**.

    De **beveiliging en meld u aan** pagina wordt weergegeven.

2. Ga omlaag naar de **gebruik tweeledige verificatie** optie in de **verificatie met twee factoren** uit en selecteer vervolgens **bewerken**.

    De **verificatie met twee factoren** pagina wordt weergegeven.

3. Selecteer **inschakelen**.

### <a name="add-your-facebook-account-to-the-app"></a>Uw Facebook-account toevoegen aan de app

1. Op de Facebook-pagina op uw PC, gaat u naar de **toevoegen van een back-up** uit en kies vervolgens **Setup** uit de **Authentication-app** gebied.

    Krijgt u een QR-code die u automatisch uw account te koppelen aan de Microsoft Authenticator-app kunt gebruiken. Sluit dit venster niet.

2. Open de Microsoft Authenticator-app, selecteer **account toevoegen** uit de **aanpassen en controle** pictogram in de rechterbovenhoek, en selecteer vervolgens **andere account (Google, Facebook, enz.)** .

3. De camera van uw apparaat gebruiken om te scannen van de QR-code uit de **twee factor authentication** pagina op uw PC.

    >[!Note]
    >Als uw camera niet goed werkt, kunt u de QR-code en de URL handmatig invoeren.

4. Controleer de **Accounts** pagina van de Microsoft Authenticator-app op uw apparaat, om te controleren of gegevens over uw account wordt rechts en er is een bijbehorende 6-cijferige verificatiecode.

    Voor extra beveiliging verandert de verificatiecode in die elke 30 seconden zo wordt voorkomen dat iemand met een code meerdere keren.

5. Selecteer **volgende** op de **twee factor authentication** pagina op uw PC en typ vervolgens de 6-cijferige verificatiecode beschikbaar in de app voor uw Facebook-account.

    Uw account is geverifieerd en u kunt de app nu gebruiken om uw account te verifiëren.

    >[!NOTE]
    >Voor meer informatie over tweeledige verificatie en uw Facebook-account, Zie [wat is verificatie met twee factoren en hoe werkt het?](https://www.facebook.com/help/148233965247823).

## <a name="add-your-github-account"></a>Uw GitHub-account toevoegen
Uw GitHub-account toevoegen door het inschakelen van tweeledige verificatie en het account vervolgens toe te voegen aan de app.

### <a name="turn-on-two-factor-verification"></a>Tweeledige verificatie inschakelen

1. Open op uw PC, GitHub, selecteer uw installatiekopie in de rechterbovenhoek en selecteer vervolgens **instellingen**.

    De **verificatie met twee factoren** pagina wordt weergegeven.

2. Selecteer **Security** uit de **persoonlijke instellingen** zijbalk en selecteer vervolgens **inschakelen tweeledige verificatie** uit de **verificatie met twee factoren**  gebied.

### <a name="add-your-github-account-to-the-app"></a>Uw GitHub-account toevoegen aan de app

1. Op de **verificatie met twee factoren** pagina op uw PC, selecteer **ingesteld met behulp van een app**.

2. De herstelcodes opslaan zodat u kunt teruggaan naar uw account als u de toegang kwijtraakt en selecteer vervolgens **volgende**. 

    U kunt uw codes opslaan door deze op uw apparaat worden gedownload, door papier af te drukken of door ze te kopiëren naar een wachtwoord manager-hulpprogramma.

3. Op de **verificatie met twee factoren** weergeeft, schakelt **ingesteld met behulp van een app**.

    De paginawijzigingen om u een QR-code weer te geven. Deze pagina niet sluiten.

4. Open de Microsoft Authenticator-app, selecteer **account toevoegen** uit de **aanpassen en controle** pictogram in de rechterbovenhoek, selecteer **andere account (Google, Facebook, enz.)** , en selecteer vervolgens **Voer deze code per SMS** uit de tekst boven aan de pagina.

    De Microsoft Authenticator-app is niet de QR-code scannen zijn, zodat u moet de code handmatig invoeren.

5. Voer een **accountnaam** (bijvoorbeeld GitHub) en typ de **geheime sleutel** uit stap 4 en selecteer vervolgens **voltooien**.

4. Op de **tweeledige authenticator** pagina op uw PC, typt u de verificatiecode van zes cijfers beschikbaar in de app voor uw GitHub-account en selecteer vervolgens **inschakelen**.

    De **Accounts** pagina van de app ziet u de accountnaam van uw en een 6-cijferige verificatiecode. Voor extra beveiliging verandert de verificatiecode in die elke 30 seconden zo wordt voorkomen dat iemand met een code meerdere keren.

    >[!NOTE]
    >Voor meer informatie over tweeledige verificatie en uw GitHub-account, Zie [over verificatie met twee factoren](https://help.github.com/articles/about-two-factor-authentication/).

## <a name="add-your-amazon-account"></a>De Amazon-account toevoegen
De Amazon-account toevoegen door het inschakelen van tweeledige verificatie en het account vervolgens toe te voegen aan de app.

### <a name="turn-on-two-factor-verification"></a>Tweeledige verificatie inschakelen

1. Open op uw PC, Amazon, selecteert u de **Account & lijsten** vervolgkeuzelijst en selecteer vervolgens **uw Account**.

2. Selecteer **aanmeldings- & beveiliging**, meld u aan bij uw Amazon-account en selecteer vervolgens **bewerken** in de **Advanced Security Settings** gebied.

    De **Advanced Security Settings** pagina wordt weergegeven.

3. Selecteer **aan de slag**.

4. Selecteer **Authenticator-App** uit de **kiezen hoe u ontvangt codes** pagina.

    De paginawijzigingen om u een QR-code weer te geven. Deze pagina niet sluiten.

5. Open de Microsoft Authenticator-app, selecteer **account toevoegen** uit de **aanpassen en controle** pictogram in de rechterbovenhoek, en selecteer vervolgens **andere account (Google, Facebook, enz.)** .

6. De camera van uw apparaat gebruiken om te scannen van de QR-code uit de **kiezen hoe u ontvangt codes** pagina op uw PC.

    >[!Note]
    >Als uw camera niet goed werkt, kunt u de QR-code en de URL handmatig invoeren.

5. Controleer de **Accounts** pagina van de Microsoft Authenticator-app op uw apparaat, om te controleren of gegevens over uw account wordt rechts en er is een bijbehorende 6-cijferige verificatiecode.

    Voor extra beveiliging verandert de verificatiecode in die elke 30 seconden zo wordt voorkomen dat iemand met een code meerdere keren.

6. Op de **kiezen hoe u ontvangt codes** pagina op uw PC, typt u de opgegeven in de app voor uw account Amazon 6-cijferige verificatiecode en selecteer vervolgens **code controleren en doorgaan**.

7. Voltooi de rest van het aanmeldingsproces, waaronder het toevoegen van een back-verificatiemethode, zoals een SMS-bericht, en selecteer vervolgens **code naartoe sturen**.

8. Op de **toevoegen van een back-verificatiemethode** pagina op uw PC, typ de 6-cijferige verificatiecode geleverd door uw back-verificatiemethode voor de Amazon-account en selecteer vervolgens **code controleren en doorgaan met**.

9. Op de Almost uitgevoerd pagina, Bepaal of u wilt maken van uw PC een vertrouwd apparaat en selecteer vervolgens **inderdaad. Verificatie in twee stappen inschakelen**.

    De **Advanced Security Settings** pagina wordt weergegeven, waarin de gegevens van uw bijgewerkte tweeledige verificatie.

    >[!NOTE]
    >Voor meer informatie over tweeledige verificatie en uw account Amazon, Zie [over verificatie in twee stappen](https://www.amazon.com/gp/help/customer/display.html?nodeId=201596330) en [aanmelden met verificatie in twee stappen](https://www.amazon.com/gp/help/customer/display.html?nodeId=201962440).


## <a name="next-steps"></a>Volgende stappen

- Nadat u uw accounts aan de app toevoegen, kunt u zich bij het gebruik van de Authenticator-app op uw apparaat. Zie voor meer informatie, [Meld u aan met behulp van de app](user-help-auth-app-sign-in.md).

- Voor apparaten met iOS, u kunt ook een back-up maken van de accountreferenties van uw en appinstellingen, zoals de volgorde van uw accounts, die betrekking hebben op de cloud. Zie voor meer informatie, [back-up en herstellen met de Microsoft Authenticator-app](user-help-auth-app-backup-recovery.md).
