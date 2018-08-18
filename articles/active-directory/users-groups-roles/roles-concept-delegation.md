---
title: Beheerdersrollen in Azure Active Directory delegeren | Microsoft Docs
description: Delegering modellen, voorbeelden en rolbeveiliging in Azure Active Directory
services: active-directory
documentationcenter: ''
author: curtand
manager: mtillman
editor: ''
ms.service: active-directory
ms.workload: identity
ms.component: users-groups-roles
ms.topic: article
ms.date: 08/09/2018
ms.author: curtand
ms.reviewer: vincesm
ms.custom: it-pro
ms.openlocfilehash: ad772a2e0c036c30aca2918496ac01f31157fc64
ms.sourcegitcommit: 30c7f9994cf6fcdfb580616ea8d6d251364c0cd1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/18/2018
ms.locfileid: "40208572"
---
# <a name="delegate-administration-in-azure-active-directory"></a>Beheer in Azure Active Directory delegeren

Met de groei van de organisatie komen meer complexiteit en één algemene antwoord is het verminderen van enkele van de overhead van het toegangsbeheer met Azure Active Directory (AD)-beheerdersrollen. U kunt de geringst mogelijke bevoegdheid toewijzen aan gebruikers voor toegang tot hun apps en hun taken uitvoeren. U kunt niet wilt dat de globale beheerdersrol toewijzen aan de eigenaar van elke toepassing, maar een afweging is dat u afdwingen dat de verantwoordelijkheden van de toepassing management op de bestaande globale beheerders. Er zijn veel redenen voor een organisatie verplaatsen naar een meer gedecentraliseerd beheer. In dit artikel kunt u van plan bent voor overdracht in uw organisatie.

<!--What about reporting? Who has which role and how do I audit?-->


## <a name="centralized-versus-delegated-permissions"></a>Gecentraliseerd ten opzichte van gedelegeerde machtigingen

Wanneer een organisatie groeit, kan het lastig zijn om bij te houden welke gebruikers specifieke-beheerdersrollen hebben. Een organisatie kan zijn vatbaar voor inbreuk op de beveiliging als een werknemer heeft administrator-rechten die ze al dan niet mogen. Over het algemeen het aantal administrators en de granulatie van hun machtigingen is afhankelijk van de grootte en complexiteit van de implementatie.

* Bij kleine of proof of concept-implementaties kan doen één of een aantal beheerders alles; Er is geen delegering. In dit geval elke beheerder te maken met de rol globale beheerder.
* Bij implementaties met meer machines, toepassingen en bureaubladen, is meer overdracht vereist. Verschillende beheerders mogelijk meer specifieke functionele verantwoordelijkheden (rollen). Bijvoorbeeld sommige mogelijk Privileged Identity-beheerders en anderen toepassingsbeheerders mogelijk. Een beheerder kan bovendien alleen bepaalde groepen objecten zoals apparaten beheren.
* Nog grotere implementaties mogelijk nog meer gedetailleerde machtigingen, plus mogelijk beheerders met niet-conventionele of hybride functies.

In de Azure AD-portal, kunt u [weergeven van alle leden van een rol](directory-manage-roles-portal.md), kunt u snel uw implementatie en gemachtigde machtigingen te controleren.

Als u geïnteresseerd bent in het delegeren van toegang tot Azure-resources en niet met beheerdersrechten toegang in Azure AD, Zie [een Role-based access control (RBAC) rol toewijzen](../../role-based-access-control/role-assignments-portal.md).

## <a name="delegation-planning"></a>Overdracht plannen

Hoewel de drempel voor het ontwikkelen van een overdrachtsmodel die past bij de unieke behoeften van uw organisatie, is de waarheid dat er zeer eenvoudige modellen die kunnen worden toegepast met kleine wijziging. Een analysemodel overdracht is een iteratieve ontwerpproces en we raden dat u als volgt te werk:

* Bepaalt de rollen die u nodig hebt
* App-beheer overdragen
* De mogelijkheid om toepassingen te registreren verlenen
* App-eigenaar delegeren
* Een beveiligingsplan ontwikkelen
* Tot stand brengen noodgevallen accounts
* Beveiligen van uw beheerdersrollen
* Bevoegde uitbreiding van bevoegdheden tijdelijke maken

## <a name="define-roles"></a>Rollen definiëren

Bepalen welke Active Directory-taken worden uitgevoerd door beheerders en de wijze waarop deze taken worden toegewezen aan rollen. Maken van de Active Directory-site is bijvoorbeeld een service-beheer-taak, terwijl de wijziging van het lidmaatschap van beveiligingsgroep in het algemeen valt onder gegevensbeheer van de. U kunt [beschrijvingen van gedetailleerde gebruikersrollen weer](directory-manage-roles-portal.md) in Azure portal.

Elke taak moet worden geëvalueerd voor de frequentie, prioriteit en problemen. Deze zijn belangrijke aspecten van de taakdefinitie omdat ze bepalen of een machtiging moet worden overgedragen. Taken die regelmatig worden uitgevoerd, beperkt risico en zijn essentieel om te voltooien, zijn uitstekende kandidaten voor overdracht. Aan de andere kant moeten taken die zelden worden uitgevoerd, maar grote gevolgen hebben in de hele organisatie en vereisen hoge vaardigheidsniveau zorgvuldig worden opgelost voordat het delegeren. In plaats daarvan kunt u [tijdelijk een account uitbreiden naar de vereiste functieservices](../active-directory-privileged-identity-management-configure.md) of opnieuw toewijzen van de taak.

## <a name="delegate-app-administration"></a>App-beheer overdragen

De verspreiding van apps binnen uw organisatie kan uw overdrachtsmodel toegenomen. Als de belasting voor toegangsbeheer voor toepassingen worden geplaatst op de globale beheerder, is het waarschijnlijk dat model, neemt de overhead toe naarmate tijd verstrijkt. Als de rol globale beheerder voor zaken zoals toepassingen voor bedrijven configureren, hebt u mensen verleend, kunt u ze nu offloaden naar de volgende rollen met minder bevoegdheden. In dat geval kunt u uw beveiligingspostuur verbeteren en vermindert de kans op fouten erg vervelend. De beheerder van de meeste bevoegdheden toepassingsrollen zijn:

* De **toepassingsbeheerder** rol, de mogelijkheid hebben voor het beheren van alle toepassingen in de map, inclusief registraties, instellingen voor eenmalige aanmelding, gebruiker en toewijzingen van groepen en licenties, Application Proxy-instellingen en toestemming. Het heeft niet de mogelijkheid voor het beheren van voorwaardelijke toegang verlenen.
* De **beheerder van de Cloudtoepassing** rol, waarmee alle mogelijkheden van de beheerder van de toepassing, maar deze geen toegang tot Application Proxy-instellingen verlenen heeft (omdat het heeft geen machtiging on-premises) worden verleend. ### gemachtigde app de eigenaar van machtigingen per-app

## <a name="delegate-app-registration"></a>App-registratie delegeren

Standaard kunnen alle gebruikers toepassingsregistraties maken. Als u verlenen selectief toepassingsregistraties maken wilt, hebt u om in te stellen **gebruikers kunnen toepassingen registreren** op Nee in gebruikersinstellingen, en vervolgens wijst u de rol van ontwikkelaar van toepassingen. Deze rol hebben de mogelijkheid om te toepassingsregistraties maken, alleen wanneer de **gebruikers kunnen toepassingen registreren** is uitgeschakeld. Ontwikkelaars van toepassingen kunnen ook toestemming geven voor zichzelf als de **gebruikers toestemming kunnen geven tot toepassingen die toegang tot bedrijfsgegevens namens hen** is ingesteld op Nee. Als u de ontwikkelaar van een toepassing maakt een nieuwe toepassing registreren, worden ze automatisch toegevoegd als de eigenaar van de eerste.

## <a name="delegate-app-ownership"></a>App-eigenaar delegeren

Voor overdracht nog nauwkeurigere app-toegang, kunt u eigenaar toewijzen aan afzonderlijke zakelijke toepassingen. Dit is een aanvulling op de bestaande ondersteuning voor het toewijzen van eigenaren van de registratie van toepassing. Eigenaar wordt toegewezen op basis van per-enterprise-toepassing in de blade van de enterprise-apps. Het voordeel is eigenaren kunnen alleen de zakelijke toepassingen die waarvan ze eigenaar beheren. Bijvoorbeeld, u kunt een eigenaar toewijzen voor de Salesforce-toepassing en die eigenaar de toegang tot en configuratie voor Salesforce en geen andere toepassingen kunt beheren. Een zakelijke toepassing kan veel eigenaars hebben en een gebruiker kan de eigenaar voor veel zakelijke toepassingen zijn. Er zijn twee app-eigenaar rollen:

* De **Enterprise Toepassingseigenaar** rol hebben de mogelijkheid voor het beheren van de ' zakelijke toepassingen maken die de gebruiker eigenaar is, met inbegrip van instellingen voor eenmalige aanmelding, gebruiker en groepstoewijzingen en het toevoegen van extra eigenaren. Deze verleent de mogelijkheid om Application Proxy-instellingen of voorwaardelijke toegang te beheren.
* De **registratie Toepassingseigenaar** rol hebben de mogelijkheid voor het beheren van de registratie voor de app die de gebruiker eigenaar is, met inbegrip van het manifest van de toepassing en het toevoegen van extra eigenaren.

## <a name="develop-a-security-plan"></a>Een beveiligingsplan ontwikkelen

Azure AD biedt een uitgebreide handleiding voor het plannen en uitvoeren van een beveiligingsplan op uw Azure AD-beheerdersrollen, [beveiligen van bevoegde toegang voor cloud- en hybride implementaties](directory-admin-roles-secure.md). 

## <a name="establish-emergency-accounts"></a>Tot stand brengen noodgevallen accounts

Als u wilt behouden toegang tot uw identity management-store als het probleem zich voordoen, voorbereiden voor toegang in noodgevallen accounts volgens [EMS-toegang met beheerdersrechten accounts maken](directory-emergency-access.md).

## <a name="secure-your-administrator-roles"></a>Beveiligen van uw beheerdersrollen

Aanvallers die beheer van bevoegde accounts krijgen kan grote schade aanrichten, dus deze accounts beschermen eerst met behulp van de [basislijn toegangsbeleid](https://cloudblogs.microsoft.com/enterprisemobility/2018/06/22/baseline-security-policy-for-azure-ad-admin-accounts-in-public-preview/) die standaard beschikbaar voor alle Azure AD-tenants is (in openbare preview). Het beleid dwingt meervoudige verificatie in Azure AD-accounts met bevoegdheden. De volgende Azure AD-rollen worden gedekt door het basisbeleid voor Azure AD:

* Globale beheerder
* SharePoint-beheerder
* Exchange-beheerder
* Voorwaardelijke-toegangsbeheerder
* Beveiligingsbeheerder

## <a name="elevate-privilege-temporarily"></a>Tijdelijk bevoegdheden verhogen

Niet alle gebruikers moeten voor de meeste dagelijkse activiteiten, global administrator-rechten. En niet alle gebruikers permanent moeten worden toegewezen aan de rol globale beheerder. Wanneer gebruikers nodig hebben om te fungeren als een globale beheerder, moeten ze de toewijzing van rollen in Azure AD activeren [Privileged Identity Management](../active-directory-privileged-identity-management-configure.md) op hun eigen account of een alternatieve Administrator-account.

## <a name="next-steps"></a>Volgende stappen

Zie voor een verwijzing naar de beschrijvingen van de Azure AD-rol, [beheerdersrollen toewijzen in Azure AD](directory-assign-admin-roles.md)