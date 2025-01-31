---
title: What is Azure Active Directory? (Engelstalig) -Azure Active Directory | Microsoft Docs
description: Overzicht en algemene informatie over Azure Active Directory, met inbegrip van de terminologie, welke licenties beschikbaar zijn en een lijst van verbonden functies met koppelingen voor meer informatie.
services: active-directory
author: msaburnley
manager: daveba
ms.service: active-directory
ms.topic: overview
ms.date: 05/08/2019
ms.author: ajburnle
ms.custom: it-pro, seodec18, seo-update-azuread-jan
ms.collection: M365-identity-device-management
ms.openlocfilehash: 8fafa7bd95801be46025727b2261fc95bc539988
ms.sourcegitcommit: f56b267b11f23ac8f6284bb662b38c7a8336e99b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/28/2019
ms.locfileid: "67440536"
---
# <a name="what-is-azure-active-directory"></a>What is Azure Active Directory? (Engelstalig)

Azure Active Directory (Azure AD) is van Microsoft cloud-gebaseerde identiteits- en toegangsbeheer management-service, waarmee uw werknemer zich aanmelden en toegang tot bronnen in:

- Externe resources, zoals Microsoft Office 365, de Azure-portal en duizenden andere SaaS-toepassingen.

- Interne resources, zoals apps op het bedrijfsnetwerk en intranet, samen met cloud-apps die door uw eigen organisatie zijn ontwikkeld.

U kunt gebruikmaken van de diverse posters van [Microsoft Cloud for Enterprise Architects Series](https://docs.microsoft.com/office365/enterprise/microsoft-cloud-it-architecture-resources#identity) voor een beter begrip van de kernidentiteitsservices in Azure, Azure AD en Office 365.

## <a name="who-uses-azure-ad"></a>Wie gebruikt Azure AD?

Azure AD is bedoeld voor:

- **IT-beheerders** Als IT-beheerder kunt u Azure AD gebruiken om de toegang tot uw apps en app-resources te beheren op basis van uw bedrijfsvereisten. U kunt bijvoorbeeld Azure AD gebruiken als u meervoudige verificatie wilt gebruiken bij het openen van belangrijke resources in de organisatie. Ook kunt u Azure AD gebruiken voor het automatisch laten inrichten van gebruikers tussen uw bestaande Windows Server AD en uw cloud-apps, waaronder Office 365. Ten slotte beschikt u met Azure AD over krachtige hulpprogramma's waarmee u automatisch gebruikersidentiteiten en -referenties kunt beveiligen en aan de vereisten voor toegangsbeheer te voldoen. U kunt aan de slag als u zich aanmeldt voor een [gratis proefversie van Azure Active Directory Premium van dertig dagen](https://azure.microsoft.com/trial/get-started-active-directory/).

- **App-ontwikkelaars.** Als app-ontwikkelaar hebt u met Azure AD een op standaarden gebaseerde benadering voor het toevoegen van eenmalige aanmelding (SSO) aan uw app, zodat deze kan werken met de al bestaande referenties van gebruikers. Azure AD biedt ook API's die u kunnen helpen zorgen dat ervaringen persoonlijke app met behulp van bestaande gegevens van de organisatie. U kunt aan de slag als u zich aanmeldt voor een [gratis proefversie van Azure Active Directory Premium van dertig dagen](https://azure.microsoft.com/trial/get-started-active-directory/). Voor meer informatie kunt u ook [Azure Active Directory voor ontwikkelaars](../develop/index.yml) raadplegen.

- **Abonnees van Microsoft 365, Office 365, Azure of Dynamics CRM Online.** Als abonnee gebruikt u al Azure AD. Elke tenant van Microsoft 365, Office 365, Azure en Dynamics CRM Online is automatisch een tenant van Azure AD. U kunt meteen beginnen met het beheren van de toegang tot uw geïntegreerde cloud-apps.

## <a name="what-are-the-azure-ad-licenses"></a>Wat zijn de licenties voor Azure AD?

Voor zakelijke Microsoft Online-services, zoals Office 365 of Microsoft Azure, hebt u Azure AD nodig om u aan te melden en voor de identiteitsbescherming. Als u zich op een zakelijke Microsoft Online service abonneert, krijgt u automatisch Azure AD met toegang tot de gratis functies.

Om uw Azure AD-implementatie te verbeteren, kunt u ook betaalde mogelijkheden toevoegen door een upgrade naar de Azure Active Directory Basic-, Premium P1- of Premium P2-licenties. Azure AD betaalde licenties zijn gebaseerd op uw bestaande gratis directory selfservice, uitgebreide bewaking, rapportage van beveiliging en veilige toegang bieden voor uw mobiele gebruikers.

>[!Note]
>Voor de prijsopties van deze licenties raadpleegt u [Prijzen voor Azure Active Directory](https://azure.microsoft.com/pricing/details/active-directory/).
>
>Azure Active Directory Premium P1, P2 Premium en Azure Active Directory Basic worden momenteel niet ondersteund in China. Voor meer informatie over de prijzen voor Azure AD, neem contact op met de [Azure Active Directory-Forum](https://azure.microsoft.com/support/community/?product=active-directory).

- **Azure Active Directory Free.** Biedt beheer van gebruikers en groepen, on-premises directory-synchronisatie, Basisrapporten, wijzigen van wachtwoorden via Self-service voor cloudgebruikers en eenmalige aanmelding voor Azure, Office 365 en vele populaire SaaS-apps.

- **Azure Active Directory Basic.** Naast de functies van de Free-licentie biedt Basic toegang tot apps in de cloud, op groepen gebaseerd toegangsbeheer, selfservice voor wachtwoordherstel voor cloud-apps en Azure Active Directory-toepassingsproxy, waarmee u on-premises web-apps kunt publiceren via Azure AD.

- **Azure Active Directory Premium P1.** Naast de Free- en Basic-licenties kunt u met P1 uw hybride gebruikers toegang verlenen tot zowel on-premises als cloudresources. P1 biedt tevens ondersteuning voor geavanceerd beheer, zoals dynamische groepen, selfservice voor groepsbeheer, Microsoft Identity Manager (een on-premises pakket voor identiteits- en toegangsbeheer) en cloudfuncties voor terugschrijven, die selfservice voor wachtwoordherstel voor on-premises gebruikers mogelijk maken.

- **Azure Active Directory Premium P2.** Naast de functies die gratis, basis en P1, P2 ook biedt [Azure Active Directory Identity Protection](../identity-protection/enable.md) om u te helpen de risico's gebaseerde voorwaardelijke toegang bieden tot uw apps en essentiële bedrijfsgegevens en [Privileged Identity Management](../privileged-identity-management/pim-getting-started.md) om te detecteren, beperken en controleren van beheerders en hun toegang tot bronnen en voor just-in-time-toegang wanneer dat nodig is.

- **Functielicenties met Betalen per gebruik.** U kunt ook aanvullende functielicenties krijgen, zoals Azure Active Directory Business-to-Customer (B2C). Met B2C kunt u identiteits- en toegangsbeheeroplossingen bieden voor klantgerichte apps. Voor meer informatie raadpleegt u de [documentatie over Azure Active Directory B2C](../../active-directory-b2c/index.yml).

Meer informatie over het koppelen van een Azure-abonnement aan Azure AD is te vinden in [Procedure: Een Azure-abonnement koppelen of toevoegen aan Azure Active Directory](active-directory-how-subscriptions-associated-directory.md); meer informatie over het toewijzen van licenties aan uw gebruikers is te vinden in [Procedure: Azure Active Directory-licenties toewijzen of verwijderen](license-users-groups.md).

## <a name="terminology"></a>Terminologie

Voor meer informatie over Azure AD en de bijbehorende documentatie raadzaam de volgende voorwaarden.

|Term of concept|Description|
|---------------|-----------|
|Identiteit| Een ding dat kunt u geverifieerde. Een identiteit kan een gebruiker met een gebruikersnaam en wachtwoord zijn. Identiteiten bevatten ook toepassingen of andere servers waarvoor verificatie via geheime sleutels of certificaten.|
|Account| Een identiteit met gegevens die zijn gekoppeld. U kunt een account zonder een identiteit kan niet hebt.|
|Azure AD-account| Een identiteit die wordt gemaakt via Azure AD of een andere cloudservice van Microsoft, bijvoorbeeld Office 365. Identiteiten worden opgeslagen in Azure AD en zijn toegankelijk voor de cloudservice-abonnementen van de organisatie. Dit account wordt ook wel een werk- of schoolaccount genoemd.|
|Azure-abonnement| Dit wordt gebruikt voor de betaling van Azure-cloudservices. U kunt zo veel abonnementen hebben als u wilt. Ze zijn gekoppeld aan uw creditcard.|
|Azure-tenant| De tenant is een speciaal en vertrouwd exemplaar van Azure AD dat automatisch wordt gemaakt wanneer uw organisatie zich registreert voor een abonnement op een cloudservice van Microsoft, bijvoorbeeld Microsoft Azure, Microsoft Intune of Office 365. Een Azure-tenant vertegenwoordigt één organisatie.|
|Eén tenant| Azure-tenants die toegang hebben tot andere services in een toegewezen omgeving, worden als één tenant beschouwd.|
|Multitenant| Azure-tenants die toegang hebben tot andere services in een gedeelde omgeving in meerdere organisaties, worden als meerdere tenants beschouwd.|
|Azure AD-directory|Elke Azure-tenant beschikt over een toegewezen en vertrouwde Azure AD-directory. Deze Azure AD-directory omvat de gebruikers, groepen en apps van de gebruiker en wordt gebruikt om identiteits- en toegangsbeheerfuncties voor resources van tenants uit te voeren.|
|Aangepast domein|Elke nieuwe Azure AD-directory heeft in eerste instantie een domeinnaam van de vorm domeinnaam.onmicrosoft.com. Naast deze initiële naam kunt u ook de domeinnamen van uw organisatie aan de lijst toevoegen. Deze omvatten de namen die u voor uw bedrijf gebruikt en waarmee uw gebruikers toegang tot de resources van de organisatie krijgen. Als u aangepaste domeinnamen toevoegt, kunt u gebruikersnamen maken waarmee uw gebruikers vertrouwd zijn, zoals alain@contoso.com.|
|Accountbeheerder|Deze rol van klassieke abonnementsbeheerder is conceptueel gezien de eigenaar facturering van een abonnement. Deze rol heeft toegang tot het [Azure-accountcentrum](https://account.azure.com/Subscriptions). Hier kunt u alle abonnementen van een account beheren. Zie voor meer informatie, [klassiek abonnement beheerdersrollen, functies van Azure Role-based access control (RBAC) en Azure AD-beheerdersrollen](../../role-based-access-control/rbac-and-directory-admin-roles.md).|
|Servicebeheerder|Met deze klassieke abonnementsbeheerdersrol kunt u alle Azure-resources beheren, inclusief de toegang. Deze rol heeft dezelfde toegang als een gebruiker met de rol van eigenaar op abonnementsniveau. Zie [Klassieke abonnementsbeheerdersrollen, Azure RBAC-rollen en Azure AD-beheerdersrollen](../../role-based-access-control/rbac-and-directory-admin-roles.md).|
|Eigenaar|Met deze rol kunt u alle Azure-resources beheren, inclusief de toegang. Deze rol bouwt voort op een nieuwer autorisatiesysteem, het zogenaamde toegangsbeheer op basis van rollen (RBAC), waarmee uiterst gedetailleerd toegangsbeheer tot Azure-resources kan worden verkregen. Zie [Klassieke abonnementsbeheerdersrollen, Azure RBAC-rollen en Azure AD-beheerdersrollen](../../role-based-access-control/rbac-and-directory-admin-roles.md).|
|Globale Azure AD-beheerder|Deze beheerdersrol wordt automatisch toegewezen aan personen die de Azure AD-tenant hebben gemaakt. Globale beheerders kan alle administratieve taken voor Azure AD en alle services die federeren naar Azure AD, zoals Exchange Online, SharePoint Online en Skype voor bedrijven Online. Er kunnen meerdere globale beheerders zijn, maar alleen globale beheerders kunnen beheerdersrollen aan gebruikers toewijzen (inclusief het toewijzen van andere globale beheerders).<br><br>**Opmerking**<br>Deze beheerdersrol wordt in de Azure-portal globale beheerder genoemd, maar in Microsoft Graph API, Azure AD Graph API en Azure AD PowerShell krijgt deze de naam **bedrijfsbeheerder**.<br><br>Zie [Machtigingen van de rol beheerder in Azure Active Directory](../users-groups-roles/directory-assign-admin-roles.md) voor meer informatie over de verschillende beheerdersrollen.|
|Microsoft-account (ook MSA genoemd)|Persoonlijke accounts die toegang verlenen tot uw consumentgerichte producten en cloudservices van Microsoft, bijvoorbeeld Outlook, OneDrive, Xbox LIVE en Office 365. Uw Microsoft-account wordt gemaakt en opgeslagen in het accountsysteem voor consumentidentiteiten van Microsoft, dat wordt beheerd door Microsoft.|

## <a name="which-features-work-in-azure-ad"></a>Welke functies werken in Azure AD?

Nadat u uw Azure AD-licentie hebt gekozen, krijgt u toegang tot sommige of alle van de volgende functies voor uw organisatie:

|Category|Description|
|-------|-----------|
|Toepassingsbeheer|Beheer uw apps in de cloud of on-premises met de toepassingsproxy, eenmalige aanmelding, de portal My Apps (ook wel Toegangsvenster genoemd) en SaaS-apps (Software als een dienst). Zie [Beveiligde externe toegang bieden voor on-premises toepassingen](../manage-apps/application-proxy.md) en [Documentatie over toepassingsbeheer](../manage-apps/index.yml) voor meer informatie.|
|Verificatie|In Azure Active Directory kunt u selfservice voor wachtwoordherstel, Multi-Factor Authentication, een aangepaste lijst met verboden wachtwoorden en slimme vergrendeling beheren. Zie [Documentatie voor Azure AD-verificatie](../authentication/index.yml) voor meer informatie.|
|Business-to-business (B2B)|U beheert gastgebruikers en externe partners, terwijl u de controle houdt over uw eigen zakelijke gegevens. Zie [Documentatie over Azure Active Directory B2B](../b2b/index.yml) voor meer informatie.|
|Business-to-customer (B2C)|U kunt aanpassen en controleren hoe uw gebruikers zich registreren, zich aanmelden en hun profielen beheren als ze uw apps gebruiken. Voor meer informatie raadpleegt u de [documentatie over Azure Active Directory B2C](../../active-directory-b2c/index.yml).|
|Voorwaardelijke toegang|Beheer de toegang tot uw cloud-apps. Zie [Documentatie over voorwaardelijke toegang voor Azure AD](../conditional-access/index.yml) voor meer informatie.|
|Azure Active Directory voor ontwikkelaars|Bouw apps waarmee alle Microsoft-identiteiten worden aangemeld en haal tokens op voor het aanroepen van Microsoft Graph, andere Microsoft-API's of aangepaste API's. Zie [Microsoft Identity Platform (Azure Active Directory voor ontwikkelaars)](../develop/index.yml) voor meer informatie.|
|Apparaatbeheer|Beheer hoe uw cloud- of on-premises apparaten toegang hebben tot uw zakelijke gegevens. Zie [Documentatie over Azure AD-apparaatbeheer](../devices/index.yml) voor meer informatie.|
|Domeinservices|Voeg virtuele Azure-machines toe aan een domein zonder gebruik te maken van domeincontrollers. Zie [Documentatie voor Azure AD Domain Services](../../active-directory-domain-services/index.yml) voor meer informatie.|
|Zakelijke gebruikers|Beheer het toewijzen van licenties, de toegang tot apps en het instellen van gedelegeerden met behulp van groepen en beheerdersrollen. Zie [Documentatie voor Azure Active Directory-gebruikersbeheer](../users-groups-roles/index.yml) voor meer informatie.|
|Hybride identiteit|Gebruik Azure Active Directory Connect en Connect Health om één gebruikersidentiteit te bieden voor verificatie en autorisatie van alle resources, ongeacht de locatie (cloud of on-premises). Zie [Documentatie voor hybride identiteit](../hybrid/index.yml) voor meer informatie.|
|Identiteitsbeheer|Beheer de identiteit van uw organisatie via besturingselementen voor werknemers, zakelijke partners, leveranciers, service en app-toegang. U kunt ook toegangsbeoordelingen uitvoeren. Zie [Documentatie over Azure AD Identity Governance](../governance/identity-governance-overview.md) en [Azure AD-toegangsbeoordelingen](../governance/access-reviews-overview.md) voor meer informatie.|
|Identiteitsbeveiliging|Detecteer potentiële beveiligingsproblemen die de identiteiten van uw organisatie treffen; configureer beleid om te reageren op verdachte activiteiten en neem vervolgens gepaste actie om de problemen op te lossen. Zie [Azure AD-identiteitsbeveiliging](../identity-protection/index.yml) voor meer informatie.|
|Beheerde identiteiten voor Azure-resources|Voorzie uw Azure-services van een automatisch beheerde identiteit in Azure AD die elke door Azure Ad ondersteunde verificatieservice kan verifiëren, inclusief Key Vault. Zie [Wat zijn beheerde identiteiten voor Azure-resources?](../managed-identities-azure-resources/overview.md) voor meer informatie.|
|Privileged Identity Management (PIM)|Beheer, controleer en bewaak de toegang binnen uw organisatie. Deze functie omvat toegang tot resources in Azure AD en Azure en andere Microsoft Online Services, zoals Office 365 en Intune. Zie [Azure AD Privileged Identity Management](../privileged-identity-management/index.yml) voor meer informatie.|
|Rapporten en controle|Krijg inzicht in de beveiliging en gebruikspatronen in uw omgeving. Zie [Azure Active Directory-rapporten en -bewaking](../reports-monitoring/index.yml) voor meer informatie.|

## <a name="next-steps"></a>Volgende stappen

- [Registreren voor Azure Active Directory Premium](active-directory-get-started-premium.md)

- [Een Azure-abonnement aan uw Azure Active Directory koppelen](active-directory-how-subscriptions-associated-directory.md)

- [Toegang tot Azure Active Directory en nieuwe tenant maken](active-directory-access-create-new-tenant.md)

- [Controlelijst voor implementatie van functies in Azure Active Directory Premium P2](active-directory-deployment-checklist-p2.md)
