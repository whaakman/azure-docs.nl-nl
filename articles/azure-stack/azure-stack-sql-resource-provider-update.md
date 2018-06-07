---
title: Met behulp van SQL-databases op Azure-Stack | Microsoft Docs
description: Meer informatie over hoe u SQL-databases kan implementeren als een service op Azure-Stack en de snelle stappen voor het implementeren van de SQL Server resource provider-adapter.
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
ms.date: 05/23/2018
ms.author: jeffgilb
ms.reviewer: jeffgo
ms.openlocfilehash: fd1c2241fe22dc35ceb09e0ba3650fa0000a77b1
ms.sourcegitcommit: 680964b75f7fff2f0517b7a0d43e01a9ee3da445
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/01/2018
ms.locfileid: "34603613"
---
# <a name="update-the-sql-resource-provider-adapter"></a>Bijwerken van de SQL resource provider-adapter
Een nieuwe SQL resource provider-adapter kan worden vrijgegeven wanneer Azure Stack-builds worden bijgewerkt. Terwijl de bestaande adapter werken blijft, wordt u aangeraden zo snel mogelijk naar de laatste build bijwerken. Updates moeten worden geïnstalleerd in volgorde: u kunt de versies niet overslaan (overzicht van de versies in [implementeert de vereisten van de provider resource](.\azure-stack-sql-resource-provider-deploy.md#prerequisites)).

Bijwerken van de resourceprovider die u gebruikt de *UpdateSQLProvider.ps1* script. Het proces is vergelijkbaar met het proces dat wordt gebruikt voor het installeren van een resourceprovider, zoals beschreven in de [implementeert de bronprovider](.\azure-stack-sql-resource-provider-deploy.md) artikel. Het script is opgenomen in het downloaden van de resourceprovider.

De *UpdateSQLProvider.ps1* script maakt een nieuwe virtuele machine met de meest recente code van de resource-provider en de instellingen van de oude virtuele machine naar de nieuwe virtuele machine wordt gemigreerd. De instellingen die worden gemigreerd database en het hosten van server-gegevens bevatten en de vereiste DNS-record.

Het script vereist gebruik van dezelfde argumenten die worden beschreven voor het script DeploySqlProvider.ps1. Geef het certificaat hier ook. 

Het is raadzaam dat u de installatiekopie van het meest recente Windows Server 2016 Core uit het beheer van de Marketplace downloaden. Als u een update installeert moet, kunt u één plaatsen. MSU-pakket in het pad van de lokale afhankelijkheid. Als meer dan één. MSU-bestand wordt gevonden, mislukt het script.

Hieronder volgt een voorbeeld van de *UpdateSQLProvider.ps1* script dat u vanuit de PowerShell-prompt uitvoeren kunt. Zorg ervoor dat de accountgegevens en wachtwoorden zo nodig wijzigen: 

> [!NOTE]
> Het updateproces geldt alleen voor geïntegreerde systemen.

```powershell
# Install the AzureRM.Bootstrapper module and set the profile.
Install-Module -Name AzureRm.BootStrapper -Force
Use-AzureRmProfile -Profile 2017-03-09-profile

# Use the NetBIOS name for the Azure Stack domain. On the Azure Stack SDK, the default is AzureStack but could have been changed at install time.
$domain = "AzureStack"

# For integrated systems, use the IP address of one of the ERCS virtual machines
$privilegedEndpoint = "AzS-ERCS01"

# Point to the directory where the resource provider installation files were extracted.
$tempDir = 'C:\TEMP\SQLRP'

# The service admin account (can be Azure AD or AD FS).
$serviceAdmin = "admin@mydomain.onmicrosoft.com"
$AdminPass = ConvertTo-SecureString "P@ssw0rd1" -AsPlainText -Force
$AdminCreds = New-Object System.Management.Automation.PSCredential ($serviceAdmin, $AdminPass)

# Set credentials for the new Resource Provider VM.
$vmLocalAdminPass = ConvertTo-SecureString "P@ssw0rd1" -AsPlainText -Force
$vmLocalAdminCreds = New-Object System.Management.Automation.PSCredential ("sqlrpadmin", $vmLocalAdminPass)

# And the cloudadmin credential required for privileged endpoint access.
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
  -DependencyFilesLocalPath $tempDir\cert
 ```

## <a name="updatesqlproviderps1-parameters"></a>UpdateSQLProvider.ps1 parameters
U kunt deze parameters opgeven op de opdrachtregel. Als u dit niet doet, of als er parametervalidatie mislukt, wordt u gevraagd de vereiste parameters bevatten.

| Parameternaam | Beschrijving | Opmerking of de standaardwaarde |
| --- | --- | --- |
| **CloudAdminCredential** | De referentie voor de beheerder van de cloud, nodig voor toegang tot de bevoegde eindpunt. | _Vereist_ |
| **AzCredential** | De referenties voor de Azure-Stack-admin-serviceaccount. Gebruik dezelfde referenties die u gebruikt voor de implementatie van Azure-Stack. | _Vereist_ |
| **VMLocalCredential** | De referenties voor het lokale administrator-account van de SQL-resourceprovider VM. | _Vereist_ |
| **PrivilegedEndpoint** | De IP-adres of de DNS-naam van het bevoegde eindpunt. |  _Vereist_ |
| **DependencyFilesLocalPath** | Het PFX-certificaatbestand moet in deze map ook worden geplaatst. | _Optionele_ (_verplichte_ voor meerdere knooppunten) |
| **DefaultSSLCertificatePassword** | Het wachtwoord voor het pfx-certificaat. | _Vereist_ |
| **MaxRetryCount** | Het aantal keren dat u wilt opnieuw proberen aan elke als er een storing optreedt.| 2 |
| **RetryDuration** |De time-interval tussen nieuwe pogingen, in seconden. | 120 |
| **Verwijderen** | Hiermee verwijdert u de resourceprovider en alle bijbehorende resources (Zie de volgende opmerkingen). | Nee |
| **Fouten opsporen-modus** | Voorkomt dat automatisch opschonen bij fouten. | Nee |


## <a name="next-steps"></a>Volgende stappen

[De SQL-resourceprovider onderhouden](azure-stack-sql-resource-provider-maintain.md)
