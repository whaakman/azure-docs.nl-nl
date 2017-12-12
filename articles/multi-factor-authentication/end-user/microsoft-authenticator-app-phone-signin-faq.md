---
title: Microsoft Authenticator phone aanmelden - Azure en Microsoft-accounts | Microsoft Docs
description: Gebruik uw telefoon aan te melden bij je Microsoft-account in plaats van uw wachtwoord te typen. Dit artikel worden veelgestelde vragen beantwoord over deze functie.
services: multi-factor-authentication
documentationcenter: 
author: barlanmsft
manager: mtillman
ms.assetid: 
ms.service: multi-factor-authentication
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/12/2017
ms.author: barlan
ms.reviewer: librown
ms.custom: end-user
ms.openlocfilehash: 86cf6ba2f2ec86a609b4ec60cc143b07d9e542b3
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/11/2017
---
# <a name="sign-in-with-your-phone-not-your-password"></a>Meld u aan met uw telefoon niet uw wachtwoord

De Microsoft Authenticator-app kunt u uw accounts beveiligen door verificatie in twee stappen uitvoert nadat u uw wachtwoord invoert. Maar wist u dat deze het wachtwoord voor je persoonlijke Microsoft-account volledig kunt vervangen?

Deze functie is beschikbaar op iOS en Android-apparaten en werkt met persoonlijke Microsoft-accounts.

## <a name="how-it-works"></a>Hoe werkt het?

Veel van u de Microsoft Authenticator-app voor verificatie in twee stappen wanneer u zich aanmeldt bij je Microsoft-account gebruiken. U Typ uw wachtwoord vervolgens gaat u naar de app een melding goedkeuren of ophalen van een verificatiecode. Met phone aanmelden, het wachtwoord overslaat en alle ID-verificatie uitvoeren op uw telefoon. Omdat het is een soort verificatie in twee stappen phone aanmelden, moet u nog steeds een ding dat u weet en een ding dat u hebt om uw identiteit te verifiëren. De telefoon is nog steeds het wat dat u hebt en de PINCODE of biometrische sleutel van uw telefoon is het u weet wat.

## <a name="how-to-get-started"></a>Hoe gaat u aan de slag

Als u wilt aanmelden bij je persoonlijke Microsoft-account met uw telefoon, de volgende stappen uit:

1. Telefoon-aanmeldingspagina voor uw account inschakelen.

  - Als er geen Microsoft Authenticator-app nog, installeren en toevoegen van uw persoonlijke Microsoft-account volgens de stappen op de [Microsoft Authenticator pagina](microsoft-authenticator-app-how-to.md). Toegevoegde accounts worden automatisch ingeschakeld, zodat u bent klaar om te beginnen.

  - Als u Microsoft Authenticator al voor verificatie in twee stappen gebruikt, selecteert u uw account vanaf de startpagina van de app en selecteer **inschakelen aanmeldingspagina phone** uit de vervolgkeuzelijst.

  >[!NOTE]
  >Ter beveiliging van uw account moet een PINCODE of biometrische vergrendeling op uw apparaat. Als u uw telefoon ontgrendeld, verschijnt de app wordt een aanvraag waarin u wordt gevraagd een vergrendeling instellen voordat u de aanmeldingspagina phone inschakelt.

3. De meeste pagina's waar normaal u het wachtwoord voor je Microsoft-account voert hebben een koppeling met de tekst **gebruik van een app in plaats daarvan**. Selecteer deze koppeling kunnen aanmelden met uw telefoon.

4. Microsoft stuurt een melding naar uw telefoon. De melding aan te melden bij uw account goedkeuren.   

## <a name="faq"></a>Veelgestelde vragen

### <a name="how-is-signing-in-with-my-phone-more-secure-than-typing-a-password"></a>Hoe wordt aanmelden met mijn telefoon veiliger dan een wachtwoord te typen?  

Vandaag de meeste mensen aanmelden bij websites of toepassingen met een gebruikersnaam en wachtwoord.  Helaas zijn wachtwoorden vaak verloren, gestolen of geraden door hackers. Bij het instellen van de Microsoft Authenticator-app aanmelden, moet er een sleutel op uw telefoon die u kunt uw account ontgrendelen gegenereerd. We deze sleutel met de PINCODE of biometrische dat u al op uw telefoon gebruikt te beschermen.  Wanneer u zich met uw telefoon aanmeldt, wordt deze sleutel wordt gebruikt om uw identiteit veilig met twee factoren – de telefoon zelf en de mogelijkheid om te ontgrendelen te bewijzen. 

De sleutel die wordt gebruikt, is vergelijkbaar met de sleutels die worden gebruikt in Windows Hello en de FIDO Alliance UAF-specificaties. Uw biografie gegevens alleen wordt wordt gebruikt voor het beveiligen van de sleutel lokaal, en nooit verzonden naar of opgeslagen in de cloud. 
 
### <a name="where-can-i-use-my-phone-to-replace-my-password-and-where-would-i-still-need-the-password"></a>Waar kan ik mijn telefoon gebruiken ter vervanging van mijn wachtwoord en waar moet ik de nog steeds het wachtwoord?  

Vandaag de dag werkt de telefoonfunctie aanmelden alleen met web-apps en services die door de persoonlijke Microsoft-accounts, iOS of Android-apps die gebruikmaken van een persoonlijk Microsoft-account en op Windows 10-apps die gebruikmaken van een persoonlijk Microsoft-account is ingeschakeld. Wanneer u zich aanmeldt bij een van deze websites of toepassingen, op de pagina waar u meestal Voer uw wachtwoord wordt een koppeling met de tekst **gebruik van een app in plaats daarvan**. 

Aanmelden van telefoon kan niet worden gebruikt voor het ontgrendelen van een Windows-PC, XBOX of bureaublad versies van Microsoft-apps, zoals Office-apps op dit moment.
 
### <a name="does-this-replace-two-step-verification-should-i-turn-it-off"></a>Vervangt deze verificatie in twee stappen? Moet ik uitschakelen?   

Soms. We proberen uit te breiden het bereik van de aanmeldingspagina telefoon, maar nu er nog steeds plaatsen in het Microsoft-ecosysteem die niet ondersteund. In deze locaties we verificatie in twee stappen nog steeds gebruiken voor veilige aanmelden. Daarom moet Nee, niet moet u uitschakelen verificatie in twee stappen voor uw account.
 
### <a name="okay-if-i-keep-two-step-verification-turned-on-for-my-account-do-i-have-to-approve-two-notifications"></a>Goed, als ik verificatie in twee stappen ingeschakeld voor mijn account houden, heb ik twee meldingen goedkeuren?

U niet Nee, het volgende geval. Aanmelden bij je Microsoft-account met uw telefoon telt als verificatie in twee stappen. In plaats van te typen in het wachtwoord, en vervolgens een melding goedkeuren u uw identiteit bewijzen door weten hoe u om uw telefoon te ontgrendelen en vervolgens goedkeuren van een melding. We won't ontvangt u een tweede bericht goed te keuren.

### <a name="what-if-i-lose-my-phone-or-dont-have-it-with-me-how-can-i-access-my-account"></a>Wat gebeurt er als ik mijn telefoon verlies of met me niet hebt, hoe kan ik toegang tot mijn account?  

U kunt altijd klikken op **gebruik in plaats daarvan een wachtwoord** op de aanmeldingspagina overschakelen naar je wachtwoord. Houd er rekening mee dat als u verificatie in twee stappen hebt gebruikt, moet u toch een tweede methode om te controleren of uw aanmelden. Daarom wordt ten zeerste aangeraden om ervoor te zorgen dat er extra, up-to-date beveiligingsgegevens voor je account. U kunt de beveiligingsgegevens voor je op https://account.live.com/proofs/manage beheren.
 
### <a name="how-do-i-stop-using-this-feature-and-go-back-to-entering-my-password"></a>Hoe stoppen met het gebruik van deze functie en ik ga terug naar mijn wachtwoord invoeren?

Klik op **gebruik in plaats daarvan een wachtwoord** wanneer u zich aanmeldt. We uw meest recente keuze onthouden en bieden die standaard de volgende keer dat u zich aanmeldt. Als u ooit wilt terugkeren naar het aanmelden met uw telefoon, klikt u op **gebruik van een app in plaats daarvan**. 
 
### <a name="can-i-use-the-app-to-sign-in-to-all-my-accounts-with-microsoft"></a>Kan ik de app aan te melden bij alle accounts met Microsoft gebruiken?   
Deze functionaliteit is alleen beschikbaar voor persoonlijke Microsoft-accounts op dit moment. 
 
### <a name="can-i-sign-into-my-pc-with-my-phone"></a>Kan ik aanmelden bij deze PC met mijn telefoon?  
Voor uw PC aangeraden aanmelden met Windows Hello op Windows 10 met uw face vingerafdruk of een PINCODE.   
 
### <a name="can-i-sign-in-with-my-windows-phone"></a>Kan ik Meld u aan met mijn Windows Phone?  
Op dit moment zijn we deze functionaliteit niet ontwikkelen voor de Microsoft-Authenticator op Windows Phone. 

## <a name="next-steps"></a>Volgende stappen
Als u de Microsoft Authenticator-app niet hebt gedownload, het uitchecken. De app is beschikbaar voor [Windows Phone](http://go.microsoft.com/fwlink/?Linkid=825071), en is beschikbaar op de Microsoft Authenticator-app voor aanmeldingspagina phone [Android](http://go.microsoft.com/fwlink/?Linkid=825072) en [iOS](http://go.microsoft.com/fwlink/?Linkid=825073).

Als u vragen over de app in het algemeen hebt, bekijk de [verificator Veelgestelde vragen over Microsoft](microsoft-authenticator-app-faq.md)
