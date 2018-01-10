---
title: Grondbeginselen van Azure identity management
description: Cloud-gebaseerde identiteiten zijn nu de beste manier om de controle behouden en inzicht in hoe en wanneer gebruikers toegang krijgen tot zakelijke toepassingen en gegevens.
keywords: 
author: jeffgilb
manager: mtillman
ms.reviewr: jsnow
ms.author: jeffgilb
ms.date: 07/05/2017
ms.topic: article
ms.prod: 
ms.service: azure
ms.technology: 
ms.assetid: 
ms.custom: it-pro
ms.openlocfilehash: a7eb669aac109ae8cde6d75129004d322c270651
ms.sourcegitcommit: 176c575aea7602682afd6214880aad0be6167c52
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/09/2018
---
# <a name="fundamentals-of-azure-identity-management"></a>Grondbeginselen van Azure identity management

Meer digitale bedrijfsbronnen Live buiten het bedrijfsnetwerk in de cloud en op apparaten, wordt een geweldige cloud-gebaseerde identiteit en toegang beheeroplossing noodzakelijk. Cloud-gebaseerde identiteiten zijn nu de beste manier om de controle behouden en inzicht in hoe en wanneer gebruikers toegang krijgen tot zakelijke toepassingen en gegevens.

Microsoft heeft identiteiten voor een cloud-gebaseerde is beveiligen via een tien jaar en nu met [Azure Active Directory (AD)](https://docs.microsoft.com/azure/active-directory/active-directory-editions), deze dezelfde beveiliging systemen voor u beschikbaar zijn. Met Azure AD zorg Ondernemingsadministrators eenvoudig gebruiker en de beheerder accountability met betere beveiliging en beheeracties dan ooit tevoren.

Azure AD Premium is een cloud-gebaseerde identiteit en toegang beheeroplossing met geavanceerde beveiliging mogelijkheden waarmee een beveiligde identiteit voor alle apps, identity protection (verbeterde door de [Microsoft intelligence beveiliging grafiek](https://www.microsoft.com/en-us/security/intelligence)), en Privileged Identity Management. Niet gewoon een bewaking of rapportage, Azure AD Premium kunt beveiligen van uw gebruikersidentiteiten in realtime en kunt u risico, adaptief toegangsbeleid ter bescherming van gegevens van uw organisatie te maken.

Bekijk deze korte video voor een snel overzicht van Azure AD identity management en beveiliging:
>[!VIDEO https://www.youtube.com/embed/9LGIJ2-FKIM]

Microsoft biedt niet alleen een identiteit waarmee u overal gaat, maar ook een set hulpprogramma's te automatiseren, helpen beveiligen en beheren IT binnen uw organisatie. Zelfs na de komst van cloudcomputing nog zich steeds verzoek om te beheren en controleren van de IT-taken, zoals de helpdesk wordt gebeld gebruikerswachtwoorden opnieuw instellen, beheer van de groep gebruikers en aanvragen van toepassingen. Verder dingen complicerende, meebrengen werknemers nu hun persoonlijke apparaten tot werk en het gebruik van direct beschikbaar SaaS-toepassingen. Hierdoor kunt u beheren van controle over hun toepassingen verschillende bedrijfsdatacenters en openbare cloudplatforms een belangrijke uitdaging.

[!INCLUDE [identity](../../includes/azure-ad-licenses.md)]

## <a name="connect-on-premises-active-directory-with-azure-ad-and-office-365"></a>Verbinding maken met on-premises Active Directory met Azure AD en Office 365
Organisaties die grote investeringen hebt gedaan in de lokale Active Directory kunnen u die investeringen in de cloud uitbreiden door hun on-premises adreslijsten integreren met Azure AD in [beheer van hybride identiteit](https://docs.microsoft.com/azure/active-directory/active-directory-hybrid-identity-design-considerations-overview). In dat geval, worden uw gebruikers productiever omdat zij één identiteit hebben voor toegang tot resources, ongeacht de locatie. Gebruikers en organisaties kunnen vervolgens gebruiken voor eenmalige aanmelding (SSO) voor toegang tot zowel lokale bronnen en cloudservices zoals Office 365.

[Azure AD Connect](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect) is het enige hulpprogramma, moet u de integratie gedaan. Azure AD Connect biedt mogelijkheden voor de ondersteuning van de behoeften van uw identiteit synchronisatie en vervangt oudere versies van identiteitsintegratie zoals DirSync en Azure AD Sync. Met Azure AD Connect wordt voor identiteits- en de synchronisatie tussen on-premises en Azure AD ingeschakeld via:

- Synchronisatie : met dit onderdeel worden gebruikers, groepen en andere objecten aangemaakt. Het kan ook gebruikt worden om te controleren of de identiteitsinformatie van uw on-premises gebruikers en groepen overeenkomt met de cloud. Terugschrijven van wachtwoord kan ook worden ingeschakeld on-premises adreslijsten om synchroon te houden wanneer een gebruiker het wachtwoord in Azure AD-updates.
- AD FS - federatie is een optionele functie die is verstrekt door Azure AD Connect die kan worden gebruikt voor het configureren van een hybride omgeving met een on-premises AD FS-infrastructuur. Federation kan worden gebruikt door organisaties adres complexe implementaties, zoals het eenmalige aanmelding, afdwinging van AD-aanmelden-beleid, en smartcard of een derde partij MFA.
- Statusbewaking - [Azure AD Connect Health](https://docs.microsoft.com/azure/active-directory/connect-health/active-directory-aadconnect-health) kan goede bewaking en een centrale locatie in de Azure portal om deze activiteit weer te geven.

## <a name="increase-productivity-and-reduce-helpdesk-costs-with-self-service-and-single-sign-on-experiences"></a>Productiviteit verhogen, en verlagen de kosten van de helpdesk met zelf en eenmalige aanmelding

Werknemers kunnen productiever wanneer ze hebben een één gebruikersnaam en wachtwoord te onthouden en een consistente ervaring vanaf elk apparaat. Ze ook tijd besparen wanneer ze kunnen uitvoeren selfservice taken, zoals [opnieuw instellen van een vergeten wachtwoord](https://docs.microsoft.com/azure/active-directory/active-directory-passwords) of het aanvragen van toegang tot een toepassing zonder te wachten op voor hulp bij de helpdesk.

Azure AD [op de lokale Active Directory breidt](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect) in de cloud, zodat gebruikers kunnen hun primaire organisatie-account gebruiken voor hun domein apparaten, bedrijfsresources, en alle web- en SaaS-toepassingen die ze nodig hebben voor hun werk. Naast niet hoeven onthouden meerdere sets met gebruikersnamen en wachtwoorden, toegang tot gebruikers toepassingen kan ook worden automatisch ingericht (of buiten gebruik gesteld) op basis van hun groepslidmaatschappen van de organisatie en hun status als een werknemer. En u kunt beheren die toegang voor galerie apps of voor uw eigen lokale apps die u hebt ontwikkeld en gepubliceerd via de [Azure AD-toepassingsproxy](https://docs.microsoft.com/azure/active-directory/active-directory-application-proxy-get-started).

## <a name="manage-and-control-access-to-corporate-resources"></a>Beheren en toegang tot bedrijfsbronnen beheren
Microsoft identiteits- en toegangsbeheer management oplossingen zorgen ervoor dat IT beveiligen toegang tot toepassingen en bronnen in het zakelijke datacenter en in de cloud, zoals het inschakelen van extra niveaus van de validatie van [multi-factorauthenticatie](https://docs.microsoft.com/azure/multi-factor-authentication/multi-factor-authentication-whats-next) en [beleidsregels voor voorwaardelijke toegang](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-azure-portal). Bewaking verdachte activiteiten via geavanceerde beveiliging rapportage, controle en waarschuwingen helpt bij het verminderen mogelijke beveiligingsproblemen.

Beleid voor voorwaardelijke toegang in Azure AD Premium geven u, de ondernemingsbeheerder, de mogelijkheid te maken van regels voor toegang op basis van beleid voor elke Azure AD verbonden-toepassing (SaaS-apps, aangepaste apps die worden uitgevoerd in de cloud of on-premises webtoepassingen). Azure AD deze beleidsregels in real-time geëvalueerd en worden ze afgedwongen wanneer een gebruiker probeert te krijgen tot een toepassing. Azure identity protection-beleid kunnen u automatisch actie ondernemen als verdachte activiteit wordt gedetecteerd. Deze acties kunnen opnemen blokkeert de toegang tot gebruikers op een hoog risico en gebruikerswachtwoorden opnieuw instellen als het lijkt erop dat de referenties zijn aangetast afdwingen van multi-factor authentication-server.


## <a name="azure-active-directory-privileged-identity-management"></a>Privileged Identity Management voor Azure Active Directory

[Privileged Identity Management](https://docs.microsoft.com/azure/active-directory/active-directory-privileged-identity-management-getting-started), opgenomen in de Azure Active Directory Premium P2-oplossing, kunt u detecteren, Beperk en bewaak beheerdersaccounts en de toegang tot bronnen in uw Azure Active Directory en andere Microsoft online services. Ook helpt u bij het beheren van beheerderstoegang op aanvraag voor de exacte periode u moet.

Privileged Identity Management kan op aanvraag administrator-rechten afgedwongen, zodat beheerders kunnen meerdere factoren geverifieerde, tijdelijke verhoging van hun bevoegdheden voor vooraf geconfigureerde perioden voordat hun accounts naar de status van een normale gebruiker teruggaat aanvragen.

## <a name="benefits-of-azure-identity"></a>Voordelen van Azure identiteit

Met Azure identity management kunt u:

-   Maken en beheren voor elke gebruiker één identiteit voor uw hele onderneming opzetten, gegevens synchroniseren gebruikers, groepen en apparaten met [Azure Active Directory Connect](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect).

-   Eenmalige aanmelding toegang bieden tot uw toepassingen met inbegrip van duizenden vooraf geïntegreerde SaaS-apps of beveiligde externe toegang bieden tot on-premises SaaS-toepassingen met behulp van de [Azure AD-toepassingsproxy](https://docs.microsoft.com/azure/active-directory/active-directory-application-proxy-get-started).

-   Toepassingsbeveiliging toegang inschakelen door af te dwingen op basis van regels [multi-Factor Authentication](https://docs.microsoft.com/azure/multi-factor-authentication/multi-factor-authentication-whats-next) voor zowel on-premises en cloud toepassingen.

-   De gebruikersproductiviteit met [selfservice voor wachtwoordherstel](https://docs.microsoft.com/azure/active-directory/active-directory-passwords), en de groep en de toepassing toegang aanvragen via de [MyApps portal](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-user-help).

-   Profiteren van de [hoge beschikbaarheid en betrouwbaarheid](https://docs.microsoft.com/azure/architecture/resiliency/high-availability-azure-applications) van een wereldwijd bedrijfsniveau cloud-gebaseerde identiteit en toegang beheeroplossing.

## <a name="next-steps"></a>Volgende stappen
[Meer informatie over Azure-identiteitsoplossingen](https://docs.microsoft.com/azure/active-directory/understand-azure-identity-solutions)