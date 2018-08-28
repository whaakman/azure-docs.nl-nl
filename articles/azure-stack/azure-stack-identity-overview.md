---
title: Overzicht van de identiteit voor Azure Stack | Microsoft Docs
description: Meer informatie over de identiteitssystemen die kunt u met Azure Stack.
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
ms.date: 2/22/2018
ms.author: brenduns
ms.reviewer: ''
ms.openlocfilehash: 9a5390b51b3b901b159f99e757ca4db1aaf8258e
ms.sourcegitcommit: 161d268ae63c7ace3082fc4fad732af61c55c949
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/27/2018
ms.locfileid: "43050964"
---
# <a name="overview-of-identity-for-azure-stack"></a>Overzicht van de identiteit voor Azure Stack

Azure Stack is vereist voor Azure Active Directory (Azure AD) of Active Directory Federation Services (AD FS), ondersteund door Active Directory als id-provider. De keuze van een provider is een eenmalige beslissing die u neemt bij het implementeren van Azure Stack. De concepten en autorisatie-informatie in dit artikel kunt u kiezen tussen het id-providers. 

Uw eigen keuze aan Azure AD of AD FS kan worden bepaald door de modus waarin u Azure Stack implementeren: 
- Wanneer u deze in een verbonden modus implementeert, kunt u een Azure AD of AD FS. 
- Wanneer u deze in een niet-verbonden modus, zonder een verbinding met internet implementeert, wordt alleen AD FS wordt ondersteund.

Zie voor meer informatie over uw opties, die afhankelijk van uw Azure Stack-omgeving zijn, de volgende artikelen:
- Azure Stack deployment kit: [identiteit overwegingen met betrekking tot](azure-stack-datacenter-integration.md#identity-considerations).
- Azure Stack-geïntegreerde systemen: [implementatie planningsbeslissingen voor Azure Stack-geïntegreerde systemen](azure-stack-deployment-decisions.md).

 
## <a name="common-concepts-for-identity"></a>Algemene concepten voor identiteit
De volgende secties worden besproken algemene concepten over id-providers en het gebruik ervan in Azure Stack.

![Terminologie voor id-providers](media/azure-stack-identity-overview/terminology.png)

### <a name="directory-tenants-and-organizations"></a>Directory-tenants en organisaties
Een directory is een container met informatie over *gebruikers*, *toepassingen*, *groepen*, en *service-principals*.
 
Een directory-tenant is een *organisatie*, zoals Microsoft of uw eigen bedrijf. 
- Azure AD biedt ondersteuning voor meerdere tenants en meerdere organisaties, elk in een eigen directory kan ondersteunen. Als u Azure AD gebruiken en meerdere tenants hebt, kunt u toepassingen en gebruikers van één tenant toegang aan andere tenants van die directory verlenen.
- AD FS ondersteunt slechts één tenant en daarom slechts één organisatie. 

### <a name="users-and-groups"></a>Gebruikers en groepen
Gebruikersaccounts (identiteiten) zijn standaard accounts die personen verifiëren met behulp van een gebruikers-ID en wachtwoord. Groepen kunnen gebruikers of andere groepen bevatten. 

Het maken en beheren van gebruikers en groepen, is afhankelijk van de identiteitsoplossing die u gebruikt. 

In Azure Stack, gebruikersaccounts: 
- Worden gemaakt in de *username@domain* indeling. Hoewel AD FS gebruikersaccounts aan een Active Directory-exemplaar toegewezen, AD FS ondersteunt niet het gebruik van de  *\<domein >\<alias >* indeling. 
- Kan worden ingesteld voor het gebruik van meervoudige verificatie. 
- Zijn beperkt tot de map waar ze zich registreren, dit de map van hun organisatie is.
- Kan worden geïmporteerd vanuit uw on-premises adreslijsten. Zie voor meer informatie, [uw on-premises directory's integreren met Azure Active Directory](/azure/active-directory/connect/active-directory-aadconnect). 

Wanneer u zich bij de tenantportal van uw organisatie aanmelden, gebruikt u de *https://portal.local.azurestack.external* URL. 

### <a name="guest-users"></a>Gastgebruikers
Gastgebruikers zijn gebruikersaccounts van andere directorytenants die zijn verleend aan de toegang tot resources in uw directory. Ter ondersteuning van gastgebruikers, gebruikt u Azure AD en ondersteuning voor multitenancy inschakelen. Als u ondersteuning is ingeschakeld, kunt u gastgebruikers ook kunnen toegang krijgen tot bronnen in uw directory-tenant, die zorgt voor hun samenwerking met externe organisaties uitnodigen. 

Als u wilt uitnodigen van gastgebruikers, cloudoperators en gebruikers kunnen gebruikmaken van [Azure AD B2B-samenwerking](/azure/active-directory/active-directory-b2b-what-is-azure-ad-b2b). Uitgenodigde gebruikers krijgen toegang tot documenten, bronnen en toepassingen van uw directory, en u controle over uw eigen resources en de gegevens beheert. 

Als een gastgebruiker, kunt u zich aanmelden bij een andere organisatie directory-tenant. Om dit te doen, u de mapnaam van die organisatie toevoegen aan de portal URL. Bijvoorbeeld, als u deel uitmaken van de Contoso-organisatie en wilt aanmelden bij de Fabrikam-map, u https://portal.local.azurestack.external/fabrikam.onmicrosoft.com.

### <a name="applications"></a>Toepassingen
U kunt naar Azure AD of AD FS-toepassingen registreren en vervolgens de toepassingen voor gebruikers aanbieden in uw organisatie. 

Toepassingen zijn onder andere:
- **Webtoepassing**: voorbeelden zijn onder meer de Azure portal en Azure Resource Manager. Deze ondersteuning voor Web-API-aanroepen. 
- **Systeemeigen client**: voorbeelden zijn onder meer Azure PowerShell, Visual Studio en Azure CLI.

Toepassingen kunnen ondersteuning bieden twee typen tenants: 
- **Één tenant**: biedt ondersteuning voor gebruikers en services alleen in dezelfde map waar de toepassing is geregistreerd. 

  > [!NOTE]    
  > Omdat AD FS slechts één map ondersteunt, worden toepassingen die u in een AD FS-topologie maakt standaard één tenant-toepassingen.

- **Multitenant**: ondersteunt het gebruik van door gebruikers en services van zowel de directory waar de toepassing is geregistreerd als een aanvullende tenant mappen. In toepassingen met meerdere tenants zich gebruikers van een andere tenant-directory (een andere Azure AD-tenant) kunnen aanmelden bij uw toepassing. 

  Zie voor meer informatie over multitenancy [multitenancy inschakelen](azure-stack-enable-multitenancy.md). 

  Zie voor meer informatie over het ontwikkelen van een app met meerdere tenants [apps met meerdere tenants](/azure/active-directory/develop/active-directory-devhowto-multi-tenant-overview).

Als u een toepassing registreert, kunt u twee objecten maken:

- **Toepassingsobject**: de algemene weergave van de toepassing voor alle tenants. Deze relatie is een-op-een met de softwaretoepassing en bestaat alleen in de map waar de toepassing voor het eerst is geregistreerd.

- **Service-principal-object**: een referentie die wordt gemaakt voor een toepassing in de map waar de toepassing voor het eerst is geregistreerd. Een service-principal wordt ook gemaakt in de directory van elke aanvullende tenant waar de toepassing wordt gebruikt. Deze relatie kan een-op-veel met de toepassing zijn. 

Zie voor meer informatie over de toepassing en service-principalobjecten [toepassing en service-principalobjecten in Azure Active Directory](/azure/active-directory/develop/active-directory-application-objects). 

### <a name="service-principals"></a>Service-principals 
Een service-principal is een set *referenties* voor een toepassing of service die toegang geven tot resources in Azure Stack. Het gebruik van een service-principal scheidt de machtigingen van de toepassing van de machtigingen van de gebruiker van de toepassing.

Een service-principal is gemaakt in elke tenant waar de toepassing wordt gebruikt. De service-principal maakt een identiteit voor het aanmelden en toegang tot resources (zoals gebruikers) die worden beveiligd door deze tenant. 

- Een toepassing met één tenant heeft slechts één service-principal in de map waar het eerst wordt gemaakt. Deze service-principal is gemaakt en hiermee akkoord gaat moet worden gebruikt tijdens de registratie van de toepassing. 
- Een webtoepassing met meerdere tenants of API is een service-principal die in elke tenant wordt gemaakt wanneer een gebruiker van deze tenant hiermee akkoord gaat met het gebruik van de toepassing. 

Referenties voor service-principals kunnen worden ofwel een sleutel die wordt gegenereerd via de Azure-portal of een certificaat. Het gebruik van een certificaat is geschikt voor automation, omdat er certificaten worden beschouwd als veiliger dan sleutels. 


> [!NOTE]    
> Wanneer u AD FS met Azure Stack, kunt alleen de beheerder service-principals maken. Met AD FS, service-principals zijn certificaten vereist en zijn gemaakt via het eindpunt van de bevoegde (PEP). Zie voor meer informatie, [toepassingen toegang verlenen tot Azure Stack](azure-stack-create-service-principals.md).

Zie voor meer informatie over service-principals voor Azure Stack, [service-principals maken](azure-stack-create-service-principals.md).


### <a name="services"></a>Services
In Azure Stack-services die met de id-provider communiceren worden geregistreerd als toepassingen met de id-provider. Registratie kunt, zoals toepassingen, een service voor verificatie met de id-systeem. 

Alle Azure-services gebruiken [OpenID Connect](/azure/active-directory/develop/active-directory-protocols-openid-connect-code) protocollen en [JSON-Webtokens](/azure/active-directory/develop/active-directory-token-and-claims) tot stand brengen van hun identiteit. Omdat Azure AD en AD FS protocollen consistent gebruiken, kunt u [Azure Active Directory Authentication Library](/azure/active-directory/develop/active-directory-authentication-libraries) (ADAL) voor verificatie van on-premises of in Azure (in een verbonden scenario). U kunt ook hulpprogramma's, zoals Azure PowerShell en Azure CLI voor cross-cloud en on-premises Resourcemanagement met ADAL gebruiken. 


### <a name="identities-and-your-identity-system"></a>Identiteiten en uw identiteitssysteem 
Identiteiten voor Azure Stack zijn gebruikersaccounts, groepen en service-principals. 

Wanneer u Azure Stack installeert, verschillende ingebouwde toepassingen en services automatisch wordt geregistreerd bij uw id-provider in de directory-tenant. Sommige services dat registreren worden voor beheer gebruikt. Andere services zijn beschikbaar voor gebruikers. De standaard-rapporten geven core services identiteiten die communiceren kunnen met elkaar en met identiteiten die u later toevoegen.

Als u Azure AD met meerdere tenants hebt ingesteld, worden sommige toepassingen doorgegeven aan de nieuwe mappen. 

## <a name="authentication-and-authorization"></a>Verificatie en autorisatie
 

### <a name="authentication-by-applications-and-users"></a>Verificatie door toepassingen en gebruikers
  
![Identiteit tussen de lagen van Azure Stack](media/azure-stack-identity-overview/identity-layers.png)

Voor toepassingen en gebruikers, wordt de architectuur van Azure Stack door vier lagen beschreven. Interactie tussen elk van deze lagen kunnen verschillende soorten verificatie gebruiken.


|Laag    |Verificatie tussen lagen  |
|---------|---------|
|Hulpprogramma's en -clients, zoals de beheerportal     | Voor het openen of wijzigen van een resource in Azure Stack, hulpprogramma's en -clients gebruiken een [JSON Web Token](/azure/active-directory/develop/active-directory-token-and-claims) voor een oproep naar Azure Resource Manager. <br>Azure Resource Manager JSON Web Token valideert en geeft weer dat de *claims* in het gepubliceerde token om een schatting van het niveau van de autorisatie te die gebruiker of service-principal heeft in Azure Stack. |
|Azure Resource Manager en de bijbehorende services core     |Azure Resource Manager communiceert met de resourceproviders om over te dragen van communicatie van gebruikers. <br> Gebruik overgedragen *direct imperatieve* aanroepen of *declaratieve* aanroepen [Azure Resource Manager-sjablonen](/azure/azure-stack/user/azure-stack-arm-templates).|
|Resourceproviders     |Aanroepen die worden doorgegeven aan de resourceproviders worden beveiligd met verificatie op basis van certificaten. <br>Vervolgens blijven in de communicatie via een API met Azure Resource Manager en de resourceprovider. Voor elke aanroep die worden ontvangen van Azure Resource Manager, valideert de resourceprovider de aanroep van dit certificaat.|
|Infrastructuur en bedrijfslogica     |Resourceproviders communiceren met bedrijfslogica en infrastructuur met behulp van een verificatiemodus van hun keuze. De standaard-resourceproviders die worden geleverd met Azure Stack Windows-verificatie gebruiken om deze communicatie te beveiligen.|

![Informatie die nodig is voor verificatie](media/azure-stack-identity-overview/authentication.png)


### <a name="authenticate-to-azure-resource-manager"></a>Naar Azure Resource Manager verifiëren
Als u wilt verifiëren met de id-provider en een JSON Web Token ontvangt, hebt u de volgende informatie: 
1.  **URL voor de identiteitssysteem (Authority)**: de URL waarmee uw id-provider kan worden bereikt. Bijvoorbeeld *https://login.windows.net*. 
2.  **URI van de App-ID voor Azure Resource Manager**: de unieke id voor Azure Resource Manager die is geregistreerd bij uw id-provider. Het is ook uniek is voor elke Azure Stack-installatie.
3.  **Referenties**: de referentie op die u gebruikt voor verificatie met de id-provider. 
4.  **URL voor Azure Resource Manager**: de URL is de locatie van de Azure Resource Manager-service. Bijvoorbeeld, *https://management.azure.com* of *https://management.local.azurestack.external*.

Wanneer een principal (een client, een toepassing of een gebruiker) een verificatieaanvraag voor toegang tot een resource maakt, moet de aanvraag zijn onder andere:
- De principal-referenties.
- De app-ID-URI van de resource die de principal toegang wil tot. 

De referenties worden gevalideerd door de id-provider. De id-provider valideert ook dat de app-ID-URI is voor een geregistreerde toepassing en dat de principal heeft de juiste machtigingen om een token voor die bron te ontvangen. Als de aanvraag geldig is, wordt een JSON Web Token verleend. 

Het token moet vervolgens doorgeven in de header van een aanvraag naar Azure Resource Manager. Azure Resource Manager biedt de volgende in een specifieke volgorde:
- Valideert de *verlener* (iss) claim om te bevestigen dat het token afkomstig van de juiste id-provider is. 
- Valideert de *doelgroep* claim (aud) om te bevestigen dat het token naar Azure Resource Manager is uitgegeven. 
- Valideert de JSON Web Token is ondertekend met een certificaat dat wordt geconfigureerd via OpenID naar Azure Resource Manager is bekend. 
- Controleer de *uitgegeven op* (iat) en *verlopen* (exp) claims om te bevestigen dat het token actief is en kunnen worden geaccepteerd. 

Als alle validaties voltooid zijn, Azure Resource Manager gebruikt de *bezwaar* (oid) en de *groepen* om een lijst met resources die toegang hebben tot de principal-claims. 

![Diagram van het token exchange-protocol](media/azure-stack-identity-overview/token-exchange.png)


### <a name="use-role-based-access-control"></a>Rollen gebaseerd toegangsbeheer gebruiken  
Rollen gebaseerd toegangsbeheer (RBAC) in Azure Stack is consistent met de implementatie in Microsoft Azure. U kunt toegang tot resources beheren met de juiste RBAC-rol toewijzen aan gebruikers, groepen en toepassingen. Zie de volgende artikelen voor meer informatie over het gebruik van RBAC met Azure Stack:
- [Aan de slag met toegangsbeheer op basis van rollen in Azure portal](/azure/role-based-access-control/overview).
- [Toegangsbeheer op basis van rollen gebruiken voor het beheren van toegang tot de resources van uw Azure-abonnement](/azure/role-based-access-control/role-assignments-portal).
- [Aangepaste rollen maken voor op rollen gebaseerd toegangsbeheer](/azure/role-based-access-control/custom-roles).
- [Toegangsbeheer op basis van rollen beheren](azure-stack-manage-permissions.md) in Azure Stack.


### <a name="authenticate-with-azure-powershell"></a>Verifiëren met Azure PowerShell  
Meer informatie over het gebruik van Azure PowerShell om te verifiëren met Azure Stack kunnen u vinden op [PowerShell-omgeving van de Azure Stack-gebruiker configureren](azure-stack-powershell-configure-user.md).

### <a name="authenticate-with-azure-cli"></a>Verifiëren met Azure CLI
Zie voor meer informatie over het gebruik van Azure PowerShell om te verifiëren met Azure Stack [installeren en configureren van Azure CLI voor gebruik met Azure Stack](/azure/azure-stack/user/azure-stack-connect-cli).

## <a name="next-steps"></a>Volgende stappen
- [Architectuur voor cloudidentiteit](azure-stack-identity-architecture.md)   
- [Datacenter-integratie - identiteit](azure-stack-integrate-identity.md)




