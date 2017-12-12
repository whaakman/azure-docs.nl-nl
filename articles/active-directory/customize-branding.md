---
title: Aanpassen van de aanmeldingspagina in Azure Active Directory | Microsoft Docs
description: Informatie over het toevoegen van een huisstijl naar de Azure-aanmeldingspagina
services: active-directory
documentationcenter: 
author: curtand
manager: mtillman
editor: 
ms.assetid: f8b932bc-8b4f-42b5-a2d3-f2c076234a78
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/18/2017
ms.author: curtand
ms.reviewer: kexia
custom: it-pro
ms.openlocfilehash: 0855129c35c0c3d0f1814e8d29b6f3ae7d950db7
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/11/2017
---
# <a name="quickstart-add-company-branding-to-your-sign-in-page-in-azure-ad"></a>Snelstartgids: Huisstijl naar de aanmeldingspagina in Azure AD toevoegen
Om verwarring te voorkomen, willen veel bedrijven een consistente look gebruiken voor alle websites en services die ze beheren. Azure Active Directory (Azure AD) biedt deze mogelijkheid. u kunt pas het uiterlijk van de aanmeldingspagina met uw bedrijfslogo en kleurenschema toepassen. De aanmeldingspagina is de pagina die wordt weergegeven wanneer u zich aanmeldt bij Office 365 of andere toepassingen op Internet die van Azure AD als id-provider gebruikmaken. U communiceert met deze pagina uw referenties in te voeren.

> [!NOTE]
> * Huisstijl is alleen beschikbaar als u een upgrade uitgevoerd naar de Premium of Basic-editie van Azure AD of een Office 365-licentie hebt. Controleer voor meer informatie als een functie wordt ondersteund door het licentietype van uw, de [prijspagina informatie voor Azure Active Directory](https://azure.microsoft.com/pricing/details/active-directory/).
> 
> * De Azure Active Directory-edities Premium en Basic zijn beschikbaar voor klanten in China via het wereldwijde exemplaar van Azure Active Directory. De edities Azure Active Directory Premium en Basic worden momenteel niet ondersteund in de Microsoft Azure-service die wordt beheerd door 21Vianet in China. Neem voor meer informatie contact met ons op via het [Azure Active Directory-forum](https://feedback.azure.com/forums/169401-azure-active-directory/).

## <a name="customizing-the-sign-in-page"></a>De aanmeldingspagina aanpassen

<!--You can customize the following elements on the sign-in page: <attach image>-->

Bedrijf huisstijl aanpassingen op de aanmeldingspagina van Azure AD worden weergegeven wanneer gebruikers toegang krijgen een tenantspecifieke URL zoals tot [ *https://outlook.com/contoso.com*](https://outlook.com/contoso.com).

Wanneer gebruikers een generieke URL, zoals www.office.com bezoekt, is de aanmeldingspagina bevat geen aanpassingen voor huisstijl omdat het systeem niet weet welke gebruiker. Nadat gebruikers hun gebruikersnaam invoeren of selecteren van een gebruikerstegel weergegeven huisstijl.

> [!NOTE]
> * Uw domeinnaam moet worden weergegeven als 'Actief' de **domeinen** gedeelte van de Azure portal waarin u de huisstijl hebt geconfigureerd. Zie voor meer informatie [toevoegen van een aangepaste domeinnaam](add-custom-domain.md).
> * De huisstijl van de aanmeldingspagina wordt niet meegenomen naar de aanmeldingspagina van persoonlijke Microsoft-accounts. Als uw werknemers of business-gasten zich met een persoonlijk Microsoft-account aanmeldt, komt hun aanmeldingspagina niet overeen met de huisstijl van uw organisatie.


### <a name="banner-logo"></a>Bannerlogo 

Beschrijving | Beperkingen | Aanbevelingen
------- | ------- | ----------
Het logo in banner wordt weergegeven op de aanmeldingspagina en de toegang deelvenster pagina's.<br>Dit betekent op de pagina aanmelden nadat de organisatie van de gebruiker is vastgesteld, gewoonlijk nadat de gebruikersnaam wordt ingevoerd.  | Transparante JPG of PNG<br>Maximale hoogte: 36 px<br>Maximale breedte: 245 px | Logo van uw organisatie hier gebruiken.<br>Gebruik een transparante afbeelding. Niet, wordt ervan uitgegaan dat de achtergrond wit zijn.<br>Opvulling om de logo in de afbeelding niet toevoegen of eruit ziet uw logo niet goed klein.

### <a name="username-hint"></a>Gebruikersnaam hint   
Beschrijving | Beperkingen | Aanbevelingen
------- | ------- | ----------
Hiermee wordt de instructietekst in het veld username aangepast. | Unicodetekst, maximaal 64 tekens<br>Alleen tekst zonder opmaak | U wordt aangeraden dat u geen dit instelt als u verwacht dat gastgebruikers buiten uw organisatie aan te melden bij uw app.
            
### <a name="sign-in-page-text"></a>Tekst van aanmeldingspagina   
Beschrijving | Beperkingen | Aanbevelingen
------- | ------- | ----------
Dit wordt weergegeven onder aan het formulier aanmelden en kan worden gebruikt om aanvullende informatie zoals het telefoonnummer op dat naar de helpdesk of een juridische mededeling te communiceren. | Unicodetekst, maximaal 256 tekens<br>Alleen tekst zonder opmaak (geen koppelingen of HTML-tags)   

### <a name="sign-in-page-image"></a>Afbeelding van de aanmeldingspagina  
Beschrijving | Beperkingen | Aanbevelingen
------- | ------- | ----------
Dit wordt weergegeven op de achtergrond van de aanmeldingspagina wordt, wordt verankerd aan het midden van de zichtbare ruimte schalen en bijsnijden zodat deze het browservenster.    <br>Scherm zoals mobiele telefoons erg smal, is deze afbeelding niet weergegeven.<br>Een zwarte masker met 0,55 dekking worden toegepast op deze afbeelding door onze code wanneer de pagina wordt geladen. | JPG of PNG<br>Afbeelding van dimensies: 1920 x 1080 px<br>De bestandsgrootte: &lt; 300 KB | <br>Installatiekopieën gebruiken wanneer er een sterk onderwerp focus niet. Het formulier voor ondoorzichtige aanmeldingspagina wordt weergegeven boven het midden van deze installatiekopie en voldoende is voor een deel van de afbeelding, afhankelijk van de grootte van het browservenster.<br>Houd de bestandsgrootte zo klein mogelijk zodat snel laadtijden. 

### <a name="background-color"></a>Achtergrondkleur
Beschrijving | Beperkingen | Aanbevelingen
------- | ------- | ----------
Deze kleur wordt gebruikt in plaats van de achtergrondafbeelding op lage bandbreedte. | RGB-kleur in de hexadecimale notatie (voorbeeld: #FFFFFF | Het is raadzaam om met behulp van de primaire kleur van het logo in banner of de kleur van uw organisatie.

### <a name="show-option-to-remain-signed-in"></a>Optie aangemeld blijven weergeven
Beschrijving | Beperkingen | Aanbevelingen
------- | ------- | ----------
Azure AD aanmelden geeft de gebruiker de optie voor het aangemelde blijven wanneer ze sluiten en opnieuw hun browser openen. Gebruik dit om te verbergen die optie.<br>Stel dit in op 'Nee' als deze optie van uw gebruikers te verbergen. | &nbsp; | Dit heeft geen invloed op de levensduur van de sessie.<br>Sommige functies van Office 2010 en SharePoint Online, is afhankelijk van gebruikers kunnen kiezen om te blijven aangemeld. Als u deze moet worden verborgen instelt, uw gebruikers zien mogelijk aanvullende en onverwachte wordt u gevraagd om aan te melden.

> [!NOTE]
> Alle elementen zijn optioneel. Bijvoorbeeld, als u een logo in banner opgeven dat er geen achtergrondafbeelding, wordt de aanmeldingspagina weergegeven uw logo en afbeelding voor de doelsite (bijvoorbeeld Office 365).

## <a name="add-company-branding-to-your-directory"></a>Huisstijl aan uw directory toevoegen

1. Aanmelden bij [de Azure-portal](https://portal.azure.com) met een account met globale beheerdersrechten voor de map.
2. Selecteer **meer services**, voer **gebruikers en groepen** in het tekstvak in en selecteer vervolgens **Enter**.

   ![Gebruikersbeheer openen](./media/customize-branding/user-management.png)
3. Op de **gebruikers en groepen** blade Selecteer **bedrijf huisstijl**.
4. Op de **gebruikers en groepen - bedrijf huisstijl** blade, selecteer de **bewerken** opdracht.

    ![Aangepaste huisstijl bewerken](./media/customize-branding/edit-branding.png)
5. Wijzig de elementen die u wilt aanpassen. Alle elementen zijn optioneel.
6. Klik op **Opslaan**.

Dit kan een uur duren voor alle wijzigingen naar de pagina aanmeldingspagina met huisstijl weergegeven.

## <a name="add-language-specific-company-branding-to-your-directory"></a>De huisstijl specifieke taal zijn gebonden aan uw directory toevoegen

1. Meld u aan bij het [beheercentrum van Azure AD](https://aad.portal.azure.com) met een account met globale beheerdersrechten voor de directory.
2. Selecteer **gebruikers en groepen** in het tekstvak in en selecteer vervolgens **Enter**.

   ![Gebruikersbeheer openen](./media/customize-branding/user-management.png)
3. Op de **gebruikers en groepen** blade Selecteer **bedrijf huisstijl**.
4. Op de **gebruikers en groepen - bedrijf huisstijl** blade, selecteer de **taal toevoegen** opdracht.

    ![De huisstijl elementen taalspecifieke toevoegen](./media/customize-branding/add-language.png)
5. Wijzig de elementen die u wilt aanpassen. Alle elementen zijn optioneel.
6. Klik op **Opslaan**.

Dit kan een uur duren voor alle wijzigingen naar de pagina aanmeldingspagina met huisstijl weergegeven.

## <a name="next-steps"></a>Volgende stappen
In deze snelstartgids hebt u geleerd hoe huisstijl aan uw Azure AD-directory toevoegen. 

U kunt de volgende koppeling voor het configureren van uw huisstijl op Azure AD via de Azure portal gebruiken.

> [!div class="nextstepaction"]
> [Een bedrijfshuisstijl toevoegen](https://aad.portal.azure.com/#blade/Microsoft_AAD_IAM/LoginTenantBrandingBlade) 