---
title: Hoe en waarom toepassingen worden toegevoegd aan de Azure Active Directory
description: Wat betekent het voor een toepassing moet worden toegevoegd aan AD Azure en hoe krijgen ze er?
services: active-directory
documentationcenter: ''
author: CelesteDG
manager: mtillman
editor: ''
ms.assetid: 3321d130-f2a8-4e38-b35e-0959693f3576
ms.service: active-directory
ms.component: develop
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 04/18/2018
ms.author: celested
ms.custom: aaddev
ms.openlocfilehash: c9ebfcba59e3f46fb30f4cd2402ec4ebb606f6d0
ms.sourcegitcommit: e14229bb94d61172046335972cfb1a708c8a97a5
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/14/2018
---
# <a name="how-and-why-applications-are-added-to-azure-ad"></a>Hoe en waarom toepassingen worden toegevoegd aan Azure AD
Er zijn twee manieren van toepassingen in Azure AD: 
* [Toepassingsobjecten](active-directory-application-objects.md#application-object) - er zijn [uitzonderingen](#notes-and-exceptions), deze kunnen worden beschouwd als de definitie van een toepassing.
* [Beveiligings-principals service](active-directory-application-objects.md#service-principal-object) -deze worden beschouwd als een instantie van een toepassing. Service-principals in het algemeen verwijst naar een application-object en een application-object kan worden verwezen door meerdere service beveiligings-principals in mappen.

## <a name="what-are-application-objects-and-where-do-they-come-from"></a>Wat zijn toepassingsobjecten en waar komen ze vandaan?
[Toepassingsobjecten](active-directory-application-objects.md#application-object) (die u kunt beheren in de portal Azure via de [App registraties](https://portal.azure.com/#blade/Microsoft_AAD_IAM/ApplicationsListBlade) optreden) beschrijven de Azure AD-toepassing en kan worden beschouwd als de definitie van de toepassing, waardoor de service te weten hoe tokens geven aan de toepassing op basis van de instellingen. Het application-object wordt alleen bestaan in de basismap, zelfs als het toepassing van een huurder met meerdere ondersteunende service beveiligings-principals in andere mappen. Het application-object kan een van de volgende (als en van aanvullende informatie die hier niet worden genoemd) bevatten:
* Naam, logo en publisher
* Antwoord-URL's
* Geheimen (symmetrische en/of asymmetrische sleutels gebruikt voor het verifiëren van de toepassing)
* API-afhankelijkheden (OAuth)
* Gepubliceerde API's / resources/scopes (OAuth)
* App-functies (RBAC)
* SSO-metagegevens en configuratie
* Gebruikers inrichten metagegevens en configuratie
* Proxy-metagegevens en configuratie

Toepassingsobjecten kunnen worden gemaakt via meerdere trajecten, met inbegrip van:
* Registraties in Azure portal Application
* Maken van een nieuwe toepassing met behulp van Visual Studio en deze configureren voor verificatie van AD Azure
* Wanneer een beheerder een toepassing toevoegt vanuit de galerie met app (dit wordt ook een service principal maken)
* Een nieuwe toepassing maken met behulp van de API van Microsoft Graph, Azure AD Graph API of PowerShell
* Vele anderen met inbegrip van ervaringen met verschillende ontwikkelaars in Azure en API explorer ervaringen over developer centers

## <a name="what-are-service-principals-and-where-do-they-come-from"></a>Wat zijn beveiligings-principals service en waar komen ze vandaan?
[Beveiligings-principals service](active-directory-application-objects.md#service-principal-object) (die u kunt beheren via de [bedrijfstoepassingen](https://portal.azure.com/#blade/Microsoft_AAD_IAM/StartboardApplicationsMenuBlade/AllApps/menuId/) optreden) zijn wat werkelijk toepassing geregeld door een verbinding te maken met Azure AD en kan worden beschouwd als het exemplaar van de toepassing in de de map. Voor een bepaalde toepassing kunnen er hooguit een application-object (dat is geregistreerd in een "home" directory) en een of meer service SPN-objecten die exemplaren van de toepassing in elke map waarin het fungeert. 

De service principal kunt opnemen:

* Een verwijzing naar een application-object door de eigenschap application-ID
* Records van lokale gebruikers- en groepstoewijzingen toepassing rol
* Records van de lokale gebruiker en admin machtigingen verleend aan de toepassing
  * Bijvoorbeeld: toestemming voor de toepassing toegang tot een bepaalde gebruiker e-mail
* Records van lokaal beleid, met inbegrip van voorwaardelijke-beleid
* Records met alternatieve lokale instellingen voor een toepassing
  * Claimregels voor transformatie
  * Kenmerktoewijzingen (gebruikersinrichting)
  * Directory-specifieke app rollen (als de toepassing aangepaste rollen ondersteunt)
  * Directory-specifieke naam of logo

Als objecten van toepassing kunnen ook service beveiligings-principals worden gemaakt via meerdere trajecten met inbegrip van:

* Wanneer gebruikers zich aanmelden met een toepassing van derden worden geïntegreerd met Azure AD
  * Tijdens gebruikers worden verzocht geeft toestemming voor de toepassing toegang tot hun profiel en andere machtigingen. De eerste persoon toestemming geven zorgt ervoor dat een service principal die de toepassing moet worden toegevoegd aan de map vertegenwoordigt.
* Wanneer gebruikers zich bij Microsoft online services aanmelden, zoals [Office 365](http://products.office.com/)
  * Wanneer u zich op Office 365 abonneren of beginnen met een proef, worden een of meer service beveiligings-principals gemaakt in de map die de verschillende services die worden gebruikt voor het leveren van alle van de functionaliteit van Office 365.
  * Sommige services, Office 365 zoals SharePoint maken beveiligings-principals service voortdurend voor beveiligde communicatie tussen componenten, met inbegrip van werkstromen toestaan.
* Wanneer een beheerder een toepassing toevoegt vanuit de galerie met app (dit maakt ook een onderliggende object app)
* Een toepassing toevoegen aan het gebruik van de [-toepassingsproxy Azure AD](https://msdn.microsoft.com/library/azure/dn768219.aspx)
* Verbinding maken met een toepassing voor eenmalige aanmelding met behulp van SAML of wachtwoord eenmalige aanmelding (SSO)
* Via programmacode via Azure AD Graph API of PowerShell

## <a name="how-are-application-objects-and-service-principals-related-to-each-other"></a>Toepassingsobjecten en service-principals relatie aan elkaar?
Een toepassing is een application-object in de basismap bevindt waarnaar wordt verwezen door een of meer service beveiligings-principals in elk van de mappen waarin zij werkzaam is (met inbegrip van de basismap van de toepassing).
![Een schematische weergave van de objecten application service-principals interactie en met elkaar en Azure AD exemplaren.][apps_service_principals_directory]

In het voorgaande diagram wordt twee mappen intern (weergegeven aan de linkerkant) dat wordt gebruikt voor het publiceren van toepassingen:

* Één voor Microsoft-Apps (directory voor services van Microsoft)
* Een vooraf geïntegreerde-toepassingen van derden (App galerij directory)

Uitgevers/leveranciers van toepassingen die met AD Azure integreren moeten over een publicatiemap (rechts wordt weergegeven als 'Sommige SaaS Directory').

Toepassingen die u zelf toevoegt (weergegeven als **App (uwe)** in het diagram) omvatten:

* Apps die u hebt ontwikkeld (geïntegreerd in Azure AD)
* Apps die u verbonden voor eenmalige aanmelding
* Apps worden gepubliceerd met de toepassingsproxy Azure AD

### <a name="notes-and-exceptions"></a>Notities en uitzonderingen
* Niet alle service-principals verwijzen naar een application-object. Azure AD is oorspronkelijk gebouwd de diensten aan toepassingen meer beperkt zijn als de service principal is voldoende voor de vaststelling van de toepassingsidentiteit van een. De oorspronkelijke service-principal is dichter in de vorm aan de Windows Server Active Directory-account. Om deze reden is het nog steeds mogelijk service-principals via verschillende trajecten, zoals het gebruik van Azure AD PowerShell, zonder eerst te maken een application-object maken. De Azure AD Graph API vereist een application-object voor het maken van een service principal.
* Niet alle van de hierboven beschreven gegevens is momenteel beschikbaar gemaakt via een programma. De volgende zijn alleen beschikbaar in de gebruikersinterface:
  * Claimregels voor transformatie
  * Kenmerktoewijzingen (gebruikersinrichting)
* Zie voor meer gedetailleerde informatie over de service principal en toepassingsobjecten de Azure AD Graph REST API-documentatie:
  * [Toepassing](https://msdn.microsoft.com/library/azure/ad/graph/api/entity-and-complex-type-reference#application-entity)
  * [Service-Principal](https://msdn.microsoft.com/library/azure/ad/graph/api/entity-and-complex-type-reference#serviceprincipal-entity)

## <a name="why-do-applications-integrate-with-azure-ad"></a>Waarom toepassingen integreren met AD Azure?
Azure AD gebruikmaken van een of meer van de services die het biedt, met inbegrip van worden toepassingen toegevoegd:

* Toepassing verificatie en autorisatie
* Verificatie en autorisatie
* Eenmalige aanmelding met federation of wachtwoord
* Gebruikersaanvragen en synchronisatie
* Op rollen gebaseerde toegangscontrole - vergunning controles in een toepassing op basis van de map gebruiken voor het definiëren van de rollen van de toepassing voor het uitvoeren van rollen
* OAuth - machtigingsservices door Office 365 en andere Microsoft-toepassingen gebruikt om u te machtigen toegang tot API's / bronnen
* Toepassing publiceren en proxy - publiceren toepassing in een particulier netwerk met het internet

## <a name="who-has-permission-to-add-applications-to-my-azure-ad-instance"></a>Wie de machtiging voor het toevoegen van toepassingen naar Mijn Azure AD-exemplaar heeft?
Er zijn bepaalde taken die alleen globale administrators kunnen doen (zoals toepassingen uit de galerie app toevoegen en configureren van een toepassing gebruikt de toepassingsproxy) standaard alle gebruikers in de directory gemachtigd om toepassing te registreren die objecten ze zijn ontwikkelen en naar keuze via welke toepassingen zij delen/geven toegang tot hun gegevens tot en met toestemming van de organisatie. Als een persoon de eerste gebruiker in de map is te melden bij een toepassing en toestemming verlenen, die een service principal maakt in uw huurder; anders wordt wordt de toestemming verlenen informatie opgeslagen op de bestaande service principal.

Gebruikers toestaan zich te registreren en toestemming voor het in eerste instantie goed betreffende de toepassingen mogelijk, maar houd rekening met het volgende:


* Toepassingen zijn Windows Server Active Directory gebruikmaken voor de verificatie voor vele jaren zonder de toepassing te worden ingeschreven of geregistreerd in Active directory. De organisatie zal hebben Verbeterde zichtbaarheid en voor welk doel precies hoeveel toepassingen maken gebruik van de map.
* Deze verantwoordelijkheid voor gebruikers overdragen van de noodzaak van een registratie van admin gestuurde toepassingen en het publicatieproces genegeerd. Met Active Directory Federation Services (ADFS) is het waarschijnlijk dat een beheerder had een toepassing toevoegen als een relying party ten behoeve van de ontwikkelaars. Ontwikkelaars kunnen nu self-service.
* Gebruikers aanmelden bij toepassingen met behulp van hun rekeningen organisatie voor zakelijke doeleinden is goed. Als zij nadien de organisatie verlaten verliezen ze automatisch toegang tot hun account in de toepassing was.
* Is een record van welke gegevens die aan de toepassing een goed is is gedeeld. Gegevens is meer dan ooit vervoerbare en is het nuttig om duidelijk wordt vastgelegd wie welke gegevens gedeeld met welke toepassingen.
* API-eigenaars die met Azure AD OAuth besluiten precies wat gebruikers machtigingen kunnen toewijzen aan toepassingen en welke machtigingen moet akkoord gaan met een beheerder om hulp. Alleen beheerders kunnen akkoord groter bereik en meer belangrijke machtigingen, terwijl de toestemming van de gebruiker is binnen het bereik van de eigen gegevens en mogelijkheden.
* Wanneer een gebruiker wordt toegevoegd of kan een toepassing toegang tot hun gegevens, kan de gebeurtenis worden gecontroleerd, zodat u de auditverslagen binnen de Azure portal om te bepalen hoe een toepassing is toegevoegd aan de map kunt bekijken.

Als u toch voorkomen dat gebruikers in de map toepassingen registreren en aanmelden bij toepassingen zonder toestemming van een beheerder wilt, zijn er twee instellingen die u wijzigen kunt om deze functionaliteit uitschakelen:

* Om te voorkomen dat gebruikers toepassingen zelf mee akkoord dat:
  1. Ga in de Azure portal naar de [gebruikersinstellingen](https://portal.azure.com/#blade/Microsoft_AAD_IAM/StartboardApplicationsMenuBlade/UserSettings/menuId/) sectie onder zakelijke toepassingen.
  2. Wijziging **gebruikers kunnen toestemming voor toegang tot bedrijfsgegevens namens hun apps** te **No**. 
     *Houd er rekening mee dat als u toestemming van de gebruiker uitschakelen, een admin toestemming voor elke nieuwe toepassing die van een gebruiker moet moet worden gebruikt.*
* Om te voorkomen dat gebruikers hun eigen toepassingen te registreren:
  1. Ga in de Azure portal naar de [gebruikersinstellingen](https://portal.azure.com/#blade/Microsoft_AAD_IAM/ActiveDirectoryMenuBlade/UserSettings) sectie onder Azure Active Directory
  2. Wijziging **gebruikers toepassingen kunnen registreren** te **No**.

> [!NOTE]
> De standaardconfiguratie van Microsoft zelf gebruikt met gebruikers kunnen toepassingen registreren en instemming met de toepassingen zelf.

<!--Image references-->
[apps_service_principals_directory]:../media/active-directory-how-applications-are-added/HowAppsAreAddedToAAD.jpg

