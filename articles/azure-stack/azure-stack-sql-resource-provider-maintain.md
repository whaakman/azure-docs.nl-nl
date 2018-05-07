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
ms.date: 05/01/2018
ms.author: jeffgilb
ms.reviewer: jeffgo
ms.openlocfilehash: 53436d131672622ae1a72a1bb84d5aa83fdbdc0c
ms.sourcegitcommit: c47ef7899572bf6441627f76eb4c4ac15e487aec
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/04/2018
---
# <a name="maintenance-operations"></a>Onderhoud 
De SQL-resourceprovider is de virtuele machine een vergrendeld. Bijwerken van de resource provider van de virtuele machine kan worden uitgevoerd via het eindpunt PowerShell net genoeg Administration (JEA) _DBAdapterMaintenance_. Een script wordt geleverd bij de RP-installatiepakket te vergemakkelijken van deze bewerkingen.

## <a name="patching-and-updating"></a>Patchen en bijwerken
De resourceprovider voor SQL is niet verwerkt als onderdeel van Azure-Stack omdat deze een onderdeel van de invoegtoepassing. Microsoft zal updates bieden voor de SQL-resourceprovider indien nodig. De SQL-resourceprovider is gemaakt op een _gebruiker_ virtuele machine onder de standaard Provider-abonnement. Daarom is het nodig zijn voor het bieden van Windows-patches, antivirus handtekeningen, enzovoort. De Windows update-pakketten die worden geleverd als onderdeel van de patch-en updatecyclus updates toepassen op de virtuele machine van Windows kan worden gebruikt. Wanneer een bijgewerkte adapter wordt uitgebracht, worden een script is opgegeven voor de update toepassen. Dit script maakt een nieuwe RP VM en migreer elke status die u al hebt.

 ## <a name="backuprestoredisaster-recovery"></a>Backup/Restore/noodherstel
 De resourceprovider voor SQL is geen back-up als onderdeel van Azure Stack BC-DR-proces, omdat deze een onderdeel van de invoegtoepassing. Scripts wordt aangeboden mogelijk te maken:
- Een back-up van de van de benodigde statusinformatie (opgeslagen in een Stack van Azure storage-account)
- Het RP herstellen mocht het volledige stack herstel niet meer nodig.
Database-servers moeten eerst worden hersteld (indien nodig), voordat de resource provider is hersteld.

## <a name="updating-sql-credentials"></a>SQL-referenties bijwerken
U bent zelf verantwoordelijk voor het maken en onderhouden van de beheerdersaccounts systeem op uw SQL-servers. De resourceprovider moet een account met deze rechten voor het beheren van databases namens gebruikers - hoeft niet toegang tot de gegevens in deze databases. Als u bijwerken van de sa-wachtwoorden op de SQL-servers wilt, kunt u de mogelijkheid van de update van de beheerinterface van de resourceprovider het opgeslagen wachtwoord gebruikt door de resourceprovider te wijzigen. Deze wachtwoorden worden opgeslagen in een Sleutelkluis voor uw Azure-Stack-exemplaar.

Om de instellingen wijzigt, klikt u op **Bladeren** &gt; **SERVERVIRTUALISATIE** &gt; **SQL-Servers die als host fungeert** &gt; **SQL-aanmeldingen** en selecteert u een aanmeldingsnaam. De wijziging moet eerst op de SQL-exemplaar worden gemaakt (en alle replica's, indien nodig). In de **instellingen** -scherm, klikt u op **wachtwoord**.

![Het beheerderswachtwoord bijwerken](./media/azure-stack-sql-rp-deploy/sqlrp-update-password.PNG)

## <a name="update-the-virtual-machine-operating-system"></a>Werk het besturingssysteem van de virtuele machine
Er zijn verschillende manieren om bij te werken van Windows Server-VM:
* Installeer het meest recente resource provider-pakket met de installatiekopie van een momenteel patches Windows Server 2016 Core
* Een Windows Update-pakket installeren tijdens de installatie of het bijwerken van de RP

## <a name="update-the-virtual-machine-windows-defender-definitions"></a>De virtuele machine Windows Defender-definities bijwerken
Volg deze stappen voor de Defender-definities bijwerken:

1. Download de Windows Defender-definities vanuit bijwerken [Windows Defender-definitie](https://www.microsoft.com/en-us/wdsi/definitions)

    Klik op deze pagina onder 'Handmatig downloaden en installeren van de definities' downloaden ' Windows Defender Antivirus voor Windows 10 en Windows 8.1 "64-bits bestand. 
    
    Directe koppeling: https://go.microsoft.com/fwlink/?LinkID=121721&arch=x64

2. Een PowerShell-sessie naar SQL RP-adapter van de virtuele machine onderhoud eindpunt maken
3. Kopieer het bestand van de update definities de DB-adapter-machine met de onderhoudsmodus endpoint-sessie
4. Op het onderhoud PowerShell sessie aanroepen de _Update DBAdapterWindowsDefenderDefinitions_ opdracht
5. Na de installatie, is het aanbevolen het updatebestand definities verwijderen. Het kan worden verwijderd op de onderhoud sessie met de _verwijderen ItemOnUserDrive)_ opdracht.


Hier volgt een voorbeeld van een script om bij te werken de Defender definities (vervang deze door het adres of de naam van de virtuele machine met de werkelijke waarde):

```powershell
# Set credentials for the diagnostic user
$diagPass = ConvertTo-SecureString "P@ssw0rd1" -AsPlainText -Force
$diagCreds = New-Object System.Management.Automation.PSCredential `
    ("dbadapterdiag", $vmLocalAdminPass)$diagCreds = Get-Credential

# Public IP Address of the DB adapter machine
$databaseRPMachine  = "XX.XX.XX.XX"
$localPathToDefenderUpdate = "C:\DefenderUpdates\mpam-fe.exe"
 
# Download Windows Defender update definitions file from https://www.microsoft.com/en-us/wdsi/definitions. 
Invoke-WebRequest -Uri https://go.microsoft.com/fwlink/?LinkID=121721&arch=x64 `
    -Outfile $localPathToDefenderUpdate 

# Create session to the maintenance endpoint
$session = New-PSSession -ComputerName $databaseRPMachine `
    -Credential $diagCreds -ConfigurationName DBAdapterMaintenance
# Copy defender update file to the db adapter machine
Copy-Item -ToSession $session -Path $localPathToDefenderUpdate `
     -Destination "User:\mpam-fe.exe"
# Install the update file
Invoke-Command -Session $session -ScriptBlock `
    {Update-AzSDBAdapterWindowsDefenderDefinitions -DefinitionsUpdatePackageFile "User:\mpam-fe.exe"}
# Cleanup the definitions package file and session
Invoke-Command -Session $session -ScriptBlock `
    {Remove-AzSItemOnUserDrive -ItemPath "User:\mpam-fe.exe"}
$session | Remove-PSSession
```


## <a name="collect-diagnostic-logs"></a>Diagnostische logboeken verzamelen
De SQL-resourceprovider is de virtuele machine een vergrendeld. Als het nodig zijn voor het verzamelen van Logboeken van de virtuele machine, een PowerShell net genoeg Administration (JEA)-eindpunt wordt _DBAdapterDiagnostics_ is opgegeven voor dit doel. Er zijn twee opdrachten beschikbaar via dit eindpunt:

* Get-AzsDBAdapterLog - bereidt een zip-pakket met RP diagnostische logboeken en plaatst deze op de schijf van de sessie van de gebruiker. De opdracht kan worden aangeroepen zonder parameters en de laatste vier uur van Logboeken verzamelt.
* Remove-AzsDBAdapterLog - ruimt bestaande logboek-pakketten op de resourceprovider VM

Een gebruikersaccount aangeroepen _dbadapterdiag_ is gemaakt tijdens de RP-implementatie of update voor de verbinding met het eindpunt van de diagnostische gegevens voor het uitpakken van RP-Logboeken. Het wachtwoord van dit account is hetzelfde als het wachtwoord voor het lokale administrator-account tijdens de implementatie-update.

Voor het gebruik van deze opdrachten moet u voor het maken van een externe PowerShell-sessie op de virtuele machine van de resource provider en de opdracht aanroepen. U kunt desgewenst FromDate en ToDate parameters opgeven. Als u een of beide van deze niet opgeeft, worden de FromDate vier uur vóór de huidige tijd en de ToDate worden de huidige tijd.

Dit voorbeeldscript wordt getoond hoe het gebruik van deze opdrachten:

```powershell
# Create a new diagnostics endpoint session.
$databaseRPMachineIP = '<RP VM IP>'
$diagnosticsUserName = 'dbadapterdiag'
$diagnosticsUserPassword = '<see above>'

$diagCreds = New-Object System.Management.Automation.PSCredential `
        ($diagnosticsUserName, $diagnosticsUserPassword)
$session = New-PSSession -ComputerName $databaseRPMachineIP -Credential $diagCreds `
        -ConfigurationName DBAdapterDiagnostics

# Sample captures logs from the previous one hour
$fromDate = (Get-Date).AddHours(-1)
$dateNow = Get-Date
$sb = {param($d1,$d2) Get-AzSDBAdapterLog -FromDate $d1 -ToDate $d2}
$logs = Invoke-Command -Session $session -ScriptBlock $sb -ArgumentList $fromDate,$dateNow

# Copy the logs
$sourcePath = "User:\{0}" -f $logs
$destinationPackage = Join-Path -Path (Convert-Path '.') -ChildPath $logs
Copy-Item -FromSession $session -Path $sourcePath -Destination $destinationPackage

# Cleanup logs
$cleanup = Invoke-Command -Session $session -ScriptBlock {Remove- AzsDBAdapterLog }
# Close the session
$session | Remove-PSSession
```

## <a name="next-steps"></a>Volgende stappen
[SQL Server die als host fungeert voor servers toevoegen](azure-stack-sql-resource-provider-hosting-servers.md)
