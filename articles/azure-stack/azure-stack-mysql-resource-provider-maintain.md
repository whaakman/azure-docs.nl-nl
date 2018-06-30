---
title: Onderhoud van de resourceprovider MySQL op Azure-Stack | Microsoft Docs
description: Meer informatie over hoe u de MySQL resource provider-service op Azure-Stack kunt onderhouden.
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
ms.date: 06/29/2018
ms.author: jeffgilb
ms.reviewer: jeffgo
ms.openlocfilehash: bc1c96d2f027d459ca20fccb70cd94ac9e5cae94
ms.sourcegitcommit: 5892c4e1fe65282929230abadf617c0be8953fd9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/29/2018
ms.locfileid: "37130135"
---
# <a name="mysql-resource-provider-maintenance-operations"></a>MySQL resource provider onderhoudsbewerkingen

De MySQL-resourceprovider wordt uitgevoerd op een vergrendelde virtuele machine. Om in te schakelen onderhoudsbewerkingen, moet u de beveiliging van de virtuele machine bijwerken. Om dit te doen met behulp van het principe van minimale bevoegdheden, kunt u PowerShell net genoeg Administration (JEA) eindpunt DBAdapterMaintenance. Het installatiepakket voor de resource-provider bevat een script voor deze bewerking.

## <a name="update-the-virtual-machine-operating-system"></a>Werk het besturingssysteem van de virtuele machine

Omdat de resourceprovider wordt uitgevoerd op een *gebruiker* virtuele machine, moet u de vereiste patches en updates van toepassing wanneer deze worden vrijgegeven. U kunt de Windows update-pakketten die worden geleverd als onderdeel van de patch-en updatecyclus updates toepassen op de virtuele machine.

Werk de provider virtuele machine met een van de volgende methoden:

- Installeer de meest recente resource provider-pakket met de installatiekopie van een momenteel patches Windows Server 2016 Core.
- Een Windows Update-pakket installeren tijdens de installatie van of bijwerken naar de resourceprovider.

## <a name="update-the-virtual-machine-windows-defender-definitions"></a>De virtuele machine Windows Defender-definities bijwerken

Volg deze stappen voor het bijwerken van de definities Defender:

1. Download de Windows Defender-definities vanuit bijwerken [Windows Defender Definition](https://www.microsoft.com/en-us/wdsi/definitions).

    Schuif omlaag naar 'Handmatig downloaden en installeren van de definities' op de pagina definities. Download het bestand van de 64-bits 'Windows Defender Antivirus voor Windows 10 en Windows 8.1 '.

    U kunt ook [deze directe koppeling](https://go.microsoft.com/fwlink/?LinkID=121721&arch=x64) op het bestand fpam fe.exe downloaden/uitvoeren.

2. Open een PowerShell-sessie met MySQL resource provider-adapter van de virtuele machine onderhoud eindpunt.

3. Kopieer het bestand van de update definities aan de resource provider adapter VM die gebruikmaakt van de onderhoudsmodus endpoint-sessie.

4. Voer op de onderhoud PowerShell-sessie de _Update DBAdapterWindowsDefenderDefinitions_ opdracht.

5. Nadat u de definities hebt geïnstalleerd, wordt aangeraden het updatebestand definities te verwijderen via de _verwijderen ItemOnUserDrive)_ opdracht.

**Voorbeeld van PowerShell-script voor het bijwerken van definities.**

U kunt bewerken en voer het volgende script om de Defender-definities bijwerken. Waarden in het script vervangen door waarden van uw omgeving.

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

## <a name="secrets-rotation"></a>Geheimen draaien

*Deze instructies zijn alleen van toepassing op Azure Stack geïntegreerde systemen versie 1804 en Later. Probeer niet om te draaien geheimen op de pre-1804 versies van Azure-Stack.*

Wanneer met behulp van de resourceproviders SQL en MySQL met Azure-Stack geïntegreerd systemen, kunt u de volgende infrastructuur (implementatie) geheimen draaien:

- Externe SSL-certificaat [opgegeven tijdens de implementatie](azure-stack-pki-certs.md).
- Het resource provider VM lokale administrator-account opgegeven wachtwoord tijdens de implementatie.
- Het wachtwoord resource provider diagnostische gebruiker (dbadapterdiag).

### <a name="powershell-examples-for-rotating-secrets"></a>PowerShell-voorbeelden voor roulatie van geheimen

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

**Het gebruikerswachtwoord diagnostische wijzigen.**

```powershell
.\SecretRotationMySQLProvider.ps1 `
    -Privilegedendpoint $Privilegedendpoint `
    -CloudAdminCredential $cloudCreds `
    -AzCredential $adminCreds `
    –DiagnosticsUserPassword  $passwd

```

**Wachtwoord voor de lokale beheerdersaccount VM wijzigen.**

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
|AzCredential|Referentie van account voor Azure Stack-servicebeheerder.|
|CloudAdminCredential|Azure Stack cloud admin-account domeinreferentie.|
|PrivilegedEndpoint|Bevoorrechte eindpunt voor toegang tot Get-AzureStackStampInformation.|
|DiagnosticsUserPassword|Diagnostische gegevens van het wachtwoord voor gebruikersaccount.|
|VMLocalCredential|Het lokale beheerdersaccount op de MySQLAdapter VM.|
|DefaultSSLCertificatePassword|Standaard SSL-certificaat (* pfx) wachtwoord.|
|DependencyFilesLocalPath|Afhankelijkheid bestanden lokaal pad.|
|     |     |

### <a name="known-issues"></a>Bekende problemen

**Probleem:**<br>
De logboeken voor rotatie geheimen worden niet automatisch verzameld als het script geheime rotatie mislukt wanneer deze wordt uitgevoerd.

**Tijdelijke oplossing:**<br>
Gebruik de cmdlet Get-AzsDBAdapterLogs voor het verzamelen van alle resource provider Logboeken, met inbegrip van AzureStack.DatabaseAdapter.SecretRotation.ps1_*.log, opgeslagen in C:\Logs.

## <a name="collect-diagnostic-logs"></a>Diagnostische logboeken verzamelen

Voor het verzamelen van Logboeken van de vergrendelde virtuele machine, kunt u het eindpunt PowerShell net genoeg Administration (JEA) DBAdapterDiagnostics. Dit eindpunt biedt de volgende opdrachten:

- **Get-AzsDBAdapterLog**. Met deze opdracht maakt een zip-pakket van de resource provider diagnostische logboeken en slaat u het bestand op de schijf van de gebruiker de sessie. U kunt deze opdracht zonder parameters uitvoeren en de laatste vier uur aan logbestanden zijn verzameld.

- **Verwijder AzsDBAdapterLog**. Met deze opdracht verwijdert bestaande logboek-pakketten op de resourceprovider VM.

### <a name="endpoint-requirements-and-process"></a>Eindpunt vereisten en -proces

Wanneer een resourceprovider is geïnstalleerd of bijgewerkt, wordt het gebruikersaccount dbadapterdiag gemaakt. U gebruikt deze account voor het verzamelen van diagnostische logboeken.

>[!NOTE]
>Het wachtwoord voor het dbadapterdiag is hetzelfde als het wachtwoord voor de lokale beheerder op de virtuele machine die gemaakt tijdens een implementatie van de provider of de update.

Gebruik de _DBAdapterDiagnostics_ opdrachten, een externe PowerShell-sessie met de resource provider virtuele machine maken en voer de **Get-AzsDBAdapterLog** opdracht.

Stelt u de tijdsduur voor logboekgegevens verzameld door de **FromDate** en **ToDate** parameters. Als u een of beide van deze parameters niet opgeeft, worden de volgende standaardwaarden gebruikt:

* FromDate is vier uur vóór de huidige tijd.
* ToDate is de huidige tijd.

**Voorbeeld van PowerShell-script voor het verzamelen van Logboeken.**

Het volgende script toont hoe u voor het verzamelen van diagnostische logboeken van de resourceprovider VM.

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

[Verwijder de MySQL-resourceprovider](azure-stack-mysql-resource-provider-remove.md)
