---
title: Onderhoud van de resourceprovider van MySQL in Azure Stack | Microsoft Docs
description: Meer informatie over hoe u de service MySQL resource provider in Azure Stack kunt onderhouden.
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
ms.date: 10/16/2018
ms.author: jeffgilb
ms.reviewer: quying
ms.openlocfilehash: 76a164ffcf918ebedcf4647f24a61ca3a271e967
ms.sourcegitcommit: 6361a3d20ac1b902d22119b640909c3a002185b3
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/17/2018
ms.locfileid: "49361912"
---
# <a name="mysql-resource-provider-maintenance-operations"></a>MySQL onderhoud resourceproviderbewerkingen

De MySQL-resourceprovider wordt uitgevoerd op een vergrendelde virtuele machine. Om in te schakelen onderhoudsbewerkingen, moet u de beveiliging van de virtuele machine bijwerken. Om dit te doen met behulp van het principe van minimale bevoegdheden, kunt u PowerShell alleen Enough Administration (JEA) eindpunt DBAdapterMaintenance. Het installatiepakket voor de resource-provider bevat een script voor deze bewerking.

## <a name="update-the-virtual-machine-operating-system"></a>Werk het besturingssysteem van de virtuele machine

Omdat de resourceprovider wordt uitgevoerd op een *gebruiker* virtuele machine, moet u de vereiste patches en updates van toepassing wanneer deze zijn vrijgegeven. U kunt de Windows update-pakketten die worden geleverd als onderdeel van de patch-en updatecyclus updates toepassen op de virtuele machine.

Update voor de provider van virtuele machine met behulp van een van de volgende methoden:

- Installeer de meest recente resource provider-pakket met behulp van een momenteel gecorrigeerde installatiekopie van Windows Server 2016 Core.
- Een Windows Update-pakket installeren tijdens de installatie van of bijwerken bij de resourceprovider.

## <a name="update-the-virtual-machine-windows-defender-definitions"></a>De virtuele machine Windows Defender-definities bijwerken

Als u wilt de Defender-definities bijwerken, de volgende stappen uit:

1. Download de Windows Defender-definities via bijwerken [Windows Defender de definitie](https://www.microsoft.com/en-us/wdsi/definitions).

    Schuif omlaag naar 'Handmatig downloaden en installeren van de definities' op de pagina definities. Download het bestand van de 64-bits 'Windows Defender Antivirus voor Windows 10 en Windows 8.1 '.

    U kunt ook [deze directe koppeling](https://go.microsoft.com/fwlink/?LinkID=121721&arch=x64) op het bestand fpam fe.exe downloaden/uitvoeren.

2. Open een PowerShell-sessie naar MySQL resource provider-adapter van de virtuele machine onderhoud eindpunt.

3. Kopieer het bestand van de update definities voor de resource provider adapter VM met behulp van de eindpunt-sessie van onderhoud.

4. Voer op de onderhoud PowerShell-sessie de _Update DBAdapterWindowsDefenderDefinitions_ opdracht.

5. Nadat u de definities worden geïnstalleerd, wordt aangeraden dat u het bestand van de update definities met behulp van verwijderen de _Remove-ItemOnUserDrive)_ opdracht.

**PowerShell-voorbeeldscript voor het bijwerken van definities.**

U kunt bewerken en voer het volgende script als u wilt de Defender-definities bijwerken. Waarden in het script vervangen door waarden van uw omgeving.

```powershell
# Set credentials for the local admin on the resource provider VM.
$vmLocalAdminPass = ConvertTo-SecureString "<local admin user password>" -AsPlainText -Force
$vmLocalAdminUser = "<local admin user name>"
$vmLocalAdminCreds = New-Object System.Management.Automation.PSCredential `
    ($vmLocalAdminUser, $vmLocalAdminPass)

# Provide the public IP address for the adapter VM.
$databaseRPMachine  = "<RP VM IP address>"
$localPathToDefenderUpdate = "C:\DefenderUpdates\mpam-fe.exe"

# Download Windows Defender update definitions file from https://www.microsoft.com/en-us/wdsi/definitions.  
Invoke-WebRequest -Uri 'https://go.microsoft.com/fwlink/?LinkID=121721&arch=x64' `
    -Outfile $localPathToDefenderUpdate  

# Create a session to the maintenance endpoint.
$session = New-PSSession -ComputerName $databaseRPMachine `
    -Credential $vmLocalAdminCreds -ConfigurationName DBAdapterMaintenance

# Copy the defender update file to the adapter virtual machine.
Copy-Item -ToSession $session -Path $localPathToDefenderUpdate `
     -Destination "User:\"

# Install the update definitions.
Invoke-Command -Session $session -ScriptBlock `
    {Update-AzSDBAdapterWindowsDefenderDefinition -DefinitionsUpdatePackageFile "User:\mpam-fe.exe"}

# Cleanup the definitions package file and session.
Invoke-Command -Session $session -ScriptBlock `
    {Remove-AzSItemOnUserDrive -ItemPath "User:\mpam-fe.exe"}
$session | Remove-PSSession

```

## <a name="secrets-rotation"></a>Geheimen, rotatie

*Deze instructies zijn alleen van toepassing op Azure Stack-geïntegreerde systemen.*

Wanneer met behulp van de resourceproviders SQL- en MySQL met Azure Stack-systemen geïntegreerde, is de Azure Stack-operators verantwoordelijk voor het draaien van de volgende resource provider infrastructuur geheimen om ervoor te zorgen dat ze niet verloopt:

- Externe SSL-certificaat [opgegeven tijdens de implementatie](azure-stack-pki-certs.md).
- Resource provider VM lokale wachtwoord van de beheerder opgegeven tijdens de implementatie.
- Wachtwoord van resource provider diagnostische gebruiker (dbadapterdiag).

### <a name="powershell-examples-for-rotating-secrets"></a>PowerShell-voorbeelden voor het draaien van geheimen

**Alle geheimen op hetzelfde moment wijzigen.**

```powershell
.\SecretRotationMySQLProvider.ps1 `
    -Privilegedendpoint $Privilegedendpoint `
    -CloudAdminCredential $cloudCreds `
    -AzCredential $adminCreds `
    –DiagnosticsUserPassword $passwd `
    -DependencyFilesLocalPath $certPath `
    -DefaultSSLCertificatePassword $certPasswd `  
    -VMLocalCredential $localCreds

```

**De diagnostische gebruikerswachtwoord wijzigen.**

```powershell
.\SecretRotationMySQLProvider.ps1 `
    -Privilegedendpoint $Privilegedendpoint `
    -CloudAdminCredential $cloudCreds `
    -AzCredential $adminCreds `
    –DiagnosticsUserPassword  $passwd

```

**Wijzig het wachtwoord van virtuele machine lokale administrator-account.**

```powershell
.\SecretRotationMySQLProvider.ps1 `
    -Privilegedendpoint $Privilegedendpoint `
    -CloudAdminCredential $cloudCreds `
    -AzCredential $adminCreds `
    -VMLocalCredential $localCreds

```

**Wijzig het wachtwoord voor het SSL-certificaat.**

```powershell
.\SecretRotationMySQLProvider.ps1 `
    -Privilegedendpoint $Privilegedendpoint `
    -CloudAdminCredential $cloudCreds `
    -AzCredential $adminCreds `
    -DependencyFilesLocalPath $certPath `
    -DefaultSSLCertificatePassword $certPasswd

```

### <a name="secretrotationmysqlproviderps1-parameters"></a>SecretRotationMySQLProvider.ps1 parameters

|Parameter|Beschrijving|
|-----|-----|
|AzCredential|Azure Stack-servicebeheerder-accountreferenties.|
|CloudAdminCredential|Azure Stack cloud domein account beheerreferenties.|
|PrivilegedEndpoint|Bevoegde eindpunt voor toegang tot de Get-AzureStackStampInformation.|
|DiagnosticsUserPassword|Diagnostische gegevens van het wachtwoord voor gebruikersaccount.|
|VMLocalCredential|Het lokale beheerdersaccount op de VM MySQLAdapter.|
|DefaultSSLCertificatePassword|Standaard SSL-certificaat (* pfx) wachtwoord.|
|DependencyFilesLocalPath|Afhankelijkheid bestanden lokaal pad.|
|     |     |

### <a name="known-issues"></a>Bekende problemen

**Probleem:**<br>
De logboeken voor geheimen, rotatie worden niet automatisch verzameld als het script geheime rotatie is mislukt wanneer deze wordt uitgevoerd.

**Tijdelijke oplossing:**<br>
Gebruik de cmdlet Get-AzsDBAdapterLogs alle de resource provider om Logboeken te verzamelen, met inbegrip van AzureStack.DatabaseAdapter.SecretRotation.ps1_*.log, opgeslagen in C:\Logs.

## <a name="collect-diagnostic-logs"></a>Logboeken met diagnostische gegevens verzamelen

Voor het verzamelen van Logboeken van de vergrendelde virtuele machine, kunt u het eindpunt van de PowerShell alleen Enough Administration (JEA) DBAdapterDiagnostics. Dit eindpunt biedt de volgende opdrachten:

- **Get-AzsDBAdapterLog**. Met deze opdracht maakt u een zip-pakket van de resource provider diagnostische logboeken en slaat het bestand op schijf van de gebruiker van de sessie. U kunt deze opdracht zonder parameters uitvoert en de laatste vier uur van de logboeken worden verzameld.

- **Remove-AzsDBAdapterLog**. Met deze opdracht verwijdert bestaande log-pakketten op de VM-resourceprovider.

### <a name="endpoint-requirements-and-process"></a>Eindpunt-vereisten en proces

Wanneer een resourceprovider wordt geïnstalleerd of bijgewerkt, wordt het gebruikersaccount dbadapterdiag gemaakt. U gebruikt dit account voor het verzamelen van diagnostische logboeken.

>[!NOTE]
>Het wachtwoord voor het dbadapterdiag is hetzelfde als het wachtwoord voor de lokale beheerder op de virtuele machine die gemaakt tijdens een implementatie van de provider of de update.

Gebruik de _DBAdapterDiagnostics_ opdrachten, een externe PowerShell-sessie met de resource provider virtuele machine maken en uitvoeren van de **Get-AzsDBAdapterLog** opdracht.

U de tijdsduur voor logboekverzameling instellen met behulp van de **FromDate** en **ToDate** parameters. Als u een of beide van deze parameters niet opgeeft, worden de volgende standaardwaarden worden gebruikt:

* FromDate staat aan vier uur vóór de huidige tijd.
* ToDate is de huidige tijd.

**PowerShell-voorbeeldscript voor het verzamelen van Logboeken.**

Het volgende script toont hoe u logboeken met diagnostische gegevens verzamelen over de VM-resourceprovider.

```powershell
# Create a new diagnostics endpoint session.
$databaseRPMachineIP = '<RP VM IP address>'
$diagnosticsUserName = 'dbadapterdiag'
$diagnosticsUserPassword = '<Enter Diagnostic password>'
$diagCreds = New-Object System.Management.Automation.PSCredential `
        ($diagnosticsUserName, (ConvertTo-SecureString -String $diagnosticsUserPassword -AsPlainText -Force))
$session = New-PSSession -ComputerName $databaseRPMachineIP -Credential $diagCreds
        -ConfigurationName DBAdapterDiagnostics

# Sample that captures logs from the previous hour.
$fromDate = (Get-Date).AddHours(-1)
$dateNow = Get-Date
$sb = {param($d1,$d2) Get-AzSDBAdapterLog -FromDate $d1 -ToDate $d2}
$logs = Invoke-Command -Session $session -ScriptBlock $sb -ArgumentList $fromDate,$dateNow

# Copy the logs to the user drive.
$sourcePath = "User:\{0}" -f $logs
$destinationPackage = Join-Path -Path (Convert-Path '.') -ChildPath $logs
Copy-Item -FromSession $session -Path $sourcePath -Destination $destinationPackage

# Cleanup the logs.
$cleanup = Invoke-Command -Session $session -ScriptBlock {Remove-AzsDBAdapterLog}
# Close the session.
$session | Remove-PSSession

```

## <a name="next-steps"></a>Volgende stappen

[Verwijderen van de MySQL-resourceprovider](azure-stack-mysql-resource-provider-remove.md)
