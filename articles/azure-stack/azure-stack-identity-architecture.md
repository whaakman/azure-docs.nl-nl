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
ms.openlocfilehash: 899e0fc0c1eb93d68c79c92c9cc042462ebc2fef
ms.sourcegitcommit: 8aab1aab0135fad24987a311b42a1c25a839e9f3
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/16/2018
---
# <a name="identity-architecture-for-azure-stack"></a>Architectuur van de identiteit voor Azure-Stack
Voordat u een id-provider te gebruiken met Azure-Stack kiest, moet u de belangrijke verschillen tussen de opties van Azure Active Directory (Azure AD) en Active Directory Federation Services (AD FS) begrijpen. 

## <a name="capabilities-and-limitations"></a>Mogelijkheden en beperkingen 
De id-provider die u kunt uw opties, waaronder ondersteuning voor multitenancy beperken. 

  

|Mogelijkheid tot of het scenario        |Azure AD  |AD FS  |
|------------------------------|----------|-------|
|Verbonden met internet     |Ja       |Optioneel|
|Ondersteuning voor multitenancy     |Ja       |Nee      |
|Marketplace-syndicatie       |Ja       |Ja. Vereist het gebruik van de [offline Marketplace Syndication](azure-stack-download-azure-marketplace-item.md#download-marketplace-items-in-a-disconnected-or-a-partially-connected-scenario-with-limited-internet-connectivity) hulpprogramma.|
|Ondersteuning voor Active Directory Authentication Library (ADAL) |Ja |Ja|
|Ondersteuning voor hulpprogramma's, zoals Azure CLI, Visual Studio en PowerShell  |Ja |Ja|
|Service-principals via de Azure portal maken     |Ja |Nee|
|Service-principals met certificaten maken      |Ja |Ja|
|Service-principals maken met geheimen (sleutels)    |Ja |Nee|
|Toepassingen kunnen gebruikmaken van de grafiek-service           |Ja |Nee|
|Toepassingen kunnen id-provider gebruiken voor aanmelding |Ja |Ja. Vereist dat toepassingen om te federeren met on-premises AD FS-exemplaren. |

## <a name="topologies"></a>Topologies
De volgende secties dialoog van de verschillende identiteit topologieën die u kunt gebruiken.

### <a name="azure-ad-single-tenant-topology"></a>Azure AD: single-tenant-topologie 
Wanneer u Azure-Stack installeren en gebruiken van Azure AD, Azure Stack gebruikt standaard een topologie met één tenant. 

De topologie van een één-tenant is handig wanneer:
- Alle gebruikers uitmaken deel van dezelfde tenant.
- Een serviceprovider als host fungeert voor een Azure-Stack-exemplaar voor een organisatie. 

![Azure Stack-één-tenant-topologie met Azure AD](media/azure-stack-identity-architecture/single-tenant.png)

Deze topologie biedt de volgende kenmerken:
- Azure Stack registreert alle toepassingen en services met dezelfde Azure AD-tenant directory. 
- Azure Stack verifieert alleen de gebruikers en toepassingen van die map, met inbegrip van tokens. 
- Identiteiten voor beheerders (cloudoperators) en tenant-gebruikers zijn in dezelfde directory-tenant. 
- Als u wilt dat een gebruiker van een andere map voor toegang tot deze Azure-Stack-omgeving, moet u [de gebruiker als gast uitnodigen](azure-stack-identity-overview.md#guest-users) naar de tenantmap. 

### <a name="azure-ad-multi-tenant-topology"></a>Azure AD: multitenant-topologie
Cloudoperators kunnen configureren voor Azure-Stack voor toegang tot toepassingen door tenants van een of meer organisaties. Gebruikers toegang tot toepassingen via de gebruikersportal. In deze configuratie wordt het beheerportal (gebruikt door de cloud-operator) vanuit een enkele map beperkt tot gebruikers. 

Een multitenant-topologie is handig wanneer:
- Een serviceprovider wil toestaan dat gebruikers van meerdere organisaties voor toegang tot Azure-Stack.

![Azure Stack multitenant-topologie met Azure AD](media/azure-stack-identity-architecture/multi-tenant.png)

Deze topologie biedt de volgende kenmerken:
- Toegang tot bronnen moet op basis van per organisatie worden. 
- Gebruikers van een organisatie moet kan geen toegang tot resources verlenen aan gebruikers die zich buiten de organisatie. 
- Identiteiten voor beheerders (cloudoperators) kunnen zich in een afzonderlijke directory-tenant van de identiteiten te beheren voor gebruikers. Deze scheiding biedt isolatie van de account op het niveau van de id-provider. 
 
### <a name="ad-fs"></a>AD FS  
De AD FS-topologie is vereist wanneer een van de volgende voorwaarden is voldaan:
- Azure Stack verbinding geen met internet.
- Azure Stack verbinding kan maken met internet, maar u kiest voor AD FS voor id-provider.
  
![Azure Stack-topologie met AD FS](media/azure-stack-identity-architecture/adfs.png)

Deze topologie biedt de volgende kenmerken:
- Ter ondersteuning van het gebruik van deze topologie in productie, moet u het exemplaar van de ingebouwde Azure-Stack AD FS integreren met een bestaand AD FS-exemplaar dat wordt ondersteund door Active Directory via een federatieve vertrouwensrelatie. 
- U kunt de service van de grafiek in Azure-Stack integreren met uw bestaande Active Directory-exemplaar. U kunt ook de Graph API op basis van een OData-service die API's die consistent met de Azure AD Graph API zijn ondersteunt. 

  Om te kunnen communiceren met uw exemplaar van Active Directory, vereist de Graph API gebruikersreferenties van uw Active Directory-exemplaar die alleen-lezen-machtigingen hebben. 
  - De ingebouwde exemplaar van AD FS is gebaseerd op Windows Server 2016. 
  - Uw AD FS en Active Directory-exemplaren moeten worden gebaseerd op Windows Server 2012 of later. 
  
  Tussen uw Active Directory-exemplaar en de ingebouwde exemplaar van AD FS interacties worden niet beperkt tot het OpenID Connect en ze kunnen wederzijds ondersteunde protocollen gebruiken. 
  - Gebruikersaccounts worden gemaakt en beheerd in uw lokale Active Directory-exemplaar.
  - Service-principals en registraties voor toepassingen worden beheerd in de ingebouwde Active Directory-exemplaar.



## <a name="next-steps"></a>Volgende stappen
- [Overzicht van identiteit](azure-stack-identity-overview.md)   
- [Datacenter-integratie - identiteit](azure-stack-integrate-identity.md)
