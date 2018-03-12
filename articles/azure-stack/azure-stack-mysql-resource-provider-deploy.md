---
title: MySQL-databases als PaaS op Azure-Stack gebruiken | Microsoft Docs
description: Ontdek hoe u implementeert de Bronprovider van MySQL en MySQL-databases opgeven als een service op Azure-Stack.
services: azure-stack
documentationCenter: 
author: mattbriggs
manager: femila
editor: 
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/07/2018
ms.author: mabrigg
ms.reviewer: jeffgo
ms.openlocfilehash: 15a1648193555ecc5847170ab65f48dfa4f6417b
ms.sourcegitcommit: 8c3267c34fc46c681ea476fee87f5fb0bf858f9e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/09/2018
---
# <a name="use-mysql-databases-on-microsoft-azure-stack"></a>MySQL-database gebruiken op Microsoft Azure-Stack

*Van toepassing op: Azure Stack geïntegreerde systemen en Azure Stack Development Kit*

U kunt een MySQL-resourceprovider op Azure-Stack implementeren. Nadat u de resourceprovider implementeert, kunt u MySQL-servers en -databases via Azure Resource Manager-implementatiesjablonen maken. U kunt ook de MySQL-databases opgeven als een service. 

MySQL-databases die op websites gelden, ondersteuning voor verschillende platforms van de website. Bijvoorbeeld: nadat u de resourceprovider hebt geïmplementeerd, kunt u WordPress websites van het platform voor Web-Apps als een service (PaaS)-invoegtoepassing voor Azure-Stack.

Kopieer het bestand voor het implementeren van de MySQL-provider op een systeem dat geen toegang tot Internet heeft [mysql-connector-net-6.10.5.msi](https://dev.mysql.com/get/Download/sConnector-Net/mysql-connector-net-6.10.5.msi) naar een lokale share. Die sharenaam vervolgens opgeven wanneer u wordt gevraagd om deze. U moet de Azure en Azure Stack PowerShell-modules installeren.


## <a name="mysql-server-resource-provider-adapter-architecture"></a>MySQL-Server resource provider adapter-architectuur

De resourceprovider bestaat uit drie onderdelen:

- **De MySQL resource provider adapter VM**, dit is een virtuele Windows-computer waarop de provider-services wordt uitgevoerd.

- **De resourceprovider zelf**, die inrichting verzoeken verwerkt en resources van de database zichtbaar gemaakt.

- **Servers die als host MySQL-Server fungeren**, die een capaciteit voor databases die worden genoemd die als host fungeert voor servers.

Deze release maakt niet langer MySQL-exemplaren. Dit betekent dat u moet ze zelf maken en/of toegang tot de externe SQL-exemplaren. Ga naar de [Azure Stack Snelstartgalerie](https://github.com/Azure/AzureStack-QuickStart-Templates/tree/master/mysql-standalone-server-windows) voor een van de voorbeeldsjabloon die kunt:
- Maak een MySQL-server voor u.
- Downloaden en implementeren van een MySQL-Server uit Azure Marketplace.

> [!NOTE]
> Hosting-servers die zijn geïnstalleerd op Azure-Stack moeten geïntegreerde systemen worden gemaakt van een tenantabonnement. Ze kunnen niet worden gemaakt van het standaard provider-abonnement. Ze moeten worden gemaakt vanuit de tenantportal of vanuit een PowerShell-sessie met een juiste aanmelden. Alle hosting-servers toerekenbare VM's zijn en moeten de juiste licenties hebt. De servicebeheerder kan de eigenaar van de tenant-abonnement zijn.

### <a name="required-privileges"></a>Vereiste bevoegdheden
Het systeem-account moet hebben de volgende bevoegdheden:

1.  Database: Maken, verwijderen
2.  Aanmelding: Maken, instellen, verwijderen, toekennen, intrekken

## <a name="deploy-the-resource-provider"></a>De resourceprovider implementeren

1. Als u dit nog niet hebt gedaan, registreren van uw development kit en downloaden van de installatiekopie van het Windows Server 2016 Datacenter Core downloadbare via Marketplace-beheer. U kunt een installatiekopie van Windows Server 2016 Core moet gebruiken. U kunt ook een script maken van een [installatiekopie van Windows Server 2016](https://docs.microsoft.com/azure/azure-stack/azure-stack-add-default-image). (Zorg ervoor dat de core-optie te selecteren.)


2. Aanmelden bij een host die toegang heeft tot de bevoegde VM-eindpunt.

    - Bij installaties van de Azure SDK, moet u zich aanmelden bij de fysieke host. 
    - Op systemen met meerdere knooppunten moet de host een systeem dat toegang heeft tot de bevoegde eindpunt.
    
    >[!NOTE]
    > Het systeem waarop het script wordt uitgevoerd *moet* worden van een Windows 10 of Windows Server 2016-systeem met de meest recente versie van de .NET runtime geïnstalleerd. Anders mislukt de installatie. De Stack-SDK van Azure-host voldoet aan dit criterium.
    

3. Download de binaire MySQL resourceprovider. Voer vervolgens de zelfstandig uitpakken om de inhoud naar een tijdelijke map te pakken.

    >[!NOTE] 
    > De resourceprovider heeft een minimale bijbehorende Azure-Stack bouwen. Zorg ervoor dat het juiste binaire bestand voor de versie van Azure-Stack met downloaden.

    | Azure Stack build | MySQL RP-installatieprogramma |
    | --- | --- |
    | 1802: 1.0.180302.1 | [MySQL RP versie 1.1.18.0](https://aka.ms/azurestackmysqlrp1802) |
    | 1712: 1.0.180102.3 of 1.0.180106.1 (met meerdere knooppunten) | [MySQL RP versie 1.1.14.0](https://aka.ms/azurestackmysqlrp1712) |
    | 1711: 1.0.171122.1 | [MySQL RP versie 1.1.12.0](https://aka.ms/azurestackmysqlrp1711) |
    | 1710: 1.0.171028.1 | [MySQL RP versie 1.1.8.0](https://aka.ms/azurestackmysqlrp1710) |

4.  Het Azure-Stack-basiscertificaat wordt opgehaald uit het bevoegde eindpunt. Voor de Azure-SDK een zelfondertekend certificaat gemaakt als onderdeel van dit proces. Voor meerdere knooppunten, moet u een geschikt certificaat opgeven.

    Als u moet uw eigen certificaat op te geven, plaatst u een .pfx-bestand in de **DependencyFilesLocalPath** die voldoet aan de volgende criteria:

    - Een jokertekencertificaat voor \*.dbadapter.\< regio\>.\< externe fqdn\> of een certificaat voor één site met een algemene naam van mysqladapter.dbadapter.\< regio\>.\< externe fqdn\>.

    - Dit certificaat moet worden vertrouwd. Dat wil zeggen moet de keten van vertrouwensrelatie bestaan zonder tussenliggende certificaten.

    - Een enkele certificaatbestand bestaat in de DependencyFilesLocalPath.
    
    - De bestandsnaam mag geen eventuele speciale tekens of spaties bevatten.


5. Open een **nieuwe** (beheerdersrechten) PowerShell-console met verhoogde bevoegdheid. Wijzig in de map waar u de bestanden hebt uitgepakt. Een nieuw venster gebruiken om problemen die mogelijk worden veroorzaakt door onjuiste PowerShell-modules die al zijn geladen in het systeem te voorkomen.

6. [Installeer Azure PowerShell versie 1.2.11](azure-stack-powershell-install.md).

7. Voer het `DeployMySqlProvider.ps1`-script uit.

Het script voert de volgende stappen uit:

* Downloadt het MySQL connector binaire bestand (dit kan worden opgegeven offline).
* De certificaten en andere artefacten uploadt naar een opslagaccount op Azure-Stack.
* Publiceert galerie pakketten zodat u de SQL-databases via de galerie kunt implementeren.
* Publiceert een gallery-pakket voor het implementeren van de hosting-servers.
* Een virtuele machine implementeert met behulp van de installatiekopie van het Windows Server 2016 die is gemaakt in stap 1. Ook de resourceprovider wordt geïnstalleerd.
* Registreert een lokale DNS-record dat is toegewezen aan uw VM-resourceprovider.
* Registreert uw resourceprovider met de lokale Azure Resource Manager (tenant en beheer).


U kunt:
- Geef de vereiste parameters op de opdrachtregel.
- Voer zonder parameters en geef ze wanneer u wordt gevraagd.

Hier volgt een voorbeeld die u vanuit de PowerShell-prompt uitvoeren kunt. Zorg ervoor dat de accountgegevens en wachtwoorden zo nodig wijzigen:


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
$tempDir = 'C:\TEMP\MYSQLRP'

# The service admin account (can be Azure Active Directory or Active Directory Federation Services).
$serviceAdmin = "admin@mydomain.onmicrosoft.com"
$AdminPass = ConvertTo-SecureString "P@ssw0rd1" -AsPlainText -Force
$AdminCreds = New-Object System.Management.Automation.PSCredential ($serviceAdmin, $AdminPass)

# Set the credentials for the new resource provider VM local administrator account
$vmLocalAdminPass = ConvertTo-SecureString "P@ssw0rd1" -AsPlainText -Force
$vmLocalAdminCreds = New-Object System.Management.Automation.PSCredential ("mysqlrpadmin", $vmLocalAdminPass)

# And the cloudadmin credential required for privileged endpoint access.
$CloudAdminPass = ConvertTo-SecureString "P@ssw0rd1" -AsPlainText -Force
$CloudAdminCreds = New-Object System.Management.Automation.PSCredential ("$domain\cloudadmin", $CloudAdminPass)

# Change the following as appropriate.
$PfxPass = ConvertTo-SecureString "P@ssw0rd1" -AsPlainText -Force

# Run the installation script from the folder where you extracted the installation files.
# Find the ERCS01 IP address first, and make sure the certificate
# file is in the specified directory.
. $tempDir\DeployMySQLProvider.ps1 -AzCredential $AdminCreds `
  -VMLocalCredential $vmLocalAdminCreds `
  -CloudAdminCredential $cloudAdminCreds `
  -PrivilegedEndpoint $privilegedEndpoint `
  -DefaultSSLCertificatePassword $PfxPass `
  -DependencyFilesLocalPath $tempDir\cert `
  -AcceptLicense

 ```


### <a name="deploymysqlproviderps1-parameters"></a>DeployMySqlProvider.ps1 parameters
U kunt deze parameters opgeven op de opdrachtregel. Als u dit niet doet, of als er parametervalidatie mislukt, wordt u gevraagd de vereiste parameters bevatten.

| Parameternaam | Beschrijving | Opmerking of de standaardwaarde |
| --- | --- | --- |
| **CloudAdminCredential** | De referentie voor de beheerder van de cloud, nodig voor toegang tot de bevoegde eindpunt. | _Vereist_ |
| **AzCredential** | De referenties voor de Azure-Stack-admin-serviceaccount. Gebruik dezelfde referenties die u gebruikt voor de implementatie van Azure-Stack. | _Vereist_ |
| **VMLocalCredential** | De referenties voor het lokale administrator-account van de resourceprovider MySQL VM. | _Vereist_ |
| **PrivilegedEndpoint** | De IP-adres of de DNS-naam van het bevoegde eindpunt. |  _Vereist_ |
| **DependencyFilesLocalPath** | Pad naar een lokale share met [mysql-connector-net-6.10.5.msi](https://dev.mysql.com/get/Downloads/Connector-Net/mysql-connector-net-6.10.5.msi). Als u een van deze paden opgeeft, moet het certificaatbestand dat u in deze map ook worden geplaatst. | _Optionele_ (_verplichte_ voor meerdere knooppunten) |
| **DefaultSSLCertificatePassword** | Het wachtwoord voor het pfx-certificaat. | _Vereist_ |
| **MaxRetryCount** | Het aantal keren dat u wilt opnieuw proberen aan elke als er een storing optreedt.| 2 |
| **RetryDuration** | De time-interval tussen nieuwe pogingen, in seconden. | 120 |
| **Verwijderen** | Hiermee verwijdert u de resourceprovider en alle bijbehorende resources (Zie de volgende opmerkingen). | Nee |
| **DebugMode** | Voorkomt dat automatisch opschonen bij fouten. | Nee |
| **AcceptLicense** | Slaat de prompt voor het accepteren van de licentie GPL.  (http://www.gnu.org/licenses/old-licenses/gpl-2.0.html) | |


## <a name="verify-the-deployment-by-using-the-azure-stack-portal"></a>Controleer of de implementatie met behulp van de Stack van Azure-portal

> [!NOTE]
>  Nadat het script voor installatie is voltooid, moet u de portal om te zien van de blade admin vernieuwen.


1. Aanmelden bij de beheerportal als de servicebeheerder.

2. Controleer of de implementatie is voltooid. Ga naar **resourcegroepen**, en selecteer vervolgens de **system.\< locatie\>.mysqladapter** resourcegroep. Controleer of alle vier implementaties is voltooid.

      ![Controleer of de implementatie van de MySQL RP](./media/azure-stack-mysql-rp-deploy/mysqlrp-verify.png)

## <a name="provide-capacity-by-connecting-to-a-mysql-hosting-server"></a>Een capaciteit door verbinding te maken met een MySQL-hostingserver

1. Aanmelden bij de Azure-Stack-portal als de beheerder van een service.

2. Selecteer **SERVERVIRTUALISATIE** > **MySQL-Servers die als host fungeert** > **+ toevoegen**.

    Op de **MySQL-Servers die als host fungeert** blade kunt u de resourceprovider MySQL-Server verbinden met de werkelijke instanties van MySQL-Server die als back-end van de resourceprovider fungeert.

    ![Hosting-servers](./media/azure-stack-mysql-rp-deploy/mysql-add-hosting-server-2.png)

3. Geef de details van de verbinding van uw MySQL-Server-exemplaar. Zorg ervoor dat de volledig gekwalificeerde domeinnaam (FQDN) of een geldig IPv4-adres en niet korte naam van de VM. Deze installatie niet langer biedt een standaardexemplaar MySQL. De grootte die opgegeven, helpt de resourceprovider voor het beheren van de capaciteit voor de database. Deze moet dicht bij de fysieke capaciteit van de database-server.

    > [!NOTE]
    >Als de MySQL-exemplaar kan worden geopend door de tenant en Azure Resource Manager-beheerder, kan deze worden geplaatst onder het beheer van de resourceprovider. Het exemplaar MySQL *moet* worden toegewezen aan de resourceprovider.

4. Als u servers toevoegt, moet u ze toewijzen aan een nieuwe of bestaande SKU om toe te staan van differentiatie van serviceaanbiedingen.
  U kunt bijvoorbeeld hebben een enterprise-exemplaar leveren:
    - capaciteit voor de database
    - Automatisch back-up
    - servers met hoge prestaties voor afzonderlijke afdelingen reserveren
 

De SKU-naam moet overeenkomen met de eigenschappen zodat tenants hun databases op de juiste wijze kunnen plaatsen. Alle hosting-servers in een SKU moeten dezelfde mogelijkheden hebben.

![Maken van een MySQL-SKU](./media/azure-stack-mysql-rp-deploy/mysql-new-sku.png)


>[!NOTE]
> SKU's kunnen een uur duren zichtbaar in de portal. U kunt een database kan niet maken, tot de SKU is gemaakt.


## <a name="test-your-deployment-by-creating-your-first-mysql-database"></a>Uw implementatie testen door uw eerste MySQL-database te maken


1. Aanmelden bij de Azure-Stack-portal als de beheerder van een service.

2. Selecteer **+ nieuw** > **gegevens en opslag** > **MySQL-Database**.

3. Geef de databasedetails.

    ![Een test MySQL-database maken](./media/azure-stack-mysql-rp-deploy/mysql-create-db.png)

4. Selecteer een SKU.

    ![Selecteer een SKU](./media/azure-stack-mysql-rp-deploy/mysql-select-a-sku.png)

5. De instelling van een aanmelding maken. U kunt een bestaande instelling van de aanmelding opnieuw of maak een nieuwe. Deze instelling bevat de gebruikersnaam en wachtwoord voor de database.

    ![Maak een nieuwe databaseaanmelding](./media/azure-stack-mysql-rp-deploy/create-new-login.png)

    De verbindingsreeks bevat de naam van de bestaande database-server. Kopieer de verbindingsreeks van de portal.

    ![De verbindingsreeks ophalen voor de MySQL-database](./media/azure-stack-mysql-rp-deploy/mysql-db-created.png)

> [!NOTE]
> De lengte van de gebruikersnamen mag maximaal 32 tekens MySQL 5.7. In eerdere versies, mag deze 16 tekens niet overschrijden.


## <a name="add-capacity"></a>Toevoegen van capaciteit

Capaciteit toevoegen door het toevoegen van aanvullende MySQL-servers in de Stack van Azure-portal. Extra servers kunnen worden toegevoegd aan een nieuwe of bestaande SKU. Zorg ervoor dat de kenmerken van de server hetzelfde zijn.


## <a name="make-mysql-databases-available-to-tenants"></a>MySQL-databases beschikbaar te maken voor tenants
Plannen en aanbiedingen MySQL-databases om beschikbaar te maken voor tenants maken. Bijvoorbeeld: de service Microsoft.MySqlAdapter toevoegen, een target toevoegen, enzovoort.

![Plannen en aanbiedingen voor zijn onder meer databases maken](./media/azure-stack-mysql-rp-deploy/mysql-new-plan.png)

## <a name="update-the-administrative-password"></a>Het beheerderswachtwoord bijwerken
U kunt het wachtwoord wijzigen door eerst op de server-exemplaar van MySQL wijzigen. Selecteer **SERVERVIRTUALISATIE** > **MySQL-Servers die als host fungeert**. Selecteer vervolgens de hostserver. In de **instellingen** Configuratiescherm, selecteer **wachtwoord**.

![Het beheerderswachtwoord bijwerken](./media/azure-stack-mysql-rp-deploy/mysql-update-password.png)

## <a name="update-the-mysql-resource-provider-adapter-multi-node-only-builds-1710-and-later"></a>Bijwerken van de MySQL resource provider-adapter (met meerdere knooppunten alleen bij builds 1710 en hoger)
Een nieuwe SQL resource provider-adapter kan worden vrijgegeven wanneer Azure Stack-builds worden bijgewerkt. Terwijl de bestaande adapter werken blijft, wordt u aangeraden zo snel mogelijk naar de laatste build bijwerken. 

Bijwerken van de resourceprovider die u gebruikt de *UpdateMySQLProvider.ps1* script. Het proces is vergelijkbaar met het proces dat wordt gebruikt voor het installeren van een resourceprovider, zoals beschreven in de [implementeert de bronprovider](#deploy-the-resource-provider) sectie van dit artikel. Het script is opgenomen in het downloaden van de resourceprovider.

De *UpdateMySQLProvider.ps1* script maakt een nieuwe virtuele machine met de meest recente code van de resource-provider en de instellingen van de oude virtuele machine naar de nieuwe virtuele machine wordt gemigreerd. De instellingen die worden gemigreerd database en het hosten van server-gegevens bevatten en de vereiste DNS-record.

Het script vereist gebruik van dezelfde argumenten die worden beschreven voor het script DeployMySqlProvider.ps1. Geef het certificaat hier ook. 

Hieronder volgt een voorbeeld van de *UpdateMySQLProvider.ps1* script dat u vanuit de PowerShell-prompt uitvoeren kunt. Zorg ervoor dat de accountgegevens en wachtwoorden zo nodig wijzigen: 

> [!NOTE]
> Het updateproces geldt alleen voor geïntegreerde systemen.

```
# Install the AzureRM.Bootstrapper module, set the profile, and install AzureRM and AzureStack modules.
Install-Module -Name AzureRm.BootStrapper -Force
Use-AzureRmProfile -Profile 2017-03-09-profile
Install-Module -Name AzureStack -RequiredVersion 1.2.11 -Force

# Use the NetBIOS name for the Azure Stack domain. On the Azure Stack SDK, the default is AzureStack but could have been changed at install time.
$domain = "AzureStack"

# For integrated systems, use the IP address of one of the ERCS virtual machines
$privilegedEndpoint = "AzS-ERCS01"

# Point to the directory where the resource provider installation files were extracted.
$tempDir = 'C:\TEMP\MYSQLRP'

# The service admin account (can be Azure Active Directory or Active Directory Federation Services).
$serviceAdmin = "admin@mydomain.onmicrosoft.com"
$AdminPass = ConvertTo-SecureString "P@ssw0rd1" -AsPlainText -Force
$AdminCreds = New-Object System.Management.Automation.PSCredential ($serviceAdmin, $AdminPass)

# Set credentials for the new resource provider VM.
$vmLocalAdminPass = ConvertTo-SecureString "P@ssw0rd1" -AsPlainText -Force
$vmLocalAdminCreds = New-Object System.Management.Automation.PSCredential ("sqlrpadmin", $vmLocalAdminPass)

# And the cloudadmin credential required for privileged endpoint access.
$CloudAdminPass = ConvertTo-SecureString "P@ssw0rd1" -AsPlainText -Force
$CloudAdminCreds = New-Object System.Management.Automation.PSCredential ("$domain\cloudadmin", $CloudAdminPass)

# Change the following as appropriate.
$PfxPass = ConvertTo-SecureString "P@ssw0rd1" -AsPlainText -Force

# Change directory to the folder where you extracted the installation files.
# Then adjust the endpoints.
. $tempDir\UpdateMySQLProvider.ps1 -AzCredential $AdminCreds `
  -VMLocalCredential $vmLocalAdminCreds `
  -CloudAdminCredential $cloudAdminCreds `
  -PrivilegedEndpoint $privilegedEndpoint `
  -DefaultSSLCertificatePassword $PfxPass `
  -DependencyFilesLocalPath $tempDir\cert `
  -AcceptLicense
 ```

### <a name="updatemysqlproviderps1-parameters"></a>UpdateMySQLProvider.ps1 parameters
U kunt deze parameters opgeven op de opdrachtregel. Als u dit niet, of als er parametervalidatie mislukt, wordt u gevraagd de vereiste parameters bevatten.

| Parameternaam | Beschrijving | Opmerking of de standaardwaarde |
| --- | --- | --- |
| **CloudAdminCredential** | De referentie voor de beheerder van de cloud, nodig voor toegang tot de bevoegde eindpunt. | _Vereist_ |
| **AzCredential** | De referenties voor de Azure-Stack-admin-serviceaccount. Dezelfde referenties gebruiken dat u gebruikt voor het implementeren van Azure-Stack. | _Vereist_ |
| **VMLocalCredential** |De referenties voor het lokale administrator-account van de SQL-resourceprovider VM. | _Vereist_ |
| **PrivilegedEndpoint** | De IP-adres of de DNS-naam van het bevoegde eindpunt. |  _Vereist_ |
| **DependencyFilesLocalPath** | Het PFX-certificaatbestand moet in deze map ook worden geplaatst. | _Optionele_ (_verplichte_ voor meerdere knooppunten) |
| **DefaultSSLCertificatePassword** | Het wachtwoord voor het pfx-certificaat | _Vereist_ |
| **MaxRetryCount** | Het aantal keren dat u wilt opnieuw proberen aan elke als er een storing optreedt.| 2 |
| **RetryDuration** | De time-interval tussen nieuwe pogingen, in seconden. | 120 |
| **Verwijderen** | Verwijder de resourceprovider en alle bijbehorende resources (Zie de volgende opmerkingen). | Nee |
| **DebugMode** | Voorkomt dat automatisch opschonen bij fouten. | Nee |
| **AcceptLicense** | Slaat de prompt voor het accepteren van de licentie GPL.  (http://www.gnu.org/licenses/old-licenses/gpl-2.0.html) | |


## <a name="collect-diagnostic-logs"></a>Diagnostische logboeken verzamelen
De MySQL-resourceprovider is de virtuele machine een vergrendeld. Als het nodig zijn voor het verzamelen van Logboeken van de virtuele machine, een PowerShell net genoeg Administration (JEA)-eindpunt wordt _DBAdapterDiagnostics_ is opgegeven voor dit doel. Er zijn twee opdrachten beschikbaar via dit eindpunt:

* Get-AzsDBAdapterLog - bereidt een zip-pakket met RP diagnostische logboeken en plaatst deze op de schijf van de sessie van de gebruiker. De opdracht kan worden aangeroepen zonder parameters en de laatste vier uur van Logboeken verzamelt.
* Remove-AzsDBAdapterLog - ruimt bestaande logboek-pakketten op de resourceprovider VM

Een gebruikersaccount aangeroepen _dbadapterdiag_ is gemaakt tijdens de RP-implementatie of update voor de verbinding met het eindpunt van de diagnostische gegevens voor het uitpakken van RP-Logboeken. Het wachtwoord van dit account is hetzelfde als het wachtwoord voor het lokale administrator-account tijdens de implementatie-update.

Voor het gebruik van deze opdrachten moet u een externe PowerShell-sessie met de resource provider virtuele machine maken en de opdracht aanroepen. U kunt desgewenst FromDate en ToDate parameters opgeven. Als u een of beide van deze niet opgeeft, worden de FromDate vier uur vóór de huidige tijd en de ToDate worden de huidige tijd.

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
De MySQL-resourceprovider is de virtuele machine een vergrendeld. Bijwerken van de resource provider van de virtuele machine kan worden uitgevoerd via het eindpunt PowerShell net genoeg Administration (JEA) _DBAdapterMaintenance_.

Een script wordt geleverd bij de RP-installatiepakket te vergemakkelijken van deze bewerkingen.


### <a name="update-the-virtual-machine-operating-system"></a>Werk het besturingssysteem van de virtuele machine
Er zijn verschillende manieren om bij te werken van Windows Server-VM:
* Installeer het meest recente resource provider-pakket met de installatiekopie van een momenteel patches Windows Server 2016 Core
* Een Windows Update-pakket installeren tijdens de installatie of het bijwerken van de RP


### <a name="update-the-virtual-machine-windows-defender-definitions"></a>De virtuele machine Windows Defender-definities bijwerken

Volg deze stappen voor de Defender-definities bijwerken:

1. Download de Windows Defender-definities vanuit bijwerken [Windows Defender-definitie](https://www.microsoft.com/en-us/wdsi/definitions)

    Klik op deze pagina onder 'Handmatig downloaden en installeren van de definities' downloaden ' Windows Defender Antivirus voor Windows 10 en Windows 8.1 "64-bits bestand. 
    
    Direct link: https://go.microsoft.com/fwlink/?LinkID=121721&arch=x64

2. Een PowerShell-sessie met MySQL RP-adapter van de virtuele machine onderhoud eindpunt maken
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


## <a name="remove-the-mysql-resource-provider-adapter"></a>Verwijder de adapter MySQL resource provider

Als u wilt verwijderen van de resourceprovider, is het essentieel voor Verwijder eerst alle afhankelijkheden.

1. Zorg ervoor dat u het oorspronkelijke implementatiepakket dat u hebt gedownload voor deze versie van de resourceprovider.

2. Alle tenant databases moeten worden verwijderd van de resourceprovider. (De tenant-databases niet verwijderen van de gegevens.) Deze taak moet worden uitgevoerd door de huurder zelf.

3. Tenants moeten Hef de registratie van de naamruimte.

4. De beheerder moet de hosting-servers verwijderen van de MySQL-Adapter.

5. De beheerder moet de schema's die verwijzen naar de MySQL-Adapter verwijderen.

6. De beheerder moet geen quota's die gekoppeld aan de MySQL-Adapter zijn verwijderen.

7. Opnieuw uit het script voor implementatie met de volgende elementen:
    - De - parameter verwijderen
    - De Azure Resource Manager-eindpunten
    - The DirectoryTenantID
    - De referenties voor de service administrator-account
