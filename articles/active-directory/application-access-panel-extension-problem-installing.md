---
title: De extensie in de browser toepassing toegang Configuratiescherm - Azure installeren | Microsoft Docs
description: Algemene fouten aangetroffen tijdens de installatie van de uitbreiding voor toegang tot Configuratiescherm browser oplossen.
services: active-directory
documentationcenter: 
author: ajamess
manager: mtillman
ms.assetid: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/15/2018
ms.author: asteen
ms.reviewer: japere
ms.openlocfilehash: c49cfad5f362f4402be476066f0e8c0158f20d73
ms.sourcegitcommit: ded74961ef7d1df2ef8ffbcd13eeea0f4aaa3219
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/29/2018
---
# <a name="install-the-access-panel-browser-extension"></a>De uitbreiding voor toegang tot Configuratiescherm browser installeren

Het toegangspaneel is een webportal. Als u een werk hebt of school-account in Azure Active Directory (Azure AD), kunt u het toegangsvenster weergeven en starten van cloud-gebaseerde toepassingen die Azure AD-beheerder u toegang tot heeft verleend. 

Als u Azure AD-edities, kunt u ook groepsbeheer met Self-service en app-mogelijkheden voor beheer via het toegangsvenster gebruiken. 

Het toegangspaneel is gescheiden van de Azure-portal. Dit vereist niet dat u een Azure-abonnement hebt.

## <a name="web-browser-requirements"></a>Vereisten voor webbrowsers

Ten minste het toegangsvenster vereist een browser die JavaScript ondersteunt en CSS is ingeschakeld. Als u wilt worden aangemeld via eenmalige aanmelding op basis van wachtwoorden in het deelvenster toegang aan toepassingen, moet u de uitbreiding van toegang tot Configuratiescherm geïnstalleerd in uw browser hebben. De extensie wordt automatisch gedownload wanneer u een toepassing die geconfigureerd voor eenmalige aanmelding op basis van wachtwoorden selecteert.

U kunt een van de volgende browsers gebruiken voor eenmalige aanmelding op basis van wachtwoorden:

- **Rand**: verjaardagseditie van Windows 10 of hoger. 
- **Chrome**: Windows 7 of hoger, en op Mac OS X of hoger.
- **Firefox 26,0 of hoger**: Windows XP SP2 of hoger, en op Mac OS X 10.6 of hoger.
- **Internet Explorer 8, 9, 10, 11**: op Windows 7 of hoger (beperkte ondersteuning).

## <a name="install-the-access-panel-browser-extension"></a>De uitbreiding voor toegang tot Configuratiescherm browser installeren

Voor het installeren van de uitbreiding voor toegang tot Configuratiescherm browser, het volgende doen:

1.  Open in een van de ondersteunde browsers en de [toegangspaneel](https://myapps.microsoft.com), en vervolgens weer aanmelden als gebruiker in uw Azure AD-account.

2.  Selecteer een toepassing eenmalige aanmelding op basis van wachtwoorden.

3.  Wanneer u wordt gevraagd, selecteert u **nu installeren**.  
    U wordt omgeleid naar de downloadkoppeling voor de geselecteerde browser. 
    
4.  Selecteer **Toevoegen**.

5.  Als u wordt gevraagd, ofwel **inschakelen** of **toestaan** de extensie.

6.  Nadat de installatie is voltooid, start u de browser opnieuw.

7.  Aanmelden bij het toegangspaneel en controleer om te zien of u uw toepassingen op basis van wachtwoorden SSO kunt starten.

U kunt ook de uitbreiding downloaden voor Chrome en rand rechtstreeks vanuit de volgende sites:

- [Chrome-uitbreiding](https://chrome.google.com/webstore/detail/access-panel-extension/ggjhpefgjjfobnfoldnjipclpcfbgbhl)
- [Edge-uitbreiding](https://www.microsoft.com/store/apps/9pc9sckkzk84) 

## <a name="use-the-my-apps-secure-sign-in-extension"></a>Gebruik de mijn Apps beveiligen aanmelden extensie
* Als u een URL voor mijn Apps dan `https://myapps.microsoft.com`, standaard-URL configureren door het volgende te doen:
   1. Terwijl u bent *niet* aangemeld bij de extensie, met de rechtermuisknop op het pictogram extensie.
   2. Selecteer in het menu **URL voor mijn Apps**.
   3. Selecteer uw standaard-URL.
   4. Selecteer het pictogram extensie.
   5. Als u wilt aanmelden bij de extensie, selecteer **melden aan de slag**.

* Om aan te melden rechtstreeks naar een app van de browser, het volgende doen:
   1. Nadat u de uitbreiding hebt geïnstalleerd, zich aanmelden bij het selecteren van **melden aan de slag**.
   2. Aanmelden bij de app met de aanmeldings-URL.  
       De aanmeldings-URL is meestal de URL van de app die het formulier aanmeldingspagina wordt weergegeven.
      De extensie moet een statuswijziging en laat u weten dat een wachtwoord beschikbaar is.
   3. Als u wilt aanmelden, selecteer het extensie-pictogram.

* Voor het starten van een app van de extensie, het volgende doen:
   1. Nadat u de uitbreiding hebt geïnstalleerd, zich aanmelden bij het selecteren van **melden aan de slag**.
   2. Selecteer het pictogram extensie om het menu te openen.
   3. Zoeken naar een app die beschikbaar is in de portal voor mijn Apps.
   4. Selecteer de app in de lijst met zoekresultaten.  
       De laatste drie apps die u hebt gebruikt, worden weergegeven in de **recent gebruikte** snelkoppeling lijst.

> [!NOTE]
> De voorgaande opties zijn alleen beschikbaar voor rand en Chrome, Firefox.

## <a name="set-up-a-group-policy-for-internet-explorer"></a>Instellen van een groepsbeleid voor Internet Explorer

U kunt een groepsbeleid waarmee u de uitbreiding van het deelvenster toegang tot voor Internet Explorer op afstand installeren op computers van uw gebruikers kunt instellen.

Voordat u een Groepsbeleid instellen, zorg ervoor dat:

-   U hebt ingesteld [Active Directory Domain Services](https://msdn.microsoft.com/library/aa362244%28v=vs.85%29.aspx), en u hebt uw gebruikers machines toegevoegd aan uw domein.

-   Als u wilt bewerken in het groepsbeleidsobject (GPO), moet u hebben *instellingen bewerken* machtigingen. Standaard wordt deze machtiging verleend aan leden van de volgende beveiligingsgroepen: Domeinadministrators, Ondernemingsadministrators en Maker Eigenaar Groepsbeleid.

Zie voor stapsgewijze instructies over het configureren van het Groepsbeleid en geïmplementeerd op gebruikers, [de extensie van het Configuratiescherm toegang voor Internet Explorer implementeren met behulp van Groepsbeleid](active-directory-saas-ie-group-policy.md).

## <a name="troubleshoot-the-access-panel-extension-in-internet-explorer"></a>Problemen met de extensie van het Configuratiescherm toegang in Internet Explorer

Zie voor toegang tot een hulpprogramma voor diagnostische gegevens en informatie over het configureren van de uitbreiding voor Internet Explorer, [oplossen met de extensie van het Configuratiescherm toegang voor Internet Explorer](active-directory-saas-ie-troubleshooting.md).

> [!NOTE]
> Internet Explorer is beperkte ondersteuning en nieuwe software-updates niet meer ontvangt. Rand is de aanbevolen browser.

## <a name="if-the-preceding-steps-do-not-resolve-the-issue"></a>Als de voorgaande stappen het probleem niet verhelpen

Als deze beschikbaar is, moet u een ondersteuningsticket opent met de volgende informatie:

-   Correlatie fout-ID
-   UPN (e-mailadres van de gebruiker)
-   TenantID
-   Browsertype
-   Tijdzone, tijd en tijdsbestek als de fout is opgetreden
-   Fiddler traceringen

## <a name="next-steps"></a>Volgende stappen
[Wat is de toegang tot toepassingen en eenmalige aanmelding bij Azure Active Directory?](active-directory-appssoaccess-whatis.md)
