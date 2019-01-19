---
title: Voorbereiden voor de host van de extensie voor Azure Stack | Microsoft Docs
description: Informatie over het voorbereiden voor extensie-host, die automatisch wordt ingeschakeld via een toekomstige Update van Azure Stack-pakket.
services: azure-stack
keywords: ''
author: mattbriggs
ms.author: mabrigg
ms.date: 11/27/2018
ms.topic: article
ms.service: azure-stack
ms.reviewer: thoroet
manager: femila
ms.openlocfilehash: ec7b1ad33ec80593b718bdf86a48269fb469e078
ms.sourcegitcommit: 82cdc26615829df3c57ee230d99eecfa1c4ba459
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/19/2019
ms.locfileid: "54413512"
---
# <a name="prepare-for-extension-host-for-azure-stack"></a>Voorbereiden voor de host van de extensie voor Azure Stack

De host van de extensie voor Azure Stack beveiliging door het aantal vereiste TCP/IP-poorten te verminderen. In dit artikel wordt behandeld Azure Stack voorbereiden voor de extensie-host, die automatisch wordt ingeschakeld via een Azure Stack-Update-pakket na de 1808 update.

## <a name="certificate-requirements"></a>Vereisten voor certificaten

De extensie-host implementeert twee nieuwe domeinen naamruimten om te garanderen unieke host-vermeldingen voor elke portalextensie. Het nieuwe domeinnaamruimten moet twee extra jokertekens certificaten voor veilige communicatie.

De tabel ziet u de nieuwe naamruimten en de bijbehorende certificaten:

| Implementatiemap | Vereiste certificaatonderwerp en alternatieve namen voor onderwerpen (SAN) | Bereik (per regio) | Subdomein naamruimte |
|-----------------------|------------------------------------------------------------------|-----------------------|------------------------------|
| Beheerder extensie host | *.adminhosting. \<regio >. \<FQDN-naam > (Wildcard-SSL-certificaten) | Beheerder extensie host | adminhosting. \<regio >. \<FQDN-naam > |
| Host van de openbare-extensie | * .hosting. \<regio >. \<FQDN-naam > (Wildcard-SSL-certificaten) | Host van de openbare-extensie | die als host fungeert. \<regio >. \<FQDN-naam > |

De gedetailleerde certificaatvereisten kunnen u vinden in de [certificaatvereisten voor Azure Stack-openbare-sleutelinfrastructuur](azure-stack-pki-certs.md) artikel.

## <a name="create-certificate-signing-request"></a>Aanvraag voor Certificaatondertekening maken

Het hulpprogramma Azure Stack Readiness Checker biedt de mogelijkheid om te maken van een aanvraag voor de twee nieuwe, vereist SSL-certificaten voor Certificaatondertekening. Volg de stappen in het artikel [Azure Stack-ondertekening aanvraag genereren van certificaten](azure-stack-get-pki-certs.md).

> [!Note]  
> U kunt deze stap, afhankelijk van hoe u uw SSL-certificaten aangevraagd overslaan.

## <a name="validate-new-certificates"></a>Nieuwe certificaten valideren

1. Open PowerShell met verhoogde machtigingen op de host van de levenscyclus van hardware of het beheerwerkstation voor Azure Stack.
2. Voer de volgende cmdlet voor het installeren van het hulpprogramma Azure Stack-gereedheid van de Registercontrole.

    ```PowerShell  
    Install-Module -Name Microsoft.AzureStack.ReadinessChecker
    ```

3. Voer het volgende script om de structuur van de vereiste map te maken:

    ```PowerShell  
    New-Item C:\Certificates -ItemType Directory

    $directories = 'ACSBlob','ACSQueue','ACSTable','Admin Portal','ARM Admin','ARM Public','KeyVault','KeyVaultInternal','Public Portal', 'Admin extension host', 'Public extension host'

    $destination = 'c:\certificates'

    $directories | % { New-Item -Path (Join-Path $destination $PSITEM) -ItemType Directory -Force}
    ```

    > [!Note]  
    > Als u met Azure Active Directory Federated Services (AD FS implementeert) de volgende mappen moeten worden toegevoegd aan **$directories** in het script: `ADFS`, `Graph`.

4. Voer de volgende cmdlets voor het starten van de certificaatcontrole:

    ```PowerShell  
    $pfxPassword = Read-Host -Prompt "Enter PFX Password" -AsSecureString 

    Start-AzsReadinessChecker -CertificatePath c:\certificates -pfxPassword $pfxPassword -RegionName east -FQDN azurestack.contoso.com -IdentitySystem AAD
    ```

5. Plaats uw certificaten in de juiste mappen.

6. Controleer de uitvoer en alle certificaten slagen voor alle tests.


## <a name="import-extension-host-certificates"></a>Extensie host certificaten importeren

Een computer die verbinding met het Azure Stack in beschermde modus-eindpunt voor de volgende stappen maken kan gebruiken. Zorg ervoor dat u toegang hebt tot de nieuwe certificaatbestanden vanaf die computer.

1. Een computer die verbinding met het Azure Stack in beschermde modus-eindpunt voor de volgende stappen maken kan gebruiken. Zorg ervoor dat u toegang tot de nieuwe certificaatbestanden vanaf die computer.
2. Open PowerShell ISE voor het uitvoeren van de volgende scriptblokken
3. Importeer het certificaat voor de beheerder die als host fungeert eindpunt.

    ```PowerShell  

    $CertPassword = read-host -AsSecureString -prompt "Certificate Password"

    $CloudAdminCred = Get-Credential -UserName <Privileged endpoint credentials> -Message "Enter the cloud domain credentials to access the privileged endpoint."

    [Byte[]]$AdminHostingCertContent = [Byte[]](Get-Content c:\certificate\myadminhostingcertificate.pfx -Encoding Byte)

    Invoke-Command -ComputerName <PrivilegedEndpoint computer name> `
    -Credential $CloudAdminCred `
    -ConfigurationName "PrivilegedEndpoint" `
    -ArgumentList @($AdminHostingCertContent, $CertPassword) `
    -ScriptBlock {
            param($AdminHostingCertContent, $CertPassword)
            Import-AdminHostingServiceCert $AdminHostingCertContent $certPassword
    }
    ```
4. Importeer het certificaat voor de hosting-eindpunt.
    ```PowerShell  
    $CertPassword = read-host -AsSecureString -prompt "Certificate Password"

    $CloudAdminCred = Get-Credential -UserName <Privileged endpoint credentials> -Message "Enter the cloud domain credentials to access the privileged endpoint."

    [Byte[]]$HostingCertContent = [Byte[]](Get-Content c:\certificate\myhostingcertificate.pfx  -Encoding Byte)

    Invoke-Command -ComputerName <PrivilegedEndpoint computer name> `
    -Credential $CloudAdminCred `
    -ConfigurationName "PrivilegedEndpoint" `
    -ArgumentList @($HostingCertContent, $CertPassword) `
    -ScriptBlock {
            param($HostingCertContent, $CertPassword)
            Import-UserHostingServiceCert $HostingCertContent $certPassword
    }
    ```

### <a name="update-dns-configuration"></a>DNS-configuratie bijwerken

> [!Note]  
> Deze stap is niet vereist als u DNS-Zone-overdracht voor DNS-integratie gebruikt.
Als een afzonderlijke host A-records voor het publiceren van Azure Stack-eindpunten zijn geconfigureerd, moet u twee extra host A-records maken:

| IP | Hostnaam | Type |
|----|------------------------------|------|
| \<IP> | *. Adminhosting. \<Regio >. \<FQDN-naam > | A |
| \<IP> | *. Die als host fungeert. \<Regio >. \<FQDN-naam > | A |

Toegewezen IP-adressen kan worden opgehaald met behulp van bevoegde eindpunt door de cmdlet **Get-AzureStackStampInformation**.

### <a name="ports-and-protocols"></a>Poorten en protocollen

Het artikel [datacenter-integratie van Azure Stack - eindpunten publiceren](azure-stack-integrate-endpoints.md), bevat informatie over de poorten en protocollen waarvoor binnenkomende communicatie met Azure Stack publiceren voordat de implementatie van de host extensie.

### <a name="publish-new-endpoints"></a>Nieuwe eindpunten publiceren

Er zijn twee nieuwe eindpunten moeten worden gepubliceerd door uw firewall. De toegewezen IP-adressen van de openbare VIP-groep kan worden opgehaald met de volgende code die moet worden uitgevoerd via uw Azure Stack [omgeving in de beschermde modus eindpunt](https://docs.microsoft.com/en-gb/azure/azure-stack/azure-stack-privileged-endpoint).

```PowerShell
# Create a PEP Session
winrm s winrm/config/client '@{TrustedHosts= "<IpOfERCSMachine>"}'
$PEPCreds = Get-Credential
$PEPSession = New-PSSession -ComputerName <IpOfERCSMachine> -Credential $PEPCreds -ConfigurationName "PrivilegedEndpoint"

# Obtain DNS Servers and Extension Host information from Azure Stack Stamp Information and find the IPs for the Host Extension Endpoints
$StampInformation = Invoke-Command $PEPSession {Get-AzureStackStampInformation} | Select-Object -Property ExternalDNSIPAddress01, ExternalDNSIPAddress02, @{n="TenantHosting";e={($_.TenantExternalEndpoints.TenantHosting) -replace "https://*.","testdnsentry"-replace "/"}},  @{n="AdminHosting";e={($_.AdminExternalEndpoints.AdminHosting)-replace "https://*.","testdnsentry"-replace "/"}},@{n="TenantHostingDNS";e={($_.TenantExternalEndpoints.TenantHosting) -replace "https://",""-replace "/"}},  @{n="AdminHostingDNS";e={($_.AdminExternalEndpoints.AdminHosting)-replace "https://",""-replace "/"}}
If (Resolve-DnsName -Server $StampInformation.ExternalDNSIPAddress01 -Name $StampInformation.TenantHosting -ErrorAction SilentlyContinue) {
    Write-Host "Can access AZS DNS" -ForegroundColor Green
    $AdminIP = (Resolve-DnsName -Server $StampInformation.ExternalDNSIPAddress02 -Name $StampInformation.AdminHosting).IPAddress
    Write-Host "The IP for the Admin Extension Host is: $($StampInformation.AdminHostingDNS) - is: $($AdminIP)" -ForegroundColor Yellow
    Write-Host "The Record to be added in the DNS zone: Type A, Name: $($StampInformation.AdminHostingDNS), Value: $($AdminIP)" -ForegroundColor Green
    $TenantIP = (Resolve-DnsName -Server $StampInformation.ExternalDNSIPAddress01 -Name $StampInformation.TenantHosting).IPAddress
    Write-Host "The IP address for the Tenant Extension Host is $($StampInformation.TenantHostingDNS) - is: $($TenantIP)" -ForegroundColor Yellow
    Write-Host "The Record to be added in the DNS zone: Type A, Name: $($StampInformation.TenantHostingDNS), Value: $($TenantIP)" -ForegroundColor Green
}
Else {
    Write-Host "Cannot access AZS DNS" -ForegroundColor Yellow
    $AdminIP = (Resolve-DnsName -Name $StampInformation.AdminHosting).IPAddress
    Write-Host "The IP for the Admin Extension Host is: $($StampInformation.AdminHostingDNS) - is: $($AdminIP)" -ForegroundColor Yellow
    Write-Host "The Record to be added in the DNS zone: Type A, Name: $($StampInformation.AdminHostingDNS), Value: $($AdminIP)" -ForegroundColor Green
    $TenantIP = (Resolve-DnsName -Name $StampInformation.TenantHosting).IPAddress
    Write-Host "The IP address for the Tenant Extension Host is $($StampInformation.TenantHostingDNS) - is: $($TenantIP)" -ForegroundColor Yellow
    Write-Host "The Record to be added in the DNS zone: Type A, Name: $($StampInformation.TenantHostingDNS), Value: $($TenantIP)" -ForegroundColor Green
}
Remove-PSSession -Session $PEPSession
```

#### <a name="sample-output"></a>Voorbeeld van uitvoer

```PowerShell
Can access AZS DNS
The IP for the Admin Extension Host is: *.adminhosting.\<region>.\<fqdn> - is: xxx.xxx.xxx.xxx
The Record to be added in the DNS zone: Type A, Name: *.adminhosting.\<region>.\<fqdn>, Value: xxx.xxx.xxx.xxx
The IP address for the Tenant Extension Host is *.hosting.\<region>.\<fqdn> - is: xxx.xxx.xxx.xxx
The Record to be added in the DNS zone: Type A, Name: *.hosting.\<region>.\<fqdn>, Value: xxx.xxx.xxx.xxx
```

> [!Note]  
> Controleer deze wijziging voordat u de host van de uitbreiding inschakelt. Hiermee wordt de Azure Stack-portals continu toegankelijk moeten zijn.

| Eindpunt (VIP) | Protocol | Poorten |
|----------------|----------|-------|
| AdminHosting | HTTPS | 443 |
| Hosting | HTTPS | 443 |

### <a name="update-existing-publishing-rules-post-enablement-of-extension-host"></a>Bijwerken van bestaande regels voor publicatie (na activering van extensie-host)

> [!Note]  
> De 1808 Azure Stack-updatepakket heeft **niet** extensie host nog inschakelen. Hiermee kunnen om voor te bereiden voor extensie-host door het importeren van de vereiste certificaten. Sluit alle poorten niet voordat de host van de extensie wordt automatisch ingeschakeld via een pakket met Azure Stack bijgewerkt na de 1808 update.

De volgende bestaande endpoint-poorten moeten worden gesloten in uw bestaande firewallregels.

> [!Note]  
> Het verdient aanbeveling om te sluiten van deze poorten na een succesvolle validatie.

| Eindpunt (VIP) | Protocol | Poorten |
|----------------------------------------|----------|-------------------------------------------------------------------------------------------------------------------------------------|
| Portal (beheerder) | HTTPS | 12495<br>12499<br>12646<br>12647<br>12648<br>12649<br>12650<br>13001<br>13003<br>13010<br>13011<br>13012<br>13020<br>13021<br>13026<br>30015 |
| Portal (gebruiker) | HTTPS | 12495<br>12649<br>13001<br>13010<br>13011<br>13012<br>13020<br>13021<br>30015<br>13003 |
| Azure Resource Manager (beheerder) | HTTPS | 30024 |
| Azure Resource Manager (user) | HTTPS | 30024 |

## <a name="next-steps"></a>Volgende stappen

- Meer informatie over [Firewall-integratie](azure-stack-firewall.md).
- Meer informatie over [Azure Stack-ondertekening aanvraag genereren van certificaten](azure-stack-get-pki-certs.md)
