---
title: Het configureren van eenmalige aanmelding tot een toepassing toepassingsproxy | Microsoft Docs
description: Het configureren van eenmalige aanmelding voor uw toepassing proxy-toepassing snel
services: active-directory
documentationcenter: 
author: ajamess
manager: femila
ms.assetid: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/11/2017
ms.author: asteen
ms.openlocfilehash: ccab427857b1439f37f3d9f193e35a4fc2237014
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/03/2017
---
# <a name="how-to-configure-single-sign-on-to-an-application-proxy-application"></a>Het configureren van eenmalige aanmelding tot een toepassing toepassingsproxy

Eenmalige aanmelding (SSO) kunt uw gebruikers toegang krijgen tot een toepassing zonder verificatie van meerdere keren. Deze staat het één verificatie om te worden uitgevoerd in de cloud, op basis van Azure Active Directory, en kan de service of de Connector om de gebruiker voor het voltooien van eventuele extra authenticatie uitdagingen van de toepassing te imiteren.

## <a name="how-to-configure-single-sign-on"></a>Het configureren van eenmalige aanmelding op
Zorg ervoor dat uw toepassing is geconfigureerd voor verificatie vooraf via Azure Active Directory voor het configureren van eenmalige aanmelding. Hiervoor gaat u naar **Azure Active Directory**  - &gt; **bedrijfstoepassingen**  - &gt; **alle toepassingen**  - &gt; uw toepassing  **- &gt; toepassingsproxy**. Op deze pagina kunt u het veld 'Vooraf-verificatie' Zie en controleer of die is ingesteld op 'Azure Active Directory. 

Zie voor meer informatie over de methoden voor verificatie vooraf stap 4 van de [app publishing document](https://docs.microsoft.com/azure/active-directory/application-proxy-publish-azure-portal).

   ![De methode vooraf-verificatie in Azure Portal](./media/application-proxy-config-sso-how-to/app-proxy.png)

## <a name="configuring-single-sign-on-modes-for-application-proxy-applications"></a>Modi voor eenmalige aanmelding configureren voor Application Proxy toepassingen
Vervolgens configureert we het specifieke type van eenmalige aanmelding. De methoden voor eenmalige aanmelding worden geclassificeerd op basis van de back-end-toepassing gebruikmaakt van welk type verificatie. Toepassingsproxy van toepassingen ondersteunt drie typen van eenmalige aanmelding:

-   **Op basis van wachtwoorden aanmelding**: op basis van wachtwoorden eenmalige aanmelding kan worden gebruikt voor andere toepassingen die gebruikmaken van de velden gebruikersnaam en wachtwoord aan te melden. Configuratiestappen vindt u in onze [wachtwoord SSO configuratiedocumentatie](https://docs.microsoft.com/azure/active-directory/active-directory-enterprise-apps-whats-new-azure-portal#bring-your-own-password-sso-applications).

-   **Geïntegreerde Windows-verificatie**: voor toepassingen met geïntegreerde Windows-verificatie (IWA), eenmalige aanmelding is ingeschakeld via Kerberos-beperkt delegatie (KCD). Dit wordt Application Proxy Connectors gemachtigd in Active Directory gebruikers, te imiteren en om het verzenden en ontvangen van tokens in hun naam. Meer informatie over het configureren van KCD vindt u in de [eenmalige aanmelding met KCD documentatie](https://docs.microsoft.com/azure/active-directory/active-directory-application-proxy-sso-using-kcd).

-   **Op basis van een koptekst aanmelding**: Header gebaseerde aanmelding via een verbinding is ingeschakeld en is aanvullende configuratie vereist. Zie voor meer informatie over de samenwerking en stapsgewijze instructies voor het configureren van eenmalige aanmelding tot een toepassing die gebruikmaakt van headers voor de verificatie, de [PingAccess voor Azure AD-documentatie](https://docs.microsoft.com/azure/active-directory/application-proxy-ping-access).

Elk van deze opties kunt u vinden door te gaan naar de toepassing in 'Bedrijfstoepassingen' en openen van de **Single Sign-On** pagina in het menu links. Houd er rekening mee dat als uw toepassing in de oude portal is gemaakt, u niet al deze opties ziet mogelijk.

Op deze pagina ook ziet u een extra optie voor eenmalige aanmelding: gekoppelde aanmelding. Dit wordt ook ondersteund door de toepassingsproxy. Houd er echter rekening mee dat deze optie geen eenmalige aanmelding tot de toepassing voegt. Die de toepassing heeft mogelijk al eenmalige aanmelding geïmplementeerd met een andere service zoals Active Directory Federation Services genoemd. 

Deze optie kunt een beheerder een koppeling maken naar een toepassing die gebruikers eerste land op bij het openen van de toepassing. Bijvoorbeeld, als er een toepassing die is geconfigureerd voor verificatie van gebruikers met behulp van Active Directory Federation Services 2.0, kan een beheerder de optie ' gekoppelde aanmelding ' gebruiken voor het maken van een koppeling in het deelvenster toegang.

## <a name="next-steps"></a>Volgende stappen
[Geef één aanmelding bij uw apps met toepassingsproxy](active-directory-application-proxy-sso-using-kcd.md)
