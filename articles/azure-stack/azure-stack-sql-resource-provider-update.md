---
title: Bijwerken van de Azure Stack-SQL-resourceprovider | Microsoft Docs
description: Meer informatie over hoe u de Azure Stack-SQL-resourceprovider kunt bijwerken.
services: azure-stack
documentationCenter: ''
author: jeffgilb
manager: femila
editor: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/29/2019
ms.author: jeffgilb
ms.reviewer: jiahan
ms.lastreviewed: 03/11/2019
ms.openlocfilehash: 5238c60493820fe6d784049da9862b4347e563c4
ms.sourcegitcommit: 22ad896b84d2eef878f95963f6dc0910ee098913
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/29/2019
ms.locfileid: "58650906"
---
# <a name="update-the-sql-resource-provider"></a>Bijwerken van de SQL-resourceprovider

*Van toepassing op: Azure Stack-geïntegreerde systemen.*

Een nieuwe SQL-resourceprovider kan worden vrijgegeven wanneer Azure Stack wordt bijgewerkt naar een nieuwe build. Hoewel de bestaande resourceprovider werken blijft, kunt u het beste bijwerken naar de nieuwste build zo snel mogelijk. 

Beginnen met de SQL-resource provider 1.1.33.0 versie, updates zijn cumulatief en hoeft niet te worden geïnstalleerd in de volgorde waarin ze zijn vrijgegeven; Als u vanaf versie 1.1.24.0 of hoger. Bijvoorbeeld als u versie 1.1.24.0 van de SQL-resourceprovider uitvoert, kunt klikt u vervolgens u upgraden naar versie 1.1.33.0 of later zonder dat eerst versie 1.1.30.0 installeren. Als u wilt controleren op beschikbare resource provider versies en de versie van Azure Stack op worden ondersteund, verwijzen naar de lijst met versies in [implementeert de vereisten van de provider resource](./azure-stack-sql-resource-provider-deploy.md#prerequisites).

Voor het bijwerken van de resourceprovider, gebruikt u de *UpdateSQLProvider.ps1* script. Met dit script wordt geleverd bij het downloaden van de nieuwe SQL-resourceprovider. De updateproces is vergelijkbaar met het proces waarmee [implementeren van de resourceprovider](./azure-stack-sql-resource-provider-deploy.md). Het updatescript maakt gebruik van dezelfde argumenten als het script DeploySqlProvider.ps1 en u moet gegevens opgeven.

 > [!IMPORTANT]
 > Vóór de upgrade van de resourceprovider, neem de releaseopmerkingen voor meer informatie over nieuwe functionaliteit, correcties en bekende problemen die invloed kunnen zijn op uw implementatie.

## <a name="update-script-processes"></a>Update-script-processen

De *UpdateSQLProvider.ps1* script maakt u een nieuwe virtuele machine (VM) met de meest recente code van de resource-provider.

> [!NOTE]
> Het is raadzaam dat u de meest recente installatiekopie van Windows Server 2016 Core uit het beheer van de Marketplace downloaden. Als u een update installeert wilt, kunt u plaatsen een **één** MSU-pakket in het afhankelijkheidspad van de lokale. Het script mislukken als er meer dan één MSU-bestand op deze locatie.

Na de *UpdateSQLProvider.ps1* script maakt u een nieuwe virtuele machine, het script worden de volgende instellingen gemigreerd van de oude VM-provider:

* database-informatie
* serverinformatie die als host fungeert
* vereiste DNS-record

## <a name="update-script-parameters"></a>Scriptparameters bijwerken

U kunt de volgende parameters vanaf de opdrachtregel opgeven tijdens het uitvoeren van de **UpdateSQLProvider.ps1** PowerShell-script. Als u dit niet doet, of als een parametervalidatie mislukt, wordt u gevraagd de vereiste parameters opgeven.

| Parameternaam | Description | Opmerking of de standaardwaarde |
| --- | --- | --- |
| **CloudAdminCredential** | De referentie voor de beheerder van de cloud, die nodig zijn voor toegang tot het eindpunt van de bevoegdheden. | _Vereist_ |
| **AzCredential** | De referenties voor het beheerdersaccount van de Azure Stack-service. Gebruik de dezelfde referenties die u gebruikt voor het implementeren van Azure Stack. | _Vereist_ |
| **VMLocalCredential** | De referenties voor het lokale administrator-account van de SQL-resourceprovider VM. | _Vereist_ |
| **PrivilegedEndpoint** | De IP-adres of de DNS-naam van het eindpunt van de bevoegdheden. |  _Vereist_ |
| **AzureEnvironment** | De Azure-omgeving van het serviceaccount van de beheerder die u gebruikt voor het implementeren van Azure Stack. Alleen vereist voor Azure AD-implementaties. Omgevingsnamen van de ondersteunde zijn **AzureCloud**, **AzureUSGovernment**, of als een Azure AD-China **AzureChinaCloud**. | AzureCloud |
| **DependencyFilesLocalPath** | U moet uw certificaat-pfx-bestand ook plaatsen in deze map. | _Optioneel voor één knooppunt, maar verplicht voor meerdere knooppunten_ |
| **DefaultSSLCertificatePassword** | Het wachtwoord voor het pfx-certificaat. | _Vereist_ |
| **MaxRetryCount** | Het aantal keren dat die u wilt dat elke bewerking wordt uitgevoerd als er een fout is.| 2 |
| **RetryDuration** |De time-outinterval tussen nieuwe pogingen in seconden. | 120 |
| **Verwijderen** | Hiermee verwijdert u de resourceprovider en alle bijbehorende resources. | Nee |
| **DebugMode** | Hiermee voorkomt u dat bij fout automatisch op te schonen. | Nee |

## <a name="update-script-powershell-example"></a>Voorbeeld van PowerShell-script bijwerken
Hier volgt een voorbeeld van het gebruik van de *UpdateSQLProvider.ps1* script dat u vanuit een PowerShell-console met verhoogde bevoegdheid uitvoeren kunt. Zorg ervoor dat de variabele informatie en wachtwoorden indien nodig wijzigen:  

> [!NOTE]
> Het bijwerkproces is alleen van toepassing op systemen met integratie van Azure Stack.

```powershell
# Install the AzureRM.Bootstrapper module, set the profile and install the AzureStack module
# Note that this might not be the most currently available version of Azure Stack PowerShell.
Install-Module -Name AzureRm.BootStrapper -Force
Use-AzureRmProfile -Profile 2018-03-01-hybrid -Force
Install-Module -Name AzureStack -RequiredVersion 1.6.0

# Use the NetBIOS name for the Azure Stack domain. On the Azure Stack SDK, the default is AzureStack but this might have been changed at installation.
$domain = "AzureStack"

# For integrated systems, use the IP address of one of the ERCS virtual machines.
$privilegedEndpoint = "AzS-ERCS01"

# Provide the Azure environment used for deploying Azure Stack. Required only for Azure AD deployments. Supported values for the <environment name> parameter are AzureCloud, AzureChinaCloud or AzureUSGovernment depending which Azure subscription you are using. 
$AzureEnvironment = "<EnvironmentName>"

# Point to the directory where the resource provider installation files were extracted.
$tempDir = 'C:\TEMP\SQLRP'

# The service administrator account (this can be Azure AD or AD FS).
$serviceAdmin = "admin@mydomain.onmicrosoft.com"
$AdminPass = ConvertTo-SecureString "P@ssw0rd1" -AsPlainText -Force
$AdminCreds = New-Object System.Management.Automation.PSCredential ($serviceAdmin, $AdminPass)

# Set the credentials for the new resource provider VM.
$vmLocalAdminPass = ConvertTo-SecureString "P@ssw0rd1" -AsPlainText -Force
$vmLocalAdminCreds = New-Object System.Management.Automation.PSCredential ("sqlrpadmin", $vmLocalAdminPass)

# Add the cloudadmin credential required for privileged endpoint access.
$CloudAdminPass = ConvertTo-SecureString "P@ssw0rd1" -AsPlainText -Force
$CloudAdminCreds = New-Object System.Management.Automation.PSCredential ("$domain\cloudadmin", $CloudAdminPass)

# Change the following as appropriate.
$PfxPass = ConvertTo-SecureString "P@ssw0rd1" -AsPlainText -Force

# Change directory to the folder where you extracted the installation files.
# Then adjust the endpoints.
. $tempDir\UpdateSQLProvider.ps1 -AzCredential $AdminCreds `
  -VMLocalCredential $vmLocalAdminCreds `
  -CloudAdminCredential $cloudAdminCreds `
  -PrivilegedEndpoint $privilegedEndpoint `
  -AzureEnvironment $AzureEnvironment `
  -DefaultSSLCertificatePassword $PfxPass `
  -DependencyFilesLocalPath $tempDir\cert 

 ```

## <a name="next-steps"></a>Volgende stappen

[Onderhouden van de SQL-resourceprovider](azure-stack-sql-resource-provider-maintain.md)
