---
title: Microsoft Authenticator aanmelden via telefoon - Azure en Microsoft-account | Microsoft Docs
description: Gebruik uw telefoon aanmelden bij uw Microsoft-account in plaats van uw wachtwoord te typen. In dit artikel vindt u antwoorden op veelgestelde vragen over deze functie.
services: multi-factor-authentication
documentationcenter: ''
author: eross-msft
manager: mtillman
ms.assetid: ''
ms.service: multi-factor-authentication
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 08/12/2017
ms.author: lizross
ms.reviewer: librown
ms.custom: end-user
ms.openlocfilehash: 589309c956ebbbebe6c423cfcca117a86c796be3
ms.sourcegitcommit: 7208bfe8878f83d5ec92e54e2f1222ffd41bf931
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/14/2018
ms.locfileid: "39060115"
---
# <a name="sign-in-with-your-phone-not-your-password"></a>Meld u aan met uw telefoon, niet uw wachtwoord

De Microsoft Authenticator-app kunt u uw accounts beveiligen door verificatie in twee stappen uitvoeren nadat u uw wachtwoord hebt ingevoerd. Maar wist u dat deze het wachtwoord voor uw persoonlijke Microsoft-account volledig kunt vervangen?

Deze functie is beschikbaar op iOS en Android-apparaten, en werkt met persoonlijke Microsoft-accounts.

## <a name="how-it-works"></a>Hoe werkt het?

Veel van u de Microsoft Authenticator-app wordt gebruikt voor verificatie in twee stappen wanneer u zich aanmeldt bij uw Microsoft-account. U typt u het wachtwoord en vervolgens gaat u naar de app een melding goed te keuren of ophalen van een verificatiecode. Met aanmelding via telefoon, die u kunt het wachtwoord overslaan en al uw identiteit te verifiëren op uw telefoon te doen. Omdat aanmelding via telefoon een type verificatie in twee stappen is, moet u nog steeds een ding die u kent en een ding hebt om uw identiteit te verifiëren. De telefoon is nog steeds het wat dat u hebt en de PINCODE of biometrische sleutel van uw telefoon is het dat u weet.

## <a name="how-to-get-started"></a>Hoe gaat u aan de slag

Als u wilt aanmelden bij uw persoonlijke Microsoft-account met uw telefoon, de volgende stappen uit:

1. Schakel aanmelden via telefoon voor uw account.

  - Als er geen de Microsoft Authenticator-app nog, installeren en toevoegen van uw persoonlijke Microsoft-account op basis van de stappen op de [Microsoft Authenticator pagina](microsoft-authenticator-app-how-to.md). Toegevoegde accounts zijn automatisch ingeschakeld, zodat u aan de slag.

  - Als u Microsoft Authenticator al voor verificatie in twee stappen gebruikt, selecteert u uw account vanaf de startpagina van de app en selecteer **aanmelden via telefoon inschakelen** uit de vervolgkeuzelijst.

  >[!NOTE]
  >Ter bescherming van uw account, moet een PINCODE of biometrische vergrendeling op uw apparaat. Als u uw telefoon ontgrendelen wilt houden, verschijnt de app een aanvraag waarin u wordt gevraagd voor het instellen van een vergrendeling voordat de aanmelding via telefoon inschakelen.

3. De meeste pagina's waar normaal gesproken u het wachtwoord van uw Microsoft-account voert hebben een koppeling met de melding dat **gebruik van een app in plaats daarvan**. Selecteer deze koppeling naar aanmelden met uw telefoon.

4. Microsoft stuurt een melding naar uw telefoon. De melding om aan te melden bij uw account goed te keuren.   

## <a name="faq"></a>Veelgestelde vragen

### <a name="how-is-signing-in-with-my-phone-more-secure-than-typing-a-password"></a>Hoe is het aanmelden met mijn telefoon veiliger dan een wachtwoord te typen?  

De meeste mensen zich vandaag nog voor web sites of apps met behulp van een gebruikersnaam en wachtwoord.  Helaas zijn wachtwoorden vaak verloren, gestolen of geraden door hackers. Bij het instellen van de Microsoft Authenticator-app aan te melden bij genereren we een sleutel op uw telefoon die u kunt uw account ontgrendelen. We deze sleutel met de PINCODE of biometrische dat u al op uw telefoon gebruikt te beschermen.  Wanneer u zich met uw telefoon aanmeldt, wordt deze sleutel wordt gebruikt om te bewijzen dat je identiteit veilig met twee factoren – de telefoon zelf, en de mogelijkheid om te ontgrendelen. 

De sleutel die wordt gebruikt, is vergelijkbaar met de sleutels die worden gebruikt in Windows Hello en de FIDO Alliance UAF-specificaties. Uw biografie van gegevens alleen is wordt gebruikt voor het beveiligen van de sleutel lokaal, en nooit verzonden naar of opgeslagen in de cloud. 
 
### <a name="where-can-i-use-my-phone-to-replace-my-password-and-where-would-i-still-need-the-password"></a>Waar kan ik mijn telefoon gebruiken voor het vervangen van mijn wachtwoord, en waar ik nog steeds moeten het wachtwoord?  

Vandaag de dag werkt de telefoonfunctie aanmelden alleen met web-apps en services die worden aangestuurd door een persoonlijke Microsoft-accounts, iOS of Android-apps die gebruikmaken van een persoonlijk Microsoft-account en apps op Windows 10 die gebruikmaken van een persoonlijk Microsoft-account. Wanneer u zich bij een van deze websites of toepassingen aanmelden, op de pagina waar u meestal Voer uw wachtwoord er is een koppeling met de melding dat **gebruik van een app in plaats daarvan**. 

Aanmelding via telefoon kan niet worden gebruikt voor het ontgrendelen van een Windows-PC, XBOX of bureaublad versies van Microsoft-apps zoals Office-apps op dit moment.
 
### <a name="does-this-replace-two-step-verification-should-i-turn-it-off"></a>Vervangt deze verificatie in twee stappen? Moet ik uitschakelen?   

Soms. Er wordt gewerkt aan het uitbreiden van het bereik van de aanmelding via telefoon, maar voor nu zijn er nog steeds plaatsen in het ecosysteem van Microsoft die deze niet ondersteunen. In deze locaties we verificatie in twee stappen nog steeds gebruiken voor beveiligde aanmelding. Om die reden Nee, mag niet u uitschakelen verificatie in twee stappen voor uw account.
 
### <a name="okay-if-i-keep-two-step-verification-turned-on-for-my-account-do-i-have-to-approve-two-notifications"></a>Oké, als ik verificatie in twee stappen ingeschakeld voor mijn account wilt houden, heb ik om goed te keuren twee meldingen?

Nee, u niet. Aanmelden bij uw Microsoft-account met uw telefoon telt als verificatie in twee stappen. In plaats van te typen in uw wachtwoord, en een melding goed te keuren u uw identiteit bewijst door te weten hoe u om uw telefoon te ontgrendelen en vervolgens een melding goed te keuren. Er wordt niet sturen u een tweede melding goed te keuren.

### <a name="what-if-i-lose-my-phone-or-dont-have-it-with-me-how-can-i-access-my-account"></a>Wat gebeurt er als ik mijn telefoon verlies of deze nog niet met mij, hoe krijg ik toegang tot mijn account?  

U kunt altijd klikken op **in plaats daarvan gebruik van een wachtwoord** op de aanmeldingspagina wilt terugkeren naar je wachtwoord gebruikt. Houd er rekening mee dat als u verificatie in twee stappen hebt gebruikt, moet u toch een tweede methode om te controleren of de aanmelding. Daarom ten zeerste aangeraden om ervoor te zorgen dat u extra, up-to-date beveiligingsgegevens voor je account hebt. U kunt uw beveiligingsgegevens op beheren https://account.live.com/proofs/manage.
 
### <a name="how-do-i-stop-using-this-feature-and-go-back-to-entering-my-password"></a>Hoe kan ik stoppen met het gebruik van deze functie en gaat u terug naar mijn wachtwoord invoeren?

Klik op **in plaats daarvan gebruik van een wachtwoord** wanneer u zich aanmeldt. We de meest recente keuze onthouden en bieden die standaard de volgende keer dat u zich aanmeldt. Als u ooit wilt terugkeren naar het aanmelden met uw telefoon, klikt u op **gebruik van een app in plaats daarvan**. 
 
### <a name="can-i-use-the-app-to-sign-in-to-all-my-accounts-with-microsoft"></a>Kan ik de app gebruiken voor aanmelding bij alle accounts met Microsoft?   
Deze functionaliteit is op dit moment alleen beschikbaar voor persoonlijke Microsoft-accounts. 
 
### <a name="can-i-sign-into-my-pc-with-my-phone"></a>Kan ik mij op mijn PC met mijn telefoon?  
Voor uw PC, wordt u aangeraden zich aanmeldt met Windows Hello op Windows 10 uw face, vingerafdruk of een PINCODE.   
 
### <a name="can-i-sign-in-with-my-windows-phone"></a>Kan ik me aanmelden met mijn Windows Phone?  
Op dit moment zijn we deze functionaliteit niet ontwikkelen voor de Microsoft Authenticator op Windows Phone. 

## <a name="next-steps"></a>Volgende stappen
Als u de Microsoft Authenticator-app nog niet hebt gedownload, bekijk het nu. De app is beschikbaar voor [Windows Phone](http://go.microsoft.com/fwlink/?Linkid=825071), en is beschikbaar op de Microsoft Authenticator-app voor aanmelding via telefoon [Android](http://go.microsoft.com/fwlink/?Linkid=825072) en [iOS](http://go.microsoft.com/fwlink/?Linkid=825073).

Als u vragen over de app in het algemeen hebt, bekijk de [Veelgestelde vragen over de Microsoft Authenticator](microsoft-authenticator-app-faq.md)
