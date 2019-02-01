---
title: Start-AzsReadinessChecker cmdlet-verwijzing | Microsoft Docs
description: PowerShell-cmdlet Help-informatie voor de Azure Stack-gereedheid Checker-module.
services: azure-stack
documentationcenter: ''
author: sethmanheim
manager: femila
editor: ''
ms.assetid: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 12/04/2018
ms.author: sethm
ms.reviewer: unknown
ms.lastreviewed: 12/04/2018
ms.openlocfilehash: 861784a2d22d15253c61884bfab62667c0560bcd
ms.sourcegitcommit: 898b2936e3d6d3a8366cfcccc0fccfdb0fc781b4
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/30/2019
ms.locfileid: "55250636"
---
# <a name="start-azsreadinesschecker-cmdlet-reference"></a>Start-AzsReadinessChecker cmdlet-verwijzing

Module: Microsoft.AzureStack.ReadinessChecker

Deze module bevat alleen een één-cmdlet.  Deze cmdlet voert een of meer vóór de implementatie of vooraf servicing-functies voor Azure Stack.

## <a name="syntax"></a>Syntaxis

```PowerShell
Start-AzsReadinessChecker
       [-CertificatePath <String>]
       -PfxPassword <SecureString>
       -RegionName <String>
       -FQDN <String>
       -IdentitySystem <String>
       [-CleanReport]
       [-OutputPath <String>]
       [-WhatIf]
       [-Confirm]
       [<CommonParameters>]
```

```PowerShell
Start-AzsReadinessChecker
       [-CertificatePath <String>]
       -PfxPassword <SecureString>
       -DeploymentDataJSONPath <String>
       [-CleanReport]
       [-OutputPath <String>]
       [-WhatIf]
       [-Confirm]
       [<CommonParameters>]
```

```PowerShell
Start-AzsReadinessChecker
       -PaaSCertificates <Hashtable>
       -DeploymentDataJSONPath <String>
       [-OutputPath <String>]
       [-WhatIf]
       [-Confirm]
       [<CommonParameters>]
```

```PowerShell
Start-AzsReadinessChecker
       -PaaSCertificates <Hashtable>
       -RegionName <String>
       -FQDN <String>
       -IdentitySystem <String>
       [-OutputPath <String>]
       [-WhatIf]
       [-Confirm]
       [<CommonParameters>]
```

```PowerShell
Start-AzsReadinessChecker
       -RegionName <String>
       -FQDN <String>
       -IdentitySystem <String>
       -Subject <OrderedDictionary>
       -RequestType <String>
       [-IncludePaaS]
       -OutputRequestPath <String>
       [-OutputPath <String>]
       [-WhatIf]
       [-Confirm]
       [<CommonParameters>]
```

```PowerShell
Start-AzsReadinessChecker
       -PfxPassword <SecureString>
       -PfxPath <String>
       -ExportPFXPath <String>
       [-OutputPath <String>]
       [-WhatIf]
       [-Confirm]
       [<CommonParameters>]
```

```PowerShell
Start-AzsReadinessChecker
       -AADServiceAdministrator <PSCredential>
       -AADDirectoryTenantName <String>
       -IdentitySystem <String>
       -AzureEnvironment <String>
       [-CleanReport]
       [-OutputPath <String>]
       [-WhatIf]
       [-Confirm]
       [<CommonParameters>]
```

```PowerShell
Start-AzsReadinessChecker
       -AADServiceAdministrator <PSCredential>
       -DeploymentDataJSONPath <String>
       [-CleanReport]
       [-OutputPath <String>]
       [-WhatIf]
       [-Confirm]
       [<CommonParameters>]
```

```PowerShell
Start-AzsReadinessChecker
       -RegistrationAccount <PSCredential>
       -RegistrationSubscriptionID <Guid>
       -AzureEnvironment <String>
       [-CleanReport]
       [-OutputPath <String>]
       [-WhatIf]
       [-Confirm]
       [<CommonParameters>]
```

```PowerShell
Start-AzsReadinessChecker
       -RegistrationAccount <PSCredential>
       -RegistrationSubscriptionID <Guid>
       -DeploymentDataJSONPath <String>
       [-CleanReport]
       [-OutputPath <String>]
       [-WhatIf]
       [-Confirm]
       [<CommonParameters>]
```

```PowerShell
Start-AzsReadinessChecker
       -ReportPath <String>
       [-ReportSections <String>]
       [-Summary]
       [-OutputPath <String>]
       [-WhatIf]
       [-Confirm]
       [<CommonParameters>]
```

## <a name="description"></a>Beschrijving

De **Start AzsReadinessChecker** cmdlet valideert certificaten, Azure-accounts, Azure-abonnementen en Azure Active Directory's. Validatie voordat u implementeert Azure Stack, of voor Azure Stack bewerkingen zoals het geheim rotatie onderhoud worden uitgevoerd. De cmdlet kan ook worden gebruikt voor het ondertekenen van certificaataanvragen genereren voor infrastructuur en eventueel PaaS certificaten.  Ten slotte de cmdlet opnieuw verpakken PFX-certificaten voor het oplossen van veelvoorkomende problemen met verpakking.

## <a name="examples"></a>Voorbeelden

### <a name="example-generate-certificate-signing-request"></a>Voorbeeld: Aanvraag voor Certificaatondertekening genereren

```PowerShell
$regionName = 'east'
$externalFQDN = 'azurestack.contoso.com'
$subjectHash = [ordered]@{"OU"="AzureStack";"O"="Microsoft";"L"="Redmond";"ST"="Washington";"C"="US"}
Start-AzsReadinessChecker -regionName $regionName -externalFQDN $externalFQDN -subject $subjectHash -IdentitySystem ADFS -requestType MultipleCSR
```

In dit voorbeeld Start AzsReadinessChecker genereert meerdere Certificate Signing-aanvragen (CSR) voor certificaten die geschikt zijn voor een AD FS Azure Stack-implementatie met de naam van een gebied van 'Oost' en een externe FQDN-naam van "azurestack.contoso.com"

### <a name="example-validate-certificates"></a>Voorbeeld: Certificaten valideren

```PowerShell
$password = Read-Host -Prompt "Enter PFX Password" -AsSecureString
Start-AzsReadinessChecker -CertificatePath .\Certificates\ -PfxPassword $password -RegionName east -FQDN azurestack.contoso.com -IdentitySystem AAD
```

In dit voorbeeld wordt het PFX-wachtwoord wordt gevraagd om veilig en Start AzsReadinessChecker controleert de relatieve map 'Certificaten' voor certificaten geldig zijn voor een AAD-implementatie met de naam van een gebied van 'Oost' en een externe FQDN-naam van "azurestack.contoso.com"

### <a name="example-validate-certificates-with-deployment-data-deployment-and-support"></a>Voorbeeld: Valideren van certificaten met de implementatiegegevens (implementatie en ondersteuning)

```PowerShell
$password = Read-Host -Prompt "Enter PFX Password" -AsSecureString
Start-AzsReadinessChecker -CertificatePath .\Certificates\ -PfxPassword $password -DeploymentDataJSONPath .\deploymentdata.json
```

In dit voorbeeld implementatie en ondersteuning voor het PFX-wachtwoord wordt gevraagd om veilig en Start AzsReadinessChecker controleert de relatieve map 'Certificaten' voor certificaten geldig zijn voor een implementatie waarbij identiteit, regio en externe FQDN-naam worden gelezen uit de implementatie gegevens JSON-bestand is gegenereerd voor de implementatie. 

### <a name="example-validate-paas-certificates"></a>Voorbeeld: PaaS-certificaten valideren

```PowerShell
$PaaSCertificates = @{
    'PaaSDBCert' = @{'pfxPath' = '<Path to DBAdapter PFX>';'pfxPassword' = (ConvertTo-SecureString -String '<Password for PFX>' -AsPlainText -Force)}
    'PaaSDefaultCert' = @{'pfxPath' = '<Path to Default PFX>';'pfxPassword' = (ConvertTo-SecureString -String '<Password for PFX>' -AsPlainText -Force)}
    'PaaSAPICert' = @{'pfxPath' = '<Path to API PFX>';'pfxPassword' = (ConvertTo-SecureString -String '<Password for PFX>' -AsPlainText -Force)}
    'PaaSFTPCert' = @{'pfxPath' = '<Path to FTP PFX>';'pfxPassword' = (ConvertTo-SecureString -String '<Password for PFX>' -AsPlainText -Force)}
    'PaaSSSOCert' = @{'pfxPath' = '<Path to SSO PFX>';'pfxPassword' = (ConvertTo-SecureString -String '<Password for PFX>' -AsPlainText -Force)}
}
Start-AzsReadinessChecker -PaaSCertificates $PaaSCertificates – RegionName east -FQDN azurestack.contoso.com
```

In dit voorbeeld wordt een hashtabel geconstrueerd met paden en wachtwoorden voor elk certificaat PaaS. Certificaten kunnen worden weggelaten. Start-AzsReadinessChecker controleert elke PFX-pad bestaat en wordt gevalideerd met behulp van de regio Oost' en externe FQDN-naam 'azurestack.contoso.com'.

### <a name="example-validate-paas-certificates-with-deployment-data"></a>Voorbeeld: PaaS-certificaten met implementatiegegevens valideren

```PowerShell
$PaaSCertificates = @{
    'PaaSDBCert' = @{'pfxPath' = '<Path to DBAdapter PFX>';'pfxPassword' = (ConvertTo-SecureString -String '<Password for PFX>' -AsPlainText -Force)}
    'PaaSDefaultCert' = @{'pfxPath' = '<Path to Default PFX>';'pfxPassword' = (ConvertTo-SecureString -String '<Password for PFX>' -AsPlainText -Force)}
    'PaaSAPICert' = @{'pfxPath' = '<Path to API PFX>';'pfxPassword' = (ConvertTo-SecureString -String '<Password for PFX>' -AsPlainText -Force)}
    'PaaSFTPCert' = @{'pfxPath' = '<Path to FTP PFX>';'pfxPassword' = (ConvertTo-SecureString -String '<Password for PFX>' -AsPlainText -Force)}
    'PaaSSSOCert' = @{'pfxPath' = '<Path to SSO PFX>';'pfxPassword' = (ConvertTo-SecureString -String '<Password for PFX>' -AsPlainText -Force)}
}
Start-AzsReadinessChecker -PaaSCertificates $PaaSCertificates -DeploymentDataJSONPath .\deploymentdata.json
```

In dit voorbeeld wordt een hashtabel geconstrueerd met paden en wachtwoorden voor elk certificaat PaaS. Certificaten kunnen worden weggelaten. Start-AzsReadinessChecker controleert of elk PFX-pad bestaat en wordt gevalideerd met behulp van de regio en externe FQDN lezen van het implementatiebestand-JSON gegenereerd voor de implementatie. 

### <a name="example-validate-azure-identity"></a>Voorbeeld: Valideren van de Azure-identiteit

```PowerShell
$serviceAdminCredential = Get-Credential -Message "Enter Credentials for Service Administrator of Azure Active Directory Tenant e.g. serviceadmin@contoso.onmicrosoft.com"
# Supported values for the <environment name> parameter are AzureCloud, AzureChinaCloud or AzureUSGovernment depending which Azure subscription you are using.
Start-AzsReadinessChecker -AADServiceAdministrator $serviceAdminCredential -AzureEnvironment "<environment name>" -AzureDirectoryTenantName azurestack.contoso.com
```

In dit voorbeeld wordt de accountreferenties van de servicebeheerder wordt gevraagd om veilig en Start AzsReadinessChecker controleert of dat de Azure-account en Azure Active Directory zijn geldig voor een AAD-implementatie met de naam van een tenant-map van "azurestack.contoso.com"

### <a name="example-validate-azure-identity-with-deployment-data-deployment-support"></a>Voorbeeld: Valideren van de Azure-identiteit met implementatiegegevens (ondersteuning voor implementatie)

```PowerSHell
$serviceAdminCredential = Get-Credential -Message "Enter Credentials for Service Administrator of Azure Active Directory Tenant e.g. serviceadmin@contoso.onmicrosoft.com"
Start-AzsReadinessChecker -AADServiceAdministrator $serviceAdminCredential -DeploymentDataJSONPath .\contoso-depploymentdata.json
```

In dit voorbeeld wordt de accountreferenties van de servicebeheerder wordt gevraagd om veilig en Start AzsReadinessChecker controleert dat de Azure-account en Azure Active Directory zijn geldig voor een AAD-implementatie waarbij AzureCloud en TenantName worden gelezen uit de implementatiegegevens JSON-bestand gegenereerd voor de implementatie.

### <a name="example-validate-azure-registration"></a>Voorbeeld: Valideren van de registratie van Azure

```PowerShell
$registrationCredential = Get-Credential -Message "Enter Credentials for Subscription Owner e.g. subscriptionowner@contoso.onmicrosoft.com"
$subscriptionID = "<subscription ID"
# Supported values for the <environment name> parameter are AzureCloud, AzureChinaCloud or AzureUSGovernment depending which Azure subscription you are using.
Start-AzsReadinessChecker -RegistrationAccount $registrationCredential -RegistrationSubscriptionID $subscriptionID -AzureEnvironment "<environment name>"
```

In dit voorbeeld wordt de Abonnementhouder referenties wordt gevraagd om veilig en Start AzsReadinessChecker vervolgens worden gevalideerd tegen het opgegeven account en abonnement om ervoor te zorgen dat deze kan worden gebruikt voor registratie in Azure Stack. 

### <a name="example-validate-azure-registration-with-deployment-data-deployment-team"></a>Voorbeeld: Registratie van Azure met implementatiegegevens (implementatieteam) valideren

```PowerShell
$registrationCredential = Get-Credential -Message "Enter Credentials for Subscription Owner e.g. subscriptionowner@contoso.onmicrosoft.com"
$subscriptionID = "<subscription ID>"
Start-AzsReadinessChecker -RegistrationAccount $registrationCredential -RegistrationSubscriptionID $subscriptionID -DeploymentDataJSONPath .\contoso-deploymentdata.json
```

In dit voorbeeld wordt de Abonnementhouder referenties wordt gevraagd om veilig en Start AzsReadinessChecker vervolgens worden gevalideerd tegen het opgegeven account en abonnement om ervoor te zorgen dat deze kan worden gebruikt voor registratie in Azure Stack waar extra details zijn lezen van het implementatiebestand-JSON gegenereerd voor de implementatie.

### <a name="example-importexport-pfx-package"></a>Voorbeeld: Import/Export-PFX-pakket

```PowerShell
$password = Read-Host -Prompt "Enter PFX Password" -AsSecureString
Start-AzsReadinessChecker -PfxPassword $password -PfxPath .\certificates\ssl.pfx -ExportPFXPath .\certificates\ssl_new.pfx
```

In dit voorbeeld wordt wordt het PFX-wachtwoord gevraagd om veilig. Het bestand ssl.pfx worden geïmporteerd in het certificaatarchief van lokale computer en opnieuw met hetzelfde wachtwoord wordt geëxporteerd en opgeslagen als ssl_new.pfx.  Deze procedure is voor gebruik wanneer certificaatvalidatie gemarkeerd dat geen persoonlijke sleutel niet de lokale computer kenmerk is ingesteld heeft, de certificaatketen verbroken is, niet van belang certificaten aanwezig zijn in de PFX zijn of de certificaatketen in de verkeerde volgorde is.

### <a name="example-view-validation-report-deployment-support"></a>Voorbeeld: Validatierapport weergeven (ondersteuning voor implementatie)

```PowerShell
Start-AzsReadinessChecker -ReportPath Contoso-AzsReadinessReport.json
```

In dit voorbeeld wordt met het team implementatie of ondersteuning ontvangen van het rapport over de gereedheid van de klant (Contoso) en Start AzsReadinessChecker gebruiken om de status van de validatie-uitvoeringen die Contoso uitgevoerd weer te geven.

### <a name="example-view-validation-report-summary-for-certificate-validation-only-deployment-and-support"></a>Voorbeeld: Validatierapport weergeven samenvatting voor het certificaat validatie alleen (implementatie en ondersteuning)

```PowerShell
Start-AzsReadinessChecker -ReportPath Contoso-AzsReadinessReport.json -ReportSections Certificate -Summary
```

In dit voorbeeld wordt met het team implementatie of ondersteuning voor het rapport over de gereedheid van de klant Contoso ontvangt en Start AzsReadinessChecker gebruiken om een samengevatte status van de certificaat-validatie uitvoeringen die Contoso uitgevoerd weer te geven.

## <a name="required-parameters"></a>Vereiste parameters

> -RegionName

Hiermee geeft u de naam van de Azure Stack-implementatie de regio.
|  |  |
|----------------------------|--------------|
|Type:                       |Reeks        |
|Positie:                   |met de naam         |
|Standaardwaarde:              |Geen          |
|Pijpleidinginvoer accepteren:      |False         |
|Jokertekens accepteren: |False         |

> -FQDN

Hiermee geeft u de Azure Stack-implementatie externe FQDN, ook de alias als ExternalFQDN en ExternalDomainName.
|  |  |
|----------------------------|--------------|
|Type:                       |Reeks        |
|Positie:                   |met de naam         |
|Standaardwaarde:              |ExternalFQDN, ExternalDomainName |
|Pijpleidinginvoer accepteren:      |False         |
|Jokertekens accepteren: |False         |

> -IdentitySystem

Hiermee geeft u de Azure Stack-implementatie Identiteitssysteem geldige waarden, AAD of ADFS, voor Azure Active Directory en Active Directory Federated Services respectievelijk.
|  |  |
|----------------------------|--------------|
|Type:                       |Reeks        |
|Positie:                   |met de naam         |
|Standaardwaarde:              |Geen          |
|Geldige waarden:               |'AAD','ADFS'  |
|Pijpleidinginvoer accepteren:      |False         |
|Jokertekens accepteren: |False         |

> -PfxPassword

Hiermee geeft u het wachtwoord dat is gekoppeld aan de certificaat-PFX-bestanden.
|  |  |
|----------------------------|---------|
|Type:                       |SecureString |
|Positie:                   |met de naam    |
|Standaardwaarde:              |Geen     |
|Pijpleidinginvoer accepteren:      |False    |
|Jokertekens accepteren: |False    |

> -PaaSCertificates

Hiermee geeft u de hash-tabel met paden en wachtwoorden voor PaaS-certificaten.
|  |  |
|----------------------------|---------|
|Type:                       |Hash-tabel |
|Positie:                   |met de naam    |
|Standaardwaarde:              |Geen     |
|Pijpleidinginvoer accepteren:      |False    |
|Jokertekens accepteren: |False    |

> -DeploymentDataJSONPath

Hiermee geeft u de Azure Stack-implementatie gegevens JSON-configuratiebestand. Dit bestand is gegenereerd voor de implementatie.
|  |  |
|----------------------------|---------|
|Type:                       |Reeks   |
|Positie:                   |met de naam    |
|Standaardwaarde:              |Geen     |
|Pijpleidinginvoer accepteren:      |False    |
|Jokertekens accepteren: |False    |

> -PfxPath

Hiermee geeft u het pad naar een problematische certificaat dat is vereist voor importeren/exporteren routine om op te lossen, zoals aangegeven door de certificaatvalidatie van het in dit hulpprogramma.
|  |  |
|----------------------------|---------|
|Type:                       |Reeks   |
|Positie:                   |met de naam    |
|Standaardwaarde:              |Geen     |
|Pijpleidinginvoer accepteren:      |False    |
|Jokertekens accepteren: |False    |

> -ExportPFXPath  

Hiermee geeft u het doelpad voor het resulterende PFX-bestand van import/export-routine.  
|  |  |
|----------------------------|---------|
|Type:                       |Reeks   |
|Positie:                   |met de naam    |
|Standaardwaarde:              |Geen     |
|Pijpleidinginvoer accepteren:      |False    |
|Jokertekens accepteren: |False    |

> -Onderwerp

Hiermee geeft u een geordende woordenlijst van het onderwerp voor het genereren van de aanvraag van het certificaat.
|  |  |
|----------------------------|---------|
|Type:                       |OrderedDictionary   |
|Positie:                   |met de naam    |
|Standaardwaarde:              |Geen     |
|Pijpleidinginvoer accepteren:      |False    |
|Jokertekens accepteren: |False    |

> -RequestType

Hiermee geeft u de SAN-type van de certificaataanvraag. Geldige waarden MultipleCSR, SingleCSR.

- *MultipleCSR* genereert meerdere certificaataanvragen, één voor elke service.
- *SingleCSR* genereert een certificaataanvraag voor alle services.

|  |  |
|----------------------------|---------|
|Type:                       |Reeks   |
|Positie:                   |met de naam    |
|Standaardwaarde:              |Geen     |
|Geldige waarden:               |'MultipleCSR','SingleCSR' |
|Pijpleidinginvoer accepteren:      |False    |
|Jokertekens accepteren: |False    |

> -OutputRequestPath

Hiermee geeft u het doelpad voor aanvraag certificaatbestanden, map moet bestaan.
|  |  |
|----------------------------|---------|
|Type:                       |Reeks   |
|Positie:                   |met de naam    |
|Standaardwaarde:              |Geen     |
|Pijpleidinginvoer accepteren:      |False    |
|Jokertekens accepteren: |False    |

> -AADServiceAdministrator

Hiermee geeft u een beheerder van Azure Active Directory Service moet worden gebruikt voor Azure Stack-implementatie.
|  |  |
|----------------------------|---------|
|Type:                       |PSCredential   |
|Positie:                   |met de naam    |
|Standaardwaarde:              |Geen     |
|Pijpleidinginvoer accepteren:      |False    |
|Jokertekens accepteren: |False    |

> -AADDirectoryTenantName

Hiermee geeft u de Azure Active Directory-naam moet worden gebruikt voor Azure Stack-implementatie.
|  |  |
|----------------------------|---------|
|Type:                       |Reeks   |
|Positie:                   |met de naam    |
|Standaardwaarde:              |Geen     |
|Pijpleidinginvoer accepteren:      |False    |
|Jokertekens accepteren: |False    |

> -AzureEnvironment

Hiermee geeft u het exemplaar van Azure-Services met de accounts, mappen en abonnementen worden gebruikt voor Azure Stack-implementatie en de registratie.
|  |  |
|----------------------------|---------|
|Type:                       |Reeks   |
|Positie:                   |met de naam    |
|Standaardwaarde:              |Geen     |
|Geldige waarden:               |'AzureCloud','AzureChinaCloud','AzureUSGovernment' |
|Pijpleidinginvoer accepteren:      |False    |
|Jokertekens accepteren: |False    |

> -RegistrationAccount

Hiermee geeft u de registratie-Account moet worden gebruikt voor registratie in Azure Stack.
|  |  |
|----------------------------|---------|
|Type:                       |Reeks   |
|Positie:                   |met de naam    |
|Standaardwaarde:              |Geen     |
|Pijpleidinginvoer accepteren:      |False    |
|Jokertekens accepteren: |False    |

> -RegistrationSubscriptionID

Hiermee geeft u de registratie abonnements-ID moet worden gebruikt voor registratie in Azure Stack.
|  |  |
|----------------------------|---------|
|Type:                       |GUID     |
|Positie:                   |met de naam    |
|Standaardwaarde:              |Geen     |
|Pijpleidinginvoer accepteren:      |False    |
|Jokertekens accepteren: |False    |

> -ReportPath

Hiermee geeft u pad voor gereedheid rapport, standaard ingesteld op de huidige map en de standaard rapportnaam.
|  |  |
|----------------------------|---------|
|Type:                       |Reeks   |
|Positie:                   |met de naam    |
|Standaardwaarde:              |Alle      |
|Pijpleidinginvoer accepteren:      |False    |
|Jokertekens accepteren: |False    |

## <a name="optional-parameters"></a>Optionele parameters

> -CertificatePath

Hiermee geeft u het pad waarin alleen het certificaat is vereist certificaat mappen aanwezig zijn.

Vereiste mappen voor Azure Stack-implementatie met Azure Active Directory-identiteitssysteem zijn:

ACSBlob, ACSQueue, ACSTable, Admin-Portal, ARM-beheerder ARM publiek, KeyVault, KeyVaultInternal, openbare-Portal

Map voor Azure Stack-implementatie met Active Directory Federation Services-identiteitssysteem zijn vereist:

ACSBlob, ACSQueue, ACSTable, ADFS, Admin Portal, ARM Admin, ARM Public, Graph, KeyVault, KeyVaultInternal, Public Portal

|  |  |
|----------------------------|---------|
|Type:                       |Reeks   |
|Positie:                   |met de naam    |
|Standaardwaarde:              |. \Certificates |
|Pijpleidinginvoer accepteren:      |False    |
|Jokertekens accepteren: |False    |

> -IncludePaaS  

Hiermee geeft u als PaaS-services/hostnamen moeten worden toegevoegd aan het certificaat aanvragen.

|  |  |
|----------------------------|------------------|
|Type:                       |SwitchParameter   |
|Positie:                   |met de naam             |
|Standaardwaarde:              |False             |
|Pijpleidinginvoer accepteren:      |False             |
|Jokertekens accepteren: |False             |

> -ReportSections

Hiermee geeft u op of het rapport alleen samenvatting weergeven Details weggelaten.
|  |  |
|----------------------------|---------|
|Type:                       |Reeks   |
|Positie:                   |met de naam    |
|Standaardwaarde:              |Alle      |
|Geldige waarden:               |'Certificate','AzureRegistration','AzureIdentity','Jobs','All' |
|Pijpleidinginvoer accepteren:      |False    |
|Jokertekens accepteren: |False    |

> -Overzicht

Hiermee geeft u op of het rapport alleen samenvatting weergeven Details weggelaten.
|  |  |
|----------------------------|------------------|
|Type:                       |SwitchParameter   |
|Positie:                   |met de naam             |
|Standaardwaarde:              |False             |
|Pijpleidinginvoer accepteren:      |False             |
|Jokertekens accepteren: |False             |

> -CleanReport

Hiermee verwijdert u de vorige uitvoering en validatie geschiedenis en validaties schrijft naar een nieuw rapport.
|  |  |
|----------------------------|------------------|
|Type:                       |SwitchParameter   |
|Aliases:                    |cf                |
|Positie:                   |met de naam             |
|Standaardwaarde:              |False             |
|Pijpleidinginvoer accepteren:      |False             |
|Jokertekens accepteren: |False             |

> -OutputPath

Hiermee geeft u een aangepast pad om op te slaan van de gereedheid van de JSON-rapport en uitgebreide logboekbestand.  Als het pad niet al bestaat, probeert het hulpprogramma te maken van de map.

|  |  |
|----------------------------|------------------|
|Type:                       |Reeks            |
|Positie:                   |met de naam             |
|Standaardwaarde:              |$ENV:TEMP\AzsReadinessChecker  |
|Pijpleidinginvoer accepteren:      |False             |
|Jokertekens accepteren: |False             |

> -Controleer of

Als u wordt gevraagd om bevestiging voordat u de cmdlet uitvoert.
|  |  |
|----------------------------|------------------|
|Type:                       |SwitchParameter   |
|Aliases:                    |cf                |
|Positie:                   |met de naam             |
|Standaardwaarde:              |False             |
|Pijpleidinginvoer accepteren:      |False             |
|Jokertekens accepteren: |False             |

> -WhatIf

Geeft aan wat er gebeuren zou als de cmdlet wordt uitgevoerd. De cmdlet wordt niet uitgevoerd.
|  |  |
|----------------------------|------------------|
|Type:                       |SwitchParameter   |
|Aliases:                    |wi                |
|Positie:                   |met de naam             |
|Standaardwaarde:              |False             |
|Pijpleidinginvoer accepteren:      |False             |
|Jokertekens accepteren: |False             |
