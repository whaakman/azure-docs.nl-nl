---
title: Help bij Microsoft Authenticator-app - Azure AD | Microsoft Docs
description: Geeft een lijst met veelgestelde vragen en antwoorden die betrekking hebben op de Microsoft Authentication-app en de Azure multi-factor Authentication.
services: multi-factor-authentication
documentationcenter: ''
author: eross-msft
manager: mtillman
ms.assetid: f04d5bce-e99e-4f75-82d1-ef6369be3402
ms.service: multi-factor-authentication
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 03/08/2018
ms.author: lizross
ms.reviewer: librown
ms.custom: end-user
ms.openlocfilehash: d86bc84653e38a9b64a336b8ce9ed7e657129e8c
ms.sourcegitcommit: 7208bfe8878f83d5ec92e54e2f1222ffd41bf931
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/14/2018
ms.locfileid: "39059828"
---
# <a name="microsoft-authenticator-app-faq"></a>Veelgestelde vragen over Microsoft Authenticator-app

In dit artikel vindt u antwoorden op veelgestelde vragen over de Microsoft Authenticator-app. Als u een antwoord op uw vraag niet ziet, gaat u naar de [Microsoft Authenticator-app forum](https://social.technet.microsoft.com/Forums/en-US/home?forum=MicrosoftAuthenticatorApp). Bovendien kunt u een andere veelgestelde vragen over een specifieke functie in de app bekijken [Meld u aan met uw telefoon Veelgestelde vragen over](microsoft-authenticator-app-phone-signin-faq.md).

De Microsoft Authenticator-app de app Azure Authenticator vervangen en is de aanbevolen app wanneer u Azure multi-factor Authentication. De Microsoft Authenticator-app is beschikbaar voor [Android](https://go.microsoft.com/fwlink/?linkid=866594), [iOS](https://go.microsoft.com/fwlink/?linkid=866594) en [Windows Phone](http://go.microsoft.com/fwlink/?Linkid=825071).

## <a name="frequently-asked-questions"></a>Veelgestelde vragen

### <a name="what-data-does-the-authenticator-store-on-my-behalf-and-how-can-i-delete-it"></a>Welke gegevens de verificator opslaan op mijn naam en hoe kan ik deze verwijderen?

Microsoft Authenticator slaat de accountgegevens die u maakt wanneer u een account toevoegen. Wanneer u Authenticator gebruikt, wordt een diagnostisch logboek voor foutopsporing wordt gemaakt en nuttige gegevens worden opgeslagen in uw hulp bij onvoorziene problemen vaststellen. U kunt toegang krijgen tot de logboekgegevens door het openen van **Help** > **logboeken verzenden** > **logboeken bekijken**.

U kunt de gegevens te verwijderen van de tegel account verwijderen. Als u de tegel account verwijdert, verwijdert u ook alle accountgegevens die wordt gebruikt door de toepassing met inbegrip van de logboeken. 

Voor meer informatie over hoe Microsoft uw gegevens gebruikt, gaat u naar: https://servicetrust.microsoft.com/ViewPage/PrivacyGettingStarted

### <a name="what-are-the-codes-in-the-app-for-why-does-the-number-keep-counting-down"></a>Wat zijn de codes in de app voor? Waarom wordt het aantal behouden tellen omlaag?

Wanneer u de Microsoft Authenticator-app opent, ziet u de accounts die u hebt toegevoegd en een getal van zes of acht cijfers door elk van deze. U ziet mogelijk een timer die 30 seconden.

Deze codes worden gebruikt wanneer u zich aanmeldt bij uw account. Nadat u uw gebruikersnaam en wachtwoord hebt ingevoerd, u mogelijk gevraagd een verificatiecode invoeren. Open de Microsoft Authenticator-app en kopieer de code die wordt weergegeven. Voer deze code op de pagina aanmelden om te voltooien.

De reden dat de codes voor elke 30 seconden wijzigen is zodat u dezelfde code nooit twee keer gebruiken. Het is niet als een wachtwoord dat u verwacht te onthouden. Het idee is dat alleen gebruikers met toegang tot uw telefoon weet dat uw verificatiecode.

De codes nodig geen internet of gegevens, zodat u niet hoeft te hoeven maken over telefoonservice aan te melden bij een. Wanneer u de app sluit, deze niet blijven uitvoeren op de achtergrond en wordt de accu niet leegmaken. U kunt de app sluiten en negeren tot de volgende keer dat u zich aanmelden.  

### <a name="i-only-get-notifications-when-i-have-the-app-open-if-the-app-isnt-open-i-dont-get-any-notifications"></a>Kan ik alleen ontvangen meldingen wanneer ik de app hebt geopend. Als de app niet geopend is, krijg ik geen meldingen.

Als u vooraf meldingen krijgt, maar ze niet ruis maken of Trillen ondanks de belsignaal op, controleert u eerst de app-instellingen. Inschakelen voor de app voor het gebruik van geluid of Trillen met meldingen.

Als u geen meldingen op alle krijgt, controleert u de volgende gevallen:

- Uw telefoon is niet storen of de stille modus? Deze modus kan voorkomen dat apps verzenden van meldingen.
- Kunt u meldingen ontvangen van andere apps? Als dat niet het geval is, kan er een probleem met de netwerkverbindingen op uw telefoon of het kanaal meldingen van Android- of Apple. U kunt de eerste optie adres in de instellingen van uw telefoon, maar u moet mogelijk om te communiceren met uw serviceprovider voor hulp bij de tweede optie.
- Kunt u meldingen voor sommige accounts op de app, maar niet voor andere ontvangen? Zo ja, de problematische account verwijderen uit uw app en voeg deze opnieuw zodat pushmeldingen te verzenden.

Als u deze suggesties voor probleemoplossing geprobeerd maar nog steeds problemen ondervindt, kunt u de logboeken voor diagnostische gegevens verzenden. Ga naar de app-instellingen en selecteer vervolgens **Help en feedback** en **logboeken verzenden**. Ga vervolgens naar de [Microsoft Authenticator-app forum](https://social.technet.microsoft.com/Forums/en-US/home?forum=MicrosoftAuthenticatorApp) en laat ons weten wat u ziet probleem en welke stappen u hebt geprobeerd tot nu toe.

### <a name="im-already-using-the-microsoft-authenticator-application-for-verification-codes-how-do-i-switch-to-one-click-push-notifications"></a>Ik gebruik al de Microsoft Authenticator-toepassing voor verificatiecodes. Hoe kan ik schakelen om één muisklik pushmeldingen te verzenden?
Goedkeuren van een aanmelding via push-bericht is alleen beschikbaar voor persoonlijke Microsoft-accounts of werk en school van Microsoft-accounts, niet voor de accounts van derden, zoals Google of Facebook. Als u een werk- of school Microsoft-account hebt, kan uw organisatie kunt deze optie uitschakelt.

Als u een Microsoft-account voor uw persoonlijke account gebruiken en wilt overschakelen om pushmeldingen te verzenden, moet u uw account opnieuw toevoegen. Het apparaat bij uw account opnieuw te registreren en pushmeldingen instellen.  

Als u Microsoft Authenticator voor uw werk gebruiken- of schoolaccount, klikt u vervolgens uw organisatie besluit of meldingen in één klik is toegestaan.

### <a name="do-one-click-push-notifications-work-for-non-microsoft-accounts"></a>Werken pushmeldingen met één Klik voor niet-Microsoft-accounts?
Nee, werken pushmeldingen alleen met Microsoft-accounts en Azure Active Directory-accounts. Als uw werk of school gebruikmaakt van Azure AD-accounts, kunnen ze deze functie uitschakelen.  

### <a name="i-got-a-new-device-or-restored-my-device-from-a-backup-how-do-i-set-up-my-accounts-in-the-microsoft-authenticator-app-again"></a>Ik kreeg een nieuw apparaat of het apparaat teruggezet vanuit een back-up. Hoe ik mijn accounts in de Microsoft Authenticator-app opnieuw instellen?
Als u een iOS-apparaat uitvoert, hebben ingeschakeld **automatische back-ups**, en een back-up van uw accounts hebt gemaakt op het oude apparaat; u kunt deze back-up herstellen van de referenties van uw account op het nieuwe apparaat. Zie voor meer informatie de [accountreferenties back-up en herstellen met de Microsoft Authenticator-app](microsoft-authenticator-app-backup-and-recovery.md) artikel. 

### <a name="i-lost-my-device-or-moved-on-to-a-new-device-how-do-i-make-sure-notifications-dont-continue-to-go-to-my-old-device"></a>Kan ik mijn apparaat verloren of verplaatst op naar een nieuw apparaat. Hoe maak ik ervoor dat meldingen niet door te gaan naar het oude apparaat?  
De Microsoft Authenticator-app toe te voegen aan uw nieuwe iOS-apparaat, wordt niet automatisch de app verwijderd uit het oude apparaat. Zelfs de app verwijderen uit het oude apparaat is niet voldoende. U moet zowel de app verwijderen uit het oude apparaat en laat weten van Microsoft of uw organisatie bent vergeten het oude apparaat en het ongedaan te maken van uw account.
- **De app vanaf een apparaat met een persoonlijk Microsoft-account verwijderen.** Ga naar het gebied van de verificatie in twee stappen van uw [accountbeveiliging](https://account.microsoft.com/security) pagina en kiest u voor verificatie voor het oude apparaat uitschakelt.  
- **Aan de app verwijderd uit een apparaat met een werk- of schoolaccount van Microsoft-account.** Ga naar het gebied van de verificatie in twee stappen van uw [MyApps](https://myapps.microsoft.com/) pagina of naar aangepaste portal van uw organisatie en kies verificatie voor het oude apparaat uitschakelen. 



### <a name="how-do-i-remove-an-account-from-the-app"></a>Hoe verwijder ik een account van de app?
* iOS: van het hoofdvenster, Veeg links op de tegel van een account. Selecteer **Verwijderen**.
* Windows Phone: Van het hoofdvenster, selecteert u de menuknop klikt **accounts bewerken**. Tik op de **X** naast de accountnaam van het.
* Android: Van het hoofdvenster, selecteert u de menuknop vervolgens **accounts bewerken**. Tik op de **X** naast de accountnaam van het.

Als u een apparaat dat is geregistreerd bij uw organisatie hebt, moet u mogelijk om een extra stap als u wilt verwijderen van uw account te voltooien. De Microsoft Authenticator-app wordt op deze apparaten automatisch geregistreerd als de apparaatbeheerder van een. Als u de app volledig te verwijderen wilt, moet u eerst de registratie van de app in de instellingen van de app ongedaan maken.

### <a name="why-does-the-app-request-so-many-permissions"></a>Waarom de app zoveel machtigingen aanvragen?
Hier volgt een volledige lijst met machtigingen die kan worden gevraagd om en hoe ze worden gebruikt in de app. De specifieke machtigingen die u ziet, is afhankelijk van het type van de telefoon hebt.

* **Camera**: gebruikt voor het QR-codes scannen als u een werk-, school- of niet-Microsoft-account toevoegen.
* **Contactpersonen en phone**: gebruikt voor het vereenvoudigen van het proces door te zoeken naar bestaande accounts op uw telefoon wanneer u zich met uw persoonlijke Microsoft-account aanmeldt.
* **SMS**: gebruikt om te controleren of het telefoonnummer dat overeenkomt met het aantal op record. Wanneer u zich aanmelden met uw persoonlijke Microsoft-account voor de eerste keer.  We verzenden een SMS-bericht naar de telefoon waar u de app met een verificatiecode van 6 tot 8 cijfer hebt gedownload. In plaats van of u wilt zoeken van deze code en voer deze in de app, wordt deze gevonden in het SMS-bericht voor u.
* **Tekenen van andere apps**: de melding krijgt die controleert of uw identiteit wordt ook weergegeven op alle andere Apps die kan worden uitgevoerd.
* **Gegevens ontvangen van internet**: met deze machtiging is vereist voor het verzenden van meldingen.
* **Voorkomen dat phone slaapstand**: als u uw apparaat bij uw organisatie registreren, uw organisatie dit beleid op uw telefoon kunt wijzigen.
* **Besturingselement trillingen**: U kunt kiezen of u een trillingen wilt wanneer u ontvangt een melding om uw identiteit te verifiëren.
* **Vingerafdruk hardware gebruiken**: sommige accounts werk- en schoolaccounts een extra PINCODE is vereist wanneer u uw identiteit verifiëren. Het proces om eenvoudiger te maken, kunt u uw vingerafdruk gebruiken in plaats van de pincode.
* **Netwerkverbindingen weergeven**: wanneer u een Microsoft-account toevoegt, app netwerk/internetverbinding nodig.
* **De inhoud van uw opslag lezen**: met deze machtiging wordt alleen gebruikt wanneer u een technisch probleem met de app-instellingen. Sommige gegevens van uw opslag worden verzameld voor het vaststellen van het probleem.
* **Volledige toegang tot het netwerk**: met deze machtiging is vereist voor het verzenden van meldingen om uw identiteit te verifiëren.
* **Uitvoeren bij het opstarten**: als u uw telefoon opnieuw start, deze machtiging zorgt ervoor dat u blijven ontvangen van meldingen om uw identiteit te verifiëren.

### <a name="why-does-the-microsoft-authenticator-app-allow-you-to-approve-a-request-without-unlocking-the-device"></a>Waarom de Microsoft Authenticator-App bent u in staat voor het goedkeuren van een aanvraag zonder het apparaat te ontgrendelen?

U hebt geen voor het ontgrendelen van het apparaat voor het goedkeuren van aanvragen voor verificatie, omdat alles wat u nodig hebt om te bewijzen dat u uw telefoon bij u hebben. Verificatie in twee stappen vereist twee dingen: een ding die u kent en een dat die u hoeft aan te tonen. Het wat u weet dat is uw wachtwoord. Het wat dat u hebt, is uw telefoon (ingesteld met de Microsoft Authenticator-app en geregistreerd als een MFA-bewijs.) Daarom hebben de telefoon en de aanvraag wordt goedgekeurd voldoet aan de criteria voor de tweede factor van de verificatie.

### <a name="what-does-the-lock-icon-in-the-account-list-mean"></a>Wat betekent het vergrendelingspictogram in de lijst met accounts?

Het hangslotpictogram geeft aan dat het apparaat is geregistreerd bij Azure AD en geregistreerd voor het account. Device Registration service voor iOS vindt plaats tijdens de registratie van Microsoft Intune.

## <a name="next-steps"></a>Volgende stappen

### <a name="contact-us"></a>Contact opnemen
Als uw vraag hier niet is beantwoord, willen we graag van u. Ga naar de [Microsoft Authenticator-app forum](https://social.technet.microsoft.com/Forums/en-US/home?forum=MicrosoftAuthenticatorApp) op uw vragen stellen en hulp krijgen van de community, of een opmerking op deze pagina.


### <a name="related-topics"></a>Verwante onderwerpen
* [Over verificatie in twee stappen](https://support.microsoft.com/help/12408/microsoft-account-about-two-step-verification) voor Microsoft-accounts
* [Problemen met verificatie in twee stappen](multi-factor-authentication-end-user-troubleshoot.md) voor uw account voor werk of school?
* [De Microsoft Authenticator gebruikt om te melden op uw telefoon](microsoft-authenticator-app-phone-signin-faq.md)
