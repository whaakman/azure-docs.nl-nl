---
title: Problemen bij het installeren van de toegang tot de toepassing van het deelvenster Browseruitbreiding | Microsoft Docs
description: Het oplossen van veelvoorkomende fouten aangetroffen bij het installeren van de uitbreiding voor toegang tot Configuratiescherm browser
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
ms.openlocfilehash: 26dc5d5ffce84206450123132c0633c2aa323e9f
ms.sourcegitcommit: 0e1c4b925c778de4924c4985504a1791b8330c71
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/06/2018
---
# <a name="problem-installing-the-application-access-panel-browser-extension"></a>Problemen bij het installeren van de toegang tot de toepassing van het deelvenster Browseruitbreiding

Het Toegangspaneel is een portal op Internet waarmee een gebruiker met een account voor werk of school in Azure Active Directory (Azure AD) voor weergave in en start cloud-gebaseerde toepassingen die de Azure AD-beheerder heeft deze toegang verleend tot. Een gebruiker met Azure AD-edities kunt ook gebruiken voor groepsbeheer met Self-service en app-mogelijkheden voor beheer via het toegangsvenster. Het Toegangspaneel is gescheiden van de Azure-portal en vereist geen gebruikers een Azure-abonnement hebben.

Als u wilt gebruiken op basis van wachtwoorden eenmalige aanmelding (SSO) in het deelvenster toegang, moet de extensie Toegangspaneel worden geïnstalleerd in de browser van de gebruiker. Deze extensie wordt automatisch gedownload wanneer een gebruiker selecteert een toepassing die is geconfigureerd voor eenmalige aanmelding op basis van wachtwoorden.

## <a name="meeting-browser-requirements-for-the-access-panel"></a>Vergadering Browservereisten voor het toegangsvenster

Het toegangsvenster vereist een browser die JavaScript ondersteunt en CSS is ingeschakeld. Als u wilt gebruiken op basis van wachtwoorden eenmalige aanmelding (SSO) in het deelvenster toegang, moet de extensie Toegangspaneel worden geïnstalleerd in de browser van de gebruiker. Deze extensie wordt automatisch gedownload wanneer een gebruiker selecteert een toepassing die is geconfigureerd voor eenmalige aanmelding op basis van wachtwoorden.

Voor eenmalige aanmelding op basis van wachtwoorden kunnen van de eindgebruiker browsers zijn:

-   Rand verjaardagseditie van Windows 10 of hoger 

-   Chrome--Op Windows 7 of hoger, en op Mac OS X of hoger

-   Firefox 26,0 of later--op Windows XP SP2 of hoger, en op Mac OS X 10,6 of hoger

-   Internet Explorer 8, 9, 10, 11--op Windows 7 of hoger (beperkte ondersteuning)
## <a name="how-to-install-the-access-panel-browser-extension"></a>Het installeren van de Browseruitbreiding toegang Configuratiescherm

Volg de onderstaande stappen voor het installeren van de Browseruitbreiding toegang Configuratiescherm:

1.  Open de [Toegangspaneel](https://myapps.microsoft.com) in een van de ondersteunde browsers en meld u aan als een **gebruiker** in uw Azure AD.

2.  Klik op een **wachtwoord SSO toepassing** in het deelvenster toegang.

3.  Selecteer in het venster met de vraag om de software te installeren, **nu installeren**.

4.  Op basis van uw browser u omgeleid naar de downloadkoppeling. **Voeg** de uitbreiding van uw browser.

5.  Als uw browser wordt gevraagd, selecteert u op een **inschakelen** of **toestaan** de extensie.

6.  Nadat deze is geïnstalleerd, **opnieuw** uw browsersessie.

7.  Meld u aan in het deelvenster toegang en zien als u kunt **starten** uw wachtwoord SSO-toepassingen

U kunt ook de uitbreiding voor Chrome en rand van de onderstaande directe koppelingen downloaden:

-   [Uitbreiding van chrome toegang Configuratiescherm](https://chrome.google.com/webstore/detail/access-panel-extension/ggjhpefgjjfobnfoldnjipclpcfbgbhl)

-   [Uitbreiding van de rand toegang Configuratiescherm](https://www.microsoft.com/store/apps/9pc9sckkzk84) 

## <a name="how-do-i-use-the-my-apps-secure-sign-in-extension"></a>Hoe gebruik ik de extensie mijn Apps beveiligen aanmelden?
Het wijzigen van de standaard-URL voor mijn Apps voor de extensie

Als u van een andere mijn Apps URL dan https://myapps.microsoft.com gebruikmaakt vervolgens moet u uw standaard-URL echter de volgende stappen uit:
1. Terwijl niet aangemeld bij de extensie **Klik met de rechtermuisknop** het extensie-pictogram.
2. Klik op **URL voor mijn Apps selecteren** in het menu.
3. **Selecteer** uw standaard-URL.
4. Klik op het pictogram extensie.
5. Aanmelden bij de extensie door te selecteren **melden aan de slag**.

Meld u aan rechtstreeks in een app van de browser
1. Na de installatie van de extensie, aanmelding bij de extensie door te selecteren **melden aan de slag**.
2. Navigeer naar de **aanmelding URL** van de app die u aanmelden wilt bij dit wordt meestal de URL van de app die wordt weergegeven het aanmeldingsformulier.
3. De extensie moet een statuswijziging en kunt u dat een wachtwoord verkrijgbaar is, klikt u op de **extensie pictogram** aan te melden.

Een app van de extensie openen
1. Na de installatie van de extensie, aanmelding bij de extensie door te selecteren **melden aan de slag**.
2. Klik op het pictogram extensie openen de **menu**.
3. **Search** voor een app in de portal voor mijn Apps beschikbaar.
4. Klik op de app vanuit de **zoekresultaten** zodat deze toepassing.
5. De laatste drie apps gestart wordt ook weergegeven in de **recent gebruikte** snelkoppeling lijst

> [!NOTE]
> Deze opties zijn alleen beschikbaar voor de rand, Chrome, Firefox.

## <a name="setting-up-a-group-policy-for-internet-explorer"></a>Instellen van een groepsbeleid voor Internet Explorer

U kunt een groepsbeleid waarmee u op afstand installeren van de extensie Toegangspaneel voor Internet Explorer op computers van uw gebruikers instellen.

De vereisten zijn:

-   U hebt ingesteld [Active Directory Domain Services](https://msdn.microsoft.com/library/aa362244%28v=vs.85%29.aspx), en u hebt uw gebruikers machines toegevoegd aan uw domein.

-   U moet de machtiging 'Instellingen bewerken' voor het bewerken van het groepsbeleidsobject (GPO) hebben. Standaard hebben leden van de volgende beveiligingsgroepen deze machtiging: Domeinadministrators, Ondernemingsadministrators en Maker Eigenaar Groepsbeleid. [Meer informatie](https://technet.microsoft.com/library/cc781991%28v=ws.10%29.aspx).

Volg de zelfstudie [het implementeren van de uitbreiding van het Configuratiescherm toegang voor Internet Explorer met behulp van Groepsbeleid](active-directory-saas-ie-group-policy.md) voor stapsgewijze instructies voor het configureren van het Groepsbeleid en het implementeren voor gebruikers.

## <a name="troubleshoot-the-access-panel-extension-in-internet-explorer"></a>Problemen met de extensie van het Configuratiescherm toegang in Internet Explorer

Ga als volgt de [oplossen met de extensie van het Configuratiescherm toegang voor Internet Explorer](active-directory-saas-ie-troubleshooting.md) geleid voor toegang tot een diagnostische hulpprogramma's en stapsgewijze instructies over het configureren van de extensie voor IE.

> [!NOTE]
> Internet Explorer is beperkte ondersteuning en nieuwe software-updates niet meer ontvangt. Rand is de aanbevolen browser.

## <a name="if-these-troubleshooting-steps-do-not-resolve-the-issue"></a>Als bovenstaande stappen voor probleemoplossing het probleem niet oplossen

een ondersteuningsticket opent met de volgende informatie, indien beschikbaar:

-   Correlatie fout-ID

-   UPN (e-mailadres van de gebruiker)

-   TenantID

-   Browsertype

-   Tijdzone en de tijd/tijdsbestek tijdens fout optreedt

-   Fiddler traceringen

## <a name="next-steps"></a>Volgende stappen
[Wat is de toegang tot toepassingen en eenmalige aanmelding bij Azure Active Directory?](active-directory-appssoaccess-whatis.md)
