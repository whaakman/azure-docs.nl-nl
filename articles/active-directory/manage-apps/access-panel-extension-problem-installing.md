---
title: Installeer de extensie in de browser toepassing toegang Configuratiescherm - Azure | Microsoft Docs
description: Oplossen van veelvoorkomende fouten aangetroffen tijdens de installatie van het deelvenster browser-extensie voor toegang.
services: active-directory
documentationcenter: ''
author: barbkess
manager: mtillman
ms.assetid: ''
ms.service: active-directory
ms.component: app-mgmt
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 5/4/18
ms.author: barbkess
ms.reviewer: japere,asteen
ms.openlocfilehash: aa0602ec0ecb97e9174fe6ef552b88fc30900d09
ms.sourcegitcommit: af9cb4c4d9aaa1fbe4901af4fc3e49ef2c4e8d5e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/11/2018
ms.locfileid: "44356674"
---
# <a name="install-the-access-panel-browser-extension"></a>De toegang tot deelvenster browser-extensie installeren

Het toegangsvenster is een portal op Internet. Als u een werk hebt- of in Azure Active Directory (Azure AD schoolaccount), kunt u het toegangsvenster gebruiken om te bekijken en starten van cloud-gebaseerde toepassingen die een Azure AD-beheerder u toegang tot heeft verleend. 

Als u Azure AD-edities, kunt u ook groepsbeheer met Self-service en mogelijkheden voor app via het toegangsvenster gebruiken. 

Het toegangsvenster is gescheiden van de Azure-portal. Dit vereist niet dat u een Azure-abonnement hebt.

## <a name="web-browser-requirements"></a>Vereisten voor webbrowsers

Ten minste het toegangsvenster vereist een browser die ondersteuning biedt voor JavaScript en CSS is ingeschakeld. Om te worden aangemeld bij toepassingen via eenmalige aanmelding op basis van wachtwoorden in het toegangsvenster, moet u de uitbreiding voor toegang tot Configuratiescherm geïnstalleerd in uw browser hebben. De extensie wordt automatisch gedownload wanneer u een toepassing selecteert die geconfigureerd voor eenmalige aanmelding op basis van wachtwoorden.

U kunt een van de volgende browsers gebruiken voor eenmalige aanmelding op basis van wachtwoorden:

- **Edge**: Windows 10 Anniversary Edition of hoger. 
- **Chrome**: op Windows 7 of hoger, en op Mac OS X of hoger.
- **Firefox 26,0 of hoger**: in Windows XP SP2 of hoger, en op Mac OS X 10.6 of hoger.

## <a name="install-the-access-panel-browser-extension"></a>De toegang tot deelvenster browser-extensie installeren

Voor het installeren van de uitbreiding voor toegang tot deelvenster browser, het volgende doen:

1.  Open in een van de ondersteunde browsers en de [Toegangsvenster](https://myapps.microsoft.com), en meldt u zich aan als een gebruiker in uw Azure AD-account.

2.  Selecteer een wachtwoord gebaseerde SSO-toepassing.

3.  Wanneer u wordt gevraagd, selecteert u **nu installeren**.  
    U bent omgeleid naar de downloadkoppeling voor de geselecteerde browser. 
    
4.  Selecteer **Toevoegen**.

5.  Als u wordt gevraagd, ofwel **inschakelen** of **toestaan** de extensie.

6.  Nadat de installatie is voltooid, start u de browser opnieuw.

7.  Meld u aan het toegangsvenster en de controle om te zien of u uw wachtwoord gebaseerde SSO-toepassingen kunt starten.

U kunt ook de extensie downloaden voor Chrome en Microsoft Edge rechtstreeks vanuit de volgende sites:

- [Chrome-extensie](https://chrome.google.com/webstore/detail/access-panel-extension/ggjhpefgjjfobnfoldnjipclpcfbgbhl)
- [Edge-extensie](https://www.microsoft.com/store/apps/9pc9sckkzk84) 

## <a name="use-the-my-apps-secure-sign-in-extension"></a>Gebruik de mijn Apps beveiligde aanmelding-extensie
* Als u een URL voor mijn Apps dan `https://myapps.microsoft.com`, de standaard-URL configureren door het volgende te doen:
   1. Terwijl u bent *niet* aangemeld bij de extensie, met de rechtermuisknop op het pictogram van de extensie.
   2. Selecteer in het menu **URL voor mijn Apps**.
   3. Selecteer de standaard-URL.
   4. Selecteer het pictogram van de extensie.
   5. Als u wilt aanmelden bij de extensie, selecteer **Meld u aan de slag**.

* Om te melden bij rechtstreeks naar een app vanuit de browser, het volgende doen:
   1. Nadat u de uitbreiding hebt geïnstalleerd, aanmelden bij het selecteren van **Meld u aan de slag**.
   2. Meld u aan de app met de aanmeldings-URL.  
       De aanmeldings-URL is meestal de URL van de app die wordt weergegeven het aanmeldingsformulier.
      De extensie moet een statuswijziging en laat u weten dat een wachtwoord beschikbaar is.
   3. Om aan te melden, selecteert u het pictogram van de extensie.

* Voor het starten van een app van de extensie, het volgende doen:
   1. Nadat u de uitbreiding hebt geïnstalleerd, aanmelden bij het selecteren van **Meld u aan de slag**.
   2. Selecteer het pictogram van de extensie om het menu te openen.
   3. Zoeken naar een app die beschikbaar is in de portal mijn Apps.
   4. Selecteer de app in de lijst met zoekresultaten.  
       De laatste drie apps die u hebt gebruikt, worden weergegeven in de **onlangs gebruikte** snelkoppeling naar de lijst.
       
* Als u wilt gebruiken intern bedrijf URL's terwijl deze extern is, het volgende doen:
    1. [Application Proxy configureren](https://docs.microsoft.com/azure/active-directory/active-directory-application-proxy-enable) op uw tenant
    2. [De toepassing publiceren](https://docs.microsoft.com/azure/active-directory/application-proxy-publish-azure-portal) en -URL via de toepassingsproxy
    3. De extensie installeren en ME niet aanmelden door te selecteren, meld u aan de slag
    4. U kunt nu naar de URL van de interne bedrijf zelfs terwijl de externe bladeren

> [!NOTE]
> De voorgaande opties zijn alleen beschikbaar voor Microsoft Edge, Chrome en Firefox.

## <a name="set-up-a-group-policy-for-internet-explorer"></a>Instellen van een groepsbeleid voor Internet Explorer

U kunt een groepsbeleid waarmee u de uitbreiding van het Configuratiescherm toegang voor Internet Explorer op afstand installeren op uw gebruikers virtuele machines kunt instellen.

Voordat u een Groepsbeleid instelt, zorg ervoor dat:

-   U hebt ingesteld [Active Directory Domain Services](https://msdn.microsoft.com/library/aa362244%28v=vs.85%29.aspx), en u hebt uw gebruikers machines toegevoegd aan uw domein.

-   Als u wilt bewerken in het groepsbeleidsobject (GPO), moet u hebben *instellingen bewerken* machtigingen. Standaard deze machtiging is verleend aan leden van de volgende beveiligingsgroepen: Domeinadministrators, Ondernemingsadministrators en beveiligingsgroep Maker Eigenaar Groepsbeleid.

Zie voor stapsgewijze instructies over het configureren van het Groepsbeleid en deze is geïmplementeerd voor gebruikers, [de Configuratiescherm-extensie voor toegang voor Internet Explorer implementeren met behulp van Groepsbeleid](deploy-access-panel-browser-extension.md).

## <a name="troubleshoot-the-access-panel-extension-in-internet-explorer"></a>Problemen met de extensie van het deelvenster toegang in Internet Explorer oplossen

Zie voor toegang tot een hulpprogramma voor diagnostische gegevens en informatie over het configureren van de extensie voor Internet Explorer, [oplossen met de extensie van het Configuratiescherm toegang voor Internet Explorer](manage-access-panel-browser-extension.md).

> [!NOTE]
> Internet Explorer is ingeschakeld beperkte ondersteuning en nieuwe software-updates niet meer ontvangt. Edge is de aanbevolen browser.

## <a name="if-the-preceding-steps-do-not-resolve-the-issue"></a>Als de voorgaande stappen het probleem niet verhelpen

Als deze beschikbaar is, moet u een ondersteuningsticket openen met de volgende informatie:

-   Correlatie-ID van fout
-   UPN (e-mailadres van gebruiker)
-   TenantID
-   Browsertype
-   Tijdzone, de tijd en tijdsbestek als de fout is opgetreden
-   Fiddler traceringen

## <a name="next-steps"></a>Volgende stappen
[What is application access and single sign-on with Azure Active Directory?](what-is-single-sign-on.md) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?)
