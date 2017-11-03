---
title: Probleem wachtwoord eenmalige aanmelding voor een toepassing niet galerie configureren | Microsoft Docs
description: Inzicht in het algemene problemen mensen vlak bij het configureren van wachtwoord eenmalige aanmelding voor aangepaste niet-galerie-toepassingen die niet worden weergegeven in de Azure AD-Toepassingsgalerie
services: active-directory
documentationcenter: 
author: ajamess
manager: femila
ms.assetid: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/11/2017
ms.author: asteen
ms.openlocfilehash: 9c76b6f3495e2dd759a156fcef97b57aece8d632
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="problem-configuring-password-single-sign-on-for-a-non-gallery-application"></a>Probleem wachtwoord eenmalige aanmelding voor een toepassing niet galerie configureren

In dit artikel helpt u bij het begrijpen van de face voor mensen van algemene problemen bij het configureren van **eenmalige aanmelding wachtwoord** met een niet-galerie-toepassing.

## <a name="how-to-capture-sign-in-fields-for-an-application"></a>Het vastleggen van velden aanmelden voor een toepassing

Aanmelden veld vastleggen wordt alleen ondersteund voor HTML-functionaliteit aanmeldingspagina's en is **niet ondersteund voor niet-standaard aanmeldingspagina's**, zoals toepassingen die gebruikmaken van Flash of andere technologieën, zonder HTML-functionaliteit.

Er zijn twee manieren waarop u kunt aanmelden velden voor uw aangepaste toepassingen vastleggen:

-   Veld voor automatische aanmelding vastleggen

-   Handmatige aanmelden veld vastleggen

**Veld voor automatische aanmelding vastleggen** werkt goed samen met de meeste ingeschakeld HTML aanmeldingspagina's, als ze gebruiken **bekende DIV-id's voor de gebruikersnaam en wachtwoord invoeren** veld. De manier waarop die dit werkt is door de HTML-code op de pagina om te zoeken DIV-id's die voldoen aan bepaalde criteria slijmen en vervolgens metagegevens voor deze toepassing op te slaan zodat u later te deze wachtwoorden kunnen worden herhaald.

**Handmatige aanmelden veld vastleggen** kan worden gebruikt in het geval dat de toepassing **leverancier komt niet label** invoervelden gebruikt voor aanmelding. Handmatige aanmelden veld vastleggen kan ook worden gebruikt in het geval wanneer de **leverancier renders meerdere velden** en kan niet worden automatisch gedetecteerd. Azure AD kan opslaan van gegevens voor de velden die zich op de aanmeldingspagina, zolang u laat ons weten waar deze velden zijn op de pagina.

In het algemeen **als het veld voor automatische aanmelding vastleggen niet werkt, altijd het is raadzaam de handmatige optie probeert.**

### <a name="how-to-automatically-capture-sign-in-fields-for-an-application"></a>Het vastleggen van automatisch velden aanmelden voor een toepassing

Voor het configureren van **op basis van wachtwoorden Single Sign-on** voor het gebruik van een toepassing **automatische aanmelding veld vastleggen**, volg de onderstaande stappen:

1.  Open de [ **Azure Portal** ](https://portal.azure.com/) en meld u aan als een **hoofdbeheerder** of **Co-beheerder.**

2.  Open de **Azure Active Directory-extensie** door te klikken op **meer services** onderaan in het navigatiemenu belangrijkste linkerkant.

3.  Typ in **' Azure Active Directory**' in het zoekvak filter en selecteer de **Azure Active Directory** item.

4.  Klik op **bedrijfstoepassingen** in het menu van Azure Active Directory linkerkant navigatie.

5.  Klik op **alle toepassingen** om een lijst met al uw toepassingen weer te geven.

  * Als u de toepassing die u wilt weergeven die hier niet ziet, gebruikt u de **Filter** besturingselement aan de bovenkant van de **lijst met alle toepassingen** en stel de **weergeven** optie naar **alle toepassingen.**

6.  Selecteer de toepassing die u wilt configureren eenmalige aanmelding.

7.  Nadat de toepassing wordt geladen, klikt u op de **eenmalige aanmelding** van navigatiemenu links aan de van de toepassing.

8.  Selecteer de modus **op basis van wachtwoorden eenmalige aanmelding.**

9.  Voer de **aanmeldings-URL**. Dit is de URL waar gebruikers hun gebruikersnaam en wachtwoord aanmelden bij invoeren. **Zorg ervoor dat de aanmeldingspagina velden zijn zichtbaar in de URL die u opgeeft**.

10. Klik op de knop **Opslaan**.

11. Zodra u dat doet, wordt je automatisch scrape die URL voor een gebruikersnaam en wachtwoord invoervak en kunt u Azure AD gebruiken voor het veilig verzenden van wachtwoorden voor deze toepassing met behulp van de uitbreiding voor toegang tot Configuratiescherm browser.

## <a name="how-to-manually-capture-sign-in-fields-for-an-application"></a>Handmatig vastleggen velden aanmelden voor een toepassing

Om vast te leggen handmatig velden aanmelden, moet u eerst de Browseruitbreiding van toegang Configuratiescherm geïnstalleerd hebben en **niet wordt uitgevoerd in de modus InPrivate-navigatie, incognito of private.** Voor het installeren van de browser-extensie, volg de stappen in de [het installeren van de Browseruitbreiding toegang Configuratiescherm](#i-cannot-manually-detect-sign-in-fields-for-my-application) sectie.

Voor het configureren van **op basis van wachtwoorden Single Sign-on** voor het gebruik van een toepassing **handmatige aanmelden veld vastleggen**, volg de onderstaande stappen:

1.  Open de [ **Azure Portal** ](https://portal.azure.com/) en meld u aan als een **hoofdbeheerder** of **Co-beheerder.**

2.  Open de **Azure Active Directory-extensie** door te klikken op **meer services** onderaan in het navigatiemenu belangrijkste linkerkant.

3.  Typ in **' Azure Active Directory**' in het zoekvak filter en selecteer de **Azure Active Directory** item.

4.  Klik op **bedrijfstoepassingen** in het menu van Azure Active Directory linkerkant navigatie.

5.  Klik op **alle toepassingen** om een lijst met al uw toepassingen weer te geven.

   * Als u de toepassing die u wilt weergeven die hier niet ziet, gebruikt u de **Filter** besturingselement aan de bovenkant van de **lijst met alle toepassingen** en stel de **weergeven** optie naar **alle toepassingen.**

6.  Selecteer de toepassing die u wilt configureren eenmalige aanmelding.

7.  Nadat de toepassing wordt geladen, klikt u op de **eenmalige aanmelding** van navigatiemenu links aan de van de toepassing.

8.  Selecteer de modus **op basis van wachtwoorden eenmalige aanmelding.**

9.  Voer de **aanmeldings-URL**. Dit is de URL waar gebruikers hun gebruikersnaam en wachtwoord aanmelden bij invoeren. **Zorg ervoor dat de aanmeldingspagina velden zijn zichtbaar in de URL die u opgeeft**.

10. Klik op de knop **Opslaan**.

11. Zodra u dat doet, wordt je automatisch scrape die URL voor een gebruikersnaam en wachtwoord invoervak en kunt u Azure AD gebruiken voor het veilig verzenden van wachtwoorden voor deze toepassing met behulp van de uitbreiding voor toegang tot Configuratiescherm browser. In het geval dat dit niet lukt, kunt u **wijzigen van de aanmelding in de modus voor het gebruik van handmatige aanmelden veld vastleggen** door u wilt doorgaan met stap 12.

12. Klik op **configureren &lt;appname&gt; instellingen voor wachtwoord eenmalige aanmelding**.

13. Selecteer de **aanmelden velden handmatig detecteren** configuratieoptie.

14. Klik op **OK**.

15. Klik op **Opslaan**.

16. Volg de instructies op het scherm voor het gebruik van het toegangsvenster.

## <a name="i-see-a-we-couldnt-find-any-sign-in-fields-at-that-url-error"></a>Er is een fout 'Niet vinden velden aanmelden op die URL'

U ziet deze fout wanneer automatische detectie van velden voor aanmelden is mislukt. U lost dit probleem, probeert u handmatige aanmelden veld detectie door de stappen in de [handmatig vastleggen velden aanmelden voor een toepassing](#how-to-manually-capture-sign-in-fields-for-an-application) sectie.

## <a name="i-see-an-unable-to-save-single-sign-on-configuration-error"></a>Er is een 'kan niet naar de Single Sign-on-configuratie op te slaan' fout

Bijwerken van de configuratie voor eenmalige aanmelding kan in bepaalde zeldzame gevallen mislukken. Deze probeert de opslaan van de eenmalige aanmelding configuratie opnieuw uit te voeren om op te lossen.

Als dit probleem zich voordoen blijft, een ondersteuningsaanvraag te openen en geef de informatie die verzameld de [hoe ziet u de details van een portal melding](#i-cannot-manually-detect-sign-in-fields-for-my-application) en [help opvragen door gegevens meldingen verzenden naar een ondersteuningstechnicus](#how-to-get-help-by-sending-notification-details-to-a-support-engineer) secties.

## <a name="i-cannot-manually-detect-sign-in-fields-for-my-application"></a>Ik kan geen aanmelding handmatig detecteren in velden voor de toepassing

Enkele van de problemen die u tegenkomen kunt wanneer handmatige detectie niet werkt:

-   Het handmatig vastleggen komt werken, maar de vastgelegd velden zijn niet juist

-   De velden rechts ophalen niet gemarkeerd bij het uitvoeren van het vastleggen

-   Het vastleggen, ga ik aan de aanmeldingspagina van de toepassing zoals verwacht, maar er gebeurt niets

-   Handmatige vastleggen lijkt te werken, maar SSO gebeurt niet wanneer mijn gebruikers naar de toepassing in het deelvenster toegang navigeren.

Controleer het volgende als u een van deze problemen optreden:

-   Zorg ervoor dat u de nieuwste versie van de uitbreiding voor toegang tot Configuratiescherm browser **geïnstalleerd** en **ingeschakeld** door de stappen in de [het installeren van de Browseruitbreiding toegang Configuratiescherm](#how-to-install-the-access-panel-browser-extension) sectie.

-   Zorg ervoor dat u het vastleggen niet probeert tijdens uw browser in **modus incognito, InPrivate- of persoonlijke**. De extensie van het Configuratiescherm toegang wordt niet ondersteund in deze modi.

-   Zorg ervoor dat uw gebruikers niet probeert aan te melden bij de toepassing van het toegangsvenster tijdens in **modus incognito, InPrivate- of persoonlijke**. De extensie van het Configuratiescherm toegang wordt niet ondersteund in deze modi.

-   Probeer het handmatige vastleggen opnieuw, zodat de rode markeringen zijn via de juiste velden.

-   Als het handmatig vastleggen lijken vastlopen of niet van toepassing zijn op de aanmeldingspagina Alles (geval 3 hierboven), het handmatige vastleggen opnieuw proberen. Maar deze tijd na het voltooien van het proces, drukt u op de **F12** knop om uw browser developer-console te openen. Eenmaal, opent u de **console** en het type **window.location= '&lt;voert u het teken in de url die u hebt opgegeven bij het configureren van de app&gt;'** en druk vervolgens op **Enter**. Deze kracht een pagina omleiden die eindigen met het vastleggen en opslaan van de velden die zijn vastgelegd.

Als geen van deze benaderingen werkt, kunnen we helpen. Een ondersteuningsaanvraag openen met de details van wat u hebt geprobeerd, evenals de informatie die verzameld de [hoe ziet u de details van een portal melding](#i-cannot-manually-detect-sign-in-fields-for-my-application) en [help opvragen door gegevens meldingen verzenden naar een ondersteuningstechnicus](#how-to-get-help-by-sending-notification-details-to-a-support-engineer) secties (indien van toepassing).

## <a name="how-to-install-the-access-panel-browser-extension"></a>Het installeren van de Browseruitbreiding toegang Configuratiescherm

Volg de onderstaande stappen voor het installeren van de Browseruitbreiding toegang Configuratiescherm:

1.  Open de [Toegangspaneel](https://myapps.microsoft.com) in een van de ondersteunde browsers en meld u aan als een **gebruiker** in uw Azure AD.

2.  Klik op een **wachtwoord SSO toepassing** in het deelvenster toegang.

3.  Selecteer in het venster met de vraag om de software te installeren, **nu installeren**.

4.  Op basis van uw browser u omgeleid naar de downloadkoppeling. **Voeg** de uitbreiding van uw browser.

5.  Als uw browser wordt gevraagd, selecteert u op een **inschakelen** of **toestaan** de extensie.

6.  Nadat deze is geïnstalleerd, **opnieuw** uw browsersessie.

7.  Meld u aan in het deelvenster toegang en zien als u kunt **starten** uw wachtwoord SSO-toepassingen.

U kunt ook de uitbreiding voor Chrome en Firefox downloaden uit de onderstaande directe koppelingen:

-   [Uitbreiding van chrome toegang Configuratiescherm](https://chrome.google.com/webstore/detail/access-panel-extension/ggjhpefgjjfobnfoldnjipclpcfbgbhl)

-   [Uitbreiding van het Configuratiescherm Firefox toegang](https://addons.mozilla.org/firefox/addon/access-panel-extension/)

## <a name="how-to-see-the-details-of-a-portal-notification"></a>Hoe ziet u de details van een melding van de portal

De details van de bedrijfsportal meldingen kunt u zien door de onderstaande stappen te volgen:

1.  Klik op de **meldingen** pictogram (bel) in de rechterbovenhoek van de Azure Portal

2.  Selecteer een melding in een **fout** status (die met een rode (!) naast deze).

  >! Houd er rekening mee] u kunt niet op meldingen in een **geslaagd** of **In uitvoering** status.
  >
  >

3.  Deze open de **Meldingsdetails** blade.

4.  Gebruik deze informatie zelf meer details over het probleem te begrijpen.

5.  Als u nog hulp nodig hebt, kunt u ook deze informatie delen met een ondersteuningstechnicus of de productgroep voor hulp bij het probleem.

6.  Klik op de **kopie** **pictogram** rechts van de **Kopieerfout** tekstvak voor het kopiëren van de Meldingsdetails te delen met een groep engineer van ondersteuning of product.

## <a name="how-to-get-help-by-sending-notification-details-to-a-support-engineer"></a>Help opvragen door gegevens meldingen verzenden naar een ondersteuningstechnicus

Het is heel belangrijk dat u delen **alle onderstaande details** met de ondersteuningstechnicus als u hulp nodig hebt zodat u ze kunt u snel. Dit kunt u doen door eenvoudig **u een schermopname** of door te klikken op de **foutpictogram kopiëren**vindt u rechts van de **Kopieerfout** textbox.

## <a name="notification-details-explained"></a>Details van de melding uitgelegd

De hieronder wordt uitgelegd meer wat elke van de melding betekent dat items en worden voorbeelden gegeven van elk van deze servers.

### <a name="essential-notification-items"></a>Melding van essentiële Items

-   **Titel** – de beschrijvende titel van de melding

    -   Voorbeeld: **Application proxy-instellingen**

-   **Beschrijving** – de beschrijving van wat is het gevolg van de bewerking

    -   Voorbeeld: **interne url ingevoerd wordt al gebruikt door een andere toepassing**

-   **Id van de melding** – de unieke id van de melding

    -   Voorbeeld: **clientNotification-2adbfc06-2073-4678-a69f-7eb78d96b068**

-   **Aanvraag-Id van client** – specifieke aanvraag-id gemaakt door uw browser

    -   Voorbeeld: **302fd775-3329-4670-a9f3-bea37004f0bc**

-   **Stempel UTC-tijd** – de tijdstempel gedurende welke de melding is opgetreden, in UTC

    -   Voorbeeld: **2017-03-23T19:50:43.7583681Z**

-   **Interne transactie-Id** – de interne ID we de fout opzoeken in onze systemen kunnen gebruiken

    -   Voorbeeld: **71a2f329-ca29-402f-aa72-bc00a7aca603**

-   **UPN** – de gebruiker die de bewerking uitgevoerd

    -   Voorbeeld:**tperkins@f128.info**

-   **Tenant-Id** – de unieke ID van de tenant die de gebruiker die de bewerking uitgevoerd lid van was

    -   Voorbeeld: **7918d4b5-0442-4a97-be2d-36f9f9962ece**

-   **Gebruikersobject-Id** – de unieke ID van de gebruiker die de bewerking uitgevoerd

    -   Voorbeeld: **17f84be4-51f8-483a-b533-383791227a99**

### <a name="detailed-notification-items"></a>Gedetailleerde melding Items

-   **Weergavenaam** – **(kan niet leeg zijn)** een meer gedetailleerde weergavenaam voor de fout

    -   Voorbeeld * – **Application proxy-instellingen**

-   **Status** – de specifieke status van de melding

    -   Voorbeeld * – **is mislukt**

-   **Object-Id** – **(kan niet leeg zijn)** de object-ID op basis waarvan de bewerking is uitgevoerd

    -   Voorbeeld: **8e08161d-f2fd-40ad-a34a-a9632d6bb599**

-   **Details** -het gedetailleerde beschrijving van wat is het gevolg van de bewerking

    -   Voorbeeld: **interne url 'http://bing.com/' is ongeldig omdat het zich al in gebruik**

-   **Fout kopiëren** : klik op de **pictogram kopiëren** rechts van de **Kopieerfout** tekstvak voor het kopiëren van de Meldingsdetails te delen met een groep engineer van ondersteuning of productupdates

    -   Voorbeeld:```{"errorCode":"InternalUrl\_Duplicate","localizedErrorDetails":{"errorDetail":"Internal url 'http://google.com/' is invalid since it is already in use"},"operationResults":\[{"objectId":null,"displayName":null,"status":0,"details":"Internal url 'http://bing.com/' is invalid since it is already in use"}\],"timeStampUtc":"2017-03-23T19:50:26.465743Z","clientRequestId":"302fd775-3329-4670-a9f3-bea37004f0bb","internalTransactionId":"ea5b5475-03b9-4f08-8e95-bbb11289ab65","upn":"tperkins@f128.info","tenantId":"7918d4b5-0442-4a97-be2d-36f9f9962ece","userObjectId":"17f84be4-51f8-483a-b533-383791227a99"}```

## <a name="next-steps"></a>Volgende stappen
[Geef één aanmelding bij uw apps met toepassingsproxy](active-directory-application-proxy-sso-using-kcd.md)

