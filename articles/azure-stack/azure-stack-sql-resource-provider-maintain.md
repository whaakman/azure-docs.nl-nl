---
title: Onderhoud van de SQL-resourceprovider in Azure Stack | Microsoft Docs
description: Meer informatie over hoe u de SQL-resource provider-service in Azure Stack kunt onderhouden.
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
ms.date: 01/11/2019
ms.author: jeffgilb
ms.reviewer: jiahan
ms.openlocfilehash: 68665cc588f8a6340de393330c7a248503b07125
ms.sourcegitcommit: f4b78e2c9962d3139a910a4d222d02cda1474440
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/12/2019
ms.locfileid: "54244984"
---
# <a name="sql-resource-provider-maintenance-operations"></a>SQL onderhoud resourceproviderbewerkingen

De SQL-resourceprovider wordt uitgevoerd op een vergrendelde virtuele machine. Om in te schakelen onderhoudsbewerkingen, moet u de beveiliging van de virtuele machine bijwerken. Om dit te doen met behulp van het principe van minimale bevoegdheden, kunt u [PowerShell alleen Enough Administration (JEA)](https://docs.microsoft.com/powershell/jea/overview) eindpunt *DBAdapterMaintenance*. Het installatiepakket voor de resource-provider bevat een script voor deze bewerking.

## <a name="patching-and-updating"></a>Patches en updates

De SQL-resourceprovider wordt niet verwerkt als onderdeel van Azure Stack, omdat het is een invoegtoepassing. Microsoft biedt updates voor de SQL-resourceprovider als nodig. Wanneer er een bijgewerkte SQL-adapter wordt uitgebracht, wordt een script beschikbaar om toe te passen van de update. Dit script maakt een nieuwe resourceprovider VM, de status van de oude provider VM migreren naar de nieuwe virtuele machine. Zie voor meer informatie, [bijwerken van de SQL-resourceprovider](azure-stack-sql-resource-provider-update.md).

### <a name="provider-virtual-machine"></a>Provider van virtuele machine

Omdat de resourceprovider wordt uitgevoerd op een *gebruiker* virtuele machine, moet u de vereiste patches en updates van toepassing wanneer deze zijn vrijgegeven. U kunt de Windows update-pakketten die worden geleverd als onderdeel van de patch-en updatecyclus updates toepassen op de virtuele machine.

## <a name="updating-sql-credentials"></a>Bijwerken van de SQL-referenties

U bent verantwoordelijk voor het maken en onderhouden van accounts sysadmin op de SQL-servers. De resourceprovider moet een account met deze bevoegdheden voor het beheren van databases voor gebruikers, maar de eigenschap hoeft niet de toegang tot gegevens van de gebruikers. Als u nodig hebt om bij te werken van de sysadmin-wachtwoorden van uw SQL-servers, kunt u de beheerinterface van de resourceprovider om een opgeslagen wachtwoord te wijzigen. Deze wachtwoorden worden opgeslagen in een Key Vault voor uw Azure Stack-exemplaar.

Als u wilt de instellingen wijzigen, selecteert u **Bladeren** &gt; **BEHEERDERSRESOURCES** &gt; **SQL-Servers die als host fungeert** &gt; **SQL-aanmeldingen** en selecteer de gebruikersnaam van een. De wijziging moet worden uitgevoerd op het SQL-exemplaar eerste (en alle replica's, indien nodig.) Onder **instellingen**, selecteer **wachtwoord**.

![Het beheerderswachtwoord bijwerken](./media/azure-stack-sql-rp-deploy/sqlrp-update-password.PNG)

## <a name="secrets-rotation"></a>Geheimen, rotatie

*Deze instructies zijn alleen van toepassing op Azure Stack-geïntegreerde systemen.*

Wanneer met behulp van de resourceproviders SQL- en MySQL met Azure Stack-systemen geïntegreerde, is de Azure Stack-operators verantwoordelijk voor het draaien van de volgende resource provider infrastructuur geheimen om ervoor te zorgen dat ze niet verloopt:

- Externe SSL-certificaat [opgegeven tijdens de implementatie](azure-stack-pki-certs.md).
- Resource provider VM lokale wachtwoord van de beheerder opgegeven tijdens de implementatie.
- Wachtwoord van resource provider diagnostische gebruiker (dbadapterdiag).

### <a name="powershell-examples-for-rotating-secrets"></a>PowerShell-voorbeelden voor het draaien van geheimen

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

**De diagnostische gebruikerswachtwoord wijzigen.**

```powershell
.\SecretRotationSQLProvider.ps1 `
    -Privilegedendpoint $Privilegedendpoint `
    -CloudAdminCredential $cloudCreds `
    -AzCredential $adminCreds `
    –DiagnosticsUserPassword  $passwd
```

**Wijzig het wachtwoord van virtuele machine lokale administrator-account.**

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

|Parameter|Description|
|-----|-----|
|AzCredential|Azure Stack-servicebeheerder-accountreferenties.|
|CloudAdminCredential|Azure Stack cloud domein account beheerreferenties.|
|PrivilegedEndpoint|Bevoegde eindpunt voor toegang tot de Get-AzureStackStampInformation.|
|DiagnosticsUserPassword|Diagnostische gegevens van het wachtwoord voor gebruikersaccount.|
|VMLocalCredential|Lokale beheerdersaccount op de VM MySQLAdapter.|
|DefaultSSLCertificatePassword|Standaard SSL-certificaat (* pfx) wachtwoord.|
|DependencyFilesLocalPath|Afhankelijkheid bestanden lokaal pad.|
|     |     |

### <a name="known-issues"></a>Bekende problemen

**Probleem**: Logboeken van de rotatie van geheimen.<br>
De logboeken voor geheimen, rotatie worden niet automatisch verzameld als het aangepaste script geheime rotatie mislukt wanneer deze wordt uitgevoerd.

**Tijdelijke oplossing**:<br>
Gebruik de cmdlet Get-AzsDBAdapterLogs om alle resource provider-Logboeken, met inbegrip van AzureStack.DatabaseAdapter.SecretRotation.ps1_*.log, opgeslagen in C:\Logs te verzamelen.

## <a name="update-the-virtual-machine-operating-system"></a>Werk het besturingssysteem van de virtuele machine

Gebruik een van de volgende methoden voor het bijwerken van het besturingssysteem van de virtuele machine.

- Installeer de meest recente resource provider-pakket met behulp van een momenteel gecorrigeerde installatiekopie van Windows Server 2016 Core.
- Een Windows Update-pakket installeren tijdens de installatie van of bijwerken bij de resourceprovider.

## <a name="update-the-virtual-machine-windows-defender-definitions"></a>De virtuele machine Windows Defender-definities bijwerken

De Windows Defender-definities bijwerken:

1. Download de Windows Defender-definities via bijwerken [Windows Defender de definitie](https://www.microsoft.com/en-us/wdsi/definitions).

   Op de definities bijwerken van de pagina, schuif omlaag naar 'Handmatig downloaden en installeren van de definities'. Download het bestand van de 64-bits 'Windows Defender Antivirus voor Windows 10 en Windows 8.1 '.

   U kunt ook [deze directe koppeling](https://go.microsoft.com/fwlink/?LinkID=121721&arch=x64) op het bestand fpam fe.exe downloaden/uitvoeren.

2. Maak een PowerShell-sessie naar SQL resource provider-adapter van de virtuele machine onderhoud eindpunt.

3. Kopieer het bestand van de update definities aan de virtuele machine met behulp van de eindpunt-sessie van onderhoud.

4. Voer op de onderhoud PowerShell-sessie de *Update DBAdapterWindowsDefenderDefinitions* opdracht.

5. Nadat u de definities worden geïnstalleerd, wordt aangeraden dat u het bestand van de update definities met behulp van verwijderen de *Remove-ItemOnUserDrive* opdracht.

**PowerShell-voorbeeldscript voor het bijwerken van definities.**

U kunt bewerken en voer het volgende script als u wilt de Defender-definities bijwerken. Waarden in het script vervangen door waarden van uw omgeving.

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

## <a name="collect-diagnostic-logs"></a>Logboeken met diagnostische gegevens verzamelen

Voor het verzamelen van Logboeken van de vergrendelde virtuele machine, kunt u het eindpunt van de PowerShell alleen Enough Administration (JEA) *DBAdapterDiagnostics*. Dit eindpunt biedt de volgende opdrachten:

- **Get-AzsDBAdapterLog**. Met deze opdracht maakt u een zip-pakket van de resource provider diagnostische logboeken en slaat het bestand op schijf van de gebruiker van de sessie. U kunt deze opdracht zonder parameters uitvoert en de laatste vier uur van de logboeken worden verzameld.
- **Remove-AzsDBAdapterLog**. Met deze opdracht verwijdert bestaande log-pakketten op de VM-resourceprovider.

### <a name="endpoint-requirements-and-process"></a>Eindpunt-vereisten en proces

Wanneer een resourceprovider is geïnstalleerd of bijgewerkt, de **dbadapterdiag** gebruikersaccount is gemaakt. U gebruikt dit account voor het verzamelen van diagnostische logboeken.

>[!NOTE]
>Het wachtwoord voor het dbadapterdiag is hetzelfde als het wachtwoord voor de lokale beheerder op de virtuele machine die gemaakt tijdens een implementatie van de provider of de update.

Gebruik de *DBAdapterDiagnostics* opdrachten, een externe PowerShell-sessie met de resource provider virtuele machine maken en uitvoeren van de **Get-AzsDBAdapterLog** opdracht.

U de tijdsduur voor logboekverzameling instellen met behulp van de **FromDate** en **ToDate** parameters. Als u een of beide van deze parameters niet opgeeft, worden de volgende standaardwaarden worden gebruikt:

- FromDate staat aan vier uur vóór de huidige tijd.
- ToDate is de huidige tijd.

**PowerShell-voorbeeldscript voor het verzamelen van Logboeken.**

Het volgende script toont hoe u logboeken met diagnostische gegevens verzamelen over de VM-resourceprovider.

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

[SQL Server die als host fungeert servers toevoegen](azure-stack-sql-resource-provider-hosting-servers.md)
