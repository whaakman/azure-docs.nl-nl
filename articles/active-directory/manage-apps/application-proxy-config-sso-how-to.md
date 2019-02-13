---
title: Het configureren van eenmalige aanmelding tot een toepassing toepassingsproxy | Microsoft Docs
description: Het configureren van eenmalige aanmelding voor uw toepassing proxytoepassing snel
services: active-directory
documentationcenter: ''
author: CelesteDG
manager: mtillman
ms.assetid: ''
ms.service: active-directory
ms.subservice: app-mgmt
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 10/22/2018
ms.author: celested
ms.reviewer: asteen
ms.collection: M365-identity-device-management
ms.openlocfilehash: 7df7469a6975fa5e84b43487b524ca888718d7d2
ms.sourcegitcommit: 301128ea7d883d432720c64238b0d28ebe9aed59
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/13/2019
ms.locfileid: "56162319"
---
# <a name="how-to-configure-single-sign-on-to-an-application-proxy-application"></a>Single sign-on bij een toepassing Application Proxy configureren

Eenmalige aanmelding (SSO) kunt uw gebruikers toegang krijgen tot een toepassing zonder verificatie van meerdere keren. Dit kunt de één verificatie kan worden uitgevoerd in de cloud, op basis van Azure Active Directory, en de service of de Connector om de gebruiker voor het voltooien van elke aanvullende verificatie uitdagingen van de toepassing te imiteren.

## <a name="how-to-configure-single-sign-on"></a>Het configureren van eenmalige aanmelding op
Zorg ervoor dat uw toepassing is geconfigureerd voor pre-authenticatie via Azure Active Directory voor het configureren van eenmalige aanmelding. Als u wilt doen deze configuratie, gaat u naar **Azure Active Directory**  - &gt; **bedrijfstoepassingen**  - &gt; **alle toepassingen**   - &gt; Uw toepassing  **- &gt; Application Proxy**. Op deze pagina kunt u zien van het veld 'Vooraf-verificatie' en zorg ervoor dat is ingesteld op 'Azure Active Directory. 

Zie voor meer informatie over de methoden voor verificatie vooraf, stap 4 van de [app publishing document](application-proxy-add-on-premises-application.md).

   ![Methode voor verificatie vooraf in Azure portal](./media/application-proxy-config-sso-how-to/app-proxy.png)

## <a name="configuring-single-sign-on-modes-for-application-proxy-applications"></a>Modi voor eenmalige aanmelding configureren voor Application Proxy-toepassingen
Het specifieke type van eenmalige aanmelding configureren. De aanmeldings-methoden zijn ingedeeld op basis van de back-endtoepassing maakt gebruik van welk type verificatie. App-Proxy toepassingen ondersteuning bieden voor drie typen van aanmelding:

-   **Op basis van wachtwoorden Sign-On**: Wachtwoord gebaseerde aanmelding kan worden gebruikt voor elke toepassing die gebruikmaakt van velden gebruikersnaam en wachtwoord aan te melden. Configuratiestappen zijn [wachtwoord eenmalige aanmelding configureren voor een toepassing in Azure AD-galerie](configure-password-single-sign-on-gallery-applications.md).

-   **Geïntegreerde Windows-authenticatie**: Voor toepassingen met behulp van geïntegreerde Windows-verificatie (IWA), is eenmalige aanmelding ingeschakeld via Kerberos-beperkte delegatie (KCD). Deze methode biedt machtiging Toepassingsproxyconnectors in Active Directory te imiteren van gebruikers, en voor het verzenden en ontvangen van tokens in hun naam. Informatie over het configureren van KCD kunnen u vinden in de [eenmalige aanmelding met KCD documentatie](application-proxy-configure-single-sign-on-with-kcd.md).

-   **Headers gebaseerde aanmelding**: Headers gebaseerde aanmelding wordt ingeschakeld via een partnerschap en enige aanvullende configuratie vereist. Zie voor meer informatie over de samenwerking en stapsgewijze instructies voor het configureren van eenmalige aanmelding tot een toepassing die gebruikmaakt van headers voor verificatie, de [PingAccess voor Azure AD-documentatie](application-proxy-configure-single-sign-on-with-ping-access.md).

Elk van deze opties kunt u vinden door te gaan naar uw toepassing in 'Enterprise Application' en openen van de **Single Sign-On** pagina in het menu links. Houd er rekening mee dat als uw toepassing in de oude portal is gemaakt, u niet al deze opties ziet mogelijk.

Op deze pagina ook ziet u een extra optie voor eenmalige aanmelding: Gekoppelde aanmelding. Deze optie wordt ook ondersteund door de toepassingsproxy. Deze optie wordt echter niet toegevoegd eenmalige aanmelding voor de toepassing. Dat gezegd dat de toepassing mogelijk al eenmalige aanmelding geïmplementeerd met behulp van een andere service, zoals Active Directory Federation Services. 

Deze optie kunt een beheerder een koppeling maken naar een toepassing die gebruikers eerste land op bij het openen van de toepassing. Bijvoorbeeld, als er een toepassing die is geconfigureerd voor het verifiëren van gebruikers met behulp van Active Directory Federation Services 2.0, kunt een beheerder de optie ' gekoppelde aanmelding ' gebruiken om te maken van een koppeling naar het in het toegangsvenster.

## <a name="next-steps"></a>Volgende stappen
[Geef single sign-on bij uw apps met Application Proxy](application-proxy-configure-single-sign-on-with-kcd.md)
