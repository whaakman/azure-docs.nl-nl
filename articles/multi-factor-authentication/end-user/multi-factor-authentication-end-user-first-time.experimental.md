---
title: Verificatie in twee stappen voor mijn werk of school-account instellen | Microsoft Docs
description: 'Wanneer uw bedrijf Azure multi-factor Authentication configureert, wordt u gevraagd om aan te melden voor verificatie in twee stappen. Informatie over hoe u instelt. '
services: multi-factor-authentication
keywords: het gebruik van azure-directory, active directory in de cloud, active directory-zelfstudie
documentationcenter: 
author: kgremban
manager: femila
editor: pblachar
ms.assetid: 46f83a6a-dbdd-4375-8dc4-e7ea77c16357
ms.service: multi-factor-authentication
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/15/2017
ms.author: kgremban
ms.custom: end-user
ms.openlocfilehash: 3499a7b23a7b4bdd44b5d9e8d7e750681efc3825
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/29/2017
---
# <a name="set-up-my-account-for-two-step-verification"></a>Mijn account voor verificatie in twee stappen instellen
Verificatie in twee stappen is een extra beveiligingsstap die voorkomen dat uw account door waardoor het moeilijker voor andere mensen om in te verbreken. Als u dit artikel leest ontvangen u waarschijnlijk een e-mailbericht van uw werk of school-beheerder over meervoudige verificatie. Of misschien probeert aan te melden en krijg een bericht waarin u wordt gevraagd voor het instellen van aanvullende beveiligingsverificatie. Als dat het geval is, **je niet aanmelden totdat u het proces voor automatische inschrijving hebt voltooid**.

Dit artikel helpt u bij het instellen van uw **werk- of schoolaccount**. Als u inschakelen, verificatie in twee stappen voor uw eigen persoonlijke Microsoft-account wilt, Zie [over verificatie in twee stappen](https://support.microsoft.com/help/12408/microsoft-account-about-two-step-verification).

## <a name="set-up-your-account"></a>Instellen van uw account

Als uw IT-afdeling vereist dat u aan de slag met verificatie in twee stappen, een scherm de melding **uw beheerder heeft ingesteld dat u dit account voor aanvullende beveiligingsverificatie ingesteld** wordt weergegeven:

![Instellen](./media/multi-factor-authentication-end-user-first-time/first.png)

Selecteer om te beginnen **het nu instellen.**

Als u een scherm als dit niet ziet wanneer u zich aanmeldt, volg de aanwijzingen in [beheren van uw instellingen voor verificatie in twee stappen](multi-factor-authentication-end-user-manage-settings.md#where-to-find-the-settings-page) vinden van de instellingenpagina waar u uw opties voor verificatie kunt beheren. 

## <a name="decide-how-you-want-to-verify-your-sign-ins"></a>Bepaal hoe u wilt controleren of uw aanmeldingen

De eerste vraag in het registratieproces is hoe u contact met u wilt. Bekijk de opties in de tabel en gebruik de koppelingen naar de installatiestappen voor elke methode.

| Contactmethode | Beschrijving |
| --- | --- |
| [Mobiele app](#use-a-mobile-app-as-the-contact-method) |- **Meldingen voor verificatie ontvangen.** Deze optie wordt een melding naar de verificator-app op uw smartphone of tablet. Bekijk de melding en als deze geldig is, selecteert u **verifiëren** in de app. Uw werk of school verlangen dat u een PINCODE invoeren voordat u gaat verifiëren.<br>- **Verificatiecode gebruiken.** In deze modus genereert de verificator-app een bevestigingscode die updates van elke 30 seconden. Voer de meest recente verificatiecode in de interface voor aanmelden.<br>De Microsoft Authenticator-app is beschikbaar voor [Windows Phone](http://go.microsoft.com/fwlink/?Linkid=825071), [Android](http://go.microsoft.com/fwlink/?Linkid=825072), en [IOS](http://go.microsoft.com/fwlink/?Linkid=825073). |
| [Mobiele telefoon telefoongesprek of tekstbericht](#use-your-mobile-phone-as-the-contact-method) |- **Telefoonoproep** plaatst een geautomatiseerd telefoongesprek, het telefoonnummer dat u opgeeft. Beantwoord de oproep en druk op # in de toetsenblok van de telefoon om te verifiëren.<br>- **SMS-bericht** eindigt een SMS-bericht met een verificatiecode. Na de prompt in de tekst, beantwoord het SMS-bericht of Voer de verificatiecode die is opgegeven in de interface voor aanmelden. |
| [Office-telefoongesprek](#use-your-office-phone-as-the-contact-method) |Een geautomatiseerd telefoongesprek, het telefoonnummer dat u opgeeft, wordt geplaatst. Beantwoord het gesprek en drukt # in op de toetsenblok van de telefoon om te verifiëren. |

## <a name="use-a-mobile-app-as-the-contact-method"></a>Een mobiele app gebruiken als primaire contactmethode
Met deze methode is vereist dat u een verificator-app op uw telefoon of tablet installeren. De stappen in dit artikel zijn gebaseerd op de Microsoft Authenticator-app, die beschikbaar is voor [Windows Phone](http://go.microsoft.com/fwlink/?Linkid=825071), [Android](http://go.microsoft.com/fwlink/?Linkid=825072), en [IOS](http://go.microsoft.com/fwlink/?Linkid=825073).

1. Selecteer **mobiele app** uit de vervolgkeuzelijst.
2. Selecteer een **ontvangen van meldingen voor verificatie** of **verificatiecode gebruiken**, selecteer daarna **instellen**.

   ![Extra beveiliging verificatie scherm](./media/multi-factor-authentication-end-user-first-time/mobileapp.png)

3. Open de app op uw telefoon of tablet, en selecteer  **+**  een account toe te voegen. (Op Android-apparaten, selecteert u de drie puntjes Klik **account toevoegen**.)
4. Geef op dat u wilt een werk- of schoolaccount toevoegen. De scanner QR-code op uw telefoon wordt geopend. Als uw camera niet goed werkt is, kunt u de gegevens van uw bedrijf handmatig invoeren. Zie voor meer informatie [handmatig toevoegen van een account](#add-an-account-manually).  
5. Scan de QR-code-afbeelding die werden weergegeven met het scherm voor het configureren van de mobiele app.  Selecteer **gedaan** te sluiten van het scherm QR-code.  

   ![QR-code scherm](./media/multi-factor-authentication-end-user-first-time/scan2.png)

6. Wanneer de activering is voltooid op de telefoon, selecteert u **Contact met mij opnemen**.  Deze stap verzendt geen melding of een verificatiecode naar uw telefoon. Selecteer **controleren**.  
7. Als uw bedrijf een PINCODE voor het goedkeuren van aanmelden verificatie vereist, voert u.

   ![Vak voor het invoeren van een PINCODE](./media/multi-factor-authentication-end-user-first-time/scan3.png)

8. Nadat de pincode is voltooid, selecteert u **sluiten**. Op dit moment is uw verificatie voltooid.
9. Het is raadzaam dat u uw mobiele telefoonnummer invoeren als u geen toegang meer tot uw mobiele app. Geef uw land op in de vervolgkeuzelijst en voer uw mobiele telefoonnummer in het vak naast de landnaam. Selecteer **volgende**.
10. Op dit moment wordt u gevraagd voor het instellen van app-wachtwoorden voor niet-browsertoepassingen zoals Outlook 2010 of ouder, of de systeemeigen e-mail-app op apparaten van Apple. Deze actie is omdat sommige apps bieden geen ondersteuning voor verificatie in twee stappen. Als u deze apps niet gebruikt, klikt u op **gedaan** en de rest van deze stappen overslaan.
11. Als u deze apps gebruikt, wordt het app-wachtwoord worden gekopieerd opgegeven en plak deze in uw toepassing in plaats van uw normale wachtwoord. U kunt hetzelfde appwachtwoord voor meerdere apps gebruiken. Voor meer informatie [help met app-wachtwoorden].
12. Klik op **Gereed**.

### <a name="add-an-account-manually"></a>Handmatig toevoegen van een account
Ga als volgt te werk als u een account handmatig toevoegen aan de mobiele app wilt, in plaats van de QR-lezer.

1. Selecteer de **account handmatig invoeren** knop.  
2. Voer de code en de URL die beschikbaar zijn op dezelfde pagina waarin u de streepjescode. Deze informatie wordt de **Code** en **URL** vakjes op de mobiele app.

    ![Instellen](./media/multi-factor-authentication-end-user-first-time/barcode2.png)
3. Wanneer de activering is voltooid, selecteert u **Contact met mij opnemen**. Deze stap verzendt geen melding of een verificatiecode naar uw telefoon. Selecteer **controleren**.

## <a name="use-your-mobile-phone-as-the-contact-method"></a>Uw mobiele telefoon gebruiken als primaire contactmethode
1. Selecteer **telefoon voor authenticatie** uit de vervolgkeuzelijst.  

    ![Instellen](./media/multi-factor-authentication-end-user-first-time/phone.png)  
2. Kies uw land in de vervolgkeuzelijst en voer uw mobiele telefoonnummer.
3. Selecteer de methode die u gebruiken wilt voor gebruik met uw mobiele telefoon - tekstbericht of telefoongesprek.
4. Selecteer **Contact met mij opnemen** om te controleren of uw telefoonnummer. Afhankelijk van de door u geselecteerde modus we sturen je een tekst of u bellen. Volg de instructies op het scherm en selecteer vervolgens **controleren**.
5. Op dit moment wordt u gevraagd voor het instellen van app-wachtwoorden voor niet-browsertoepassingen zoals Outlook 2010 of ouder, of de systeemeigen e-mail-app op apparaten van Apple. Deze actie is omdat sommige apps bieden geen ondersteuning voor verificatie in twee stappen. Als u deze apps niet gebruikt, klikt u op **gedaan** en de rest van de stappen overslaan.
6. Als u deze apps gebruikt, wordt het app-wachtwoord worden gekopieerd opgegeven en plak deze in uw toepassing in plaats van uw normale wachtwoord. U kunt hetzelfde appwachtwoord voor meerdere apps gebruiken. Voor meer informatie [help met app-wachtwoorden].
7. Klik op **Gereed**.

## <a name="use-your-office-phone-as-the-contact-method"></a>Uw telefoon (werk) gebruiken als primaire contactmethode
1. Selecteer **telefoon (werk)** in de vervolgkeuzelijst  

    ![Instellen](./media/multi-factor-authentication-end-user-first-time/office.png)  
2. Het vak telefoonnummer wordt automatisch gevuld met de contactgegevens van uw bedrijf. Als het nummer onjuist of ontbreekt is, vraag uw beheerder om wijzigingen te brengen.
3. Selecteer **Contact met mij opnemen** om te controleren of je telefoon nummer, en wij belt het nummer van uw. Volg de instructies op het scherm en selecteer vervolgens **controleren**.
4. Op dit moment wordt u gevraagd voor het instellen van app-wachtwoorden voor niet-browsertoepassingen zoals Outlook 2010 of ouder, of de systeemeigen e-mail-app op apparaten van Apple. Deze actie is omdat sommige apps bieden geen ondersteuning voor verificatie in twee stappen. Als u deze apps niet gebruikt, klikt u op **gedaan** en de rest van de stappen overslaan.
5. Als u deze apps gebruikt, wordt het app-wachtwoord worden gekopieerd opgegeven en plak deze in uw toepassing in plaats van uw normale wachtwoord. U kunt hetzelfde appwachtwoord voor meerdere apps gebruiken. Zie voor meer informatie [wat zijn App-wachtwoorden](multi-factor-authentication-end-user-app-passwords.md).
6. Klik op **Gereed**.

## <a name="next-steps"></a>Volgende stappen
* Wijzig de gewenste opties en [beheren van uw instellingen voor verificatie in twee stappen](multi-factor-authentication-end-user-manage-settings.md)
* Instellen van [app-wachtwoorden](multi-factor-authentication-end-user-app-passwords.md) voor systeemeigen apparaat-apps die verificatie in twee stappen niet ondersteunen.
* Bekijk de [Microsoft Authenticator-app](microsoft-authenticator-app-how-to.md) veilige authenticatie voor een snelle, zelfs wanneer er geen service van de cel.

