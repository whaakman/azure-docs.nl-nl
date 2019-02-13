---
title: Delegering admin role - Azure Active Directory | Microsoft Docs
description: Delegering modellen, voorbeelden en rolbeveiliging in Azure Active Directory
services: active-directory
documentationcenter: ''
author: curtand
manager: mtillman
editor: ''
ms.service: active-directory
ms.workload: identity
ms.subservice: users-groups-roles
ms.topic: article
ms.date: 01/31/2019
ms.author: curtand
ms.reviewer: vincesm
ms.custom: it-pro
ms.collection: M365-identity-device-management
ms.openlocfilehash: b0cb6e2b1df062c3d056bd9a5aa0c1ff89f6636a
ms.sourcegitcommit: 301128ea7d883d432720c64238b0d28ebe9aed59
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/13/2019
ms.locfileid: "56201462"
---
# <a name="delegate-administration-in-azure-active-directory"></a>Beheer in Azure Active Directory delegeren

Wordt geleverd complexiteit met groei van de organisatie. Er is een algemene antwoord te verminderen enkele van de werkbelasting van toegangsbeheer met Azure Active Directory (AD)-beheerdersrollen. U kunt de geringst mogelijke bevoegdheid toewijzen aan gebruikers voor toegang tot hun apps en hun taken uitvoeren. Zelfs als u niet de globale beheerdersrol aan de eigenaar van elke toepassing toewijzen, kunt u de verantwoordelijkheden van application management wilt plaatsen op de bestaande globale beheerders. Er zijn veel redenen voor een organisatie verplaatsen naar een meer gedecentraliseerd beheer. In dit artikel kunt u van plan bent voor overdracht in uw organisatie.

<!--What about reporting? Who has which role and how do I audit?-->

## <a name="centralized-versus-delegated-permissions"></a>Gecentraliseerd ten opzichte van gedelegeerde machtigingen

Wanneer een organisatie groeit, kan het lastig zijn om bij te houden welke gebruikers specifieke-beheerdersrollen hebben. Als een werknemer administrator-rechten die ze al dan niet mogen heeft, is uw organisatie kan zijn vatbaar voor inbreuk op de beveiliging. Over het algemeen het aantal beheerders ondersteund en hoe nauwkeurig de machtigingen zijn afhankelijk van de grootte en complexiteit van uw implementatie.

* Bij kleine of proof of concept-implementaties kan doen één of een aantal beheerders alles; Er is geen delegering. In dit geval elke beheerder te maken met de rol globale beheerder.
* Bij implementaties met meer machines, toepassingen en bureaubladen, is meer overdracht vereist. Verschillende beheerders mogelijk meer specifieke functionele verantwoordelijkheden (rollen). Bijvoorbeeld sommige mogelijk Privileged Identity-beheerders en anderen toepassingsbeheerders mogelijk. Een beheerder kan bovendien alleen bepaalde groepen objecten zoals apparaten beheren.
* Nog grotere implementaties mogelijk nog meer gedetailleerde machtigingen, plus mogelijk beheerders met niet-conventionele of hybride functies.

In de Azure AD-portal, kunt u [weergeven van alle leden van een rol](directory-manage-roles-portal.md), kunt u snel uw implementatie en gemachtigde machtigingen te controleren.

Als u geïnteresseerd bent in toegang tot Azure-resources in plaats van met beheerdersrechten toegang delegeren in Azure AD, Zie [een Role-based access control (RBAC) rol toewijzen](../../role-based-access-control/role-assignments-portal.md).

## <a name="delegation-planning"></a>Overdracht plannen

Het werk voor het ontwikkelen van een overdrachtsmodel die past bij uw behoeften. Een analysemodel overdracht is een iteratieve ontwerpproces en we raden dat u als volgt te werk:

* Bepaalt de rollen die u nodig hebt
* App-beheer overdragen
* De mogelijkheid om toepassingen te registreren verlenen
* App-eigenaar delegeren
* Een beveiligingsplan ontwikkelen
* Tot stand brengen noodgevallen accounts
* Beveiligen van uw beheerdersrollen
* Bevoegde uitbreiding van bevoegdheden tijdelijke maken

## <a name="define-roles"></a>Rollen definiëren

De Active Directory-taken die worden uitgevoerd door beheerders en hoe deze worden toegewezen aan rollen bepalen. U kunt [beschrijvingen van gedetailleerde gebruikersrollen weer](directory-manage-roles-portal.md) in Azure portal.

Elke taak moet worden geëvalueerd voor de frequentie, prioriteit en problemen. Deze criteria zijn belangrijke aspecten van de taakdefinitie omdat ze bepalen of een machtiging moet worden overgedragen:

* Taken regelmatig doen, beperkt risico en pretje om zijn voltooid, zijn uitstekende kandidaten voor overdracht.
* Taken die u zelden maar grote gevolgen in de hele organisatie hebben en hoge vaardigheidsniveau vereisen moeten zorgvuldig worden overwogen voordat het delegeren. In plaats daarvan kunt u [tijdelijk een account uitbreiden naar de vereiste functieservices](../active-directory-privileged-identity-management-configure.md) of opnieuw toewijzen van de taak.

## <a name="delegate-app-administration"></a>App-beheer overdragen

De verspreiding van apps binnen uw organisatie kan uw overdrachtsmodel toegenomen. Als de belasting voor toegangsbeheer voor toepassingen worden geplaatst op de globale beheerder, is het waarschijnlijk dat model, neemt de overhead toe naarmate tijd verstrijkt. Als de rol globale beheerder voor zaken zoals toepassingen voor bedrijven configureren, hebt u mensen verleend, kunt u ze nu offloaden naar de volgende rollen met minder bevoegdheden. In dat geval kunt u uw beveiligingspostuur verbeteren en vermindert de kans op fouten erg vervelend. De beheerder van de meeste bevoegdheden toepassingsrollen zijn:

* De **toepassingsbeheerder** rol, de mogelijkheid hebben voor het beheren van alle toepassingen in de map, inclusief registraties, instellingen voor eenmalige aanmelding, gebruiker en toewijzingen van groepen en licenties, Application Proxy-instellingen en toestemming. Het verlenen niet de mogelijkheid voor het beheren van voorwaardelijke toegang.
* De **beheerder van de Cloudtoepassing** rol, waarmee alle mogelijkheden van de beheerder van de toepassing, tenzij deze geen toegang aan Application Proxy-instellingen verlenen (omdat het heeft geen machtiging on-premises) worden verleend.

## <a name="delegate-app-registration"></a>App-registratie delegeren

Standaard kunnen alle gebruikers toepassingsregistraties maken. Selectief verlenen toepassingsregistraties maken:

* Stel **gebruikers kunnen toepassingen registreren** op Nee in **gebruikersinstellingen**
* De gebruiker toewijzen aan de rol van ontwikkelaar van toepassingen

De mogelijkheid om in te stemmen dat een toepassing voor toegang tot gegevens selectief verlenen:

* Stel **gebruikers toestemming kunnen geven tot bedrijfsgegevens namens hen toegang tot toepassingen** op Nee in **gebruikersinstellingen**
* De gebruiker toewijzen aan de rol van ontwikkelaar van toepassingen

Als u de ontwikkelaar van een toepassing maakt een nieuwe toepassing registreren, worden ze automatisch toegevoegd als de eigenaar van de eerste.

## <a name="delegate-app-ownership"></a>App-eigenaar delegeren

Voor overdracht nog nauwkeurigere app-toegang, kunt u eigenaar toewijzen aan afzonderlijke zakelijke toepassingen. Dit is een aanvulling op de bestaande ondersteuning voor het toewijzen van eigenaren van de registratie van toepassing. Eigenaar wordt toegewezen op basis van per-enterprise-toepassing in de blade voor bedrijfstoepassingen. Het voordeel is eigenaren kunnen alleen de zakelijke toepassingen die waarvan ze eigenaar beheren. Bijvoorbeeld, u kunt een eigenaar toewijzen voor de Salesforce-toepassing en die eigenaar de toegang tot en configuratie voor Salesforce en geen andere toepassingen kunt beheren. Een zakelijke toepassing kan veel eigenaars hebben en een gebruiker kan de eigenaar voor veel zakelijke toepassingen zijn. Er zijn twee app-eigenaar rollen:

* De **Enterprise Toepassingseigenaar** rol hebben de mogelijkheid voor het beheren van de ' zakelijke toepassingen maken die de gebruiker eigenaar is, met inbegrip van instellingen voor eenmalige aanmelding, gebruiker en groepstoewijzingen en het toevoegen van extra eigenaren. Het verlenen niet de mogelijkheid voor het beheren van de Application Proxy-instellingen of voorwaardelijke toegang.
* De **registratie Toepassingseigenaar** rol hebben de mogelijkheid voor het beheren van de registratie voor de app die de gebruiker eigenaar is, met inbegrip van het manifest van de toepassing en het toevoegen van extra eigenaren.

## <a name="develop-a-security-plan"></a>Een beveiligingsplan ontwikkelen

Azure AD biedt een uitgebreide handleiding voor het plannen en uitvoeren van een beveiligingsplan op uw Azure AD-beheerdersrollen, [beveiligen van bevoegde toegang voor cloud- en hybride implementaties](directory-admin-roles-secure.md).

## <a name="establish-emergency-accounts"></a>Tot stand brengen noodgevallen accounts

Als u wilt behouden toegang tot uw identity management-store als het probleem zich voordoet, voorbereiden voor toegang in noodgevallen accounts volgens [EMS-toegang met beheerdersrechten accounts maken](directory-emergency-access.md).

## <a name="secure-your-administrator-roles"></a>Beveiligen van uw beheerdersrollen

Aanvallers die beheer van bevoegde accounts krijgen kan grote schade aanrichten, dus deze accounts beschermen eerst met behulp van de [basislijn toegangsbeleid](https://cloudblogs.microsoft.com/enterprisemobility/2018/06/22/baseline-security-policy-for-azure-ad-admin-accounts-in-public-preview/) die standaard beschikbaar voor alle Azure AD-tenants is (in openbare preview). Het beleid dwingt meervoudige verificatie in Azure AD-accounts met bevoegdheden. De volgende Azure AD-rollen worden gedekt door het basisbeleid voor Azure AD:

* Globale beheerder
* SharePoint-beheerder
* Exchange-beheerder
* Voorwaardelijke-toegangsbeheerder
* Beveiligingsbeheerder

## <a name="elevate-privilege-temporarily"></a>Tijdelijk bevoegdheden verhogen

Niet alle gebruikers moeten global administrator-rechten voor de meeste dagelijkse activiteiten, en niet alle permanent moet worden toegewezen aan de rol globale beheerder. Wanneer gebruikers de machtigingen van een globale beheerder moeten, moeten ze de toewijzing van rollen in Azure AD activeren [Privileged Identity Management](../active-directory-privileged-identity-management-configure.md) op hun eigen account of een alternatieve Administrator-account.

## <a name="next-steps"></a>Volgende stappen

Zie voor een verwijzing naar de beschrijvingen van de Azure AD-rol, [beheerdersrollen toewijzen in Azure AD](directory-assign-admin-roles.md)
