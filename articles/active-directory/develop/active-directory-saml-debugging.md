---
title: Fouten opsporen in op basis van SAML eenmalige aanmelding - Azure Active Directory | Microsoft Docs
description: Foutopsporing op basis van SAML eenmalige aanmelding tot toepassingen in Azure Active Directory.
services: active-directory
author: CelesteDG
documentationcenter: na
manager: mtillman
ms.service: active-directory
ms.component: develop
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 06/15/2018
ms.author: celested
ms.custom: aaddev
ms.reviewer: hirsin, dastrock, smalser
ms.openlocfilehash: 8bb0df567fbac6e8f8a2e2f64f868b4f219e05ac
ms.sourcegitcommit: 6eb14a2c7ffb1afa4d502f5162f7283d4aceb9e2
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/25/2018
ms.locfileid: "36751391"
---
# <a name="debug-saml-based-single-sign-on-to-applications-in-azure-active-directory"></a>Foutopsporing op basis van SAML eenmalige aanmelding tot toepassingen in Azure Active Directory

Meer informatie over het oplossen van [eenmalige aanmelding](../manage-apps/what-is-single-sign-on.md) problemen voor toepassingen in Azure Active Directory (Azure AD) die ondersteuning bieden voor [Security Assertion Markup Language (SAML) 2.0](https://en.wikipedia.org/wiki/Security_Assertion_Markup_Language). 

## <a name="before-you-begin"></a>Voordat u begint
We adviseren te installeren de [mijn Apps beveiligen aanmelden extensie](../active-directory-saas-access-panel-user-help.md#i-am-having-trouble-installing-the-my-apps-secure-sign-in-extension). Deze extensie browser kunt gemakkelijk de SAML-aanvraag en informatie van de SAML-reactie die u nodig hebt voor het oplossen van problemen met eenmalige aanmelding te verzamelen. Als u de extensie niet installeren, dit artikel ziet u het oplossen van problemen met en zonder de extensie die is geïnstalleerd.

Als u wilt downloaden en installeren van de extensie mijn Apps beveiligen aanmeldingspagina, gebruikt u een van de volgende koppelingen.

- [Chrome](https://go.microsoft.com/fwlink/?linkid=866367)
- [Rand](https://go.microsoft.com/fwlink/?linkid=845176)
- [Firefox](https://go.microsoft.com/fwlink/?linkid=866366)


## <a name="test-saml-based-single-sign-on"></a>Testen op basis van SAML eenmalige aanmelding

Testen op basis van SAML eenmalige aanmelding tussen AAD en een doeltoepassing:

1.  Aanmelden bij de [Azure-portal](https://portal.azure.com) als globale beheerder of een andere beheerder dat is gemachtigd om toepassingen te beheren.
2.  Klik op de blade links **Azure Active Directory**, en klik vervolgens op **bedrijfstoepassingen**. 
3.  Klik op de toepassing die u wilt testen eenmalige aanmelding en klik vervolgens in de opties in het linkerdeelvenster klikt u op uit de lijst van zakelijke toepassingen, **eenmalige aanmelding**.
4.  De SAML-gebaseerde één aanmelding testen ervaring openen in de **domein en de URL's** sectie Klik **Test SAML instelling**. Als de knop testen SAML-instelling wordt grijs, moet u eerst vult u af en sla de vereiste kenmerken.
5.  In de **Test eenmalige aanmelding** blade gebruik van uw zakelijke referenties aanmelden bij de doeltoepassing. U kunt zich aanmelden als de huidige gebruiker of een andere gebruiker. Als u zich als een andere gebruiker aanmelden, wordt een prompt u gevraagd te verifiëren.

    ![SAML testpagina](media/active-directory-saml-debugging/testing.png)


Als u bent aangemeld, wordt de test is geslaagd. Azure AD uitgegeven in dit geval een SAML-token antwoord voor de toepassing. De toepassing gebruikt het SAML-token is om u te registreren.

Als u een fout op de aanmeldingspagina van bedrijf of de pagina van de toepassing hebt, gebruikt u een van de volgende secties de fout op te lossen.


## <a name="resolve-a-sign-in-error-on-your-company-sign-in-page"></a>Omzetten van een fout aanmelden op de aanmeldingspagina van bedrijf

Wanneer u probeert aan te melden ziet u mogelijk een fout op de aanmeldingspagina van bedrijf. 

![Fout bij aanmelden](media/active-directory-saml-debugging/error.png)

Voor foutopsporing van deze fout, moet u het foutbericht en de SAML-aanvraag. De extensie mijn Apps beveiligen aanmelden automatisch deze informatie en geeft weer resolutie richtlijnen op Azure AD. 

Los de fout aanmelden met de MyApps Secure aanmelden uitbreiding geïnstalleerd:

1.  Wanneer er een fout optreedt, de uitbreiding wordt doorgestuurd naar de Azure Ad **Test eenmalige aanmelding** blade. 
2.  Op de **Test eenmalige aanmelding** blade, klikt u op **downloaden van het SAML-aanvraag**. 
3.  Hier ziet u specifieke oplossingsstatus richtlijnen op basis van de fout en de waarden in de SAML-aanvraag. Bekijk de gids.

De fout oplossen zonder MyApps Secure aanmelden uitbreiding te installeren:

1. Kopieer het foutbericht in de rechterbenedenhoek van de pagina. Het foutbericht bevat:
    - Een CorrelationID en een tijdstempel. Deze waarden zijn belangrijk wanneer u een ondersteuningsaanvraag met Microsoft maken omdat ze de engineers helpen voor uw probleem vast te stellen en geef een nauwkeurige oplossing voor uw probleem.
    - Een instructie de hoofdoorzaak van het probleem te identificeren.
2.  Ga terug naar Azure AD en zoek de **Test eenmalige aanmelding** blade.
3.  In het bovenstaande tekstvak **ophalen resolutie richtlijnen**, plakt u het foutbericht.
3.  Klik op **ophalen resolutie richtlijnen** om de stappen voor het oplossen van het probleem weer te geven. De richtlijnen mogelijk gegevens van het SAML-aanvraag of SAML-reactie. Als u niet de extensie MyApps Secure aanmelden, moet u mogelijk een hulpprogramma zoals [Fiddler](http://www.telerik.com/fiddler) voor het ophalen van de SAML-aanvraag en het antwoord.
4.  Controleer of dat de bestemming in de SAML-aanvraag komt overeen met de SAML Single Sign-On Service-URL ophalen uit Azure Active Directory
5.  Controleer of dat de uitgever in de SAML-aanvraag wordt dezelfde id die u hebt geconfigureerd voor de toepassing in Azure Active Directory. Azure AD maakt gebruik van de verlener vinden van een toepassing in uw directory.
6.  Controleer of AssertionConsumerServiceURL waar de toepassing verwacht te ontvangen van het SAML-token van Azure Active Directory. U kunt deze waarde configureren in Azure Active Directory, maar dit is niet verplicht als deze deel uitmaakt van de SAML-aanvraag.


## <a name="resolve-a-sign-in-error-on-the-application-page"></a>Een fout aanmelden op de toepassingspagina oplossen

U kunt aanmelden is gelukt en vervolgens ziet een fout op de pagina van de toepassing. Dit gebeurt wanneer u Azure AD een token krijgen tot de toepassing, maar het antwoord wordt niet geaccepteerd door de toepassing.   

De fout oplossen:

1. Als de toepassing bevindt zich in de galerie van Azure AD, controleert u of dat u alle stappen voor het integreren van de toepassing met Azure AD hebt gevolgd. Zie informatie over de integratie-instructies voor uw toepassing de [lijst met SaaS-toepassing integratie zelfstudies](../saas-apps/tutorial-list.md).
2. De SAML-reactie ophalen.
    - Als de extensie mijn Apps beveiligen aanmelden is geïnstalleerd, uit de **Test eenmalige aanmelding** blade, klikt u op **downloaden van de SAML-reactie**.
    - Als de extensie niet is geïnstalleerd, gebruikt u een hulpprogramma zoals [Fiddler](http://www.telerik.com/fiddler) voor het ophalen van de SAML-reactie. 
3. U ziet deze elementen in het SAML-token voor antwoord:
    - De unieke id van de gebruiker van NameID waarde en de indeling
    - Uitgegeven claims in het token
    - Het certificaat dat wordt gebruikt voor het ondertekenen van het token. Zie voor meer informatie over het controleren van de SAML-reactie [Single Sign-On SAML-protocol](active-directory-single-sign-on-protocol-reference.md).
4. Zie voor meer informatie over de SAML-reactie [Single Sign-on SAML-protocol](active-directory-single-sign-on-protocol-reference.md).
5. Nu u de SAML-reactie hebt doorgenomen, gaat u naar [fout op de pagina voor een toepassing na het aanmelden](../application-sign-in-problem-application-error.md) voor hulp bij het oplossen van het probleem. 
6. Als u nog steeds niet lukt is aanmelden, kunt u de leverancier van de toepassing vragen wat ontbreekt in de SAML-reactie aan te geven.


## <a name="next-steps"></a>Volgende stappen
Nu dat eenmalige aanmelding voor uw toepassing werkt, kunt u [gebruikers inrichten en opheffen van inrichting tot SaaS-toepassingen automatiseren](../active-directory-saas-app-provisioning.md), of [aan de slag met voorwaardelijke toegang](../active-directory-conditional-access-azure-portal-get-started.md).


