---
title: Het verstrekken van veilige externe toegang tot lokale apps
description: Bevat informatie over hoe u Azure AD-toepassingsproxy gebruiken voor veilige externe toegang tot uw lokale apps.
services: active-directory
documentationcenter: 
author: daveba
manager: mtillman
ms.assetid: d5450da1-9e06-4d08-8146-011c84922ab5
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/04/2017
ms.author: daveba
ms.reviewer: harshja
ms.custom: it-pro
ms.openlocfilehash: 24def898f282c4e122ae53932ae86047e815595c
ms.sourcegitcommit: 3cdc82a5561abe564c318bd12986df63fc980a5a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/05/2018
---
# <a name="how-to-provide-secure-remote-access-to-on-premises-applications"></a>Het verstrekken van veilige externe toegang tot on-premises toepassingen

Werknemers willen vandaag om productief te zijn op een locatie op elk gewenst moment en vanaf elk apparaat. Ze willen werken op hun eigen apparaten, of deze nu tablets en telefoons en laptops. En ze toegang kunnen krijgen tot alle bijbehorende toepassingen, zowel SaaS-apps in de cloud en zakelijke apps on-premises verwachten. Toegang bieden tot on-premises toepassingen is traditioneel betrokken virtuele particuliere netwerken (VPN's) of demilitarized zones (DMZ's). Niet alleen deze oplossingen zijn complex en moeilijk te beveiligen, maar ze zijn kostbare instellen en beheren.

Er is een betere manier!

Een moderne medewerkers in de mobiele-eerste cloud eerste world moet een moderne RAS-oplossing. Azure AD-toepassingsproxy is een functie van Azure Active Directory biedt externe toegang als een service. Dit betekent dat het is eenvoudig te implementeren, gebruiken en beheren.

[!INCLUDE [identity](../../includes/azure-ad-licenses.md)]

## <a name="what-is-azure-active-directory-application-proxy"></a>Wat is Azure Active Directory-toepassingsproxy?
Azure AD-toepassingsproxy biedt eenmalige aanmelding (SSO) en veilige externe toegang voor webtoepassingen die lokaal worden gehost. Sommige apps die u wilt publiceren omvatten SharePoint-sites, Outlook Web Access of andere LOB webtoepassingen hebt. Deze on-premises webtoepassingen zijn geïntegreerd met Azure AD, de dezelfde identiteit en de besturingselement-platform dat wordt gebruikt door O365. Eindgebruikers hebben toegang tot uw on-premises toepassingen dezelfde manier als ze toegang tot O365 en andere SaaS-apps die is geïntegreerd met Azure AD. U hoeft niet te wijzigen van de netwerkinfrastructuur of VPN voor deze oplossing voor uw gebruikers vereisen.

## <a name="why-is-application-proxy-a-better-solution"></a>Waarom is een betere oplossing toepassingsproxy?
Azure AD-toepassingsproxy biedt een eenvoudige, rendabele en veilige externe toegang-oplossing voor uw on-premises toepassingen.

Azure AD-toepassingsproxy is:

* **Eenvoudige**
   * U hoeft niet te wijzigen of bijwerken van uw toepassingen werken met toepassingsproxy. 
   * Uw gebruikers krijgen een consistente verificatie-ervaring. De portal MyApps kan worden gebruikt om eenmalige aanmelding tot beide SaaS-apps in de cloud en uw apps on-premises. 
* **Beveiligen**
   * Wanneer u uw apps met behulp van Azure AD-toepassingsproxy publiceert, kunt u profiteren van de uitgebreide autorisatie besturingselementen en beveiligingsanalyse in Azure. U ophalen cloud-scale beveiligings- en Azure-beveiligingsfuncties zoals voorwaardelijke toegang en in twee stappen verificatie.
   * U hoeft niet te openen van alle binnenkomende verbindingen via uw firewall om uw gebruikers externe toegang. 
* **Rendabele**
   * Toepassingsproxy werkt in de cloud, zodat u tijd en geld kunt besparen. Oplossingen voor on-premises normaal gesproken moeten u instellen en onderhouden DMZ's, randservers of andere complexe infrastructuren.  

## <a name="what-kind-of-applications-work-with-application-proxy"></a>Wat voor soort toepassingen werken met toepassingsproxy?
Met Azure AD-toepassingsproxy hebt u toegang tot verschillende soorten interne toepassingen:

* Webtoepassingen die gebruikmaken van [geïntegreerde Windows-verificatie](active-directory-application-proxy-sso-using-kcd.md) voor verificatie  
* Webtoepassingen die gebruikmaken van op basis van formulieren of [op basis van een koptekst](application-proxy-ping-access.md) toegang  
* Web-API's die u wilt weergeven voor de uitgebreide toepassingen op verschillende apparaten  
* Toepassingen die worden gehost achter een [extern bureaublad-Gateway](application-proxy-publish-remote-desktop.md)  
* Rich client-apps die zijn geïntegreerd met de Active Directory Authentication Library (ADAL)

## <a name="how-does-application-proxy-work"></a>Hoe werkt de toepassingsproxy?
Er zijn twee onderdelen die u nodig hebt om te configureren zodat de toepassingsproxy werken: een connector en een extern eindpunt. 

De connector is een lichtgewicht agent die zich op een Windows-Server in uw netwerk. De connector vereenvoudigt het netwerkverkeer van de service voor toepassingsproxy in de cloud met uw toepassing on-premises. Uitgaande verbindingen wordt alleen gebruikt zodat u niet hoeft te openen van poorten voor inkomend verkeer of alles in het Perimeternetwerk plaatst. De connectors zijn staatloze en ophalen uit de cloud indien nodig. Voor meer informatie over connectors worden, zoals hoe ze verdelen en worden geverifieerd, Zie [inzicht in Azure AD-toepassingsproxy connectors](application-proxy-understand-connectors.md). 

Het externe eindpunt is hoe uw gebruikers bereiken voor uw toepassingen terwijl buiten uw netwerk. Ze kunnen ofwel rechtstreeks naar een externe URL die u wilt bepalen of toegang te krijgen tot de toepassing via de portal MyApps. Wanneer gebruikers naar een van deze eindpunten gaat, worden ze zich verifiëren bij Azure AD en vervolgens worden gerouteerd via de connector van de on-premises toepassing.

 ![Toepassingsproxy AzureAD-diagram](./media/active-directory-application-proxy-get-started/azureappproxxy.png)

1. De gebruiker toegang heeft tot de toepassing via de toepassingsproxy-service en wordt omgeleid naar de aanmeldingspagina van Azure AD om te verifiëren.
2. Na een geslaagde aanmelden, is een token gegenereerd en verzonden naar het clientapparaat.
3. De client stuurt het token naar de service-toepassingsproxy, die de user principal name (UPN) en de beveiligings-principal name (SPN) opgehaald uit het token en stuurt de aanvraag naar de connector voor toepassingsproxy.
4. Als u eenmalige aanmelding hebt geconfigureerd, voert de connector aanvullende verificatie namens de gebruiker vereist.
5. De connector stuurt de aanvraag naar de on-premises toepassing.  
6. Het antwoord wordt verzonden via de Webtoepassingsproxy-service en -connector voor de gebruiker.

### <a name="single-sign-on"></a>Eenmalige aanmelding
Azure AD-toepassingsproxy biedt eenmalige aanmelding (SSO) aan toepassingen die gebruikmaken van geïntegreerde Windows-verificatie (IWA) of de claimbewuste toepassingen. Als uw toepassing gebruikmaakt van IWA, imiteert toepassingsproxy de gebruiker die het gebruik van Kerberos-beperkte overdracht voor een eenmalige aanmelding. Als u een claimbewuste toepassing die door Azure Active Directory wordt vertrouwd hebt, werkt eenmalige aanmelding, omdat de gebruiker is al geverifieerd door Azure AD.

Zie voor meer informatie over Kerberos [alle u wilt weten over Kerberos-beperkt delegatie (KCD)](https://blogs.technet.microsoft.com/applicationproxyblog/2015/09/21/all-you-want-to-know-about-kerberos-constrained-delegation-kcd).

### <a name="managing-apps"></a>Apps beheren
Als uw app met toepassingsproxy is gepubliceerd, kunt u hem kunt beheren als elke andere enterprise-app in de Azure portal. U kunt u Azure Active Directory-beveiligingsfuncties zoals voorwaardelijke toegang en in twee stappen verificatie, gebruikersmachtigingen beheren en de huisstijl voor uw app aanpassen. 

## <a name="get-started"></a>Aan de slag

Voordat u Application Proxy configureert, controleert u of er een ondersteunde [Azure Active Directory-editie](https://azure.microsoft.com/pricing/details/active-directory/) en een Azure AD-directory waarvoor u een globale beheerder bent.

Aan de slag met toepassingsproxy in twee stappen:

1. [Toepassingsproxy inschakelen en configureren van de connector](active-directory-application-proxy-enable.md).    
2. [Toepassingen publiceren](active-directory-application-proxy-publish.md) -gebruik de wizard snel en eenvoudig om uw apps lokale gepubliceerde en toegankelijk is op afstand.

## <a name="whats-next"></a>Volgende stappen
Nadat u uw eerste app publiceert, vindt u veel meer u met toepassingsproxy doen kunt.

* [Eenmalige aanmelding inschakelen](active-directory-application-proxy-sso-using-kcd.md)
* [Toepassingen publiceren met uw eigen domeinnaam](active-directory-application-proxy-custom-domains.md)
* [Meer informatie over Azure AD-toepassingsproxy connectors](application-proxy-understand-connectors.md)
* [Werken met bestaande lokale proxyservers](application-proxy-working-with-proxy-servers.md) 
* [Een aangepaste startpagina ingesteld](application-proxy-office365-app-launcher.md)

Ga naar het [blog over toepassingsproxy](http://blogs.technet.com/b/applicationproxyblog/) voor nieuws en updates.

