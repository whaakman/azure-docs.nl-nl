---
title: Azure Stack Public Key Infrastructure-certificaten voor de implementatie van de geïntegreerde Azure Stack-systemen valideren | Microsoft Docs
description: Beschrijft hoe u controleert de Azure Stack PKI-certificaten voor geïntegreerde Azure Stack-systemen. Bevat informatie over met behulp van de Registercontrole Azure Stack-certificaat.
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
ms.date: 01/08/2019
ms.author: mabrigg
ms.reviewer: ppacent
ms.lastreviewed: 01/08/2019
ms.openlocfilehash: 9300e60902b9234af01a64173eefcfb1bc033c61
ms.sourcegitcommit: 94305d8ee91f217ec98039fde2ac4326761fea22
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/05/2019
ms.locfileid: "57410193"
---
# <a name="validate-azure-stack-pki-certificates"></a>Valideren van Azure Stack PKI-certificaten

Azure Stack gereedheid van de Registercontrole die worden beschreven in dit artikel vindt u [vanuit de PowerShell Gallery](https://aka.ms/AzsReadinessChecker). U kunt het hulpprogramma gebruiken om te valideren dat de [PKI-certificaten die zijn gegenereerd](azure-stack-get-pki-certs.md) zijn geschikt voor voorafgaand aan de implementatie. Certificaten valideren door te laten voldoende tijd om te testen en certificaten opnieuw uitgegeven, indien nodig.

De gereedheid van de Registercontrole voert de volgende validaties van certificaat:

- **PFX lezen**  
    Controleert of geldig PFX-bestand, wachtwoord, en of de openbare informatie door het wachtwoord is niet beveiligd. 
- **Handtekeningalgoritme**  
    Controleert dat de handtekeningalgoritme SHA1 niet.
- **Persoonlijke sleutel**  
    Controleert of de persoonlijke sleutel aanwezig is en wordt geëxporteerd met het kenmerk van de lokale computer. 
- **Certificaatketen**  
    De certificaatketen gecontroleerd is intact waaronder een controle voor zelfondertekende certificaten.
- **DNS-namen**  
    Controleert of het SAN bevat relevante DNS-namen voor elk eindpunt, of als een ondersteunende jokertekens aanwezig is.
- **Sleutelgebruik**  
    Als het gebruik van de sleutel een digitale handtekening en sleutelcodering bevat en het uitgebreide sleutelgebruik serververificatie en clientverificatie bevat wordt gecontroleerd.
- **Sleutelgrootte**  
    Controleert of de sleutelgrootte 2048 of groter is.
- **Ketenvolgorde**  
    Controleert of de volgorde van de certificaten gevalideerd of de volgorde juist is.
- **Andere certificaten**  
    Zorg ervoor dat er geen andere certificaten worden ingepakt in PFX dan het relevante leaf-certificaat en de keten.
- **Er is geen profiel**  
    Controleert of dat een nieuwe gebruiker het PFX-gegevens zonder dat het profiel van een gebruiker zijn geladen laden kunt, het gedrag van accounts gMSA mimicking tijdens het onderhoud van het certificaat.

> [!IMPORTANT]  
> Het PKI-certificaat is een PFX-bestand en wachtwoord moet worden beschouwd als vertrouwelijke informatie.

## <a name="prerequisites"></a>Vereisten

Uw systeem moet voldoen aan de volgende vereisten voordat u PKI-certificaten voor een Azure Stack-implementatie valideren:

- Microsoft Azure Stack-gereedheid Checker
- SSL certificaten geëxporteerd na de [voorbereiding instructies](azure-stack-prepare-pki-certs.md)
- DeploymentData.json
- Windows 10 of Windows Server 2016

## <a name="perform-core-services-certificate-validation"></a>Validatie van het servercertificaat core services uitvoeren

Volg deze stappen om voor te bereiden en voor het valideren van de Azure Stack PKI-certificaten voor de implementatie en geheime rotatie:

1. Installeer **AzsReadinessChecker** vanuit een PowerShell-prompt (5.1 of hoger), door de volgende cmdlet:

    ```PowerShell  
        Install-Module Microsoft.AzureStack.ReadinessChecker -force 
    ```

2. De mapstructuur certificaat maken. U kunt wijzigen in het onderstaande voorbeeld `<c:\certificates>` naar een nieuwe directorypad van uw keuze.
    ```PowerShell  
    New-Item C:\Certificates -ItemType Directory
    
    $directories = 'ACSBlob','ACSQueue','ACSTable','Admin Portal','ARM Admin','ARM Public','KeyVault','KeyVaultInternal','Public Portal','Admin Extension Host','Public Extension Host'
    
    $destination = 'c:\certificates'
    
    $directories | % { New-Item -Path (Join-Path $destination $PSITEM) -ItemType Directory -Force}
    ```
    
    > [!Note]  
    > AD FS en een graaf zijn vereist als u AD FS worden gebruikt als uw identiteitssysteem. Bijvoorbeeld:
    >
    > ```PowerShell  
    > $directories = 'ADFS','Graph','ACSBlob','ACSQueue','ACSTable','Admin Portal','ARM Admin','ARM Public','KeyVault','KeyVaultInternal','Public Portal','Admin Extension Host','Public Extension Host'
    > ```
    
     - Plaats uw certificaten in de juiste mappen in de vorige stap hebt gemaakt. Bijvoorbeeld:  
        - `c:\certificates\ACSBlob\CustomerCertificate.pfx`
        - `c:\certificates\Admin Portal\CustomerCertificate.pfx`
        - `c:\certificates\ARM Admin\CustomerCertificate.pfx`

3. In de PowerShell-venster, wijzigt u de waarden van **RegionName** en **FQDN** naar de Azure Stack-omgeving nodig en voer de volgende opdracht uit:

    ```PowerShell  
    $pfxPassword = Read-Host -Prompt "Enter PFX Password" -AsSecureString 

    Invoke-AzsCertificateValidation -CertificatePath c:\certificates -pfxPassword $pfxPassword -RegionName east -FQDN azurestack.contoso.com -IdentitySystem AAD  
    ```

4. Controleer de uitvoer en alle certificaten slagen voor alle tests. Bijvoorbeeld:

```PowerShell
Invoke-AzsCertificateValidation v1.1809.1005.1 started.
Testing: ARM Public\ssl.pfx
Thumbprint: 7F6B27****************************E9C35A
    PFX Encryption: OK
    Signature Algorithm: OK
    DNS Names: OK
    Key Usage: OK
    Key Size: OK
    Parse PFX: OK
    Private Key: OK
    Cert Chain: OK
    Chain Order: OK
    Other Certificates: OK
Testing: Admin Extension Host\ssl.pfx
Thumbprint: A631A5****************************35390A
    PFX Encryption: OK
    Signature Algorithm: OK
    DNS Names: OK
    Key Usage: OK
    Key Size: OK
    Parse PFX: OK
    Private Key: OK
    Cert Chain: OK
    Chain Order: OK
    Other Certificates: OK
Testing: Public Extension Host\ssl.pfx
Thumbprint: 4DBEB2****************************C5E7E6
    PFX Encryption: OK
    Signature Algorithm: OK
    DNS Names: OK
    Key Usage: OK
    Key Size: OK
    Parse PFX: OK
    Private Key: OK
    Cert Chain: OK
    Chain Order: OK
    Other Certificates: OK

Log location (contains PII): C:\Users\username\AppData\Local\Temp\AzsReadinessChecker\AzsReadinessChecker.log
Report location (contains PII): C:\Users\username\AppData\Local\Temp\AzsReadinessChecker\AzsReadinessCheckerReport.json
Invoke-AzsCertificateValidation Completed
```

### <a name="known-issues"></a>Bekende problemen

**Symptoom**: Tests worden overgeslagen

**Oorzaak**: AzsReadinessChecker worden bepaalde tests overgeslagen als een afhankelijkheid niet is voldaan:

 - Andere certificaten worden overgeslagen als de certificaatketen mislukt.

    ```PowerShell  
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

    Log location (contains PII): C:\Users\username\AppData\Local\Temp\AzsReadinessChecker\AzsReadinessChecker.log
    Report location (contains PII): C:\Users\username\AppData\Local\Temp\AzsReadinessChecker\AzsReadinessCheckerReport.json
    Invoke-AzsCertificateValidation Completed
    ```

**Oplossing**: Volg de instructies van het hulpprogramma in de detailsectie onder elke reeks tests uit voor elk certificaat.

## <a name="perform-platform-as-a-service-certificate-validation"></a>Platform als een service-certificaatvalidatie uitvoeren

Volg deze stappen voor het voorbereiden en valideren van de Azure Stack PKI-certificaten voor platform als een service (PaaS)-certificaten, als SQL/MySQL of Services van App-implementaties gepland zijn.

1.  Installeer **AzsReadinessChecker** vanuit een PowerShell-prompt (5.1 of hoger), door de volgende cmdlet:

    ```PowerShell  
      Install-Module Microsoft.AzureStack.ReadinessChecker -force
    ```

2.  Maak een geneste hash-tabel met paden en het wachtwoord op elk PaaS-certificaat dat u hoeft de validatie. In de PowerShell-venster worden uitgevoerd:

    ```PowerShell  
        $PaaSCertificates = @{
        'PaaSDBCert' = @{'pfxPath' = '<Path to DBAdapter PFX>';'pfxPassword' = (ConvertTo-SecureString -String '<Password for PFX>' -AsPlainText -Force)}
        'PaaSDefaultCert' = @{'pfxPath' = '<Path to Default PFX>';'pfxPassword' = (ConvertTo-SecureString -String '<Password for PFX>' -AsPlainText -Force)}
        'PaaSAPICert' = @{'pfxPath' = '<Path to API PFX>';'pfxPassword' = (ConvertTo-SecureString -String '<Password for PFX>' -AsPlainText -Force)}
        'PaaSFTPCert' = @{'pfxPath' = '<Path to FTP PFX>';'pfxPassword' = (ConvertTo-SecureString -String '<Password for PFX>' -AsPlainText -Force)}
        'PaaSSSOCert' = @{'pfxPath' = '<Path to SSO PFX>';'pfxPassword' = (ConvertTo-SecureString -String '<Password for PFX>' -AsPlainText -Force)}
        }
    ```

3.  Wijzig de waarden van **RegionName** en **FQDN** zodat deze overeenkomen met uw Azure Stack-omgeving voor het starten van de validatie. Voer vervolgens

    ```PowerShell  
    Invoke-AzsCertificateValidation -PaaSCertificates $PaaSCertificates -RegionName east -FQDN azurestack.contoso.com 
    ```
4.  Controleer of de uitvoer en dat alle certificaten slagen voor alle tests.

    ```PowerShell
    Invoke-AzsCertificateValidation v1.0 started.
    Thumbprint: 95A50B****************************FA6DDA
        Signature Algorithm: OK
        Parse PFX: OK
        Private Key: OK
        Cert Chain: OK
        DNS Names: OK
        Key Usage: OK
        Key Size: OK
        Chain Order: OK
        Other Certificates: OK
    Thumbprint: EBB011****************************59BE9A
        Signature Algorithm: OK
        Parse PFX: OK
        Private Key: OK
        Cert Chain: OK
        DNS Names: OK
        Key Usage: OK
        Key Size: OK
        Chain Order: OK
        Other Certificates: OK
    Thumbprint: 76AEBA****************************C1265E
        Signature Algorithm: OK
        Parse PFX: OK
        Private Key: OK
        Cert Chain: OK
        DNS Names: OK
        Key Usage: OK
        Key Size: OK
        Chain Order: OK
        Other Certificates: OK
    Thumbprint: 8D6CCD****************************DB6AE9
        Signature Algorithm: OK
        Parse PFX: OK
        Private Key: OK
        Cert Chain: OK
        DNS Names: OK
        Key Usage: OK
        Key Size: OK
    ```

## <a name="certificates"></a>Certificaten

| Directory | Certificaat |
| ---    | ----        |
| acsBlob | wildcard_blob_\< regio >\< externalFQDN > |
| ACSQueue  |  wildcard_queue\< regio >\< externalFQDN > |
| ACSTable  |  wildcard_table\< regio >\< externalFQDN > |
| Beheerdersuitbreiding Host  |  wildcard_adminhosting\< regio >\< externalFQDN > |
| Beheerportal  |  adminportal\< regio >\< externalFQDN > |
| ARM-beheerder  |  adminmanagement\< regio >\< externalFQDN > |
| ARM-algemeen  |  Management\< regio >\< externalFQDN > |
| KeyVault  |  wildcard_vault\< regio >\< externalFQDN > |
| KeyVaultInternal  |  wildcard_adminvault\< regio >\< externalFQDN > |
| Host van de openbare-extensie  |  wildcard_hosting\< regio >\< externalFQDN > |
| Openbare-Portal  |  Portal\< regio > _\< externalFQDN > |

## <a name="using-validated-certificates"></a>Met behulp van gevalideerde certificaten

Als uw certificaten zijn geverifieerd door de AzsReadinessChecker, bent u klaar om ze te gebruiken in uw Azure Stack-implementatie of voor Azure Stack geheime rotatie. 

 - Voor de implementatie veilig overdragen uw certificaten naar de engineer van uw implementatie zodat ze deze kunnen kopiëren naar de host van de implementatie die zijn opgegeven in de [documentatie voor Azure Stack PKI-vereisten](azure-stack-pki-certs.md).
 - Voor geheime roteren, kunt u de certificaten gebruiken om bij te werken van oude certificaten voor uw Azure Stack-omgeving infrastructuur voor openbare eindpunten door de [documentatie voor Azure Stack-geheim rotatie](azure-stack-rotate-secrets.md).
 - Voor PaaS-services, kunt u de certificaten gebruiken voor het installeren van SQL, MySQL en Resourceproviders voor App Services in Azure Stack door de [overzicht van services in de documentatie over Azure Stack-aanbieding](azure-stack-offer-services-overview.md).

## <a name="next-steps"></a>Volgende stappen

[Datacenter-identiteitintegratie](azure-stack-integrate-identity.md)
