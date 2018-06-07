---
title: Azure-Stack Public Key Infrastructure-certificaten voor de implementatie van Azure-Stack geïntegreerd systemen valideren | Microsoft Docs
description: Beschrijft hoe de Azure-Stack PKI-certificaten voor Azure-Stack geïntegreerd systemen valideren. Bevat informatie over het hulpprogramma voor het Azure-Stack certificaat Checker.
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
editor: ''
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/24/2018
ms.author: mabrigg
ms.reviewer: ppacent
ms.openlocfilehash: e381d2ed3c6a972d776dd31f311fcebe2e35823a
ms.sourcegitcommit: 680964b75f7fff2f0517b7a0d43e01a9ee3da445
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/01/2018
ms.locfileid: "34605607"
---
# <a name="validate-azure-stack-pki-certificates"></a>Valideren van Azure Stack PKI-certificaten

Azure-Stack gereedheid van de Registercontrole beschreven in dit artikel vindt u [van de PowerShell Gallery](https://aka.ms/AzsReadinessChecker). U kunt het hulpprogramma gebruiken om te valideren dat de [gegenereerd PKI-certificaten](azure-stack-get-pki-certs.md) geschikt zijn voor de voorafgaand aan de implementatie. U moet certificaten valideren door te laten voldoende tijd om te testen en certificaten opnieuw uitgegeven, indien nodig.

De gereedheid van de Registercontrole voert de volgende validaties van certificaat:

- **PFX lezen**  
    Controleert op geldig PFX-bestand, juiste wachtwoord en een waarschuwing als openbare informatie niet wordt beveiligd door het wachtwoord. 
- **Algoritme voor handtekening**  
    Controleert de algoritme voor handtekening is niet SHA1.
- **Persoonlijke sleutel**  
    Controleert of de persoonlijke sleutel aanwezig is en is geëxporteerd met het kenmerk van de lokale computer. 
- **Certificaatketen**  
    Certificaatketen controles is intact waaronder een controle voor zelf-ondertekende certificaten.
- **DNS-namen**  
    Controleert de SAN bevat relevante DNS-namen voor elk eindpunt of als een ondersteunende jokerteken aanwezig is.
- **Sleutelgebruik**  
    Als het sleutelgebruik digitale handtekening en sleutelcodering bevat en uitgebreide-sleutelgebruik serververificatie en clientverificatie bevat controleert.
- **Sleutelgrootte**  
    Controleert of de sleutelgrootte 2048 aan of groter zijn is.
- **Ketenvolgorde**  
    Controleert de volgorde van de andere certificaten valideren dat de volgorde juist is.
- **Andere certificaten**  
    Zorg ervoor dat er geen andere certificaten worden verpakt door PFX dan het relevante leaf-certificaat en de keten.
- **Er is geen profiel**  
    Controleert of een nieuwe gebruiker het PFX-gegevens zonder een gebruikersprofiel geladen laden kunt, het gedrag van gMSA accounts mimicking tijdens het onderhoud van het certificaat.

> [!IMPORTANT]  
> Het PKI-certificaat is een PFX-bestand en het wachtwoord moet worden behandeld als gevoelige informatie.

## <a name="prerequisites"></a>Vereisten

Uw systeem aan de volgende vereisten voldoen voordat de PKI-certificaten voor de implementatie van een Azure-Stack valideren:

- Microsoft Azure-Stack gereedheid Checker
- SSL certificaten geëxporteerd na de [voorbereiding-instructies](azure-stack-prepare-pki-certs.md)
- DeploymentData.json
- Windows 10 of Windows Server 2016

## <a name="perform-core-services-certificate-validation"></a>Validatie van het servercertificaat core services uitvoeren

Volg deze stappen om voor te bereiden en voor het valideren van de Azure-Stack PKI-certificaten voor de implementatie en de geheime rotatie:

1. Installeer **AzsReadinessChecker** vanuit een PowerShell-prompt (5.1 of hoger), door de volgende cmdlet:

    ````PowerShell  
        Install-Module Microsoft.AzureStack.ReadinessChecker -force 
    ````

2. De mapstructuur certificaat maken. U kunt wijzigen in het onderstaande voorbeeld `<c:\certificates>` naar een nieuw directorypad van uw keuze.

    ````PowerShell  
    New-Item C:\Certificates -ItemType Directory
    
    $directories = 'ACSBlob','ACSQueue','ACSTable','ADFS','Admin Portal','ARM Admin','ARM Public','Graph','KeyVault','KeyVaultInternal','Public Portal'
    
    $destination = 'c:\certificates'
    
    $directories | % { New-Item -Path (Join-Path $destination $PSITEM) -ItemType Directory -Force}
    ````
    
    > [!Note]  
    > AD FS en grafiek zijn vereist als u AD FS worden gebruikt als uw identiteitssysteem.
    
     - Plaats uw certificaten in de juiste mappen in de vorige stap hebt gemaakt. Bijvoorbeeld:  
        - `c:\certificates\ACSBlob\CustomerCertificate.pfx`
        - `c:\certificates\Certs\Admin Portal\CustomerCertificate.pfx`
        - `c:\certificates\Certs\ARM Admin\CustomerCertificate.pfx`

3. De waarden van wijzigen in het venster PowerShell **RegionName** en **FQDN** van toepassing op de Stack van Azure-omgeving en voer de volgende:

    ````PowerShell  
    $pfxPassword = Read-Host -Prompt "Enter PFX Password" -AsSecureString 

    Start-AzsReadinessChecker -CertificatePath c:\certificates -pfxPassword $pfxPassword -RegionName east -FQDN azurestack.contoso.com -IdentitySystem AAD 

    ````

4. Controleer de uitvoer en alle certificaten slagen voor alle tests. Bijvoorbeeld:

    ````PowerShell
    AzsReadinessChecker v1.1803.405.3 started
    Starting Certificate Validation

    Starting Azure Stack Certificate Validation 1.1803.405.3
    Testing: ARM Public\ssl.pfx
        Read PFX: OK
        Signature Algorithm: OK
        Private Key: OK
        Cert Chain: OK
        DNS Names: OK
        Key Usage: OK
        Key Size: OK
        Chain Order: OK
        Other Certificates: OK
    Testing: ACSBlob\ssl.pfx
        Read PFX: OK
        Signature Algorithm: OK
        Private Key: OK
        Cert Chain: OK
        DNS Names: OK
        Key Usage: OK
        Key Size: OK
        Chain Order: OK
        Other Certificates: OK
    Detailed log can be found C:\AzsReadinessChecker\CertificateValidation\CertChecker.log

    Finished Certificate Validation

    AzsReadinessChecker Log location: C:\AzsReadinessChecker\AzsReadinessChecker.log
    AzsReadinessChecker Report location: 
    C:\AzsReadinessChecker\AzsReadinessReport.json
    AzsReadinessChecker Completed
    ````

### <a name="known-issues"></a>Bekende problemen

**Symptoom**: Tests worden overgeslagen

**Oorzaak**: AzsReadinessChecker bepaalde tests worden overgeslagen als een afhankelijkheid niet is voldaan:

 - Andere certificaten worden overgeslagen als de certificaatketen is mislukt.

    ````PowerShell  
    Testing: ACSBlob\singlewildcard.pfx
        Read PFX: OK
        Signature Algorithm: OK
        Private Key: OK
        Cert Chain: OK
        DNS Names: Fail
        Key Usage: OK
        Key Size: OK
        Chain Order: OK
        Other Certificates: Skipped
    Details:
    The certificate records '*.east.azurestack.contoso.com' do not contain a record that is valid for '*.blob.east.azurestack.contoso.com'. Please refer to the documentation for how to create the required certificate file.
    The Other Certificates check was skipped because Cert Chain and/or DNS Names failed. Follow the guidance to remediate those issues and recheck. 
    Detailed log can be found C:\AzsReadinessChecker\CertificateValidation\CertChecker.log

    Finished Certificate Validation

    AzsReadinessChecker Log location: C:\AzsReadinessChecker\AzsReadinessChecker.log
    AzsReadinessChecker Report location (for OEM): C:\AzsReadinessChecker\AzsReadinessChecker.log
    AzsReadinessChecker Completed
    ````

**Resolutie**: Volg de instructies van het hulpprogramma in de detailsectie onder elke reeks tests uit voor elk certificaat.

## <a name="perform-platform-as-a-service-certificate-validation"></a>Platform als een validatie van het servercertificaat uitvoeren

Volg deze stappen voor het voorbereiden en valideren van de Azure-Stack PKI-certificaten voor platform als een service (PaaS)-certificaten als SQL/MySQL of App Services implementaties zijn gepland.

1.  Installeer **AzsReadinessChecker** vanuit een PowerShell-prompt (5.1 of hoger), door de volgende cmdlet:

    ````PowerShell  
      Install-Module Microsoft.AzureStack.ReadinessChecker -force
    ````

2.  Geneste hashtabel met paden en het wachtwoord op elk validatie hoeven PaaS-certificaat maken. In het venster PowerShell uitvoeren:

    ```PowerShell
        $PaaSCertificates = @{
        'PaaSDBCert' = @{'pfxPath' = '<Path to DBAdapter PFX>';'pfxPassword' = (ConvertTo-SecureString -String '<Password for PFX>' -AsPlainText -Force)}
        'PaaSDefaultCert' = @{'pfxPath' = '<Path to Default PFX>';'pfxPassword' = (ConvertTo-SecureString -String '<Password for PFX>' -AsPlainText -Force)}
        'PaaSAPICert' = @{'pfxPath' = '<Path to API PFX>';'pfxPassword' = (ConvertTo-SecureString -String '<Password for PFX>' -AsPlainText -Force)}
        'PaaSFTPCert' = @{'pfxPath' = '<Path to FTP PFX>';'pfxPassword' = (ConvertTo-SecureString -String '<Password for PFX>' -AsPlainText -Force)}
        'PaaSSSOCert' = @{'pfxPath' = '<Path to SSO PFX>';'pfxPassword' = (ConvertTo-SecureString -String '<Password for PFX>' -AsPlainText -Force)}
        }
    ```

3.  Wijzig de waarden van **RegionName** en **FQDN** overeenkomen met uw Azure-Stack-omgeving voor het starten van de validatie. Voer vervolgens:

    ```PowerShell
    Start-AzsReadinessChecker -PaaSCertificates $PaaSCertificates -RegionName east -FQDN azurestack.contoso.com 
    ```
4.  Controleer of de uitvoer en dat alle certificaten slagen voor alle tests.

    ```PowerShell
    AzsReadinessChecker v1.1805.425.2 started
    Starting PaaS Certificate Validation
    
    Starting Azure Stack Certificate Validation 1.0 
    Testing: PaaSCerts\wildcard.appservice.pfx
        Read PFX: OK
        Signature Algorithm: OK
        Private Key: OK
        Cert Chain: OK
        DNS Names: OK
        Key Usage: OK
        Key Size: OK
        Chain Order: OK
        Other Certificates: OK
    Testing: PaaSCerts\api.appservice.pfx
        Read PFX: OK
        Signature Algorithm: OK
        Private Key: OK
        Cert Chain: OK
        DNS Names: OK
        Key Usage: OK
        Key Size: OK
        Chain Order: OK
        Other Certificates: OK
    Testing: PaaSCerts\wildcard.dbadapter.pfx
        Read PFX: OK
        Signature Algorithm: OK
        Private Key: OK
        Cert Chain: OK
        DNS Names: OK
        Key Usage: OK
        Key Size: OK
        Chain Order: OK
        Other Certificates: OK
    Testing: PaaSCerts\sso.appservice.pfx
        Read PFX: OK
        Signature Algorithm: OK
        Private Key: OK
        Cert Chain: OK
        DNS Names: OK
        Key Usage: OK
        Key Size: OK
    ```

## <a name="using-validated-certificates"></a>Met behulp van gevalideerde certificaten

Als uw certificaten zijn geverifieerd door de AzsReadinessChecker, bent u klaar met ze te gebruiken in uw Azure-Stack-implementatie of voor Azure-Stack geheime rotatie. 

 - Voor implementatie, draagt de certificaten voor uw implementatie engineer zodat ze deze kunnen kopiëren naar de host van de implementatie die zijn opgegeven in de [Azure Stack PKI vereisten documentatie](azure-stack-pki-certs.md).
 - Voor geheime rotatie, kunt u de certificaten oude certificaten voor uw Azure-Stack-omgeving infrastructuur voor openbare eindpunten bijwerken door de [Azure Stack geheim rotatie documentatie](azure-stack-rotate-secrets.md).
 - Voor PaaS-services, kunt u de certificaten gebruiken voor het installeren van SQL, MySQL en Resourceproviders voor App-Services in Azure-Stack door de [overzicht van services in Azure Stack documentatie aanbieden](azure-stack-offer-services-overview.md).

## <a name="next-steps"></a>Volgende stappen

[Identiteitintegratie Datacenter](azure-stack-integrate-identity.md)