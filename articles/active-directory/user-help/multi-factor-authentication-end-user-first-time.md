---
title: Verificatie - Azure Active Directory instellen | Microsoft Docs
description: Als uw bedrijf heeft Azure multi-factor Authentication geconfigureerd, wordt u gevraagd om u te registreren voor verificatie in twee stappen. Leer hoe u jenkins instelt.
services: active-directory
keywords: het gebruik van azure-directory, active directory in de cloud, active directory-zelfstudie
author: eross-msft
manager: mtillman
ms.reviewer: richagi
ms.assetid: 46f83a6a-dbdd-4375-8dc4-e7ea77c16357
ms.workload: identity
ms.service: active-directory
ms.component: user-help
ms.topic: conceptual
ms.date: 05/15/2017
ms.author: lizross
ms.openlocfilehash: 9f602b08e94071a335d0c88377912ea9b53f37b2
ms.sourcegitcommit: da3459aca32dcdbf6a63ae9186d2ad2ca2295893
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/07/2018
ms.locfileid: "51244437"
---
# <a name="set-up-my-account-for-two-step-verification"></a>Mijn account voor verificatie in twee stappen instellen
Verificatie in twee stappen is een extra beveiligingsstap waarmee de beveiliging van uw account door waardoor het moeilijker is voor anderen in breken. Als u dit artikel leest, hebt u waarschijnlijk een e-mailbericht van uw werk- of schoolaccount-beheerder over multi-factor Authentication. Of misschien heeft geprobeerd aan te melden bij en krijg een bericht waarin u wordt gevraagd voor het instellen van aanvullende beveiligingsverificatie. Als dat het geval is, **u niet aanmelden totdat het proces van automatische inschrijving is voltooid**.

Dit artikel helpt u bij het instellen van uw **werk- of schoolaccount**. Als u inschakelen van verificatie in twee stappen voor uw eigen persoonlijke Microsoft-account wilt, Zie [over verificatie in twee stappen](https://support.microsoft.com/help/12408/microsoft-account-about-two-step-verification).

## <a name="set-up-your-account"></a>Uw account instellen

Wanneer het ondersteuningsteam van uw bedrijf vereist dat u aan de slag met verificatie in twee stappen, ziet u een scherm waarin wordt gemeld **uw beheerder heeft ingesteld dat u dit account voor aanvullende beveiligingsverificatie instellen**:

![Instellen](./media/multi-factor-authentication-end-user-first-time/first.png)

Selecteer om te beginnen, **nu instellen.**

Als u een scherm zoals dit niet ziet wanneer u zich aanmeldt, volg de aanwijzingen in [uw instellingen beheren voor verificatie in twee stappen](multi-factor-authentication-end-user-manage-settings.md#where-to-find-the-settings-page) te vinden van de instellingenpagina waar u uw opties voor verificatie kunt beheren.

## <a name="decide-how-you-want-to-verify-your-sign-ins"></a>Bepaal hoe u wilt controleren of uw aanmeldingen

De eerste vraag in het inschrijvingsproces is hoe u moet er contact met u wilt. Bekijk de opties in de tabel en gebruik de koppelingen naar de installatiestappen uit voor elke methode.

| Neem contact op met de methode | Beschrijving |
| --- | --- |
| [Mobiele app](#use-a-mobile-app-as-the-contact-method) |- **Meldingen ontvangen voor verificatie.** Deze optie wordt een melding naar de authenticator-app op uw smartphone of tablet gepusht. Bekijk de melding en, als deze geldig is, selecteert u **verifiëren** in de app. Uw werk of school mogelijk dat u een PINCODE invoeren voordat u gaat verifiëren.<br>- **Verificatiecode gebruiken.** In deze modus genereert de authenticator-app een verificatiecode waarmee elke 30 seconden wordt bijgewerkt. Voer de meest recente verificatiecode in de interface voor aanmelden.<br>De Microsoft Authenticator-app is beschikbaar voor [Android](https://go.microsoft.com/fwlink/?linkid=866594), [iOS](https://go.microsoft.com/fwlink/?linkid=866594) en [Windows Phone](https://go.microsoft.com/fwlink/?Linkid=825071). |
| [Mobiele telefoon telefoongesprek of tekstbericht](#use-your-mobile-phone-as-the-contact-method) |- **Telefonische oproep** plaatst een geautomatiseerd telefoongesprek naar het telefoonnummer dat u opgeeft. Beantwoord de oproep en druk op # in het toetsenblok van de telefoon om te verifiëren.<br>- **SMS-bericht** een SMS-bericht met een verificatiecode wordt beëindigd. Na de prompt in de tekst, beantwoord het SMS-bericht of Voer de verificatiecode die is opgegeven in de interface van aanmelding. |
| [Office-telefoongesprek](#use-your-office-phone-as-the-contact-method) |Plaatst een geautomatiseerd telefoongesprek naar het telefoonnummer dat u opgeeft. Beantwoord het gesprek en drukt # in het toetsenblok van de telefoon om te verifiëren. |

## <a name="use-a-mobile-app-as-the-contact-method"></a>Een mobiele app gebruiken als de primaire contactmethode
Met deze methode is vereist dat u een verificator-app op uw telefoon of tablet installeren. De stappen in dit artikel zijn gebaseerd op de Microsoft Authenticator-app die beschikbaar is voor [Windows Phone](https://go.microsoft.com/fwlink/?Linkid=825071), [Android](https://go.microsoft.com/fwlink/?Linkid=825072), en [iOS](https://go.microsoft.com/fwlink/?Linkid=825073).

>[!NOTE]
>U hebt geen gebruik van de Microsoft Authenticator-app. Als u al een andere verificator-app gebruikt, kunt u blijven gebruiken.

1. Selecteer **mobiele app** uit de vervolgkeuzelijst.
2. Selecteer een **meldingen ontvangen voor verificatie** of **verificatiecode gebruiken**en selecteer vervolgens **instellen**.

   ![Scherm voor extra beveiliging-verificatie](./media/multi-factor-authentication-end-user-first-time/mobileapp.png)

3. Open de app op uw telefoon of tablet, en selecteer **+** een account toe te voegen. (Op Android-apparaten, selecteert u de drie puntjes vervolgens **account toevoegen**.)
4. Opgeven dat u een account voor werk- of schoolaccount toe te voegen. Hiermee opent u de QR-codescanner op uw telefoon. Als uw camera niet goed werkt is, kunt u handmatig invoeren van gegevens van uw bedrijf. Zie voor meer informatie, [handmatig toevoegen van een account](#add-an-account-manually).  
5. Scan de QR-code-afbeelding die werd weergegeven met het scherm voor het configureren van de mobiele app.  Selecteer **gedaan** te sluiten van het scherm QR-code.  

   ![QR-code scherm](./media/multi-factor-authentication-end-user-first-time/scan2.png)

6. Wanneer de activering is voltooid op de telefoon, selecteert u **Contact me**.  Deze stap wordt een melding of een verificatiecode naar uw telefoon verzonden. Selecteer **controleren**.  
7. Als een PINCODE is vereist voor het goedkeuren van aanmelding bij verificatie van uw bedrijf, voert u deze.

   ![Vak voor het invoeren van een PINCODE](./media/multi-factor-authentication-end-user-first-time/scan3.png)

8. Nadat de pincode is voltooid, schakelt u **sluiten**. Op dit moment is uw verificatie voltooid.
9. Het is raadzaam dat u uw mobiele telefoonnummer invoeren in het geval u geen toegang meer tot uw mobiele app. Geef uw land op in de vervolgkeuzelijst en voer uw mobiele telefoonnummer in het vak naast de naam van het land/regio. Selecteer **Volgende**.
10. Op dit moment wordt u gevraagd voor het instellen van app-wachtwoorden voor niet-browsertoepassingen zoals Outlook 2010 of ouder bent, of de systeemeigen e-mail-app voor Apple-apparaten. Dit is omdat sommige apps bieden geen ondersteuning voor verificatie in twee stappen. Als u deze apps niet gebruikt, klikt u op **gedaan** en de rest van de stappen overslaan.
11. Als u deze apps gebruikt, wordt het app-wachtwoord kopiëren opgegeven en plak deze in uw toepassing in plaats van uw normale wachtwoord. U kunt hetzelfde appwachtwoord voor meerdere apps gebruiken. Voor meer informatie [hulp bij app-wachtwoorden].
12. Klik op **Gereed**.

### <a name="add-an-account-manually"></a>Handmatig een account toevoegen
Volg deze stappen als u een account handmatig toevoegen aan de mobiele app wilt, in plaats van de QR-lezer.

1. Selecteer de **account handmatig invoeren** knop.  
2. Voer de code en de URL die beschikbaar zijn op de pagina die leest u de streepjescode. Deze informatie wordt de **Code** en **URL** vakken op de mobiele app.

    ![Instellen](./media/multi-factor-authentication-end-user-first-time/barcode2.png)
3. Wanneer de activering is voltooid, selecteert u **Contact me**. Deze stap wordt een melding of een verificatiecode naar uw telefoon verzonden. Selecteer **controleren**.

## <a name="use-your-mobile-phone-as-the-contact-method"></a>Uw mobiele telefoon gebruiken als de primaire contactmethode
1. Selecteer **telefoon voor authenticatie** uit de vervolgkeuzelijst.  

    ![Instellen](./media/multi-factor-authentication-end-user-first-time/phone.png)  
2. Kies uw land of regio in de vervolgkeuzelijst en voer uw mobiele telefoonnummer.
3. Selecteer de methode die u wilt gebruiken met uw mobiele telefoon: tekst of gesprek.
4. Selecteer **Contact me** om te controleren of uw telefoonnummer. Afhankelijk van de door u geselecteerde modus we sturen u een SMS-bericht of aanroep die u hebt. Volg de instructies op het scherm en selecteer vervolgens **controleren**.
5. Op dit moment wordt u gevraagd voor het instellen van app-wachtwoorden voor niet-browsertoepassingen zoals Outlook 2010 of ouder bent, of de systeemeigen e-mail-app voor Apple-apparaten. Dit is omdat sommige apps bieden geen ondersteuning voor verificatie in twee stappen. Als u deze apps niet gebruikt, klikt u op **gedaan** en de rest van de stappen overslaan.
6. Als u deze apps gebruikt, wordt het app-wachtwoord kopiëren opgegeven en plak deze in uw toepassing in plaats van uw normale wachtwoord. U kunt hetzelfde appwachtwoord voor meerdere apps gebruiken. Voor meer informatie [hulp bij app-wachtwoorden].
7. Klik op **Gereed**.

## <a name="use-your-office-phone-as-the-contact-method"></a>Uw zakelijke telefoon gebruiken als de primaire contactmethode
1. Selecteer **telefoon (werk)** in de vervolgkeuzelijst  

    ![Instellen](./media/multi-factor-authentication-end-user-first-time/office.png)  
2. Het vak telefoonnummer wordt automatisch gevuld met de contactgegevens van uw bedrijf. Als het nummer onjuist of ontbreekt is, vraag uw beheerder te wijzigen.
3. Selecteer **Contact me** om te controleren of uw telefoon-nummer en we het nummer van uw worden gebeld. Volg de instructies op het scherm en selecteer vervolgens **controleren**.
4. Op dit moment wordt u gevraagd voor het instellen van app-wachtwoorden voor niet-browsertoepassingen zoals Outlook 2010 of ouder bent, of de systeemeigen e-mail-app voor Apple-apparaten. Dit is omdat sommige apps bieden geen ondersteuning voor verificatie in twee stappen. Als u deze apps niet gebruikt, klikt u op **gedaan** en de rest van de stappen overslaan.
5. Als u deze apps gebruikt, wordt het app-wachtwoord kopiëren opgegeven en plak deze in uw toepassing in plaats van uw normale wachtwoord. U kunt hetzelfde appwachtwoord voor meerdere apps gebruiken. Zie voor meer informatie, [wat zijn App-wachtwoorden](multi-factor-authentication-end-user-app-passwords.md).
6. Klik op **Gereed**.

## <a name="next-steps"></a>Volgende stappen
* Wijzig de gewenste opties en [uw instellingen beheren voor verificatie in twee stappen](multi-factor-authentication-end-user-manage-settings.md)
* Instellen van [app-wachtwoorden](multi-factor-authentication-end-user-app-passwords.md) voor systeemeigen apparaat-apps die verificatie in twee stappen niet ondersteunen.
* Bekijk de [Microsoft Authenticator-app](microsoft-authenticator-app-how-to.md) voor een snelle, veilige authenticatie, zelfs als u geen service van de cel.
