---
title: Overzicht van de identiteit voor Azure-Stack | Microsoft Docs
description: Meer informatie over de identiteitssystemen die kunt u met Azure-Stack.
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
ms.openlocfilehash: 607c7938a789b3504a425057645b291bd4c8235b
ms.sourcegitcommit: 9cdd83256b82e664bd36991d78f87ea1e56827cd
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/16/2018
ms.locfileid: "31399028"
---
# <a name="overview-of-identity-for-azure-stack"></a>Overzicht van de identiteit voor Azure-Stack

Azure-Stack is vereist voor Azure Active Directory (Azure AD) of Active Directory Federation Services (AD FS), die door Active Directory als een id-provider wordt ondersteund. De keuze van een provider is een eenmalige beslissing die u aanbrengt bij het implementeren van Azure-Stack. De concepten en autorisatie-informatie in dit artikel kunt u kiezen tussen het id-providers. 

Uw keuze van Azure AD of AD FS kan worden bepaald door de modus waarin u Azure-Stack implementeert: 
- Wanneer u deze in een modus voor verbonden implementeert, kunt u ofwel Azure AD of AD FS. 
- Wanneer u deze in een modus zonder verbinding zonder verbinding met internet implementeren, wordt alleen voor AD FS ondersteund.

Zie de volgende artikelen voor meer informatie over uw opties, afhankelijk van uw Azure-Stack-omgeving:
- Azure Stack deployment kit: [identiteit overwegingen](azure-stack-datacenter-integration.md#identity-considerations).
- Azure Stack geïntegreerd systemen: [implementatie planningsbeslissingen voor Azure-Stack geïntegreerd systemen](azure-stack-deployment-decisions.md).

 
## <a name="common-concepts-for-identity"></a>Algemene concepten voor identiteit
De volgende secties worden besproken algemene concepten over het id-providers en het gebruik ervan in Azure-Stack.

![Terminologie voor id-providers](media/azure-stack-identity-overview/terminology.png)

### <a name="directory-tenants-and-organizations"></a>Directory-tenants en organisaties
Een directory is een container met informatie over *gebruikers*, *toepassingen*, *groepen*, en *service-principals*.
 
Een directory-tenant is een *organisatie*, zoals Microsoft of uw eigen bedrijf. 
- Azure AD biedt ondersteuning voor meerdere tenants en meerdere organisaties, elk in een eigen map kan ondersteunen. Als u Azure AD en meerdere tenants hebt, kunt u toepassingen en andere tenants van dezelfde directory gebruikers uit één tenant toegang verlenen.
- AD FS ondersteunt slechts één tenant en daarom wordt slechts één organisatie. 

### <a name="users-and-groups"></a>Gebruikers en groepen
Gebruikersaccounts (identiteiten) zijn standaard accounts die personen verifiëren met behulp van een gebruikersnaam en wachtwoord. Groepen kunnen bestaan gebruikers- of andere groepen. 

Hoe u maken en beheren van gebruikers en groepen, is afhankelijk van de identiteitsoplossing die u gebruikt. 

In Azure-Stack gebruikersaccounts: 
- Worden gemaakt in de *username@domain* indeling. Hoewel de AD FS toegewezen gebruikersaccounts aan een Active Directory-exemplaar, AD FS ondersteunt niet het gebruik van de  *\<domain >\<alias >* indeling. 
- Kan worden ingesteld voor meervoudige verificatie gebruiken. 
- Zijn beperkt tot de map waar ze wordt eerst registreren, die de adreslijst van hun organisatie.
- Kan worden geïmporteerd vanuit uw on-premises adreslijsten. Zie voor meer informatie [uw on-premises adreslijsten integreren met Azure Active Directory](/azure/active-directory/connect/active-directory-aadconnect). 

Wanneer u zich bij de tenantportal van uw organisatie aanmelden, gebruikt u de *https://portal.local.azurestack.external* URL. 

### <a name="guest-users"></a>Gastgebruikers
Gastgebruikers zijn gebruikersaccounts van andere directory-tenants die toegang tot bronnen in uw directory zijn verleend. Ter ondersteuning van gastgebruikers, Azure AD gebruikt en ondersteuning voor multitenancy inschakelen. Wanneer ondersteuning is ingeschakeld, kunt u gastgebruikers toegang krijgen tot bronnen in uw directory-tenant, die de samenwerking met externe organisaties zorgt uitnodigen. 

Om uit te nodigen gastgebruikers, cloudoperators en gebruikers kunnen gebruikmaken van [Azure AD B2B-samenwerking](/azure/active-directory/active-directory-b2b-what-is-azure-ad-b2b). Uitgenodigde gebruikers toegang krijgen tot documenten, bronnen en toepassingen vanuit uw directory en controle over uw resources en de gegevens te behouden. 

Als een gastgebruiker kunt u zich aanmeldt bij een andere organisatie directory-tenant. Om dit te doen, u de mapnaam die organisatie toevoegen aan de portal URL. Bijvoorbeeld, als u deel uitmaken van de organisatie Contoso en aan te melden bij de directory Fabrikam, u https://portal.local.azurestack.external/fabrikam.onmicrosoft.com.

### <a name="applications"></a>Toepassingen
U kunt registreren met Azure AD of AD FS-toepassingen en bieden vervolgens de toepassingen voor gebruikers in uw organisatie. 

Toepassingen zijn onder andere:
- **Webtoepassing**: voorbeelden zijn onder meer Azure-portal en Azure Resource Manager. Deze ondersteuning voor Web-API-aanroepen. 
- **Native client**: voorbeelden zijn onder meer Azure PowerShell, Visual Studio en Azure CLI.

Toepassingen kunnen ondersteuning bieden twee soorten tenancymodus: 
- **Single-tenant**: biedt ondersteuning voor gebruikers en services alleen uit dezelfde map waarin de toepassing is geregistreerd. 

  > [!NOTE]    
  > Aangezien AD FS slechts één map ondersteunt, hebben toepassingen die u in een AD FS-topologie maakt door het ontwerp voor één tenant toepassingen.

- **Multitenant**: ondersteunt gebruik door gebruikers en services van de map waar de toepassing is geregistreerd en de aanvullende tenant mappen. Met multitenant-toepassingen aanmelden gebruikers van een andere tenant directory (een andere Azure AD-tenant) kunnen bij uw toepassing. 

  Zie voor meer informatie over multi-tenancymodus [inschakelen multi-tenancymodus](azure-stack-enable-multitenancy.md). 

  Zie voor meer informatie over het ontwikkelen van een app multitenant [multitenant apps](/azure/active-directory/develop/active-directory-devhowto-multi-tenant-overview).

Wanneer u een toepassing registreert, kunt u twee objecten maakt:

- **Object voor de toepassing**: de globale weergave van de toepassing op alle tenants. Deze relatie is een-op-een met de softwaretoepassing en bestaat alleen in de map waar de toepassing voor het eerst is geregistreerd.

- **Service-principal-object**: een referentie die wordt gemaakt van een toepassing in de map waar de toepassing voor het eerst is geregistreerd. Een service-principal wordt ook gemaakt in de map van elke aanvullende tenant waar de toepassing wordt gebruikt. Deze relatie kan een-op-veel met de softwaretoepassing zijn. 

Zie voor meer informatie over de toepassing en service-principals, [toepassing en service-principal objecten in Azure Active Directory](/azure/active-directory/develop/active-directory-application-objects). 

### <a name="service-principals"></a>Service-principals 
Een service-principal is een reeks *referenties* voor een toepassing of service die toegang tot resources in Azure-Stack verlenen. Het gebruik van een service-principal scheidt de toepassing worden machtigingen van de machtigingen van de gebruiker van de toepassing.

Een service-principal gemaakt in elke tenant waar de toepassing wordt gebruikt. De service-principal wordt een identiteit voor het aanmelden en toegang tot bronnen (zoals gebruikers) die worden beveiligd door deze tenant. 

- Een toepassing voor één tenant heeft slechts één service-principal in de map waar het eerst wordt gemaakt. Deze service-principal gemaakt en wordt hiermee akkoord gaat worden gebruikt tijdens de registratie van de toepassing. 
- Een multitenant-webtoepassing of API heeft een service-principal die in elke tenant wordt gemaakt wanneer een gebruiker van de tenant die hiermee akkoord gaat met het gebruik van de toepassing. 

Referenties voor service-principals mag ofwel een sleutel die wordt gegenereerd via de Azure portal of een certificaat. Het gebruik van een certificaat is geschikt voor automatisering omdat certificaten worden beschouwd als veiliger dan sleutels. 


> [!NOTE]    
> Wanneer u AD FS met Azure-Stack gebruikt, kunt alleen de beheerder de service-principals maken. Met AD FS, service-principals zijn certificaten vereist en worden gemaakt via de bevoorrechte (PEP)-eindpunt. Zie voor meer informatie [toepassingen toegang bieden tot Azure-Stack](azure-stack-create-service-principals.md).

Zie voor meer informatie over service-principals voor Azure-Stack, [maken van de service-principals](azure-stack-create-service-principals.md).


### <a name="services"></a>Services
Services in Azure-Stack die met de id-provider communiceren zijn als toepassingen met de id-provider geregistreerd. Net als bij toepassingen kan registratie van een service te verifiëren met het identiteitssysteem. 

Alle Azure-services gebruiken [OpenID Connect](/azure/active-directory/develop/active-directory-protocols-openid-connect-code) protocollen en [JSON Web Tokens](/azure/active-directory/develop/active-directory-token-and-claims) om hun identiteit stand te brengen. Omdat Azure AD en AD FS protocollen consistent gebruiken, kunt u [Azure Active Directory Authentication Library](/azure/active-directory/develop/active-directory-authentication-libraries) (ADAL) voor de verificatie van lokale of naar Azure (in een verbonden scenario). U kunt ook hulpprogramma's zoals Azure PowerShell en Azure CLI voor cross-cloud en on-premises Resourcemanagement met ADAL gebruiken. 


### <a name="identities-and-your-identity-system"></a>Identiteiten en uw identiteitssysteem 
Identiteiten voor een Azure-Stack bevatten gebruikersaccounts, groepen en service-principals. 

Wanneer u Azure-Stack installeert, verschillende ingebouwde toepassingen en services die automatisch wordt geregistreerd met de id-provider in de directory-tenant. Sommige services die geregistreerd zijn gebruikt voor beheer. Andere services zijn beschikbaar voor gebruikers. De standaard-registraties geven core services identiteiten die communiceren kunnen met elkaar en met identiteiten die u later toevoegen.

Als u Azure AD met multi-tenancymodus hebt ingesteld, worden sommige toepassingen doorgeven aan de nieuwe mappen. 

## <a name="authentication-and-authorization"></a>Verificatie en autorisatie
 

### <a name="authentication-by-applications-and-users"></a>Verificatie door toepassingen en gebruikers
  
![Identiteit tussen lagen van Azure-Stack](media/azure-stack-identity-overview/identity-layers.png)

Voor toepassingen en gebruikers, wordt de architectuur van Azure-Stack beschreven met vier lagen. Interactie tussen elk van deze lagen kunnen verschillende soorten verificatie gebruiken.


|Laag    |Verificatie tussen lagen  |
|---------|---------|
|Hulpprogramma's en -clients, zoals het beheerportal     | Toegang tot of het wijzigen van een resource in Azure-Stack tools en -clients gebruiken een [JSON Web Token](/azure/active-directory/develop/active-directory-token-and-claims) om te kiezen in Azure Resource Manager. <br>Azure Resource Manager JSON Web Token valideert en geeft weer dat de *claims* in het gepubliceerde token om in te schatten het machtigingsniveau die gebruiker of service-principal heeft in Azure-Stack. |
|Azure Resource Manager en de bijbehorende services core     |Azure Resource Manager communiceert met resourceproviders om over te brengen van communicatie van gebruikers. <br> Gebruik overdraagt *direct imperatieve* aanroepen of *declaratieve* aanroepen [Azure Resource Manager-sjablonen](/azure/azure-stack/user/azure-stack-arm-templates.md).|
|Resourceproviders     |Aanroepen die worden doorgegeven aan resourceproviders zijn beveiligd met verificatie op basis van certificaten. <br>Azure Resource Manager en de resourceprovider vervolgens blijven in de communicatie via een API. Voor elke oproep die wordt ontvangen van Azure Resource Manager, valideert de resourceprovider de aanroep van dit certificaat.|
|Infrastructuur en zakelijke logica     |Resourceproviders communiceren met bedrijfslogica en infrastructuur met behulp van een verificatiemodus van hun keuze. De standaard resourceproviders die worden geleverd met Azure-Stack Windows-verificatie gebruiken om deze communicatie te beveiligen.|

![Informatie die nodig is voor verificatie](media/azure-stack-identity-overview/authentication.png)


### <a name="authenticate-to-azure-resource-manager"></a>In Azure Resource Manager verifiëren
Als u wilt verifiëren met de id-provider en ontvangt een JSON Web Token, hebt u de volgende informatie: 
1.  **URL voor het identiteitssysteem (Authority)**: de URL waarmee de id-provider kan worden bereikt. Bijvoorbeeld: *https://login.windows.net*. 
2.  **App-ID-URI voor Azure Resource Manager**: de unieke id voor Azure Resource Manager dat is geregistreerd met de id-provider. Het is ook uniek is voor elke installatie van een Azure-Stack.
3.  **Referenties**: de referenties die u met de id-provider gebruiken om te verifiëren. 
4.  **URL voor Azure Resource Manager**: de URL is de locatie van de Azure Resource Manager-service. Bijvoorbeeld: *https://management.azure.com* of *https://management.local.azurestack.external*.

Wanneer een principal (een client, toepassing of gebruiker) een verificatieaanvraag maakt voor toegang tot een bron, wordt de aanvraag moet omvatten:
- De principal-referenties.
- De app ID URI van de resource die de principal toegang wil tot. 

De referenties zijn geverifieerd door de identiteitsprovider. De id-provider ook geverifieerd dat de app-ID-URI voor een geregistreerde toepassing en dat de principal de juiste machtigingen heeft voor een token voor die bron verkrijgen. Als de aanvraag geldig is, wordt een JSON Web Token verleend. 

Het token moet vervolgens worden doorgegeven in de header van een aanvraag in Azure Resource Manager. Azure Resource Manager biedt de volgende, in willekeurige volgorde:
- Valideert de *verlener* (iss) claim om te bevestigen dat het token van de juiste id-provider is. 
- Valideert de *doelgroep* claim (aud) om te bevestigen dat het token is uitgegeven in Azure Resource Manager. 
- Valideert de JSON Web Token is ondertekend met een certificaat dat wordt geconfigureerd via OpenID bekend in Azure Resource Manager. 
- Controleer de *uitgegeven op* (iat) en *verlopen* (exp) claims te bevestigen dat het token actief is en kunnen worden geaccepteerd. 

Als alle validaties voltooid zijn, Azure Resource Manager gebruikt de *bezwaar* (oid) en de *groepen* beweert te maken van een lijst met bronnen waartoe de principal toegang heeft. 

![Diagram van het token exchange-protocol](media/azure-stack-identity-overview/token-exchange.png)


### <a name="use-role-based-access-control"></a>Op rollen gebaseerde toegangsbeheer gebruiken  
Op rollen gebaseerde toegangsbeheer (RBAC) in Azure-Stack is consistent met de implementatie in Microsoft Azure. U kunt toegang tot bronnen beheren door de juiste RBAC-rol toewijzen aan gebruikers, groepen en toepassingen. Zie de volgende artikelen voor meer informatie over het gebruik van RBAC met Azure-Stack:
- [Aan de slag met toegangsbeheer op basis van rollen in Azure portal](/azure/role-based-access-control/overview).
- [Toegangsbeheer op basis van rollen gebruiken voor het beheren van toegang tot de resources van uw Azure-abonnement](/azure/role-based-access-control/role-assignments-portal).
- [Aangepaste rollen maken voor op rollen gebaseerd toegangsbeheer](/azure/role-based-access-control/custom-roles).
- [Toegangsbeheer op basis van rollen beheren](azure-stack-manage-permissions.md) in Azure-Stack.


### <a name="authenticate-with-azure-powershell"></a>Verificatie met Azure PowerShell  
Meer informatie over het gebruik van Azure PowerShell om te verifiëren met Azure-Stack kunnen worden gevonden op [configureren van de gebruiker van de Stack van Azure PowerShell-omgeving](azure-stack-powershell-configure-user.md).

### <a name="authenticate-with-azure-cli"></a>Verificatie met Azure CLI
Zie voor meer informatie over het gebruik van Azure PowerShell om te verifiëren met Azure-Stack [installeren en configureren van Azure CLI voor gebruik met Azure-Stack](/azure/azure-stack/user/azure-stack-connect-cli.md).

## <a name="next-steps"></a>Volgende stappen
- [Architectuur van identiteit](azure-stack-identity-architecture.md)   
- [Datacenter-integratie - identiteit](azure-stack-integrate-identity.md)




