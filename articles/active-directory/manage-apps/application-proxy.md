---
title: Externe toegang tot on-premises toepassingen - Azure Active Directory-toepassingsproxy | Microsoft Docx
description: Azure Active Directory Application Proxy biedt veilige externe toegang tot on-premises webtoepassingen. Na een single sign-on bij Azure AD, kunnen gebruikers toegang tot van zowel cloud en on-premises toepassingen via een externe URL of een interne applicatie-portal. Bijvoorbeeld, kan Application Proxy RAS- en eenmalige aanmelding bieden aan een extern bureaublad, Sharepoint, Teams, Tableau, Qlik en line-of-business (LOB)-toepassingen.
services: active-directory
author: barbkess
manager: mtillman
ms.service: active-directory
ms.component: app-mgmt
ms.workload: identity
ms.topic: conceptual
ms.date: 01/07/2019
ms.author: barbkess
ms.reviewer: japere
ms.openlocfilehash: 21e11575fc90e761f748af4bf9c89dd4614c986e
ms.sourcegitcommit: fbf0124ae39fa526fc7e7768952efe32093e3591
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/08/2019
ms.locfileid: "54078895"
---
# <a name="remote-access-to-on-premises-applications-through-azure-active-directorys-application-proxy"></a>Externe toegang tot on-premises toepassingen via de toepassingsproxy van Azure Active Directory 

Azure Active Directory Application Proxy biedt veilige externe toegang tot on-premises webtoepassingen. Na een single sign-on bij Azure AD, kunnen gebruikers toegang tot van zowel cloud en on-premises toepassingen via een externe URL of een interne applicatie-portal. Bijvoorbeeld, kan Application Proxy RAS- en eenmalige aanmelding bieden aan een extern bureaublad, Sharepoint, Teams, Tableau, Qlik en line-of-business (LOB)-toepassingen.

Azure AD Application Proxy is:

- **Eenvoudig te gebruiken**. Gebruikers kunnen toegang krijgen tot uw on-premises toepassingen dezelfde manier als ze toegang krijgen tot O365 en andere SaaS-apps die is geïntegreerd met Azure AD. U hoeft niet te wijzigen of bijwerken van uw toepassingen werken met Application Proxy. 

- **Beveiligde**. On-premises toepassingen kunnen gebruiken voor de besturingselementen voor autorisatie en de beveiligingsanalyses van Azure. On-premises toepassingen kunnen bijvoorbeeld gebruiken voor voorwaardelijke toegang en verificatie in twee stappen. Application Proxy, hoeft u binnenkomende verbindingen via de firewall te openen.
 
- **Rendabele**. On-premises oplossingen normaal gesproken moeten u instellen en onderhouden van gedemilitariseerde zones (DMV's), randservers of andere ingewikkelde infrastructuren. Application Proxy wordt uitgevoerd in de cloud, waardoor het eenvoudig te gebruiken. Voor het gebruik van Application Proxy, moet u niet wijzigen van de netwerkinfrastructuur of meer apparaten in uw on-premises omgeving installeren.

## <a name="what-is-application-proxy"></a>Wat is Application Proxy?
Application Proxy is een functie van Azure AD waarmee gebruikers toegang krijgen tot on-premises webtoepassingen vanaf een externe client. Application Proxy bevat zowel de Application Proxy-service die wordt uitgevoerd in de cloud en de Application Proxy-connector die wordt uitgevoerd op een on-premises server. Azure AD de Application Proxy-service en de Application Proxy connector werken samen om veilig het gebruiker aanmeldings-token van Azure AD naar de webtoepassing.

Proxy voor servicetoepassing werkt met:

* Webtoepassingen die gebruikmaken van [geïntegreerde Windows-verificatie](application-proxy-configure-single-sign-on-with-kcd.md) voor verificatie  
* Webtoepassingen die gebruikmaken van formulier-gebaseerde of [headers gebaseerde](application-proxy-configure-single-sign-on-with-ping-access.md) toegang  
* Web-API's die u wilt weergeven voor de uitgebreide toepassingen op verschillende apparaten  
* Toepassingen die worden gehost achter een [extern bureaublad-Gateway](application-proxy-integrate-with-remote-desktop-services.md)  
* Rich client-apps die kunnen worden geïntegreerd met de Active Directory Authentication Library (ADAL)

Application Proxy biedt ondersteuning voor eenmalige aanmelding. Zie voor meer informatie over de ondersteunde methoden [kiezen van een methode voor eenmalige aanmelding](what-is-single-sign-on.md#choosing-a-single-sign-on-method).

## <a name="how-application-proxy-works"></a>De werking van de toepassingsproxy

Het volgende diagram laat zien hoe Azure AD en Application Proxy werken samen voor single sign-on bij on-premises toepassingen.

![AzureAD Application Proxy-diagram](./media/application-proxy/azureappproxxy.png)

1. Nadat de gebruiker toegang gekregen de toepassing via een eindpunt tot heeft, wordt de gebruiker omgeleid naar de aanmeldingspagina van Azure AD. 
2. Azure AD wordt na een geslaagde aanmelding, een token naar de client-apparaat van de gebruiker verzonden.
3. De client stuurt het token naar de Application Proxy-service, die de user principal name (UPN) en de naam van de beveiligingsprincipal (SPN) opgehaald uit het token. Application Proxy stuurt de aanvraag naar de Application Proxy-connector.
4. Als u eenmalige aanmelding hebt geconfigureerd, voert de connector aanvullende verificatie namens de gebruiker vereist.
5. De connector stuurt de aanvraag naar de on-premises toepassing.  
6. Het antwoord is verzonden via de connector en de service voor toepassingsproxy voor de gebruiker.

| Onderdeel | Description |
| --------- | ----------- |
| Eindpunt  | Het eindpunt is een URL of een [eindgebruikersportal](end-user-experiences.md). Gebruikers kunnen toepassingen terwijl buiten uw netwerk bereiken door het openen van een externe URL. Gebruikers in uw netwerk kunnen de toepassing openen via een URL of een eindgebruikersportal. Wanneer gebruikers naar een van deze eindpunten gaat, worden ze verifiëren in Azure AD en vervolgens worden gerouteerd via de connector op de on-premises toepassing.|
| Azure AD | Azure AD voert de verificatie met behulp van de tenant-directory die zijn opgeslagen in de cloud. |
| Application Proxy-service | Deze Application Proxy-service wordt uitgevoerd in de cloud als onderdeel van Azure AD. Deze geeft de token van de aanmelding van de gebruiker aan de Connector voor toepassingsproxy. Application Proxy stuurt een toegankelijk headers van de aanvraag en de headers aan de hand van het protocol ingesteld op de client-IP-adres. Als de inkomende aanvraag naar de proxy al die header heeft, wordt het client-IP-adres toegevoegd aan het einde van de door komma's gescheiden lijst die de waarde van de header.|
| Connector voor toepassingsproxy | De connector is een lichtgewicht-agent die wordt uitgevoerd op een Windows-Server in uw netwerk. De connector beheert de communicatie tussen de service voor toepassingsproxy in de cloud en de on-premises toepassing. Uitgaande verbindingen maakt alleen gebruik van de connector, zodat u hoeft inkomende poorten openen of iets in de DMZ te plaatsen. De connectors zijn staatloos en ophalen van informatie vanuit de cloud naar behoefte. Voor meer informatie over connectors, zoals hoe ze verdelen en geverifieerd, Zie [over Azure AD Application Proxy connectors](application-proxy-connectors.md).|
| Active Directory (AD) | Active Directory wordt on-premises, voor verificatie uitvoeren voor domeinaccounts uitgevoerd. Wanneer eenmalige aanmelding is geconfigureerd, wordt de connector communiceert met AD om uit te voeren van aanvullende verificatie vereist.
| On-premises toepassing | Ten slotte heeft de gebruiker toegang tot een on-premises toepassing. 

## <a name="next-steps"></a>Volgende stappen
Als u wilt gaan met Application Proxy, Zie [zelfstudie: Toevoegen van een on-premises toepassing voor externe toegang via toepassingsproxy](application-proxy-add-on-premises-application.md). 

Zie voor het laatste nieuws en updates, de [Application Proxy-blog](https://blogs.technet.com/b/applicationproxyblog/)


