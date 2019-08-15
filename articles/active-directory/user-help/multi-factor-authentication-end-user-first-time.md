---
title: Verificatie in twee stappen instellen-Azure Active Directory | Microsoft Docs
description: Wanneer uw bedrijf Azure multi-factor Authentication configureert, wordt u gevraagd om u aan te melden voor verificatie in twee stappen. Meer informatie over hoe u deze kunt instellen.
services: active-directory
keywords: Azure Directory gebruiken, Active Directory in de Cloud, zelf studie voor Active Directory
author: eross-msft
manager: daveba
ms.reviewer: richagi
ms.assetid: 46f83a6a-dbdd-4375-8dc4-e7ea77c16357
ms.workload: identity
ms.service: active-directory
ms.subservice: user-help
ms.topic: conceptual
ms.date: 05/15/2017
ms.author: lizross
ms.collection: M365-identity-device-management
ms.openlocfilehash: 75bc067bfe8a98ef2337f368243b3221be1677d6
ms.sourcegitcommit: 78ebf29ee6be84b415c558f43d34cbe1bcc0b38a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/12/2019
ms.locfileid: "68949897"
---
# <a name="set-up-my-account-for-two-step-verification"></a>Mijn account instellen voor verificatie in twee stappen
Verificatie in twee stappen is een extra beveiligings stap die uw account helpt beveiligen door het moeilijker te maken voor andere mensen. Als u dit artikel leest, hebt u waarschijnlijk een e-mail ontvangen van uw werk-of school beheerder over multi-factor Authentication. Of misschien hebt u geprobeerd zich aan te melden en een bericht te ontvangen waarin u wordt gevraagd om een extra beveiligings verificatie in te stellen. Als dat het geval is, **kunt u zich pas aanmelden nadat u het proces voor automatische inschrijving hebt voltooid**.

Dit artikel helpt u bij het instellen van uw **werk-of school account**. Als u verificatie in twee stappen wilt inschakelen voor uw eigen persoonlijke Microsoft-account, raadpleegt u [over verificatie in twee stappen](https://support.microsoft.com/help/12408/microsoft-account-about-two-step-verification).

## <a name="set-up-your-account"></a>Uw account instellen

Wanneer het ondersteunings team van uw bedrijf u moet gebruiken om verificatie in twee stappen te kunnen uitvoeren, ziet u een scherm dat aangeeft dat **uw beheerder heeft ingesteld dat u dit account moet instellen voor aanvullende beveiligings verificatie**:

![Instellen](./media/multi-factor-authentication-end-user-first-time/first.png)

Selecteer **nu instellen** om aan de slag te gaan.

Als u een scherm zoals dit niet ziet wanneer u zich aanmeldt, volgt u de instructies in [uw instellingen beheren voor verificatie in twee stappen](multi-factor-authentication-end-user-manage-settings.md#using-the-additional-security-verification-page) om de pagina instellingen te vinden waar u uw verificatie opties kunt beheren.

## <a name="decide-how-you-want-to-verify-your-sign-ins"></a>Bepalen hoe u aanmeldingen wilt verifiëren

Bij de eerste vraag in het inschrijvings proces kunt u contact met u opnemen. Bekijk de opties in de tabel en gebruik de koppelingen om naar de installatie stappen voor elke methode te gaan.

| Contactmethode | Description |
| --- | --- |
| [Mobiele app](#use-a-mobile-app-as-the-contact-method) |- **Meldingen ontvangen voor verificatie.** Met deze optie wordt een melding naar de verificator-app op uw smartphone of Tablet gepusht. Bekijk de melding en selecteer **verifiëren** in de app als deze geldig is. Uw werk of school vereist mogelijk dat u een pincode invoert voordat u zich verifieert.<br>- **Verificatie code gebruiken.** In deze modus genereert de verificator-app een verificatie code die elke 30 seconden wordt bijgewerkt. Voer de meest actuele verificatie code in de aanmeldings interface in.<br>De Microsoft Authenticator-app is beschikbaar voor [Android](https://go.microsoft.com/fwlink/?linkid=866594) en [iOS](https://go.microsoft.com/fwlink/?linkid=866594).|
| [Mobiele telefoon oproep of-tekst](#use-your-mobile-phone-as-the-contact-method) |- Met **telefoon oproep** wordt een geautomatiseerd telefoon gesprek met het door u geleverde nummer geplaatst. Vraag de oproep af en druk op # in het telefoon blok om te verifiëren.<br>- **SMS-bericht** eindigt een tekst bericht met een verificatie code. Na de prompt in de tekst antwoordt u op het tekst bericht of voert u de verificatie code in die u in de aanmeldings interface hebt opgegeven. |
| [Zakelijke telefoon oproep](#use-your-office-phone-as-the-contact-method) |Hiermee wordt een geautomatiseerd telefoon gesprek met het door u verstrekte nummer geplaatst. Beantwoord het gesprek en druk op # in het telefoon blok om te verifiëren. |

## <a name="use-a-mobile-app-as-the-contact-method"></a>Een mobiele app gebruiken als de contact methode
Voor het gebruik van deze methode moet u een verificator-app op uw telefoon of Tablet installeren. De stappen in dit artikel zijn gebaseerd op de Microsoft Authenticator-app, die beschikbaar is voor [Android](https://go.microsoft.com/fwlink/?Linkid=825072) en [IOS](https://go.microsoft.com/fwlink/?Linkid=825073).

>[!NOTE]
>U hoeft de Microsoft Authenticator-app niet te gebruiken. Als u al een andere verificator-app gebruikt, kunt u deze blijven gebruiken.

1. Selecteer **mobiele app** in de vervolg keuzelijst.
2. Selecteer **meldingen ontvangen voor verificatie** of **verificatie code gebruiken**en selecteer vervolgens **instellen**.

   ![Scherm aanvullende beveiligings verificatie](./media/multi-factor-authentication-end-user-first-time/mobileapp.png)

3. Open de app op uw telefoon of Tablet en selecteer **+** om een account toe te voegen. (Selecteer op Android-apparaten de drie punten en vervolgens **account toevoegen**.)
4. Geef aan dat u een werk-of school account wilt toevoegen. De QR-code scanner op uw telefoon wordt geopend. Als uw camera niet goed werkt, kunt u selecteren om uw bedrijfs gegevens hand matig in te voeren. Zie [een account hand matig toevoegen](#add-an-account-manually)voor meer informatie.  
5. Scan de foto van de QR-code die werd weer gegeven met het scherm voor het configureren van de mobiele app.  Selecteer **gereed** om het scherm QR-code te sluiten.  

   ![Scherm QR-code](./media/multi-factor-authentication-end-user-first-time/scan2.png)

6. Wanneer de activering op de telefoon is voltooid, selecteert u **contact met mij opnemen**.  Met deze stap wordt een melding of een verificatie code naar uw telefoon verzonden. Selecteer **verifiëren**.  
7. Als uw bedrijf een pincode vereist voor het goed keuren van de aanmeldings verificatie, voert u deze in.

   ![Vak voor het invoeren van een pincode](./media/multi-factor-authentication-end-user-first-time/scan3.png)

8. Nadat de pincode is opgegeven, selecteert u **sluiten**. Op dit moment moet uw verificatie slagen.
9. Het is raadzaam om uw mobiele telefoon nummer in te voeren voor het geval u geen toegang meer hebt tot uw mobiele app. Geef uw land/regio op in de vervolg keuzelijst en voer uw mobiele telefoon nummer in het vak naast de naam van het land/de regio in. Selecteer **Volgende**.
10. Op dit moment wordt u gevraagd om app-wacht woorden in te stellen voor niet-browser toepassingen, zoals Outlook 2010 of ouder, of de systeem eigen e-mail-app op Apple-apparaten. De reden hiervoor is dat sommige apps verificatie in twee stappen niet ondersteunen. Als u deze apps niet gebruikt, klikt u op **gereed** en slaat u de overige stappen over.
11. Als u deze apps gebruikt, kopieert u het opgegeven app-wacht woord en plakt u het in uw toepassing in plaats van uw normale wacht woord. U kunt hetzelfde app-wacht woord gebruiken voor meerdere apps. Voor meer informatie, [Help bij app-wacht woorden].
12. Klik op **Gereed**.

### <a name="add-an-account-manually"></a>Een account hand matig toevoegen
Als u een account hand matig wilt toevoegen aan de mobiele app, voert u de volgende stappen uit in plaats van de QR-lezer te gebruiken.

1. Selecteer de knop **account hand matig invoeren** .  
2. Voer de code en de URL in die zijn opgegeven op de pagina die u de streepjes code weergeeft. Deze informatie gaat in de velden **code** en **URL** in de mobiele app.

    ![Instellen](./media/multi-factor-authentication-end-user-first-time/barcode2.png)
3. Wanneer de activering is voltooid, selecteert u **contact opnemen**. Met deze stap wordt een melding of een verificatie code naar uw telefoon verzonden. Selecteer **verifiëren**.

## <a name="use-your-mobile-phone-as-the-contact-method"></a>Uw mobiele telefoon gebruiken als de contact methode
1. Selecteer **telefoon** voor authenticatie in de vervolg keuzelijst.  

    ![Instellen](./media/multi-factor-authentication-end-user-first-time/phone.png)  
2. Kies uw land/regio in de vervolg keuzelijst en voer uw mobiele telefoon nummer in.
3. Selecteer de methode die u wilt gebruiken met uw mobiele telefoon-tekst of telefoon gesprek.
4. Selecteer **contact opnemen** om uw telefoon nummer te verifiëren. Afhankelijk van de modus die u hebt geselecteerd, sturen we u een tekst of bellen u. Volg de instructies die worden weer gegeven op het scherm en selecteer vervolgens **verifiëren**.
5. Op dit moment wordt u gevraagd om app-wacht woorden in te stellen voor niet-browser toepassingen, zoals Outlook 2010 of ouder, of de systeem eigen e-mail-app op Apple-apparaten. De reden hiervoor is dat sommige apps verificatie in twee stappen niet ondersteunen. Als u deze apps niet gebruikt, klikt u op **gereed** en slaat u de overige stappen over.
6. Als u deze apps gebruikt, kopieert u het opgegeven app-wacht woord en plakt u het in uw toepassing in plaats van uw normale wacht woord. U kunt hetzelfde app-wacht woord gebruiken voor meerdere apps. Voor meer informatie, [Help bij app-wacht woorden].
7. Klik op **Gereed**.

## <a name="use-your-office-phone-as-the-contact-method"></a>Uw zakelijke telefoon gebruiken als de contact methode
1. Selecteer **Office Phone** in de vervolg keuzelijst  

    ![Instellen](./media/multi-factor-authentication-end-user-first-time/office.png)  
2. Het vak telefoon nummer wordt automatisch ingevuld met de contact gegevens van uw bedrijf. Als het nummer onjuist is of ontbreekt, vraagt u uw beheerder om wijzigingen aan te brengen.
3. Selecteer **contact opnemen** om uw telefoon nummer te verifiëren. uw nummer wordt dan gebeld. Volg de instructies die worden weer gegeven op het scherm en selecteer vervolgens **verifiëren**.
4. Op dit moment wordt u gevraagd om app-wacht woorden in te stellen voor niet-browser toepassingen, zoals Outlook 2010 of ouder, of de systeem eigen e-mail-app op Apple-apparaten. De reden hiervoor is dat sommige apps verificatie in twee stappen niet ondersteunen. Als u deze apps niet gebruikt, klikt u op **gereed** en slaat u de overige stappen over.
5. Als u deze apps gebruikt, kopieert u het opgegeven app-wacht woord en plakt u het in uw toepassing in plaats van uw normale wacht woord. U kunt hetzelfde app-wacht woord gebruiken voor meerdere apps. Zie [Wat zijn app-wacht woorden](multi-factor-authentication-end-user-app-passwords.md)? voor meer informatie.
6. Klik op **Gereed**.

## <a name="next-steps"></a>Volgende stappen
* Uw voorkeurs opties wijzigen en [uw instellingen beheren voor verificatie in twee stappen](multi-factor-authentication-end-user-manage-settings.md)
* Stel [app-wacht woorden](multi-factor-authentication-end-user-app-passwords.md) in voor apps van systeem eigen apparaten die geen verificatie in twee stappen ondersteunen.
* Bekijk de [Microsoft Authenticator-app](user-help-auth-app-download-install.md) voor snelle, veilige verificatie, zelfs als u niet beschikt over de service van de cel.
