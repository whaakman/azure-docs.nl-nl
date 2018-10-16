---
title: Datacenter-integratie Azure Stack - DNS
description: Meer informatie over het Azure Stack DNS integreren met uw datacenter DNS
services: azure-stack
author: jeffgilb
manager: femila
ms.service: azure-stack
ms.topic: article
ms.date: 10/15/2018
ms.author: jeffgilb
ms.reviewer: wfayed
keywords: ''
ms.openlocfilehash: bf412809f9d10296ad600e64abb6d870dbb88d3e
ms.sourcegitcommit: 1aacea6bf8e31128c6d489fa6e614856cf89af19
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/16/2018
ms.locfileid: "49339675"
---
# <a name="azure-stack-datacenter-integration---dns"></a>Datacenter-integratie Azure Stack - DNS
Mogelijk moet toegang hebben tot Azure Stack-eindpunten (**portal**, **adminportal**, **management**, **adminmanagement**, enz.)  van buiten Azure Stack moet u de Azure Stack-DNS-services integreren met de DNS-servers die als host van de DNS-zones die u wilt gebruiken in Azure Stack.

## <a name="azure-stack-dns-namespace"></a>Azure Stack-DNS-naamruimte
U bent op te geven van enkele belangrijke gegevens met betrekking tot DNS wanneer u Azure Stack implementeren.


|Veld  |Beschrijving  |Voorbeeld|
|---------|---------|---------|
|Regio|De geografische locatie van uw Azure Stack-implementatie.|`east`|
|De naam van het externe domein|De naam van de zone die u wilt gebruiken voor uw Azure Stack-implementatie.|`cloud.fabrikam.com`|
|De naam van het interne domein|De naam van de interne zone die wordt gebruikt voor infrastructuurservices in Azure Stack.  Het is Active Directory-integratie en persoonlijke (niet bereikbaar is vanaf buiten de Azure Stack-implementatie).|`azurestack.local`|
|DNS-doorstuurserver|DNS-servers die worden gebruikt voor het doorsturen van DNS-query's, DNS-zones en records die worden gehost buiten Azure Stack, op het bedrijfsintranet of het openbare internet.|`10.57.175.34`<br>`8.8.8.8`|
|Naamvoorvoegsel (optioneel)|Het naamvoorvoegsel dat u wilt dat uw Azure Stack-infrastructuur-rol instantienamen machine hebben.  Als niet is opgegeven, is de standaardinstelling `azs`.|`azs`|

De volledig gekwalificeerde domeinnaam (FQDN) van uw Azure Stack-implementatie en eindpunten is de combinatie van de parameters regio en het externe domeinnaam. Met behulp van de waarden van de voorbeelden in de vorige tabel, zou de FQDN-naam voor deze Azure Stack-implementatie de naam van de volgende zijn:

`east.cloud.fabrikam.com`

Daarom zou enkele voorbeelden van een van de eindpunten voor deze implementatie eruit de volgende URL's:

`https://portal.east.cloud.fabrikam.com`

`https://adminportal.east.cloud.fabrikam.com`

Als u wilt gebruiken in dit voorbeeld van de DNS-naamruimte voor de implementatie van een Azure Stack, moeten de volgende voorwaarden:

- De zone `fabrikam.com` is geregistreerd via een domeinregistrar, een interne zakelijke DNS-server of beide, afhankelijk van uw vereisten voor naamomzetting.
- Het onderliggende domein `cloud.fabrikam.com` bestaat onder de zone `fabrikam.com`.
- De DNS-servers die als host van de zones `fabrikam.com` en `cloud.fabrikam.com` kan worden bereikt vanaf de Azure Stack-implementatie.

Als u DNS-namen voor Azure Stack-eindpunten en exemplaren van buiten Azure Stack oplossen, moet u integreren van de DNS-servers die als host van de externe DNS-zone voor Azure Stack met de DNS-servers die als host van de bovenliggende zone die u wilt gebruiken.


## <a name="resolution-and-delegation"></a>Omzetting en delegering

Er zijn twee typen DNS-servers:

- Een gezaghebbende DNS-server fungeert als host van de DNS-zones. Deze beantwoordt DNS-query's voor records in deze zones.
- Een recursieve DNS-server host geen DNS-zones. De server beantwoordt alle DNS-query's door gezaghebbende DNS-servers aan te roepen om de benodigde gegevens te verzamelen.

Azure Stack bevat beide gezaghebbende en recursieve DNS-servers. De recursieve-servers die worden gebruikt voor het omzetten van namen van alles wat met uitzondering van de interne privézone en de externe openbare DNS-zone voor die Azure Stack-implementatie. 

![Azure Stack-DNS-architectuur](media/azure-stack-integrate-dns/Integrate-DNS-01.png)

## <a name="resolving-external-dns-names-from-azure-stack"></a>Het omzetten van externe DNS-namen van Azure Stack

DNS-namen voor eindpunten buiten Azure Stack oplossen (bijvoorbeeld: www.bing.com), moet u DNS-servers die Azure Stack gebruiken kunt voor het doorsturen van DNS-aanvragen waarvoor Azure Stack niet gemachtigd is. DNS-servers die Azure Stack stuurt aanvragen door naar zijn voor de implementatie vereist in het werkblad voor implementatie (in het veld DNS-doorstuurserver). Geef ten minste twee servers in dit veld voor fouttolerantie. Zonder deze waarden mislukt de implementatie van Azure Stack.

### <a name="configure-conditional-dns-forwarding"></a>Voorwaardelijk doorsturen van DNS configureren

> [!IMPORTANT]
> Dit geldt alleen voor een AD FS-implementatie.

Om te schakelen naamomzetting met uw bestaande DNS-infrastructuur, configureert u voorwaardelijk doorsturen.

Als u wilt een voorwaardelijke doorstuurserver toevoegen, moet u het eindpunt van de bevoegde gebruiken.

Voor deze procedure gebruikt u een computer in uw netwerk van datacenters die met de bevoorrechte eindpunt in Azure Stack communiceren kan.

1. Open een verhoogde Windows PowerShell-sessie (als administrator uitvoeren) en verbinding maken met het IP-adres van de bevoegde eindpunt. De referenties voor CloudAdmin verificatie gebruiken.

   ```
   $cred=Get-Credential 
   Enter-PSSession -ComputerName <IP Address of ERCS> -ConfigurationName PrivilegedEndpoint -Credential $cred
   ```

2. Nadat u verbinding met het eindpunt van de bevoegde maakt, moet u de volgende PowerShell-opdracht uitvoeren. Vervang de voorbeeldwaarden opgegeven met de domeinnaam van uw en IP-adressen van de DNS-servers die u wilt gebruiken.

   ```
   Register-CustomDnsServer -CustomDomainName "contoso.com" -CustomDnsIPAddresses "192.168.1.1","192.168.1.2"
   ```

## <a name="resolving-azure-stack-dns-names-from-outside-azure-stack"></a>Azure Stack-DNS-namen van buiten Azure Stack oplossen
De gezaghebbende servers zijn degene die de externe DNS-zone-informatie bevatten, en een gebruiker gemaakte zones. Integreren met deze servers om in te schakelen zoneoverdracht of voorwaardelijk doorsturen voor de Azure Stack-DNS-naamomzetting van buiten Azure Stack.

## <a name="get-dns-server-external-endpoint-information"></a>Lees informatie over het externe eindpunt van DNS-Server

Als u wilt uw Azure Stack-implementatie integreren in uw DNS-infrastructuur, moet u de volgende informatie:

- DNS-server FQDN-namen
- IP-adressen van DNS-server

De FQDN-namen voor de Azure Stack-DNS-servers hebben de volgende notatie:

`<NAMINGPREFIX>-ns01.<REGION>.<EXTERNALDOMAINNAME>`

`<NAMINGPREFIX>-ns02.<REGION>.<EXTERNALDOMAINNAME>`

Met behulp van de voorbeeldwaarden, de FQDN-namen voor de DNS-zijn servers:

`azs-ns01.east.cloud.fabrikam.com`

`azs-ns02.east.cloud.fabrikam.com`


Deze informatie wordt ook gemaakt aan het einde van alle Azure Stack-implementaties in een bestand met de naam `AzureStackStampDeploymentInfo.json`. Dit bestand bevindt zich de `C:\CloudDeployment\logs` map van de implementatie van virtuele machine. Als u niet zeker weet welke waarden zijn voor uw Azure Stack-implementatie gebruikt, kunt u de waarden komen vanaf deze locatie.

Als de implementatie van virtuele machine niet langer beschikbaar is of niet toegankelijk is, kunt u de waarden door verbinding te maken met het eindpunt van de bevoegdheden uit te voeren de `Get-AzureStackInfo` PowerShell-cmdlet. Zie voor meer informatie, [bevoegde eindpunt](azure-stack-privileged-endpoint.md).

## <a name="setting-up-conditional-forwarding-to-azure-stack"></a>Instellen van voorwaardelijk doorsturen naar Azure Stack

De eenvoudigste en veiligste manier om te integreren in Azure Stack uw DNS-infrastructuur is om te doen van voorwaardelijk doorsturen van de zone van de server die als host fungeert voor de bovenliggende zone. Deze benadering wordt aanbevolen als u direct controle over de DNS-servers die als host fungeren voor de bovenliggende zone voor uw Azure Stack externe DNS-naamruimte.

Als u niet bekend met hoe u voorwaardelijk doorsturen met DNS doet bent, ziet u het volgende TechNet-artikel: [een voorwaardelijke doorstuurserver toewijzen voor een domeinnaam](https://technet.microsoft.com/library/cc794735), of de specifieke documentatie bij uw DNS-oplossing.

In scenario's waarin u de externe Azure Stack DNS-Zone zodat het eruitziet als een onderliggend domein van uw zakelijke domeinnaam hebt opgegeven, kan niet voorwaardelijk doorsturen worden gebruikt. DNS-delegering moet worden geconfigureerd.

Voorbeeld:

- Bedrijfsnetwerk voor DNS-domeinnaam: `contoso.com`
- Azure Stack externe DNS-domeinnaam: `azurestack.contoso.com`

## <a name="delegating-the-external-dns-zone-to-azure-stack"></a>De externe DNS-zone naar Azure Stack delegeren

Voor DNS-namen worden omgezet vanuit buiten een Azure Stack-implementatie, moet u voor het instellen van DNS-delegering.

Elke registrar heeft zijn eigen hulpprogramma's voor DNS-beheer om de naamserverrecords voor een domein te wijzigen. -DNS-beheerpagina van de registrar, bewerk de NS-records en vervang de NS-records voor de zone met die in Azure Stack.

De meeste DNS-registrars moeten u voor een minimum van twee DNS-servers voor het delegeren hebt voltooid.

## <a name="next-steps"></a>Volgende stappen

[Firewall-integratie](azure-stack-firewall.md)
