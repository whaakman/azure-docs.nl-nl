---
title: Wat is Azure Active Directory (Azure AD)? | Microsoft Docs
description: Gebruik Azure Active Directory om uw bestaande on-premises identiteiten in de cloud uitbreiden of het ontwikkelen van Azure AD geïntegreerde toepassingen.
services: active-directory
documentationcenter: ''
author: eross-msft
manager: mtillman
ms.author: lizross
ms.assetid: 498820c4-9ebe-42be-bda2-ecf38cc514ca
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/09/2018
ms.custom: it-pro
ms.openlocfilehash: 003ce2edda3e2069eb7e05f58ecc2e208c818946
ms.sourcegitcommit: 9cdd83256b82e664bd36991d78f87ea1e56827cd
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/16/2018
---
# <a name="what-is-azure-active-directory"></a>What is Azure Active Directory? (Engelstalig)
Azure Active Directory (Azure AD) is de map voor multitenant en cloudservices van Microsoft en de identity management-service die core directoryservices, beheer van de toepassing toegang en identiteitsbeveiliging in één oplossing combineert. Azure AD biedt ook een rijke, op standaarden gebaseerd platform waarmee ontwikkelaars toegangsbeheer leveren aan hun toepassingen, op basis van regels en gecentraliseerde beleid.

![Azure AD Connect Stack](./media/active-directory-whatis/Azure_Active_Directory.png)

- **Voor IT-beheerders.** Azure AD levert een veiligere oplossing voor uw organisatie met behulp van de sterkere identiteits- en eenmalige aanmelding (SSO) toegang tot duizenden [SaaS-apps cloud-gebaseerde](active-directory-saas-tutorial-list.md) en on-premises apps. Via deze apps krijgt u ook op basis van cloud app security, naadloze toegang, verbeterde samenwerking en automatisering van de levenscyclus van de identiteit voor uw medewerkers, het verbeteren van zowel beveiliging en naleving.

    Bovendien met just [vier klikken](./connect/active-directory-aadconnect-get-started-express.md), kunt u Azure AD integreren met een bestaande Windows Server Active Directory, zodat uw organisatie gebruikt uw bestaande on-premises identity investeringen voor het beheren van cloud-gebaseerde SaaS app toegang tot .

- **Voor app-ontwikkelaars.** Azure AD kunt die u zich richten op het bouwen van uw apps doordat die u integreren met een oplossing voor identiteitsbeheer die wordt gebruikt door miljoenen organisaties over de hele wereld.

- **Voor Office 365, Azure of Dynamics CRM Online-klanten.** U al gebruikmaakt van Azure AD. Elke Office 365, Azure en Dynamics CRM Online-tenant is daadwerkelijk een Azure AD-tenant, zodat u onmiddellijk starten voor het beheren van uw werknemers-toegang hebben tot uw geïntegreerde cloud-apps.

## <a name="how-reliable-is-azure-ad"></a>Hoe betrouwbaar is Azure AD?
Het ontwerp van multitenant, geografisch verspreide, hoge beschikbaarheid van Azure AD-betekent dat u erop voor uw meest kritieke zakelijke behoeften vertrouwen kunt. Bijna 28 datacenters over de hele wereld met automatische failover, hebt u de deur van weten dat Azure AD maximaal betrouwbaar is en dat zelfs als een datacenter uitvalt, kopieën van uw directorygegevens live in ten minste twee meer regionaal zijn verspreide gegevens draait en beschikbaar voor directe toegang.

Zie voor meer informatie over serviceovereenkomsten [Service Level Agreements](https://azure.microsoft.com/support/legal/sla/).

## <a name="choose-an-edition"></a>Kies een editie
Alle zakelijke services van Microsoft Online zijn afhankelijk van de Azure Active Directory (Azure AD) voor aanmelding en andere identiteit. Als u zich abonneert op een van de Microsoft Online zakelijke services (bijvoorbeeld Office 365 of Microsoft Azure), krijgt u Azure AD met toegang tot alle van de gratis functies. Met de editie Azure Active Directory Free, kunt u gebruikers en groepen beheren, synchroniseren met on-premises adreslijsten, eenmalige aanmelding in Azure, Office 365 en duizenden populaire SaaS-toepassingen zoals Salesforce, Workday, Concur, DocuSign, Google ophalen Apps, Box, ServiceNow, Dropbox en meer. 

Voor het verbeteren van uw Azure Active Directory, kunt u betaald mogelijkheden met de Azure Active Directory Basic, Premium P1 en Premium P2-edities kunt toevoegen. Azure Active Directory betaald edities zijn gebaseerd op uw bestaande gratis directory het bieden van enterprise klasse spanning selfservice uitgebreide controle, beveiliging reporting multi-factor Authentication (MFA) en veilige toegang van uw mobiele werknemers.

> [!NOTE]
> Zie voor de prijscategorie opties van deze edities [Azure Active Directory prijzen](https://azure.microsoft.com/pricing/details/active-directory/). Azure Active Directory Premium-P1 Premium P2 en Azure Active Directory Basic worden momenteel niet ondersteund in China. Voor meer informatie over prijzen voor Azure AD, kunt u contact op met de Azure Active Directory-Forum.
>

* **Azure Active Directory Basic** -ontworpen voor taakgerichte medewerkers met cloud eerste behoeften en deze editie biedt cloud gericht toegang en selfservice toepassingsidentiteit oplossingen. Met de Basic-editie van Azure Active Directory beschikt u over functies die de productiviteit verhogen en de kosten verlagen, zoals op groepen gebaseerd toegangsbeheer, via selfservice wachtwoorden voor cloudtoepassingen opnieuw instellen en Azure Active Directory Application Proxy (voor het publiceren van on-premises webtoepassingen met behulp van Azure Active Directory), en dat alles wordt gegarandeerd door een SLA op ondernemingsniveau voor een bedrijfstijd van 99,9%.
* **Azure Active Directory Premium-P1** - ontworpen om te zorgen dat organisaties met meer veeleisende identiteits-en toegang nodig heeft, Azure Active Directory Premium edition toegevoegd mogelijkheden voor identiteitsbeheer voor uitgebreide functionaliteit op bedrijfsniveau en Hiermee kunnen gebruikers naadloos toegang tot on-premises en in de cloud mogelijkheden hybride. Deze versie bevat alles wat die u nodig hebt voor de Informatiemedewerker en beheerders van de identiteit in hybride omgevingen tussen beveiliging van toegang tot toepassingen en Self-service identiteitsbeheer toegangsbeheer (IAM), identiteit en beveiliging in de cloud. Het ondersteunt beheer en overdracht bronnen zoals dynamische groepen en groepsbeheer met selfservice. Dit omvat Microsoft Identity Manager (een lokale identiteits- en toegangsbeheer management suite) en biedt cloud terugschrijven inschakelen oplossingen zoals selfservice voor wachtwoordherstel voor uw on-premises gebruikers.
* **Azure Active Directory Premium-P2** -ontworpen met geavanceerde beveiliging voor alle gebruikers en beheerders, bevat deze nieuwe aanbieding alle mogelijkheden in Azure AD Premium-P1 Identity Protection en Privileged Identity Management. Azure Active Directory: Identity Protection maakt gebruik van miljarden signalen risico gebaseerde voorwaardelijke toegang tot uw toepassingen en essentiële bedrijfsgegevens te verlenen. We ook helpen u te beheren en beveiligen van bevoegde accounts met Azure Active Directory Privileged Identity Management, zodat u detecteren kunt, beperken, en beheerders en toegang krijgen tot resources bewaken en just-in-time-toegang wanneer deze nodig is.  

> [!NOTE]
> Een aantal Azure Active Directory-mogelijkheden zijn beschikbaar via de edities 'omslagstelsel':
>
> * Active Directory B2C is de oplossing voor identiteits- en toegangsbeheer voor uw consumententoepassingen. Zie voor meer informatie [Azure Active Directory B2C](https://azure.microsoft.com/documentation/services/active-directory-b2c/)
> * Azure multi-factor Authentication kan worden gebruikt via per gebruiker of per verificatieproviders. Zie voor meer informatie [wat is Azure multi-factor Authentication?](../multi-factor-authentication/multi-factor-authentication.md)
>

## <a name="how-can-i-get-started"></a>Hoe kan ik aan de slag?

**Als u een IT-beheerder:**

* [Probeer het nu!](https://azure.microsoft.com/trial/get-started-active-directory/) -u kunt zich aanmelden voor een gratis proefversie van 30 dagen vandaag en implementeren van uw eerste cloudoplossing in onder vijf minuten via deze koppeling

* Lees [aan de slag met Azure AD](https://docs.microsoft.com/azure/active-directory/active-directory-get-started-premium) voor actief en werkend snelle tips en trucs over het ophalen van een Azure AD tenant.

**Als u een ontwikkelaar:**
 
* Bekijk de [-handleiding voor ontwikkelaars](active-directory-developers-guide.md) aan Azure Active Directory

* [Start een proefversie](https://azure.microsoft.com/trial/get-started-active-directory/) : aanmelden voor een gratis proefversie van 30 dagen vandaag en start het integreren van uw apps met Azure AD

## <a name="next-steps"></a>Volgende stappen
[Meer informatie over de grondbeginselen van Azure management voor identiteits- en toegangsbeheer](https://docs.microsoft.com/azure/active-directory/identity-fundamentals)
