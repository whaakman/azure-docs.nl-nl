---
title: Azure identity & access best practices voor beveiliging | Microsoft Docs
description: In dit artikel biedt een set van aanbevolen procedures voor het identiteitsbeheer van en toegang beheren met ingebouwde mogelijkheden van Azure.
services: security
documentationcenter: na
author: barclayn
manager: mbaldwin
editor: TomSh
ms.assetid: 07d8e8a8-47e8-447c-9c06-3a88d2713bc1
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 04/26/2018
ms.author: barclayn
ms.openlocfilehash: e3fe033de05ed42d221795159461048790e1cec8
ms.sourcegitcommit: eaad191ede3510f07505b11e2d1bbfbaa7585dbd
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/03/2018
ms.locfileid: "39493299"
---
# <a name="azure-identity-management-and-access-control-security-best-practices"></a>Azure-identiteitsbeheer en toegangsbeheer best practices voor beveiliging

Veel Houd rekening met de identiteit van de nieuwe grens laag voor de beveiliging over die functie van het traditionele netwerk gerichte perspectief te nemen. Deze evolutie van het primaire pivot voor aandacht voor beveiliging en investeringen afkomstig zijn uit het feit dat netwerkverbindingen steeds poreuze zijn geworden en die defense perimeter mag niet zo effectief als ze eenmaal zijn voordat de explosie van [BYOD ](http://aka.ms/byodcg) apparaten en toepassingen.

In dit artikel wordt besproken hoe een verzameling Azure-identiteitsbeheer en aanbevolen procedures voor de beveiliging van access control. Deze aanbevolen procedures zijn afgeleid van onze ervaring met [Azure AD](../active-directory/fundamentals/active-directory-whatis.md) en de ervaringen van klanten, zoals zelf.

Voor elke best practice wordt we het volgende uitgelegd:

* Wat de beste manier is
* Waarom u deze wilt inschakelen, die best practices
* Wat kan het resultaat zijn als u een failover naar de aanbevolen procedure inschakelen
* Mogelijke alternatieven voor de aanbevolen procedure
* Hoe u meer informatie kunt krijgen om in te schakelen van de aanbevolen procedure

Deze Azure identiteits- en toegangsbeveiliging aanbevolen procedures voor is gebaseerd op een consensus advies en mogelijkheden van Azure-platform en functiesets, zoals deze bestaan in de tijd in dit artikel is geschreven. Adviezen en -technologieën na verloop van tijd worden gewijzigd en regelmatig op basis van deze wijzigingen in dit artikel wordt bijgewerkt.

Azure-identiteit en besturingselement best practices voor beveiliging die in dit artikel worden besproken omvatten:

* Centraliseer uw identiteitsbeheer
* Eenmalige aanmelding (SSO) inschakelen
* Wachtwoordbeheer implementeren
* Multi-factor authentication (MFA) voor gebruikers afdwingen
* Op rollen gebaseerd toegangsbeheer (RBAC) gebruiken
* Besturingselement locaties waar resources worden gemaakt met behulp van Resource Manager
* Handleiding voor ontwikkelaars kunnen gebruikmaken van mogelijkheden voor identiteits voor SaaS-apps
* Controleren voor verdachte activiteiten

## <a name="centralize-your-identity-management"></a>Centraliseer uw identiteitsbeheer

Een belangrijke stap voor het beveiligen van uw identiteit is om ervoor te zorgen dat kan IT-accounts beheren vanaf één locatie met betrekking tot waar dit account is gemaakt. Hoewel het merendeel van de bedrijven IT-organisaties hebben hun primaire account directory on-premises, hybride cloudimplementaties zijn gewerkt en het is belangrijk dat u hoe u begrijpt on-premises integreren en mappen in de cloud en bieden een naadloze ervaring voor de eindgebruiker.

Om dit te doen [hybride identiteit](../active-directory/active-directory-hybrid-identity-design-considerations-overview.md) scenario wordt aangeraden twee opties:

* Uw on-premises directory worden gesynchroniseerd met uw clouddirectory met Azure AD Connect
* Schakel eenmalige aanmelding met [wachtwoord-hashsynchronisatie](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnectsync-implement-password-hash-synchronization), [Pass through-verificatie](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-pass-through-authentication-faq) of uw on-premises identiteit met uw cloud directory met federeren [Active Directory Federatieservices](https://docs.microsoft.com/windows-server/identity/ad-fs/deployment/deploying-federation-servers) (AD FS)

Organisaties die niet voldoen aan hun on-premises identiteiten integreren in hun cloudidentiteit ervaren hogere administratieve overhead bij het beheren van accounts, waardoor de kans op fouten en schendingen van de beveiliging.

Zie het artikel voor meer informatie over Azure AD-synchronisatie [uw on-premises identiteiten integreren met Azure Active Directory](../active-directory/active-directory-aadconnect.md).

## <a name="enable-single-sign-on-sso"></a>Eenmalige aanmelding (SSO) inschakelen

Wanneer u meerdere mappen voor het beheren van hebt, wordt dit een probleem met beheerdersrechten niet alleen voor IT, maar ook voor eindgebruikers die meerdere wachtwoorden onthouden. Met behulp van [SSO](https://azure.microsoft.com/documentation/videos/overview-of-single-sign-on/) bieden van uw gebruikers de mogelijkheid van dezelfde set referenties gebruiken om te melden en toegang tot de bronnen die ze nodig hebben, ongeacht waar deze resource zich bevindt on-premises of in de cloud.

Eenmalige aanmelding gebruiken waarmee gebruikers toegang krijgen tot hun [SaaS-toepassingen](../active-directory/manage-apps/what-is-single-sign-on.md) op basis van hun organisatie-account in Azure AD. Dit is van toepassing niet alleen voor Microsoft SaaS-apps, maar ook met andere apps, zoals [Google Apps](../active-directory/saas-apps/google-apps-tutorial.md) en [Salesforce](../active-directory/saas-apps/salesforce-tutorial.md). Uw toepassing kan worden geconfigureerd voor het gebruik van Azure AD als een [SAML gebaseerde identiteit](../active-directory/fundamentals-identity.md) provider. Als een besturingselement voor beveiliging geeft Azure AD een token zodat ze zich aanmelden bij de toepassing, tenzij toegang met behulp van Azure AD hebben gekregen. U kunt toegang verlenen rechtstreeks of via een groep dat ze lid van zijn.

> [!NOTE]
> de beslissing om eenmalige aanmelding gebruiken is van invloed op hoe u uw on-premises directory's integreren met uw clouddirectory. Als u wilt dat eenmalige aanmelding, moet u Federatie, gebruiken omdat adreslijstsynchronisatie alleen leveren [dezelfde aanmelding](../active-directory/active-directory-aadconnect.md).
>
>

Organisaties die eenmalige aanmelding niet voor hun gebruikers en toepassingen verplicht worden meer blootgesteld aan scenario's waarbij gebruikers meerdere wachtwoorden, die rechtstreeks verhoogt de kans dat gebruikers wachtwoorden opnieuw gebruiken of met behulp van zwakke wachtwoorden.

U kunt meer informatie over Azure AD-eenmalige aanmelding lezen van het artikel [AD FS-beheer en aanpassingen met Azure AD Connect](../active-directory/active-directory-aadconnect-federation-management.md).

## <a name="deploy-password-management"></a>Wachtwoordbeheer implementeren

In scenario's waarin u meerdere tenants hebt, of u wilt dat gebruikers kunnen [hun eigen wachtwoord opnieuw instellen](../active-directory/user-help/active-directory-passwords-update-your-own-password.md), is het belangrijk dat u passende beveiligingsbeleidsregels gebruiken om misbruik te voorkomen. In Azure, kunt u gebruikmaken van de mogelijkheid van self-service voor wachtwoord opnieuw instellen en aanpassen van de beveiligingsopties om te voldoen aan uw zakelijke vereisten.

Het is belangrijk om te verkrijgen van feedback van gebruikers en leer van hun ervaringen als ze proberen uit te voeren van deze stappen. Op basis van deze ervaringen, een plan voor het oplossen van mogelijke problemen die zich tijdens de implementatie voor een grotere groep voordoen kunnen uitwerken. Het is ook raadzaam dat u de [activiteitenrapport voor wachtwoord opnieuw instellen van inschrijving](../active-directory/active-directory-passwords-get-insights.md) voor het bewaken van de gebruikers die zijn geregistreerd.

Organisaties die om te voorkomen dat wachtwoord wijzigen ondersteuningsaanvragen worden ingediend, maar schakel gebruikers hun eigen wachtwoorden opnieuw kunnen worden vatbaar voor een groter aanroepvolume naar de servicedesk vanwege problemen met wachtwoord. In organisaties die meerdere tenants hebt, is het dat u dit type functionaliteit implementeren en de gebruikers om uit te voeren binnen de beveiligingsgrenzen van de die zijn vastgelegd in het beveiligingsbeleid voor wachtwoordherstel inschakelen.

U kunt meer informatie over wachtwoordherstel door het lezen van het artikel [wachtwoordbeheer implementeren en gebruikers trainen om deze te gebruiken](../active-directory/authentication/howto-sspr-deployment.md).

## <a name="enforce-multi-factor-authentication-mfa-for-users"></a>Multi-factor authentication (MFA) voor gebruikers afdwingen

Voor organisaties die moeten voldoen aan industriële standaarden, zoals [PCI DSS versie 3.2](http://blog.pcisecuritystandards.org/preparing-for-pci-dss-32), meervoudige verificatie is een moet hebben de mogelijkheid voor het verifiëren van gebruikers. Buiten te voldoen aan industriële standaarden, het afdwingen van MFA om gebruikers te verifiëren ook-organisaties kan helpen om te beperken van referentie diefstal type aanval, zoals [Pass-the-Hash (PtH)](http://aka.ms/PtHPaper).

Als u Azure MFA inschakelt voor uw gebruikers, toevoegen u een tweede beveiligingslaag aan gebruikersaanmeldingen en transacties. In dit geval een transactie kan worden toegang tot een document in een bestandsserver of in uw SharePoint Online. Azure MFA helpt ook bij IT te verminderen de kans dat een verdachte referenties toegang tot de gegevens van de organisatie heeft.

Bijvoorbeeld: u Azure MFA afdwingen voor uw gebruikers en configureer deze voor het gebruik van een telefoongesprek of SMS-bericht als verificatie. Als de referenties van de gebruiker worden geknoeid, kan de aanvaller geen toegang tot alle bronnen omdat ze geen toegang tot het telefoonnummer van gebruiker. Organisaties die geen extra lagen van identiteitsbeveiliging toevoegen zijn vatbaar voor referentie diefstal aanval, wat tot inbreuk op gegevens leiden kan.

Een alternatief voor organisaties die de volledige verificatieset besturingselement willen on-premises houden is het gebruik van [Azure multi-factor Authentication-Server](../active-directory/authentication/howto-mfaserver-deploy.md), ook wel genoemd MFA on-premises. Met deze methode, zich kunt u nog steeds multi-factor authentication, terwijl de MFA server on-premises afdwingen.

Zie het artikel voor meer informatie over Azure MFA [aan de slag met Azure multi-factor Authentication in de cloud](../active-directory/authentication/howto-mfa-getstarted.md).

## <a name="use-role-based-access-control-rbac"></a>Op rollen gebaseerd toegangsbeheer (RBAC) gebruiken

Toegang beperken op basis van de [moet weten](https://en.wikipedia.org/wiki/Need_to_know) en [minimale bevoegdheden](https://en.wikipedia.org/wiki/Principle_of_least_privilege) beveiligingsprincipes is van cruciaal belang voor organisaties die willen beveiligingsbeleid voor toegang tot gegevens afdwingen. Azure Role-Based Access Control (RBAC) kan worden gebruikt om machtigingen toewijzen aan gebruikers, groepen en toepassingen met een bepaald bereik. Het bereik van een roltoewijzing kan een abonnement, een resourcegroep of één resource zijn.

U kunt gebruikmaken van [ingebouwde RBAC](../role-based-access-control/built-in-roles.md) rollen in Azure machtigingen toewijzen aan gebruikers. Overweeg het gebruik van *Inzender voor Opslagaccounts* voor cloudoperators die nodig hebt om opslagaccounts te beheren en *Inzender voor klassieke Opslagaccounts* rol voor het beheren van klassieke opslagaccounts. Voor cloudoperators die nodig zijn voor het beheren van virtuele machines en storage-account, houd rekening met toe te voegen aan *Inzender voor virtuele machines* rol.

Organisaties die gegevenstoegangsbeheer niet afdwingen door gebruik te maken van mogelijkheden zoals RBAC mogelijk meer bevoegdheden dan nodig is om hun gebruikers geven. Dit kan leiden tot gegevens inbreuk op door dat onbevoegde gebruikers toegang tot bepaalde typen gegevens (bijvoorbeeld hoge impact op bedrijf) die ze in de eerste plaats al dan niet mogen hebben.

U kunt meer informatie over Azure RBAC lezen van het artikel [toegangsbeheer](../role-based-access-control/role-assignments-portal.md).

## <a name="control-locations-where-resources-are-created-using-resource-manager"></a>Besturingselement locaties waar resources worden gemaakt met behulp van Resource Manager

Het inschakelen van cloudoperators voor het uitvoeren van taken bij het voorkomen dat belangrijke conventies die nodig zijn om de resources van uw organisatie te beheren is belangrijk. Organisaties die willen bepalen de locaties waar resources zijn gemaakt moeten harde code voor deze locaties.

Om dit te bereiken, kunnen organisaties maken van beveiligingsbeleid die zijn gedefinieerd die worden beschreven van de acties of resources die worden geweigerd. U toewijzen die beleidsdefinities op het gewenste bereik, zoals het abonnement, resourcegroep of een afzonderlijke resource.

> [!NOTE]
> Dit is niet hetzelfde als RBAC, het daadwerkelijk maakt gebruik van RBAC voor het verifiëren van de gebruikers die beschikken over de bevoegdheid om deze resources te maken.
>
>

Maak gebruik van [Azure Resource Manager](../azure-resource-manager/resource-group-overview.md) om aangepaste beleid ook voor scenario's waarin de organisatie wil kunnen bewerkingen alleen wanneer de juiste kostenplaats gekoppeld is; anders wordt deze de aanvraag weigeren te maken.

Organisaties die niet zijn bepalen hoe resources worden gemaakt, zijn vatbaar voor gebruikers die misbruik van de service maken kunnen door het maken van meer resources dan ze nodig hebben. Beperking van het proces voor het maken van resources is een belangrijke stap voor het beveiligen van een scenario met meerdere tenants.

U kunt meer informatie over het maken van beleid met Azure Resource Manager met het lezen van het artikel [wat is Azure Policy?](../azure-policy/azure-policy-introduction.md)

## <a name="guide-developers-to-leverage-identity-capabilities-for-saas-apps"></a>Handleiding voor ontwikkelaars kunnen gebruikmaken van mogelijkheden voor identiteits voor SaaS-apps

Gebruikers-id wordt gebruikt in veel scenario's wanneer gebruikers toegang krijgen tot [SaaS-apps](https://azure.microsoft.com/marketplace/active-directory/all/) die kunnen worden geïntegreerd met on-premises of in de cloud directory. Allereerst omdat het beste ontwikkelaars gebruiken een veilige methode voor het ontwikkelen van deze apps, zoals [Microsoft Security Development Lifecycle (SDL)](https://www.microsoft.com/sdl/default.aspx). Azure AD authentication vereenvoudigt voor ontwikkelaars door op te geven van de identiteit als een service, met ondersteuning voor standaardprotocollen zoals [OAuth 2.0](http://oauth.net/2/) en [OpenID Connect](http://openid.net/connect/), ook als open source -bibliotheken voor verschillende platforms.

Zorg ervoor dat u het registreren van alle toepassingen die Azure AD-verificatie heeft, is dit een verplichte procedure. De reden achter dit is omdat de Azure AD moet coördineren van de communicatie met de toepassing bij het verwerken van eenmalige aanmelding (SSO) of uitwisselen van tokens. Sessie van de gebruiker is verlopen wanneer de levensduur van het token dat is uitgegeven door Azure AD is verlopen. Altijd evalueren als deze tijd moet in uw toepassing gebruiken of als u dit moment kunt verminderen. Vermindering van de levensduur kan fungeren als veiligheidsmaatregel die afdwingen gebruikers dat wordt zich afmelden op basis van een periode van inactiviteit.

Organisaties die niet afgedwongen door beheer van de identiteit voor toegang tot apps en kunnen niet de ontwikkelaars handleiding over hoe u apps veilig te integreren met hun identiteitsbeheersysteem mogelijk gevoeliger voor referentie diefstal type aanval, zoals [zwakke verificatie en sessie management wordt beschreven in Open Web Application Security Project (OWASP) Top 10](https://www.owasp.org/index.php/OWASP_Top_Ten_Cheat_Sheet).

U kunt meer informatie over verificatiescenario's voor SaaS-apps lezen [Verificatiescenario's voor Azure AD](../active-directory/develop/authentication-scenarios.md).

## <a name="actively-monitor-for-suspicious-activities"></a>Controleren voor verdachte activiteiten

Volgens [Verizon 2016 gegevens geschonden rapport](http://www.verizonenterprise.com/verizon-insights-lab/dbir/2016/), referentiediefstal is nog steeds in de groei en daardoor een van de meest winstgevende bedrijven cyber criminelen. Daarom is het belangrijk dat u hebt een actieve identiteitssysteem voor de monitor die snel kan activiteit verdacht gedrag detecteren en activeren van een waarschuwing voor verder onderzoek. Azure AD heeft twee belangrijke mogelijkheden waarmee organisaties hun identiteit te controleren: Azure AD Premium [anomaliedetectie rapporten](../active-directory/active-directory-view-access-usage-reports.md) en Azure AD [identiteitsbeveiliging](../active-directory/active-directory-identityprotection.md) mogelijkheid.

Zorg ervoor dat de anomaliedetectie-rapporten gebruiken om te identificeren van pogingen gedaan tot aanmelding [zonder worden getraceerd](../active-directory/active-directory-reporting-sign-ins-from-unknown-sources.md), [beveiligingsaanvallen](../active-directory/active-directory-reporting-sign-ins-after-multiple-failures.md) aanvallen op basis van een bepaald account, wil zich aanmelden vanaf meerdere locaties, zich aanmelden vanaf [ geïnfecteerde apparaten en verdachte IP-adressen. Houd er rekening mee dat dit rapporten zijn. U moet met andere woorden, processen en procedures voor IT-beheerders om uit te voeren van deze rapporten op het dagelijks uit te voeren of op aanvraag (meestal in een scenario voor respons op incidenten) hebben.

Daarentegen, Azure AD identity protection is een actieve bewakingssysteem en de huidige risico's op een eigen dashboard worden gemarkeerd. Behalve dat ontvangt u ook dagelijkse samenvatting meldingen via e-mail. Het is raadzaam dat u het risiconiveau op basis van uw zakelijke vereisten aanpassen. Het risiconiveau voor een risicogebeurtenis wordt aangegeven (hoog, Gemiddeld of laag) van de ernst van de risicogebeurtenis. Het risiconiveau helpt gebruikers bij Identity Protection prioriteit van de acties die ze uitvoeren moeten, vermindert het risico voor hun organisatie.

Organisaties die hun identiteitssystemen actief niet kunnen controleren, zijn op risico's van de gebruiker referenties zijn aangetast. Plaatsen met deze referenties zonder kennis die verdachte activiteiten worden genomen, organisaties niet mogelijk om te beperken van dit type bedreiging.
U kunt meer informatie over Azure Identity protection lezen [Azure Active Directory Identity Protection](../active-directory/active-directory-identityprotection.md).
