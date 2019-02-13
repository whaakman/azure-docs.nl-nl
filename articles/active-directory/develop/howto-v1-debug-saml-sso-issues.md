---
title: Fouten opsporen in SAML gebaseerde eenmalige aanmelding - Azure Active Directory | Microsoft Docs
description: Fouten opsporen op SAML gebaseerde eenmalige aanmelding voor toepassingen in Azure Active Directory.
services: active-directory
author: CelesteDG
documentationcenter: na
manager: mtillman
ms.service: active-directory
ms.subservice: develop
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 06/15/2018
ms.author: celested
ms.custom: aaddev
ms.reviewer: hirsin, dastrock, smalser
ms.collection: M365-identity-device-management
ms.openlocfilehash: 53c31e4ee11c11c816f9fb243a88240cd78522aa
ms.sourcegitcommit: 301128ea7d883d432720c64238b0d28ebe9aed59
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/13/2019
ms.locfileid: "56198895"
---
# <a name="debug-saml-based-single-sign-on-to-applications-in-azure-active-directory"></a>Fouten opsporen in SAML gebaseerde eenmalige aanmelding voor toepassingen in Azure Active Directory

Meer informatie over het vinden en op te lossen [eenmalige aanmelding](../manage-apps/what-is-single-sign-on.md) problemen voor toepassingen in Azure Active Directory (Azure AD) die ondersteuning bieden voor [Security Assertion Markup Language (SAML) 2.0](https://en.wikipedia.org/wiki/Security_Assertion_Markup_Language). 

## <a name="before-you-begin"></a>Voordat u begint
Het beste installeren de [mijn Apps beveiligde aanmelding extensie](../user-help/active-directory-saas-access-panel-user-help.md#i-am-having-trouble-installing-the-my-apps-secure-sign-in-extension). Deze browserextensie kunt u eenvoudig zijn voor het verzamelen van de SAML-aanvraag en de SAML-antwoord informatie die u nodig hebt voor het oplossen van problemen met eenmalige aanmelding. Als u de extensie niet installeren, in dit artikel wordt beschreven hoe u het oplossen van problemen met en zonder de extensie geïnstalleerd.

Als u wilt downloaden en installeren van de extensie mijn Apps beveiligen aanmelden, moet u een van de volgende koppelingen gebruiken.

- [Chrome](https://go.microsoft.com/fwlink/?linkid=866367)
- [Microsoft Edge](https://go.microsoft.com/fwlink/?linkid=845176)
- [Firefox](https://go.microsoft.com/fwlink/?linkid=866366)


## <a name="test-saml-based-single-sign-on"></a>SAML gebaseerde eenmalige aanmelding testen

Voor het testen van SAML gebaseerde eenmalige aanmelding in tussen AAD en een doeltoepassing:

1.  Aanmelden bij de [Azure-portal](https://portal.azure.com) als een globale beheerder of een andere beheerder die is gemachtigd voor het beheren van toepassingen.
2.  Klik in de linkerblade op **Azure Active Directory**, en klik vervolgens op **bedrijfstoepassingen**. 
3.  In de lijst van zakelijke toepassingen, klikt u op de toepassing die u wilt testen eenmalige aanmelding en klik vervolgens in de opties aan de linkerkant Klik op **eenmalige aanmelding**.
4.  De SAML gebaseerde eenmalige aanmelding testen mogelijk, te openen in de **domein en URL's** sectie Klik **Test SAML instelling**. Als de knop Test SAML-instelling grijs wordt weergegeven, moet u eerst af en sla de vereiste kenmerken invullen.
5.  In de **eenmalige aanmelding testen** blade gebruik van uw zakelijke referenties aanmelden bij de doeltoepassing. U kunt zich aanmelden als de huidige gebruiker of een andere gebruiker. Als u zich als een andere gebruiker aanmelden, een prompt wordt u gevraagd om te verifiëren.

    ![SAML-testpagina](./media/howto-v1-debug-saml-sso-issues/testing.png)


Als u bent aangemeld, wordt de test is verstreken. In dit geval verstrekt Azure AD een SAML-antwoordtoken aan de toepassing. De toepassing gebruikt het SAML-token is aanmelden.

Als u een fout op de aanmeldingspagina van bedrijf of de pagina van de toepassing hebt, gebruikt u een van de volgende secties de fout op te lossen.


## <a name="resolve-a-sign-in-error-on-your-company-sign-in-page"></a>Los een fout aanmelden op de aanmeldingspagina van bedrijf

Wanneer u probeert aan te melden ziet u mogelijk een fout op de aanmeldingspagina van bedrijf. 

![Aanmeldingsfout](./media/howto-v1-debug-saml-sso-issues/error.png)

Om op te sporen deze fout, moet u het foutbericht en de SAML-aanvraag. De extensie mijn Apps beveiligde aanmelding automatisch deze gegevens worden verzameld en resolutie richtlijnen weergegeven in Azure AD. 

Los de fout aanmelden met het beveiligen van de MyApps-aanmelden-extensie geïnstalleerd:

1.  Wanneer een fout optreedt, stuurt u met de extensie terug naar de Azure Ad **eenmalige aanmelding testen** blade. 
2.  Op de **eenmalige aanmelding testen** blade, klikt u op **downloaden van de SAML-aanvraag**. 
3.  Hier ziet u specifieke oplossingsstatus richtlijnen op basis van de fout en de waarden in de SAML-aanvraag. Bekijk de gids.

De fout kunt oplossen zonder dat MyApps beveiligde aanmelding-extensie wordt geïnstalleerd:

1. Kopieer het foutbericht in de rechterbenedenhoek van de pagina. Bevat het foutbericht:
    - Een CorrelationID en een tijdstempel. Deze waarden zijn belangrijk wanneer u een ondersteuningsaanvraag met Microsoft maken omdat ze de engineers om te bepalen van het probleem en een nauwkeurige oplossing voor uw probleem te bieden.
    - Een instructie de hoofdoorzaak van het probleem te identificeren.
2.  Ga terug naar Azure AD en zoek de **eenmalige aanmelding testen** blade.
3.  In het bovenstaande tekstvak **resolutie begeleiding**, plakt u het foutbericht.
3.  Klik op **resolutie begeleiding** om stappen voor het oplossen van het probleem weer te geven. De richtlijnen mogelijk gegevens uit de SAML-aanvraag of SAML-antwoord. Als u niet de extensie MyApps beveiligde aanmelding gebruikt, moet u mogelijk een hulpprogramma zoals [Fiddler](https://www.telerik.com/fiddler) om op te halen van de SAML-aanvraag en het antwoord.
4.  Controleer of het doel in de SAML-aanvraag overeenkomt met de SAML Single Sign-On Service-URL ophalen uit Azure Active Directory
5.  Controleer of dat de uitgever in de SAML-aanvraag wordt dezelfde id die u hebt geconfigureerd voor de toepassing in Azure Active Directory. Azure AD maakt gebruik van de uitgever van een toepassing zoeken in uw directory.
6.  Controleer of dat assertionconsumerserviceurl is waar de toepassing verwacht voor het ontvangen van het SAML-token uit Azure Active Directory. U kunt deze waarde configureren in Azure Active Directory, maar dit is niet verplicht als het deel van de SAML-aanvraag uitmaakt.


## <a name="resolve-a-sign-in-error-on-the-application-page"></a>Een fout aanmelden op de toepassingspagina oplossen

U kunt zich aanmelden is en er vervolgens een fout weergegeven op de pagina van de toepassing. Dit treedt op wanneer Azure AD een token tot de toepassing krijgen, maar de toepassing geen het antwoord accepteert.   

De fout kunt oplossen:

1. Als de toepassing bevindt zich in de Azure AD-galerie, controleert u of dat u de stappen voor het integreren van de toepassing met Azure AD hebt gevolgd. De integratie-instructies voor uw toepassing, Zie de [lijst met zelfstudies over integratie van SaaS-toepassing](../saas-apps/tutorial-list.md).
2. De SAML-antwoord ophalen.
    - Als de mijn Apps beveiligde aanmelding-extensie is geïnstalleerd, uit de **eenmalige aanmelding testen** blade, klikt u op **downloaden van het SAML-antwoord**.
    - Als de extensie is niet geïnstalleerd, gebruikt u een hulpprogramma zoals [Fiddler](https://www.telerik.com/fiddler) om op te halen van het SAML-antwoord. 
3. U ziet deze elementen in het SAML-antwoord-token:
    - De unieke id van de gebruiker van NameID-waarde en indeling
    - Uitgegeven claims in het token
    - Het certificaat is gebruikt voor het ondertekenen van het token. Zie voor meer informatie over het controleren van de SAML-reactie [Single Sign-On SAML-protocol](single-sign-on-saml-protocol.md).
4. Zie voor meer informatie over het SAML-antwoord [Single Sign-on SAML-protocol](single-sign-on-saml-protocol.md).
5. Nu dat u hebt de SAML-reactie gecontroleerd, Zie [fout op de pagina van een toepassing na de aanmelding](../manage-apps/application-sign-in-problem-application-error.md) voor hulp bij het oplossen van het probleem. 
6. Als u nog niet aan te melden bij is, kunt u de leverancier van de toepassing vragen wat ontbreekt in het SAML-antwoord aan te geven.


## <a name="next-steps"></a>Volgende stappen
Nu dat eenmalige aanmelding voor uw toepassing werkt, kunt u [automatiseren van gebruikersinrichting en -opheffing in SaaS-toepassingen](../manage-apps/user-provisioning.md), of [aan de slag met voorwaardelijke toegang](../conditional-access/app-based-conditional-access.md).


