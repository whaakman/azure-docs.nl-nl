---
title: Voorwaardelijke toegang van Azure Active Directory | Microsoft Docs
description: Meer informatie over hoe voorwaardelijke toegang in Azure Active Directory kunt u beheren toegang vanaf een centrale locatie.
services: active-directory
keywords: voorwaardelijke toegang tot apps, voorwaardelijke toegang met Azure AD, beveiligde toegang tot bedrijfsresources, beleidsregels voor voorwaardelijke toegang
documentationcenter: ''
author: MarkusVi
manager: mtillman
editor: ''
ms.assetid: 8c1d978f-e80b-420e-853a-8bbddc4bcdad
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 02/23/2018
ms.author: markvi
ms.reviewer: calebb
ms.openlocfilehash: 94d34df771334319db36009053a7264f0aeacf46
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/28/2018
---
# <a name="conditional-access-in-azure-active-directory"></a>Voorwaardelijke toegang in Azure Active Directory

Beveiliging is een belangrijkste aandachtspunt voor organisaties met behulp van de cloud. Een belangrijk aspect van cloud-beveiliging is identiteits- en toegangsbeheer als het gaat om het beheer van uw cloudresources. Gebruikers hebben toegang tot bronnen van uw organisatie met behulp van verschillende apparaten en apps vanaf elke locatie in een wereld mobiel eerste, cloud eerste. Als gevolg van deze is alleen te focussen op wie toegang heeft tot een bron niet voldoende meer. Om de hoofdsleutel van de evenwicht tussen beveiliging en productiviteit, moeten IT-professionals ook rekening te houden hoe een resource wordt geopend in een besturingselement toegang besluit. U kunt deze vereiste oplossen met voorwaardelijke toegang van Azure AD. Voorwaardelijke toegang is een functie van Azure Active Directory waarmee u kunt besturingselementen op de toegang tot apps in uw omgeving op basis van bepaalde voorwaarden van een centrale locatie worden afgedwongen. 


![Besturingselement](./media/active-directory-conditional-access-azure-portal/81.png)

Dit artikel vindt u een conceptueel overzicht van voorwaardelijke toegang in Azure AD.


## <a name="common-scenarios"></a>Algemene scenario's

In een wereld mobiel eerste, cloud eerste kunnen Azure Active Directory eenmalige aanmelding aan apparaten, apps en services vanaf elke locatie. Met de komst van apparaten (inclusief BYOD), werken zakelijke netwerken en derden SaaS-apps, IT-professionals worden momenteel geconfronteerd met twee tegengestelde doelstellingen:

- Gebruikers kunnen productief zijn overal en wanneer
- Beveiligen van zakelijke activa op elk gewenst moment

U kunt de juiste toegangscontrole onder de vereiste voorwaarden toepassen met behulp van beleid voor voorwaardelijke toegang. Voorwaardelijke toegang van Azure AD biedt extra beveiliging wanneer nodig en buiten manier van uw gebruikers blijft wanneer deze niet. 

Hieronder volgen enkele algemene toegang problemen die het voorwaardelijke toegang u met kunt:



- **[Aanmelden risico](active-directory-conditional-access-conditions.md#sign-in-risk)**: Azure AD Identity Protection detecteert aanmelden risico's. Hoe u de toegang beperken als een risico voor gedetecteerde aanmelden wordt aangegeven een onjuiste actor dat? Wat gebeurt er als u wilt ophalen van een sterkere bewijs dat een aanmeldingspagina is uitgevoerd door de geldige gebruiker of uw twijfel sterk genoeg is zijn om bepaalde gebruikers toegang krijgen tot een app kunt zelfs blokkeren?

- **[Netwerklocatie](active-directory-conditional-access-locations.md)**: Azure AD is toegankelijk vanaf elke locatie. Wat gebeurt er als een toegangspoging wordt uitgevoerd vanaf een netwerklocatie die niet onder het beheer van uw IT-afdeling? Een combinatie van gebruikersnaam en het wachtwoord mogelijk voldoende als bewijs van identiteit voor toegang tot uw resources wil vanuit uw bedrijfsnetwerk. Wat gebeurt er als u vraag een sterkere bewijs van identiteit voor toegang probeert die geïnitieerd worden vanuit andere onverwachte landen of regio's van de wereld? Wat gebeurt er als u zelfs toegang wilt blokkeren probeert van bepaalde locaties?  

- **[Apparaatbeheer](active-directory-conditional-access-conditions.md#device-platforms)**: In Azure AD gebruikers hebben toegang tot cloud-apps van een breed scala aan apparaten, zoals mobiele en ook op persoonlijke apparaten. Wat gebeurt er als u vraag die toegangspogingen moet alleen worden uitgevoerd met behulp van apparaten die worden beheerd door uw IT-afdeling? Wat gebeurt er als zelfs wilt u bepaalde typen apparaten toegang hebben tot cloud-apps in uw omgeving blokkeren? 

- **[Clienttoepassing](active-directory-conditional-access-conditions.md#client-apps)**: vandaag de dag u toegang hebt tot veel cloud-apps met behulp van verschillende typen Apps die zoals web gebaseerde apps, mobiele apps of desktop-apps. Wat gebeurt er als een toegangspoging wordt uitgevoerd met behulp van een client-app-type dat ervoor zorgt bekende problemen dat? Wat gebeurt er als u een apparaat dat wordt beheerd door uw IT-afdeling voor bepaalde typen app nodig? 

Deze vragen en de bijbehorende antwoorden vertegenwoordigen gangbare scenario's voor voorwaardelijke toegang van Azure AD. Voorwaardelijke toegang is een functie van Azure Active Directory waarmee u voor het afhandelen van scenario's met een benadering op basis van beleid.


## <a name="conditional-access-policies"></a>Voorwaardelijk toegangsbeleid

Beleid voor voorwaardelijke toegang is de definitie van een access-scenario met het volgende patroon volgen:

![Besturingselement](./media/active-directory-conditional-access-azure-portal/10.png)

**Dit doen** definieert de reactie van uw beleid. Het is belangrijk te weten dat het doel van een beleid voor voorwaardelijke toegang is niet om toegang te verlenen aan een cloud-app. In Azure AD is verlenen van toegang tot cloud-apps onderwerp van de toewijzingen van gebruikers. Met een beleid voor voorwaardelijke toegang u bepalen hoe geautoriseerde gebruikers (gebruikers toegang tot een cloud-app gekregen hebben) hebben toegang tot cloud-apps onder bepaalde omstandigheden. U afdwingen aanvullende vereisten zoals multi-factor authentication-server, een beheerd apparaat en anderen in uw antwoord. In de context van voorwaardelijke toegang van Azure AD, worden de vereisten die uw beleid zorgt ervoor dat toegangsbeheer genoemd. In het meest beperkende formulier kan uw beleid voor de toegang blokkeren. Zie voor meer informatie [toegangscontroles van voorwaardelijke toegang van Azure Active Directory](active-directory-conditional-access-controls.md).
     

**Wanneer dit gebeurt** definieert de reden voor het activeren van uw beleid. Deze reden wordt gekenmerkt door een groep van de bestaande voorwaarden is voldaan. In de voorwaardelijke toegang van Azure AD heeft de toewijzing van twee voorwaarden een speciale rol spelen:

- **[Gebruikers](active-directory-conditional-access-conditions.md#users-and-groups)**: de gebruikers voor het uitvoeren van een toegangspoging (**die**). 

- **[Cloud-apps](active-directory-conditional-access-conditions.md#cloud-apps)**: de doelen van een toegangspoging (**wat**).    

Deze twee voorwaarden zijn verplicht in een beleid voor voorwaardelijke toegang. Naast de verplichte twee voorwaarden, kunt u ook aanvullende voorwaarden waarin wordt beschreven hoe de toegangspoging wordt uitgevoerd. Algemene voorbeelden gebruiken mobiele apparaten of de locaties die zich buiten uw bedrijfsnetwerk. Zie voor meer informatie [voorwaarden in voorwaardelijke toegang van Azure Active Directory](active-directory-conditional-access-conditions.md).   

De combinatie van voorwaarden met uw toegangsbeheer vertegenwoordigt een beleid voor voorwaardelijke toegang. 

![Besturingselement](./media/active-directory-conditional-access-azure-portal/51.png)

Met voorwaardelijke toegang van Azure AD, kunt u bepalen hoe gemachtigde gebruikers hebben toegang tot uw cloud-apps. Het doel van een beleid voor voorwaardelijke toegang is om af te dwingen aanvullende toegangsbeheer bij een poging toegang tot een cloud-app die wordt aangedreven door hoe een toegangspoging wordt uitgevoerd.

Een voordeel van het gebruik van een benadering op basis van beleid voor het beveiligen van toegang tot uw cloud-apps is dat u de vereisten voor uw omgeving met behulp van de structuur die worden beschreven in dit artikel zonder dat u de technische implementatie opstellen kunt starten. 

## <a name="what-you-need-to-know"></a>Wat u moet weten

### <a name="general-requirements-for-using-conditional-access"></a>Algemene vereisten voor het gebruik van voorwaardelijke toegang

U kunt voorwaardelijke toegang van Azure AD cloud om apps te beveiligen als een verificatiepoging afkomstig van is:

- Een webbrowser

- Een client-app die gebruikmaakt van moderne verificatie

- Exchange ActiveSync

Zie voor meer informatie [client-apps](active-directory-conditional-access-conditions.md#client-apps).

Sommige [cloud-apps](active-directory-conditional-access-conditions.md#cloud-apps) bieden ook ondersteuning voor legacy-verificatieprotocollen. Dit geldt, bijvoorbeeld voor SharePoint Online en Exchange Online. Wanneer een client-app u een verouderde verificatieprotocol gebruiken kunt voor toegang tot een cloud-app, kan geen beleid voor voorwaardelijke toegang op deze toegangspoging door Azure AD afdwingen. Om te voorkomen dat een client-app voor het omzeilen van het afdwingen van beleid, moet u controleren of het is mogelijk alleen moderne verificatie op de desbetreffende cloud-apps inschakelen.

Voorbeelden voor voorwaardelijke toegang van apps niet van toepassing op client zijn:

- Office 2010 en eerder

- Office 2013 wanneer moderne verificatie niet is ingeschakeld

Zie voor meer informatie [SharePoint Online en Exchange Online instellen voor voorwaardelijke toegang van Azure Active Directory](active-directory-conditional-access-no-modern-authentication.md).


### <a name="license-requirements-for-using-conditional-access"></a>Licentievereisten voor het gebruik van voorwaardelijke toegang

Voorwaardelijke toegang, is een Azure AD Premium-licentie vereist. Zie informatie over de juiste licentie voor uw vereisten [algemeen beschikbaar functies van de vrije, Basic en Premium-edities vergelijken](https://azure.microsoft.com/pricing/details/active-directory/).


## <a name="next-steps"></a>Volgende stappen

- Als u meer weten wilt over:
    - Voorwaarden, Zie [voorwaarden in voorwaardelijke toegang van Azure Active Directory](active-directory-conditional-access-conditions.md).

    - Toegang tot besturingselementen, Zie [toegangscontroles van voorwaardelijke toegang van Azure Active Directory](active-directory-conditional-access-controls.md).

- Als u wilt ophalen enige ervaring met het beleid voor voorwaardelijke toegang configureren, Zie [aan de slag met voorwaardelijke toegang in Azure Active Directory](active-directory-conditional-access-azure-portal-get-started.md).

- Als u klaar om te configureren van beleidsregels voor voorwaardelijke toegang voor uw omgeving bent, Zie de [best practices voor voorwaardelijke toegang in Azure Active Directory](active-directory-conditional-access-best-practices.md). 
