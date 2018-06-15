---
title: Start AzsReadinessChecker cmdlet-verwijzing | Microsoft Docs
description: PowerShell-cmdlet help voor de module Azure Stack gereedheid Checker.
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
ms.date: 05/08/2018
ms.author: brenduns
ms.reviewer: ''
ms.openlocfilehash: 8481fbd6c7cb82b34070f9bc8cc6d7f3f4b2518c
ms.sourcegitcommit: d98d99567d0383bb8d7cbe2d767ec15ebf2daeb2
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/10/2018
ms.locfileid: "33937864"
---
# <a name="start-azsreadinesschecker-cmdlet-reference"></a>Start AzsReadinessChecker cmdlet-verwijzing

Module: Microsoft.AzureStack.ReadinessChecker

Deze module bevat slechts één cmdlet.  Deze cmdlet voert een of meer voorafgaand aan de implementatie of vooraf onderhoud functies voor Azure-Stack.

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
De **Start AzsReadinessChecker** cmdlet valideert certificaten, Azure-accounts, Azure-abonnementen en Azure Active Directory's. Voer het validatieproces voordat u Azure-Stack implementeert of voor Azure-Stack bewerkingen zoals het geheim rotatie onderhoud uit. De cmdlet kan ook worden gebruikt voor het genereren van aanvragen voor het ondertekenen van certificaten voor infrastructuur-certificaten en eventueel PaaS-certificaten.  Ten slotte de cmdlet opnieuw inpakken PFX-certificaten voor het oplossen van veelvoorkomende problemen met pakketten.

## <a name="examples"></a>Voorbeelden
**Voorbeeld: Aanvraag voor Certificaatondertekening genereren**

```PowerShell
$regionName = 'east'
$externalFQDN = 'azurestack.contoso.com'
$subjectHash = [ordered]@{"OU"="AzureStack";"O"="Microsoft";"L"="Redmond";"ST"="Washington";"C"="US"}
Start-AzsReadinessChecker -regionName $regionName -externalFQDN $externalFQDN -subjectName $subjectHash -IdentitySystem ADFS -requestType MultipleCSR
```

In dit voorbeeld Start AzsReadinessChecker genereert meerdere aanvragen (Certificaatondertekening) voor de certificaten die geschikt is voor een Azure-Stack van AD FS-implementatie met de naam van een gebied van 'Oost' en 'azurestack.contoso.com' externe FQDN

**Voorbeeld: Certificaten valideren**
```PowerShell
$password = Read-Host -Prompt "Enter PFX Password" -AsSecureString
Start-AzsReadinessChecker -CertificatePath .\Certificates\ -PfxPassword $password -RegionName east -FQDN azurestack.contoso.com -IdentitySystem AAD
```

In dit voorbeeld het PFX-wachtwoord wordt gevraagd om veilig en Start AzsReadinessChecker controleert de relatieve map 'Certificaten' voor certificaten geldig voor een AAD-implementatie met de naam van een gebied van 'Oost' en 'azurestack.contoso.com' externe FQDN 

**Voorbeeld: Certificaten met implementatiegegevens (implementatie en ondersteuning) valideren**
```PowerShell
$password = Read-Host -Prompt "Enter PFX Password" -AsSecureString
Start-AzsReadinessChecker -CertificatePath .\Certificates\ -PfxPassword $password -DeploymentDataJSONPath .\deploymentdata.json
```
In dit voorbeeld implementatie en ondersteuning voor het PFX-wachtwoord wordt gevraagd veilig en Start AzsReadinessChecker controleert de relatieve map 'Certificaten' voor certificaten geldig voor een implementatie waarbij identiteit, regio en externe FQDN worden gelezen uit de implementatie gegevens JSON-bestand is gegenereerd voor de implementatie. 

**Voorbeeld: PaaS certificaten valideren**
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

In dit voorbeeld wordt een hashtabel geconstrueerd met paden en wachtwoorden voor elke PaaS-certificaat. Certificaten kunnen worden weggelaten. Start AzsReadinessChecker controleert elk PFX-pad bestaat en deze met de regio Oost' en de externe FQDN-naam 'azurestack.contoso.com' worden gevalideerd.

**Voorbeeld: PaaS-certificaten met implementatiegegevens valideren**
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

In dit voorbeeld wordt een hashtabel geconstrueerd met paden en wachtwoorden voor elke PaaS-certificaat. Certificaten kunnen worden weggelaten. Start AzsReadinessChecker controleert elk PFX-pad bestaat en valideert deze met de regio en externe FQDN lezen uit het implementatiebestand-JSON genereren voor de implementatie. 

**Voorbeeld: Azure identiteit valideren**
```PowerShell
$serviceAdminCredential = Get-Credential -Message "Enter Credentials for Service Administrator of Azure Active Directory Tenant e.g. serviceadmin@contoso.onmicrosoft.com"
Start-AzsReadinessChecker -AADServiceAdministrator $serviceAdminCredential -AzureEnvironment AzureCloud -AzureDirectoryTenantName azurestack.contoso.com
```

In dit voorbeeld de servicebeheerder accountreferenties wordt gevraagd om veilig en Start AzsReadinessChecker controleert dat de Azure-account en Azure Active Directory zijn geldig voor een AAD-implementatie met de naam van een tenant-map van 'azurestack.contoso.com'


**Voorbeeld: Azure identiteit met implementatiegegevens (ondersteuning voor implementatie) valideren**
```PowerSHell
$serviceAdminCredential = Get-Credential -Message "Enter Credentials for Service Administrator of Azure Active Directory Tenant e.g. serviceadmin@contoso.onmicrosoft.com"
Start-AzsReadinessChecker -AADServiceAdministrator $serviceAdminCredential -DeploymentDataJSONPath .\contoso-depploymentdata.json
```

In dit voorbeeld de servicebeheerder accountreferenties wordt gevraagd om veilig en Start AzsReadinessChecker controleert dat de Azure-account en Azure Active Directory zijn geldig voor de implementatie van een AAD waar AzureCloud en TenantName worden gelezen uit de implementatiegegevens JSON-bestand is gegenereerd voor de implementatie.


**Voorbeeld: Azure registratie valideert**
```PowerShell
$registrationCredential = Get-Credential -Message "Enter Credentials for Subscription Owner"e.g. subscriptionowner@contoso.onmicrosoft.com"
$subscriptionID = "f7c26209-cd2d-4625-86ba-724ebeece794"
Start-AzsReadinessChecker -RegistrationAccount $registrationCredential -RegistrationSubscriptionID $subscriptionID -AzureEnvironment AzureCloud
```

In dit voorbeeld wordt de eigenaar van abonnement referenties wordt gevraagd om veilig en Start AzsReadinessChecker voert validatie met het opgegeven account en abonnement zodat deze kan worden gebruikt voor registratie bij de Azure-Stack. 


**Voorbeeld: Valideren Azure registratie bij de implementatiegegevens (implementatieteam)**
```PowerShell
$registrationCredential = Get-Credential -Message "Enter Credentials for Subscription Owner"e.g. subscriptionowner@contoso.onmicrosoft.com"
$subscriptionID = "f7c26209-cd2d-4625-86ba-724ebeece794"
Start-AzsReadinessChecker -RegistrationAccount $registrationCredential -RegistrationSubscriptionID $subscriptionID -DeploymentDataJSONPath .\contoso-deploymentdata.json
```

In dit voorbeeld wordt de eigenaar van abonnement referenties wordt gevraagd om veilig en Start AzsReadinessChecker voert validatie met het opgegeven account en abonnement zodat deze kan worden gebruikt voor registratie bij de Azure-Stack waar extra details zijn lezen van het implementatiebestand-JSON gegenereerd voor de implementatie.

**Voorbeeld: Import/Export PFX-pakket**
```PowerShell
$password = Read-Host -Prompt "Enter PFX Password" -AsSecureString
Start-AzsReadinessChecker -PfxPassword $password -PfxPath .\certificates\ssl.pfx -ExportPFXPath .\certificates\ssl_new.pfx
```

In dit voorbeeld wordt wordt het PFX-wachtwoord gevraagd om veilig. Het bestand ssl.pfx worden geïmporteerd in het certificaatarchief van de lokale computer en opnieuw met hetzelfde wachtwoord wordt geëxporteerd en opgeslagen als ssl_new.pfx.  Deze procedure is voor gebruik wanneer validatie van het servercertificaat gemarkeerd dat een persoonlijke sleutel geen kenmerkset van de lokale computer heeft, de certificaatketen verbroken is, irrelevante certificaten aanwezig in de PFX zijn of de certificaatketen in de verkeerde volgorde is.


**Voorbeeld: Bekijk validatierapport (ondersteuning voor implementatie)**
```PowerShell
Start-AzsReadinessChecker -ReportPath Contoso-AzsReadinessReport.json
```

In dit voorbeeld is het team implementatie of ondersteuning het rapport gereedheid van de klant (Contoso) ontvangt, en Start AzsReadinessChecker gebruiken om de status van de validatie-uitvoeringen die Contoso uitgevoerd weer te geven.

**Voorbeeld: Overzicht met eigenschappen voor certificaat validatie alleen (implementatie en ondersteuning) Validatierapport weergeven**
```PowerShell
Start-AzsReadinessChecker -ReportPath Contoso-AzsReadinessReport.json -ReportSections Certificate -Summary
```

In dit voorbeeld is het team implementatie of ondersteuning het rapport gereedheid van de klant Contoso ontvangt, en Start AzsReadinessChecker gebruiken om een samengevatte status van het certificaat validatie uitvoeringen die Contoso uitgevoerd weer te geven.



## <a name="required-parameters"></a>Vereiste parameters
> -RegionName

Hiermee geeft u de naam van de implementatie van de Azure-Stack regio.
|  |  |
|----------------------------|--------------|
|Type:                       |Tekenreeks        |
|Positie:                   |Met de naam         |
|Standaardwaarde:              |Geen          |
|Pijpleidinginvoer accepteren:      |False         |
|Jokertekens accepteren: |False         |

> -FQDN-NAAM    

FQDN-naam van de implementatie van de Azure-Stack externe, ook een alias als ExternalFQDN en ExternalDomainName.
|  |  |
|----------------------------|--------------|
|Type:                       |Tekenreeks        |
|Positie:                   |Met de naam         |
|Standaardwaarde:              |ExternalFQDN, ExternalDomainName |
|Pijpleidinginvoer accepteren:      |False         |
|Jokertekens accepteren: |False         |

 

> -IdentitySystem    

Bevat van de implementatie van de Azure-Stack identiteitsbeheersysteem geldige waarden zijn, AAD of ADFS, respectievelijk voor Azure Active Directory en Active Directory Federated Services.
|  |  |
|----------------------------|--------------|
|Type:                       |Tekenreeks        |
|Positie:                   |Met de naam         |
|Standaardwaarde:              |Geen          |
|Geldige waarden:               |AAD', 'ADFS'  |
|Pijpleidinginvoer accepteren:      |False         |
|Jokertekens accepteren: |False         |

> -PfxPassword    

Hiermee geeft u het wachtwoord voor PFX-certificaatbestanden.
|  |  |
|----------------------------|---------|
|Type:                       |SecureString |
|Positie:                   |Met de naam    |
|Standaardwaarde:              |Geen     |
|Pijpleidinginvoer accepteren:      |False    |
|Jokertekens accepteren: |False    |

> -PaaSCertificates

Hiermee geeft u de hash-tabel met de paden en wachtwoorden voor PaaS-certificaten.
|  |  |
|----------------------------|---------|
|Type:                       |Hash-tabel |
|Positie:                   |Met de naam    |
|Standaardwaarde:              |Geen     |
|Pijpleidinginvoer accepteren:      |False    |
|Jokertekens accepteren: |False    |

> -DeploymentDataJSONPath

Hiermee geeft u het Azure-Stack implementatie gegevens JSON-configuratiebestand. Dit bestand wordt gegenereerd voor de implementatie.
|  |  |
|----------------------------|---------|
|Type:                       |Tekenreeks   |
|Positie:                   |Met de naam    |
|Standaardwaarde:              |Geen     |
|Pijpleidinginvoer accepteren:      |False    |
|Jokertekens accepteren: |False    |

> -PfxPath

Hiermee geeft u het pad naar een problematisch certificaat dat is vereist voor importeren/exporteren routine om op te lossen, zoals aangegeven door de validatie van het certificaat in dit hulpprogramma.
|  |  |
|----------------------------|---------|
|Type:                       |Tekenreeks   |
|Positie:                   |Met de naam    |
|Standaardwaarde:              |Geen     |
|Pijpleidinginvoer accepteren:      |False    |
|Jokertekens accepteren: |False    |

> -ExportPFXPath  

Hiermee geeft u het doelpad voor het resulterende PFX-bestand van importeren/exporteren routine.  
|  |  |
|----------------------------|---------|
|Type:                       |Tekenreeks   |
|Positie:                   |Met de naam    |
|Standaardwaarde:              |Geen     |
|Pijpleidinginvoer accepteren:      |False    |
|Jokertekens accepteren: |False    |

> -Onderwerp

Hiermee geeft u een geordende woordenlijst van het onderwerp voor het genereren van de aanvraag certificaat.
|  |  |
|----------------------------|---------|
|Type:                       |OrderedDictionary   |
|Positie:                   |Met de naam    |
|Standaardwaarde:              |Geen     |
|Pijpleidinginvoer accepteren:      |False    |
|Jokertekens accepteren: |False    |

> -RequestType

Hiermee wordt de SAN-type van de certificaataanvraag. Geldige waarden MultipleCSR, SingleCSR.
- *MultipleCSR* genereert meerdere aanvragen voor certificaten, één voor elke service.
- *SingleCSR* genereert een certificaataanvraag voor alle services.   

|  |  |
|----------------------------|---------|
|Type:                       |Tekenreeks   |
|Positie:                   |Met de naam    |
|Standaardwaarde:              |Geen     |
|Geldige waarden:               |MultipleCSR', 'SingleCSR' |
|Pijpleidinginvoer accepteren:      |False    |
|Jokertekens accepteren: |False    |

> -OutputRequestPath

Hiermee geeft u het doelpad voor aanvraag certificaatbestanden, map moet bestaan.
|  |  |
|----------------------------|---------|
|Type:                       |Tekenreeks   |
|Positie:                   |Met de naam    |
|Standaardwaarde:              |Geen     |
|Pijpleidinginvoer accepteren:      |False    |
|Jokertekens accepteren: |False    |

> -AADServiceAdministrator

Hiermee geeft u een Azure Active Directory-servicebeheerder om te worden gebruikt voor implementatie van de Azure-Stack.
|  |  |
|----------------------------|---------|
|Type:                       |PSCredential   |
|Positie:                   |Met de naam    |
|Standaardwaarde:              |Geen     |
|Pijpleidinginvoer accepteren:      |False    |
|Jokertekens accepteren: |False    |

> -AADDirectoryTenantName

Hiermee geeft u de Azure Active Directory-naam moet worden gebruikt voor de implementatie van Azure-Stack.
|  |  |
|----------------------------|---------|
|Type:                       |Tekenreeks   |
|Positie:                   |Met de naam    |
|Standaardwaarde:              |Geen     |
|Pijpleidinginvoer accepteren:      |False    |
|Jokertekens accepteren: |False    |

> -AzureEnvironment

Hiermee geeft u het exemplaar van Azure-Services met de accounts, mappen en abonnementen moet worden gebruikt voor de implementatie van de Azure-Stack en registratie.
|  |  |
|----------------------------|---------|
|Type:                       |Tekenreeks   |
|Positie:                   |Met de naam    |
|Standaardwaarde:              |Geen     |
|Geldige waarden:               |'AzureCloud', 'AzureChinaCloud', 'AzureGermanCloud' |
|Pijpleidinginvoer accepteren:      |False    |
|Jokertekens accepteren: |False    |

> -RegistrationAccount

Hiermee geeft u de registratie-Account moet worden gebruikt voor registratie bij de Azure-Stack.
|  |  |
|----------------------------|---------|
|Type:                       |Tekenreeks   |
|Positie:                   |Met de naam    |
|Standaardwaarde:              |Geen     |
|Pijpleidinginvoer accepteren:      |False    |
|Jokertekens accepteren: |False    |

> -RegistrationSubscriptionID

Hiermee geeft u de registratie abonnements-ID moet worden gebruikt voor registratie bij de Azure-Stack.
|  |  |
|----------------------------|---------|
|Type:                       |GUID     |
|Positie:                   |Met de naam    |
|Standaardwaarde:              |Geen     |
|Pijpleidinginvoer accepteren:      |False    |
|Jokertekens accepteren: |False    |

> -ReportPath

Hiermee geeft u pad voor gereedheid van rapport, wordt standaard ingesteld op de huidige map en standaard rapportnaam.
|  |  |
|----------------------------|---------|
|Type:                       |Tekenreeks   |
|Positie:                   |Met de naam    |
|Standaardwaarde:              |Alle      |
|Pijpleidinginvoer accepteren:      |False    |
|Jokertekens accepteren: |False    |



## <a name="optional-parameters"></a>Optionele parameters
> -CertificatePath     

Geeft het pad waaronder alleen het certificaat is vereist certificaat mappen aanwezig zijn.

Vereiste mappen voor de implementatie van de Stack van Azure met Azure Active Directory-identiteitssysteem zijn:

ACSBlob, ACSQueue, ACSTable, -beheerportal, ARM-beheerder, ARM openbaar, KeyVault, KeyVaultInternal, openbare-Portal

Map voor Azure-Stack-implementatie met Active Directory Federation Services identiteitsbeheersysteem zijn vereist:

ACSBlob, ACSQueue, ACSTable, ADFS, -beheerportal, ARM-beheer, ARM openbaar, grafiek, KeyVault, KeyVaultInternal, openbare-Portal


|  |  |
|----------------------------|---------|
|Type:                       |Tekenreeks   |
|Positie:                   |Met de naam    |
|Standaardwaarde:              |. \Certificates |
|Pijpleidinginvoer accepteren:      |False    |
|Jokertekens accepteren: |False    |


> -IncludePaaS  

Hiermee geeft u aan als PaaS services/hostnamen moeten worden toegevoegd aan de certificaat-aanvragen.


|  |  |
|----------------------------|------------------|
|Type:                       |SwitchParameter   |
|Positie:                   |Met de naam             |
|Standaardwaarde:              |False             |
|Pijpleidinginvoer accepteren:      |False             |
|Jokertekens accepteren: |False             |


> -ReportSections        

Hiermee geeft u op of voor het rapport alleen samenvatting weergeven worden weggelaten detail.
|  |  |
|----------------------------|---------|
|Type:                       |Tekenreeks   |
|Positie:                   |Met de naam    |
|Standaardwaarde:              |Alle      |
|Geldige waarden:               |'Certificaat', 'AzureRegistration', 'AzureIdentity', 'Taken', 'All' |
|Pijpleidinginvoer accepteren:      |False    |
|Jokertekens accepteren: |False    |


> -Overzicht 

Hiermee geeft u op of voor het rapport alleen samenvatting weergeven worden weggelaten detail.
|  |  |
|----------------------------|------------------|
|Type:                       |SwitchParameter   |
|Positie:                   |Met de naam             |
|Standaardwaarde:              |False             |
|Pijpleidinginvoer accepteren:      |False             |
|Jokertekens accepteren: |False             |


> -CleanReport  

Hiermee verwijdert u de vorige uitvoering en validatie geschiedenis en validaties schrijft naar een nieuw rapport.
|  |  |
|----------------------------|------------------|
|Type:                       |SwitchParameter   |
|Aliassen:                    |CF                |
|Positie:                   |Met de naam             |
|Standaardwaarde:              |False             |
|Pijpleidinginvoer accepteren:      |False             |
|Jokertekens accepteren: |False             |


> -OutputPath    

Hiermee geeft u een aangepast pad voor het opslaan van de gereedheid van de JSON-rapport en uitgebreide logboekbestand.  Als het pad niet al bestaat, probeert het hulpprogramma te maken van de map.
|  |  |
|----------------------------|------------------|
|Type:                       |Tekenreeks            |
|Positie:                   |Met de naam             |
|Standaardwaarde:              |$ENV: TEMP\AzsReadinessChecker  |
|Pijpleidinginvoer accepteren:      |False             |
|Jokertekens accepteren: |False             |


> -Bevestigen  

Als u wordt gevraagd om bevestiging voordat u de cmdlet uitvoert.
|  |  |
|----------------------------|------------------|
|Type:                       |SwitchParameter   |
|Aliassen:                    |CF                |
|Positie:                   |Met de naam             |
|Standaardwaarde:              |False             |
|Pijpleidinginvoer accepteren:      |False             |
|Jokertekens accepteren: |False             |


> -WhatIf  

Hiermee wordt weergegeven wat er gebeuren zou als de cmdlet wordt uitgevoerd. De cmdlet wordt niet uitgevoerd.
|  |  |
|----------------------------|------------------|
|Type:                       |SwitchParameter   |
|Aliassen:                    |Wi                |
|Positie:                   |Met de naam             |
|Standaardwaarde:              |False             |
|Pijpleidinginvoer accepteren:      |False             |
|Jokertekens accepteren: |False             |

 
