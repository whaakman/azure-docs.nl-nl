---
title: Onderhoud van de SQL-resourceprovider op Azure-Stack | Microsoft Docs
description: Meer informatie over hoe u de SQL resource provider-service op Azure-Stack kunt onderhouden.
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
ms.date: 06/20/2018
ms.author: jeffgilb
ms.reviewer: jeffgo
ms.openlocfilehash: ad899739dab1dc51d64368d2136ab87f73f6f3a0
ms.sourcegitcommit: ea5193f0729e85e2ddb11bb6d4516958510fd14c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/21/2018
ms.locfileid: "36300907"
---
# <a name="sql-resource-provider-maintenance-operations"></a>SQL resource provider onderhoudsbewerkingen

De SQL-resourceprovider wordt uitgevoerd op een vergrendelde virtuele machine. Om in te schakelen onderhoudsbewerkingen, moet u de beveiliging van de virtuele machine bijwerken. Om dit te doen met behulp van het principe van minimale bevoegdheden, kunt u [PowerShell net genoeg Administration (JEA)](https://docs.microsoft.com/en-us/powershell/jea/overview) eindpunt *DBAdapterMaintenance*. Het installatiepakket voor de resource-provider bevat een script voor deze bewerking.

## <a name="patching-and-updating"></a>Patchen en bijwerken

De resourceprovider voor SQL is niet verwerkt als onderdeel van Azure-Stack omdat deze een onderdeel van de invoegtoepassing. Microsoft biedt updates voor de SQL-resourceprovider indien nodig. Wanneer een bijgewerkte SQL-adapter wordt uitgebracht, worden een script is opgegeven voor de update toepassen. Dit script maakt een nieuwe virtuele machine, de status van de oude provider VM migreren naar de nieuwe virtuele machine van de resourceprovider. Zie voor meer informatie [bijwerken van de SQL-resourceprovider](azure-stack-sql-resource-provider-update.md).

### <a name="provider-virtual-machine"></a>Provider van virtuele machine

Omdat de resourceprovider wordt uitgevoerd op een *gebruiker* virtuele machine, moet u de vereiste patches en updates van toepassing wanneer deze worden vrijgegeven. U kunt de Windows update-pakketten die worden geleverd als onderdeel van de patch-en updatecyclus updates toepassen op de virtuele machine.

## <a name="backuprestoredisaster-recovery"></a>Backup/Restore/noodherstel

 Omdat deze een onderdeel van de invoegtoepassing, de SQL-resourceprovider is niet een back-up als onderdeel van een proces Azure Stack zakelijke continuïteit Disaster Recovery (BCDR). Scripts wordt aangeboden voor de volgende bewerkingen:

- Back-ups van informatie over de status (opgeslagen in een Stack van Azure storage-account).
- De resourceprovider herstellen als een volledige stack moet worden hersteld.

>[!NOTE]
>Als er een herstelbewerking uit te voeren, moeten database-servers worden hersteld voordat de resourceprovider is hersteld.

## <a name="updating-sql-credentials"></a>SQL-referenties bijwerken

U bent zelf verantwoordelijk voor het maken en beheren van accounts sysadmin op de SQL-servers. De resourceprovider moet een account met deze rechten voor het beheren van databases voor gebruikers, maar deze hoeft niet de toegang tot gegevens van de gebruikers. Als u bijwerken van de sysadmin-wachtwoorden van uw SQL-servers wilt, kunt u de beheerinterface van de resourceprovider om een opgeslagen wachtwoord te wijzigen. Deze wachtwoorden worden opgeslagen in een Sleutelkluis voor uw Azure-Stack-exemplaar.

Selecteer om de instellingen wijzigt, **Bladeren** &gt; **SERVERVIRTUALISATIE** &gt; **SQL-Servers die als host fungeert** &gt; **SQL-aanmeldingen** en selecteer de naam van een gebruiker. De wijziging moet eerst op de SQL-exemplaar worden gemaakt (en alle replica's, indien nodig.) Onder **instellingen**, selecteer **wachtwoord**.

![Het beheerderswachtwoord bijwerken](./media/azure-stack-sql-rp-deploy/sqlrp-update-password.PNG)

## <a name="secrets-rotation"></a>Geheimen draaien

*Deze instructies zijn alleen van toepassing op Azure Stack geïntegreerde systemen versie 1804 en Later. Probeer niet om te draaien geheimen op de pre-1804 Azure Stack-versies.*

Wanneer met behulp van de resourceproviders SQL en MySQL met Azure-Stack geïntegreerd systemen, kunt u de volgende infrastructuur (implementatie) geheimen draaien:

- Externe SSL-certificaat [opgegeven tijdens de implementatie](azure-stack-pki-certs.md).
- Het resource provider VM lokale administrator-account opgegeven wachtwoord tijdens de implementatie.
- Het wachtwoord resource provider diagnostische gebruiker (dbadapterdiag).

### <a name="powershell-examples-for-rotating-secrets"></a>PowerShell-voorbeelden voor roulatie van geheimen

**Alle geheimen op hetzelfde moment wijzigen.**

```powershell
.\SecretRotationSQLProvider.ps1 `
    -Privilegedendpoint $Privilegedendpoint `
    -CloudAdminCredential $cloudCreds `
    -AzCredential $adminCreds `
    –DiagnosticsUserPassword $passwd `
    -DependencyFilesLocalPath $certPath `
    -DefaultSSLCertificatePassword $certPasswd  `
    -VMLocalCredential $localCreds
```

**Het gebruikerswachtwoord diagnostische wijzigen.**

```powershell
.\SecretRotationSQLProvider.ps1 `
    -Privilegedendpoint $Privilegedendpoint `
    -CloudAdminCredential $cloudCreds `
    -AzCredential $adminCreds `
    –DiagnosticsUserPassword  $passwd
```

**Wachtwoord voor de lokale beheerdersaccount VM wijzigen.**

```powershell
.\SecretRotationSQLProvider.ps1 `
    -Privilegedendpoint $Privilegedendpoint `
    -CloudAdminCredential $cloudCreds `
    -AzCredential $adminCreds `
    -VMLocalCredential $localCreds
```

**Wijzig het wachtwoord voor het SSL-certificaat.**

```powershell
.\SecretRotationSQLProvider.ps1 `
    -Privilegedendpoint $Privilegedendpoint `
    -CloudAdminCredential $cloudCreds `
    -AzCredential $adminCreds `
    -DependencyFilesLocalPath $certPath `
    -DefaultSSLCertificatePassword $certPasswd
```

### <a name="secretrotationsqlproviderps1-parameters"></a>SecretRotationSQLProvider.ps1 parameters

|Parameter|Beschrijving|
|-----|-----|
|AzCredential|Referentie van account voor Azure Stack-servicebeheerder.|
|CloudAdminCredential|Azure Stack cloud admin-account domeinreferentie.|
|PrivilegedEndpoint|Bevoorrechte eindpunt voor toegang tot Get-AzureStackStampInformation.|
|DiagnosticsUserPassword|Diagnostische gegevens van het wachtwoord voor gebruikersaccount.|
|VMLocalCredential|Lokale beheerdersaccount op de MySQLAdapter VM.|
|DefaultSSLCertificatePassword|Standaard SSL-certificaat (* pfx) wachtwoord.|
|DependencyFilesLocalPath|Afhankelijkheid bestanden lokaal pad.|
|     |     |

### <a name="known-issues"></a>Bekende problemen

**Probleem**: geheimen rotatie Logboeken.<br>
De logboeken voor rotatie geheimen worden niet automatisch verzameld als de geheime rotatie aangepast script mislukt wanneer deze wordt uitgevoerd.

**Tijdelijke oplossing**:<br>
Gebruik de cmdlet Get-AzsDBAdapterLogs voor het verzamelen van alle resource provider-Logboeken, met inbegrip van AzureStack.DatabaseAdapter.SecretRotation.ps1_*.log, opgeslagen in C:\Logs.

## <a name="update-the-virtual-machine-operating-system"></a>Werk het besturingssysteem van de virtuele machine

Gebruik een van de volgende methoden om het besturingssysteem van de virtuele machine bijwerken.

- Installeer de meest recente resource provider-pakket met de installatiekopie van een momenteel patches Windows Server 2016 Core.
- Een Windows Update-pakket installeren tijdens de installatie van of bijwerken naar de resourceprovider.

## <a name="update-the-virtual-machine-windows-defender-definitions"></a>De virtuele machine Windows Defender-definities bijwerken

De Windows Defender-definities bijwerken:

1. Download de Windows Defender-definities vanuit bijwerken [Windows Defender Definition](https://www.microsoft.com/en-us/wdsi/definitions).

   Op de definities bijwerken pagina, schuif omlaag naar 'Handmatig downloaden en installeren van de definities'. Download het bestand van de 64-bits 'Windows Defender Antivirus voor Windows 10 en Windows 8.1 '.

   U kunt ook [deze directe koppeling](https://go.microsoft.com/fwlink/?LinkID=121721&arch=x64) op het bestand fpam fe.exe downloaden/uitvoeren.

2. Maak een PowerShell-sessie naar SQL resource provider-adapter van de virtuele machine onderhoud eindpunt.

3. Kopieer het bestand van de update definities aan de virtuele machine met behulp van de onderhoudsmodus endpoint-sessie.

4. Voer op de onderhoud PowerShell-sessie de *Update DBAdapterWindowsDefenderDefinitions* opdracht.

5. Nadat u de definities hebt geïnstalleerd, wordt aangeraden het updatebestand definities te verwijderen via de *verwijderen ItemOnUserDrive* opdracht.

**Voorbeeld van PowerShell-script voor het bijwerken van definities.**

U kunt bewerken en voer het volgende script om de Defender-definities bijwerken. Waarden in het script vervangen door waarden van uw omgeving.

```powershell
# Set credentials for local admin on the resource provider VM.
$vmLocalAdminPass = ConvertTo-SecureString "<local admin user password>" -AsPlainText -Force
$vmLocalAdminUser = "<local admin user name>"
$vmLocalAdminCreds = New-Object System.Management.Automation.PSCredential `
    ($vmLocalAdminUser, $vmLocalAdminPass)

# Provide the public IP address for the adapter VM.
$databaseRPMachine  = "<RP VM IP address>"
$localPathToDefenderUpdate = "C:\DefenderUpdates\mpam-fe.exe"

# Download the Windows Defender update definitions file from https://www.microsoft.com/en-us/wdsi/definitions.
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

## <a name="collect-diagnostic-logs"></a>Diagnostische logboeken verzamelen

Voor het verzamelen van Logboeken van de vergrendelde virtuele machine, kunt u het eindpunt PowerShell net genoeg Administration (JEA) *DBAdapterDiagnostics*. Dit eindpunt biedt de volgende opdrachten:

- **Get-AzsDBAdapterLog**. Met deze opdracht maakt een zip-pakket van de resource provider diagnostische logboeken en slaat u het bestand op de schijf van de gebruiker de sessie. U kunt deze opdracht zonder parameters uitvoeren en de laatste vier uur aan logbestanden zijn verzameld.
- **Verwijder AzsDBAdapterLog**. Met deze opdracht verwijdert bestaande logboek-pakketten op de resourceprovider VM.

### <a name="endpoint-requirements-and-process"></a>Eindpunt vereisten en -proces

Wanneer een resourceprovider is geïnstalleerd of bijgewerkt, de **dbadapterdiag** gebruikersaccount wordt gemaakt. U gebruikt deze account voor het verzamelen van diagnostische logboeken.

>[!NOTE]
>Het wachtwoord voor het dbadapterdiag is hetzelfde als het wachtwoord voor de lokale beheerder op de virtuele machine die gemaakt tijdens een implementatie van de provider of de update.

Gebruik de *DBAdapterDiagnostics* opdrachten, een externe PowerShell-sessie met de resource provider virtuele machine maken en voer de **Get-AzsDBAdapterLog** opdracht.

Stelt u de tijdsduur voor logboekgegevens verzameld door de **FromDate** en **ToDate** parameters. Als u een of beide van deze parameters niet opgeeft, worden de volgende standaardwaarden gebruikt:

- FromDate is vier uur vóór de huidige tijd.
- ToDate is de huidige tijd.

**Voorbeeld van PowerShell-script voor het verzamelen van Logboeken.**

Het volgende script toont hoe u voor het verzamelen van diagnostische logboeken van de resourceprovider VM.

```powershell
# Create a new diagnostics endpoint session.
$databaseRPMachineIP = '<RP VM IP address>'
$diagnosticsUserName = 'dbadapterdiag'
$diagnosticsUserPassword = '<Enter Diagnostic password>'

$diagCreds = New-Object System.Management.Automation.PSCredential `
        ($diagnosticsUserName, (ConvertTo-SecureString -String $diagnosticsUserPassword -AsPlainText -Force))
$session = New-PSSession -ComputerName $databaseRPMachineIP -Credential $diagCreds `
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
$cleanup = Invoke-Command -Session $session -ScriptBlock {Remove- AzsDBAdapterLog }
# Close the session.
$session | Remove-PSSession
```

## <a name="next-steps"></a>Volgende stappen

[SQL Server die als host fungeert voor servers toevoegen](azure-stack-sql-resource-provider-hosting-servers.md)
