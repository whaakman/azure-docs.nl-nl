---
title: Architectuur van de identiteit voor Azure-Stack | Microsoft Docs
description: Meer informatie over de architectuur van de identiteit die kunt u met Azure-Stack.
services: azure-stack
documentationcenter: 
author: brenduns
manager: femila
editor: 
ms.assetid: 
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 2/28/2018
ms.author: brenduns
ms.reviewer: 
ms.openlocfilehash: 7f2ec78da38f3c97fde810fb8fc965cfbb6fda08
ms.sourcegitcommit: 782d5955e1bec50a17d9366a8e2bf583559dca9e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/02/2018
---
# <a name="identity-architecture-for-azure-stack"></a>Architectuur van de identiteit voor Azure-Stack
Voordat u een id-provider te gebruiken met Azure-Stack kiest, begrijpt u belangrijke verschillen tussen de opties van Azure Active Directory (Azure AD) en Active Directory Federated Services (AD FS). 

## <a name="capabilities-and-limitations"></a>Mogelijkheden en beperkingen 
De id-provider die u kunt uw opties, waaronder ondersteuning voor multitenancy beperken. 

  

|Mogelijkheid tot of het scenario        |Azure AD  |AD FS  |
|------------------------------|----------|-------|
|Verbonden met internet     |Ja       |Optioneel|
|Ondersteuning voor multitenancy     |Ja       |Nee      |
|Marketplace-syndicatie       |Ja       |Ja - vereist gebruik van de [offline Marketplace Syndication](azure-stack-download-azure-marketplace-item.md#download-marketplace-items-in-a-disconnected-or-a-partially-connected-scenario-with-limited-internet-connectivity) hulpprogramma.|
|Ondersteuning voor Active Directory Authentication Library (ADAL) |Ja |Ja|
|Ondersteuning voor hulpprogramma's zoals Azure-opdrachtregelinterface (CLI), Visual Studio (VS) en PowerShell  |Ja |Ja|
|Service-principals via de portal maken     |Ja |Nee|
|Service-principals met certificaten maken      |Ja |Ja|
|Service-principals maken met geheimen (sleutels)    |Ja |Nee|
|Toepassingen kunnen gebruikmaken van de grafiek-service           |Ja |Nee|
|Toepassingen kunnen id-provider gebruiken voor aanmelding |Ja |Ja - vereist toepassingen wilt federeren met uw on-premises AD FS. |

## <a name="topologies"></a>Topologies
De volgende secties bevatten informatie over identiteit topologieën die u kunt gebruiken.

### <a name="azure-ad--single-tenant"></a>Azure AD-één-tenant 
Wanneer u Azure-Stack installeren en gebruiken van Azure AD, Azure Stack gebruikt standaard een topologie met één tenant. 

De topologie van een één-tenant is handig wanneer:
- Alle gebruikers uitmaken deel van dezelfde tenant.
- Een serviceprovider als host fungeert voor een Azure-Stack-exemplaar voor een organisatie.  

![Azure Stack-topologie met behulp van een topologie met één tenant met Azure AD](media/azure-stack-identity-architecture/single-tenant.png)

Met deze topologie:
- Azure Stack registreert alle toepassingen en services met dezelfde Azure AD-tenant directory. 
- Azure Stack verifieert alleen de gebruikers en toepassingen van die map, met inbegrip van Tokens. 
- Identiteiten voor beheerders (cloudoperators) en tenant-gebruikers zijn in dezelfde directory-tenant. 
- Als u wilt dat een gebruiker van een andere map voor toegang tot deze Azure-Stack-omgeving, moet u [de gebruiker als gast uitnodigen](azure-stack-identity-overview.md#guest-users) naar de tenantmap.  

### <a name="azure-ad--multi-tenant"></a>Azure AD: meerdere tenants
Cloudoperators kunnen configureren voor Azure-Stack voor toegang tot toepassingen door tenants van een of meer organisaties. Gebruikers toegang tot toepassingen via de gebruikersportal. In deze configuratie wordt het beheerportal (gebruikt door de cloud-operator) vanuit een enkele map beperkt tot gebruikers. 

Een multitenant-topologie is handig wanneer:
- Een serviceprovider wil toestaan dat gebruikers van meerdere organisaties voor toegang tot Azure-Stack.

![Azure Stack-topologie met behulp van een multitenant-topologie met Azure AD](media/azure-stack-identity-architecture/multi-tenant.png)

Met deze topologie:
- Toegang tot bronnen moet op basis van per organisatie worden. 
- Er mag geen gebruikers van een organisatie kunnen toegang tot resources verlenen aan gebruikers die zich buiten de organisatie.  
- Identiteiten voor beheerders (cloudoperators) kunnen zich in een afzonderlijke map tenant dan de identiteiten te beheren voor gebruikers. Deze scheiding biedt isolatie van de account op het niveau van de id-provider. 
 
### <a name="ad-fs"></a>AD FS  
De AD FS-topologie is vereist wanneer een van de volgende voorwaarden is voldaan:
- Azure Stack verbinding geen met Internet.
- Azure Stack verbinding kan maken met Internet, maar u kiest voor AD FS voor id-provider.
  
![Azure Stack-topologie met AD FS](media/azure-stack-identity-architecture/adfs.png)

Met deze topologie:
- Ondersteuning bieden voor gebruik in productie, moet u het exemplaar van de ingebouwde Azure-Stack AD FS integreren met een bestaand exemplaar van AD FS ondersteund door Active Directory (AD), via een federatieve vertrouwensrelatie. 
- U kunt de service van de grafiek in Azure-Stack integreren met uw bestaande AD.  U kunt ook de OData Graph API-service die API's die consistent met de Azure AD Graph API zijn ondersteunt.  

  Om te kunnen communiceren met uw AD, moet de Graph API gebruikersreferenties van uw AD die alleen-lezen-machtiging hebben uw AD. 
  - De ingebouwde AD FS is gebaseerd op de Server 2016. 
  - Uw AD FS en AD moet worden gebaseerd op Server 2012 of later.  
  
  Tussen uw AD en de ingebouwde AD FS interacties worden niet beperkt tot het OpenID Connect en elk protocol voor wederzijds ondersteunde kunnen gebruiken.  
  - Gebruikersaccounts worden gemaakt en beheerd in uw on-premises AD.
  - Service-principals en registraties voor toepassingen worden beheerd in de ingebouwde AD.



## <a name="next-steps"></a>Volgende stappen
- [Overzicht van identiteit](azure-stack-identity-overview.md)   
- [Datacenter-integratie - identiteit](azure-stack-integrate-identity.md)