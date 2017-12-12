---
title: Het ophalen van AppSource gecertificeerd voor Azure Active Directory | Microsoft Docs
description: Meer informatie over het ophalen van uw toepassing AppSource gecertificeerd voor Azure Active Directory.
services: active-directory
documentationcenter: 
author: andretms
manager: mtillman
editor: 
ms.assetid: 21206407-49f8-4c0b-84d1-c25e17cd4183
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 08/03/2017
ms.author: andret
ms.custom: aaddev
ms.openlocfilehash: 5601ad80e271364fec519cf34bcdc2f650f3bb92
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/11/2017
---
# <a name="how-to-get-appsource-certified-for-azure-active-directory"></a>Het ophalen van AppSource gecertificeerd voor Azure Active Directory
[Microsoft AppSource](https://appsource.microsoft.com/) een doel voor zakelijke gebruikers om te detecteren, probeer en beheren van LOB-SaaS-toepassingen (zelfstandige SaaS en add-on voor bestaande Microsoft SaaS-producten).

Als een zelfstandige SaaS-toepassing op AppSource wilt weergeven, moet uw toepassing accepteren eenmalige aanmelding van werkaccounts van een bedrijf of organisatie die Azure Active Directory heeft. Het proces voor aanmelden moet gebruiken de [OpenID Connect](./active-directory-protocols-openid-connect-code.md) of [OAuth 2.0](./active-directory-protocols-oauth-code.md) protocollen. SAML-integratie wordt niet geaccepteerd voor AppSource-certificering.

## <a name="guides-and-code-samples"></a>Handleidingen en codevoorbeelden
Als u wilt weten over het integreren van uw toepassing met Azure Active Directory met Open ID verbinding, onze richtlijnen te volgen en codevoorbeelden in de [ontwikkelaarshandleiding Azure Active Directory](./active-directory-developers-guide.md#get-started "aan de slag met Azure AD voor ontwikkelaars").

## <a name="multi-tenant-applications"></a>Multitenant-toepassingen

Een toepassing die aan de aanmeldingen van gebruikers van een bedrijf of organisatie die Azure Active Directory zijn zonder een afzonderlijk exemplaar, de configuratie of de implementatie accepteert wordt ook wel een *multitenant toepassing*. AppSource raadt aan dat de multi-tenancymodus om in te schakelen voor het implementeren van toepassingen de *éénkliks-* gratis evaluatieversie.

Om in te schakelen multi-tenancymodus op uw toepassing:
- Ingesteld `Multi-Tenanted` eigenschap `Yes` van gegevens van uw toepassing registreren in de [Azure Portal](https://portal.azure.com/#blade/Microsoft_AAD_IAM/ActiveDirectoryMenuBlade/RegisteredApps) (toepassingen die zijn gemaakt in de Azure Portal zijn standaard geconfigureerd als *single-tenant*)
- Werk uw code voor het verzenden van aanvragen voor de '`common`' eindpunt (bijwerken van het eindpunt van *https://login.microsoftonline.com/ {yourtenant}* naar *https://login.microsoftonline.com/common*)
- Voor sommige platformen, zoals ASP.NET, moet u ook werk uw code voor het accepteren van meerdere uitgevers van certificaten

Zie voor meer informatie over multi-tenancymodus: [aanmelden met een Azure Active Directory (AD) gebruiker met behulp van het patroon toepassing met meerdere tenants](./active-directory-devhowto-multi-tenant-overview.md).

### <a name="single-tenant-applications"></a>Toepassingen voor één tenant
Toepassingen die alleen aanmeldingen van gebruikers van een gedefinieerde Azure Active Directory-exemplaar accepteren worden aangeduid als *toepassing voor één tenant*. Externe gebruikers (inclusief werk of School accounts van andere organisaties of persoonlijk account) kunnen aanmelden bij een toepassing voor één tenant na het toevoegen van elke gebruiker als *gastaccount* met Azure Active Directory-instantie die de toepassing is geregistreerd. U kunt gebruikers als gastaccounts toevoegen aan een Azure Active Directory via de [ *Azure AD B2B-samenwerking* ](../active-directory-b2b-what-is-azure-ad-b2b.md) - en kan worden gedaan [via programmacode](../active-directory-b2b-code-samples.md). Wanneer u een gebruiker als Gast-account aan een Azure Active Directory toevoegt, wordt een uitnodiging voor een e-mailbericht verzonden naar de gebruiker die de uitnodiging te accepteren door te klikken op de koppeling in de uitnodiging via e-mail. Uitnodigingen die worden verzonden naar een extra gebruiker in een uitnodiging organisatie die deel uitmaakt van de partnerorganisatie zijn niet vereist voor het accepteren van een uitnodiging aan te melden.

Toepassingen voor één tenant kunnen inschakelen de *Contact met mij opnemen* ervaring, maar als u de éénkliks- / gratis proefabonnement ervaring die AppSource aanbeveelt inschakelen wilt, schakelt u multi-tenancymodus op uw toepassing in plaats daarvan.


## <a name="appsource-trial-experiences"></a>De evaluatieversie ervaringen AppSource

### <a name="free-trial-customer-led-trial-experience"></a>Gratis proefversie (klant geleid proefversie experience) 
De *klant geleid proefversie* de ervaring die AppSource aanbeveelt zoals een één-op-toegang tot uw toepassing biedt is. Hieronder een illustratie van hoe ziet deze ervaring:<br/><br/>

<table >
<tr>
    <td valign="top" width="33%">1.<br/><img src="media/active-directory-devhowto-appsource-certified/customer-led-trial-step1.png" width="85%"/><ul><li>Gebruiker uw toepassing worden gevonden in AppSource website</li><li>Optie 'Gratis proefversie' geselecteerd</li></ul></td>
    <td valign="top" width="33%">2.<br/><img src="media/active-directory-devhowto-appsource-certified/customer-led-trial-step2.png" width="85%" /><ul><li>AppSource wordt de gebruiker omgeleid naar een URL in uw website</li><li>De website wordt opgestart de <i>eenmalige aanmelding</i> processen automatisch (laden van de pagina)</li></ul></td>
    <td valign="top" width="33%">3.<br/><img src="media/active-directory-devhowto-appsource-certified/customer-led-trial-step3.png" width="85%"/><ul><li>Gebruiker wordt omgeleid naar de Microsoft-aanmeldingspagina</li><li>Gebruiker heeft referenties aan te melden</li></ul></td>
</tr>
<tr>
    <td valign="top" width="33%">4.<br/><img src="media/active-directory-devhowto-appsource-certified/customer-led-trial-step4.png" width="85%"/><ul><li>Gebruiker geeft toestemming voor uw toepassing</li></ul></td>
    <td valign="top" width="33%">5.<br/><img src="media/active-directory-devhowto-appsource-certified/customer-led-trial-step5.png" width="85%"/><ul><li>Aanmelden is voltooid en de gebruiker wordt omgeleid naar uw website</li><li>Gebruiker begint de gratis proefversie</li></ul></td>
    <td></td>
</tr>
</table>

### <a name="contact-me-partner-led-trial-experience"></a>Contact met Me (proefversie experience partners)
De *evaluatieversie partner* kan worden gebruikt wanneer een handmatige of een langdurige bewerking moet worden uitgevoerd voor het inrichten van de gebruiker / company: bijvoorbeeld uw toepassing nodig voor het inrichten van virtuele machines, database-exemplaren of bewerkingen die veel tijd in beslag nemen. In dit geval nadat de gebruiker selecteert de *'Proefversie aanvragen'* knop en de opvulling van een formulier, AppSource verzendt u de contactgegevens van de gebruiker. Bij ontvangst van deze informatie vervolgens inrichten van de omgeving en de instructies verzenden naar de gebruiker over toegang krijgen tot de evaluatieversie:<br/><br/>

<table valign="top">
<tr>
    <td valign="top" width="33%">1.<br/><img src="media/active-directory-devhowto-appsource-certified/partner-led-trial-step1.png" width="85%"/><ul><li>Uw toepassing in de website AppSource gevonden met een gebruiker</li><li>De optie 'Contact Me' geselecteerd</li></ul></td>
    <td valign="top" width="33%">2.<br/><img src="media/active-directory-devhowto-appsource-certified/partner-led-trial-step2.png" width="85%"/><ul><li>Een formulier met contactgegevens invult</li></ul></td>
     <td valign="top" width="33%">3.<br/><br/>
        <table bgcolor="#f7f7f7">
        <tr>
            <td><img src="media/active-directory-devhowto-appsource-certified/UserContact.png" width="55%"/></td>
            <td>U ontvangt gebruikersgegevens</td>
        </tr>
        <tr>
            <td><img src="media/active-directory-devhowto-appsource-certified/SetupEnv.png" width="55%"/></td>
            <td>Setup-omgeving</td>
        </tr>
        <tr>
            <td><img src="media/active-directory-devhowto-appsource-certified/ContactCustomer.png" width="55%"/></td>
            <td>Neem contact op met gebruiker met de proefversie info</td>
        </tr>
        </table><br/><br/>
        <ul><li>U ontvangt van de gebruiker informatie en proefversie exemplaar van setup</li><li>Verzenden van de hyperlink voor toegang tot uw toepassing aan de gebruiker</li></ul>
    </td>
</tr>
<tr>
    <td valign="top" width="33%">4.<br/><img src="media/active-directory-devhowto-appsource-certified/partner-led-trial-step3.png" width="85%"/><ul><li>Gebruiker heeft toegang tot uw toepassing en het proces voor eenmalige aanmelding te voltooien</li></ul></td>
    <td valign="top" width="33%">5.<br/><img src="media/active-directory-devhowto-appsource-certified/partner-led-trial-step4.png" width="85%"/><ul><li>Gebruiker geeft toestemming voor uw toepassing</li></ul></td>
    <td valign="top" width="33%">6.<br/><img src="media/active-directory-devhowto-appsource-certified/customer-led-trial-step5.png" width="85%"/><ul><li>Aanmelden is voltooid en de gebruiker wordt omgeleid naar uw website</li><li>Gebruiker begint de gratis proefversie</li></ul></td>
   
</tr>
</table>

### <a name="more-information"></a>Meer informatie
Zie voor meer informatie over de evaluatieversie AppSource [in deze video](https://aka.ms/trialexperienceforwebapps). 
 
## <a name="next-steps"></a>Volgende stappen

- Zie voor meer informatie over het bouwen van toepassingen die ondersteuning bieden voor Azure Active Directory aanmeldingen [verificatie scenario's voor Azure AD](https://docs.microsoft.com/azure/active-directory/develop/active-directory-authentication-scenarios) 

- Ga voor informatie over hoe u uw SaaS-toepassing in AppSource, Zie [AppSource partnergegevens](https://appsource.microsoft.com/partners)


## <a name="get-support"></a>Krijg ondersteuning
We gebruiken voor Azure Active Directory-integratie [Stack Overflow](http://stackoverflow.com/questions/tagged/azure-active-directory+appsource) met de community om ondersteuning te bieden. 

We raden u eerst uw vragen stellen over stackoverloop en blader bestaande problemen om te zien of iemand anders uw vraag voordat heeft gesteld. Zorg ervoor dat uw vragen of opmerkingen zijn gelabeld met [ `[azure-active-directory]` en `[appsource]` ](http://stackoverflow.com/questions/tagged/azure-active-directory+appsource).

Gebruik de volgende sectie met opmerkingen uw feedback en help ons verfijnen en onze content vorm.

<!--Reference style links -->
[AAD-Auth-Scenarios]: ./active-directory-authentication-scenarios.md
[AAD-Auth-Scenarios-Browser-To-WebApp]: ./active-directory-authentication-scenarios.md#web-browser-to-web-application
[AAD-Dev-Guide]: ./active-directory-developers-guide.md
[AAD-Howto-Multitenant-Overview]: ./active-directory-devhowto-multi-tenant-overview.md
[AAD-QuickStart-Web-Apps]: ./active-directory-developers-guide.md#get-started


<!--Image references-->