---
title: Architectuur voor cloudidentiteit voor Azure Stack | Microsoft Docs
description: Meer informatie over de architectuur voor cloudidentiteit die kunt u met Azure Stack.
services: azure-stack
documentationcenter: ''
author: brenduns
manager: femila
editor: ''
ms.assetid: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 08/01/2018
ms.author: brenduns
ms.reviewer: ''
ms.openlocfilehash: bf69c71a8b361e4a147263bc60324573c710818f
ms.sourcegitcommit: 96f498de91984321614f09d796ca88887c4bd2fb
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/02/2018
ms.locfileid: "39412693"
---
# <a name="identity-architecture-for-azure-stack"></a>Architectuur voor cloudidentiteit voor Azure Stack
Voordat u ervoor een id-provider kiest te gebruiken met Azure Stack, begrijpt u de belangrijke verschillen tussen de opties van Azure Active Directory (Azure AD) en Active Directory Federation Services (AD FS). 

## <a name="capabilities-and-limitations"></a>Mogelijkheden en beperkingen 
De id-provider die u kunt uw opties, waaronder ondersteuning voor multitenancy beperken. 

  

|Capaciteit of scenario        |Azure AD  |AD FS  |
|------------------------------|----------|-------|
|Verbonden met internet     |Ja       |Optioneel|
|Ondersteuning voor multitenancy     |Ja       |Nee      |
|Items in de Marketplace bieden |Ja       |Ja. Vereist het gebruik van de [offline Marketplace Syndication](azure-stack-download-azure-marketplace-item.md#disconnected-or-a-partially-connected-scenario) hulpprogramma.|
|Ondersteuning voor Active Directory Authentication Library (ADAL) |Ja |Ja|
|Ondersteuning voor hulpprogramma's, zoals Azure CLI, Visual Studio en PowerShell  |Ja |Ja|
|Service-principals via Azure portal maken     |Ja |Nee|
|Service-principals met certificaten maken      |Ja |Ja|
|Service-principals met geheimen (sleutels) maken    |Ja |Nee|
|Toepassingen kunnen gebruikmaken van de Graph-service           |Ja |Nee|
|Toepassingen kunnen id-provider gebruiken voor aanmelden |Ja |Ja. Vereist dat toepassingen om te federeren met on-premises AD FS-exemplaren. |

## <a name="topologies"></a>Topologieën
De volgende secties dialoog de verschillende identiteit topologieën die u kunt gebruiken.

### <a name="azure-ad-single-tenant-topology"></a>Azure AD: één tenant-topologie 
Wanneer u Azure Stack installeren en gebruiken van Azure AD, Azure Stack gebruikt standaard een topologie met één tenant. 

Een topologie met één tenant is handig wanneer:
- Alle gebruikers maken deel uit van dezelfde tenant.
- Een serviceprovider als host fungeert voor een Azure Stack-exemplaar voor een organisatie. 

![Azure Stack één tenant-topologie met Azure AD](media/azure-stack-identity-architecture/single-tenant.png)

Deze topologie biedt de volgende kenmerken:
- Azure Stack registreert alle toepassingen en services met dezelfde Azure AD directory-tenant. 
- Azure Stack verifieert alleen de gebruikers en toepassingen uit die map, met inbegrip van tokens. 
- Identiteiten voor beheerders (cloudoperators) en tenantgebruikers zich in dezelfde directory-tenant. 
- Als u wilt dat een gebruiker van een andere directory voor toegang tot deze Azure Stack-omgeving, moet u [de gebruiker als gast uitnodigen](azure-stack-identity-overview.md#guest-users) aan de tenant-directory. 

### <a name="azure-ad-multi-tenant-topology"></a>Azure AD: topologie voor meerdere tenants
Cloudoperators kunnen configureren voor Azure Stack voor toegang tot toepassingen door tenants van een of meer organisaties. Gebruikers toegang tot toepassingen door de gebruikersportal. In deze configuratie is de beheerportal (gebruikt door de cloud-operator) beperkt tot gebruikers van één map. 

Een topologie met meerdere tenants is handig wanneer:
- Een serviceprovider wil toestaan dat gebruikers van meerdere organisaties toegang tot Azure Stack.

![Azure Stack-multitenant-topologie met Azure AD](media/azure-stack-identity-architecture/multi-tenant.png)

Deze topologie biedt de volgende kenmerken:
- Toegang tot resources moet zich op basis van per organisatie. 
- Gebruikers van een organisatie moet kan geen toegang tot resources verlenen aan gebruikers die zich buiten de organisatie. 
- Identiteiten voor beheerders (cloudoperators) kunnen zich in een afzonderlijke directory-tenant van de identiteit voor gebruikers. Dankzij deze scheiding biedt isolatie van het account op het niveau van de id-provider. 
 
### <a name="ad-fs"></a>AD FS  
De AD FS-topologie is vereist wanneer een van de volgende voorwaarden wordt voldaan:
- Azure Stack verbinding geen met internet.
- Azure Stack kunt verbinding maken met internet, maar u wilt gebruiken van AD FS voor uw id-provider.
  
![Azure Stack-topologie met behulp van AD FS](media/azure-stack-identity-architecture/adfs.png)

Deze topologie biedt de volgende kenmerken:
- Ter ondersteuning van het gebruik van deze topologie in de productieomgeving, moet u de ingebouwde Azure Stack AD FS-exemplaar met een bestaande AD FS-exemplaar dat wordt ondersteund door Active Directory, via een federatieve vertrouwensrelatie te integreren. 
- U kunt de Graph-service in Azure Stack integreren met uw bestaande Active Directory-exemplaar. U kunt ook de Graph-API op basis van OData-service die ondersteuning biedt voor API's die consistent met de Azure AD Graph API zijn gebruiken. 

  Om te communiceren met uw Active Directory-exemplaar, vereist de Graph API gebruikersreferenties van uw Active Directory-exemplaar dat alleen-lezen-machtigingen hebben. 
  - Het ingebouwde AD FS-exemplaar is gebaseerd op Windows Server 2016. 
  - De AD FS en Active Directory-instanties moeten zijn gebaseerd op Windows Server 2012 of hoger. 
  
  Interacties worden niet beperkt tot het OpenID Connect tussen uw Active Directory-exemplaar en de ingebouwde AD FS-exemplaar, en ze alle sluiten elkaar wederzijds ondersteunde protocollen kunnen gebruiken. 
  - Gebruikersaccounts worden gemaakt en beheerd in uw on-premises Active Directory-exemplaar.
  - Service-principals en registraties voor toepassingen worden beheerd in de ingebouwde Active Directory-exemplaar.



## <a name="next-steps"></a>Volgende stappen
- [Overzicht van identiteit](azure-stack-identity-overview.md)   
- [Datacenter-integratie - identiteit](azure-stack-integrate-identity.md)
