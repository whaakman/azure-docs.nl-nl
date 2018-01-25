---
title: De aanmeldingspagina aanpassen voor uw Azure Active Directory-tenant | Microsoft Docs
description: Informatie over het toevoegen van een huisstijl naar de Azure-aanmeldingspagina
services: active-directory
documentationcenter: 
author: curtand
manager: mtillman
editor: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: 
ms.devlang: 
ms.topic: article
ms.date: 01/19/2018
ms.author: curtand
ms.reviewer: kexia
custom: it-pro
ms.openlocfilehash: 03a6b82f769ed9a36c5d3ff9934de75d1536e1ae
ms.sourcegitcommit: 28178ca0364e498318e2630f51ba6158e4a09a89
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/24/2018
---
# <a name="quickstart-add-company-branding-to-your-sign-in-page-in-azure-ad"></a>Snelstartgids: Huisstijl naar de aanmeldingspagina in Azure AD toevoegen
Om verwarring te voorkomen, willen veel bedrijven een consistente look gebruiken voor alle websites en services die ze beheren. Azure Active Directory (Azure AD) biedt deze mogelijkheid. u kunt pas het uiterlijk van de aanmeldingspagina met uw bedrijfslogo en kleurenschema toepassen. De aanmeldingspagina wordt weergegeven wanneer u zich aanmeldt bij webgebaseerde toepassingen zoals Office 365 die gebruikmaken van Azure AD als id-provider. U communiceert met deze pagina uw referenties in te voeren.

> [!NOTE]
> * Huisstijl is alleen beschikbaar als u Premium of Basic licentie hebt aangeschaft voor Azure AD of een Office 365-licentie hebt. Controleer voor meer informatie als een functie wordt ondersteund door het licentietype van uw, de [prijspagina informatie voor Azure Active Directory](https://azure.microsoft.com/pricing/details/active-directory/).
> 
> * Edities Azure AD Premium en Basic zijn beschikbaar voor klanten in China via het wereldwijde exemplaar van Azure Active Directory. Edities Azure AD Premium en Basic worden momenteel niet ondersteund in de Azure-service beheerd door 21Vianet in China. Neem contact op met ons op voor meer informatie de [Azure Active Directory-Forum](https://feedback.azure.com/forums/169401-azure-active-directory/).

## <a name="customizing-the-sign-in-page"></a>De aanmeldingspagina aanpassen

<!--You can customize the following elements on the sign-in page: <attach image>-->

Bedrijf huisstijl aanpassingen op de aanmeldingspagina van Azure AD worden weergegeven wanneer gebruikers toegang krijgen een tenantspecifieke URL zoals tot [ *https://outlook.com/contoso.com*](https://outlook.com/contoso.com).

Wanneer gebruikers www.office.com bezoekt, wordt niet de aanmeldingspagina een huisstijl aanpassingen omdat de gebruiker nog geen referenties heeft ingevoerd weergegeven. Nadat een gebruiker krijgt de hun gebruikers-ID of een gebruikerstegel selecteert, wordt de huisstijl weergegeven.

> [!NOTE]
> * Uw domeinnaam moet worden weergegeven als 'Actief' de **domeinen** gedeelte van de Azure portal waarin u de huisstijl hebt geconfigureerd. Zie voor meer informatie [toevoegen van een aangepaste domeinnaam](add-custom-domain.md).
> * De huisstijl van de aanmeldingspagina wordt niet meegenomen naar de aanmeldingspagina van persoonlijke Microsoft-accounts. Als uw werknemers of business-gasten zich met een persoonlijk Microsoft-account aanmeldt, komt hun aanmeldingspagina niet overeen met de huisstijl van uw organisatie.


### <a name="banner-logo"></a>Bannerlogo 

Beschrijving | Beperkingen | Aanbevelingen
------- | ------- | ----------
Het logo in banner wordt weergegeven op de aanmeldingspagina en de toegang deelvenster pagina's.<br>Op de aanmeldingspagina ziet u het logo nadat de gebruikersnaam wordt ingevoerd. | Transparante JPG of PNG<br>Maximale hoogte: 36 px<br>Maximale breedte: 245 px | Logo van uw organisatie hier gebruiken.<br>Gebruik een transparante afbeelding. Niet, wordt ervan uitgegaan dat de achtergrond wit zijn.<br>Opvulling om de logo in de afbeelding niet toevoegen of eruit ziet uw logo niet goed klein.

### <a name="username-hint"></a>Gebruikersnaam hint   
Beschrijving | Beperkingen | Aanbevelingen
------- | ------- | ----------
Deze optie wordt de instructietekst in het veld username aangepast. | Unicodetekst, maximaal 64 tekens<br>Alleen tekst zonder opmaak | Als u verwacht gastgebruikers buiten uw organisatie aan te melden bij uw app dat, wordt u aangeraden dat u niet Stel deze optie.
            
### <a name="sign-in-page-text"></a>Tekst van aanmeldingspagina   
Beschrijving | Beperkingen | Aanbevelingen
------- | ------- | ----------
Deze optie wordt weergegeven onder aan het formulier aanmelden en kan worden gebruikt om aanvullende informatie zoals het telefoonnummer op dat naar de helpdesk of een juridische mededeling te communiceren. | Unicodetekst, maximaal 256 tekens<br>Alleen tekst zonder opmaak (geen koppelingen of HTML-tags)    

### <a name="sign-in-page-image"></a>Afbeelding van de aanmeldingspagina  
Beschrijving | Beperkingen | Aanbevelingen
------- | ------- | ----------
Deze optie wordt weergegeven op de achtergrond van de aanmeldingspagina wordt verankerd aan het midden van de ruimte die kunnen worden weergegeven, en kan worden geschaald en gewassen voor het vervullen van het browservenster.    <br>Scherm zoals mobiele telefoons erg smal, is deze afbeelding niet weergegeven.<br>Een zwarte masker met 0,55 dekking wordt toegepast via deze installatiekopie wanneer de pagina wordt geladen. | JPG of PNG<br>Afbeelding van dimensies: 1920 x 1080 px<br>De bestandsgrootte: &lt; 300 KB | <br>Installatiekopieën gebruiken wanneer er een sterk onderwerp focus niet. Het formulier voor ondoorzichtige aanmeldingspagina wordt weergegeven boven het midden van deze installatiekopie en voldoende is voor een deel van de afbeelding, afhankelijk van de grootte van het browservenster.<br>Houd de bestandsgrootte klein zijn om ervoor te zorgen snelle laadtijden. 

### <a name="sign-in-page-background-color"></a>Achtergrondkleur van de aanmeldingspagina
Beschrijving | Beperkingen | Aanbevelingen
------- | ------- | ----------
Deze kleur wordt gebruikt in plaats van de achtergrondafbeelding op lage bandbreedte. | RGB-kleur in de hexadecimale notatie (voorbeeld: #FFFFFF | Het is raadzaam om met behulp van de primaire kleur van het logo in banner of de kleur van uw organisatie.

### <a name="square-logo-image"></a>Logoafbeelding van vierkante
Beschrijving | Beperkingen | Aanbevelingen
------- | ------- | ----------
Deze afbeelding wordt weergegeven tijdens de installatie voor nieuwe Enterprise Windows 10-computers. Dit biedt een context aan werknemers wanneer ze hun nieuwe werk-PC instelt. De afbeelding wordt weergegeven voor tenants die gebruikmaken van [Windows Automatische piloot](https://blogs.windows.com/business/2017/06/29/delivering-modern-promise-windows-10/?utm_source=dlvr.it&utm_medium=twitter#gDTp1u6q35bvDWIS.97) voor het implementeren van hun apparaten werk en op wachtwoordinvoer pagina's in andere Windows-10-ervaringen. | Transparant PNG (aanbevolen) of JPG<br>Afbeelding van dimensies: 240 x 240 px<br>De bestandsgrootte: &lt; 10 KB | Logo van uw organisatie hier gebruiken.<br> Gebruik een transparante afbeelding.<br>Niet, wordt ervan uitgegaan dat de achtergrond wit zijn.<br>Opvulling aan uw logo in de afbeelding niet toevoegen of eruit ziet uw logo niet goed klein.

### <a name="show-option-to-remain-signed-in"></a>Optie weergeven om aangemeld te blijven
Beschrijving | Beperkingen | Aanbevelingen
------- | ------- | ----------
Azure AD aanmelden geeft de gebruiker de optie om te blijven is aangemeld wanneer ze sluit en de browser open. Deze instelling wordt die optie.<br>Ingesteld op **Nee** deze optie van uw gebruikers te verbergen. | &nbsp; | De optie verbergen, heeft dit geen invloed op de levensduur van de sessie.<br>Sommige functies van Office 2010 en SharePoint Online, is afhankelijk van gebruikers kunnen kiezen om te blijven aangemeld. Als u deze optie instelt op **Nee**, uw gebruikers zien mogelijk aanvullende en onverwachte wordt u gevraagd om aan te melden.

> [!NOTE]
> Alle elementen zijn optioneel. Bijvoorbeeld, als u een logo in banner opgeven dat er geen achtergrondafbeelding, wordt de aanmeldingspagina weergegeven uw logo en afbeelding voor de doelsite (bijvoorbeeld Office 365).

## <a name="add-company-branding-to-your-directory"></a>Huisstijl aan uw directory toevoegen

1. Aanmelden bij [het Azure AD-beheercentrum](https://aad.portal.azure.com) met een account met globale beheerdersrechten voor de tenant.
2. Selecteer **gebruikers en groepen** > **bedrijf huisstijl** > **bewerken**.
  
  ![Aangepaste huisstijl openen](./media/customize-branding/navigation-to-branding.png)
3. Wijzig de elementen die u wilt aanpassen. Alle elementen zijn optioneel.
  
  ![Aangepaste huisstijl bewerken](./media/customize-branding/edit-branding.png)
5. Wanneer u bent klaar, selecteert u **opslaan**.

Dit kan een uur duren voor alle wijzigingen naar de pagina aanmeldingspagina met huisstijl weergegeven.

## <a name="add-language-specific-company-branding-to-your-directory"></a>De huisstijl specifieke taal zijn gebonden aan uw directory toevoegen

1. Meld u aan bij het [beheercentrum van Azure AD](https://aad.portal.azure.com) met een account met globale beheerdersrechten voor de directory.
2. Selecteer **gebruikers en groepen** > **bedrijf huisstijl** > **nieuwe taal**.
  
  ![De huisstijl elementen taalspecifieke toevoegen](./media/customize-branding/add-language.png)
5. Wijzig de elementen die u wilt aanpassen. Alle elementen zijn optioneel.
6. Wanneer u bent klaar, selecteert u **opslaan**.

Dit kan een uur duren voor alle wijzigingen naar de pagina aanmeldingspagina met huisstijl weergegeven.

## <a name="next-steps"></a>Volgende stappen
In deze snelstartgids hebt u geleerd hoe huisstijl aan uw Azure AD-directory toevoegen. 

U kunt de volgende koppeling voor het configureren van uw huisstijl op Azure AD via de Azure portal gebruiken.

> [!div class="nextstepaction"]
> [Een bedrijfshuisstijl toevoegen](https://aad.portal.azure.com/#blade/Microsoft_AAD_IAM/LoginTenantBrandingBlade) 