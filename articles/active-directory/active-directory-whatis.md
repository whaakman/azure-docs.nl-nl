---
title: What is Azure Active Directory? (Engelstalig)
description: "Gebruik Azure Active Directory om uw bestaande on-premises identiteiten in de cloud uitbreiden of het ontwikkelen van Azure AD geïntegreerde toepassingen."
services: active-directory
documentationcenter: 
author: jeffgilb
manager: mtillman
ms.reviewer: jsnow
ms.author: jeffgilb
ms.assetid: 498820c4-9ebe-42be-bda2-ecf38cc514ca
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/17/2017
ms.custom: it-pro
ms.openlocfilehash: e5eafd4d25d2638ab5d9f904a7e0c00b36501d68
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/11/2017
---
# <a name="what-is-azure-active-directory"></a>What is Azure Active Directory? (Engelstalig)
Azure Active Directory (Azure AD) is de cloud gebaseerde directory en identity management-service, Microsoft van meerdere tenants. Azure AD combineert core directoryservices, governance geavanceerde identiteit en toegang Toepassingsbeheer. Azure AD biedt ook een rijke, op standaarden gebaseerd platform waarmee ontwikkelaars toegangsbeheer leveren aan hun toepassingen, op basis van regels en gecentraliseerde beleid. 

Voor IT-beheerders Azure AD biedt een oplossing voor betaalbare, eenvoudig te gebruiken om werknemers en zakelijke partners eenmalige aanmelding (SSO) toegang geven tot [duizenden cloud SaaS-toepassingen](active-directory-saas-tutorial-list.md) , zoals Office365, Salesforce.com, DropBox en Concur.

Voor ontwikkelaars van toepassingen, Azure AD kunt u zich richten op het bouwen van uw toepassing met zodat u snel en eenvoudig te integreren met een wereld klasse oplossing voor identiteitsbeheer die wordt gebruikt door miljoenen organisaties over de hele wereld.

Azure AD bevat ook een volledige reeks mogelijkheden voor identiteitsbeheer zoals multi-factor authentication-server, apparaatregistratie, selfservice voor wachtwoordherstel management, self-service groepsbeheer, bevoegd accountmanagement, op rollen gebaseerd toegangsbeheer , gebruik bewaking, uitgebreide controle en beveiligingsbewaking en waarschuwingen. Deze mogelijkheden kunnen helpen beveiligen in de cloud-toepassingen, IT-processen te stroomlijnen, kosten knippen en ervoor te zorgen dat de doelstellingen van de naleving wordt voldaan.

Bovendien met just [vier klikken](./connect/active-directory-aadconnect-get-started-express.md), Azure AD kan worden geïntegreerd met een bestaande Windows Server Active Directory, zodat organisaties kunnen gebruikmaken van hun bestaande lokale investeringen identiteit voor het beheren van toegang tot cloud gebaseerde SaaS-toepassingen.

Als u een Office 365, Azure of Dynamics CRM Online-klant bent, beseft u waarschijnlijk niet dat u al van Azure AD gebruikmaakt. Elke Office 365, Azure en Dynamics CRM-tenant is daadwerkelijk al een Azure AD-tenant. Als u wilt, dat kunt u gaan die tenant te gebruiken voor het beheren van toegang tot duizenden andere cloudtoepassingen, Azure AD kan worden geïntegreerd met!

![Azure AD Connect Stack](./media/active-directory-whatis/Azure_Active_Directory.png)

## <a name="how-reliable-is-azure-ad"></a>Hoe betrouwbaar is Azure AD?
Het ontwerp van multitenant, geografisch verspreide, hoge beschikbaarheid van Azure AD-betekent dat u erop voor uw meest kritieke zakelijke behoeften vertrouwen kunt. Bijna 28 datacenters over de hele wereld met automatische failover, hebt u de deur van weten dat Azure AD maximaal betrouwbaar is en dat zelfs als een datacenter uitvalt, kopieën van uw directorygegevens live in ten minste twee meer regionaal zijn verspreide gegevens draait en beschikbaar voor directe toegang.

Zie voor meer informatie [Service Level Agreements](https://azure.microsoft.com/support/legal/sla/).

## <a name="choose-an-edition"></a>Kies een editie
Alle zakelijke services van Microsoft Online zijn afhankelijk van de Azure Active Directory (Azure AD) voor aanmelding en andere identiteit. Als u zich abonneert op een van de Microsoft Online zakelijke services (bijvoorbeeld Office 365 of Microsoft Azure), krijgt u Azure AD met toegang tot alle van de gratis functies. Met de editie Azure Active Directory Free, kunt u gebruikers en groepen beheren, synchroniseren met on-premises adreslijsten, eenmalige aanmelding in Azure, Office 365 en duizenden populaire SaaS-toepassingen zoals Salesforce, Workday, Concur, DocuSign, Google ophalen Apps, Box, ServiceNow, Dropbox en meer. 

Voor het verbeteren van uw Azure Active Directory, kunt u betaald mogelijkheden met de Azure Active Directory Basic, Premium P1 en Premium P2-edities kunt toevoegen. Azure Active Directory betaald edities zijn gebaseerd op uw bestaande gratis directory het bieden van enterprise klasse spanning selfservice uitgebreide controle, beveiliging reporting multi-factor Authentication (MFA) en veilige toegang van uw mobiele werknemers.

> [!NOTE]
> Zie voor de prijscategorie opties van deze edities [Azure Active Directory prijzen](https://azure.microsoft.com/pricing/details/active-directory/). Azure Active Directory Premium-P1 Premium P2 en Azure Active Directory Basic worden momenteel niet ondersteund in China. Neem contact op met de Azure Active Directory-Forum voor meer informatie.
>

* **Azure Active Directory Basic** -ontworpen voor taakgerichte medewerkers met cloud eerste behoeften en deze editie biedt cloud gericht toepassing toegang en selfservice oplossingen voor identiteitsbeheer. Met de Basic-editie van Azure Active Directory beschikt u over functies die de productiviteit verhogen en de kosten verlagen, zoals op groepen gebaseerd toegangsbeheer, via selfservice wachtwoorden voor cloudtoepassingen opnieuw instellen en Azure Active Directory Application Proxy (voor het publiceren van on-premises webtoepassingen met behulp van Azure Active Directory), en dat alles wordt gegarandeerd door een SLA op ondernemingsniveau voor een bedrijfstijd van 99,9%.
* **Azure Active Directory Premium-P1** - ontworpen om te zorgen dat organisaties met meer veeleisende identiteits-en toegang nodig heeft, Azure Active Directory Premium edition toegevoegd mogelijkheden voor identiteitsbeheer voor uitgebreide functionaliteit op bedrijfsniveau en Hiermee kunnen gebruikers naadloos toegang tot on-premises en in de cloud mogelijkheden hybride. Deze editie omvat alles wat u nodig hebt voor informatiemedewerkers en identiteitsbeheerders in hybride omgevingen voor het verschaffen van toegang tot de diverse toepassingen, voor selfservice identiteits- en toegangsbeheer (IAM), identiteitsbescherming en beveiliging in de cloud. Het ondersteunt beheer en overdracht bronnen zoals dynamische groepen en groepsbeheer met selfservice. Dit omvat Microsoft Identity Manager (een lokale identiteits- en toegangsbeheer management suite) en biedt cloud terugschrijven inschakelen oplossingen zoals selfservice voor wachtwoordherstel voor uw on-premises gebruikers.
* **Azure Active Directory Premium-P2** -ontworpen met geavanceerde beveiliging voor alle gebruikers en beheerders, bevat deze nieuwe aanbieding alle mogelijkheden in Azure AD Premium-P1 evenals onze nieuwe Identity Protection en Privileged Identity Beheer. Azure Active Directory: Identity Protection maakt gebruik van miljarden signalen risico gebaseerde voorwaardelijke toegang tot uw toepassingen en essentiële bedrijfsgegevens te verlenen. We ook helpen u te beheren en beveiligen van bevoegde accounts met Azure Active Directory Privileged Identity Management, zodat u detecteren kunt, beperken en beheerders en toegang krijgen tot resources bewaken en just-in-time-toegang wanneer deze nodig is.  

> [!NOTE]
> Een aantal Azure Active Directory-mogelijkheden zijn beschikbaar via de edities 'omslagstelsel':
>
> * Active Directory B2C is de oplossing voor identiteits- en toegangsbeheer voor uw consumententoepassingen. Zie voor meer informatie [Azure Active Directory B2C](https://azure.microsoft.com/documentation/services/active-directory-b2c/)
> * Azure multi-factor Authentication kan worden gebruikt via per gebruiker of per verificatieproviders. Zie voor meer informatie [wat is Azure multi-factor Authentication?](../multi-factor-authentication/multi-factor-authentication.md)
>

## <a name="how-can-i-get-started"></a>Hoe kan ik aan de slag?

**Als u een IT-beheerder:**

* [Probeer het nu!](https://azure.microsoft.com/trial/get-started-active-directory/) -u kunt nu registreren voor een gratis proefversie van 30 dagen en implementeren van uw eerste cloudoplossing onder 5 minuten via deze koppeling

* Lees [aan de slag met Azure AD](https://docs.microsoft.com/azure/active-directory/active-directory-get-started-premium) voor actief en werkend snelle tips en trucs over het ophalen van een Azure AD tenant.

**Als u een ontwikkelaar:**
 
* Bekijk onze [-handleiding voor ontwikkelaars](active-directory-developers-guide.md) aan Azure Active Directory

* [Start een proefversie](https://azure.microsoft.com/trial/get-started-active-directory/) : vandaag registreren voor een gratis proefversie van 30 dagen en start het integreren van uw apps met Azure AD

## <a name="next-steps"></a>Volgende stappen
[Meer informatie over de grondbeginselen van Azure management voor identiteits- en toegangsbeheer](https://docs.microsoft.com/azure/active-directory/identity-fundamentals)
