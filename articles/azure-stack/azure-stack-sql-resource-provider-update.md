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
ms.date: 11/15/2018
ms.author: jeffgilb
ms.reviewer: quying
ms.openlocfilehash: ad1a89c69d34dbb48b87a75778e18c9a995f749a
ms.sourcegitcommit: 7804131dbe9599f7f7afa59cacc2babd19e1e4b9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/17/2018
ms.locfileid: "51853554"
---
# <a name="update-the-sql-resource-provider"></a>Bijwerken van de SQL-resourceprovider

*Is van toepassing op: Azure Stack-geïntegreerde systemen.*

Een nieuwe SQL-resourceprovider kan worden vrijgegeven wanneer Azure Stack wordt bijgewerkt naar een nieuwe build. Hoewel de bestaande adapter werken blijft, kunt u het beste bijwerken naar de nieuwste build zo snel mogelijk.

> [!IMPORTANT]
> U moet updates installeren in de volgorde waarin die ze zijn vrijgegeven. U kunt de versies niet overslaan. Raadpleeg de lijst met versies in [implementeert de vereisten van de provider resource](.\azure-stack-sql-resource-provider-deploy.md#prerequisites).

## <a name="overview"></a>Overzicht

Voor het bijwerken van de resourceprovider, gebruikt u de *UpdateSQLProvider.ps1* script. Met dit script wordt geleverd bij het downloaden van de nieuwe SQL-resourceprovider. De updateproces is vergelijkbaar met het proces waarmee [implementeren van de resourceprovider](.\azure-stack-sql-resource-provider-deploy.md). Het updatescript maakt gebruik van dezelfde argumenten als het script DeploySqlProvider.ps1 en u moet gegevens opgeven.

### <a name="update-script-processes"></a>Update-script-processen

De *UpdateSQLProvider.ps1* script maakt u een nieuwe virtuele machine (VM) met de meest recente code van de resource-provider.

> [!NOTE]
> Het is raadzaam dat u de meest recente installatiekopie van Windows Server 2016 Core uit het beheer van de Marketplace downloaden. Als u een update installeert wilt, kunt u plaatsen een **één** MSU-pakket in het afhankelijkheidspad van de lokale. Het script mislukken als er meer dan één MSU-bestand op deze locatie.

Na de *UpdateSQLProvider.ps1* script maakt u een nieuwe virtuele machine, het script worden de volgende instellingen gemigreerd van de oude VM-provider:

* database-informatie
* serverinformatie die als host fungeert
* vereiste DNS-record

### <a name="update-script-powershell-example"></a>Voorbeeld van PowerShell-script bijwerken

U kunt bewerken en voer het volgende script vanaf een verhoogde PowerShell ISE. 

Vergeet niet om de accountgegevens en wachtwoorden als nodig zijn voor uw omgeving te wijzigen.

> [!NOTE]
> Het bijwerkproces is alleen van toepassing op systemen met integratie van Azure Stack.

```powershell
# Install the AzureRM.Bootstrapper module, set the profile and install the AzureStack module
Install-Module -Name AzureRm.BootStrapper -Force
Use-AzureRmProfile -Profile 2018-03-01-hybrid -Force
Install-Module -Name AzureStack -RequiredVersion 1.5.0

# Use the NetBIOS name for the Azure Stack domain. On the Azure Stack SDK, the default is AzureStack but this might have been changed at installation.
$domain = "AzureStack"

# For integrated systems, use the IP address of one of the ERCS virtual machines.
$privilegedEndpoint = "AzS-ERCS01"

# Provide the Azure environment used for deploying Azure Stack. Required only for Azure AD deployments. Supported environment names are AzureCloud, AzureUSGovernment, or AzureChinaCloud. 
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
  -DependencyFilesLocalPath $tempDir\cert `

 ```

## <a name="updatesqlproviderps1-parameters"></a>UpdateSQLProvider.ps1 parameters

U kunt de volgende parameters vanaf de opdrachtregel opgeven wanneer u het script uitvoert. Als u dit niet doet, of als een parametervalidatie mislukt, wordt u gevraagd de vereiste parameters opgeven.

| Parameternaam | Beschrijving | Opmerking of de standaardwaarde |
| --- | --- | --- |
| **CloudAdminCredential** | De referentie voor de beheerder van de cloud, die nodig zijn voor toegang tot het eindpunt van de bevoegdheden. | _Vereist_ |
| **AzCredential** | De referenties voor het beheerdersaccount van de Azure Stack-service. Gebruik de dezelfde referenties die u gebruikt voor het implementeren van Azure Stack. | _Vereist_ |
| **VMLocalCredential** | De referenties voor het lokale administrator-account van de SQL-resourceprovider VM. | _Vereist_ |
| **PrivilegedEndpoint** | De IP-adres of de DNS-naam van het eindpunt van de bevoegdheden. |  _Vereist_ |
| **Azure-omgeving** | De Azure-omgeving van het serviceaccount van de beheerder die u gebruikt voor het implementeren van Azure Stack. Alleen vereist voor Azure AD-implementaties. Omgevingsnamen van de ondersteunde zijn **AzureCloud**, **AzureUSGovernment**, of als een Azure AD-China **AzureChinaCloud**. | AzureCloud |
| **DependencyFilesLocalPath** | U moet uw certificaat-pfx-bestand ook plaatsen in deze map. | _Optioneel voor één knooppunt, maar verplicht voor meerdere knooppunten_ |
| **DefaultSSLCertificatePassword** | Het wachtwoord voor het pfx-certificaat. | _Vereist_ |
| **MaxRetryCount** | Het aantal keren dat die u wilt dat elke bewerking wordt uitgevoerd als er een fout is.| 2 |
| **RetryDuration** |De time-outinterval tussen nieuwe pogingen in seconden. | 120 |
| **Verwijderen** | Hiermee verwijdert u de resourceprovider en alle bijbehorende resources. | Nee |
| **Fouten opsporen-modus** | Hiermee voorkomt u dat bij fout automatisch op te schonen. | Nee |

## <a name="next-steps"></a>Volgende stappen

[Onderhouden van de SQL-resourceprovider](azure-stack-sql-resource-provider-maintain.md)
