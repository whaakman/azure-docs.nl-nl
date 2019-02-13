---
title: Hoe en waarom toepassingen worden toegevoegd aan Azure Active Directory
description: Wat betekent het dat voor een toepassing moet worden toegevoegd aan Azure AD en hoe ze krijg er?
services: active-directory
documentationcenter: ''
author: CelesteDG
manager: mtillman
editor: ''
ms.assetid: 3321d130-f2a8-4e38-b35e-0959693f3576
ms.service: active-directory
ms.subservice: develop
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 04/18/2018
ms.author: celested
ms.custom: aaddev
ms.reviewer: elisol, lenalepa
ms.collection: M365-identity-device-management
ms.openlocfilehash: 82ccfe6a8d77926d2b908f84db600b0665565d51
ms.sourcegitcommit: 301128ea7d883d432720c64238b0d28ebe9aed59
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/13/2019
ms.locfileid: "56173888"
---
# <a name="how-and-why-applications-are-added-to-azure-ad"></a>Hoe en waarom worden toepassingen toegevoegd aan Azure AD

Er zijn twee manieren van toepassingen in Azure AD: 
* [Toepassingsobjecten](app-objects-and-service-principals.md#application-object) : Er is wel [uitzonderingen](#notes-and-exceptions), objecten voor toepassingen kunnen worden beschouwd als de definitie van een toepassing.
* [Service-principals](app-objects-and-service-principals.md#service-principal-object) -kunnen worden beschouwd als een exemplaar van een toepassing. Service-principals in het algemeen verwijzen naar een toepassingsobject en een toepassingsobject kan worden verwezen door meerdere service-principals in mappen.

## <a name="what-are-application-objects-and-where-do-they-come-from"></a>Wat zijn toepassingsobjecten en waar komen ze vandaan?
U kunt beheren [toepassingsobjecten](app-objects-and-service-principals.md#application-object) in Azure portal via de [App-registraties](https://portal.azure.com/#blade/Microsoft_AAD_IAM/ApplicationsListBlade) optreden. Toepassingsobjecten beschrijven de toepassing naar Azure AD en kunnen worden beschouwd als de definitie van de toepassing, zodat de service wilt weten hoe tokens uitgeven aan de toepassing op basis van de instellingen. Het toepassingsobject alleen aanwezig in de oorspronkelijke directory, zelfs als het een toepassing met meerdere tenants ondersteunen van service-principals in andere directory's. Het toepassingsobject omvat mogelijk een van de volgende (als goed als aanvullende informatie die hier niet worden vermeld):
* De naam, logo en uitgever
* Antwoord-URL's
* Geheimen (symmetrische en/of asymmetrische sleutels gebruikt voor het verifiëren van de toepassing)
* API-afhankelijkheden (OAuth)
* Gepubliceerde API's / resources/bereiken (OAuth)
* App-rollen (RBAC)
* De metagegevens van eenmalige aanmelding en configuratie
* Gebruikers inrichten van metagegevens en configuratie
* De metagegevens van de proxy- en configuratie

Objecten voor toepassingen kunnen worden gemaakt via meerdere paden, met inbegrip van:
* Registratie in Azure portal
* Het maken van een nieuwe toepassing met behulp van Visual Studio en het configureren van het gebruik van Azure AD-verificatie
* Wanneer een beheerder een toepassing toevoegt in de app-galerie (dit wordt ook een service-principal maken)
* Met behulp van de Microsoft Graph API, Azure AD Graph API of PowerShell om een nieuwe toepassing te maken
* Vele andere verschillende ontwikkelaarservaringen met de, waaronder in Azure en in de API explorer ervaringen over developer centers

## <a name="what-are-service-principals-and-where-do-they-come-from"></a>Wat zijn service-principals en waar komen ze vandaan?
U kunt beheren [service-principals](app-objects-and-service-principals.md#service-principal-object) in Azure portal via de [bedrijfstoepassingen](https://portal.azure.com/#blade/Microsoft_AAD_IAM/StartboardApplicationsMenuBlade/AllApps/menuId/) optreden. Service-principals zijn wat een toepassing die verbinding maken met Azure AD beheren en kunnen worden beschouwd als het exemplaar van de toepassing in uw directory. Voor een bepaalde toepassing heeft dit maximaal één toepassingsobject (die is geregistreerd in een ' '-basismap) en een of meer service-principalobjecten voor exemplaren van de toepassing in elke map waarin het fungeert. 

De service-principal kunt opnemen:

* Een verwijzing naar een toepassingsobject via de eigenschap van de toepassing-ID
* Records van de lokale gebruiker en groep-toepassing-roltoewijzingen
* Records van lokale gebruikers- en beheerdersactiviteiten machtigingen verleend aan de toepassing
  * Bijvoorbeeld: machtiging voor de toepassing te krijgen tot een bepaalde gebruiker e-mail
* Records van lokaal beleid, met inbegrip van beleid voor voorwaardelijke toegang
* Records van alternatieve lokale instellingen voor een toepassing
  * Claims transformatie regels
  * Kenmerktoewijzingen (inrichten van gebruikers)
  * Directory-specifieke app-rollen (indien de toepassing biedt ondersteuning voor aangepaste rollen)
  * Directory-specifieke naam of het logo

Net als objecten van de toepassing, worden service-principals ook gemaakt via meerdere paden, met inbegrip van:

* Wanneer gebruikers zich aanmelden bij een toepassing van derden worden geïntegreerd met Azure AD
  * Tijdens het aanmelden, worden gebruikers gevraagd om machtigingen voor de toepassing om toegang tot hun profiel en andere machtigingen te geven. De eerste persoon toestemming geven zorgt ervoor dat een service-principal die de toepassing moet worden toegevoegd aan de map vertegenwoordigt.
* Wanneer gebruikers zich aanmelden bij Microsoft online services zoals [Office 365](https://products.office.com/)
  * Wanneer u zich op Office 365 abonneert of een proefversie starten, worden een of meer service-principals zijn gemaakt in de map voor de verschillende services die worden gebruikt om alle functionaliteit die is gekoppeld aan Office 365.
  * Sommige Office 365-services, zoals SharePoint maken service-principals regelmatig zodat beveiligde communicatie mogelijk tussen onderdelen, met inbegrip van werkstromen.
* Wanneer een beheerder een toepassing toevoegt in de app-galerie (dit wordt ook een onderliggende app-object gemaakt)
* Een toepassing toevoegen aan het gebruik van de [Azure AD-toepassingsproxy](https://msdn.microsoft.com/library/azure/dn768219.aspx)
* Verbinding maken met een toepassing voor eenmalige aanmelding over het gebruik van SAML of het wachtwoord van de eenmalige aanmelding (SSO)
* Via een programma via de Azure AD Graph API of PowerShell

## <a name="how-are-application-objects-and-service-principals-related-to-each-other"></a>Hoe worden toepassingsobjecten en service-principals aan elkaar gerelateerd?
Een toepassing heeft een toepassingsobject in de basisdirectory waarnaar wordt verwezen door een of meer service-principals in elk van de mappen waar deze werkt (met inbegrip van de basismap van de toepassing).
![Een diagram waarin u ziet hoe toepassingsobjecten en service-principals met elkaar en exemplaren van Azure AD communiceren.][apps_service_principals_directory]

In het voorgaande diagram, behoudt Microsoft twee directory's intern (weergegeven aan de linkerkant) die worden gebruikt om toepassingen te publiceren:

* Een voor Microsoft-Apps (Microsoft services directory)
* Een voor vooraf geïntegreerde toepassingen van derden (App-galerie directory)

Toepassing uitgevers/leveranciers die met Azure AD integreren zijn vereist om een map (weergegeven aan de rechterkant als 'Sommige SaaS Directory').

Toepassingen die u zelf toevoegen (weergegeven als **App (u)** in het diagram) omvatten:

* Apps die u hebt ontwikkeld (geïntegreerd met Azure AD)
* Apps die u voor verbonden single-sign-on
* Apps worden gepubliceerd met behulp van de Azure AD-toepassingsproxy

### <a name="notes-and-exceptions"></a>Opmerkingen bij de en uitzonderingen
* Niet alle service-principals verwijzen naar een toepassingsobject. Wanneer Azure AD was oorspronkelijk bedoeld voor de services die worden geleverd aan toepassingen is beperktere en de service-principal is voldoende voor het tot stand brengen van een toepassings-id. De oorspronkelijke service-principal is dichter in de vorm aan het Windows Server Active Directory-serviceaccount. Daarom is het nog steeds mogelijk te maken van service-principals via verschillende paden, zoals het gebruik van Azure AD PowerShell, zonder eerst een toepassingsobject maken. De Azure AD Graph API is een toepassingsobject vereist voordat u een service-principal maken.
* Niet alle van de hierboven beschreven gegevens is momenteel beschikbaar gemaakt via een programma. De volgende zijn alleen beschikbaar in de gebruikersinterface:
  * Claims transformatie regels
  * Kenmerktoewijzingen (inrichten van gebruikers)
* Zie voor meer gedetailleerde informatie over de service-principal en objecten voor toepassingen, de Azure AD Graph REST API-referentiedocumentatie:
  * [Toepassing](https://msdn.microsoft.com/library/azure/ad/graph/api/entity-and-complex-type-reference#application-entity)
  * [Service Principal](https://msdn.microsoft.com/library/azure/ad/graph/api/entity-and-complex-type-reference#serviceprincipal-entity)

## <a name="why-do-applications-integrate-with-azure-ad"></a>Waarom toepassingen integreren met Azure AD?
Toepassingen worden toegevoegd aan Azure AD gebruikmaken van een of meer van de services die het biedt, met inbegrip van:

* Toepassing verificatie en autorisatie
* Verificatie en autorisatie
* Eenmalige aanmelding met Federatie of het wachtwoord
* Inrichten van gebruikers en synchronisatie
* Op rollen gebaseerd toegangsbeheer - gebruik de map voor het definiëren van rollen van de toepassing om uit te voeren op basis van rollen autorisatie controleert in een toepassing
* OAuth-autorisatieservices - gebruikt door Office 365 en andere Microsoft-toepassingen voor de autorisatie van toegang tot API's / resources
* Toepassingen publiceren en proxy - publiceren een toepassing van een particulier netwerk naar internet

## <a name="who-has-permission-to-add-applications-to-my-azure-ad-instance"></a>Wie heeft machtigingen voor toepassingen toevoegen aan mijn Azure AD-instantie?
Er zijn enkele taken die alleen globale beheerders uitvoeren kunnen (zoals toepassingen uit de app-galerie toevoegen en configureren van een toepassing voor het gebruik van de toepassingsproxy) standaard alle gebruikers in uw directory over rechten voor het registreren van de toepassing die objecten ze zijn ontwikkelen en over welke toepassingen ze share/geven toegang tot de gegevens van hun organisatie via toestemming en billijkheid vastgesteld. Als een persoon de eerste gebruiker in uw directory is te melden bij een toepassing toestemming te verlenen, wordt die een service-principal maken in uw tenant; anders wordt worden de toestemming verlenen gegevens opgeslagen op de bestaande service-principal.

Zodat gebruikers kunnen registreren en instemmen met toepassingen kan in eerste instantie geluid over, maar houd rekening met het volgende:


* Toepassingen zijn kunnen Windows Server Active Directory gebruiken voor gebruikersverificatie voor vele jaren zonder dat de toepassing om te worden ingeschreven of geregistreerd in de map. Nu de organisatie heeft een verbeterde zichtbaarheid aan precies hoeveel toepassingen van de directory gebruikmaakt en voor welk doel.
* De noodzaak van een beheerder gebaseerde toepassing registreren en het publicatieproces overdragen van deze verantwoordelijkheden voor gebruikers worden genegeerd. Met Active Directory Federation Services (ADFS) is het waarschijnlijk dat een beheerder had een toepassing toevoegen als een relying party namens hun ontwikkelaars. Ontwikkelaars kunnen nu selfservice.
* Gebruikers aanmelden bij toepassingen met behulp van hun organisatie-account voor zakelijke doeleinden is een goede prestaties. Als ze vervolgens laat u de organisatie die automatisch zullen zij toegang verliezen tot hun account in de toepassing die ze gebruikten.
* Een record van welke gegevens worden gedeeld waarmee de toepassing een goede prestaties is hebben. Gegevens meer vervoerbare dan ooit en is het nuttig om een duidelijke record van wie welke gegevens gedeeld met welke toepassingen hebt.
* API-eigenaren die Azure AD voor OAuth gebruiken bepalen precies welke gebruikers machtigingen kunnen verlenen tot toepassingen en machtigingen die u moeten een beheerder om te accepteren. Alleen beheerders kunnen instemmen met de groter bereik en belangrijker machtigingen, terwijl de toestemming van de gebruiker is afgestemd op de eigen gebruikersgegevens en -mogelijkheden.
* Wanneer een gebruiker wordt toegevoegd of kan een toepassing voor toegang tot hun gegevens, kan de gebeurtenis worden gecontroleerd, zodat u de Audit Reports in Azure portal om te bepalen hoe een toepassing is toegevoegd aan de directory kunt bekijken.

Als u nog steeds wilt om te voorkomen dat gebruikers in uw directory kunnen toepassingen registreren en aanmelden bij toepassingen zonder goedkeuring van de beheerder, zijn er twee instellingen die u wijzigen kunt als u deze functionaliteit uitschakelen:

* Om te voorkomen dat gebruikers ze toepassingen namens hun eigen goedkeuren:
  1. In de Azure-portal, gaat u naar de [gebruikersinstellingen](https://portal.azure.com/#blade/Microsoft_AAD_IAM/StartboardApplicationsMenuBlade/UserSettings/menuId/) sectie onder zakelijke toepassingen.
  2. Wijziging **gebruikers toestemming kunnen geven voor apps die toegang tot bedrijfsgegevens namens hen** naar **Nee**. 
      > [!NOTE]
      > Als u besluit uitschakelen toestemming van de gebruiker, kan een beheerder moeten akkoord gaan met een nieuwe toepassing die een gebruiker moet worden gebruikt.    
* Om te voorkomen dat gebruikers hun eigen toepassingen registreren:
  1. In de Azure-portal, gaat u naar de [gebruikersinstellingen](https://portal.azure.com/#blade/Microsoft_AAD_IAM/ActiveDirectoryMenuBlade/UserSettings) sectie onder Azure Active Directory
  2. Wijziging **gebruikers kunnen toepassingen registreren** naar **Nee**.

> [!NOTE]
> Microsoft zelf maakt gebruik van de standaardconfiguratie met gebruikers kunnen toepassingen registreren en instemmen met toepassingen in hun eigen naam.

<!--Image references-->
[apps_service_principals_directory]:../media/active-directory-how-applications-are-added/HowAppsAreAddedToAAD.jpg

