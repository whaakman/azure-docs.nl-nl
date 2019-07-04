---
title: Problemen bij het configureren van SSO-wachtwoord voor een toepassing buiten de galerie | Microsoft Docs
description: Veelvoorkomende problemen die optreden bij het configureren van wachtwoord eenmalige aanmelding (SSO) voor aangepaste apps die zich niet in de Azure AD-toepassingsgalerie.
services: active-directory
documentationcenter: ''
author: CelesteDG
manager: mtillman
ms.assetid: ''
ms.service: active-directory
ms.subservice: app-mgmt
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 07/11/2017
ms.author: celested
ms.collection: M365-identity-device-management
ms.openlocfilehash: 24330dc874173ba1c6f15abb7b4caf9f23e2e00c
ms.sourcegitcommit: f56b267b11f23ac8f6284bb662b38c7a8336e99b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/28/2019
ms.locfileid: "67440354"
---
# <a name="problems-configuring-password-single-sign-on-for-a-non-gallery-application"></a>Problemen met het wachtwoord eenmalige aanmelding voor een toepassing buiten de galerie configureren

In dit artikel beschrijft bekende problemen die optreden kunnen bij het configureren van *eenmalige aanmelding wachtwoord* (SSO) voor een app buiten de galerie.

## <a name="capture-sign-in-fields-for-an-app"></a>Aanmeldingsvelden voor een app vastleggen

Aanmeldingsvelden vastleggen wordt alleen ondersteund voor HTML-functionaliteit aanmeldingspagina's. Het is niet ondersteund voor niet-standaard aanmeldingspagina's, zoals die die gebruikmaken van Adobe Flash of andere niet-HTML-technologieën.

Er zijn twee manieren om vast te leggen van de aanmeldingsvelden voor uw aangepaste apps:

- **Automatische aanmeldingsvelden vastleggen** werkt goed samen met de meeste ingeschakeld HTML aanmeldingspagina's, *als ze bekende div-element-id's gebruiken* voor de velden van de gebruiker en wachtwoord. De HTML-code op de pagina is die is geëxtraheerd om te zoeken div-element-id's die aan bepaalde criteria voldoen. Metagegevens worden opgeslagen zodat deze kan opnieuw worden afgespeeld in de app later opnieuw.

- **Handmatige aanmeldingsvelden vastleggen** wordt gebruikt als de leverancier van de app *de invoervelden aanmelden niet label*. Handmatige vastleggen wordt ook gebruikt als de leverancier van de *Hiermee maakt u meerdere velden die niet kunnen automatisch gedetecteerd worden*. Azure Active Directory (Azure AD) kunt opslaan van gegevens voor velden die er zijn op de pagina aanmelden als u weten waar deze velden zijn op de pagina.

Als automatische aanmeldingsvelden vastleggen niet in het algemeen werkt, probeert de handmatige optie.

### <a name="automatically-capture-sign-in-fields-for-an-app"></a>Aanmeldingsvelden voor een app automatisch vastleggen

Voor het configureren van eenmalige aanmelding op basis van wachtwoorden met behulp van automatische aanmeldingsvelden vastleggen, de volgende stappen uit:

1. Open de [Azure Portal](https://portal.azure.com/). Meld u aan als een globale beheerder of co-beheerder.

2. Selecteer in het navigatiedeelvenster aan de linkerkant **alle services** openen van de Azure AD-extensie.

3. Type **Azure Active Directory** in het zoekvak van filter en selecteer vervolgens **Azure Active Directory**.

4. Selecteer **bedrijfstoepassingen** in het navigatievenster van de Azure AD.

5. Selecteer **alle toepassingen** om een lijst van uw apps weer te geven.

   > [!NOTE]
   > Als u de app die u wilt niet ziet, gebruikt u de **Filter** besturingselement aan de bovenkant van de **alle toepassingen** lijst. Stel de **weergeven** optie "Alle toepassingen."

6. Selecteer de app die u wilt configureren voor eenmalige aanmelding.

7. Nadat de app wordt geladen, selecteert u **eenmalige aanmelding** in het navigatiedeelvenster aan de linkerkant.

8. Selecteer **op basis van wachtwoorden Sign-on** modus.

9. Voer de **aanmeldings-URL**, dit is de URL van de pagina waar gebruikers hun gebruikersnaam en wachtwoord invoeren. *Zorg ervoor dat de aanmeldingsvelden voor de URL die u opgeeft op de pagina zichtbaar zijn*.

10. Selecteer **Opslaan**.

    Voor de naam en het wachtwoord invoervakken voor de gebruiker wordt automatisch de pagina die is geëxtraheerd. U kunt nu Azure AD veilig wachtwoorden van de App te verzenden met behulp van de Browseruitbreiding van het toegangsvenster gebruiken.

### <a name="manually-capture-sign-in-fields-for-an-app"></a>Handmatig vastleggen aanmeldingsvelden voor een app

Voor het handmatig aanmeldingsvelden vastleggen, moet u de uitbreiding van het toegangsvenster browser geïnstalleerd hebben. Bovendien uw browser kan niet worden uitgevoerd in de *InPrivate-navigatie*, *incognito*, of *persoonlijke* modus.

Voor het installeren van de extensie, Zie de [installeren van de extensie toegang deelvenster Browser](#install-the-access-panel-browser-extension) sectie van dit artikel.

Voor het configureren van eenmalige aanmelding op basis van wachtwoorden voor een app met behulp van handmatige aanmeldingsvelden vastleggen, de volgende stappen uit:

1. Open de [Azure Portal](https://portal.azure.com/). Meld u aan als een globale beheerder of co-beheerder.

2. Selecteer in het navigatiedeelvenster aan de linkerkant **alle services** openen van de Azure AD-extensie.

3. Type **Azure Active Directory** in het zoekvak van filter en selecteer vervolgens **Azure Active Directory**.

4. Selecteer **bedrijfstoepassingen** in het navigatievenster van de Azure AD.

5. Selecteer **alle toepassingen** om een lijst van uw apps weer te geven.

   > [!NOTE] 
   > Als u de app die u wilt niet ziet, gebruikt u de **Filter** besturingselement aan de bovenkant van de **alle toepassingen** lijst. Stel de **weergeven** optie "Alle toepassingen."

6. Selecteer de app die u wilt configureren voor eenmalige aanmelding.

7. Nadat de app wordt geladen, selecteert u **eenmalige aanmelding** in het navigatiedeelvenster aan de linkerkant.

8. Selecteer **op basis van wachtwoorden Sign-on** modus.

9. Voer de **aanmeldings-URL**, dit is de pagina waar gebruikers hun gebruikersnaam en wachtwoord invoeren. *Zorg ervoor dat de aanmeldingsvelden voor de URL die u opgeeft op de pagina zichtbaar zijn*.

10. Selecteer **configureren *&lt;appname&gt;* wachtwoord Single Sign-on instellingen**.

11. Selecteer **aanmeldingsvelden handmatig detecteren**.

14. Selecteer **OK**.

15. Selecteer **Opslaan**.

16. Volg de instructies voor het toegangsvenster gebruiken.

## <a name="troubleshoot-problems"></a>Problemen oplossen

### <a name="i-get-a-we-couldnt-find-any-sign-in-fields-at-that-url-error"></a>Er verschijnt een foutbericht 'Niet vinden alle velden aanmelden via deze URL'

U krijgt dit foutbericht wordt weergegeven wanneer automatische detectie van aanmeldingsvelden is mislukt. Los het probleem, probeert u handmatige aanmeldingsvelden detectie. Zie de [handmatig vastleggen aanmeldingsvelden voor een toepassing](#manually-capture-sign-in-fields-for-an-app) sectie van dit artikel.

### <a name="i-get-an-unable-to-save-single-sign-on-configuration-error"></a>Er verschijnt een "kan geen configuratie voor eenmalige aanmelding opslaan' fout

Zelden, mislukt de SSO-configuratie bijwerken. U lost dit probleem, probeer het opslaan van de configuratie opnieuw uit.

Als u steeds de fout, kunt u een ondersteuningsaanvraag openen. Bevat de informatie die wordt beschreven in de [portalmelding details weergeven](#view-portal-notification-details) en [details van melding verzenden naar een ondersteuningsmedewerker om hulp te krijgen](#send-notification-details-to-a-support-engineer-to-get-help) secties van dit artikel.

### <a name="i-cant-manually-detect-sign-in-fields-for-my-app"></a>Ik kan geen aanmeldingsvelden handmatig detecteren voor mijn app

U merkt wellicht de volgende gedragingen wanneer handmatige detectie niet werkt:

- Het handmatig vastleggen leek te werken, maar de vastgelegde velden niet juist zijn.

- De juiste velden ophalen niet gemarkeerd wanneer het vastleggen wordt uitgevoerd.

- Het vastleggen gaat u naar de aanmeldingspagina van de app zoals verwacht, maar er gebeurt niets.

- Handmatige vastleggen lijkt te werken, maar SSO gebeurt niet wanneer gebruikers Navigeer naar de app in Toegangsvenster.

Als u een van deze problemen ondervindt, kunt u het volgende doen:

- Zorg ervoor dat u de nieuwste versie van de Browseruitbreiding van het toegangsvenster hebt *geïnstalleerd en ingeschakeld*. Zie de [installeren van de Browseruitbreiding van het toegangsvenster](#install-the-access-panel-browser-extension) sectie van dit artikel.

- Zorg ervoor dat uw browser niet *incognito*, *InPrivate-navigatie*, of *persoonlijke* modus tijdens het vastleggen. De extensie Toegangsvenster wordt niet ondersteund in deze modi.

- Zorg ervoor dat uw gebruikers zijn niet wilt aanmelden bij de app vanuit Toegangsvenster tijdens in *incognito*, *InPrivate-navigatie*, of *privémodus*.

- Probeer het opnieuw het handmatig vastleggen. Zorg ervoor dat de rode markeringen over de juiste velden zijn.

- Als het handmatig vastleggen lijkt niet meer reageert of de aanmeldingspagina niet reageert, kunt u het vastleggen van handmatige proces opnieuw. Maar deze keer nadat het proces is voltooid, drukt u op de F12-toets om van uw browser developer-console te openen. Selecteer de **console** tabblad. Type **window.location= " *&lt;de aanmelding-URL die u hebt opgegeven bij het configureren van de app&gt;* "** , en druk op Enter. Dit zorgt ervoor dat de omleiding van een pagina die de capture-proces wordt beëindigd en slaat de velden die zijn vastgelegd.

### <a name="contact-support"></a>Neem contact op met ondersteuning

Als u nog steeds problemen ondervindt, kunt u een aanvraag opent bij Microsoft Support. Beschrijf wat u hebt geprobeerd. De gegevens die worden beschreven in de [portalmelding details weergeven](#view-portal-notification-details) en [details van melding verzenden naar een ondersteuningsmedewerker om hulp te krijgen](#send-notification-details-to-a-support-engineer-to-get-help) secties van dit artikel (indien van toepassing).

## <a name="install-the-access-panel-browser-extension"></a>Installeer de extensie van de browser Toegangsvenster

Volg deze stappen:

1. Open [Toegangsvenster](https://myapps.microsoft.com) in een ondersteunde browser. Aanmelden bij Azure AD als een *gebruiker*.

2. Selecteer **wachtwoord-SSO-toepassing** in Toegangsvenster.

3. Wanneer u wordt gevraagd om de software te installeren, selecteert u **nu installeren**.

4. U wordt omgeleid naar een downloadpagina voor uw browser. Kies aan **toevoegen** de extensie.

5. Als u wordt gevraagd, selecteert u **inschakelen** of **toestaan**.

6. Na de installatie opnieuw opstarten van uw browser.

7. Meld u aan bij het toegangsvenster voor. Zie als u uw wachtwoord-SSO-functionaliteit geschikte apps kunt openen.

U kunt ook rechtstreeks de browserextensie voor Chrome en Firefox downloaden via deze koppelingen:

-   [Chrome Toegangsvenster-extensie](https://chrome.google.com/webstore/detail/access-panel-extension/ggjhpefgjjfobnfoldnjipclpcfbgbhl)

-   [Toegangsvenster Firefox-extensie](https://addons.mozilla.org/firefox/addon/access-panel-extension/)

## <a name="view-portal-notification-details"></a>Details van de portalmelding weergeven

Als u wilt zien van de details van een portalmelding, de volgende stappen uit:

1. Selecteer de **meldingen** pictogram (de klok) in de rechterbovenhoek van de Azure-portal.

2. Selecteer geen melding waarin een *fout* staat. (Ze hebben een rode "!".)

   > [!NOTE]
   > U kunt geen selecteren meldingen die zich in de *geslaagd* of *In voortgang* staat.

3. De **Meldingsdetails** deelvenster wordt geopend. Lees de informatie voor meer informatie over het probleem.

5. Als u nog steeds hulp nodig hebt, kunt u de informatie delen met een ondersteuningsmedewerker of de productgroep. Selecteer de **kopie** pictogram aan de rechterkant van de **fout kopiëren** in om te kopiëren van de details van de melding om te delen.

## <a name="send-notification-details-to-a-support-engineer-to-get-help"></a>Details van melding verzenden naar een ondersteuningsmedewerker om hulp te krijgen

Het is belangrijk dat u deelt *alle* de details die worden vermeld in deze sectie met ondersteuning, zodat ze helpen u snel kunnen. Als u wilt opnemen, kunt u een schermopname maken of selecteren **fout kopiëren**.

De volgende informatie wordt uitgelegd wat elk item melding betekent en bevat voorbeelden.

### <a name="essential-notification-items"></a>Essentiële kennisgeving items

- **Titel**: de beschrijvende titel van de melding.

   Voorbeeld: *Instellingen van de toepassingsproxy*

- **Beschrijving**: Wat is het gevolg van de bewerking.

   Voorbeeld: *De opgegeven interne URL wordt al gebruikt door een andere toepassing.*

- **Meldings-ID**: de unieke ID van de melding.

    Voorbeeld: *clientNotification-2adbfc06-2073-4678-a69f-7eb78d96b068*

- **Clientaanvraag-ID**: de specifieke aanvraag-ID die uw browser.

    Voorbeeld: *302fd775-3329-4670-a9f3-bea37004f0bc*

- **Tijd stempel UTC**: de timestamp van wanneer de melding is opgetreden, in UTC.

    Voorbeeld: *2017-03-23T19:50:43.7583681Z*

- **Interne transactie-ID**: de interne ID die wordt gebruikt om te controleren of de fout in onze systemen.

    Voorbeeld: **71a2f329-ca29-402f-aa72-bc00a7aca603**

- **UPN**: De gebruiker die de bewerking is uitgevoerd.

    Voorbeeld: *tperkins\@f128.info*

- **Tenant-ID**: de unieke ID van de tenant die de gebruiker die de bewerking is een lid van is.

    Voorbeeld: *7918d4b5-0442-4a97-be2d-36f9f9962ece*

- **Object-ID gebruiker**: De unieke ID van de gebruiker die de bewerking is uitgevoerd.

    Voorbeeld: *17f84be4-51f8-483a-b533-383791227a99*

### <a name="detailed-notification-items"></a>Gedetailleerde melding items

- **Weergavenaam**: (mag leeg zijn) een gedetailleerder weergavenaam voor de fout.

    Voorbeeld: *Instellingen van de toepassingsproxy*

- **Status**: de specifieke status van de melding.

    Voorbeeld: *Mislukt*

- **Object-ID**: (mag leeg zijn) de object-ID op waarvoor de bewerking is uitgevoerd.

   Voorbeeld: *8e08161d-f2fd-40ad-a34a-a9632d6bb599*

- **Details**: de gedetailleerde beschrijving van wat is het gevolg van de bewerking.

    Voorbeeld: *Interne url '<https://bing.com/>' is ongeldig omdat deze al in gebruik.*

- **Fout bij kopiëren**: Hiermee kunt u selecteren de **kopieerpictogram** aan de rechterkant van de **fout kopiëren** tekstvak om te kopiëren van de details van de melding om te helpen met ondersteuning.

    Voorbeeld:   ```{"errorCode":"InternalUrl\_Duplicate","localizedErrorDetails":{"errorDetail":"Internal url 'https://google.com/' is invalid since it is already in use"},"operationResults":\[{"objectId":null,"displayName":null,"status":0,"details":"Internal url 'https://bing.com/' is invalid since it is already in use"}\],"timeStampUtc":"2017-03-23T19:50:26.465743Z","clientRequestId":"302fd775-3329-4670-a9f3-bea37004f0bb","internalTransactionId":"ea5b5475-03b9-4f08-8e95-bbb11289ab65","upn":"tperkins@f128.info","tenantId":"7918d4b5-0442-4a97-be2d-36f9f9962ece","userObjectId":"17f84be4-51f8-483a-b533-383791227a99"}```

## <a name="next-steps"></a>Volgende stappen
[Geef single sign-on bij uw apps met Application Proxy](application-proxy-configure-single-sign-on-with-kcd.md)
