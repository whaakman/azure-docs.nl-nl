---
title: Microsoft Authenticator-app help en ondersteuning | Microsoft Docs
description: Geeft een lijst met veelgestelde vragen en antwoorden die betrekking hebben op de Microsoft Authentication-app en de Azure multi-factor Authentication.
services: multi-factor-authentication
documentationcenter: ''
author: barlanmsft
manager: mtillman
ms.assetid: f04d5bce-e99e-4f75-82d1-ef6369be3402
ms.service: multi-factor-authentication
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/08/2018
ms.author: lizross
ms.reviewer: librown
ms.custom: end-user
ms.openlocfilehash: e9c152fddbfcd603a84caf2c168ec4f12368dcfa
ms.sourcegitcommit: fa493b66552af11260db48d89e3ddfcdcb5e3152
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/23/2018
ms.locfileid: "31795864"
---
# <a name="microsoft-authenticator-app-faq"></a>Veelgestelde vragen over Microsoft Authenticator-app

In dit artikel antwoorden op veelgestelde vragen over de Microsoft Authenticator-app. Als u een antwoord op uw vraag niet ziet, gaat u naar de [Microsoft Authenticator-app-forum](https://social.technet.microsoft.com/Forums/en-US/home?forum=MicrosoftAuthenticatorApp). Bovendien kunt u een andere veelgestelde vragen over een specifieke functie in de app bekijken [aanmelden met uw telefoon Veelgestelde vragen over](microsoft-authenticator-app-phone-signin-faq.md).

De Microsoft Authenticator-app de Azure Authenticator-app vervangen en de aanbevolen app is wanneer u Azure multi-factor Authentication. De Microsoft Authenticator-app is beschikbaar voor [Android](https://go.microsoft.com/fwlink/?linkid=866594), [iOS](https://go.microsoft.com/fwlink/?linkid=866594), en [Windows Phone](http://go.microsoft.com/fwlink/?Linkid=825071).

## <a name="frequently-asked-questions"></a>Veelgestelde vragen

### <a name="what-data-does-the-authenticator-store-on-my-behalf-and-how-can-i-delete-it"></a>Welke gegevens de verificator slaat namens mijn en hoe kan ik het verwijderen?

Microsoft Authenticator slaat de accountgegevens die u maakt wanneer u een account toevoegt. Wanneer u verificator gebruikt, wordt een diagnostische logboek voor foutopsporing wordt gemaakt en nuttige gegevens opslaat in helpt Microsoft onvoorziene problemen diagnosticeren. U kunt de logboekgegevens openen door het openen van **Help** > **logboeken verzenden** > **logboeken bekijken**.

U kunt de gegevens verwijderen door de tegel account te verwijderen. Als u de tegel account verwijdert, verwijdert tevens alle accountgegevens die wordt gebruikt door de toepassing met inbegrip van de logboeken. 

Voor meer informatie over hoe Microsoft uw gegevens gebruikt, gaat u naar: https://servicetrust.microsoft.com/ViewPage/PrivacyGettingStarted

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

Als u deze suggesties voor probleemoplossing probeert, maar nog steeds problemen ondervindt, kunt u de logboeken voor diagnostische gegevens verzenden. Ga naar de app-instellingen en selecteer vervolgens **Help en feedback** en **logboeken verzenden**. Ga vervolgens naar de [Microsoft Authenticator-app-forum](https://social.technet.microsoft.com/Forums/en-US/home?forum=MicrosoftAuthenticatorApp) en laat ons weten wat u ziet, probleem- en welke stappen u hebt geprobeerd tot nu toe.

### <a name="im-already-using-the-microsoft-authenticator-application-for-verification-codes-how-do-i-switch-to-one-click-push-notifications"></a>Ik gebruik al de toepassing Microsoft Authenticator voor verificatiecodes. Hoe schakel ik om één muisklik pushmeldingen te verzenden
Goedkeuren van een aanmeldingspagina via push-bericht is alleen beschikbaar voor persoonlijke Microsoft-accounts of werken en Microsoft-accounts, niet voor de accounts van derden zoals Google of Facebook school. Als u een werk- of school Microsoft-account hebt, moet uw organisatie kunt deze optie is uitgeschakeld.

Als u een Microsoft-account voor uw persoonlijke account gebruiken, en u overschakelen wilt via om pushmeldingen te verzenden, moet u uw account opnieuw toevoegen. Het apparaat bij uw account opnieuw te registreren en pushmeldingen in te stellen.  

Als u Microsoft Authenticator voor uw werk gebruiken- of schoolaccount, beslist uw organisatie vervolgens of meldingen van één muisklik toestaan.

### <a name="do-one-click-push-notifications-work-for-non-microsoft-accounts"></a>Werken pushmeldingen met één Klik voor niet-Microsoft-accounts?
Nee, werken pushmeldingen alleen met Microsoft-accounts en Azure Active Directory-accounts. Als uw werk of school gebruikmaakt van Azure AD-accounts, kunnen ze deze functie uitschakelen.  

### <a name="i-got-a-new-device-or-restored-my-device-from-a-backup-how-do-i-set-up-my-accounts-in-the-microsoft-authenticator-app-again"></a>Ik hebt u een nieuw apparaat of het apparaat teruggezet vanuit een back-up. Hoe stel ik mijn accounts in de Microsoft Authenticator-app opnieuw?
Als u werkt met een iOS-apparaat, hebt ingeschakeld **automatische back-**, en een back-up van uw accounts hebt gemaakt op het oude apparaat; u kunt back-up herstellen van de referenties van uw account op het nieuwe apparaat. Zie voor meer informatie de [accountreferenties back-up en herstellen met de app Microsoft Authenticator](microsoft-authenticator-app-backup-and-recovery.md) artikel. 

### <a name="i-lost-my-device-or-moved-on-to-a-new-device-how-do-i-make-sure-notifications-dont-continue-to-go-to-my-old-device"></a>Ik mijn apparaat verloren of verplaatst op naar een nieuw apparaat. Hoe maak ik ervoor meldingen niet door te gaan naar mijn oude apparaat?  
De Microsoft Authenticator-app toe te voegen aan uw nieuwe iOS-apparaat verwijdert niet automatisch verwijderd de app op uw oude apparaat. Zelfs de app uit het oude apparaat verwijderd, niet voldoende. U moet zowel de app verwijderen uit het oude apparaat en laat Microsoft of uw organisatie bent vergeten het oude apparaat en het ongedaan te maken van uw account.
- **De app verwijderen van een apparaat met een persoonlijk Microsoft-account.** Ga naar het gedeelte van de verificatie in twee stappen van uw [accountbeveiliging](https://account.microsoft.com/security) pagina en verificatie voor uw oude apparaat uitschakelen.  
- **Aan de app verwijderen van een apparaat met een werk- of schoolaccount van Microsoft-account.** Ga naar het gedeelte van de verificatie in twee stappen van uw [MyApps](https://myapps.microsoft.com/) pagina of naar uw organisatie aangepaste portal en verificatie voor uw oude apparaat uitschakelen. 



### <a name="how-do-i-remove-an-account-from-the-app"></a>Hoe verwijder ik een account uit de app?
* iOS: van het hoofdvenster van Veeg links op de tegel van een account. Selecteer **Verwijderen**.
* Windows Phone: Van het hoofdvenster worden weergegeven, selecteert u de menuknop Klik **accounts bewerken**. Tik op de **X** naast de accountnaam.
* Android: Van het hoofdvenster worden weergegeven, selecteert u de menuknop Klik **accounts bewerken**. Tik op de **X** naast de accountnaam.

Als u een apparaat dat is geregistreerd bij uw organisatie hebt, moet u wellicht een extra stap om uw account te verwijderen voltooid. De Microsoft Authenticator-app wordt op deze apparaten automatisch geregistreerd als de apparaatbeheerder van een. Als u wilt dat de app volledig verwijderen, moet u eerst Hef de registratie van de app in de app-instellingen.

### <a name="why-does-the-app-request-so-many-permissions"></a>Waarom de app zo veel machtigingen aanvragen?
Hier volgt een volledige lijst met machtigingen die kunnen worden gevraagd en hoe ze worden gebruikt in de app. De specifieke machtigingen die u ziet, is afhankelijk van het type van de telefoon die u hebt.

* **Camera**: gebruikt voor het scannen van QR-codes wanneer u een werk-, school- of niet-Microsoft-account toevoegt.
* **Contactpersonen en phone**: gebruikt voor het proces vereenvoudigen door bestaande accounts op uw telefoon zoeken wanneer u zich met je persoonlijke Microsoft-account aanmeldt.
* **SMS**: gebruikt om ervoor te zorgen dat uw telefoonnummer overeenkomt met het nummer op record. Wanneer u zich aan met je persoonlijke Microsoft-account voor de eerste keer.  We sturen een SMS-bericht naar de telefoon waar u de app met een verificatiecode 6-8-cijferige hebt gedownload. In plaats van de vraag of u vindt deze code en voer deze in de app, deze gevonden in het SMS-bericht voor u.
* **Tekenen via andere apps**: de melding dat u dat uw identiteit verifieert, wordt ook weergegeven op alle andere Apps die kan worden uitgevoerd.
* **Gegevens van het internet ontvangt**: deze machtiging is vereist voor het verzenden van meldingen.
* **Telefoon voorkomen in de slaapstand**: als u uw apparaat bij uw organisatie registreren, uw organisatie dit beleid op uw telefoon kunt wijzigen.
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
