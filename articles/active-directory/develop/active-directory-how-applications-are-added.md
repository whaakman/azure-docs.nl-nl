---
title: Hoe toepassingen worden toegevoegd aan Azure Active Directory.
description: Dit artikel wordt beschreven hoe toepassingen worden toegevoegd aan een exemplaar van Azure Active Directory.
services: active-directory
documentationcenter: 
author: shoatman
manager: mtillman
editor: 
ms.assetid: 3321d130-f2a8-4e38-b35e-0959693f3576
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 02/09/2016
ms.author: shoatman
ms.custom: aaddev
ms.openlocfilehash: 51ef7e554b6fd3764893f0fd35464088e42e49f8
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/11/2017
---
# <a name="how-and-why-applications-are-added-to-azure-ad"></a>Hoe en waarom toepassingen worden toegevoegd aan Azure AD
Een van de in eerste instantie puzzling dingen tijdens het weergeven van een lijst met toepassingen in uw exemplaar van Azure Active Directory is het waar de toepassingen vandaan komen en waarom ze zijn er registreren.  In dit artikel biedt een hoog niveau overzicht van hoe toepassingen worden weergegeven in de map en bieden u context die u u helpt begrijpen hoe een toepassing zijn geworden in uw directory.

## <a name="what-services-does-azure-ad-provide-to-applications"></a>Welke services biedt Azure AD voor toepassingen?
Toepassingen worden toegevoegd aan Azure AD gebruikmaken van een of meer van de services biedt.  Deze services zijn onder andere:

* App-verificatie en autorisatie
* Verificatie en autorisatie
* Eenmalige aanmelding (SSO) met behulp van Federatie of wachtwoord
* Gebruikers inrichten & synchronisatie
* Toegangsbeheer op basis van rollen; Gebruik de map toepassingsrollen om uit te voeren rollen definiëren op basis van autorisatie controles in een app.
* oAuth-autorisatieservices (gebruikt door Office 365 en andere Microsoft-apps toegang verlenen aan de API's / bronnen.)
* Toepassingen publiceren en & proxyverkeer; Een app vanaf een particulier netwerk met het internet publiceren

## <a name="how-are-applications-represented-in-the-directory"></a>Hoe worden toepassingen weergegeven in de map?
Toepassingen worden weergegeven in de Azure AD 2-objecten gebruiken: een object voor de toepassing en een service-principal-object.  Er is een object van de toepassing, geregistreerd in een "home" / 'eigenaar' of 'publiceren' directory en een of meer service-principal-objecten die de toepassing in elke directory het fungeert.  

Het toepassingsobject beschrijft de app naar Azure AD (de multitenant-service) en bevatten geen van de volgende mogelijk: (*Opmerking*: dit is geen uitputtende lijst.)

* Naam, Logo & uitgever
* Geheimen (symmetrische en/of asymmetrische sleutels gebruikt voor het verifiëren van de app)
* API-afhankelijkheden (oAuth)
* API's / resources/bereiken gepubliceerd (oAuth)
* App-functies (RBAC)
* SSO-metagegevens en -configuratie (SSO)
* Gebruikers inrichten metagegevens en configuratie
* Proxy-metagegevens en configuratie

De service-principal is een record van de toepassing in elke map, waar de toepassing fungeert de basisdirectory inclusief.  De service-principal:

* Terugverwijst naar een application-object via de app-id-eigenschap
* Registreert lokale gebruiker en groep-app-roltoewijzingen
* Lokale gebruikers en de beheerder machtigingen registreert verleend bij de app
  * Bijvoorbeeld: machtiging voor de app toegang tot een bepaalde gebruikers e-mail
* Records lokaal beleid, met inbegrip van beleid voor voorwaardelijke toegang
* Registreert lokale alternatieve lokale instellingen voor een app
  * Claimregels voor transformatie
  * Kenmerktoewijzingen (gebruikersinrichting)
  * Specifieke app-functies voor de tenantsleutel (als de app aangepaste rollen ondersteunt)
  * Naam/het Logo

### <a name="a-diagram-of-application-objects-and-service-principals-across-directories"></a>Een diagram van toepassingsobjecten en service-principals op mappen
![Een diagram illustreert hoe toepassing objecten en service-principals bestaande met exemplaren van Azure AD.][apps_service_principals_directory]

Zoals u in het bovenstaande diagram zien kunt.  Microsoft onderhoudt twee directory's intern (aan de linkerkant) wordt gebruikt om toepassingen te publiceren.

* Één voor Microsoft-Apps (directory voor services van Microsoft)
* Een voor vooraf geïntegreerde 3e partij Apps (App-galerie directory)

Toepassing uitgevers/leveranciers die met Azure AD integreren zijn vereist om een map publiceren.  (Sommige SAAS Directory).

Toepassingen die u zelf toevoegen omvatten:

* Apps die u hebt ontwikkeld (geïntegreerd met AAD)
* Apps die u verbonden voor eenmalige aanmelding
* Apps die u hebt gepubliceerd met behulp van de Azure AD-toepassingsproxy.

### <a name="a-couple-of-notes-and-exceptions"></a>Een aantal opmerkingen en uitzonderingen
* Niet alle service-principals verwijzen naar toepassingsobjecten.  Huh? Wanneer Azure AD oorspronkelijk is gemaakt de services die worden geleverd voor toepassingen zijn veel meer beperkt en de service-principal is voldoende voor het tot stand brengen van een app-identiteit.  De oorspronkelijke service-principal is dichter in de vorm aan de Windows Server Active Directory-account.  Om deze reden's het nog steeds mogelijk te maken met behulp van Azure AD PowerShell zonder eerst te maken object voor een service-principals.  De Graph API vereist een app-object voordat u een service-principal maken.
* Niet alle van de hierboven beschreven gegevens is momenteel beschikbaar gemaakt via een programma.  De volgende zijn alleen beschikbaar in de gebruikersinterface:
  * Claimregels voor transformatie
  * Kenmerktoewijzingen (gebruikersinrichting)
* Voor meer gedetailleerde informatie over de service-principal en toepassingsobjecten Raadpleeg de documentatie van Azure AD Graph REST-API-verwijzing.  *Hint*: de Azure AD Graph API-documentatie het dichtstbijzijnde wat u moet een schemaverwijzing voor Azure AD dat beschikbaar is.  
  * [Toepassing](https://msdn.microsoft.com/library/azure/ad/graph/api/entity-and-complex-type-reference#application-entity)
  * [Service-Principal](https://msdn.microsoft.com/library/azure/ad/graph/api/entity-and-complex-type-reference#serviceprincipal-entity)

## <a name="how-are-apps-added-to-my-azure-ad-instance"></a>Hoe worden apps toegevoegd aan mijn Azure AD-exemplaar?
Er zijn veel manieren die een app kan worden toegevoegd aan Azure AD:

* Toevoegen van een app van de [App-galerie van Azure Active Directory](https://azure.microsoft.com/updates/azure-active-directory-over-1000-apps/)
* Meld u aan van/naar een 3rd Party App is geïntegreerd met Azure Active Directory (bijvoorbeeld: [Smartsheet](https://app.smartsheet.com/b/home) of [DocuSign](https://www.docusign.net/member/MemberLogin.aspx))
  * Tijdens de aanmelding omhoog/gebruikers wordt toestemming naar de app voor toegang tot hun profiel en andere machtigingen geven om gevraagd.  De eerste persoon toestemming geven zorgt ervoor dat een service-principal voor de app moet worden toegevoegd aan de map.
* Aanmelding van/naar Microsoft online services, zoals [Office 365](http://products.office.com/)
  * Wanneer u zich abonneert op Office 365 of beginnen met een proefversie van een of meer service-principals zijn gemaakt in de map voor de verschillende services die worden gebruikt voor het leveren van alle functionaliteit die is gekoppeld aan Office 365.
  * Service-principals maken sommige Office 365-services, zoals SharePoint op basis van continu voor beveiligde communicatie tussen onderdelen, waaronder de werkstromen.
* Een app die u ontwikkelt toevoegen in de Azure-beheerportal zien: https://msdn.microsoft.com/library/azure/dn132599.aspx
* Toevoegen aan een app die u ontwikkelt met Visual Studio Zie:
  * [ASP.Net-verificatiemethoden](http://www.asp.net/visual-studio/overview/2013/creating-web-projects-in-visual-studio#orgauthoptions)
  * [Verbonden Services](http://blogs.msdn.com/b/visualstudio/archive/2014/11/19/connecting-to-cloud-services.aspx)
* Toevoegen van een app kan gebruiken om het gebruik van de [Azure AD-toepassingsproxy](https://msdn.microsoft.com/library/azure/dn768219.aspx)
* Verbinding maken met een app voor eenmalige aanmelding via SAML of SSO-wachtwoord
* Vele andere met inbegrip van verschillende ervaringen met de ontwikkelaar in Azure en/in API explorer optreedt tussen ontwikkelaars centers

## <a name="who-has-permission-to-add-applications-to-my-azure-ad-instance"></a>Wie de machtiging voor het toevoegen van toepassingen naar Mijn Azure AD-exemplaar heeft?
Alleen globale beheerders kunnen:

* Apps uit de galerie van Azure AD-app (vooraf geïntegreerde 3e partij Apps) toevoegen
* Een app publiceren met de Azure AD-toepassingsproxy

Alle gebruikers in uw directory zijn machtigingen voor het toevoegen van toepassingen die ze ontwikkelt en discreet via welke toepassingen ze share/geven toegang tot hun organisatie-gegevens.  *Gebruiker aanmelden omhoog/moet u een app en machtigingen verlenen kan leiden tot een service-principal wordt gemaakt.*

Deze in eerste instantie geluid betreffende mogelijk, maar het volgende rekening houden:

* Apps zijn kunnen gebruikmaken van Windows Server Active Directory voor gebruikersverificatie jaren zonder de toepassing worden geregistreerd/vastgelegd in de directory.  Zichtbaarheid met exact hoeveel apps de directory en what for gebruiken wordt nu aan de organisatie zijn verbeterd.
* U hoeft voor beheer-app publiceren/registratieproces aangestuurd.  Met Active Directory Federation Services is het waarschijnlijk dat een beheerder had een app toevoegen als een relying party namens ontwikkelaars.  Ontwikkelaars kunnen nu self-service.
* Gebruikers die zich in/tot apps met behulp van hun organisatie-account voor zakelijke doeleinden is goed.  Als ze later niet meer de organisatie zullen zij toegang verliezen bij hun account in de toepassing die ze gebruikten.
* Is een record van welke gegevens die aan de toepassing een goed is is gedeeld.  Gegevens zijn meer dan ooit vervoerbare en met een lege record van wie welke gegevens gedeeld met welke toepassingen zijn nuttig.
* Apps die gebruikmaken van Azure AD voor oAuth besluiten precies welke machtigingen die gebruikers kunnen verlenen tot toepassingen en waarvoor machtigingen zijn vereist als een beheerder om te accepteren.  Deze vanzelf dat alleen beheerders met grotere scopes en meer machtigingen instemmen kunnen.
* Gebruikers toevoegen en het toestaan van apps te krijgen tot hun gegevens zijn gecontroleerd gebeurtenissen, zodat u kunt de rapporten Audit binnen de Azure Management portal om te bepalen hoe een app is toegevoegd aan de map weergeven.

**Opmerking:** *Microsoft zelf is uitgevoerd met behulp van de standaardconfiguratie voor nu veel maanden.*

Met alle die gezegd is het mogelijk om te voorkomen dat gebruikers in uw directory toepassingen toe te voegen en uit te oefenen beslissen over welke informatie ze delen met toepassingen door het wijzigen van de configuratie van de Directory in de Azure-beheerportal.  De volgende configuratie kan worden benaderd binnen de Azure Management portal op het tabblad voor uw Directory 'Configureren'.

![Een schermopname van de gebruikersinterface voor het configureren van de geïntegreerde appinstellingen][app_settings]

<!--Every topic should have next steps and links to the next logical set of content to keep the customer engaged-->
## <a name="next-steps"></a>Volgende stappen
Meer informatie over het toevoegen van toepassingen naar Azure AD en het configureren van services voor apps.

* Ontwikkelaars: [informatie over het integreren van een toepassing met AAD](https://msdn.microsoft.com/library/azure/dn151122.aspx)
* Ontwikkelaars: [voorbeeldcode voor apps die zijn geïntegreerd met Azure Active Directory op GitHub bekijken](https://github.com/AzureADSamples)
* Ontwikkelaars en IT-professionals: [de REST-API-documentatie voor Azure Active Directory Graph API doornemen](https://msdn.microsoft.com/library/azure/hh974478.aspx)
* IT-professionals: [informatie over het gebruik van Azure Active Directory vooraf geïntegreerde toepassingen van de App-galerie](https://msdn.microsoft.com/library/azure/dn308590.aspx)
* IT-professionals: [zelfstudies voor het configureren van specifieke vooraf geïntegreerde apps](https://msdn.microsoft.com/library/azure/dn893637.aspx)
* IT-professionals: [informatie over het publiceren van een app met Azure Active Directory Application Proxy](https://msdn.microsoft.com/library/azure/dn768219.aspx)

## <a name="see-also"></a>Zie ook
* [Article Index for Application Management in Azure Active Directory](../active-directory-apps-index.md) (Artikelindex voor toepassingsbeheer in Azure Active Directory)

<!--Image references-->
[apps_service_principals_directory]:../media/active-directory-how-applications-are-added/HowAppsAreAddedToAAD.jpg
[app_settings]:../media/active-directory-how-applications-are-added/IntegratedAppSettings.jpg
