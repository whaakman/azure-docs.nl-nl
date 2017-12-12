---
title: Microsoft Authenticator-app help en ondersteuning | Microsoft Docs
description: Geeft een lijst met veelgestelde vragen en antwoorden die betrekking hebben op de Microsoft Authentication-app en de Azure multi-factor Authentication.
services: multi-factor-authentication
documentationcenter: 
author: barlanmsft
manager: mtillman
ms.assetid: f04d5bce-e99e-4f75-82d1-ef6369be3402
ms.service: multi-factor-authentication
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/17/2017
ms.author: barlan
ms.reviewer: librown
ms.custom: end-user
ms.openlocfilehash: 073b8adfcbe5fdcc2a6d1dba820a1101fac83218
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/11/2017
---
# <a name="microsoft-authenticator-app-faq"></a>Veelgestelde vragen over Microsoft Authenticator-app

In dit artikel antwoorden op veelgestelde vragen die we over de Microsoft Authenticator-app ontvangen. Als u een antwoord op uw vraag niet ziet, gaat u naar de [Microsoft Authenticator-app-forum](https://social.technet.microsoft.com/Forums/en-US/home?forum=MicrosoftAuthenticatorApp). We hebben ook een andere veelgestelde vragen over een specifieke functie in de app [aanmelden met uw telefoon Veelgestelde vragen over](microsoft-authenticator-app-phone-signin-faq.md).

De Microsoft Authenticator-app de Azure Authenticator-app vervangen en de aanbevolen app is wanneer u Azure multi-factor Authentication. De Microsoft Authenticator-app is beschikbaar voor [Windows Phone](http://go.microsoft.com/fwlink/?Linkid=825071), [Android](http://go.microsoft.com/fwlink/?Linkid=825072), en [iOS](http://go.microsoft.com/fwlink/?Linkid=825073).

## <a name="frequently-asked-questions"></a>Veelgestelde vragen

### <a name="what-are-the-codes-in-the-app-for-why-does-the-number-keep-counting-down"></a>Wat zijn de codes in de app voor? Waarom wordt het aantal behouden tellen omlaag?

Als u de Microsoft Authenticator-app opent, ziet u de accounts die u hebt toegevoegd en een nummer 6-cijferige of acht door elk van deze. U ziet een timer die 30 seconden.

Deze codes worden gebruikt wanneer u zich aanmeldt bij uw account. Nadat u uw gebruikersnaam en wachtwoord hebt ingevoerd, wordt u mogelijk gevraagd een verificatiecode invoeren. Open de Microsoft Authenticator-app en kopieer de code die wordt weergegeven. Voer deze code in de aanmeldingspagina te voltooien.

De reden waarom de codes elke 30 seconden wijzigen is zodat u de dezelfde code nooit twee keer gebruikt. Het is niet als een wachtwoord die u verwacht te onthouden. Het is de bedoeling is dat alleen gebruikers met toegang tot uw telefoon uw verificatiecode kent.

De codes nodig geen internet of gegevens, zodat u niet hoeft te hoeven maken over een telefoonservice aan te melden. Wanneer u de app sluit, wordt niet op de achtergrond houden uitgevoerd en deze niet verwijderen uit de accu. U kunt de toepassing sluiten en negeren, tot de volgende keer dat u zich aanmeldt.  

### <a name="i-only-get-notifications-when-i-have-the-app-open-if-the-app-isnt-open-i-dont-get-any-notifications"></a>Ik alleen ophalen meldingen wanneer ik de app hebt geopend. Als de app niet geopend is, krijg ik geen meldingen.

Als u meldingen krijgt, maar ze niet ruis maken of Trillen ondanks uw belsignaal wordt op, Controleer eerst de app-instellingen. De app voor het gebruik van geluid of Trillen met meldingen inschakelen.

Als u geen meldingen op alle krijgt, controleert u de volgende gevallen:

- Uw telefoon is niet storen of stille modus? Deze modus kan voorkomen dat apps verzenden van meldingen.
- Kunt u meldingen ontvangen van andere apps? Als dat niet het geval is, kan er een probleem met de netwerkverbindingen op uw telefoon of het kanaal meldingen van Android- of Apple. U kunt de eerste optie oplossen in de instellingen van uw telefoon, maar mogelijk moet u contact opnemen met uw serviceprovider voor hulp bij de tweede optie.
- Kunt u meldingen voor sommige accounts op de app, maar andere niet ontvangen? Zo ja, de problematisch account verwijderen uit uw app en voeg deze opnieuw uit om te pushmeldingen inschakelen.

Als u probeert deze suggesties voor het oplossen van problemen, maar nog steeds problemen ondervindt, stuur ons uw logboeken voor diagnostische gegevens. Ga naar de app-instellingen en selecteer vervolgens **Help en feedback** en **logboeken verzenden**. Ga vervolgens naar de [Microsoft Authenticator-app-forum](https://social.technet.microsoft.com/Forums/en-US/home?forum=MicrosoftAuthenticatorApp) en laat ons weten wat u ziet, probleem- en welke stappen u hebt geprobeerd tot nu toe.

### <a name="im-already-using-the-microsoft-authenticator-application-for-verification-codes-how-do-i-switch-to-one-click-push-notifications"></a>Ik gebruik al de toepassing Microsoft Authenticator voor verificatiecodes. Hoe schakel ik om één muisklik pushmeldingen te verzenden
Goedkeuren van een aanmeldingspagina via push-bericht is alleen beschikbaar voor persoonlijke Microsoft-accounts of werken en Microsoft-accounts, niet voor de accounts van derden zoals Google of Facebook school. Als u een werk- of school Microsoft-account hebt, moet uw organisatie kunt deze optie is uitgeschakeld.

Als u een Microsoft-account voor uw persoonlijke account gebruiken, en u overschakelen wilt via om pushmeldingen te verzenden, moet u uw account opnieuw toevoegen. Het apparaat bij uw account opnieuw te registreren en pushmeldingen in te stellen.  

Als u Microsoft Authenticator voor uw werk gebruiken- of schoolaccount, beslist uw organisatie vervolgens of meldingen van één muisklik toestaan.

### <a name="do-one-click-push-notifications-work-for-non-microsoft-accounts"></a>Werken pushmeldingen met één Klik voor niet-Microsoft-accounts?
Nee, werken pushmeldingen alleen met Microsoft-accounts en Azure Active Directory-accounts. Als uw werk of school gebruikmaakt van Azure AD-accounts, kunnen ze deze functie uitschakelen.  

### <a name="i-restored-my-device-from-a-backup-and-my-account-codes-are-missing-or-not-working-what-happened"></a>Ik mijn apparaat hersteld vanaf een back-up en Mijn accountcodes zijn ontbreekt of niet werkt. Wat is er gebeurd?
Om veiligheidsredenen herstellen niet we accounts vanuit back-ups van app.  Nadat u de app hebt hersteld, moet uw accounts verwijderen en deze opnieuw toevoegen.

### <a name="i-got-a-new-device-how-do-i-remove-the-microsoft-authenticator-app-from-my-old-device-and-move-to-the-new-one"></a>Ik heb een nieuw apparaat. Hoe ik Microsoft Authenticator-app verwijderen uit het oude apparaat en verplaatsen naar de nieuwe?
De Microsoft Authenticator-app aan een nieuw apparaat toe te voegen komt niet automatisch verwijderd van alle andere apparaten. Om te beheren welke apparaten zijn geconfigureerd voor uw account, gaat u naar dezelfde website die u gebruikt voor het beheren van verificatie in twee stappen en kies verwijderen van oude apps.

Deze website is voor persoonlijke Microsoft-accounts, uw [accountbeveiliging](https://account.microsoft.com/security) pagina. Voor Microsoft-accounts, werk- of schoolaccount, deze website is mogelijk een [MyApps](https://myapps.microsoft.com) of een aangepaste portal die uw organisatie is ingesteld.

### <a name="how-do-i-remove-an-account-from-the-app"></a>Hoe verwijder ik een account uit de app?
* iOS: van het hoofdvenster van Veeg links op de tegel van een account. Selecteer **Verwijderen**.
* Windows Phone: Van het hoofdvenster worden weergegeven, selecteert u de menuknop Klik **accounts bewerken**. Tik op de **X** naast de accountnaam.
* Android: Van het hoofdvenster worden weergegeven, selecteert u de menuknop Klik **accounts bewerken**. Tik op de **X** naast de accountnaam.

Als u een apparaat dat is geregistreerd bij uw organisatie hebt, moet u wellicht een extra stap om uw account te verwijderen voltooid. De Microsoft Authenticator-app wordt op deze apparaten automatisch geregistreerd als de apparaatbeheerder van een. Als u wilt dat de app volledig verwijderen, moet u eerst Hef de registratie van de app in de app-instellingen.

### <a name="why-does-the-app-request-so-many-permissions"></a>Waarom de app zo veel machtigingen aanvragen?
Hier volgt een volledige lijst met machtigingen vragen we voor en hoe ze worden gebruikt in de app. De specifieke machtigingen die u ziet, is afhankelijk van het type van de telefoon die u hebt.

* **Camera**: We gebruik uw camera om te scannen van QR-codes wanneer u een werk-, school- of niet-Microsoft-account toevoegt.
* **Contactpersonen en phone**: wanneer u zich met je persoonlijke Microsoft-account aanmeldt, we proberen te vereenvoudigen door bestaande accounts die u op uw telefoon gebruikt zoeken.
* **SMS**: tijdens het aanmelden bij je persoonlijke Microsoft-account voor de eerste keer dat we hebben om ervoor te zorgen dat uw telefoonnummer overeenkomt met de structuur die wij in de record hebben. We sturen een SMS-bericht naar de telefoon waar u de app hebt gedownload. Het bericht bevat een 6-8 cijfers-verificatiecode. In plaats van de vraag of u vindt deze code en voer deze in de app, vinden wij in het SMS-bericht voor u.
* **Tekenen via andere apps**: wanneer u een melding om uw identiteit te verifiëren ontvangt, we dat meldingen worden weergeven via alle andere Apps die kan worden uitgevoerd.
* **Gegevens van het internet ontvangt**: deze machtiging is vereist voor het verzenden van meldingen.
* **Telefoon voorkomen in de slaapstand**: als u uw apparaat bij uw organisatie registreren, kunnen dit beleid op uw telefoon wijzigen.
* **Beheren van trillingen**: U kunt kiezen of u een trillingen dat wilt wanneer u ontvangt een melding om uw identiteit te verifiëren.
* **Vingerafdruk hardware gebruiken**: sommige accounts werk- en schoolaccounts een extra PINCODE is vereist wanneer u uw identiteit verifiëren. Het proces om gemakkelijker te maken, kunt u uw vingerafdruk gebruiken in plaats van de pincode.
* **Netwerkverbindingen weergeven**: wanneer u een Microsoft-account toevoegt, de app netwerk/internet connection vereist.
* **De inhoud van uw opslag lezen**: met deze machtiging wordt alleen gebruikt wanneer u een technisch probleem via de app-instellingen. Sommige gegevens van uw opslag worden verzameld voor het vaststellen van het probleem.
* **Volledige toegang tot het netwerk**: deze machtiging is vereist voor het verzenden van meldingen om uw identiteit te verifiëren.
* **Uitgevoerd bij het opstarten**: als u uw telefoon opstarten, deze machtiging zorgt ervoor dat u blijven ontvangen van meldingen om uw identiteit te verifiëren.

### <a name="why-does-the-microsoft-authenticator-app-allow-you-to-approve-a-request-without-unlocking-the-device"></a>Waarom de verificator-App van Microsoft kunt u een aanvraag goedkeuren zonder het apparaat te ontgrendelen?

U hoeft niet te ontgrendelen van het apparaat voor het goedkeuren van aanvragen voor verificatie, omdat alles wat u nodig hebt om te bewijzen dat u uw telefoon bij u hebben. Verificatie in twee stappen vereist twee dingen: een ding dat u weet en een dat die u hoeft aan te tonen. Het u weet wat is uw wachtwoord. Het hebt u wat is uw telefoon (geregistreerd als een bewijs MFA en ingesteld met de Microsoft Authenticator-app.) Daarom is het telefoonnummer en de aanvraag goedkeuren voldoet aan de criteria voor de tweede factor van verificatie.

### <a name="what-does-the-lock-icon-in-the-account-list-mean"></a>Wat betekent het vergrendelingspictogram in de lijst met accounts?

Het hangslot geeft aan dat het apparaat is geregistreerd in Azure AD en geregistreerd voor het account. Apparaatregistratie voor iOS-vindt plaats tijdens de inschrijving bij Microsoft Intune.

## <a name="next-steps"></a>Volgende stappen

### <a name="contact-us"></a>Contact opnemen
Als uw vraag hier niet is beantwoord, willen we graag van u. Ga naar de [Microsoft Authenticator-app-forum](https://social.technet.microsoft.com/Forums/en-US/home?forum=MicrosoftAuthenticatorApp) Stuur uw vraag hulp van de community of en laat een opmerking op deze pagina.


### <a name="related-topics"></a>Verwante onderwerpen
* [Over verificatie in twee stappen](https://support.microsoft.com/help/12408/microsoft-account-about-two-step-verification) voor Microsoft-accounts
* [Problemen met verificatie in twee stappen](multi-factor-authentication-end-user-troubleshoot.md) voor uw werk of school-account?
* [De Microsoft-Authenticator aanmelden op uw telefoon gebruiken](microsoft-authenticator-app-phone-signin-faq.md)
