---
title: MySQL-databases als PaaS op Azure-Stack gebruiken | Microsoft Docs
description: Ontdek hoe u implementeert de Bronprovider van MySQL en MySQL-databases opgeven als een service op Azure-Stack.
services: azure-stack
documentationCenter: 
author: mattbriggs
manager: bradleyb
editor: 
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/10/2018
ms.author: mabrigg
ms.openlocfilehash: 3273f435cb65411c85e3a22369682d51e7a12baf
ms.sourcegitcommit: 95500c068100d9c9415e8368bdffb1f1fd53714e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/14/2018
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
> Hosting-servers die zijn geïnstalleerd op een Azure-Stack-implementatie met meerdere knooppunten moet worden gemaakt van een tenantabonnement. Ze kunnen niet worden gemaakt van het standaard provider-abonnement. Ze moeten worden gemaakt vanuit de tenantportal of vanuit een PowerShell-sessie met een juiste aanmelden. Alle hosting-servers toerekenbare VM's zijn en moeten de juiste licenties hebt. De servicebeheerder kan de eigenaar van de tenant-abonnement zijn.

### <a name="required-privileges"></a>Vereiste bevoegdheden
Het systeem-account moet hebben de volgende bevoegdheden:

1.  Database: Maken, verwijderen
2.  Aanmelding: Maken, instellen, verwijderen, toekennen, intrekken

## <a name="deploy-the-resource-provider"></a>De resourceprovider implementeren

1. Als u dit nog niet hebt gedaan, registreren van uw development kit en downloaden van de installatiekopie van het Windows Server 2016 Datacenter Core downloadbare via Marketplace-beheer. U kunt een installatiekopie van Windows Server 2016 Core moet gebruiken. U kunt ook een script maken van een [installatiekopie van Windows Server 2016](https://docs.microsoft.com/azure/azure-stack/azure-stack-add-default-image). (Zorg ervoor dat de core-optie te selecteren.) De runtime .NET 3.5 is niet langer vereist.


2. Aanmelden bij een host die toegang heeft tot de bevoegde VM-eindpunt.

    - Bij installaties van de Azure SDK, moet u zich aanmelden bij de fysieke host. 
    - Op systemen met meerdere knooppunten moet de host een systeem dat toegang heeft tot de bevoegde eindpunt.
    
    >[!NOTE]
    > Het systeem waarop het script wordt uitgevoerd *moet* worden van een Windows 10 of Windows Server 2016-systeem met de meest recente versie van de .NET runtime geïnstalleerd. Anders mislukt de installatie. De Azure SDK host voldoet aan deze criteria.
    

3. Download de binaire MySQL resourceprovider. Voer vervolgens de zelfstandig uitpakken om de inhoud naar een tijdelijke map te pakken.

    >[!NOTE] 
    > De resource provider-build overeenkomt met de opbouw van de Azure-Stack. Zorg ervoor dat het juiste binaire bestand voor de versie van Azure-Stack met downloaden.

    | Azure Stack build | MySQL RP-installatieprogramma |
    | --- | --- |
    | 1.0.180102.3 of 1.0.180106.1 (met meerdere knooppunten) | [MySQL RP versie 1.1.14.0](https://aka.ms/azurestackmysqlrp1712) |
    | 1.0.171122.1 | [MySQL RP versie 1.1.12.0](https://aka.ms/azurestackmysqlrp1711) |
    | 1.0.171028.1 | [MySQL RP versie 1.1.8.0](https://aka.ms/azurestackmysqlrp1710) |

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

# Use the NetBIOS name for the Azure Stack domain. On the Azure SDK, the default is AzureStack, and the default prefix is AzS.
# For integrated systems, the domain and the prefix are the same.
$domain = "AzureStack"
$prefix = "AzS"
$privilegedEndpoint = "$prefix-ERCS01"

# Point to the directory where the resource provider installation files were extracted.
$tempDir = 'C:\TEMP\MYSQLRP'

# The service admin account (can be Azure Active Directory or Active Directory Federation Services).
$serviceAdmin = "admin@mydomain.onmicrosoft.com"
$AdminPass = ConvertTo-SecureString "P@ssw0rd1" -AsPlainText -Force
$AdminCreds = New-Object System.Management.Automation.PSCredential ($serviceAdmin, $AdminPass)

# Set the credentials for the new resource provider VM.
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


### <a name="deploysqlproviderps1-parameters"></a>DeploySqlProvider.ps1 parameters
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



Afhankelijk van de snelheid van de prestaties en downloaden voor system, installatie slechts 20 minuten of als long als enkele uren duren. Als de **MySQLAdapter** blade is niet beschikbaar, vernieuw de beheerportal.

> [!NOTE]
> Als de installatie van meer dan 90 minuten duurt, kan het mislukken. Als dit het geval is, ziet u een foutbericht op het scherm en in het logboekbestand. De implementatie wordt van de mislukte stap geprobeerd. Systemen die niet voldoen aan de aanbevolen specificaties voor geheugen en core kan mogelijk niet de MySQL RP implementeren.



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
Wanneer de Azure-Stack-build wordt bijgewerkt, wordt een nieuwe MySQL resource provider-adapter wordt vrijgegeven. De bestaande adapter mogelijk blijven werken. We raden echter bijwerken naar de laatste build zo snel mogelijk nadat de Azure-Stack is bijgewerkt. 

Het updateproces is vergelijkbaar met het installatieproces die eerder is beschreven. U kunt een nieuwe virtuele machine maken met de meest recente resource provider-code. Vervolgens migreert u de instellingen voor dit nieuwe exemplaar, met inbegrip van de database en het hosten van servergegevens. U kunt ook de benodigde DNS-record migreren.

Het script UpdateMySQLProvider.ps1 gebruiken met dezelfde argumenten die eerder zijn beschreven. Geef het certificaat hier ook.

> [!NOTE]
> Bijwerken wordt alleen ondersteund op systemen met meerdere knooppunten.

```
# Install the AzureRM.Bootstrapper module, set the profile, and install AzureRM and AzureStack modules.
Install-Module -Name AzureRm.BootStrapper -Force
Use-AzureRmProfile -Profile 2017-03-09-profile
Install-Module -Name AzureStack -RequiredVersion 1.2.11 -Force

# Use the NetBIOS name for the Azure Stack domain. On the Azure SDK, the default is AzureStack and the default prefix is AzS.
# For integrated systems, the domain and the prefix are the same.
$domain = "AzureStack"
$prefix = "AzS"
$privilegedEndpoint = "$prefix-ERCS01"

# Point to the directory where the resource provider installation files were extracted.
$tempDir = 'C:\TEMP\SQLRP'

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
