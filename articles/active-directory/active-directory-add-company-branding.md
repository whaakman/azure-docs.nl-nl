---
title: "De huisstijl van uw bedrijf toevoegen aan uw aanmeldingspagina en pagina’s met een toegangsvenster in Azure Active Directory"
description: Ontdek hoe u uw huisstijl kunt toevoegen aan de Azure-aanmeldingspagina en aan de toegangsvensterpagina
services: active-directory
documentationcenter: 
author: curtand
manager: femila
editor: 
ms.assetid: f74621b4-4ef0-4899-8c0e-0c20347a8c31
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 04/07/2017
ms.author: curtand
translationtype: Human Translation
ms.sourcegitcommit: 538f282b28e5f43f43bf6ef28af20a4d8daea369
ms.openlocfilehash: 6d4fbfe97288fcb76628b45649b8b678152198a9
ms.lasthandoff: 04/07/2017


---
# <a name="add-company-branding-to-sign-in-and-access-panel-pages"></a>De huisstijl van uw bedrijf toevoegen aan de aanmeldingspagina en de pagina’s met een toegangsvenster
Veel bedrijven willen alle websites en services die ze beheren, een consistent uiterlijk geven. Azure Active Directory biedt hen deze mogelijkheid. IT-professionals kunnen namelijk het uiterlijk van de volgende webpagina’s aanpassen met het bedrijfslogo en afbeeldingen:

* **Aanmeldingspagina** - dit is de pagina die verschijnt wanneer uw werknemers en zakelijke gasten zich aanmelden bij Office 365 of andere toepassingen waarvoor Azure AD wordt gebruikt.
* **Pagina met toegangsvenster** - het toegangsvenster is een portal op internet waarin u de cloudtoepassingen waartoe uw Azure AD-beheerder u toegang heeft verleend, kunt bekijken en starten. Het toegangsvenster is te vinden op: [https://myapps.microsoft.com](https://myapps.microsoft.com).

In dit onderwerp wordt uitgelegd hoe u de aanmeldingspagina en de pagina met het toegangsvenster kunt aanpassen.

> [!NOTE]
> * De huisstijl van uw bedrijf is alleen beschikbaar als u een upgrade hebt uitgevoerd naar de Premium- of Basic-editie van Azure Active Directory of als u een Office 365-licentie hebt. Zie Azure Active Directory-edities voor meer informatie.
> 
> * De Azure Active Directory-edities Premium en Basic zijn beschikbaar voor klanten in China via het wereldwijde exemplaar van Azure Active Directory. De edities Azure Active Directory Premium en Basic worden momenteel niet ondersteund in de Microsoft Azure-service die wordt beheerd door 21Vianet in China. Neem voor meer informatie contact met ons op via het Azure Active Directory-forum.


## <a name="customizing-the-sign-in-page"></a>De aanmeldingspagina aanpassen
Gebruikers maken gewoonlijk gebruik van de Azure AD-aanmeldingspagina wanneer ze toegang proberen te krijgen tot de cloudtoepassingen en - services waarop uw bedrijf is geabonneerd.

Als u wijzigingen hebt aangebracht in de huisstijl van uw aanmeldingspagina, kan het wel een uur duren voordat de wijzigingen zichtbaar worden voor eindgebruikers.

Elementen van de aangepaste huisstijl verschijnen op de aanmeldingspagina van Azure AD wanneer gebruikers een tenant-specifieke URL, zoals https://outlook.com/contoso.com, gebruiken.

Wanneer gebruikers een service gebruiken via een algemene URL, zoals www.office.com, bevat de aanmeldingspagina nog geen huisstijlgegevens omdat binnen het systeem niet bekend is wie de gebruiker is. De huisstijl wordt echter wel weergegeven nadat gebruikers hun gebruikers-id hebben ingevoerd of een gebruikerstegel hebben geselecteerd.

> [!NOTE]
> * Uw domeinnaam moet als Actief worden weergegeven in het gedeelte **Active Directory** > **Directory** > **Domeinen** van de klassieke Azure Portal waarin u de huisstijl hebt geconfigureerd.
> * De huisstijl van de aanmeldingspagina wordt niet meegenomen naar de aanmeldingspagina van persoonlijke Microsoft-accounts. Als uw werknemers of zakelijke gasten zich aanmelden met een persoonlijk Microsoft-account, wordt de huisstijl van uw bedrijf niet weergegeven op hun aanmeldingspagina.
>

In de volgende schermafbeeldingen wordt uitgelegd hoe de aanmeldingspagina's worden aangepast.

### <a name="scenario-1-contoso-employee-goes-to-a-generic-app-url-for-example-wwwofficecom"></a>Scenario 1: Een Contoso-werknemer gaat naar een algemene URL van een app (bijvoorbeeld www.office.com)

In dit voorbeeld meldt een Contoso-gebruiker zich aan bij een mobiele toepassing of webtoepassing met een algemene URL. In de afbeelding aan de linkerkant wordt altijd de app weergegeven en in de afbeelding aan de rechterkant wordt het venster weergegeven dat is bijgewerkt met elementen van de Contoso-huisstijl, waar dat van toepassing is.

![De Office 365-aanmeldingspagina vóór en na de aanpassing][1]

### <a name="scenario-2-contoso-employee-goes-to-contoso-app-thats-restricted-to-internal-users"></a>Scenario 2: Een Contoso-werknemer gaat naar de Contoso-app waarvan de toegang is beperkt tot interne gebruikers

In dit voorbeeld meldt een Contoso-gebruiker zich aan bij een interne toepassing via een bedrijfsspecifieke URL. In de afbeelding aan de linkerkant wordt de huisstijl van het bedrijf (Contoso) weergegeven. Het interactiedeelvenster aan de rechterkant bevat de Contoso-huisstijl en is bedoeld om Contoso-werknemers te helpen bij de aanmeldingsprocedure.

![beperkt toegankelijke aanmeldingspagina voor een app][2]

### <a name="scenario-3-contoso-employee-goes-to-a-contoso-app-thats-open-to-external-users"></a>Scenario 3: Een Contoso-werknemer gaat naar een Contoso-app die ook toegankelijk is voor externe gebruikers

In dit voorbeeld melden gebruikers zich aan bij een LoB-toepassing vanuit Contoso, maar de gebruiker kan een Contoso-werknemer, maar ook een externe gebruiker zijn. In de afbeelding aan de linkerkant wordt de eigenaar van de resource (Contoso) weergegeven, net als in scenario \#2 hierboven. Maar deze keer bevat het interactiedeelvenster aan de rechterkant niet de Contoso-huisstijl om aan te geven dat ook externe gebruikers welkom zijn om zich aan te melden.

![aanmelden bij onbeperkte toegang][3]

### <a name="scenario-4-fabrikam-business-guest-goes-to-contoso-app-thats-open-to-external-users"></a>Scenario 4: Een zakelijke gast van Fabrikam gaat naar de Contoso-app die toegankelijk is voor externe gebruikers

In dit voorbeeld meldt een Contoso-gebruiker zich aan bij een interne toepassing via een bedrijfsspecifieke URL. In de afbeelding aan de linkerkant wordt de huisstijl van het bedrijf (Contoso) weergegeven. Het interactiedeelvenster aan de rechterkant bevat de Contoso-huisstijl en is bedoeld om Contoso-werknemers te helpen bij de aanmeldingsprocedure.

![aanmelden als een externe gebruiker][4]


## <a name="what-elements-on-the-page-can-i-customize"></a>Welke elementen op de pagina kan ik aanpassen?

U kunt de volgende elementen op de aanmeldingspagina aanpassen:

![][5]

| Pagina-element | Locatie op de pagina |
|:--- | --- |
| Logo in banner | Wordt in de rechterbovenhoek van de pagina weergegeven. Vervangt het app-logo zodra de organisatie van de gebruiker is vastgesteld (meestal nadat de gebruikersnaam is opgegeven). |
| Achtergrondafbeelding | Weergegeven als een afbeelding die het hele linkerdeelvenster van de aanmeldingspagina in beslag neemt. Vervangt de afbeelding van de app in aanmeldingsscenario's voor tenants (wanneer gebruikers toegang proberen te krijgen tot een toepassing die is gepubliceerd door hun eigen organisatie of door een organisatie waarvan ze een zakelijke gast zijn).<br>Voor verbindingen met een lage bandbreedte wordt de achtergrondafbeelding vervangen door een achtergrondkleur. Op smalle schermen, zoals die van telefoons, wordt de afbeelding niet weergegeven.<br>De achtergrondafbeelding wordt bijgesneden als gebruikers het formaat van hun browser veranderen. Zorg er bij het ontwerpen van uw achtergrondafbeelding voor dat de belangrijkste visuele elementen in de linkerbovenhoek staan, zodat deze niet uit beeld verdwijnen als de afbeelding wordt bijgesneden. | 
| Selectievakje Aangemeld blijven | Weergegeven onder het vak **Wachtwoord**. |
| Tekst van aanmeldingspagina | Standaardtekst moet worden weergegeven boven de paginavoettekst. Deze kan worden gebruikt om nuttige informatie weer te geven voor gebruikers, zoals het telefoonnummer van de helpdesk of een juridische verklaring. |

> [!NOTE]
> Alle elementen zijn optioneel. Als u bijvoorbeeld een bannerlogo, maar geen afbeelding opgeeft, worden op de aanmeldingspagina uw logo en de afbeelding van de doelsite weergegeven (in dit geval de Office 365-afbeelding van een snelweg in Californië).
>

Op de aanmeldingspagina kunnen gebruikers er met het selectievakje **Aangemeld blijven** voor zorgen dat ze aangemeld blijven wanneer ze hun browser sluiten en opnieuw openen. Deze optie heeft overigens geen invloed op de sessielevensduur.

Of het selectievakje wordt weergegeven, hangt af van wat er bij **KMSI verbergen** is ingesteld.

![Instelling KMSI verbergen][6]

Als u het selectievakje wilt verbergen, stelt u deze instelling in op **Verborgen**.

> [!NOTE]
> Sommige functies van SharePoint Online en Office 2010 zijn alleen beschikbaar als gebruikers dit selectievakje kunnen inschakelen. Als u deze instelling instelt op Verborgen, krijgen uw gebruikers mogelijk extra en onverwachte prompts te zien om zich aan te melden.
>
>

U kunt alle elementen op deze pagina lokaliseren. Wanneer u een standaardset aangepaste elementen hebt geconfigureerd, kunt u meer versies configureren voor verschillende talen. U kunt ook een combinatie van verschillende elementen gebruiken. U kunt bijvoorbeeld:

* Een standaardafbeelding maken die geschikt is voor alle culturen en vervolgens afzonderlijke versies maken voor Engels- en Franstaligen. Wanneer een browser op een van die twee talen wordt ingesteld, wordt de aangepaste afbeelding weergegeven. Bij alle overige talen wordt de standaardafbeelding weergegeven.
* Voor uw organisatie verschillende logo's configureren (bijvoorbeeld een Japanse en een Hebreeuwse versie).

## <a name="access-panel-page-customization"></a>De pagina met het toegangspaneel aanpassen
De pagina met het toegangspaneel is als het ware een portalpagina voor snelle toegang tot de cloud-apps waar de beheerder u toegang toe heeft verleend. Op deze pagina worden uw apps weergegeven als toepassingstegels waarop kan worden geklikt.

In de volgende schermafbeelding ziet u een voorbeeld van een pagina met een toegangspaneel nadat deze is aangepast.

![][8]

## <a name="configure-your-directory-with-company-branding"></a>Uw directory configureren met de huisstijl van uw bedrijf
In de klassieke Azure Portal kunt u per directory één standaardset aanpasbare elementen configureren. Wanneer de standaardset is opgeslagen, kan een beheerder vertaalde versies van elk element toevoegen voor gebruik in andere taalgebieden. Alle aanpasbare elementen zijn optioneel.

Als u bijvoorbeeld een standaardlogo voor in de banner configureert, maar geen grote afbeelding, wordt in de rechterbovenhoek van de aanmeldingspagina uw logo weergegeven. Daarbij wordt de standaardafbeelding van de site weergegeven.

Stel dat de volgende configuratie wordt gebruikt:

* Een standaardlogo voor in de banner en aanmeldingspaginatekst in het Engels
* Een taalspecifieke aanmeldingspaginatekst in het Duits

Als uw voorkeurstaal Duits is, krijgt u het standaardlogo in de banner te zien, maar de Duitse tekst.

Hoewel u technisch gezien een afzonderlijke set zou kunnen configureren voor elke taal die door Azure AD wordt ondersteund, doet u er verstandig aan om het aantal variaties klein te houden. Dit vereenvoudigt het onderhoud en houdt de prestaties goed.
 
**Als u de huisstijl van uw bedrijf aan de directory wilt toevoegen, voert u de volgende stappen uit:**

1. Meld u bij de [klassieke Azure Portal](https://manage.windowsazure.com) aan als beheerder van de directory die u wilt aanpassen.
2. Selecteer de directory die u wilt aanpassen.
3. Klik in de werkbalk bovenaan op **Configureren**.
4. Klik op **Huisstijl aanpassen**.
5. Wijzig de elementen die u wilt aanpassen. Alle velden zijn optioneel.
6. Klik op **Opslaan**.

Het kan een uur duren voordat de wijzigingen die u aan de huisstijl van de aanmeldingspagina hebt aangebracht, worden weergegeven.

**Als u een taalspecifieke huisstijlwijzigingen wilt doorvoeren, volgt u de volgende stappen:**

1. Meld u bij de [klassieke Azure Portal](https://manage.windowsazure.com) aan als beheerder van de directory die u wilt aanpassen.
2. Selecteer de directory die u wilt aanpassen.
fs3. Klik in de werkbalk bovenaan op **Configureren**.
4. Klik op **Huisstijl aanpassen**.
5. Klik op **Huisstijl voor een specifieke taal toevoegen**.
6. Selecteer de taal waarvoor u het logo wilt aanpassen en klik vervolgens op **Volgende**.
7. Bewerk alleen de elementen waarvoor u taalspecifieke overschrijvingen wilt configureren. Alle velden zijn optioneel. Als een veld leeg wordt gelaten, wordt de aangepaste standaardwaarde weergegeven (of de Microsoft-standaard als er geen aangepaste standaard is geconfigureerd).
8. Klik op **Opslaan**.

**Als u de huisstijl van uw bedrijf uit de directory wilt verwijderen, voert u de volgende stappen uit:**

1. Meld u bij de [klassieke Azure Portal](https://manage.windowsazure.com) aan als beheerder van de directory die u wilt aanpassen.
2. Selecteer de directory die u wilt aanpassen.
3. Klik in de werkbalk bovenaan op **Configureren**.
4. Klik op **Huisstijl aanpassen**.
5. Selecteer op de pagina Huisstijl aanpassen de optie **Bestaande huisstijlinstellingen bewerken** en ga naar de volgende pagina.
6. Afhankelijk van de elementen die u wilt verwijderen, voert u een of meer van de volgende acties uit:

    a. Selecteer onder **Logo in banner** de optie **Geüploade logo verwijderen**.

    b. Selecteer onder **Logo in tegel** de optie **Geüploade logo verwijderen**.

    c. Verwijder de tekst uit alle tekstvakken.

    d. Klik op **Volgende**.

    e. Verwijder de tekst uit alle tekstvakken.
7. Klik op **Opslaan** om de elementen te verwijderen.
8. Klik zo nodig opnieuw op **Huisstijl aanpassen** en herhaal deze stappen voor alle taalspecifieke huisstijlonderdelen die moeten worden verwijderd.
    Alle huisstijlinstellingen zijn verwijderd wanneer u op **Huisstijl aanpassen** klikt en er in het formulier **Standaardhuisstijl aanpassen** geen bestaande instellingen zijn geconfigureerd.


## <a name="customizable-elements"></a>Aanpasbare elementen
Bedrijfslogo’s worden gebruikt voor de aanmeldingspagina en de pagina’s met een toegangsvenster, terwijl andere elementen alleen op de aanmeldingspagina gebruikt worden. In de volgende tabel vindt u meer informatie over de verschillende aanpasbare elementen.

| Naam | Beschrijving | Beperkingen | Aanbevelingen |
| --- | --- | --- | --- |
| Bannerlogo |Het bannerlogo wordt weergegeven op de aanmeldingspagina en in het toegangsvenster. |<p>JPG of PNG</p><p>60 x 280 pixels</p><p>10 kB</p> |<p>Gebruik het volledige logo van uw organisatie (inclusief pictogram en logotype)</p><p>Gebruik een hoogte van maximaal 30 pixels om te voorkomen dat er op mobiele apparaten schuifbalken worden weergegeven</p><p>Gebruik een grootte van maximaal 4 kB</p><p>Gebruik een transparant PNG-bestand (ga er niet van uit dat de aanmeldingspagina een witte achtergrond heeft)</p> |
| Tegellogo | Wordt momenteel niet gebruikt |<p>JPG of PNG</p><p>120 x 120 pixels</p><p>10 kB</p> |<p>Houd het eenvoudig (geen kleine tekst), omdat deze afbeelding tot wel 50% kleiner kan worden gemaakt. |
| </p> | | | |
| Label met aanmeldnaam van gebruiker | Wordt momenteel niet gebruikt |<p>Unicodetekst, maximaal 50 tekens</p><p>Alleen tekst zonder opmaak (geen koppelingen of HTML-tags)</p> |<p>Houd het kort en eenvoudig</p><p>Vraag uw gebruikers hoe ze het werk- of schoolaccount waar u ze van voorziet, meestal noemen.</p> |
| Standaardtekst van aanmeldingspagina |Deze standaardtekst wordt onder het formulier van de aanmeldingspagina weergegeven en kan worden gebruikt om aanvullende instructies te geven of om de gebruiker te laten weten waar ze hulp en ondersteuning kunnen krijgen. |<p>Unicodetekst, maximaal 256 tekens</p><p>Alleen tekst zonder opmaak (geen koppelingen of HTML-tags)</p> |Houd het korter dan 250 tekens (ongeveer 3 regels tekst) |
| Achtergrondafbeelding van aanmeldingspagina | Grote afbeelding die aan de linkerkant van de aanmeldingspagina wordt weergegeven (aan de rechterkant voor v.r.n.l.-talen) wanneer gebruikers toegang proberen te krijgen tot tenant-specifieke URL's. |<p>JPG of PNG</p><p>1420 x 1200</p><p>500 kB</p> |<p>1420 x 1200 pixels</p><p>Belangrijk: houd de afbeelding zo klein mogelijk, liefst kleiner dan 200 kB. Als deze afbeelding te groot is, kan dat de prestaties van de aanmeldingspagina beïnvloeden wanneer de afbeelding niet in het cachegeheugen is opgeslagen</p><p>Deze afbeelding wordt vrijwel altijd bijgesneden om te worden aangepast aan verschillende hoogte-breedteverhoudingen van schermen. Plaats de belangrijkste visuele elementen daarom altijd in de linkerbovenhoek.</p> |
| Achtergrondkleur van de aanmeldingspagina | Voor verbindingen met een lage bandbreedte wordt deze effen kleur gebruikt in plaats van de achtergrondafbeelding. | Dit moet een RGB-kleur zijn met een hexadecimale notatie (voorbeeld: \#FFFFFF) | Het is een goed idee om de belangrijkste kleur van het bannerlogo te gebruiken. |

## <a name="next-steps"></a>Volgende stappen
* [Aan de slag met Azure Active Directory Premium](active-directory-get-started-premium.md)
* [Uw toegangs- en gebruiksrapporten weergeven](active-directory-view-access-usage-reports.md)

<!--Image references-->
[1]: ./media/active-directory-add-company-branding/signin-page_before-customization.png
[2]: ./media/active-directory-add-company-branding/signin-page-restricted-app.png
[3]: ./media/active-directory-add-company-branding/signin-page-open-access.png
[4]: ./media/active-directory-add-company-branding/signin-page-external-guest.png
[5]: ./media/active-directory-add-company-branding/which-elements-can-i-customize.png
[6]: ./media/active-directory-add-company-branding/hide-kmsi.png
[8]: ./media/active-directory-add-company-branding/APBranding.png
[9]: ./media/active-directory-add-company-branding/hidekmsi.png

