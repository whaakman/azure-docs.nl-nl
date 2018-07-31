---
title: De aanmeldingspagina voor uw Azure AD-tenant aanpassen | Microsoft Docs
description: Ontdek hoe u uw huisstijl kunt toevoegen aan de Azure-aanmeldingspagina
services: active-directory
documentationcenter: ''
author: eross-msft
manager: mtillman
editor: ''
ms.service: active-directory
ms.workload: identity
ms.component: fundamentals
ms.topic: quickstart
ms.date: 07/20/2018
ms.author: lizross
ms.reviewer: kexia
custom: it-pro
ms.openlocfilehash: 7804d6b0d4a100997fb545e678458424dac6ceed
ms.sourcegitcommit: 44fa77f66fb68e084d7175a3f07d269dcc04016f
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/24/2018
ms.locfileid: "39227271"
---
# <a name="quickstart-add-company-branding-to-your-sign-in-page-in-azure-ad"></a>Snelstart: Huisstijl toevoegen aan de Azure AD-aanmeldingspagina
Om verwarring te voorkomen, willen veel bedrijven een consistente look gebruiken voor alle websites en services die ze beheren. Azure Active Directory (Azure AD) biedt deze mogelijkheid. U kunt het uiterlijk van de aanmeldingspagina aanpassen en uw eigen bedrijfslogo en kleurenschema’s toepassen. De aanmeldingspagina wordt weergegeven wanneer u zich aanmeldt bij webtoepassingen, zoals Office 365, die gebruikmaken van Azure AD als identiteitsprovider. Op deze pagina voert u uw referenties in.

> [!NOTE]
> * De huisstijl van uw bedrijf is alleen beschikbaar als u de Premium- of Basic-licentie van Azure AD hebt aangeschaft of als u een Office 365-licentie hebt. Raadpleeg de pagina [Prijzen voor Azure Active Directory](https://azure.microsoft.com/pricing/details/active-directory/) om na te gaan of een functie wordt ondersteund door uw licentietype.
> 
> * De Azure AD-edities Premium en Basic zijn beschikbaar voor klanten in China via het wereldwijde exemplaar van Azure Active Directory. De Azure AD-edities Premium en Basic worden momenteel niet ondersteund in de Azure-service die wordt beheerd door 21Vianet in China. Neem contact met ons op via het [Azure Active Directory-forum](https://feedback.azure.com/forums/169401-azure-active-directory/) voor meer informatie.

## <a name="customizing-the-sign-in-page"></a>De aanmeldingspagina aanpassen

<!--You can customize the following elements on the sign-in page: <attach image>-->

Huisstijlaanpassingen worden weergegeven op de Azure AD-aanmeldingspagina wanneer gebruikers toegang krijgen tot een tenantspecifieke URL zoals [*https://outlook.com/contoso.com*](https://outlook.com/contoso.com) of de domeinvariabele doorgeven in de URL, zoals [*https://passwordreset.microsoftonline.com/?whr=contoso.com*](https://passwordreset.microsoftonline.com/?whr=contoso.com)

Wanneer een gebruiker bijvoorbeeld www.office.com bezoekt, worden de huisstijlaanpassingen niet weergegeven op de aanmeldingspagina omdat de gebruiker nog geen referenties heeft ingevoerd. Nadat een gebruiker zijn/haar gebruikers-ID heeft ingevoerd of een gebruikerstegel heeft geselecteerd, wordt de huisstijl van uw bedrijf weergegeven.

> [!NOTE]
> * Uw domeinnaam moet worden weergegeven als 'Actief' in het gedeelte **Domeinen** van Azure Portal waarin u de huisstijl hebt geconfigureerd. Zie [Een aangepaste domeinnaam toevoegen](add-custom-domain.md) voor meer informatie.
> * De huisstijl van de aanmeldingspagina wordt niet meegenomen naar de aanmeldingspagina van persoonlijke Microsoft-accounts. Als uw werknemers of zakelijke gasten zich aanmelden met een persoonlijk Microsoft-account, wordt de huisstijl van uw bedrijf niet weergegeven op hun aanmeldingspagina.


### <a name="banner-logo"></a>Bannerlogo 

Beschrijving | Beperkingen | Aanbevelingen
------- | ------- | ----------
Het logo in de banner wordt weergegeven op de aanmeldingspagina en in het toegangsvenster.<br>Het logo wordt op de aanmeldingspagina weergegeven nadat de gebruikersnaam is ingevoerd. | Transparante JPG of PNG<br>Maximale hoogte: 36 px<br>Maximale breedte: 245 px | Gebruik hier het logo van uw organisatie.<br>Gebruik een transparante afbeelding. Ga er niet van uit de achtergrond wit is.<br>Gebruik geen opvulling rondom uw logo in de afbeelding, anders ziet uw logo er onevenredig klein uit.

### <a name="username-hint"></a>Hint voor gebruikersnaam   
Beschrijving | Beperkingen | Aanbevelingen
------- | ------- | ----------
Met deze optie wordt de tekst van de hint in het gebruikersnaamveld aangepast. | Unicodetekst, maximaal 64 tekens<br>Alleen tekst zonder opmaak | Als u verwacht dat gastgebruikers buiten uw organisatie zich aanmelden bij uw app, wordt u aangeraden deze optie niet in te stellen.
            
### <a name="sign-in-page-text"></a>Tekst van aanmeldingspagina   
Beschrijving | Beperkingen | Aanbevelingen
------- | ------- | ----------
Deze optie wordt onder aan het aanmeldingsformulier weergegeven en kan worden gebruikt om aanvullende informatie, zoals het telefoonnummer van uw helpdesk of een juridische verklaring, weer te geven. | Unicodetekst, maximaal 256 tekens<br>Alleen tekst zonder opmaak (geen koppelingen of HTML-tags)    

### <a name="sign-in-page-image"></a>Afbeelding van aanmeldingspagina  
Beschrijving | Beperkingen | Aanbevelingen
------- | ------- | ----------
Deze optie wordt op de achtergrond van de aanmeldingspagina weergegeven, is verankerd in het midden van de zichtbare ruimte en wordt aangepast aan de grootte van het browservenster.    <br>Op smalle schermen, zoals die van telefoons, wordt deze afbeelding niet weergegeven.<br>Er wordt een zwart masker met 0,55 dekking toegepast op deze afbeelding wanneer de pagina wordt geladen. | JPG of PNG<br>Afbeeldingsgrootte: 1920 x 1080 pixels<br>Bestandsgrootte: &lt; 300 KB | <br>Gebruik afbeeldingen wanneer er geen sterke focus op het onderwerp is. Het ondoorzichtige aanmeldingsformulier wordt weergegeven in het midden van deze afbeelding en kan een willekeurig deel van de afbeelding bedekken, afhankelijk van de grootte van het browservenster.<br>Houd de bestandsgrootte klein voor snelle laadtijden. 

### <a name="sign-in-page-background-color"></a>Achtergrondkleur van de aanmeldingspagina
Beschrijving | Beperkingen | Aanbevelingen
------- | ------- | ----------
Voor verbindingen met een lage bandbreedte wordt deze gebruikt in plaats van de achtergrondafbeelding. | RGB-kleur zijn met een hexadecimale notatie (bijvoorbeeld: #FFFFFF | We raden u aan de primaire kleur van het bannerlogo of de kleur van uw organisatie te gebruiken.

### <a name="square-logo-image"></a>Vierkante logo-afbeelding
Beschrijving | Beperkingen | Aanbevelingen
------- | ------- | ----------
Deze afbeelding wordt weergegeven tijdens het instellen van nieuwe Enterprise Windows 10-pc's. Deze biedt werknemers context wanneer ze hun nieuwe zakelijke pc instellen. De afbeelding wordt weergegeven voor tenants die gebruikmaken van [Windows AutoPilot](https://blogs.windows.com/business/2017/06/29/delivering-modern-promise-windows-10/?utm_source=dlvr.it&utm_medium=twitter#gDTp1u6q35bvDWIS.97) om hun zakelijke apparaten te implementeren en op wachtwoordinvoerpagina's in andere Windows-10-ervaringen. | Transparante PNG (bij voorkeur) of JPG<br>Afbeeldingsgrootte: 240 x 240 px<br>Bestandsgrootte: &lt; 10 KB | Gebruik hier het logo van uw organisatie.<br> Gebruik een transparante afbeelding.<br>Ga er niet van uit de achtergrond wit is.<br>Voeg geen opvulling toe aan uw logo in de afbeelding, anders ziet uw logo er onevenredig klein uit.

### <a name="show-option-to-remain-signed-in"></a>Optie weergeven om aangemeld te blijven
Beschrijving | Beperkingen | Aanbevelingen
------- | ------- | ----------
De aanmeldingsoptie van Azure AD biedt de gebruiker de mogelijkheid om aangemeld te blijven wanneer deze de browser sluit en opnieuw opent. Met deze instelling wordt die optie verborgen.<br>Stel deze optie in op **Nee** om deze voor uw gebruikers te verbergen. | &nbsp; | Als u de optie verbergt, heeft dit geen invloed op de levensduur van de sessie.<br>Of sommige functies van SharePoint Online en Office 2010 beschikbaar zijn, hangt ervan af of gebruikers ervoor kunnen kiezen aangemeld te blijven. Als u deze instelling op **Nee** instelt, krijgen uw gebruikers mogelijk extra en onverwachte prompts te zien om zich aan te melden.

> [!NOTE]
> Alle elementen zijn optioneel. Als u bijvoorbeeld een bannerlogo zonder achtergrondafbeelding opgeeft, worden op de aanmeldingspagina uw logo en de achtergrondafbeelding voor de doelsite (bijvoorbeeld Office 365) weergegeven.

## <a name="add-company-branding-to-your-directory"></a>Huisstijl toevoegen aan uw directory

1. Meld u aan bij het [beheercentrum van Azure AD](https://aad.portal.azure.com) met een account met globale beheerdersrechten voor de tenant.
2. Selecteer **Azure Active Directory** > **Huisstijl** > **Bewerken**.
  
  ![Aangepaste huisstijl openen](./media/customize-branding/navigation-to-branding.png)
3. Wijzig de elementen die u wilt aanpassen. Alle elementen zijn optioneel.
  
  ![Aangepaste huisstijl bewerken](./media/customize-branding/edit-branding.png)
4. Selecteer **Opslaan** als u klaar bent.

Het kan een uur duren voordat de wijzigingen die u aan de huisstijl van de aanmeldingspagina hebt aangebracht, worden weergegeven.

## <a name="add-language-specific-company-branding-to-your-directory"></a>Taalspecifieke huisstijl toevoegen aan uw directory

1. Meld u aan bij het [beheercentrum van Azure AD](https://aad.portal.azure.com) met een account met globale beheerdersrechten voor de directory.
2. Selecteer **Azure Active Directory** > **Huisstijl** > **Nieuwe taal**.
  
  ![Taalspecifieke huisstijlelementen toevoegen](./media/customize-branding/add-language.png)
3. Wijzig de elementen die u wilt aanpassen. Alle elementen zijn optioneel.
4. Selecteer **Opslaan** als u klaar bent.

Het kan een uur duren voordat de wijzigingen die u aan de huisstijl van de aanmeldingspagina hebt aangebracht, worden weergegeven.

## <a name="next-steps"></a>Volgende stappen
In deze snelstart hebt u geleerd hoe u een huisstijl kunt toevoegen aan uw Azure AD Directory. 

U kunt de volgende koppeling gebruiken om uw huisstijl te configureren in Azure Active Directory vanuit Azure Portal.

> [!div class="nextstepaction"]
> [Een bedrijfshuisstijl toevoegen](https://aad.portal.azure.com/#blade/Microsoft_AAD_IAM/LoginTenantBrandingBlade) 