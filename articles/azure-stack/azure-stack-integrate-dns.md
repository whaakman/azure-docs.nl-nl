---
title: Stack datacenter integratie van Azure - DNS-
description: Meer informatie over het Azure-Stack DNS integreren in uw datacenter DNS
services: azure-stack
author: troettinger
ms.service: azure-stack
ms.topic: article
ms.date: 10/10/2017
ms.author: victorh
keywords: 
ms.openlocfilehash: 40d6d4858ef2e3df61d04dc68c00e09c04f000e2
ms.sourcegitcommit: 6acb46cfc07f8fade42aff1e3f1c578aa9150c73
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/18/2017
---
# <a name="azure-stack-datacenter-integration---dns"></a>Stack datacenter integratie van Azure - DNS-

*Van toepassing op: Azure Stack geïntegreerd systemen*

Voor toegang tot Azure-Stack-eindpunten kunnen (`portal`, `adminportal`, `management`, `adminmanagement`, enz.)  u moet de DNS-Stack van Azure-services integreren met de DNS-servers die als host fungeren van de DNS-zones die u wilt gebruiken in Azure-Stack uit externe Azure-Stack.

## <a name="azure-stack-dns-namespace"></a>Azure-Stack DNS-naamruimte
U zijn vereist voor een aantal belangrijke informatie met betrekking tot DNS wanneer u Azure-Stack implementeert.


|Veld  |Beschrijving  |Voorbeeld|
|---------|---------|---------|
|Regio|De geografische locatie van uw Azure-Stack-implementatie.|`east`|
|Externe domeinnaam|De naam van de zone die u wilt gebruiken voor uw Azure-Stack-implementatie.|`cloud.fabrikam.com`|
|Interne domeinnaam|De naam van de interne zone die wordt gebruikt voor infrastructuurservices in Azure-Stack.  Het is geïntegreerd met Active Directory en persoonlijke (niet bereikbaar is vanaf buiten de Azure-Stack-implementatie).|`azurestack.local`|
|DNS-doorstuurserver|DNS-servers die worden gebruikt voor het doorsturen van DNS-query's, DNS-zones en records die worden gehost buiten Azure Stack, op het bedrijfsintranet of het openbare internet.|`10.57.175.34`<br>`8.8.8.8`|
|Naamgeving voorvoegsel (optioneel)|Het naming voorvoegsel dat u wilt dat uw Azure-Stack infrastructuur-rol exemplaarnamen machine hebben.  Als niet wordt opgegeven, is de standaardinstelling `azs`.|`azs`|

De volledig gekwalificeerde domeinnaam (FQDN) van uw Azure-Stack-implementatie en de eindpunten is de combinatie van de parameter regio en de externe Domain Name-parameter. Met de waarden van de voorbeelden in de vorige tabel, zou de FQDN-naam voor deze implementatie Azure Stack de naam van de volgende zijn:

`east.cloud.fabrikam.com`

Enkele voorbeelden van een van de eindpunten voor deze implementatie eruit als zodanig als de volgende URL's:

`https://portal.east.cloud.fabrikam.com`

`https://adminportal.east.cloud.fabrikam.com`

Als u wilt gebruiken in dit voorbeeld DNS-naamruimte voor de implementatie van een Azure-Stack, zijn de volgende voorwaarden zijn vereist:

- De zone `fabrikam.com` is geregistreerd met een domeinregistrar, een interne zakelijke DNS-server of beide, afhankelijk van uw vereisten voor naamomzetting.
- Het onderliggende domein `cloud.fabrikam.com` bestaat onder de zone `fabrikam.com`.
- De DNS-servers die als host fungeren van de zones `fabrikam.com` en `cloud.fabrikam.com` kan worden bereikt vanaf de Azure-Stack-implementatie.

Om te kunnen omzetten van DNS-namen voor Azure-Stack-eindpunten en exemplaren van de externe Azure-Stack, moet u de DNS-servers die de externe DNS-zone met de DNS-servers die als host fungeren van de bovenliggende zone die u wilt gebruiken als host voor Azure-Stack integreren.


## <a name="resolution-and-delegation"></a>Omzetting en delegering

Er zijn twee typen DNS-servers:

- Een gezaghebbende DNS-server fungeert als host van de DNS-zones. Deze beantwoordt DNS-query's voor records in deze zones.
- Een recursieve DNS-server host geen DNS-zones. De server beantwoordt alle DNS-query's door gezaghebbende DNS-servers aan te roepen om de benodigde gegevens te verzamelen.

Azure Stack bevat zowel gezaghebbende en recursieve DNS-servers. De recursieve-servers worden gebruikt voor het omzetten van namen van alles behalve de interne persoonlijke zone en de externe openbare DNS-zone voor die Azure-Stack-implementatie. 

![Azure DNS-Stack architectuur](media/azure-stack-integrate-dns/Integrate-DNS-01.png)

## <a name="resolving-external-dns-names-from-azure-stack"></a>Het omzetten van externe DNS-namen uit Azure Stack

DNS-namen voor eindpunten buiten Azure Stack omzetten (bijvoorbeeld: www.bing.com), moet u DNS-servers die Azure-Stack gebruiken kunt voor het doorsturen van DNS-aanvragen waarvoor Azure-Stack niet gemachtigd is. Voor de implementatie van zijn DNS-servers die Azure-Stack aanvragen naar verzendt vereist in het werkblad implementatie (in het veld DNS-doorstuurserver). Geef ten minste twee servers in dit veld voor fouttolerantie. Zonder deze waarden mislukt de implementatie van de Azure-Stack.

### <a name="configure-conditional-dns-forwarding"></a>Voorwaardelijk doorsturen van DNS configureren

> [!IMPORTANT]
> Dit geldt alleen voor AD FS-implementatie.

Configureer voorwaardelijk doorsturen zodat naamomzetting met uw bestaande DNS-infrastructuur.

Als u wilt een voorwaardelijke doorstuurserver toevoegen, moet u het bevoegde eindpunt.

Voor deze procedure gebruikt u een computer in uw datacenternetwerk die met het bevoorrechte eindpunt in Azure-Stack communiceren kan.

1. Open een verhoogde Windows PowerShell-sessie (als administrator uitvoeren) en verbinding maken met het IP-adres van de bevoegde eindpunt. De referenties voor CloudAdmin verificatie gebruiken.

   ```
   $cred=Get-Credential 
   Enter-PSSession -ComputerName <IP Address of ERCS> -ConfigurationName PrivilegedEndpoint -Credential $cred
   ```

2. Nadat u verbinding met het bevoorrechte eindpunt maakt, moet u de volgende PowerShell-opdracht uitvoeren. Vervang de voorbeeldwaarden die is opgegeven met de domeinnaam van uw en IP-adressen van de DNS-servers die u wilt gebruiken.

   ```
   Register-CustomDnsServer -CustomDomainName "contoso.com" -CustomDnsIPAddresses "192.168.1.1","192.168.1.2"
   ```

## <a name="resolving-azure-stack-dns-names-from-outside-azure-stack"></a>Het omzetten van Azure Stack DNS-namen van de externe Azure-Stack
De gezaghebbende servers zijn degene die de externe DNS-zone-informatie bevatten en geen zones gebruiker gemaakt. Geïntegreerd met deze servers om in te schakelen zoneoverdracht of voorwaardelijk doorsturen naar Azure Stack DNS-namen van de externe Azure-Stack omzetten.

## <a name="get-dns-server-external-endpoint-information"></a>DNS-Server extern eindpuntinformatie ophalen

Als u wilt uw Azure-Stack-implementatie integreren in uw DNS-infrastructuur, moet u de volgende informatie:

- DNS-server FQDN-namen
- DNS-server IP-adressen

De FQDN-namen voor de Azure-Stack DNS-servers hebben de volgende indeling:

`<NAMINGPREFIX>-ns01.<REGION>.<EXTERNALDOMAINNAME>`

`<NAMINGPREFIX>-ns02.<REGION>.<EXTERNALDOMAINNAME>`

Met behulp van de voorbeeldwaarden, de FQDN-namen voor de DNS-zijn servers:

`azs-ns01.east.cloud.fabrikam.com`

`azs-ns02.east.cloud.fabrikam.com`


Deze informatie wordt ook gemaakt aan het einde van alle Azure-Stack-implementaties in een bestand met de naam `AzureStackStampDeploymentInfo.json`. Dit bestand bevindt zich de `C:\CloudDeployment\logs` map van de implementatie van virtuele machine. Als u niet zeker weet welke waarden voor uw Azure-Stack-implementatie zijn gebruikt, krijgt u de waarden van hieruit.

Als de implementatie van virtuele machine niet langer beschikbaar is of niet toegankelijk is, kunt u de waarden door verbinding te maken met het bevoorrechte eindpunt en de `Get-AzureStackInfo` PowerShell-cmdlet. Zie voor meer informatie over het bevoegde eindpunt (insert hier artikel koppelen).

## <a name="setting-up-conditional-forwarding-to-azure-stack"></a>Voorwaardelijk doorsturen naar Azure Stack instellen

De eenvoudigste en veiligste manier om Azure-Stack integreren met uw DNS-infrastructuur is voorwaardelijk doorsturen van de zone van de server die als host fungeert voor de bovenliggende zone doen. Deze aanpak wordt aanbevolen als u direct controle over de DNS-servers die als host fungeren voor de bovenliggende zone voor uw Azure-Stack externe DNS-naamruimte.

Als u niet bekend met werkwijze voorwaardelijk doorsturen met DNS bent, raadpleegt u het volgende TechNet-artikel: [een voorwaardelijke doorstuurserver toewijzen voor een domeinnaam](https://technet.microsoft.com/library/cc794735), of de specifieke documentatie bij uw DNS-oplossing.

Voorwaardelijk doorsturen kan niet worden gebruikt in scenario's waarbij u uw externe DNS-Zone van een Azure-Stack eruitzien zoals bij een onderliggend domein van uw zakelijke domeinnaam opgegeven. DNS-delegering moet worden geconfigureerd.

Voorbeeld:

- Bedrijfsnetwerk voor DNS-domeinnaam:`contoso.com`
- Azure Stack externe DNS-domeinnaam:`azurestack.contoso.com`

## <a name="delegating-the-external-dns-zone-to-azure-stack"></a>De externe DNS-zone naar Azure Stack delegeren

Voor DNS-namen om te kunnen worden omgezet vanuit buiten een Azure-Stack-implementatie, moet u voor het instellen van DNS-delegering.

Elke registrar heeft zijn eigen hulpprogramma's voor DNS-beheer om de naamserverrecords voor een domein te wijzigen. Bewerk de NS-records in pagina van de registrar DNS-beheer en de NS-records voor de zone vervangt door de waarden in Azure-Stack.

De meeste DNS-registrars moeten u minimaal twee DNS-servers voor het voltooien van de overdracht te bieden.

## <a name="next-steps"></a>Volgende stappen

[Stack datacenter integratie van Azure - identiteit](azure-stack-integrate-identity.md)
