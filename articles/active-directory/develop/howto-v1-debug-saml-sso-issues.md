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
ms.date: 02/18/2019
ms.author: celested
ms.custom: aaddev
ms.reviewer: luleon, hirsin, smalser
ms.collection: M365-identity-device-management
ms.openlocfilehash: 9fcc6cb40d83c06a1c9f0a97c72565464e74e655
ms.sourcegitcommit: 02d17ef9aff49423bef5b322a9315f7eab86d8ff
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/21/2019
ms.locfileid: "58336068"
---
# <a name="debug-saml-based-single-sign-on-to-applications-in-azure-active-directory"></a>Fouten opsporen in SAML gebaseerde eenmalige aanmelding voor toepassingen in Azure Active Directory

Meer informatie over het vinden en op te lossen [eenmalige aanmelding](../manage-apps/what-is-single-sign-on.md) problemen voor toepassingen in Azure Active Directory (Azure AD) die ondersteuning bieden voor [Security Assertion Markup Language (SAML) 2.0](https://en.wikipedia.org/wiki/Security_Assertion_Markup_Language). 

## <a name="before-you-begin"></a>Voordat u begint

Het beste installeren de [mijn Apps beveiligde aanmelding extensie](../user-help/my-apps-portal-end-user-troubleshoot.md#im-having-trouble-installing-the-my-apps-secure-sign-in-extension). Deze browserextensie kunt u eenvoudig zijn voor het verzamelen van de SAML-aanvraag en de SAML-antwoord informatie die u nodig hebt voor het oplossen van problemen met eenmalige aanmelding. Als u de extensie niet installeren, in dit artikel wordt beschreven hoe u het oplossen van problemen met en zonder de extensie geïnstalleerd.

Als u wilt downloaden en installeren van de extensie mijn Apps beveiligen aanmelden, moet u een van de volgende koppelingen gebruiken.

- [Chrome](https://go.microsoft.com/fwlink/?linkid=866367)
- [Microsoft Edge](https://go.microsoft.com/fwlink/?linkid=845176)
- [Firefox](https://go.microsoft.com/fwlink/?linkid=866366)


## <a name="test-saml-based-single-sign-on"></a>SAML gebaseerde eenmalige aanmelding testen

Voor het testen van SAML gebaseerde eenmalige aanmelding tussen Azure AD en een doeltoepassing:

1. Aanmelden bij de [Azure-portal](https://portal.azure.com) als een globale beheerder of een andere beheerder die is gemachtigd voor het beheren van toepassingen.
1. Selecteer in de linkerblade **Azure Active Directory**, en selecteer vervolgens **bedrijfstoepassingen**. 
1. Selecteer in de lijst van zakelijke toepassingen, de toepassing die u wilt testen eenmalige aanmelding en klik vervolgens in de opties op de linker selecteren **eenmalige aanmelding**.
1. Als u wilt de SAML gebaseerde eenmalige aanmelding testen mogelijk openen, gaat u naar **eenmalige aanmelding testen** (stap 5). Als de **Test** knop wordt grijs weergegeven, moet u eerst af en sla de vereiste kenmerken Vul in de **SAML-basisconfiguratie** sectie.
1. In de **eenmalige aanmelding testen** blade gebruik van uw zakelijke referenties aanmelden bij de doeltoepassing. U kunt zich aanmelden als de huidige gebruiker of een andere gebruiker. Als u zich als een andere gebruiker aanmelden, een prompt wordt u gevraagd om te verifiëren.

    ![SAML-testpagina](./media/howto-v1-debug-saml-sso-issues/test-single-sign-on.png)


Als u bent aangemeld, wordt de test is verstreken. In dit geval verstrekt Azure AD een SAML-antwoordtoken aan de toepassing. De toepassing gebruikt het SAML-token is aanmelden.

Als u een fout op de aanmeldingspagina van bedrijf of de pagina van de toepassing hebt, gebruikt u een van de volgende secties de fout op te lossen.


## <a name="resolve-a-sign-in-error-on-your-company-sign-in-page"></a>Los een fout aanmelden op de aanmeldingspagina van bedrijf

Wanneer u probeert aan te melden, ziet u mogelijk een foutbericht op uw bedrijf aanmeldingspagina opgeven, die vergelijkbaar is met het volgende voorbeeld.

![Aanmeldingsfout](./media/howto-v1-debug-saml-sso-issues/error.png)

Om op te sporen deze fout, moet u het foutbericht en de SAML-aanvraag. De extensie mijn Apps beveiligde aanmelding automatisch deze gegevens worden verzameld en resolutie richtlijnen weergegeven in Azure AD. 

### <a name="to-resolve-the-sign-in-error-with-the-my-apps-secure-sign-in-extension-installed"></a>Los de fout aanmelden met de mijn Apps beveiligde aanmelding bij de extensie is geïnstalleerd

1. Wanneer een fout optreedt, stuurt u met de extensie terug naar de Azure AD **eenmalige aanmelding testen** blade. 
1. Op de **eenmalige aanmelding testen** Selecteer **downloaden van de SAML-aanvraag**. 
1. Hier ziet u specifieke oplossingsstatus richtlijnen op basis van de fout en de waarden in de SAML-aanvraag.
1. U ziet een **Fix it** knop automatisch bijwerken van de configuratie in Azure AD om het probleem te verhelpen. Als u deze knop niet ziet, klikt u vervolgens is het probleem aanmelden niet vanwege een onjuiste configuratie in Azure AD.

Als geen oplossing die is opgegeven voor de fout aanmelden, het is raadzaam dat u het tekstvak feedback gebruiken om u te laat het ons weten.

### <a name="to-resolve-the-error-without-installing-the-my-apps-secure-sign-in-extension"></a>De fout kunt oplossen zonder dat de mijn Apps beveiligde aanmelding-extensie wordt geïnstalleerd

1. Kopieer het foutbericht in de rechterbenedenhoek van de pagina. Bevat het foutbericht:
    - Een CorrelationID en een tijdstempel. Deze waarden zijn belangrijk wanneer u een ondersteuningsaanvraag met Microsoft maken omdat ze de engineers om te bepalen van het probleem en een nauwkeurige oplossing voor uw probleem te bieden.
    - Een instructie de hoofdoorzaak van het probleem te identificeren.
1. Ga terug naar Azure AD en zoek de **eenmalige aanmelding testen** blade.
1. In het bovenstaande tekstvak **resolutie begeleiding**, plakt u het foutbericht.
1. Klik op **resolutie begeleiding** om stappen voor het oplossen van het probleem weer te geven. De richtlijnen mogelijk gegevens uit de SAML-aanvraag of SAML-antwoord. Als u niet de extensie mijn Apps beveiligen aanmelden, moet u mogelijk een hulpprogramma zoals [Fiddler](https://www.telerik.com/fiddler) om op te halen van de SAML-aanvraag en het antwoord.
1. Controleer of dat het doel in de SAML-aanvraag overeenkomt met de SAML Single Sign-On Service-URL ontleend aan het Azure AD.
1. Controleer of dat de uitgever in de SAML-aanvraag wordt dezelfde id die u hebt geconfigureerd voor de toepassing in Azure AD. Azure AD maakt gebruik van de uitgever van een toepassing zoeken in uw directory.
1. Controleer of dat assertionconsumerserviceurl is waar de toepassing verwacht voor het ontvangen van het SAML-token van Azure AD. U kunt deze waarde configureren in Azure AD, maar dit is niet verplicht als het deel van de SAML-aanvraag uitmaakt.


## <a name="resolve-a-sign-in-error-on-the-application-page"></a>Een fout aanmelden op de toepassingspagina oplossen

U kunt zich aanmelden is en er vervolgens een fout weergegeven op de pagina van de toepassing. Dit treedt op wanneer Azure AD een token tot de toepassing krijgen, maar de toepassing geen het antwoord accepteert.   

U kunt de fout oplossen door de volgende stappen uit:

1. Als de toepassing zich in de Azure AD-galerie, moet u controleren of u de stappen voor het integreren van de toepassing met Azure AD hebt gevolgd. De integratie-instructies voor uw toepassing, Zie de [lijst met zelfstudies over integratie van SaaS-toepassing](../saas-apps/tutorial-list.md).
1. De SAML-antwoord ophalen.
    - Als de mijn Apps beveiligde aanmelding-extensie is geïnstalleerd, uit de **eenmalige aanmelding testen** blade, klikt u op **downloaden van het SAML-antwoord**.
    - Als de extensie is niet geïnstalleerd, gebruikt u een hulpprogramma zoals [Fiddler](https://www.telerik.com/fiddler) om op te halen van het SAML-antwoord. 
1. U ziet deze elementen in het SAML-antwoord-token:
   - De unieke id van de gebruiker van NameID-waarde en indeling
   - Uitgegeven claims in het token
   - Het certificaat is gebruikt voor het ondertekenen van het token. 

     Zie voor meer informatie over het SAML-antwoord [Single Sign-on SAML-protocol](single-sign-on-saml-protocol.md).

1. Nu dat u hebt de SAML-reactie gecontroleerd, Zie [fout op de pagina van een toepassing na de aanmelding](../manage-apps/application-sign-in-problem-application-error.md) voor instructies over het probleem op te lossen. 
1. Als u nog steeds niet kunnen aanmelden is, kunt u de leverancier van de toepassing vragen wat ontbreekt in het SAML-antwoord aan te geven.


## <a name="next-steps"></a>Volgende stappen

Nu dat eenmalige aanmelding voor uw toepassing werkt, kunt u [automatiseren van gebruikersinrichting en -opheffing in SaaS-toepassingen](../manage-apps/user-provisioning.md) of [aan de slag met voorwaardelijke toegang](../conditional-access/app-based-conditional-access.md).
