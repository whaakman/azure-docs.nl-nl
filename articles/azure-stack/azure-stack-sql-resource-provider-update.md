---
title: Bijwerken van de Azure-Stack SQL-resourceprovider | Microsoft Docs
description: Meer informatie over hoe u de Azure-Stack SQL-resourceprovider kunt bijwerken.
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
ms.date: 06/11/2018
ms.author: jeffgilb
ms.reviewer: jeffgo
ms.openlocfilehash: ac5073d1abc32b7598a869750f9c5a801559e9e6
ms.sourcegitcommit: 301855e018cfa1984198e045872539f04ce0e707
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/19/2018
ms.locfileid: "36264074"
---
# <a name="update-the-sql-resource-provider"></a>Bijwerken van de SQL-resourceprovider

*Van toepassing op: Azure Stack geïntegreerd systemen.*

Een nieuwe resourceprovider voor SQL kan worden vrijgegeven wanneer Azure Stack wordt bijgewerkt naar een nieuwe build. Hoewel de bestaande adapter werken blijft, wordt u aangeraden zo snel mogelijk naar de laatste build bijwerken.

>[!IMPORTANT]
>In de volgorde waarin die ze worden vrijgegeven, moet u updates installeren. U kunt de versies niet overslaan. Raadpleeg de lijst met versies in [implementeert de vereisten van de provider resource](.\azure-stack-sql-resource-provider-deploy.md#prerequisites).

## <a name="overview"></a>Overzicht

Voor het bijwerken van de resourceprovider, gebruiken de *UpdateSQLProvider.ps1* script. Dit script is opgenomen in het downloaden van de nieuwe SQL-resourceprovider. De updateproces is vergelijkbaar met het proces waarmee [implementeert de bronprovider](.\azure-stack-sql-resource-provider-deploy.md). Het script bijwerken gebruikt dezelfde argumenten als het script DeploySqlProvider.ps1 en moet u geeft certificaatgegevens.

### <a name="update-script-processes"></a>Bijwerken van de script-processen

De *UpdateSQLProvider.ps1* script maakt een nieuwe virtuele machine (VM) met de meest recente resource provider-code.

>[!NOTE]
>Het is raadzaam dat u de installatiekopie van het meest recente Windows Server 2016 Core uit het beheer van de Marketplace downloaden. Als u een update installeert moet, schakelt u een **één** MSU-pakket in het pad van de lokale afhankelijkheid. Het script mislukt als er meer dan één MSU-bestand op deze locatie.

Na de *UpdateSQLProvider.ps1* script maakt een nieuwe virtuele machine, wordt het script de volgende instellingen van de oude VM-provider wordt gemigreerd:

* Database-informatie
* hosting-servergegevens
* vereiste DNS-record

### <a name="update-script-powershell-example"></a>Voorbeeld van PowerShell-script bijwerken

U kunt bewerken en voer het volgende script vanaf een verhoogde PowerShell ISE. Vergeet niet de accountgegevens en -wachtwoorden nodig zijn voor uw omgeving te wijzigen.

> [!NOTE]
> Het bijwerkproces is alleen van toepassing op systemen met Azure-Stack geïntegreerd.

```powershell
# Install the AzureRM.Bootstrapper module and set the profile.
Install-Module -Name AzureRm.BootStrapper -Force
Use-AzureRmProfile -Profile 2017-03-09-profile

# Use the NetBIOS name for the Azure Stack domain. On the Azure Stack SDK, the default is AzureStack but this might have been changed at installation.
$domain = "AzureStack"

# For integrated systems, use the IP address of one of the ERCS virtual machines.
$privilegedEndpoint = "AzS-ERCS01"

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
  -DefaultSSLCertificatePassword $PfxPass `
  -DependencyFilesLocalPath $tempDir\cert `

 ```

## <a name="updatesqlproviderps1-parameters"></a>UpdateSQLProvider.ps1 parameters

U kunt de volgende parameters vanaf de opdrachtregel opgeven wanneer u het script uitvoeren. Als u dit niet, of als een parametervalidatie mislukt, wordt u gevraagd de vereiste parameters bevatten.

| Parameternaam | Beschrijving | Opmerking of de standaardwaarde |
| --- | --- | --- |
| **CloudAdminCredential** | De referentie voor de beheerder van de cloud, nodig voor toegang tot de bevoegde eindpunt. | _Vereist_ |
| **AzCredential** | De referenties voor de Azure-Stack-beheerdersaccount. Gebruik dezelfde referenties die u gebruikt voor de implementatie van Azure-Stack. | _Vereist_ |
| **VMLocalCredential** | De referenties voor het lokale administrator-account van de SQL-resourceprovider VM. | _Vereist_ |
| **PrivilegedEndpoint** | De IP-adres of de DNS-naam van het bevoegde eindpunt. |  _Vereist_ |
| **DependencyFilesLocalPath** | U moet ook het PFX-certificaatbestand in deze map geplaatst. | _Optioneel voor één knooppunt, maar verplicht voor meerdere knooppunten_ |
| **DefaultSSLCertificatePassword** | Het wachtwoord voor het pfx-certificaat. | _Vereist_ |
| **MaxRetryCount** | Het aantal keren dat u wilt opnieuw proberen aan elke als er een storing optreedt.| 2 |
| **RetryDuration** |De time-interval tussen nieuwe pogingen, in seconden. | 120 |
| **Verwijderen** | Hiermee verwijdert u de resourceprovider en alle bijbehorende resources. | Nee |
| **Fouten opsporen-modus** | Voorkomt dat automatisch opschonen bij fouten. | Nee |

## <a name="next-steps"></a>Volgende stappen

[De SQL-resourceprovider onderhouden](azure-stack-sql-resource-provider-maintain.md)
