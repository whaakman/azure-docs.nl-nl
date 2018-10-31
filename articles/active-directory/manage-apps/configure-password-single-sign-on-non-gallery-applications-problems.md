---
title: Probleem bij het configureren wachtwoord eenmalige aanmelding voor een toepassing buiten de galerie | Microsoft Docs
description: Inzicht in het gezicht voor mensen van algemene problemen bij het configureren van wachtwoord Single Sign-on voor aangepaste niet-galerietoepassingen die niet worden vermeld in de Azure AD-Toepassingsgalerie
services: active-directory
documentationcenter: ''
author: barbkess
manager: mtillman
ms.assetid: ''
ms.service: active-directory
ms.component: app-mgmt
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 07/11/2017
ms.author: barbkess
ms.openlocfilehash: 0d069490f2e5b495b417b5a4d511f02fa7891ea9
ms.sourcegitcommit: dbfd977100b22699823ad8bf03e0b75e9796615f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/30/2018
ms.locfileid: "50240100"
---
# <a name="problem-configuring-password-single-sign-on-for-a-non-gallery-application"></a>Probleem bij het configureren wachtwoord eenmalige aanmelding voor een toepassing buiten de galerie

In dit artikel kunt u inzicht krijgt in het gezicht voor mensen van algemene problemen bij het configureren van **eenmalige aanmelding wachtwoord** met een toepassing buiten de galerie.

## <a name="how-to-capture-sign-in-fields-for-an-application"></a>Aanmeldingsvelden voor een toepassing vastleggen

Aanmeldingsvelden vastleggen wordt alleen ondersteund voor HTML-functionaliteit aanmeldingspagina's en is **niet ondersteund voor niet-standaard aanmeldingspagina's**, zoals degenen die gebruikmaken van Flash of andere technologieën voor niet-HTML-functionaliteit.

Er zijn twee manieren kunt u aanmeldingsvelden vastleggen voor uw aangepaste toepassingen:

-   Automatische aanmeldingsvelden vastleggen

-   Handmatige aanmeldingsvelden vastleggen

**Automatische aanmeldingsvelden vastleggen** werkt goed samen met de meeste ingeschakeld HTML aanmeldingspagina's, als ze met **bekende div-element-id's voor de gebruikersnaam en wachtwoord invoeren** veld. De manier waarop die dit werkt, is door de HTML-code op de pagina om te zoeken div-element-id's die voldoen aan bepaalde criteria slijmen en metagegevens voor deze toepassing voor de replay van wachtwoorden naar het later opnieuw op te slaan.

**Handmatige aanmeldingsvelden vastleggen** kan worden gebruikt in het geval dat de toepassing **leverancier heeft geen label** de velden die worden gebruikt voor aanmelding bij. Handmatige aanmeldingsvelden vastleggen kan ook worden gebruikt in het geval wanneer de **leverancier rendert meerdere velden** die kan niet worden automatisch gedetecteerd. Azure AD kan opslaan van gegevens voor velden die op de pagina aanmelden, zolang u laat ons weten waar deze velden zijn op de pagina.

In het algemeen **als automatische aanmeldingsvelden vastleggen niet werkt, probeert u de handmatige optie.**

### <a name="how-to-automatically-capture-sign-in-fields-for-an-application"></a>Automatisch vastleggen aanmeldingsvelden voor een toepassing

Het configureren van **wachtwoord gebaseerde Single Sign-on** voor het gebruik van een toepassing **automatische aanmeldingsvelden vastleggen**, volg de onderstaande stappen:

1.  Open de [ **Azure-portal** ](https://portal.azure.com/) en meld u aan als een **hoofdbeheerder** of **Co-beheerder.**

2.  Open de **Azure Active Directory-extensie** door te klikken op **alle services** aan de bovenkant van het menu links hoofdgedeelte voor navigatie.

3.  Typ in **' Azure Active Directory**' in het zoekvak van filter en selecteer de **Azure Active Directory** item.

4.  Klik op **bedrijfstoepassingen** in het navigatiemenu aan Azure Active Directory.

5.  Klik op **alle toepassingen** om een lijst met al uw toepassingen weer te geven.

  * Als u de toepassing die u wilt weergeven die hier niet ziet, gebruikt u de **Filter** besturingselement aan de bovenkant van de **lijst met alle toepassingen** en stel de **weergeven** optie naar **alle Toepassingen.**

6.  Selecteer de toepassing die u wilt configureren van eenmalige aanmelding.

7.  Nadat de toepassing wordt geladen, klikt u op de **eenmalige aanmelding** in het navigatiemenu aan de van de toepassing.

8.  Selecteer de modus **wachtwoord gebaseerde aanmelding.**

9.  Voer de **aanmeldings-URL**, de URL waar gebruikers hun gebruikersnaam en wachtwoord aan te melden bij invoeren. **Zorg ervoor dat de aanmelding-velden worden weergegeven op de URL die u opgeeft**.

10. Klik op de knop **Opslaan**.

11. Zodra u dat doen, die URL is automatisch die de is geëxtraheerd gevraagd een gebruikersnaam en wachtwoord invoervak en kunt u Azure AD gebruiken voor het veilig verzenden van wachtwoorden bij die toepassing met behulp van het deelvenster browser-extensie voor toegang.

## <a name="how-to-manually-capture-sign-in-fields-for-an-application"></a>Handmatig vastleggen aanmeldingsvelden voor een toepassing

Voor het handmatig aanmeldingsvelden vastleggen, moet u eerst de toegang tot deelvenster Browser-extensie geïnstalleerd hebben en **niet worden uitgevoerd in de inPrivate-, incognito- of persoonlijke modus.** Volg de stappen in de om browserextensie te installeren, de [het installeren van de extensie toegang deelvenster Browser](#i-cannot-manually-detect-sign-in-fields-for-my-application) sectie.

Het configureren van **wachtwoord gebaseerde Single Sign-on** voor het gebruik van een toepassing **handmatige aanmeldingsvelden vastleggen**, volg de onderstaande stappen:

1.  Open de [ **Azure-portal** ](https://portal.azure.com/) en meld u aan als een **hoofdbeheerder** of **Co-beheerder.**

2.  Open de **Azure Active Directory-extensie** door te klikken op **alle services** aan de bovenkant van het menu links hoofdgedeelte voor navigatie.

3.  Typ in **' Azure Active Directory**' in het zoekvak van filter en selecteer de **Azure Active Directory** item.

4.  Klik op **bedrijfstoepassingen** in het navigatiemenu aan Azure Active Directory.

5.  Klik op **alle toepassingen** om een lijst met al uw toepassingen weer te geven.

   * Als u de toepassing die u wilt weergeven die hier niet ziet, gebruikt u de **Filter** besturingselement aan de bovenkant van de **lijst met alle toepassingen** en stel de **weergeven** optie naar **alle Toepassingen.**

6.  Selecteer de toepassing die u wilt configureren van eenmalige aanmelding.

7.  Nadat de toepassing wordt geladen, klikt u op de **eenmalige aanmelding** in het navigatiemenu aan de van de toepassing.

8.  Selecteer de modus **wachtwoord gebaseerde aanmelding.**

9.  Voer de **aanmeldings-URL**, de URL waar gebruikers hun gebruikersnaam en wachtwoord aan te melden bij invoeren. **Zorg ervoor dat de aanmelding-velden worden weergegeven op de URL die u opgeeft**.

10. Klik op de knop **Opslaan**.

11. Zodra u dat doen, die URL is automatisch die de is geëxtraheerd gevraagd een gebruikersnaam en wachtwoord invoervak en kunt u Azure AD gebruiken voor het veilig verzenden van wachtwoorden bij die toepassing met behulp van het deelvenster browser-extensie voor toegang. In het geval van storing optreedt, kunt u **wijzigen van de aanmelding in de modus voor het gebruik van handmatige aanmeldingsvelden vastleggen** voort te zetten naar stap 12.

12. Klik op **configureren &lt;appname&gt; wachtwoord Single Sign-on instellingen**.

13. Selecteer de **aanmeldingsvelden handmatig detecteren** configuratie-optie.

14. Klik op **OK**.

15. Klik op **Opslaan**.

16. Volg de instructies op het scherm het toegangsvenster gebruiken.

## <a name="i-see-a-we-couldnt-find-any-sign-in-fields-at-that-url-error"></a>Ik zie een 'Niet vinden alle velden aanmelden via deze URL'-fout

U kunt deze fout ziet wanneer automatische detectie van aanmeldingsvelden mislukt. Los het probleem, probeert u detectie van handmatige aanmelding veld met de volgende stappen in de [handmatig vastleggen aanmeldingsvelden voor een toepassing](#how-to-manually-capture-sign-in-fields-for-an-application) sectie.

## <a name="i-see-an-unable-to-save-single-sign-on-configuration-error"></a>Ik zie een 'kan niet op te slaan van de configuratie van eenmalige aanmelding' fout

In sommige zeldzame gevallen mislukken het bijwerken van de configuratie voor eenmalige aanmelding. Probeer om op te lossen, op te slaan van de eenmalige aanmelding configuratie opnieuw uit.

Als de pogingen mislukken consistent blijven, een ondersteuningsaanvraag openen en geef de informatie die verzameld de [hoe ziet u de details van een portalmelding](#i-cannot-manually-detect-sign-in-fields-for-my-application) en [hulp krijgen door de details van melding verzenden naar een ondersteuning engineer](#how-to-get-help-by-sending-notification-details-to-a-support-engineer) secties.

## <a name="i-cannot-manually-detect-sign-in-fields-for-my-application"></a>Ik kan geen aanmeldingsvelden handmatig detecteren voor mijn toepassing

De problemen die u tegenkomen kunt wanneer handmatige detectie niet werkt onder andere:

-   Het handmatig vastleggen leek te werken, maar de velden die zijn vastgelegd zijn niet correct

-   De juiste velden ophalen niet gemarkeerd bij het uitvoeren van het vastleggen

-   Het vastleggen, ga ik naar de aanmeldingspagina van de toepassing zoals verwacht, maar er gebeurt niets

-   Handmatige vastleggen lijkt te werken, maar SSO gebeurt niet wanneer mijn gebruikers naar de toepassing vanuit het toegangsvenster navigeren.

Controleer het volgende als u een van deze problemen optreden:

-   Zorg ervoor dat u de nieuwste versie van de uitbreiding voor toegang tot deelvenster browser hebt **geïnstalleerd** en **ingeschakeld** met de volgende stappen in de [het installeren van de Browseruitbreiding van toegang deelvenster](#how-to-install-the-access-panel-browser-extension) sectie.

-   Zorg ervoor dat u de capture-proces niet probeert terwijl uw browser in **incognito, InPrivate- of persoonlijke modus**. De extensie van het Configuratiescherm toegang wordt niet ondersteund in deze modi.

-   Zorg ervoor dat uw gebruikers niet wilt aanmelden bij de toepassing van het toegangsvenster, terwijl in **incognito, InPrivate- of persoonlijke modus**. De extensie van het Configuratiescherm toegang wordt niet ondersteund in deze modi.

-   Probeer het handmatige proces voor vastleggen, ervoor te zorgen dat de rode markeringen zijn over de juiste velden.

-   Als het handmatig vastleggen lijkt te reageren of de aanmeldingspagina doet het nog (geval 3 hierboven), het handmatig vastleggen opnieuw proberen. Maar deze keer na het voltooien van het proces, drukt u op de **F12** knop voor het openen van uw browser developer-console. Zodra er, open de **console** en het type **window.location= "&lt;u hebt opgegeven bij het configureren van de app url invoeren&gt;"** en druk vervolgens op **Enter** . Dit zorgt ervoor dat de omleiding van een pagina die de capture-proces wordt beëindigd en slaat de velden die zijn vastgelegd.

Als geen van deze methoden voor u werkt, kunt ondersteuning. Een ondersteuningsaanvraag openen met de details van wat u hebt geprobeerd, evenals de informatie die verzameld de [hoe ziet u de details van een portalmelding](#i-cannot-manually-detect-sign-in-fields-for-my-application) en [hulp krijgen door de details van melding verzenden naar een ondersteuningsmedewerker ](#how-to-get-help-by-sending-notification-details-to-a-support-engineer) secties (indien van toepassing).

## <a name="how-to-install-the-access-panel-browser-extension"></a>Het installeren van de extensie toegang deelvenster Browser

Voor het installeren van de Browseruitbreiding van toegang deelvenster de volgende stappen uit te voeren:

1.  Open de [Toegangsvenster](https://myapps.microsoft.com) in een van de ondersteunde browsers en meld u aan als een **gebruiker** in uw Azure AD.

2.  Klik op een **wachtwoord-SSO-toepassing** in het toegangsvenster.

3.  Selecteer in de vraag of de software te installeren, **nu installeren**.

4.  Op basis van uw browser u omgeleid naar de downloadkoppeling. **Voeg** de uitbreiding van uw browser.

5.  Als hierom wordt gevraagd, selecteert u op een **inschakelen** of **toestaan** de extensie.

6.  Nadat deze is geïnstalleerd, **opnieuw** uw browsersessie.

7.  Meld u aan in het toegangsvenster en zien als u kunt **starten** uw wachtwoord-SSO-toepassingen.

U kunt ook de extensie voor Chrome en Firefox downloaden via de rechtstreekse koppelingen hieronder:

-   [Chrome-extensie voor toegang deelvenster](https://chrome.google.com/webstore/detail/access-panel-extension/ggjhpefgjjfobnfoldnjipclpcfbgbhl)

-   [Uitbreiding van het Configuratiescherm Firefox-toegang](https://addons.mozilla.org/firefox/addon/access-panel-extension/)

## <a name="how-to-see-the-details-of-a-portal-notification"></a>Hoe ziet u de details van een portalmelding

U kunt de details van een portalmelding zien door de onderstaande stappen te volgen:

1.  Klik op de **meldingen** pictogram (de klok) in de rechterbovenhoek van de Azure-portal

2.  Selecteer een bericht in een **fout** status (die met een rode (!) ernaast).

  >! Houd er rekening mee] u kunt niet klikken meldingen in een **geslaagd** of **In voortgang** staat.
  >
  >

3.  De **Meldingsdetails** deelvenster wordt geopend.

4.  Gebruik de informatie zelf om meer informatie over het probleem te begrijpen.

5.  Als u nog steeds hulp nodig hebt, kunt u ook de informatie delen met een ondersteuningsmedewerker of de productgroep voor hulp bij het probleem.

6.  Klik op de **kopie** **pictogram** aan de rechterkant van de **fout kopiëren** tekstvak kopiëren alle details van de melding om te delen met een medewerker van de groep ondersteuning of product.

## <a name="how-to-get-help-by-sending-notification-details-to-a-support-engineer"></a>Hulp krijgen door de details van melding verzenden naar een ondersteuningsmedewerker

Het is heel belangrijk dat u deelt **alle onderstaande details** met de ondersteuningstechnicus als u hulp nodig hebt, zodat deze u snel kunnen ondersteunen. U kunt **maken van een Beeldschermopname** of klik op de **pictogram voor kopiëren-fout**, gevonden aan de rechterkant van de **fout kopiëren** tekstvak.

## <a name="notification-details-explained"></a>Details van melding uitgelegd

De hieronder wordt uitgelegd meer wat elk van de melding betekent dat items, en worden voorbeelden gegeven van elk van deze.

### <a name="essential-notification-items"></a>Essentiële kennisgeving Items

-   **Titel** – de beschrijvende titel van de melding

    -   Voorbeeld: **Application proxy-instellingen**

-   **Beschrijving** : de beschrijving van wat is het gevolg van de bewerking

    -   Voorbeeld: **opgegeven interne url wordt al gebruikt door een andere toepassing**

-   **Meldings-ID** : de unieke id van de melding

    -   Voorbeeld: **clientNotification-2adbfc06-2073-4678-a69f-7eb78d96b068**

-   **Clientaanvraag-ID** – de specifieke aanvraag-id die is gemaakt door uw browser

    -   Voorbeeld: **302fd775-3329-4670-a9f3-bea37004f0bc**

-   **Tijd stempel UTC** – de tijdstempel gedurende welke de melding is opgetreden, in UTC

    -   Voorbeeld: **2017-03-23T19:50:43.7583681Z**

-   **Interne transactie-ID** : de interne ID gebruikt om te zoeken de fout in onze systemen

    -   Voorbeeld: **71a2f329-ca29-402f-aa72-bc00a7aca603**

-   **UPN** – de gebruiker die de bewerking heeft uitgevoerd

    -   Voorbeeld: **tperkins@f128.info**

-   **Tenant-ID** : de unieke ID van de tenant die de gebruiker die de bewerking heeft uitgevoerd, een lid van is

    -   Voorbeeld: **7918d4b5-0442-4a97-be2d-36f9f9962ece**

-   **Object-ID gebruiker** : de unieke ID van de gebruiker die de bewerking heeft uitgevoerd

    -   Voorbeeld: **17f84be4-51f8-483a-b533-383791227a99**

### <a name="detailed-notification-items"></a>Gedetailleerde melding Items

-   **Weergavenaam** – **(mag leeg zijn)** een meer gedetailleerde weergavenaam voor de fout

    -   Voorbeeld van de * – **Application proxy-instellingen**

-   **Status** – de specifieke status van de melding

    -   Voorbeeld van de * – **is mislukt**

-   **Object-ID** – **(mag leeg zijn)** de object-ID op basis waarvan de bewerking is uitgevoerd

    -   Voorbeeld: **8e08161d-f2fd-40ad-a34a-a9632d6bb599**

-   **Details** : de gedetailleerde beschrijving van wat is het gevolg van de bewerking

    -   Voorbeeld: **interne url 'http://bing.com/' is ongeldig omdat deze al in gebruik**

-   **Fout kopiëren** : klik op de **kopieerpictogram** aan de rechterkant van de **fout kopiëren** tekstvak kopiëren alle details van de melding om te delen met een groep ondersteuning of product engineer

    -   Voorbeeld: ```{"errorCode":"InternalUrl\_Duplicate","localizedErrorDetails":{"errorDetail":"Internal url 'http://google.com/' is invalid since it is already in use"},"operationResults":\[{"objectId":null,"displayName":null,"status":0,"details":"Internal url 'http://bing.com/' is invalid since it is already in use"}\],"timeStampUtc":"2017-03-23T19:50:26.465743Z","clientRequestId":"302fd775-3329-4670-a9f3-bea37004f0bb","internalTransactionId":"ea5b5475-03b9-4f08-8e95-bbb11289ab65","upn":"tperkins@f128.info","tenantId":"7918d4b5-0442-4a97-be2d-36f9f9962ece","userObjectId":"17f84be4-51f8-483a-b533-383791227a99"}```

## <a name="next-steps"></a>Volgende stappen
[Geef single sign-on bij uw apps met Application Proxy](application-proxy-configure-single-sign-on-with-kcd.md)

