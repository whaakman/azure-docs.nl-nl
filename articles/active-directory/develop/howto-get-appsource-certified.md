---
title: AppSource certificeren voor Azure Active Directory verkrijgen | Microsoft Docs
description: Als u meer informatie over hoe u uw toepassing AppSource-gecertificeerd voor Azure Active Directory.
services: active-directory
documentationcenter: ''
author: CelesteDG
manager: mtillman
editor: ''
ms.assetid: 21206407-49f8-4c0b-84d1-c25e17cd4183
ms.service: active-directory
ms.component: develop
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 08/21/2018
ms.author: celested
ms.reviewer: andret
ms.custom: aaddev
ms.openlocfilehash: 603da0add9efba4e68088bf0eb6ab919ec9988e2
ms.sourcegitcommit: 5c00e98c0d825f7005cb0f07d62052aff0bc0ca8
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/24/2018
ms.locfileid: "49955484"
---
# <a name="how-to-get-appsource-certified-for-azure-active-directory"></a>Over het verkrijgen van AppSource-gecertificeerd voor Azure Active Directory

[Microsoft AppSource](https://appsource.microsoft.com/) is een doel voor zakelijke gebruikers om te ontdekken, uitproberen en line-of-business SaaS-toepassingen (zelfstandige SaaS en invoegtoepassing voor bestaande Microsoft SaaS-producten) beheren.

Als u een zelfstandige SaaS-toepassing op AppSource, moet uw toepassing accepteren eenmalige aanmelding van werkaccounts van een bedrijf of organisatie die Azure Active Directory (Azure AD) is. Het proces voor aanmelden moet gebruiken de [OpenID Connect](v1-protocols-openid-connect-code.md) of [OAuth 2.0](v1-protocols-oauth-code.md) protocollen. SAML-integratie wordt niet geaccepteerd voor AppSource-certificering.

## <a name="guides-and-code-samples"></a>Handleidingen en codevoorbeelden

Als u wilt meer informatie over het integreren van uw toepassing met Azure AD met behulp van Open ID verbinden, volgt u onze richtlijnen en codevoorbeelden in de [ontwikkelaarsgids van Azure Active Directory](v1-overview.md#get-started "aan de slag met Azure AD ontwikkelaars").

## <a name="multi-tenant-applications"></a>Toepassingen met meerdere tenants

Een *toepassing met meerdere tenants* is een toepassing die aanmeldingen van gebruikers van een bedrijf of organisatie die Azure AD hebben zonder een afzonderlijk exemplaar, de configuratie of de implementatie accepteert. AppSource raadt aan dat de multitenancy om in te schakelen voor het implementeren van toepassingen de *met één klik* gratis proefversie.

Om in te schakelen multitenancy op uw toepassing, de volgende stappen uit:
1. Instellen `Multi-Tenanted` eigenschap `Yes` op de informatie van de registratie van uw toepassing in de [Azure-portal](https://portal.azure.com/#blade/Microsoft_AAD_IAM/ActiveDirectoryMenuBlade/RegisteredApps). Toepassingen die zijn gemaakt in Azure portal zijn standaard geconfigureerd als  *[één tenant](#single-tenant-applications)*.
1. Werk uw code voor het verzenden van aanvragen voor de `common` eindpunt. U doet dit door bijwerken van het eindpunt van `https://login.microsoftonline.com/{yourtenant}` naar `https://login.microsoftonline.com/common*`.
1. Voor sommige platformen, zoals ASP .NET, moet u ook uw code voor het accepteren van meerdere certificaatverleners bijwerken.

Zie voor meer informatie over multitenancy [aanmelden bij een Azure Active Directory (Azure AD) gebruiker met behulp van het patroon voor multitenant-toepassingen](howto-convert-app-to-be-multi-tenant.md).

### <a name="single-tenant-applications"></a>Toepassingen met één tenant

Een *één tenant toepassing* is een toepassing die alleen aanmeldingen van gebruikers van een opgegeven Azure accepteert AD-exemplaar. Externe gebruikers (met inbegrip van werk- of schoolaccounts van andere organisaties of persoonlijke accounts) kunnen zich aanmelden bij een toepassing met één tenant na elke gebruiker toe te voegen als een Gast-account met het Azure AD-exemplaar dat de toepassing is geregistreerd. 

U kunt gebruikers als Gast-account toevoegen aan Azure AD via de [Azure AD B2B-samenwerking](../b2b/what-is-b2b.md) en u kunt dit doen [programmatisch](../../active-directory-b2c/code-samples.md). Wanneer u B2B, kunnen gebruikers een self-service portal waarvoor een uitnodiging aan te melden bij maken. Zie voor meer informatie, [self-serviceportal voor aanmelding bij Azure AD B2B-samenwerking](https://docs.microsoft.com/azure/active-directory/b2b/self-service-portal).

Toepassingen met één tenant kunnen inschakelen de *Contact Me* optreden, maar als u de één-klik/gratis proefversie waarmee AppSource raadt inschakelen wilt, multitenancy inschakelen op uw toepassing in plaats daarvan.

## <a name="appsource-trial-experiences"></a>Proefversie ervaringen van AppSource

### <a name="free-trial-customer-led-trial-experience"></a>Gratis proefversie (klant geleide proefversie) 

De klant geleide proef is de ervaring die AppSource wordt aanbevolen als het biedt een één-op-toegang tot uw toepassing. Hieronder een voorbeeld van hoe deze ervaring eruitziet:<br/><br/>

<table >
<tr>
    <td valign="top" width="33%">1.<br/><img src="media/active-directory-devhowto-appsource-certified/customer-led-trial-step1.png" width="85%"/><ul><li>Gebruiker zoeken naar uw toepassing in de AppSource-website</li><li>Optie 'Gratis proefversie' geselecteerd</li></ul></td>
    <td valign="top" width="33%">2.<br/><img src="media/active-directory-devhowto-appsource-certified/customer-led-trial-step2.png" width="85%" /><ul><li>AppSource wordt de gebruiker omgeleid naar een URL in uw website</li><li>Uw website begint de <i>single-sign-on</i> automatisch (op de pagina laden)</li></ul></td>
    <td valign="top" width="33%">3.<br/><img src="media/active-directory-devhowto-appsource-certified/customer-led-trial-step3.png" width="85%"/><ul><li>Gebruiker wordt omgeleid naar de Microsoft-aanmeldingspagina opgeven</li><li>Gebruiker heeft de referenties voor het aanmelden</li></ul></td>
</tr>
<tr>
    <td valign="top" width="33%">4.<br/><img src="media/active-directory-devhowto-appsource-certified/customer-led-trial-step4.png" width="85%"/><ul><li>Gebruiker geeft toestemming voor uw toepassing</li></ul></td>
    <td valign="top" width="33%">5.<br/><img src="media/active-directory-devhowto-appsource-certified/customer-led-trial-step5.png" width="85%"/><ul><li>Aanmelden is voltooid en de gebruiker wordt omgeleid naar uw website</li><li>Gebruiker begint de gratis proefversie</li></ul></td>
    <td></td>
</tr>
</table>

### <a name="contact-me-partner-led-trial-experience"></a>Contact met mij (door partner geleide proef experience)

U kunt de partner-testervaring gebruiken wanneer een handmatige of een langdurige bewerking moet worden opgestart voor het inrichten van die de gebruiker/bedrijf--bijvoorbeeld uw toepassing nodig heeft om het inrichten van virtuele machines, database-exemplaren of bewerkingen die veel tijd in beslag nemen. In dit geval nadat de gebruiker selecteert de **proefversie aanvragen** knop en vult van een formulier, AppSource verzendt u de contactgegevens van de gebruiker. Wanneer u deze informatie ontvangt, klikt u vervolgens de omgeving inrichten en de instructies voor de gebruiker bij het openen van de testervaring verzenden:<br/><br/>

<table valign="top">
<tr>
    <td valign="top" width="33%">1.<br/><img src="media/active-directory-devhowto-appsource-certified/partner-led-trial-step1.png" width="85%"/><ul><li>Gebruiker vindt uw toepassing AppSource-website</li><li>Optie voor 'Contact opnemen'</li></ul></td>
    <td valign="top" width="33%">2.<br/><img src="media/active-directory-devhowto-appsource-certified/partner-led-trial-step2.png" width="85%"/><ul><li>Invullen van een formulier met contactgegevens</li></ul></td>
     <td valign="top" width="33%">3.<br/><br/>
        <table bgcolor="#f7f7f7">
        <tr>
            <td><img src="media/active-directory-devhowto-appsource-certified/UserContact.png" width="55%"/></td>
            <td>U ontvangt gebruikersinformatie</td>
        </tr>
        <tr>
            <td><img src="media/active-directory-devhowto-appsource-certified/SetupEnv.png" width="55%"/></td>
            <td>Omgeving instellen</td>
        </tr>
        <tr>
            <td><img src="media/active-directory-devhowto-appsource-certified/ContactCustomer.png" width="55%"/></td>
            <td>Neem contact op met gebruiker met de proefversie info</td>
        </tr>
        </table><br/><br/>
        <ul><li>U ontvangt de informatie en setup proefversie exemplaar van de gebruiker</li><li>U verzendt de hyperlink voor toegang tot uw toepassing aan de gebruiker</li></ul>
    </td>
</tr>
<tr>
    <td valign="top" width="33%">4.<br/><img src="media/active-directory-devhowto-appsource-certified/partner-led-trial-step3.png" width="85%"/><ul><li>Gebruiker toegang heeft tot uw toepassing en het proces voor eenmalige aanmelding te voltooien</li></ul></td>
    <td valign="top" width="33%">5.<br/><img src="media/active-directory-devhowto-appsource-certified/partner-led-trial-step4.png" width="85%"/><ul><li>Gebruiker geeft toestemming voor uw toepassing</li></ul></td>
    <td valign="top" width="33%">6.<br/><img src="media/active-directory-devhowto-appsource-certified/customer-led-trial-step5.png" width="85%"/><ul><li>Aanmelden is voltooid en de gebruiker wordt omgeleid naar uw website</li><li>Gebruiker begint de gratis proefversie</li></ul></td>
   
</tr>
</table>

### <a name="more-information"></a>Meer informatie

Zie voor meer informatie over de AppSource-testervaring [in deze video](https://aka.ms/trialexperienceforwebapps). 
 
## <a name="next-steps"></a>Volgende stappen

- Zie voor meer informatie over het bouwen van toepassingen die ondersteuning bieden voor Azure AD-aanmeldingen, [Verificatiescenario's voor Azure AD](https://docs.microsoft.com/azure/active-directory/develop/authentication-scenarios).
- Ga voor informatie over hoe u uw SaaS-toepassing in AppSource, Zie [partnergegevens AppSource](https://appsource.microsoft.com/partners)


## <a name="get-support"></a>Ondersteuning krijgen

Voor Azure AD-integratie, gebruiken we [Stack Overflow](http://stackoverflow.com/questions/tagged/azure-active-directory+appsource) met de community om ondersteuning te bieden. 

Wij raden u eerst uw vragen stellen op Stack Overflow en bestaande problemen om te zien als iemand anders uw vraag voordat u heeft gevraagd. Zorg ervoor dat uw vragen of opmerkingen worden gemarkeerd met [ `[azure-active-directory]` en `[appsource]` ](http://stackoverflow.com/questions/tagged/azure-active-directory+appsource).

Gebruik de volgende sectie met opmerkingen uw feedback en help ons verfijnen en vorm van onze inhoud.

<!--Reference style links -->
[AAD-Auth-Scenarios]:authentication-scenarios.md
[AAD-Auth-Scenarios-Browser-To-WebApp]:authentication-scenarios.md#web-browser-to-web-application
[AAD-Dev-Guide]: v1-overview.md
[AAD-Howto-Multitenant-Overview]: howto-convert-app-to-be-multi-tenant.md
[AAD-QuickStart-Web-Apps]: v1-overview.md#get-started


<!--Image references-->
