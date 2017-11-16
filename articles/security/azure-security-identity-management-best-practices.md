---
title: Azure identity & access aanbevolen beveiligingsprocedures | Microsoft Docs
description: In dit artikel biedt een set met aanbevolen procedures voor het identiteitsbeheer en toegang beheren met ingebouwde mogelijkheden van Azure.
services: security
documentationcenter: na
author: YuriDio
manager: swadhwa
editor: TomSh
ms.assetid: 07d8e8a8-47e8-447c-9c06-3a88d2713bc1
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 01/30/2017
ms.author: yurid
ms.openlocfilehash: d80fdd5a2e4339823c05368d76de333f3314d4ec
ms.sourcegitcommit: 9a61faf3463003375a53279e3adce241b5700879
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/15/2017
---
# <a name="azure-identity-management-and-access-control-security-best-practices"></a>Azure voor identiteits- en toegang beheren best practices voor beveiliging
Veel Overweeg identiteit van de nieuwe grens laag voor beveiliging die rol van de traditionele netwerk gericht perspectief overneemt. Deze evolutie van het primaire pivot voor aandacht voor beveiliging en investeringen afkomstig zijn uit het feit dat netwerkverbindingen steeds poreuze zijn geworden, en dat perimeternetwerk kan niet zo effectief als ze eenmaal worden zijn voordat de explosie van [BYOD ](http://aka.ms/byodcg) apparaten en cloud-toepassingen.

In dit artikel wordt een verzameling Azure identiteits- en aanbevolen beveiligingsprocedures access control besproken. Deze aanbevolen procedures zijn afgeleid van onze ervaring met [Azure AD](../active-directory/active-directory-whatis.md) en de ervaringen van klanten, zoals zelf.

Voor elke aanbevolen procedure wordt uitgelegd:

* Wat de beste handelswijze is
* Waarom u wilt dat het beste inschakelen
* Wat zijn het resultaat als u niet de aanbevolen procedure inschakelen
* Mogelijke alternatieven voor de aanbevolen procedure
* Hoe u kunt informatie over het inschakelen van de aanbevolen procedure

Deze Azure identiteits- en toegangsbeveiliging best practices artikel is gebaseerd op een advies consensus en mogelijkheden van Azure-platform en functiesets, zoals deze bestaan in de tijd in dit artikel is geschreven. Adviezen en technologieën op den duur veranderen en dit artikel wordt regelmatig gecontroleerd op basis van deze wijzigingen worden bijgewerkt.

Azure identity management en toegang besturingselement aanbevolen beveiligingsprocedures besproken in dit artikel zijn onder andere:

* Uw identiteitsbeheer centraliseren
* Eenmalige aanmelding (SSO) inschakelen
* Wachtwoordbeheer implementeren
* Afdwingen van multi-factor authentication (MFA) voor gebruikers
* Gebruik rollen gebaseerd toegangsbeheer (RBAC)
* Locaties waar resources worden gemaakt met resourcemanager beheren
* Handleiding voor ontwikkelaars van bureaubladtoepassingen identiteitsmogelijkheden voor SaaS-apps
* Actief bewaken voor verdachte activiteiten

## <a name="centralize-your-identity-management"></a>Uw identiteitsbeheer centraliseren
Een belangrijke stap voor het beveiligen van uw identiteit is om ervoor te zorgen dat kan IT accounts beheren vanaf één enkele locatie met betrekking tot waarop dit account is gemaakt. Hoewel de meeste van de ondernemingen IT-organisaties hun primaire account directory on-premises, hybride cloud-implementaties zijn in de lift en is het belangrijk dat u hoe integreren van lokale begrijpt en cloudadreslijsten en bieden een naadloze ervaring voor de eindgebruiker.

Om dit te bereiken [hybride identiteit](../active-directory/active-directory-hybrid-identity-design-considerations-overview.md) scenario raadzaam twee opties:

* Uw on-premises directory worden gesynchroniseerd met uw clouddirectory via Azure AD Connect
* Uw on-premises identiteits met uw cloud directory met federeren [Active Directory Federation Services](https://msdn.microsoft.com/library/bb897402.aspx) (AD FS)

Organisaties die niet voldoen aan hun on-premises identiteits integreren met hun cloudidentiteit krijgen hogere administratieve overhead bij het beheren van accounts, waardoor de kans op fouten en beveiligingsrisico's.

Lees het artikel voor meer informatie over Azure AD-synchronisatie [uw on-premises identiteiten integreren met Azure Active Directory](../active-directory/active-directory-aadconnect.md).

## <a name="enable-single-sign-on-sso"></a>Eenmalige aanmelding (SSO) inschakelen
Wanneer u meerdere mappen voor het beheren van hebt, dit wordt een administratieve probleem niet alleen voor IT, maar ook voor eindgebruikers die meerdere wachtwoorden onthouden. Met behulp van [SSO](https://azure.microsoft.com/documentation/videos/overview-of-single-sign-on/) u uw gebruikers de mogelijkheid van dezelfde set referenties gebruiken voor aanmelden en toegang tot de bronnen die ze nodig hebben, ongeacht waar deze resource zich lokaal is of in de cloud biedt.

Via eenmalige aanmelding waarmee gebruikers toegang krijgen tot hun [SaaS-toepassingen](../active-directory/active-directory-appssoaccess-whatis.md) op basis van hun organisatieaccount in Azure AD. Dit geldt niet alleen voor Microsoft SaaS-apps, maar ook met andere apps, zoals [Google Apps](../active-directory/active-directory-saas-google-apps-tutorial.md) en [Salesforce](../active-directory/active-directory-saas-salesforce-tutorial.md). Uw toepassing kan worden geconfigureerd voor het gebruik van Azure AD als een [SAML gebaseerde identiteit](../active-directory/fundamentals-identity.md) provider. Als een beveiligingscontrole geeft Azure AD geen token van een zodat ze zich aanmeldt bij de toepassing, tenzij ze gebruikmaken van Azure AD toegang hebben gekregen. U kunt toegang verlenen rechtstreeks of via een groep dat ze lid van zijn.

> [!NOTE]
> de beslissing om te gebruiken van eenmalige aanmelding heeft invloed op hoe u uw on-premises directory integreren met uw clouddirectory. Als u wilt dat eenmalige aanmelding, moet u Federatie, gebruiken omdat adreslijstsynchronisatie alleen leveren [dezelfde ervaring voor eenmalige aanmelding](../active-directory/active-directory-aadconnect.md).
>
>

Organisaties die eenmalige aanmelding niet voor hun gebruikers en toepassingen afdwingen worden meer blootgesteld aan scenario's waarbij gebruikers worden meerdere wachtwoorden die rechtstreeks verhoogt de kans op gebruikers hergebruik van wachtwoorden of met behulp van zwakke wachtwoorden.

U meer informatie over Azure AD SSO door te lezen van het artikel [AD FS-beheer en aanpassingen met Azure AD Connect](../active-directory/active-directory-aadconnect-federation-management.md).

## <a name="deploy-password-management"></a>Wachtwoordbeheer implementeren
In scenario's waar hebt u meerdere tenants of u wilt dat gebruikers kunnen [hun eigen wachtwoord opnieuw instellen](../active-directory/active-directory-passwords-update-your-own-password.md), is het belangrijk dat u passende beveiligingsbeleid gebruiken om te voorkomen dat misbruik. U kunt gebruikmaken van de mogelijkheid van zelf uw wachtwoord opnieuw instellen en aanpassen van de beveiligingsopties om te voldoen aan uw zakelijke vereisten in Azure.

Het is vooral belangrijk om feedback te verzamelen van deze gebruikers en te leren van hun ervaringen als ze proberen deze stappen uit te voeren. Op basis van deze ervaring, een abonnement om het risico van potentiële problemen die tijdens de implementatie voor een grotere groep optreden kunnen te lichten. Het is ook raadzaam dat u de [activiteit voor wachtwoord opnieuw instellen registratie rapport](../active-directory/active-directory-passwords-get-insights.md) voor het bewaken van de gebruikers die zijn geregistreerd.

Organisaties die u wilt voorkomen dat wachtwoord wijzigen ondersteuningsoproepen maar schakel gebruikers hun eigen wachtwoorden opnieuw kunnen instellen zijn vatbaarder voor een hogere Belvolume naar de servicedesk vanwege wachtwoordproblemen met. In organisaties die meerdere tenants hebben, is het noodzakelijk dat u dit type mogelijkheid implementeren en gebruikers kunnen uitvoeren binnen de beveiligingsgrenzen van de die zijn vastgelegd in het beveiligingsbeleid voor wachtwoordherstel inschakelen.

U kunt meer informatie over wachtwoord opnieuw instellen door te lezen van het artikel [wachtwoordbeheer implementeren en gebruikers leren gebruiken](../active-directory/active-directory-passwords-best-practices.md).

## <a name="enforce-multi-factor-authentication-mfa-for-users"></a>Afdwingen van multi-factor authentication (MFA) voor gebruikers
Voor organisaties die moeten zijn compatibel met de industriestandaarden, zoals [PCI DSS versie 3.2](http://blog.pcisecuritystandards.org/preparing-for-pci-dss-32), multi-factor authentication-server is een moet hebben de mogelijkheid voor het verifiëren van gebruikers. Afgezien van dat compatibel is met de industriestandaarden, afdwingen van MFA om gebruikers te verifiëren kan ook helpen organisaties te verhelpen credential theft type aanval, zoals [Pass-the-Hash (PtH)](http://aka.ms/PtHPaper).

Door Azure MFA inschakelen voor uw gebruikers, kunt u een tweede beveiligingslaag wilt toevoegen aan de gebruikersaanmeldingen en transacties. In dit geval een transactie kan toegang krijgen tot een document dat zich in een bestandsserver of op uw SharePoint Online. Azure MFA ook helpt IT verminderen de kans dat een verdachte referenties toegang tot gegevens van de organisatie hebben.

Bijvoorbeeld: u Azure MFA afdwingen voor uw gebruikers en configureer het om een telefoongesprek of tekstbericht gebruiken als verificatie. Als de referenties van de gebruiker verdacht zijn, kunnen de kwaadwillende persoon toegang tot alle bronnen omdat hij geen toegang tot het telefoonnummer van gebruiker niet mogelijk. Organisaties die Voeg geen extra beveiligingslagen identiteit zijn vatbaarder zijn voor de aanval diefstal van referenties, wat tot inbreuk op gegevens leiden kan.

Een alternatief voor organisaties die u behouden van de gehele verificatie besturingselement wilt on-premises is [Azure multi-factor Authentication-Server](../multi-factor-authentication/multi-factor-authentication-get-started-server.md), ook wel MFA lokale. Met deze methode kunt zich u nog steeds multi-factor authentication, terwijl de MFA-server on-premises afdwingen.

Lees het artikel voor meer informatie over Azure MFA [aan de slag met Azure multi-factor Authentication in de cloud](../multi-factor-authentication/multi-factor-authentication-get-started-cloud.md).

## <a name="use-role-based-access-control-rbac"></a>Gebruik rollen gebaseerd toegangsbeheer (RBAC)
De toegang beperken op basis van de [moet weten](https://en.wikipedia.org/wiki/Need_to_know) en [minimale bevoegdheden](https://en.wikipedia.org/wiki/Principle_of_least_privilege) beveiligingsprincipes is van cruciaal belang voor organisaties die willen beveiligingsbeleid instellen voor toegang tot gegevens. Azure op rollen gebaseerde toegangsbeheer (RBAC) kan worden gebruikt om machtigingen te wijzen aan gebruikers, groepen en toepassingen op een bepaalde scope. Het bereik van een roltoewijzing kan dit een abonnement, resourcegroep of één resource.

U kunt gebruikmaken van [ingebouwde RBAC](../active-directory/role-based-access-built-in-roles.md) rollen in Azure rechten toewijzen aan gebruikers. Overweeg het gebruik van *Storage Account Inzender* voor cloudoperators die nodig zijn voor het beheren van storage-accounts en *klassieke Storage Account Inzender* rol voor het beheren van klassieke opslagaccounts. Voor cloudoperators die nodig zijn voor het beheren van virtuele machines en storage-account, kunt toevoegen aan *Virtual Machine Contributor* rol.

Organisaties die niet afgedwongen door toegangsbeheer gegevens dankzij het gebruik van mogelijkheden, zoals RBAC mogelijk meer bevoegdheden beschikt dan nodig is om hun gebruikers geven. Dit kan leiden tot gegevens inbreuk door dat gebruikers toegang tot bepaalde typen gegevens (bijvoorbeeld invloedrijk) die ze in eerste instantie mag geen typen.

U kunt meer informatie over Azure RBAC lezen van het artikel [rollen gebaseerd toegangsbeheer](../active-directory/role-based-access-control-configure.md).

## <a name="control-locations-where-resources-are-created-using-resource-manager"></a>Locaties waar resources worden gemaakt met resourcemanager beheren
Het inschakelen van cloudoperators voor het uitvoeren van taken bij het voorkomen dat conventies die nodig zijn voor het beheren van bronnen van uw organisatie op te splitsen is erg belangrijk. Organisaties die willen bepalen van de locaties waar resources zijn gemaakt moeten harde code voor deze locaties.

Om dit te bereiken, kunnen organisaties maken van beleidsregels voor veiligheid zijn gedefinieerd met een beschrijving van de acties of resources die specifiek worden geweigerd. U toewijzen deze beleidsdefinities op het gewenste bereik, zoals het abonnement, resourcegroep of een afzonderlijke resource.

> [!NOTE]
> Dit is niet hetzelfde zijn als RBAC, het daadwerkelijk maakt gebruik van RBAC om de gebruikers die over de bevoegdheid voor het maken van deze bronnen te verifiëren.
>
>

Hefboomwerking [Azure Resource Manager](../azure-resource-manager/resource-group-overview.md) aangepaste beleidsregels maken ook voor scenario's waar de organisatie wil operations Sta alleen toe als de juiste kostenplaats gekoppeld is; anders wordt deze de aanvraag wordt geweigerd.

Organisaties die niet zijn beheren hoe resources worden gemaakt zijn vatbaarder voor gebruikers die de service mogelijk misbruik door het maken van meer bronnen dan ze nodig hebben. Beperking van een proces voor het maken van de resource is een belangrijke stap voor het beveiligen van een scenario met meerdere tenants.

U kunt meer informatie over het maken van beleid met Azure Resource Manager door te lezen van het artikel [wat is Azure beleid?](../azure-policy/azure-policy-introduction.md).

## <a name="guide-developers-to-leverage-identity-capabilities-for-saas-apps"></a>Handleiding voor ontwikkelaars van bureaubladtoepassingen identiteitsmogelijkheden voor SaaS-apps
Gebruikersidentiteit zal worden gebruikt in veel scenario's als gebruikers toegang hebben tot [SaaS-apps](https://azure.microsoft.com/marketplace/active-directory/all/) die kunnen worden geïntegreerd met on-premises of clouddirectory. Allereerst omdat het is raadzaam dat ontwikkelaars een beveiligde methodologie gebruiken deze om apps te ontwikkelen, zoals [Microsoft Security Development Lifecycle (SDL)](https://www.microsoft.com/sdl/default.aspx). Azure AD authentication vereenvoudigt voor ontwikkelaars dankzij de identiteit als een service met ondersteuning voor industriestandaard-protocollen, zoals [OAuth 2.0](http://oauth.net/2/) en [OpenID Connect](http://openid.net/connect/), evenals de open-source bibliotheken voor verschillende platforms.

Zorg ervoor dat alle toepassingen die verificatie met Azure AD heeft geregistreerd, is dit een verplichte procedure. De reden achter dit is omdat de Azure AD moet coördineren van de communicatie met de toepassing bij het verwerken van eenmalige aanmelding (SSO) of uitwisselen van tokens. De gebruikerssessie verloopt wanneer de levensduur van het token dat is uitgegeven door Azure AD is verlopen. Altijd geëvalueerd als uw toepassing deze tijd gebruikt of als u kunt deze tijd verkorten. Vermindering van de levensduur kan fungeren als veiligheidsmaatregel die gebruikers dwingen wordt om af te melden op basis van een periode van inactiviteit.

Het is mogelijk dat organisaties die niet afgedwongen door beheer van de identiteit voor toegang tot apps en komen niet hun ontwikkelaars begeleiden voor het veilig apps integreren met hun identiteitsbeheersysteem vatbaar voor referentietype diefstal van een aanval, zoals [zwakke verificatie- en management beschreven in de toepassing van Open Web Project beveiliging (OWASP) Top 10](https://www.owasp.org/index.php/OWASP_Top_Ten_Cheat_Sheet).

U meer informatie over verificatie scenario's voor SaaS-apps door te lezen [verificatie scenario's voor Azure AD](../active-directory/active-directory-authentication-scenarios.md).

## <a name="actively-monitor-for-suspicious-activities"></a>Actief bewaken voor verdachte activiteiten
Volgens [Verizon 2016 Data Breach rapport](http://www.verizonenterprise.com/verizon-insights-lab/dbir/2016/), referentiediefstal wordt nog steeds de stijging en hoe een van de meest winstgevend bedrijven cyberbeveiliging criminelen. Daarom is het belangrijk om te beschikken over een systeem van de monitor active identiteit dat snel kunt detecteren van de activiteit verdacht gedrag en activeren van een waarschuwing voor verder onderzoek. Azure AD heeft twee belangrijke mogelijkheden waarmee organisaties hun identiteit te controleren: Azure AD Premium [afwijkingsdetectie rapporten](../active-directory/active-directory-view-access-usage-reports.md) en Azure AD [identity protection](../active-directory/active-directory-identityprotection.md) mogelijkheid.

Zorg ervoor dat de afwijkingsdetectie-rapporten gebruiken om u te identificeren probeert aan te melden [zonder wordt getraceerd](../active-directory/active-directory-reporting-sign-ins-from-unknown-sources.md), [beveiligingsaanvallen](../active-directory/active-directory-reporting-sign-ins-after-multiple-failures.md) aanvallen op basis van een bepaalde account, wil zich aanmelden vanaf meerdere locaties, aan te melden [geïnfecteerde apparaten](../active-directory/active-directory-reporting-sign-ins-from-possibly-infected-devices.md) en verdachte IP-adressen. Houd er rekening mee dat deze rapporten. Met andere woorden, u moet beschikken over processen en procedures voor IT-beheerders voor het uitvoeren van deze rapporten op basis van de dagelijkse of op aanvraag (gewoonlijk in een respons op incidenten scenario).

Daarentegen, Azure AD identity protection is een actieve controlesysteem en wordt de huidige risico's op een eigen dashboard wordt gemarkeerd. Behalve dat ontvangt u ook dagelijkse samenvatting meldingen via e-mail. Het is raadzaam dat u het risiconiveau volgens de vereisten van uw bedrijf aanpassen. Het risiconiveau voor een risicogebeurtenis is een indicatie van de ernst van de risicogebeurtenis (hoog, Gemiddeld of laag). Het risiconiveau helpt Identity Protection gebruikers prioriteren van de acties die ze nemen moeten om het risico beperken tot hun organisatie.

Organisaties die niet actief bewaak hun identiteitssystemen, lopen risico dat de referenties van de gebruiker is geknoeid. Plaatsen met deze referenties zonder medeweten die verdachte activiteiten zijn genomen, organisaties niet mogelijk om het risico van dit type aanval te.
U meer informatie over Azure Identity protection door te lezen [Azure Active Directory: Identity Protection](../active-directory/active-directory-identityprotection.md).
