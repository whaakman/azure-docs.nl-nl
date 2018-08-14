---
title: Wat is Azure Active Directory (Azure AD)? | Microsoft Docs
description: Gebruik Azure Active Directory om uw bestaande on-premises identiteiten uit te breiden in de cloud of om geïntegreerde Azure AD-toepassingen te ontwikkelen.
services: active-directory
documentationcenter: ''
author: eross-msft
manager: mtillman
ms.author: lizross
ms.assetid: 498820c4-9ebe-42be-bda2-ecf38cc514ca
ms.service: active-directory
ms.component: fundamentals
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: overview
ms.date: 04/09/2018
ms.custom: it-pro
ms.openlocfilehash: 5087f759d682382bc22b5f95f462fe0f08619cc8
ms.sourcegitcommit: 1d850f6cae47261eacdb7604a9f17edc6626ae4b
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/02/2018
ms.locfileid: "39449991"
---
# <a name="what-is-azure-active-directory"></a>What is Azure Active Directory? (Engelstalig)
Azure Active Directory (Azure AD) is de multitenant directory- en identiteitsbeheerservice van Microsoft in de cloud die belangrijke directory services, toegangsbeheer voor toepassingen en identiteitsbeveiliging in één oplossing combineert. Azure AD biedt ontwikkelaars tevens een uitgebreid op standaarden gebaseerd platform waarmee ze toegangsbeheer kunnen bieden voor hun toepassingen, op basis van gecentraliseerd beleid en regels.

![Azure AD Connect Stack](./media/active-directory-whatis/Azure_Active_Directory.png)

- **Voor IT-beheerders.** Azure AD biedt een veiligere oplossing voor uw organisatie dankzij het gebruik van sterker identiteitsbeheer en SSO-toegang (eenmalige aanmelding) tot duizenden [cloudgebaseerde SaaS-apps](../saas-apps/tutorial-list.md) en on-premises apps. Via deze apps verkrijgt u ook beveiliging van cloudgebaseerde apps, naadloze toegang, verbeterde samenwerking en automatisering van de identiteitslevenscyclus voor uw werknemers, die zowel de beveiliging als de naleving bevorderen.

    En daarnaast kunt u Azure AD met slechts [vier klikken](./../connect/active-directory-aadconnect-get-started-express.md) integreren met een bestaande Windows Server Active Directory, zodat uw organisatie uw bestaande on-premises identiteitsinvesteringen kan gebruiken om de toegang tot SaaS-apps te beheren.

- **Voor app-ontwikkelaars.** Met Azure AD kunt u zich focussen op het bouwen van apps doordat u ze kunt integreren met een oplossing voor identiteitsbeheer die door miljoenen organisaties over de hele wereld wordt gebruikt.

- **Voor klanten van Office 365, Azure of Dynamics CRM Online.** U gebruikt al Azure AD. Elke Office 365-, Azure- en Dynamics CRM Online-tenant is eigenlijk een Azure AD-tenant. U kunt dus meteen beginnen met het beheren van uw werknemerstoegang tot uw geïntegreerde cloud-apps.

## <a name="how-reliable-is-azure-ad"></a>Hoe betrouwbaar is Azure AD?
Het multitenant, geografisch gedistribueerde ontwerp met hoge beschikbaarheid van Azure AD-betekent dat u erop kunt vertrouwen voor uw meest kritieke zakelijke behoeften. Azure AD wordt uitgevoerd in 28 datacentrums over de hele wereld en heeft automatische failover. Dat geeft het vertrouwen dat Azure AD uiterst betrouwbaar is en zelfs als een datacentrum uitvalt, weet u dat kopieën van uw directorygegevens zijn opgeslagen in ten minste twee andere regionaal verspreide datacentrums en direct toegankelijk zijn.

Voor meer informatie over serviceovereenkomsten raadpleegt u [Service Level Agreements](https://azure.microsoft.com/support/legal/sla/).

## <a name="choose-an-edition"></a>Een editie kiezen
Alle Microsoft Online-bedrijfsservices zijn voor aanmelding en andere identiteitsbehoeften afhankelijk van Azure Active Directory (Azure AD). Als u zich abonneert op een van de zakelijke services van Microsoft Online (bijvoorbeeld Office 365 of Microsoft Azure), krijgt u Azure AD met toegang tot alle gratis functies. Met de versie van Azure Active Directory Free kunt u gebruikers en groepen beheren, synchroniseren met on-premises directory's en eenmalige aanmelding gebruiken voor Azure, Office 365 en duizenden populaire SaaS-toepassingen, zoals Salesforce, Workday, Concur, DocuSign, Google Apps, Box, ServiceNow, Dropbox en nog veel meer. 

Om uw Azure Active Directory te verbeteren, kunt u betaalde mogelijkheden toevoegen met de Azure Active Directory Basic-, Premium P1- en Premium P2-edities. De betaalde edities van Azure Active Directory zijn gebouwd op uw bestaande gratis directory. Ze bieden mogelijkheden van topkwaliteit op het gebied van onder andere selfservice, uitgebreide bewaking, beveiligingsrapportage, MFA (Multi-Factor Authentication) en beveiligde toegang voor uw mobiele werknemers.

> [!NOTE]
> Voor de prijsopties van deze edities raadpleegt u [Prijzen voor Azure Active Directory](https://azure.microsoft.com/pricing/details/active-directory/). Azure Active Directory Premium P1, P2 Premium en Azure Active Directory Basic worden momenteel niet ondersteund in China. Voor meer informatie over de prijzen voor Azure AD, kunt u contact opnemen met het Azure Active Directory-forum.
>

* **Azure Active Directory Basic**: deze editie is ontworpen voor taakwerkers met voornamelijk cloudbehoeften en biedt cloudgerichte toegang tot toepassingen en selfservice oplossingen voor identiteitsbeheer. Met de Basic-editie van Azure Active Directory beschikt u over functies die de productiviteit verhogen en de kosten verlagen, zoals op groepen gebaseerd toegangsbeheer, via selfservice wachtwoorden voor cloudtoepassingen opnieuw instellen en Azure AD-toepassingsproxy (voor het publiceren van on-premises webtoepassingen met behulp van Azure Active Directory), en dat alles wordt gegarandeerd door een SLA op ondernemingsniveau voor een bedrijfstijd van 99,9%.
* **Azure Active Directory Pemium P1**: deze editie is ontworpen voor organisaties met verdergaande behoeften op het gebied van identiteits- en toegangsbeheer. Azure Active Directory Premium-editie biedt extra mogelijkheden voor identiteitsbeheer op ondernemingsniveau, en stelt hybride gebruikers in staat naadloos toegang te krijgen tot on-premises functies en cloudfuncties. Deze editie omvat alles wat u nodig hebt voor informatiemedewerkers en identiteitsbeheerders in hybride omgevingen voor het verschaffen van toegang tot de diverse toepassingen, voor selfservice identiteits- en toegangsbeheer (IAM), identiteitsbescherming en beveiliging in de cloud. Deze biedt ondersteuning voor geavanceerde beheer en de delegatie van resources, zoals dynamische groepen en selfservicegroepsbeheer. Het bevat Microsoft Identity Manager (een pakket voor on-premises identiteits- en toegangsbeheer) en biedt mogelijkheden voor terugschrijven naar de cloud, die oplossingen als selfservice voor wachtwoordherstel voor uw on-premises gebruikers mogelijk maken.
* **Azure Active Directory Premium P2**: deze editie is ontworpen met geavanceerde beveiliging voor alle gebruikers en beheerders en bevat alle functionaliteit van Azure AD Premium P1, aangevuld met Azure AD Identity Protection en Privileged Identity Management. Met behulp van miljarden signalen biedt Azure Active Directory Identity Protection op risico's gebaseerde voorwaardelijke toegang tot uw toepassingen en essentiële bedrijfsgegevens. Met Azure Active Directory Privileged Identity Management helpen we u ook met het beheren en beveiligen van bevoegde accounts, zodat u beheerders en hun toegang kunt detecteren, beperken en bewaken en just-in-time-toegang kunt bieden wanneer dat nodig is.  

> [!NOTE]
> Een aantal mogelijkheden van Azure Active Directory zijn beschikbaar via 'betalen naar gebruik'-edities:
>
> * Active Directory B2C is de oplossing voor identiteits- en toegangsbeheer voor uw consumentgerichte toepassingen. Voor meer informatie raadpleegt u [Azure Active Directory B2C](https://azure.microsoft.com/documentation/services/active-directory-b2c/)
> * Azure Multi-Factor Authentication kan worden gebruikt per gebruiker of per verificatieprovider. Voor meer informatie raadpleegt u [Wat is Azure Multi-Factor Authentication?](../authentication/multi-factor-authentication.md)
>

## <a name="how-can-i-get-started"></a>Hoe ga ik aan de slag?

**Als u een IT-beheerder bent:**

* [Probeer het nu!](https://azure.microsoft.com/trial/get-started-active-directory/) - u kunt zich vandaag aanmelden voor een gratis proefperiode van 30 dagen en via deze koppeling binnen vijf minuten uw eerste cloudoplossing implementeren

* Lees [Aan de slag met Azure AD](https://docs.microsoft.com/azure/active-directory/active-directory-get-started-premium) voor tips en trucs voor het ophalen van een Azure AD-tenant en om snel aan de slag te gaan

**Als u een ontwikkelaar bent:**
 
* Bekijk de [Handleiding voor ontwikkelaars](../develop/azure-ad-developers-guide.md) voor Azure Active Directory

* [Start een gratis proefversie](https://azure.microsoft.com/trial/get-started-active-directory/): meld u vandaag nog aan voor een gratis proefversie van 30 dagen en begin met de integratie van uw apps met Azure AD

## <a name="next-steps"></a>Volgende stappen
[Meer informatie over de grondbeginselen van identiteits- en toegangsbeheer met Azure](https://docs.microsoft.com/azure/active-directory/identity-fundamentals)
