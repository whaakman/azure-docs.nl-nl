---
title: Met behulp van SQL-databases op Azure-Stack | Microsoft Docs
description: Meer informatie over hoe u SQL-databases kan implementeren als een service op Azure-Stack en de snelle stappen voor het implementeren van de SQL Server resource provider-adapter.
services: azure-stack
documentationCenter: ''
author: mattbriggs
manager: femila
editor: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/07/2018
ms.author: mabrigg
ms.reviewer: jeffgo
ms.openlocfilehash: 1754d4b121fdaf4f7b8e4c1954dfd4202ba344b1
ms.sourcegitcommit: 34e0b4a7427f9d2a74164a18c3063c8be967b194
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/30/2018
---
# <a name="use-sql-databases-on-microsoft-azure-stack"></a>SQL-databases op Microsoft Azure-Stack gebruiken

*Van toepassing op: Azure Stack geïntegreerde systemen en Azure Stack Development Kit*

Gebruik van de adapter van SQL Server resource provider voor SQL-databases weergeven als een service van [Azure Stack](azure-stack-poc.md). Na het installeren van de resourceprovider en verbinden met een of meer exemplaren van SQL Server, kunnen u en uw gebruikers maken:
- Databases voor cloud-systeemeigen apps.
- Websites op basis van SQL.
- Werklasten die zijn gebaseerd op SQL.
Er geen voor het inrichten van een virtuele machine (VM) die als host fungeert voor SQL Server elke keer.

De resourceprovider biedt geen ondersteuning voor alle database beheermogelijkheden van [Azure SQL Database](https://azure.microsoft.com/services/sql-database/). Bijvoorbeeld, pools voor elastische databases en de mogelijkheid om de prestaties van de database automatisch omhoog en omlaag bellen zijn niet beschikbaar. Echter, de resource provider biedt ondersteuning voor vergelijkbare maken, lezen, bijwerken en verwijderen (CRUD)-bewerkingen. De API is niet compatibel met SQL-Database.

## <a name="sql-resource-provider-adapter-architecture"></a>Architectuur van SQL resource provider-adapter
De resourceprovider bestaat uit drie onderdelen:

- **De SQL resource provider adapter VM**, dit is een virtuele Windows-computer die de provider-services worden uitgevoerd.
- **De resourceprovider zelf**, die inrichting verzoeken verwerkt en resources van de database zichtbaar gemaakt.
- **Servers waarop SQL Server worden gehost**, hosting-servers die een capaciteit voor databases worden aangeroepen.

U moet een (of meer) exemplaren van SQL Server maken en/of toegang tot de externe SQL Server-exemplaren.

> [!NOTE]
> Hosting-servers die zijn geïnstalleerd op Azure-Stack moeten geïntegreerde systemen worden gemaakt van een tenantabonnement. Ze kunnen niet worden gemaakt van het standaard provider-abonnement. Ze moeten worden gemaakt vanuit de tenantportal of vanuit een PowerShell-sessie met een juiste aanmelden. Alle hosting-servers toerekenbare VM's zijn en moeten de juiste licenties hebt. De servicebeheerder kan de eigenaar van de tenant-abonnement zijn.

## <a name="deploy-the-resource-provider"></a>De resourceprovider implementeren

1. Als u dit nog niet hebt gedaan, registreren van uw development kit en downloaden van de installatiekopie van het Windows Server 2016 Datacenter Core downloadbare via Marketplace-beheer. U kunt een installatiekopie van Windows Server 2016 Core moet gebruiken. U kunt ook een script maken van een [installatiekopie van Windows Server 2016](https://docs.microsoft.com/azure/azure-stack/azure-stack-add-default-image). (Zorg ervoor dat de core-optie te selecteren.)

2. Aanmelden bij een host die toegang heeft tot de bevoegde VM-eindpunt.

    - Op Azure Stack Development Kit-installaties, moet u zich aanmelden bij de fysieke host.

    - Op systemen met meerdere knooppunten moet de host een systeem dat toegang heeft tot de bevoegde eindpunt.
    
    >[!NOTE]
    > Het systeem waarop het script wordt uitgevoerd *moet* worden van een Windows 10 of Windows Server 2016-systeem met de meest recente versie van de .NET runtime geïnstalleerd. Anders mislukt de installatie. De Stack-SDK van Azure-host voldoet aan dit criterium.


3. De SQL-resourceprovider binaire downloaden. Voer vervolgens de zelfstandig uitpakken om de inhoud naar een tijdelijke map te pakken.

    >[!NOTE] 
    > De resourceprovider heeft een minimale bijbehorende Azure-Stack bouwen. Zorg ervoor dat het juiste binaire bestand voor de versie van Azure-Stack met downloaden.

    | Azure Stack build | SQL resource provider-installatieprogramma |
    | --- | --- |
    | 1802: 1.0.180302.1 | [SQL RP versie 1.1.18.0](https://aka.ms/azurestacksqlrp1802) |
    | 1712: 1.0.180102.3, 1.0.180103.2 of 1.0.180106.1 (met meerdere knooppunten) | [SQL RP versie 1.1.14.0](https://aka.ms/azurestacksqlrp1712) |
    | 1711: 1.0.171122.1 | [SQL RP versie 1.1.12.0](https://aka.ms/azurestacksqlrp1711) |
    | 1710: 1.0.171028.1 | [SQL RP versie 1.1.8.0](https://aka.ms/azurestacksqlrp1710) |
  

4. Het vereiste certificaat opgeven. Een zelfondertekend certificaat gemaakt als onderdeel van dit proces voor de Stack Azure SDK, is er geen aanvullend certificaat vereist. Voor geïntegreerde systemen, moet u een geschikt certificaat opgeven.

   Voor uw eigen certificaat, plaatst u een .pfx-bestand in de **DependencyFilesLocalPath** als volgt:

    - Een jokertekencertificaat voor \*.dbadapter.\< regio\>.\< externe fqdn\> of een certificaat voor één site met een algemene naam van sqladapter.dbadapter.\< regio\>.\< externe fqdn\>.

    - Dit certificaat moet worden vertrouwd. Dat wil zeggen moet de keten van vertrouwensrelatie bestaan zonder tussenliggende certificaten.

    - Een enkele certificaatbestand kan in de map waarnaar wordt verwezen door de parameter DependencyFilesLocalPath bestaan.

    - De bestandsnaam moet de speciale tekens niet bevatten.


5. Open een **nieuwe** (beheerdersrechten) PowerShell-console met verhoogde bevoegdheid en de wijzigingen in de map waar u de bestanden hebt uitgepakt. Een nieuw venster gebruiken om problemen die mogelijk worden veroorzaakt door onjuiste PowerShell-modules die al zijn geladen in het systeem te voorkomen.

6. [Installeer Azure PowerShell versie 1.2.11](azure-stack-powershell-install.md).

7. Voer het script DeploySqlProvider.ps1, waarmee de volgende stappen uit:

    - De certificaten en andere artefacten uploadt naar een opslagaccount op Azure-Stack.
    - Publiceert galerie pakketten zodat u de SQL-databases via de galerie kunt implementeren.
    - Publiceert een gallery-pakket voor het implementeren van de hosting-servers.
    - Een virtuele machine implementeert met behulp van de installatiekopie van het Windows Server 2016 die is gemaakt in stap 1 en vervolgens de resourceprovider wordt geïnstalleerd.
    - Registreert een lokale DNS-record dat is toegewezen aan uw VM-resourceprovider.
    - Registreert uw resourceprovider met de lokale Azure Resource Manager (gebruiker en beheer).
    - Eventueel installeert één Windows update tijdens de installatie van de RP

8. Het is raadzaam dat u de installatiekopie van het meest recente Windows Server 2016 Core uit het beheer van de Marketplace downloaden. Als u een update installeert moet, kunt u één plaatsen. MSU-pakket in het pad van de lokale afhankelijkheid. Als meer dan één. MSU-bestand wordt gevonden, mislukt het script.


Hier volgt een voorbeeld die u vanuit de PowerShell-prompt uitvoeren kunt. (Zorg ervoor dat de accountgegevens en -wachtwoorden zo nodig wijzigen.)

```
# Install the AzureRM.Bootstrapper module, set the profile, and install the AzureRM and AzureStack modules.
Install-Module -Name AzureRm.BootStrapper -Force
Use-AzureRmProfile -Profile 2017-03-09-profile
Install-Module -Name AzureStack -RequiredVersion 1.2.11 -Force

# Use the NetBIOS name for the Azure Stack domain. On the Azure Stack SDK, the default is AzureStack but could have been changed at install time.
$domain = "AzureStack"

# For integrated systems, use the IP address of one of the ERCS virtual machines
$privilegedEndpoint = "AzS-ERCS01"

# Point to the directory where the resource provider installation files were extracted.
$tempDir = 'C:\TEMP\SQLRP'

# The service admin account (can be Azure Active Directory or Active Directory Federation Services).
$serviceAdmin = "admin@mydomain.onmicrosoft.com"
$AdminPass = ConvertTo-SecureString "P@ssw0rd1" -AsPlainText -Force
$AdminCreds = New-Object System.Management.Automation.PSCredential ($serviceAdmin, $AdminPass)

# Set credentials for the new resource provider VM local administrator account
$vmLocalAdminPass = ConvertTo-SecureString "P@ssw0rd1" -AsPlainText -Force
$vmLocalAdminCreds = New-Object System.Management.Automation.PSCredential ("sqlrpadmin", $vmLocalAdminPass)

# And the cloudadmin credential that's required for privileged endpoint access.
$CloudAdminPass = ConvertTo-SecureString "P@ssw0rd1" -AsPlainText -Force
$CloudAdminCreds = New-Object System.Management.Automation.PSCredential ("$domain\cloudadmin", $CloudAdminPass)

# Change the following as appropriate.
$PfxPass = ConvertTo-SecureString "P@ssw0rd1" -AsPlainText -Force

# Change directory to the folder where you extracted the installation files.
# Then adjust the endpoints.
. $tempDir\DeploySQLProvider.ps1 -AzCredential $AdminCreds `
  -VMLocalCredential $vmLocalAdminCreds `
  -CloudAdminCredential $cloudAdminCreds `
  -PrivilegedEndpoint $privilegedEndpoint `
  -DefaultSSLCertificatePassword $PfxPass `
  -DependencyFilesLocalPath $tempDir\cert
 ```

### <a name="deploysqlproviderps1-parameters"></a>DeploySqlProvider.ps1 parameters
U kunt deze parameters opgeven op de opdrachtregel. Als u dit niet doet, of als er parametervalidatie mislukt, wordt u gevraagd de vereiste parameters bevatten.

| Parameternaam | Beschrijving | Opmerking of de standaardwaarde |
| --- | --- | --- |
| **CloudAdminCredential** | De referentie voor de beheerder van de cloud, nodig voor toegang tot de bevoegde eindpunt. | _Vereist_ |
| **AzCredential** | De referenties voor de Azure-Stack-admin-serviceaccount. Dezelfde referenties gebruiken dat u gebruikt voor het implementeren van Azure-Stack. | _Vereist_ |
| **VMLocalCredential** | De referenties voor het lokale administrator-account van de SQL-resourceprovider VM. | _Vereist_ |
| **PrivilegedEndpoint** | De IP-adres of de DNS-naam van het bevoegde eindpunt. |  _Vereist_ |
| **DependencyFilesLocalPath** | Het PFX-certificaatbestand moet in deze map ook worden geplaatst. | _Optionele_ (_verplichte_ voor meerdere knooppunten) |
| **DefaultSSLCertificatePassword** | Het wachtwoord voor het pfx-certificaat. | _Vereist_ |
| **MaxRetryCount** | Het aantal keren dat u wilt opnieuw proberen aan elke als er een storing optreedt.| 2 |
| **RetryDuration** | De time-interval tussen nieuwe pogingen, in seconden. | 120 |
| **Verwijderen** | Hiermee verwijdert u de resourceprovider en alle bijbehorende resources (Zie de volgende opmerkingen). | Nee |
| **DebugMode** | Voorkomt dat automatisch opschonen bij fouten. | Nee |


## <a name="verify-the-deployment-using-the-azure-stack-portal"></a>Controleer of de implementatie met de Stack van Azure-portal

> [!NOTE]
>  Nadat het script voor installatie is voltooid, moet u de portal om te zien van de blade admin vernieuwen.


1. Aanmelden bij de beheerportal als de servicebeheerder.

2. Controleer of de implementatie is voltooid. Ga naar **resourcegroepen**. Selecteer vervolgens de **system.\< locatie\>.sqladapter** resourcegroep. Controleer of alle vier implementaties is voltooid.

      ![Controleer of de implementatie van de SQL-resourceprovider](./media/azure-stack-sql-rp-deploy/sqlrp-verify.png)


## <a name="update-the-sql-resource-provider-adapter-multi-node-only-builds-1710-and-later"></a>Bijwerken van de SQL resource provider-adapter (met meerdere knooppunten alleen bij builds 1710 en hoger)
Een nieuwe SQL resource provider-adapter kan worden vrijgegeven wanneer Azure Stack-builds worden bijgewerkt. Terwijl de bestaande adapter werken blijft, wordt u aangeraden zo snel mogelijk naar de laatste build bijwerken. Updates moeten worden geïnstalleerd in volgorde: u kunt de versies niet overslaan (Zie de tabel in stap 3 van [implementeert de bronprovider](#deploy-the-resource-provider)).

Bijwerken van de resourceprovider die u gebruikt de *UpdateSQLProvider.ps1* script. Het proces is vergelijkbaar met het proces dat wordt gebruikt voor het installeren van een resourceprovider, zoals beschreven in de [implementeert de bronprovider](#deploy-the-resource-provider) sectie van dit artikel. Het script is opgenomen in het downloaden van de resourceprovider.

De *UpdateSQLProvider.ps1* script maakt een nieuwe virtuele machine met de meest recente code van de resource-provider en de instellingen van de oude virtuele machine naar de nieuwe virtuele machine wordt gemigreerd. De instellingen die worden gemigreerd database en het hosten van server-gegevens bevatten en de vereiste DNS-record.

Het script vereist gebruik van dezelfde argumenten die worden beschreven voor het script DeploySqlProvider.ps1. Geef het certificaat hier ook. 

Het is raadzaam dat u de installatiekopie van het meest recente Windows Server 2016 Core uit het beheer van de Marketplace downloaden. Als u een update installeert moet, kunt u één plaatsen. MSU-pakket in het pad van de lokale afhankelijkheid. Als meer dan één. MSU-bestand wordt gevonden, mislukt het script.

Hieronder volgt een voorbeeld van de *UpdateSQLProvider.ps1* script dat u vanuit de PowerShell-prompt uitvoeren kunt. Zorg ervoor dat de accountgegevens en wachtwoorden zo nodig wijzigen: 

> [!NOTE]
> Het updateproces geldt alleen voor geïntegreerde systemen.

```
# Install the AzureRM.Bootstrapper module, set the profile, and install the AzureRM and AzureStack modules.
Install-Module -Name AzureRm.BootStrapper -Force
Use-AzureRmProfile -Profile 2017-03-09-profile
Install-Module -Name AzureStack -RequiredVersion 1.2.11 -Force

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

### <a name="updatesqlproviderps1-parameters"></a>UpdateSQLProvider.ps1 parameters
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
| **DebugMode** | Voorkomt dat automatisch opschonen bij fouten. | Nee |


## <a name="collect-diagnostic-logs"></a>Diagnostische logboeken verzamelen
De SQL-resourceprovider is de virtuele machine een vergrendeld. Als het nodig zijn voor het verzamelen van Logboeken van de virtuele machine, een PowerShell net genoeg Administration (JEA)-eindpunt wordt _DBAdapterDiagnostics_ is opgegeven voor dit doel. Er zijn twee opdrachten beschikbaar via dit eindpunt:

* Get-AzsDBAdapterLog - bereidt een zip-pakket met RP diagnostische logboeken en plaatst deze op de schijf van de sessie van de gebruiker. De opdracht kan worden aangeroepen zonder parameters en de laatste vier uur van Logboeken verzamelt.
* Remove-AzsDBAdapterLog - ruimt bestaande logboek-pakketten op de resourceprovider VM

Een gebruikersaccount aangeroepen _dbadapterdiag_ is gemaakt tijdens de RP-implementatie of update voor de verbinding met het eindpunt van de diagnostische gegevens voor het uitpakken van RP-Logboeken. Het wachtwoord van dit account is hetzelfde als het wachtwoord voor het lokale administrator-account tijdens de implementatie-update.

Voor het gebruik van deze opdrachten moet u voor het maken van een externe PowerShell-sessie op de virtuele machine van de resource provider en de opdracht aanroepen. U kunt desgewenst FromDate en ToDate parameters opgeven. Als u een of beide van deze niet opgeeft, worden de FromDate vier uur vóór de huidige tijd en de ToDate worden de huidige tijd.

Dit voorbeeldscript wordt getoond hoe het gebruik van deze opdrachten:

```
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

## <a name="maintenance-operations-integrated-systems"></a>Onderhoudsbewerkingen (geïntegreerde systemen)
De SQL-resourceprovider is de virtuele machine een vergrendeld. Bijwerken van de resource provider van de virtuele machine kan worden uitgevoerd via het eindpunt PowerShell net genoeg Administration (JEA) _DBAdapterMaintenance_.

Een script wordt geleverd bij de RP-installatiepakket te vergemakkelijken van deze bewerkingen.

### <a name="update-the-virtual-machine-operating-system"></a>Werk het besturingssysteem van de virtuele machine
Er zijn verschillende manieren om bij te werken van Windows Server-VM:
* Installeer het meest recente resource provider-pakket met de installatiekopie van een momenteel patches Windows Server 2016 Core
* Een Windows Update-pakket installeren tijdens de installatie of het bijwerken van de RP


### <a name="update-the-virtual-machine-windows-defender-definitions"></a>De virtuele machine Windows Defender-definities bijwerken

Volg deze stappen voor de Defender-definities bijwerken:

1. Download de Windows Defender-definities vanuit bijwerken [Windows Defender-definitie](https://www.microsoft.com/en-us/wdsi/definitions)

    Klik op deze pagina onder 'Handmatig downloaden en installeren van de definities' downloaden ' Windows Defender Antivirus voor Windows 10 en Windows 8.1 "64-bits bestand. 
    
    Directe koppeling: https://go.microsoft.com/fwlink/?LinkID=121721&arch=x64

2. Een PowerShell-sessie naar SQL RP-adapter van de virtuele machine onderhoud eindpunt maken
3. Kopieer het bestand van de update definities de DB-adapter-machine met de onderhoudsmodus endpoint-sessie
4. Op het onderhoud PowerShell sessie aanroepen de _Update DBAdapterWindowsDefenderDefinitions_ opdracht
5. Na de installatie, is het aanbevolen het updatebestand definities verwijderen. Het kan worden verwijderd op de onderhoud sessie met de _verwijderen ItemOnUserDrive)_ opdracht.


Hier volgt een voorbeeld van een script om bij te werken de Defender definities (vervang deze door het adres of de naam van de virtuele machine met de werkelijke waarde):

```
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

## <a name="remove-the-sql-resource-provider-adapter"></a>Verwijder de adapter SQL resource provider

Als u wilt verwijderen van de resourceprovider, is het essentieel voor Verwijder eerst alle afhankelijkheden.

1. Zorg ervoor dat u het oorspronkelijke implementatiepakket dat u voor deze versie van de adapter SQL resource provider hebt gedownload.

2. Alle gebruikersdatabases moeten worden verwijderd van de resourceprovider. (De databases niet verwijderen van de gegevens.) Deze taak moet worden uitgevoerd door de gebruikers zelf.

3. De beheerder moet de hosting-servers verwijderen van de SQL resource provider-adapter.

4. De beheerder moet de schema's die verwijzen naar de SQL resource provider-adapter verwijderen.

5. De beheerder moet elk SKU's en quota's die gekoppeld aan de SQL resource provider-adapter zijn verwijderen.

6. Opnieuw uit het script voor implementatie met de volgende elementen:
    - De - parameter verwijderen
    - De Azure Resource Manager-eindpunten
    - The DirectoryTenantID
    - De referenties voor de service administrator-account


## <a name="next-steps"></a>Volgende stappen

[Hosting-servers toevoegen](azure-stack-sql-resource-provider-hosting-servers.md) en [maken van databases](azure-stack-sql-resource-provider-databases.md).

Probeer andere [PaaS services](azure-stack-tools-paas-services.md) zoals de [MySQL-Server resourceprovider](azure-stack-mysql-resource-provider-deploy.md) en de [resourceprovider App Services](azure-stack-app-service-overview.md).
