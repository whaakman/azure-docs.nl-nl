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
ms.date: 08/03/2017
ms.author: celested
ms.reviewer: andret
ms.custom: aaddev
ms.openlocfilehash: 9c2140d0e482089be632d9a21560349a6381968e
ms.sourcegitcommit: eaad191ede3510f07505b11e2d1bbfbaa7585dbd
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/03/2018
ms.locfileid: "39495234"
---
# <a name="how-to-get-appsource-certified-for-azure-active-directory"></a>Over het verkrijgen van AppSource-gecertificeerd voor Azure Active Directory
[Microsoft AppSource](https://appsource.microsoft.com/) is een doel voor zakelijke gebruikers om te ontdekken, uitproberen en line-of-business SaaS-toepassingen (zelfstandige SaaS en invoegtoepassing voor bestaande Microsoft SaaS-producten) beheren.

Als u een zelfstandige SaaS-toepassing op AppSource, moet uw toepassing accepteren eenmalige aanmelding van werkaccounts van een bedrijf of organisatie die Azure Active Directory heeft. Het proces voor aanmelden moet gebruiken de [OpenID Connect](./active-directory-protocols-openid-connect-code.md) of [OAuth 2.0](./active-directory-protocols-oauth-code.md) protocollen. SAML-integratie wordt niet geaccepteerd voor AppSource-certificering.

## <a name="guides-and-code-samples"></a>Handleidingen en codevoorbeelden
Als u wilt meer informatie over het integreren van uw toepassing met Azure Active Directory met behulp van Open ID verbinden, volgt u onze richtlijnen en codevoorbeelden in de [ontwikkelaarsgids van Azure Active Directory](azure-ad-developers-guide.md#get-started "aan de slag met Azure AD voor ontwikkelaars").

## <a name="multi-tenant-applications"></a>Toepassingen met meerdere tenants

Een toepassing die aanmeldingen van gebruikers van een bedrijf of organisatie die Azure Active Directory hebben zonder een afzonderlijk exemplaar, de configuratie of de implementatie accepteert wordt ook wel een *toepassing met meerdere tenants*. AppSource raadt aan dat de multitenancy om in te schakelen voor het implementeren van toepassingen de *met één klik* gratis proefversie.

Om in te schakelen multitenancy op uw toepassing:
- Instellen `Multi-Tenanted` eigenschap `Yes` op de informatie van de registratie van uw toepassing in de [Azure Portal](https://portal.azure.com/#blade/Microsoft_AAD_IAM/ActiveDirectoryMenuBlade/RegisteredApps) (standaard, toepassingen die zijn gemaakt in de Azure Portal zijn geconfigureerd als *metééntenant*)
- Werk uw code voor het verzenden van aanvragen voor de '`common`' eindpunt (bijwerken van het eindpunt van *https://login.microsoftonline.com/{yourtenant}* naar *https://login.microsoftonline.com/common*)
- Voor sommige platformen, zoals ASP.NET, moet u ook uw code voor het accepteren van meerdere certificaatverleners bijwerken

Zie voor meer informatie over multitenancy: [aanmelden bij een Azure Active Directory (AD) gebruiker met behulp van het patroon voor multitenant-toepassingen](./active-directory-devhowto-multi-tenant-overview.md).

### <a name="single-tenant-applications"></a>Toepassingen met één tenant
Toepassingen die alleen aanmeldingen van gebruikers van een gedefinieerde Azure Active Directory-exemplaar accepteren worden aangeduid als *toepassing met één tenant*. Externe gebruikers (met inbegrip van werk of School-accounts van andere organisaties of persoonlijk account) kunnen zich aanmelden bij een toepassing met één tenant na het toevoegen van elke gebruiker als *gastaccount* naar de Azure Active Directory-exemplaar dat de toepassing is geregistreerd. U kunt gebruikers als Gast-account toevoegen aan een Azure Active Directory via de [ *Azure AD B2B-samenwerking* ](../b2b/what-is-b2b.md) - en kan worden gedaan [programmatisch](../b2b/code-samples.md). Wanneer u een gebruiker als Gast-account aan een Azure Active Directory toevoegen, wordt een uitnodiging via e-mail verzonden naar de gebruiker, met de uitnodiging te accepteren door te klikken op de koppeling in de uitnodiging per e-mail. Uitnodigingen die worden verzonden naar een andere gebruiker in een organisatie die ook lid is van de partnerorganisatie zijn niet vereist om een uitnodiging aan te melden bij te accepteren.

Toepassingen met één tenant kunnen inschakelen de *Contact Me* optreden, maar als u de éénkliks- / gratis proefversie waarmee AppSource raadt inschakelen wilt, multitenancy inschakelen op uw toepassing in plaats daarvan.


## <a name="appsource-trial-experiences"></a>Proefversie ervaringen van AppSource

### <a name="free-trial-customer-led-trial-experience"></a>Gratis proefversie (klant geleide proefversie) 
De *klant geleide proef* is de ervaring die AppSource wordt aanbevolen als het biedt een één-op-toegang tot uw toepassing. Hieronder een voorbeeld van hoe deze ervaring eruitziet:<br/><br/>

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

### <a name="contact-me-partner-led-trial-experience"></a>Contact met mij (door Partner geleide proef experience)
De *testervaring partner* kan worden gebruikt wanneer een handmatige of een langdurige bewerking moet worden opgestart voor het inrichten van de gebruiker / company: bijvoorbeeld de toepassing nodig heeft voor het inrichten van virtuele machines, database-exemplaren of bewerkingen die veel tijd in beslag nemen. In dit geval nadat de gebruiker selecteert de *'Proefversie aanvragen'* knop en vult van een formulier, AppSource verzendt u de contactgegevens van de gebruiker. Na ontvangst van deze informatie u vervolgens de omgeving inrichten en de instructies voor de gebruiker bij het openen van de testervaring verzenden:<br/><br/>

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

- Zie voor meer informatie over het bouwen van toepassingen die ondersteuning bieden voor Azure Active Directory-aanmeldingen, [Verificatiescenario's voor Azure AD](https://docs.microsoft.com/azure/active-directory/develop/active-directory-authentication-scenarios) 

- Ga voor informatie over hoe u uw SaaS-toepassing in AppSource, Zie [partnergegevens AppSource](https://appsource.microsoft.com/partners)


## <a name="get-support"></a>Krijg ondersteuning
Voor Azure Active Directory-integratie, gebruiken we [Stack Overflow](http://stackoverflow.com/questions/tagged/azure-active-directory+appsource) met de community om ondersteuning te bieden. 

Wij raden u eerst uw vragen stellen op Stack Overflow en bestaande problemen om te zien als iemand anders uw vraag voordat u heeft gevraagd. Zorg ervoor dat uw vragen of opmerkingen worden gemarkeerd met [ `[azure-active-directory]` en `[appsource]` ](http://stackoverflow.com/questions/tagged/azure-active-directory+appsource).

Gebruik de volgende sectie met opmerkingen uw feedback en help ons verfijnen en vorm van onze inhoud.

<!--Reference style links -->
[AAD-Auth-Scenarios]:authentication-scenarios.md
[AAD-Auth-Scenarios-Browser-To-WebApp]:authentication-scenarios.md#web-browser-to-web-application
[AAD-Dev-Guide]: azure-ad-developers-guide.md
[AAD-Howto-Multitenant-Overview]: ./active-directory-devhowto-multi-tenant-overview.md
[AAD-QuickStart-Web-Apps]: azure-ad-developers-guide.md#get-started


<!--Image references-->