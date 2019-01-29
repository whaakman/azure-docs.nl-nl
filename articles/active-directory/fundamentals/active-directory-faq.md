---
title: Veelgestelde vragen (FAQ) - Azure Active Directory | Microsoft Docs
description: Veelgestelde vragen en antwoorden over Azure en Azure Active Directory, wachtwoordbeheer en toegang tot toepassingen.
services: active-directory
author: eross-msft
manager: daveba
ms.assetid: b8207760-9714-4871-93d5-f9893de31c8f
ms.service: active-directory
ms.subservice: fundamentals
ms.workload: identity
ms.topic: conceptual
ms.date: 11/12/2018
ms.author: lizross
ms.custom: it-pro, seodec18
ms.openlocfilehash: 2ff8f9764e61eb7f497e688c880b16529d1d6b8a
ms.sourcegitcommit: eecd816953c55df1671ffcf716cf975ba1b12e6b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/28/2019
ms.locfileid: "55093101"
---
# <a name="frequently-asked-questions-about-azure-active-directory"></a>Veelgestelde vragen over Azure Active Directory
Azure Active Directory (Azure AD) is een uitgebreide IDaaS-oplossing (Identity as a Service) waarin alle aspecten van identiteit, toegangsbeheer en beveiliging zijn opgenomen.

Zie [Wat is Azure Active Directory?](active-directory-whatis.md) voor meer informatie.


## <a name="access-azure-and-azure-active-directory"></a>Toegang tot Azure en Azure Active Directory
**V: Waarom krijg ik 'geen abonnementen gevonden' wanneer ik wil toegang tot Azure AD in Azure portal?**

**A:** Voor toegang tot de Azure-portal, moet elke gebruiker machtigingen hebben in een Azure-abonnement. Als u een betaald abonnement voor Office 365 of Azure AD hebt, gaat u naar [https://aka.ms/accessAAD](https://aka.ms/accessAAD) voor eenmalige activering in één stap. Anders moet u een gratis [Azure-account](https://azure.microsoft.com/pricing/free-trial/) activeren of een betaald abonnement afsluiten.

Zie voor meer informatie:

* [Hoe Azure-abonnementen worden gekoppeld aan Azure Active Directory](active-directory-how-subscriptions-associated-directory.md)

- - -
**V: Wat is de relatie tussen Azure AD, Office 365 en Azure?**

**A:** Azure AD biedt u algemene identiteit en toegang tot alle webservices. Als u Office 365, Microsoft Azure, Intune of andere toepassingen gebruikt, gebruikt u Azure AD al bij het inschakelen van aanmeldings- en toegangsbeheer voor deze services.

Alle gebruikers die webservices kunnen gebruiken, worden gedefinieerd als gebruikersaccounts in een of meer exemplaren van Azure AD. U kunt deze accounts instellen voor gratis Azure AD-functies, zoals toegang tot cloudtoepassingen.

Betaalde Azure AD-services zoals Enterprise Mobility + Security vormen een aanvulling op andere webservices, zoals Office 365 en Microsoft Azure, met uitgebreide oplossingen voor beheer en beveiliging die ook geschikt zijn voor grote organisaties.

- - -

**V:  Wat zijn de verschillen tussen de eigenaar en de globale beheerder?**

**A:** Standaard is de persoon die zich aanmeldt voor een Azure-abonnement de rol van eigenaar voor Azure-resources toegewezen. Een eigenaar kunt gebruiken voor een Microsoft-account of een werk- of school-account uit de directory waaraan het Azure-abonnement is gekoppeld.  Deze rol is gemachtigd om services in Azure Portal te beheren.

Als anderen zich moeten aanmelden en toegang tot services met behulp van hetzelfde abonnement, kunt u ze toewijzen de juiste [ingebouwde rol](../../role-based-access-control/built-in-roles.md). Zie voor meer informatie, [toegang met RBAC en de Azure-portal beheren](../../role-based-access-control/role-assignments-portal.md).

Standaard is de persoon die zich aanmeldt voor een Azure-abonnement de rol globale beheerder voor de map toegewezen. De globale beheerder heeft toegang tot alle functies van Azure AD-directory. Azure AD heeft een andere set beheerdersrollen voor het beheren van de directory en identiteitsgerelateerde functies. Deze beheerders hebben toegang tot verschillende functies in Azure portal. De rol van beheerder bepaalt wat ze kunnen doen, zoals het maken of bewerken van gebruikers, beheerdersrollen toewijzen aan anderen, gebruikerswachtwoorden opnieuw instellen, Gebruikerslicenties beheren of domeinen beheren.  Zie voor meer informatie over Azure AD-directorybeheerders en hun rollen, [een gebruiker toewijzen aan beheerdersrollen in Azure Active Directory](active-directory-users-assign-role-azure-portal.md) en [beheerdersrollen toewijzen in Azure Active Directory](../users-groups-roles/directory-assign-admin-roles.md).

Daarnaast vormen betaalde Azure AD-services zoals Enterprise Mobility + Security een aanvulling op andere webservices, zoals Office 365 en Microsoft Azure, met uitgebreide oplossingen voor beheer en beveiliging die ook geschikt zijn voor grote organisaties.

- - -
**V: Is er een rapport waarin staat wanneer mijn Azure AD-gebruikerslicenties verlopen?**

**A:** Nee.  Dit is momenteel niet beschikbaar.

- - -

## <a name="get-started-with-hybrid-azure-ad"></a>Aan de slag met hybride Azure AD


**V: Hoe verlaat ik een tenant wanneer ik ben toegevoegd als samenwerker?**

**A:** Wanneer u wordt toegevoegd aan de tenant van een andere organisatie als samenwerker, kunt u de 'tenantschakelaar' in de rechterbovenhoek gebruiken om over te schakelen tussen tenants.  Op dit moment is er geen manier om de organisatie die u heeft uitgenodigd, te verlaten. Microsoft is bezig om deze functie te ontwikkelen.  Tot deze functie beschikbaar is, kunt u de organisatie die u heeft uitgenodigd, vragen om u uit de tenant te verwijderen.
- - -
**V: Hoe kan ik mijn on-premises directory verbinden met Azure AD?**

**A:** U kunt uw on-premises directory verbinden met Azure AD met behulp van Azure AD Connect.

Zie [Integrating your on-premises identities with Azure Active Directory](../hybrid/whatis-hybrid-identity.md) (Uw on-premises identiteiten integreren met Azure Active Directory) voor meer informatie.

- - -
**V: Hoe stel ik eenmalige aanmelding in tussen mijn on-premises directory en Mijn cloudtoepassingen?**

**A:** U hoeft alleen het instellen van eenmalige aanmelding (SSO) tussen uw on-premises directory en Azure AD. Zo lang u uw cloudtoepassingen opent via Azure AD, wordt er met de service automatisch voor gezorgd dat uw gebruikers juist worden geverifieerd met hun on-premises referenties.

U kunt eenmalige aanmelding eenvoudig on-premises implementeren met federatieoplossingen, zoals Active Directory Federation Services (AD FS), of door wachtwoordhashsynchronisatie te configureren. U kunt beide opties eenvoudig implementeren met de wizard Azure AD Connect-configuratie.

Zie [Integrating your on-premises identities with Azure Active Directory](../hybrid/whatis-hybrid-identity.md) (Uw on-premises identiteiten integreren met Azure Active Directory) voor meer informatie.

- - -
**V: Biedt Azure AD een selfserviceportal voor gebruikers in mijn organisatie?**

**A:** Ja, Azure AD biedt u de [Azure AD-Toegangsvenster](https://myapps.microsoft.com) voor selfservice van gebruiker en toegang tot toepassingen. Als u Office 365 gebruikt, vindt u veel van dezelfde mogelijkheden in de Office 365-portal.

Zie [Introduction to the Access Panel](../user-help/active-directory-saas-access-panel-introduction.md) (Inleiding tot het toegangsvenster) voor meer informatie.

- - -
**V: Helpt Azure AD mij bij het beheren van mijn on-premises infrastructuur?**

**A:** Ja. Azure AD Connect Health is opgenomen in de Azure AD Premium-versie. Azure AD Connect Health helpt u om inzicht te verkrijgen in uw on-premises infrastructuur voor identiteiten en de synchronisatieservices.  

Zie [Monitor your on-premises identity infrastructure and synchronization services in the cloud](../hybrid/whatis-hybrid-identity-health.md) (Uw on-premises infrastructuur voor identiteiten en synchronisatieservices in de cloud controleren) voor meer informatie.  

- - -
## <a name="password-management"></a>Wachtwoordbeheer
**V: Kan ik Azure AD terugschrijven van wachtwoord gebruiken zonder Wachtwoordsynchronisatie? (Is het in dit scenario mogelijk om Azure AD selfservice voor wachtwoordherstel (SSPR) met terugschrijven van wachtwoorden te gebruiken zonder dat er wachtwoorden in de cloud worden opgeslagen?)**

**A:** U hoeft niet te synchroniseren van uw Active Directory-wachtwoorden naar Azure AD om terugschrijven inschakelen. In een federatieve omgeving is eenmalige aanmelding van Azure AD (SSO) afhankelijk van de on-premises directory voor het verifiëren van de gebruiker. In dit geval is er geen on-premises wachtwoord vereist dat moet worden bijgehouden in Azure AD.

- - -
**V: Hoe lang duurt het om een wachtwoord worden teruggeschreven naar on-premises Active Directory?**

**A:** Wachtwoord terugschrijven werkt in realtime.

Zie voor meer informatie [Getting started with password management](../authentication/quickstart-sspr.md).

- - -
**V: Kan ik terugschrijven van wachtwoord gebruiken met wachtwoorden die worden beheerd door een beheerder?**

**A:** Ja, hebt u wachtwoord terugschrijven is ingeschakeld, de wachtwoordbewerkingen van een beheerder teruggeschreven naar uw on-premises omgeving.  

Zie [Password management frequently asked questions](../authentication/active-directory-passwords-faq.md) (Veelgestelde vragen over wachtwoordbeheer) voor meer antwoorden op vragen over wachtwoorden.
- - -
**V:  Wat moet ik doen als ik mijn bestaande Office 365/Azure AD-wachtwoord tijdens het wijzigen van mijn wachtwoord vergeten?**

**A:** Er zijn een aantal opties voor dit type van de situatie.  Gebruik selfservice voor wachtwoordherstel (SSPR) als deze optie beschikbaar is.  Of SSPR werkt, is afhankelijk van de configuratie.  Zie voor meer informatie [Hoe werkt de portal voor wachtwoordherstel?](../authentication/howto-sspr-deployment.md).

Office 365-gebruikers kunnen hun beheerder vragen het wachtwoord opnieuw in te stellen via de stappen die worden beschreven in [Gebruikerswachtwoorden opnieuw instellen](https://support.office.com/article/Admins-Reset-user-passwords-7A5D073B-7FAE-4AA5-8F96-9ECD041ABA9C?ui=en-US&rs=en-US&ad=US).

Beheerders kunnen wachtwoorden van Azure AD-accounts via een van de volgende opties opnieuw instellen:

- [Accounts in Azure Portal opnieuw instellen](active-directory-users-reset-password-azure-portal.md)
- [PowerShell gebruiken](/powershell/module/msonline/set-msoluserpassword?view=azureadps-1.0)


- - -
## <a name="security"></a>Beveiliging
**V: Worden accounts na een bepaald aantal mislukte pogingen vergrendeld of wordt er een meer geavanceerde strategie gebruikt?**

We gebruiken een geavanceerdere strategie om account te vergrendelen.  Dit is gebaseerd op het IP-adres van de aanvraag en op de ingevoerde wachtwoorden. De duur van de vergrendeling wordt ook langer op basis van de kans dat het een aanval betreft.  

**V:  Bepaalde (algemene) wachtwoorden worden geweigerd met de berichten 'dit wachtwoord is gebruikt voor het aantal keren', heeft dit betrekking op wachtwoorden die worden gebruikt in de huidige active directory?**

Dit verwijst naar de wachtwoorden die wereldwijd gemeenschappelijk, zoals varianten van 'Wachtwoord' en '123456' zijn.

**V: Wordt een aanmeldingsaanvraag van twijfelachtige bronnen (botnets, tor-eindpunten) in een B2C-tenant geblokkeerd of is hiervoor een Basic of Premium-tenant?**

We hebben wel een gateway waarmee aanvragen worden gefilterd en die enige bescherming biedt tegen botnets. Deze wordt toegepast op alle B2C-tenants.

## <a name="application-access"></a>Toegang tot toepassingen

**V: Waar vind ik een lijst met toepassingen die vooraf geïntegreerd met Azure AD zijn en de bijbehorende mogelijkheden?**

**A:** Azure AD heeft meer dan 2.600 vooraf geïntegreerde toepassingen van Microsoft, toepassingsserviceproviders en partners. Alle vooraf geïntegreerde toepassingen bieden ondersteuning voor eenmalige aanmelding (SSO). Via SSO kunt u uw bedrijfsreferenties gebruiken om toegang te krijgen tot uw apps. Een aantal toepassingen ondersteunt ook geautomatiseerde inrichting en het ongedaan maken van de inrichting.

Zie de [Active Directory Marketplace](https://azure.microsoft.com/marketplace/active-directory/) voor een volledige lijst met vooraf geïntegreerde toepassingen.

- - -
**V: Wat gebeurt er als de toepassing die ik nodig heb is niet in de Azure AD marketplace?**

**A:** U kunt met Azure AD Premium, toevoegen en configureren van een toepassing die u wilt. U kunt eenmalige aanmelding en geautomatiseerde inrichting configureren, afhankelijk van de mogelijkheden van uw toepassing en uw voorkeuren.  

Zie voor meer informatie:

* [Configuring single sign-on to applications that are not in the Azure Active Directory application gallery](../manage-apps/configure-federated-single-sign-on-non-gallery-applications.md) (Eenmalige aanmelding configureren voor toepassingen die zich niet in de Azure Active Directory-toepassingsgalerie bevinden)
* [Using SCIM to enable automatic provisioning of users and groups from Azure Active Directory to applications](../manage-apps/use-scim-to-provision-users-and-groups.md) (SCIM gebruiken om in te stellen dat gebruikers en groepen van Azure Active Directory automatisch worden ingericht voor toepassingen)

- - -
**V: Hoe kunnen gebruikers zich toepassingen met behulp van Azure AD?**

**A:** Azure AD biedt verschillende manieren voor gebruikers kunnen zien en toegang krijgen tot hun toepassingen, zoals:

* Het deelvenster Azure AD Access
* Het startprogramma voor toepassingen van Office 365
* Directe aanmelding bij federatieve apps
* Dieptekoppelingen naar federatieve apps, op basis van wachtwoorden, of bestaande apps

Zie voor meer informatie, [eindgebruiker ervaringen voor toepassingen](../manage-apps/end-user-experiences.md).

- - -
**V: Wat zijn de verschillende manieren waarop Azure AD kunnen verificatie en eenmalige aanmelding voor toepassingen?**

**A:** Azure AD ondersteunt een groot aantal gestandaardiseerde protocollen voor verificatie en autorisatie, zoals SAML 2.0, OpenID Connect, OAuth 2.0 en WS-Federation. Daarnaast ondersteunt Azure AD het gebruik van wachtwoordkluizen en mogelijkheden voor automatische aanmelding voor apps die alleen ondersteuning bieden voor verificatie op basis van formulieren.  

Zie voor meer informatie:

* [Authentication Scenarios for Azure AD](../develop/authentication-scenarios.md) (Verificatiescenario's voor Azure AD)
* [Verificatie- en autorisatieprotocollen](https://msdn.microsoft.com/library/azure/dn151124.aspx)
* [Eenmalige aanmelding voor toepassingen in Azure AD](../manage-apps/what-is-single-sign-on.md)

- - -
**V: Kan ik toepassingen toevoegen die ik ben on-premises uitgevoerd?**

**A:** Azure AD Application Proxy biedt u eenvoudig en veilig toegang tot on-premises webtoepassingen die u kiest. U kunt deze toepassingen in Azure AD op dezelfde manier openen als uw SaaS-apps (Software as a Service). U hebt geen VPN nodig en u hoeft uw netwerkinfrastructuur niet te wijzigen.  

Zie [How to provide secure remote access to on-premises applications](../manage-apps/application-proxy.md) (Beveiligde externe toegang bieden voor on-premises toepassingen) voor meer informatie.

- - -
**V: Hoe moet ik multi-factor authentication voor gebruikers die toegang hebben tot een bepaalde toepassing?**

**A:** Met Azure AD voor voorwaardelijke toegang, kunt u voor elke toepassing een uniek toegangsbeleid toewijzen. In uw beleid kunt u aangeven dat Multi-Factor Authentication altijd is vereist of alleen wanneer gebruikers niet zijn verbonden met het lokale netwerk.  

Zie [Securing access to Office 365 and other apps connected to Azure Active Directory](../active-directory-conditional-access-azure-portal.md) (De toegang beveiligen tot Office 365 en andere apps die zijn verbonden met Azure Active Directory) voor meer informatie.

- - -
**V: Wat is geautomatiseerde gebruikersinrichting voor SaaS-apps?**

**A:** Azure AD gebruiken voor het maken, onderhoud en verwijderen van gebruikers-id's in veel populaire cloud-SaaS-apps automatiseren.

Zie [Automate user provisioning and deprovisioning to SaaS applications with Azure Active Directory](../manage-apps/user-provisioning.md) (Automatisch gebruikers inrichten en de inrichting ongedaan maken voor SaaS-toepassingen met Azure Active Directory) voor meer informatie.

- - -
**V:  Kan ik een veilige LDAP-verbinding met Azure AD instellen?**

**A:**  Nee. Azure AD biedt geen ondersteuning voor het LDAP-protocol. U kunt echter secure LDAP configureren met Azure AD Domain Services.
