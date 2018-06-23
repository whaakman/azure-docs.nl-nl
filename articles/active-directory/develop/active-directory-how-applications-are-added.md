---
title: Hoe en waarom toepassingen worden toegevoegd aan Azure Active Directory
description: Wat houdt het voor een toepassing moet worden toegevoegd aan Azure AD en hoe ze krijgen er?
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
ms.reviewer: elisol, lenalepa
ms.openlocfilehash: 5c8ae9534e79b8dc801262f85d8a007e050f4da7
ms.sourcegitcommit: 65b399eb756acde21e4da85862d92d98bf9eba86
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/22/2018
ms.locfileid: "36316956"
---
# <a name="how-and-why-applications-are-added-to-azure-ad"></a>Hoe en waarom toepassingen worden toegevoegd aan Azure AD

Er zijn twee weergaven van toepassingen in Azure AD: 
* [Toepassingsobjecten](active-directory-application-objects.md#application-object) : Er is wel [uitzonderingen](#notes-and-exceptions), toepassingsobjecten kunnen worden beschouwd als de definitie van een toepassing.
* [Service-principals](active-directory-application-objects.md#service-principal-object) -kunnen worden beschouwd als een exemplaar van een toepassing. Service-principals meestal verwijzen naar een object voor de toepassing en een object van de toepassing kan worden verwezen door meerdere service-principals op mappen.

## <a name="what-are-application-objects-and-where-do-they-come-from"></a>Wat zijn toepassingsobjecten en waar komen ze vandaan?
U kunt beheren [toepassingsobjecten](active-directory-application-objects.md#application-object) in de Azure portal via de [App registraties](https://portal.azure.com/#blade/Microsoft_AAD_IAM/ApplicationsListBlade) optreden. Toepassingsobjecten beschrijven de toepassing naar Azure AD en de definitie van de toepassing, waardoor de service om te weten hoe het uitgeven van tokens voor de toepassing op basis van de instellingen kunnen worden overwogen. Het toepassingsobject wordt alleen in de basismap bestaan, zelfs als deze een multitenant-toepassing ondersteunende service-principals in andere directory's. Het toepassingsobject kan omvatten het volgende (als en van aanvullende informatie is hier niet zijn genoemd):
* Naam, logo en uitgever
* Antwoord-URL's
* Geheimen (symmetrische en/of asymmetrische sleutels gebruikt voor het verifiëren van de toepassing)
* API-afhankelijkheden (OAuth)
* Gepubliceerde API's / resources/bereiken (OAuth)
* App-functies (RBAC)
* SSO-metagegevens en configuratie
* Gebruikers inrichten metagegevens en configuratie
* Proxy-metagegevens en configuratie

Toepassingsobjecten kunnen worden gemaakt via meerdere paden, met inbegrip van:
* Registratie van de toepassing in de Azure-portal
* Maken van een nieuwe toepassing met behulp van Visual Studio en te configureren voor Azure AD-verificatie
* Wanneer een beheerder een toepassing toevoegt in de app-galerie (dit wordt ook een service-principal maken)
* Een nieuwe toepassing maken met behulp van de Microsoft Graph API, Azure AD Graph API of PowerShell
* Vele andere met inbegrip van verschillende ervaringen met de ontwikkelaar in Azure en API explorer ervaringen via developer centers

## <a name="what-are-service-principals-and-where-do-they-come-from"></a>Wat zijn service-principals en waar komen ze vandaan?
U kunt beheren [service-principals](active-directory-application-objects.md#service-principal-object) in de Azure portal via de [bedrijfstoepassingen](https://portal.azure.com/#blade/Microsoft_AAD_IAM/StartboardApplicationsMenuBlade/AllApps/menuId/) optreden. Service-principals zijn wat betrekking op een verbinding te maken met Azure AD-toepassing en het exemplaar van de toepassing in uw directory kunnen worden overwogen. Deze kan maximaal één toepassingsobject (die is geregistreerd in een ' ' basismap) en een of meer service SPN-objecten die exemplaren van de toepassing in elke directory het fungeert hebben voor een bepaalde toepassing. 

De service-principal kunt opnemen:

* Een verwijzing naar een application-object via de toepassing-ID-eigenschap
* Records van lokale gebruikers en toepassingsrol groepstoewijzingen
* Records van lokale gebruikers en de beheerder machtigingen te krijgen tot de toepassing
  * Bijvoorbeeld: machtiging voor de toepassing toegang tot een bepaalde gebruiker e-mail
* Records van lokaal beleid, met inbegrip van beleid voor voorwaardelijke toegang
* Records van alternatieve lokale instellingen voor een toepassing
  * Claimregels voor transformatie
  * Kenmerktoewijzingen (gebruikersinrichting)
  * Map specifieke app-functies (als de toepassing aangepaste rollen ondersteunt)
  * Map specifieke naam of het logo

Zoals toepassingsobjecten, worden service-principals ook gemaakt via meerdere paden, met inbegrip van:

* Wanneer gebruikers zich bij een toepassing van derden geïntegreerd met Azure AD
  * Tijdens het aanmelden, worden gebruikers gevraagd om machtiging voor de toepassing toegang krijgen tot hun profiel en andere machtigingen te geven. De eerste persoon toestemming geven zorgt ervoor dat een service-principal dat de toepassing moet worden toegevoegd aan de map vertegenwoordigt.
* Wanneer gebruikers zich bij Microsoft online services aanmelden, zoals [Office 365](http://products.office.com/)
  * Wanneer u zich op Office 365 abonneert of een proefabonnement begint, moet een of meer service-principals zijn gemaakt in de map voor de verschillende services die worden gebruikt voor het leveren van alle functionaliteit die is gekoppeld aan Office 365.
  * Sommige Office 365-services, zoals SharePoint maken service-principals op voortdurend voor beveiligde communicatie tussen onderdelen, waaronder de werkstromen.
* Wanneer een beheerder een toepassing toevoegt in de app-galerie (dit wordt ook een onderliggend object van de app maken)
* Een toepassing toevoegen aan het gebruik van de [Azure AD-toepassingsproxy](https://msdn.microsoft.com/library/azure/dn768219.aspx)
* Verbinding maken met een toepassing voor eenmalige aanmelding op het gebruik van SAML of wachtwoord eenmalige aanmelding (SSO)
* Programmatisch via de Azure AD Graph API of PowerShell

## <a name="how-are-application-objects-and-service-principals-related-to-each-other"></a>Hoe worden toepassingsobjecten en service-principals aan elkaar gerelateerd?
Een toepassing heeft een object van de toepassing in de basisdirectory waarnaar wordt verwezen door een of meer service-principals in elk van de mappen waar optreedt (met inbegrip van de basismap van de toepassing).
![Een diagram illustreert hoe toepassingsobjecten en service-principals met elkaar en exemplaren van Azure AD communiceren.][apps_service_principals_directory]

In het voorgaande diagram onderhoudt Microsoft twee directory's intern (weergegeven aan de linkerkant) die worden gebruikt om toepassingen te publiceren:

* Één voor Microsoft-Apps (directory voor services van Microsoft)
* Één voor vooraf geïntegreerde toepassingen van derden (App-galerie directory)

Toepassing uitgevers/leveranciers die met Azure AD integreren hoeven te hebben van een publicatie map (weergegeven aan de rechterkant als "Some SaaS Directory").

Toepassingen die u zelf toevoegen (weergegeven als **App (jouw e-mailadres)** in het diagram) omvatten:

* Apps die u hebt ontwikkeld (geïntegreerd met Azure AD)
* Apps die u verbonden voor eenmalige aanmelding
* Apps worden gepubliceerd met behulp van de Azure AD-toepassingsproxy

### <a name="notes-and-exceptions"></a>Opmerkingen en uitzonderingen
* Niet alle service-principals verwijzen naar een object voor de toepassing. Azure AD oorspronkelijk is gemaakt de services die worden geleverd voor toepassingen zijn meer beperkte als de service-principal is voldoende voor het tot stand brengen van een toepassings-id. De oorspronkelijke service-principal is dichter in de vorm aan de Windows Server Active Directory-account. Daarom is het nog steeds mogelijk te maken via verschillende paden, zoals het gebruik van Azure AD PowerShell zonder eerst te maken object voor een service-principals. De Azure AD Graph API vereist een toepassingsobject voordat het maken van een service principal.
* Niet alle van de hierboven beschreven gegevens is momenteel beschikbaar gemaakt via een programma. De volgende zijn alleen beschikbaar in de gebruikersinterface:
  * Claimregels voor transformatie
  * Kenmerktoewijzingen (gebruikersinrichting)
* Zie voor meer gedetailleerde informatie over de service-principal en toepassingsobjecten naslagdocumentatie over de REST-API van Azure AD Graph:
  * [Toepassing](https://msdn.microsoft.com/library/azure/ad/graph/api/entity-and-complex-type-reference#application-entity)
  * [Service-Principal](https://msdn.microsoft.com/library/azure/ad/graph/api/entity-and-complex-type-reference#serviceprincipal-entity)

## <a name="why-do-applications-integrate-with-azure-ad"></a>Waarom toepassingen integreren met Azure AD?
Toepassingen worden toegevoegd aan Azure AD gebruikmaken van een of meer van de services met inbegrip van biedt:

* Toepassing verificatie en autorisatie
* Verificatie en autorisatie
* Eenmalige aanmelding met Federatie of wachtwoord
* Gebruikers inrichten en synchroniseren
* Toegangsbeheer op basis van functie - gebruik de map voor het definiëren van rollen om uit te voeren op basis van rollen autorisatie toepassing controleert in een toepassing
* De autorisatieservices OAuth - gebruikt door Office 365 en andere Microsoft-toepassingen toegang verlenen aan de API's / resources
* Toepassingen publiceren en proxy - publiceren een toepassing van een particulier netwerk met het internet

## <a name="who-has-permission-to-add-applications-to-my-azure-ad-instance"></a>Wie de machtiging voor het toevoegen van toepassingen naar Mijn Azure AD-exemplaar heeft?
Er zijn een aantal taken dat alleen globale beheerders doen kunnen (zoals het toevoegen van toepassingen van de app-galerie en configureren van een toepassing te gebruiken de toepassingsproxy) standaard alle gebruikers in uw directory over rechten voor het registreren van de toepassing die objecten ze zijn ontwikkelen en discreet via welke toepassingen ze share/geven toegang tot de gegevens van hun organisatie via toestemming. Als een persoon de eerste gebruiker in uw directory te melden bij een toepassing toestemming verlenen is, wordt die een service-principal maken in de tenant; anders wordt worden de toestemming verlenen gegevens opgeslagen op de bestaande service-principal.

Zodat gebruikers kunnen registreren en instemmen met toepassingen mogelijk in eerste instantie geluid betreffende, maar houd rekening met het volgende:


* Toepassingen is kunnen gebruikmaken van Windows Server Active Directory voor gebruikersverificatie jaren zonder de toepassing om te worden ingeschreven of geregistreerd in de map. Nu de organisatie wordt hebben een verbeterde zichtbaarheid aan precies hoeveel toepassingen maken gebruik van de map en wat is het doel.
* De noodzaak van een toepassing beheerder geïnitieerde inschrijving en publicatieproces delegeren deze verantwoordelijken aan gebruikers worden genegeerd. Met Active Directory Federation Services (ADFS) is het waarschijnlijk dat een beheerder had een toepassing toevoegen als een relying party namens hun ontwikkelaars. Ontwikkelaars kunnen nu self-service.
* Gebruikers die zijn aangemeld bij toepassingen die gebruikmaken van hun organisatie-account voor zakelijke doeleinden is goed. Als ze later niet meer de organisatie verliezen ze automatisch toegang tot hun account in de toepassing die ze gebruikten.
* Is een record van welke gegevens die aan de toepassing een goed is is gedeeld. Gegevens is meer dan ooit vervoerbare en is het nuttig om een lege record van wie welke gegevens gedeeld met welke toepassingen hebt.
* API-eigenaars die Azure AD voor OAuth gebruiken bepalen precies welke machtigingen kunnen gebruikers zich te verlenen tot toepassingen en machtigingen die u moeten een beheerder om te accepteren. Alleen beheerders kunnen toestemming tot grotere scopes en meer machtigingen terwijl toestemming van de gebruiker is afgestemd op de gegevens van gebruikers zelf en mogelijkheden.
* Wanneer een gebruiker wordt toegevoegd of kan een toepassing te krijgen tot hun gegevens, kan de gebeurtenis worden gecontroleerd, zodat u kunt de rapporten Audit binnen de Azure-portal om te bepalen hoe een toepassing is toegevoegd aan de map weergeven.

Als u nog steeds te voorkomen dat gebruikers in uw directory toepassingen registreren en wilt aanmelden bij toepassingen zonder goedkeuring van de beheerder, zijn er twee instellingen die u wijzigen kunt als u wilt uitschakelen van deze mogelijkheden:

* Om te voorkomen dat gebruikers ermee akkoord dat toepassingen in hun eigen naam:
  1. In de Azure portal, gaat u naar de [gebruikersinstellingen](https://portal.azure.com/#blade/Microsoft_AAD_IAM/StartboardApplicationsMenuBlade/UserSettings/menuId/) sectie onder bedrijfstoepassingen.
  2. Wijziging **gebruikers kunnen instemmen met apps die toegang tot bedrijfsgegevens namens hen** naar **Nee**. 
      > [!NOTE]
      > Als u besluit uitschakelen toestemming van de gebruiker, is een beheerder worden vereist om toestemming naar een nieuwe toepassing die een gebruiker moet worden gebruikt.    
* Om te voorkomen dat gebruikers hun eigen toepassingen registreren:
  1. In de Azure portal, gaat u naar de [gebruikersinstellingen](https://portal.azure.com/#blade/Microsoft_AAD_IAM/ActiveDirectoryMenuBlade/UserSettings) sectie onder Azure Active Directory
  2. Wijziging **gebruikers toepassingen kunnen registreren** naar **Nee**.

> [!NOTE]
> Microsoft zichzelf gebruikt de standaardconfiguratie met kunnen registreren van toepassingen en instemmen met toepassingen die hun eigen namens gebruikers.

<!--Image references-->
[apps_service_principals_directory]:../media/active-directory-how-applications-are-added/HowAppsAreAddedToAAD.jpg

