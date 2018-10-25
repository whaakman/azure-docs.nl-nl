---
title: Wat is Azure Active Directory (Azure AD)? | Microsoft Docs
description: Leer hoe de Azure Active Directory te gebruiken om uw bestaande on-premises identiteiten uit te breiden in de cloud of om geïntegreerde Azure AD-toepassingen te ontwikkelen.
services: active-directory
author: eross-msft
manager: mtillman
ms.author: lizross
ms.assetid: 498820c4-9ebe-42be-bda2-ecf38cc514ca
ms.service: active-directory
ms.component: fundamentals
ms.workload: identity
ms.topic: overview
ms.date: 09/13/2018
ms.custom: it-pro
ms.openlocfilehash: 406baeac60c7c0cdf5f74876e5fc29ea23d3d6f6
ms.sourcegitcommit: 5c00e98c0d825f7005cb0f07d62052aff0bc0ca8
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/24/2018
ms.locfileid: "49957544"
---
# <a name="what-is-azure-active-directory"></a>What is Azure Active Directory? (Engelstalig)
Azure Active Directory (Azure AD) is de multitenant map in de cloud en identiteitsbeheerservice van Microsoft. Azure AD combineert belangrijke adreslijstservices, toegangsbeheer voor toepassingen en identiteitsbeveiliging in één oplossing en biedt een op standaarden gebaseerd platform waarmee ze toegangsbeheer kunnen bieden aan hun apps, op basis van gecentraliseerd beleid en regels voor ontwikkelaars.

![Azure AD Connect Stack](./media/active-directory-whatis/Azure_Active_Directory.png)

## <a name="benefits-of-azure-ad"></a>Voordelen van Azure AD
Azure AD helpt u bij:

-   Voor elke gebruiker één identiteit in de hele onderneming te maken en te beheren, waardoor gebruikers, groepen en apparaten gesynchroniseerd blijven met [Azure AD Connect](../connect/active-directory-aadconnect.md).

-   Toegang bieden tot uw toepassingen via eenmalige aanmelding, inclusief duizenden vooraf geïntegreerde SaaS-apps, en veilige externe toegang bieden tot on-premises SaaS-toepassingen, met behulp van de [Azure AD-toepassingsproxy](../manage-apps/application-proxy.md).

-   Toegangsbeveiliging tot toepassingen toestaan door [Multi-Factor Authentication](../authentication/concept-mfa-howitworks.md) op basis van regels af te dwingen voor zowel on-premises toepassingen als cloud-apps.

-   De gebruikersproductiviteit verbeteren met [selfservice voor wachtwoordherstel](../user-help/user-help-reset-password.md) en toegangsaanvragen voor groepen en toepassingen met behulp van de [MyApps-portal](../user-help/active-directory-saas-access-panel-introduction.md).

-   Uw voordeel doen met de [hoge beschikbaarheid en betrouwbaarheid](https://docs.microsoft.com/azure/architecture/checklist/availability) van een wereldwijde zakelijke oplossing voor cloud-identiteit en toegangsbeheer.

## <a name="who-uses-azure-ad"></a>Wie gebruikt Azure AD
Azure AD is bedoeld voor IT-beheerders, app-ontwikkelaars, en voor gebruikers van Office 365, Azure of Dynamics CRM Online.

- **IT-beheerders** Azure AD biedt een veiligere oplossing voor uw organisatie dankzij het gebruik van sterker identiteitsbeheer en SSO-toegang (eenmalige aanmelding) tot duizenden [cloudgebaseerde SaaS-apps](../saas-apps/tutorial-list.md) en on-premises apps. Via deze apps verkrijgt u ook beveiliging van cloudgebaseerde apps, naadloze toegang, verbeterde samenwerking en automatisering van de identiteitslevenscyclus voor uw werknemers, die zowel de beveiliging als de naleving bevorderen.

    En daarnaast kunt u Azure AD met [Azure AD Connect](../connect/active-directory-aadconnect-get-started-express.md) integreren met een bestaande Windows Server Active Directory, zodat uw organisatie uw bestaande on-premises identiteitsinvesteringen kan gebruiken om de toegang tot SaaS-apps te beheren.

- **Voor app-ontwikkelaars.** Azure AD helpt u te focussen op het bouwen van apps door u integratie te verstrekken met een oplossing voor identiteitsbeheer die door miljoenen organisaties over de hele wereld wordt gebruikt.

- **Voor klanten van Office 365, Azure of Dynamics CRM Online.** U gebruikt al Azure AD. Elke Office 365-, Azure- en Dynamics CRM Online-tenant is eigenlijk een Azure AD-tenant. U kunt dus meteen beginnen met het beheren van uw werknemerstoegang tot uw geïntegreerde cloud-apps.

## <a name="how-reliable-is-azure-ad"></a>Hoe betrouwbaar is Azure AD?
Het multitenant, geografisch gedistribueerde ontwerp met hoge beschikbaarheid van Azure AD-ontwerp betekent dat u erop kunt vertrouwen voor uw meest kritieke zakelijke behoeften. Azure AD wordt uitgevoerd vanuit de datacenters 28 over de hele wereld met automatische failover. Dit betekent dat zelfs als een datacenter uitvalt, kopieën van uw mapgegevens live zijn in ten minste twee regionaal verspreide datacenters en beschikbaar voor directe toegang.

Voor meer informatie over serviceovereenkomsten raadpleegt u [Service Level Agreements](https://azure.microsoft.com/support/legal/sla/).

## <a name="choose-an-edition"></a>Een editie kiezen
Alle Microsoft Online-bedrijfsservices zijn voor aanmelding en andere identiteitsbehoeften afhankelijk van Azure AD. Als u zich abonneert op een van de zakelijke services van Microsoft Online (bijvoorbeeld Office 365 of Microsoft Azure), krijgt u automatisch Azure AD met toegang tot alle gratis functies. Met behulp van de versie van Azure Active Directory Free kunt u gebruikers en groepen beheren, synchroniseren met on-premises directory's en eenmalige aanmelding gebruiken voor Azure, Office 365 en duizenden populaire SaaS-apps, zoals Salesforce, Workday, Concur, DocuSign, Google Apps, Box, ServiceNow, Dropbox en nog veel meer. 

Om uw Azure AD-implementatie te verbeteren, kunt u ook betaalde mogelijkheden toevoegen door een upgrade naar de Azure Active Directory Basic-, Premium P1- of Premium P2-edities. De betaalde edities van Azure AD zijn gebouwd op uw bestaande gratis directory. Ze bieden mogelijkheden van topkwaliteit op het gebied van onder andere selfservice, uitgebreide bewaking, beveiligingsrapportage, MFA (Multi-Factor Authentication) en beveiligde toegang voor uw mobiele werknemers.

> [!NOTE]
> Voor de prijsopties van deze edities raadpleegt u [Prijzen voor Azure Active Directory](https://azure.microsoft.com/pricing/details/active-directory/). Azure Active Directory Premium P1, P2 Premium en Azure Active Directory Basic worden momenteel niet ondersteund in China. Voor meer informatie over de prijzen voor Azure AD, kunt u contact opnemen met het Azure Active Directory-forum.

- **Azure Active Directory Basic.** Bedoeld voor de cloud gerichte toegang voor toepassingen evenals oplossingen voor selfservice identiteitsbeheer. Met de Basic-editie beschikt u over functies die de productiviteit verhogen en de kosten verlagen, zoals op groepen gebaseerd toegangsbeheer, via selfservice wachtwoorden voor cloud-apps opnieuw instellen en Azure Active Directory-toepassingsproxy (voor het publiceren van on-premises webapps met behulp van Azure AD), en dat alles wordt gegarandeerd door een ondernemings-SLA voor een bedrijfstijd van 99,9%.

- **Azure Active Directory Premium P1.** Bedoeld voor organisaties met verdergaande behoeften op het gebied van identiteits- en toegangsbeheer. Azure Active Directory Premium-editie biedt extra mogelijkheden voor identiteitsbeheer op ondernemingsniveau, en stelt hybride gebruikers in staat naadloos toegang te krijgen tot on-premises functies en cloudfuncties. Deze editie omvat alles wat u nodig hebt voor informatiemedewerkers en identiteitsbeheerders in hybride omgevingen voor het verschaffen van toegang tot de diverse toepassingen, voor selfservice identiteits- en toegangsbeheer (IAM), identiteitsbescherming en beveiliging in de cloud. Deze biedt ondersteuning voor geavanceerde beheer en de delegatie van resources, zoals dynamische groepen en selfservicegroepsbeheer. Het bevat Microsoft Identity Manager (een pakket voor on-premises identiteits- en toegangsbeheer) en biedt mogelijkheden voor terugschrijven naar de cloud, die oplossingen als selfservice voor wachtwoordherstel voor uw on-premises gebruikers mogelijk maken.

- **Azure Active Directory Premium P2.** Ontworpen met geavanceerde beveiliging voor uw gebruikers en beheerders en bevat alle functionaliteit van Azure AD Premium P1, aangevuld met Azure AD Identity Protection en Privileged Identity Management. Met behulp van miljarden signalen biedt Azure Active Directory Identity Protection op risico's gebaseerde voorwaardelijke toegang tot uw apps en essentiële bedrijfsgegevens. Met Azure Active Directory Privileged Identity Management helpen we u ook met het beheren en beveiligen van bevoegde accounts, zodat u beheerders en hun toegang kunt detecteren, beperken en bewaken en just-in-time-toegang kunt bieden wanneer dat nodig is.  

> [!NOTE]
> Een aantal mogelijkheden van Azure Active Directory zijn ook beschikbaar via 'betalen naar gebruik'-edities:<ul><li>**Azure Active Directory B2C.** Identiteits- en toegangsbeheer voor uw consumentgerichte apps. Voor meer informatie raadpleegt u [Azure Active Directory B2C](https://azure.microsoft.com/documentation/services/active-directory-b2c/).</li><li>**Azure Multi-Factor Authentication.** Gebruikt per gebruiker of per authenticatie provider. Voor meer informatie raadpleegt u [Wat is Azure Multi-Factor Authentication?](../authentication/multi-factor-authentication.md).

## <a name="as-an-admin-how-do-i-get-started"></a>Hoe ga ik als administrator aan de slag?
Meld u aan voor een gratis proefversie van 30 dagen en implementeer uw eerste cloudoplossing, zie [Azure Active Directory Premium-proefversie](https://azure.microsoft.com/trial/get-started-active-directory/).

## <a name="as-a-developer-how-do-i-get-started"></a>Hoe ga ik als ontwikkelaar aan de slag?
Meld u aan voor een gratis proefversie van 30 dagen en begin met de integratie van uw apps met Azure AD, zie [Azure Active Directory Premium-proefversie](https://azure.microsoft.com/trial/get-started-active-directory/). Voor meer informatie kunt u ook de [Handleiding voor ontwikkelaars](../develop/v1-overview.md) voor Azure Active Directory raadplegen.

## <a name="next-steps"></a>Volgende stappen
- [Meer informatie over de grondbeginselen van identiteits- en toegangsbeheer met Azure](identity-fundamentals.md).

- [Azure AD integreren met Windows Server Active Directory](../hybrid/how-to-connect-install-express.md).
