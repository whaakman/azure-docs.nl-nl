---
title: Overzicht van de identiteit voor Azure Stack | Microsoft Docs
description: Meer informatie over de identiteitssystemen die kunt u met Azure-Stack.
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
ms.date: 2/22/2018
ms.author: brenduns
ms.reviewer: 
ms.openlocfilehash: 609ea3300806f3cfed4a3285a096f59be491c684
ms.sourcegitcommit: fbba5027fa76674b64294f47baef85b669de04b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/24/2018
---
# <a name="overview-of-identity-for-azure-stack"></a>Overzicht van de identiteit voor Azure Stack

Azure-Stack is vereist voor Azure AD of Active Directory federatieve Services (AD FS) die wordt ondersteund door Active Directory (AD) als een id-provider. De volgende concepten en autorisatie-informatie kunt u ervoor de keuze tussen identiteitsproviders zijn. De keuze van een provider is een eenmalige besluit dat u maakt bij het implementeren van Azure-Stack.  

Uw keuze tussen Azure AD en AD FS mogelijk beperkt door de modus waarin u Azure-Stack implementeert: 
- Wanneer u in een modus voor verbonden implementeert, kunt u Azure AD of AD FS. 
- Wanneer u in een niet-verbonden modus zonder verbinding met Internet implementeert, wordt alleen voor AD FS ondersteund.

Voor meer informatie over deze opties weergeven van de volgende artikelen afhankelijk van uw Azure-Stack-omgeving:
- Azure Stack Deployment Kit Zie [identiteit overwegingen](azure-stack-datacenter-integration.md#identity-considerations).
- Azure Stack geïntegreerd systemen, Zie [implementatie planningsbeslissingen voor Azure-Stack geïntegreerd systemen](azure-stack-deployment-decisions.md).

 
## <a name="common-concepts-for-identity"></a>Algemene concepten voor identiteit
De volgende secties worden besproken algemene concepten voor id-providers en het gebruik ervan in Azure-Stack.
![Terminologie voor id-providers](media/azure-stack-identity-overview/terminology.png)


### <a name="directories-tenants-and-organizations"></a>Mappen tenants en organisaties
Een directory is een container met informatie over *gebruikers*, *toepassingen*, *groepen*, en *service-principals*.
 
Een directory-tenant is een *organisatie*, zoals Microsoft of uw eigen bedrijf. 
- Azure AD biedt ondersteuning voor meerdere tenants en biedt ondersteuning voor meerdere organisaties, elk in hun eigen directory. Als u Azure AD en meerdere tenants hebt, kunt u toepassingen en andere tenants van dezelfde directory gebruikers uit één tenant toegang verlenen.
- AD FS ondersteunt slechts één tenant en daarom één organisatie. 

### <a name="users-and-groups"></a>Gebruikers en groepen
Gebruikersaccounts (identiteiten) zijn standaard accounts die personen met een gebruikersnaam en wachtwoord worden geverifieerd. Groepen kunnen bestaan gebruikers- of andere groepen. 

Hoe u maken en beheren van gebruikers en groepen, is afhankelijk van de identiteitsoplossing die u gebruikt. 

In Azure-Stack gebruikersaccounts: 
- Worden gemaakt in de  *username@domain*  indeling. Hoewel de AD FS toegewezen gebruikersaccounts aan een Active Directory, AD FS ondersteunt geen gebruik van de  _&lt;domain >\<alias >_ indeling. 
- Kan worden ingesteld voor meervoudige verificatie gebruiken. 
- Zijn beperkt tot de map waar ze wordt eerst registreren, dit de directory organisaties is.
- Kan worden geïmporteerd vanuit uw on-premises adreslijsten. Zie voor meer informatie [uw on-premises adreslijsten integreren met Azure Active Directory](/azure/active-directory/connect/active-directory-aadconnect) in de documentatie van Azure.  

Wanneer u zich bij uw tenantportal organisaties aanmelden, gebruikt u https://portal.local.azurestack.external. 

### <a name="guest-users"></a>Gastgebruikers
Gastgebruikers zijn gebruikersaccounts van andere directory-tenants die toegang tot bronnen in uw directory zijn verleend. Ter ondersteuning van gastgebruiker, moet u ondersteuning voor multitenancy inschakelen en gebruiken van Azure AD. Als wordt ondersteund, kunt u een gastgebruiker voor toegang tot bronnen in uw directory-tenant, waardoor de samenwerking met externe organisaties uitnodigen. 

Om uit te nodigen gastgebruikers, cloudoperators en gebruikers kunnen gebruikmaken van [Azure AD B2B-samenwerking](/azure/active-directory/active-directory-b2b-what-is-azure-ad-b2b). Gebruikers krijg toegang tot documenten, bronnen en toepassingen vanuit uw directory uitgenodigd terwijl controle over uw resources en de gegevens te behouden. 

Als een gastgebruiker kunt u zich aanmelden bij een andere organisaties directory-tenant. Om dit te doen, moet u die organisaties directorynaam toevoegen aan de portal URL.  Bijvoorbeeld als u deel uitmaken van contoso.com, maar moet worden vastgelegd in de map Fabrikam, u https://portal.local.azurestack.external/fabrikam.onmicrosoft.com gebruiken.

### <a name="applications"></a>Toepassingen
U kunt registreren met Azure AD of AD FS-toepassingen en vervolgens bieden aan gebruikers in uw organisatie. 

Toepassingen zijn onder andere:
- **Webtoepassing** – voorbeelden zijn onder meer de Azure portal en Azure Resource Manager.  Deze ondersteuning voor Web-API-aanroepen. 
- **Native client** – voorbeelden zijn onder meer Azure PowerShell, Visual Studio en Azure-opdrachtregelinterface (CLI).

Toepassingen kunnen ondersteuning bieden twee soorten tenancymodus: 
- **Single-tenant** toepassingen ondersteuning bieden voor gebruikers en services alleen uit dezelfde map waarin de toepassing is geregistreerd. 

  > [!NOTE]    
  > Omdat AD FS slechts één map ondersteunt, zijn de toepassingen die u in een AD FS-topologie maakt aan het ontwerp één tenant toepassingen.
- **Multitenant** toepassingen ondersteuning voor gebruik door gebruikers en services uit zowel de map waar de toepassing is geregistreerd, en aanvullende tenant mappen.  Met multitenant-toepassingen aanmelden gebruikers van een andere tenant directory (een andere Azure AD-tenant) kunnen bij uw toepassing.     

  Zie voor meer informatie over multi-tenancymodus [inschakelen multi-tenancymodus](azure-stack-enable-multitenancy.md).  

  Zie voor meer informatie over het ontwikkelen van een app multitenant [multitenant apps](/azure/active-directory/develop/active-directory-devhowto-multi-tenant-overview).

Wanneer u een toepassing registreert, worden twee objecten gemaakt:
- **Object voor de toepassing** -object van de toepassing is de globale weergave van de toepassing op alle tenants. Deze relatie is een-op-een met de softwaretoepassing en alleen aanwezig is in de map waarin de toepassing eerst registers bevinden zich.

 
   
- **Service-principal-object** – een service-principal is een referentie die wordt gemaakt van een toepassing in de map waar de toepassing voor het eerst is geregistreerd. Een service-principal wordt ook gemaakt in de map van elke aanvullende tenant waar de toepassing wordt gebruikt. Deze relatie kan een een-op-veel met de softwaretoepassing zijn.   

Zie voor meer informatie over de toepassing en service-principals, [toepassing en service-principal objecten in Azure Active Directory (Azure AD)](/azure/active-directory/develop/active-directory-application-objects). 

### <a name="service-principals"></a>Service-principals 
Een service-principal is een reeks *referenties* voor een toepassing of service die toegang tot resources in Azure-Stack verlenen. Gebruik van een service-principal scheidt de toepassing worden machtigingen van de machtigingen van de gebruiker die de toepassing gebruikt.

Een service-principal gemaakt in elke tenant waar de toepassing wordt gebruikt. De service-principal wordt een identiteit voor het aanmelden en toegang tot bronnen (zoals gebruikers) die worden beveiligd door deze tenant.   

- Een toepassing voor één tenant heeft slechts één service-principal in de map waar het eerst wordt gemaakt.  Deze service-principal gemaakt en hiermee akkoord gaat gebruiken tijdens de registratie van de toepassing. 
- Een multitenant-webtoepassing of API heeft een service-principal in elke tenant wordt gemaakt wanneer een gebruiker van de tenant die hiermee akkoord gaat met het gebruik van de toepassing.  

Referenties voor service-principals kunnen sleutel (gegenereerd via de Azure portal) of een certificaat zijn.  Gebruik van een certificaat is geschikt voor automatisering omdat certificaten worden beschouwd als veiliger dan het gebruik van sleutels. 


> [!NOTE]    
> Wanneer u AD FS met Azure-Stack gebruikt, kunt alleen de beheerder de service-principals maken. Met AD FS, service-principals zijn certificaten vereist en worden gemaakt via de bevoorrechte (PEP)-eindpunt. Voor meer informatie ziet, [toepassingen toegang bieden tot Azure-Stack](azure-stack-create-service-principals.md).

Zie voor meer informatie over service-principals voor Azure-Stack, [maken van de service-principals](azure-stack-create-service-principals.md).


### <a name="services"></a>Services
Services in Azure-Stack die communiceren met de id-provider zijn als toepassingen met de id-provider geregistreerd. Net als bij toepassingen kan registratie van een service te verifiëren met het identiteitssysteem. 

Alle Azure-services gebruiken [OpenID Connect](/azure/active-directory/develop/active-directory-protocols-openid-connect-code) protocollen en [JSON Web Tokens](/azure/active-directory/develop/active-directory-token-and-claims) (JWT) tot stand brengen van hun identiteit. Vanwege consistent gebruik van de protocollen die door Azure AD en AD FS, kunt u Azure [Active Directory Authentication Library](/azure/active-directory/develop/active-directory-authentication-libraries) (ADAL) voor de verificatie van lokale of naar Azure (in een verbonden scenario). ADAL ook kunt u gebruikmaken van hulpprogramma's zoals Azure PowerShell en CLI voor cross-cloud en on-premises resourcemanagement. 


### <a name="identities-and-your-identity-system"></a>Identiteiten en uw identiteitssysteem 
Identiteiten voor een Azure-Stack bevatten gebruikersaccounts, groepen en service-principals. Wanneer u Azure-Stack installeert, verschillende ingebouwde toepassingen en services die automatisch wordt geregistreerd met de id-provider in de directory-tenant. Sommige services die geregistreerd zijn gebruikt voor beheer. Andere services zijn beschikbaar voor gebruikers. De standaard-registraties geven core services identiteiten die met elkaar communiceren kunnen en met identiteiten u later toevoegen.
Als u Azure AD met multi-tenancymodus hebt ingesteld, worden sommige toepassingen doorgeven aan de nieuwe mappen.  

## <a name="authentication-and-authorization"></a>Verificatie en autorisatie
 

### <a name="authentication-by-applications-and-users"></a>Verificatie door toepassingen en gebruikers
  
![Identiteit tussen lagen van Azure-Stack](media/azure-stack-identity-overview/identity-layers.png)

Voor toepassingen en gebruikers, wordt de architectuur van Azure-Stack beschreven met vier lagen. Interactie tussen elk van deze lagen kunnen verschillende soorten verificatie gebruiken.


|Laag    |Verificatie tussen lagen  |
|---------|---------|
|Hulpprogramma's en -clients, zoals het beheerportal     | Toegang tot of het wijzigen van een resource in Azure-Stack tools en -clients gebruiken een [JSON Web Tokens](/azure/active-directory/develop/active-directory-token-and-claims) (JWT) voor het plaatsen van een aanroep van de Azure Resource Manager.  <br><br> Azure Resource Manager valideert de JWT en geeft weer dat de *claims* in het gepubliceerde token om in te schatten het machtigingsniveau die gebruiker of service-principal heeft in Azure-Stack.        |
|Azure Resource Manager en de bijbehorende services core     |Azure Resource Manager communiceert met Resourceproviders om over te brengen van communicatie van gebruikers. <br><br> Gebruik overdraagt *direct imperatieve* aanroepen of *declaratieve* aanroepen [Azure Resource Manager-sjablonen](/azure/azure-stack/user/azure-stack-arm-templates.md).|
|Resourceproviders     |Aanroepen doorgegeven aan de Resourceproviders zijn beveiligd met verificatie op basis van certificaten. <br><br>Azure Resource Manager en de Resource Provider vervolgens blijven in de communicatie via een API. Voor elke oproep ontvangen van Azure Resource Manager, valideert de Resourceprovider de aanroep van dit certificaat.|
|Infrastructuur en zakelijke logica     |Resourceproviders communiceren met bedrijfslogica en infrastructuur met behulp van een verificatiemodus van hun keuze. De standaard Resourceproviders die worden geleverd met Azure-Stack Windows-verificatie gebruiken om deze communicatie te beveiligen.|

![Informatie die nodig is voor verificatie](media/azure-stack-identity-overview/authentication.png)


### <a name="authenticate-to-azure-resource-manager"></a>In Azure Resource Manager verifiëren
Als u wilt verifiëren met de id-provider en ontvangt van een JWT, hebt u de volgende informatie: 
1.  **URL voor het identiteitssysteem (Authority)** : de URL waarmee de id-provider kan worden bereikt. Bijvoorbeeld:  *&lt;https://login.windows.net>*. 
2.  **App-ID-URI voor Azure Resource Manager** – de unieke id voor de Azure Resource Manager die is geregistreerd bij uw id-provider en uniek is voor elke installatie van een Azure-Stack.
3.  **Referenties** – deze de referenties die u gebruikt om te verifiëren met de id-provider.  
4.  **URL voor Azure Resource Manager** : de URL is de locatie van de Azure Resource Manager-service. Bijvoorbeeld:  *&lt;https://management.azure.com>* of  *&lt;https://management.local.azurestack.external>*.

Wanneer een principal (een client, toepassing of gebruiker) een verificatieaanvraag maakt voor toegang tot een bron, moet deze aanvraag zijn:
- De principal-referenties.
- De App ID URI van de bron die ze willen openen.  

De referenties zijn geverifieerd door de identiteitsprovider. De id-provider ook geverifieerd dat de App ID URI voor een geregistreerde toepassing, en of de principal is de juiste machtigingen voor het ophalen van een token voor die bron. Als de aanvraag geldig is, wordt een JSON Web Token verleend. 

Het token moet vervolgens worden doorgegeven in de header van een aanvraag in de Azure Resource Manager. De Azure Resource Manager biedt de volgende validaties in willekeurige volgorde:
- Valideer de *verlener* (iss) claim om te bevestigen dat het token van de juiste id-provider is. 
- Valideer de *doelgroep* claim (aud) om te bevestigen dat het token in de Azure Resource Manager is uitgegeven. 
- Valideren dat de JWT is ondertekend met een certificaat dat wordt geconfigureerd via OpenID en die bekend in Azure Resource Manager. 
- Controleer de *uitgegeven op* (iat) en *verstrijken* (exp) claims te bevestigen dat het token actief is en kunnen worden geaccepteerd. 

Als alle validaties voltooid zijn, Azure Resource Manager gebruikt de *bezwaar* (oid) en de *groepen* beweert te maken van een lijst met bronnen waartoe de principal toegang heeft. 

![Diagram van het token exchange-protocol](media/azure-stack-identity-overview/token-exchange.png)


### <a name="use-role-based-access-control"></a>Op rollen gebaseerde toegangsbeheer gebruiken  
Op rollen gebaseerde toegangsbeheer (RBAC) in Azure-Stack is consistent met de implementatie in Microsoft Azure.  U kunt toegang tot bronnen beheren door de juiste RBAC-rol toewijzen aan gebruikers, groepen en toepassingen. Raadpleeg de volgende artikelen voor meer informatie over het gebruik van RBAC met Azure-Stack:
- [Aan de slag met toegangsbeheer op basis van rollen in Azure portal](/azure/active-directory/role-based-access-control-what-is).
- [Toegangsbeheer op basis van rollen gebruiken voor het beheren van toegang tot de resources van uw Azure-abonnement](/azure/active-directory/role-based-access-control-configure).
- [Aangepaste rollen maken voor op rollen gebaseerd toegangsbeheer](/azure/active-directory/role-based-access-control-custom-roles).
- [Toegangsbeheer op basis van rollen beheren](azure-stack-manage-permissions.md) in Azure-Stack.


### <a name="authenticate-with-azure-powershell"></a>Verificatie met Azure PowerShell  
Meer informatie over het gebruik van Azure PowerShell om te verifiëren met Azure-Stack kunnen worden gevonden op [configureren van de gebruiker van de Stack van Azure PowerShell-omgeving](azure-stack-powershell-configure-user.md).

### <a name="authenticate-with-azure-cli"></a>Verificatie met Azure CLI
Meer informatie over het gebruik van Azure PowerShell om te verifiëren met Azure-Stack kunnen worden gevonden op [installeren en CLI configureren voor gebruik met Azure-Stack](/azure/azure-stack/user/azure-stack-connect-cli.md).

## <a name="next-steps"></a>Volgende stappen
- [Architectuur van identiteit](azure-stack-identity-architecture.md)   
- [Datacenter-integratie - identiteit](azure-stack-integrate-identity.md)




