---
title: De grondbeginselen van Azure-identiteitsbeheer | Microsoft Docs
description: Cloud-identiteiten zijn momenteel de beste manier om controle te houden over en inzicht te hebben in de manier waarop gebruikers toegang hebben tot bedrijfstoepassingen en -gegevens en wanneer ze deze gebruiken.
keywords: ''
author: eross-msft
manager: mtillman
ms.reviewer: jsnow
ms.author: lizross
ms.date: 08/07/2018
ms.topic: overview
ms.prod: ''
ms.service: active-directory
ms.component: fundamentals
ms.technology: ''
ms.assetid: ''
ms.custom: it-pro
ms.openlocfilehash: 797c35bad03c063203e3616740af633b71835a9c
ms.sourcegitcommit: 4de6a8671c445fae31f760385710f17d504228f8
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/08/2018
ms.locfileid: "39640184"
---
# <a name="fundamentals-of-azure-identity-management"></a>Grondbeginselen van Azure-identiteitsbeheer

Omdat steeds meer digitale bedrijfsresources zich buiten het bedrijfsnetwerk bevinden, in de cloud en op apparaten, is het noodzakelijk geworden om een goede oplossing te hebben voor cloud-identiteiten en toegangsbeheer. Cloud-identiteiten zijn momenteel de beste manier om controle te houden over en inzicht te hebben in de manier waarop gebruikers toegang hebben tot bedrijfstoepassingen en -gegevens en wanneer ze deze gebruiken.

Cloud-identiteiten worden al meer dan een decennium lang beveiligd in Microsoft. En met [Azure Active Directory (AD)](active-directory-whatis.md) zijn dezelfde beveiligingssystemen nu ook beschikbaar voor u. Met Azure AD kunnen ondernemingsbeheerders eenvoudiger zorgen voor aansprakelijkheid van gebruikers en beheerders, met betere beveiliging en beheer dan ooit tevoren.

Azure AD Premium is een oplossing voor cloud-identiteit en toegangsbeheer met geavanceerde mogelijkheden voor beveiliging die één veilige identiteit voor alle apps, identiteitsbeveiliging (uitgebreid met de [beveiligingsgrafiek van Microsoft Intelligence](https://www.microsoft.com/en-us/security/intelligence)) en Privileged Identity Management biedt. Azure AD Premium is niet gewoon het zoveelste hulpprogramma voor controle of rapportage, maar biedt beveiliging in realtime voor gebruikersidentiteiten en zorgt ervoor dat u adaptief op risico’s gebaseerd toegangsbeleid kunt maken om de gegevens in uw organisatie te beveiligen.

Bekijk deze korte video voor een snel overzicht van Azure AD-identiteitsbeheer en -beveiliging:
>[!VIDEO https://www.youtube.com/embed/9LGIJ2-FKIM]

Microsoft biedt niet alleen een algemene identiteit, maar ook een set hulpprogramma’s om IT in uw organisatie te automatiseren, te helpen beveiligen en te beheren. Zelfs na de komst van cloudcomputing is er nog vraag naar het beheren van IT-taken, zoals oproepen naar de helpdesk voor het opnieuw instellen van gebruikerswachtwoorden, beheer van gebruikersgroepen en aanvragen voor toepassingen. Wat de zaken nog ingewikkelder maakt, is dat werknemers nu hun persoonlijke apparaten mee naar het werk nemen en eerder beschikbare SaaS-toepassingen gebruiken, waardoor het een grote uitdaging is om al deze toepassingen te beheren in verschillende datacenters en op verschillende openbare cloudplatforms.

[!INCLUDE [identity](../../../includes/azure-ad-licenses.md)]

## <a name="connect-on-premises-active-directory-with-azure-ad-and-office-365"></a>On-premises Active Directory verbinden met Azure AD en Office 365
Organisaties die grote investeringen hebben gedaan in on-premises Active Directory, kunnen deze investeringen uitbreiden naar de cloud door hun on-premises mappen met Azure AD te integreren in [hybride identiteitsbeheer](https://aka.ms/aadframework). Door dit te doen kunnen gebruikers productiever zijn doordat ze een algemene identiteit krijgen voor toegang tot resources, ongeacht de locatie. Gebruikers en organisaties kunnen vervolgens eenmalige aanmelding (SSO) gebruiken voor toegang tot zowel on-premises resources als cloudservices zoals Office 365.

[Azure AD Connect](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect) is het enige hulpprogramma dat u nodig hebt om de integratie uit te voeren. Azure AD Connect biedt mogelijkheden voor de ondersteuning van uw behoeften op het gebied van identiteitssynchronisatie en vervangt oudere versies van hulpprogramma’s voor identiteitsintegratie, zoals DirSync en Azure AD Sync. Met Azure AD Connect wordt identiteitsbeheer en -synchronisatie tussen on-premises en Azure AD mogelijk via:

- Synchronisatie: met dit onderdeel worden gebruikers, groepen en andere objecten aangemaakt. Het kan ook gebruikt worden om te controleren of de identiteitsinformatie van uw on-premises gebruikers en groepen overeenkomt met de cloud. Het terugschrijven van wachtwoorden kan ook worden ingeschakeld om on-premises mappen gesynchroniseerd te houden, wanneer een gebruiker een wachtwoord bijwerkt in Azure AD.
- Verificatie: wanneer Azure AD het nieuwe beheerpunt is, is verificatie de basis van cloudtoegang. Het kiezen van de juiste verificatiemethode is een belangrijke beslissing bij het instellen van een oplossing voor hybride Azure AD-identiteit. Raadpleeg [deze handleiding](https://aka.ms/auth-options) voor het maken van een keuze tussen cloudverificatie (Wachtwoord-hashsynchronisatie / Pass-through-verificatie) of federatieve verificatie (AD FS) voor uw organisatie.
- Statuscontrole: [Azure AD Connect Health](https://docs.microsoft.com/azure/active-directory/connect-health/active-directory-aadconnect-health) biedt goede controle en een centrale locatie in de Azure-portal om deze activiteit weer te geven.

## <a name="increase-productivity-and-reduce-helpdesk-costs-with-self-service-and-single-sign-on-experiences"></a>De productiviteit verhogen en de helpdesk-kosten beperken met behulp van selfservice en eenmalige aanmelding

Werknemers zijn productiever wanneer ze maar één gebruikersnaam en wachtwoord hoeven te onthouden en ze op verschillende apparaten een consistente ervaring hebben. Ze besparen ook tijd wanneer ze selfservicetaken kunnen uitvoeren, zoals [een vergeten wachtwoord opnieuw instellen](https://docs.microsoft.com/azure/active-directory/active-directory-passwords) of toegang tot een toepassing aanvragen zonder te hoeven wachten op hulp van de helpdesk.

Azure AD [is een uitbreiding van on-premises Active Directory](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect) naar de cloud. Hierdoor kunnen gebruikers hun primaire organisatieaccount gebruiken voor aan een domein toegevoegde apparaten, bedrijfsresources en alle web- en SaaS-toepassingen die ze nodig hebben om hun werk goed te kunnen doen. Naast het feit dat ze niet meerdere combinaties van gebruikersnaam/wachtwoord hoeven te onthouden, kan de toegang van gebruikers tot toepassingen ook automatisch worden ingericht (of de inrichting ervan ongedaan worden gemaakt) op basis van hun groepslidmaatschappen in de organisatie en hun status als werknemer. En u kunt deze toegang beheren voor galerie-apps of voor uw eigen on-premises apps die u hebt ontwikkeld en gepubliceerd via de [Azure AD-toepassingsproxy](https://docs.microsoft.com/azure/active-directory/active-directory-application-proxy-get-started).

## <a name="manage-and-control-access-to-corporate-resources"></a>Toegang tot bedrijfsresources beheren
Microsoft-oplossingen voor identiteits- en toegangsbeheer helpen IT-beheerders om de toegang te beveiligen tot toepassingen en resources in het hele bedrijfsdatacenter en in de cloud. Er worden extra validatieniveaus ingeschakeld, zoals [Multi-Factor Authentication ](https://docs.microsoft.com/azure/multi-factor-authentication/multi-factor-authentication-whats-next) en [voorwaardelijk toegangsbeleid](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-azure-portal). Het controleren van verdachte activiteiten via geavanceerde beveiligingsrapportage, controle en waarschuwingen, helpt om potentiële beveiligingsproblemen te verminderen.

Met voorwaardelijk toegangsbeleid in Azure AD Premium krijgt u, de ondernemingsbeheerder, de mogelijkheid om toegangsregels te maken op basis van beleid voor elke toepassing die is verbonden met Azure AD (SaaS-apps, aangepaste apps die worden uitgevoerd in de cloud of on-premises webtoepassingen). In Azure AD wordt dit beleid in realtime geëvalueerd en afgedwongen wanneer een gebruiker probeert om toegang te krijgen tot een toepassing. Op basis van beveiligingsbeleid voor Azure-identiteit kunt u automatisch actie ondernemen wanneer een verdachte activiteit wordt ontdekt. Mogelijke acties: toegang blokkeren voor gebruikers met een hoog risico, Multi-Factor Authentication afdwingen, en gebruikerswachtwoorden opnieuw instellen wanneer u denkt dat de referenties zijn aangetast.


## <a name="azure-active-directory-privileged-identity-management"></a>Azure Active Directory Privileged Identity Management

[Privileged Identity Management](https://docs.microsoft.com/azure/active-directory/active-directory-privileged-identity-management-getting-started) is opgenomen in de Azure Active Directory Premium P2-aanbieding, en biedt u de mogelijkheid om administratieve accounts en de bijbehorende toegang tot resources in Azure Active Directory en andere Microsoft-onlineservices te detecteren, te beperken en te controleren. Het helpt u ook bij het beheren van beheerderstoegang op aanvraag voor de exacte tijdsperiode dat deze toegang nodig is.

Met Privileged Identity Management kunnen beheerdersrechten op aanvraag worden afgedwongen, zodat beheerders een tijdelijke, op Multi-Factor Authentication gebaseerde, verhoging van hun rechten kunnen aanvragen gedurende een vooraf geconfigureerde tijdsperiode, voordat het account weer de normale gebruikersstatus krijgt.

## <a name="benefits-of-azure-identity"></a>Voordelen van Azure-identiteit

Met Azure-identiteitsbeheer kunt u:

-   Voor elke gebruiker één identiteit in de hele onderneming maken en beheren, waardoor gebruikers, groepen en apparaten gesynchroniseerd blijven met [Azure Active Directory Connect](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect).

-   Toegang bieden tot uw toepassingen via eenmalige aanmelding, inclusief duizenden vooraf geïntegreerde SaaS-apps, of veilige externe toegang bieden tot on-premises SaaS-toepassingen, met behulp van de [Azure Active Directory-toepassingsproxy](https://docs.microsoft.com/azure/active-directory/active-directory-application-proxy-get-started).

-   Toegangsbeveiliging tot toepassingen inschakelen door [Multi-Factor Authentication](https://docs.microsoft.com/azure/multi-factor-authentication/multi-factor-authentication-whats-next) op basis van regels af te dwingen voor zowel on-premises toepassingen als cloudtoepassingen.

-   De gebruikersproductiviteit verbeteren met [selfservice voor wachtwoordherstel](https://docs.microsoft.com/azure/active-directory/active-directory-passwords) en toegangsaanvragen voor groepen en toepassingen met behulp van de [MyApps-portal](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-user-help).

-   Uw voordeel doen met de [hoge beschikbaarheid en betrouwbaarheid](https://docs.microsoft.com/azure/architecture/resiliency/high-availability-azure-applications) van een wereldwijde zakelijke oplossing voor cloud-identiteit en toegangsbeheer.

## <a name="next-steps"></a>Volgende stappen
[Meer informatie over identiteitsoplossingen van Azure](https://docs.microsoft.com/azure/active-directory/understand-azure-identity-solutions)
