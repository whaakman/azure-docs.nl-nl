---
title: Het bieden van veilige externe toegang tot on-premises toepassingen
description: Bevat informatie over het gebruik van Azure AD Application Proxy voor veilige externe toegang tot uw on-premises toepassingen.
services: active-directory
documentationcenter: ''
author: barbkess
manager: mtillman
ms.service: active-directory
ms.component: app-mgmt
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 01/31/2018
ms.author: barbkess
ms.reviewer: harshja
ms.custom: it-pro
ms.openlocfilehash: ec5c75b5de912988efeb5167107f6d0dfe07da2e
ms.sourcegitcommit: 78ec955e8cdbfa01b0fa9bdd99659b3f64932bba
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/10/2018
ms.locfileid: "53139948"
---
# <a name="how-to-provide-secure-remote-access-to-on-premises-applications"></a>Het bieden van veilige externe toegang tot on-premises toepassingen

De huidige werknemer wil overal, op elke plek en op elk apparaat productief kunnen zijn. Ze willen werken op hun eigen apparaten, ongeacht of ze tablets of telefoons laptops. En ze verwachten dat ze toegang krijgen tot al hun toepassingen, zowel SaaS-apps in de cloud en zakelijke apps on-premises. Voorheen was bij het toegang bieden tot on-premises toepassingen een VPN (Virtual Private Network) of een DMZ (gedemilitariseerde zone) nodig. Dit is niet alleen ingewikkeld en moeilijk te beveiligen, maar het instellen en beheren ervan is duur.

Er is een betere manier.

Een moderne medewerkers van de mobiliteit, cloudgeoriënteerde wereld moet een moderne RAS-oplossing. Azure AD Application Proxy is een functie van Azure Active Directory biedt externe toegang als een service. Dit betekent dat het is eenvoudig te implementeren, gebruiken en beheren.

[!INCLUDE [identity](../../../includes/azure-ad-licenses.md)]

## <a name="what-is-azure-active-directory-application-proxy"></a>Wat is Azure Active Directory Application Proxy?
Azure AD-toepassingsproxy biedt eenmalige aanmelding (SSO) en veilige externe toegang voor webtoepassingen die on-premises gehost. Sommige apps die u wilt publiceren bevatten SharePoint-sites, Outlook Web Access of een andere LOB-webtoepassingen hebt. Deze on-premises webtoepassingen zijn geïntegreerd met Azure AD, de dezelfde identiteit en de controle-platform dat wordt gebruikt door O365. Eindgebruikers hebben toegang tot uw on-premises toepassingen dezelfde manier als ze toegang krijgen tot O365 en andere SaaS-apps die is geïntegreerd met Azure AD. U hoeft niet te wijzigen van de infrastructuur van het netwerk of VPN voor deze oplossing voor uw gebruikers vereisen.

## <a name="why-is-application-proxy-a-better-solution"></a>Waarom is een betere oplossing Application Proxy?
Azure AD-toepassingsproxy biedt een eenvoudige, veilige en voordelige RAS-oplossing naar uw on-premises toepassingen.

Azure AD Application Proxy is:

* **Eenvoudige**
   * U hoeft niet te wijzigen of bijwerken van uw toepassingen werken met Application Proxy. 
   * Uw gebruikers krijgen een consistente verificatie-ervaring. Ze kunnen de MyApps-portal gebruiken om op te halen single sign-on bij beide SaaS-apps in de cloud en uw apps on-premises. 
* **Beveiligen**
   * Wanneer u uw apps met Azure AD-toepassingsproxy publiceert, kunt u profiteren van de uitgebreide besturingselementen voor autorisatie en de beveiligingsanalyses in Azure. U profiteert van schaalbare security en Azure-beveiliging-functies zoals voorwaardelijke toegang en verificatie in twee stappen verificatie.
   * U hebt geen inkomende verbindingen via de firewall, zodat uw gebruikers RAS openen. 
* **Cost-effective**
   * Application Proxy werkt in de cloud, zodat u tijd en geld kunt besparen. On-premises oplossingen normaal gesproken moeten u instellen en onderhouden van DMZ's, randservers of andere ingewikkelde infrastructuren.  

## <a name="what-kind-of-applications-work-with-application-proxy"></a>Wat voor soort Apps werken met Application Proxy?
Met Azure AD-toepassingsproxy, kunt u verschillende soorten interne toepassingen openen:

* Webtoepassingen die gebruikmaken van [geïntegreerde Windows-verificatie](application-proxy-configure-single-sign-on-with-kcd.md) voor verificatie  
* Webtoepassingen die gebruikmaken van formulier-gebaseerde of [headers gebaseerde](application-proxy-configure-single-sign-on-with-ping-access.md) toegang  
* Web-API's die u wilt weergeven voor de uitgebreide toepassingen op verschillende apparaten  
* Toepassingen die worden gehost achter een [extern bureaublad-Gateway](application-proxy-integrate-with-remote-desktop-services.md)  
* Rich client-apps die kunnen worden geïntegreerd met de Active Directory Authentication Library (ADAL)

## <a name="how-does-application-proxy-work"></a>Hoe werkt Application Proxy?
Er zijn twee onderdelen die u nodig hebt om te configureren zodat Application Proxy werken: een connector en een eindpunt. 

De connector is een lichtgewicht agent die bevindt zich op een Windows-Server in uw netwerk. De connector vereenvoudigt het uitvoeren van de verkeersstroom van de service voor toepassingsproxy in de cloud naar uw toepassing on-premises. Uitgaande verbindingen wordt alleen gebruikt, zodat u hoeft inkomende poorten openen of iets in de DMZ te plaatsen. De connectors zijn staatloos en ophalen van informatie vanuit de cloud naar behoefte. Voor meer informatie over connectors, zoals hoe ze verdelen en geverifieerd, Zie [over Azure AD Application Proxy connectors](application-proxy-connectors.md). 

Het eindpunt mag een URL of een [eindgebruikersportal](end-user-experiences.md). Gebruikers kunnen toepassingen terwijl buiten uw netwerk bereiken door het openen van een externe URL. Gebruikers in uw netwerk kunnen de toepassing openen via een URL of een eindgebruikersportal. Wanneer gebruikers naar een van deze eindpunten gaat, worden ze verifiëren in Azure AD en vervolgens worden gerouteerd via de connector op de on-premises toepassing.

 ![AzureAD Application Proxy-diagram](./media/application-proxy/azureappproxxy.png)

1. Nadat de gebruiker toegang gekregen de toepassing via een eindpunt tot heeft, wordt de gebruiker omgeleid naar de aanmeldingspagina van Azure AD. 
2. Na een geslaagde aanmelding, is een token gegenereerd en verzonden naar de client-apparaat van de gebruiker.
3. De client stuurt het token naar de Application Proxy-service, die wordt opgehaald van de user principal name (UPN) en de naam van de beveiligingsprincipal (SPN) van het token en stuurt de aanvraag naar de Application Proxy-connector.
4. Als u eenmalige aanmelding hebt geconfigureerd, voert de connector aanvullende verificatie namens de gebruiker vereist.
5. De connector stuurt de aanvraag naar de on-premises toepassing.  
6. Het antwoord is verzonden via de service voor toepassingsproxy en connector voor de gebruiker.

### <a name="single-sign-on"></a>Eenmalige aanmelding
Azure AD-toepassingsproxy biedt eenmalige aanmelding (SSO) voor toepassingen die gebruikmaken van geïntegreerde Windows-verificatie (IWA) of de claimbewuste toepassingen. Als uw toepassing gebruikmaakt van IWA, imiteert Application Proxy de gebruiker met beperkte Kerberos-delegering bieden van eenmalige aanmelding. Als u een claimbewuste toepassing die door Azure Active Directory wordt vertrouwd hebt, wordt eenmalige aanmelding werkt, omdat de gebruiker is al geverifieerd door Azure AD.

Zie voor meer informatie over Kerberos, [alle u wilt weten over Kerberos-beperkte delegatie (KCD)](https://blogs.technet.microsoft.com/applicationproxyblog/2015/09/21/all-you-want-to-know-about-kerberos-constrained-delegation-kcd).

### <a name="managing-apps"></a>Apps beheren
Zodra uw app is gepubliceerd met toepassingsproxy, kunt u deze kunt beheren, zoals elke andere enterprise-app in Azure portal. U kunt Azure Active Directory-beveiligingsfuncties, zoals voorwaardelijke toegang en verificatie in twee stappen verificatie gebruiken, gebruikersmachtigingen beheren en de huisstijl van uw app aanpassen. 

## <a name="get-started"></a>Aan de slag

Voordat u Application Proxy configureert, controleert u of u hebt een ondersteunde [editie van Azure Active Directory](https://azure.microsoft.com/pricing/details/active-directory/) en een Azure AD-directory waarvan u een globale beheerder zijn.

Aan de slag met Application Proxy in twee stappen:

1. [Toepassingsproxy inschakelen en configureren van de connector](application-proxy-add-on-premises-application.md).    
2. [Toepassingen publiceren](application-proxy-add-on-premises-application.md) -gebruik van de wizard snel en eenvoudig uw on-premises toepassingen gepubliceerde en toegankelijk is op afstand ophalen.

## <a name="whats-next"></a>Volgende stappen
Nadat u uw eerste app publiceert, is er veel meer u met Application Proxy doen kunt:

* [Eenmalige aanmelding inschakelen](application-proxy-configure-single-sign-on-with-kcd.md)
* [Toepassingen publiceren met uw eigen domeinnaam](application-proxy-configure-custom-domain.md)
* [Meer informatie over Azure AD Application Proxy connectors](application-proxy-connectors.md)
* [Werken met bestaande on-premises Proxy-servers](application-proxy-configure-connectors-with-proxy-servers.md) 
* [Een aangepaste startpagina instellen](application-proxy-configure-custom-home-page.md)

Ga naar het [blog over toepassingsproxy](https://blogs.technet.com/b/applicationproxyblog/) voor nieuws en updates.

