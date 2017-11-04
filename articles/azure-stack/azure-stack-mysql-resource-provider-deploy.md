---
title: MySQL-databases als PaaS op Azure-Stack gebruiken | Microsoft Docs
description: Meer informatie over hoe u implementeert de Bronprovider van MySQL en MySQL-databases opgeven als een service op Azure-Stack
services: azure-stack
documentationCenter: 
author: JeffGoldner
manager: bradleyb
editor: 
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/10/2017
ms.author: JeffGo
ms.openlocfilehash: badaefb4986f573362babea81d704bf2be067d6b
ms.sourcegitcommit: 804db51744e24dca10f06a89fe950ddad8b6a22d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/30/2017
---
# <a name="use-mysql-databases-on-microsoft-azure-stack"></a>MySQL-database gebruiken op Microsoft Azure-Stack

*Van toepassing op: Azure Stack geïntegreerde systemen en Azure Stack Development Kit*

U kunt een MySQL-resourceprovider op Azure-Stack implementeren. Nadat u de resourceprovider hebt geïmplementeerd, kunt u MySQL-servers en -databases via Azure Resource Manager deployment sjablonen maken en MySQL-databases opgeven als een service. MySQL-databases die op websites gelden, ondersteuning voor verschillende platforms van de website. Als u bijvoorbeeld nadat u de bronprovider implementeert kunt u WordPress websites van de Web-Apps van Azure-platform als een service (PaaS)-invoegtoepassing voor Azure-Stack.

Als u wilt implementeren de MySQL-provider op een systeem die geen toegang tot internet heeft, kunt u het bestand kopiëren [mysql-connector-net-6.9.9.msi](https://dev.mysql.com/get/Download/sConnector-Net/mysql-connector-net-6.9.9.msi) naar een lokale share. Vervolgens opgeven die sharenaam wanneer u wordt gevraagd. U moet ook de Azure en Azure Stack PowerShell-modules installeren.


## <a name="mysql-server-resource-provider-adapter-architecture"></a>Architectuur van MySQL Server Resource Provider netwerkadapter

De resourceprovider bestaat uit drie onderdelen:

- **De MySQL resource provider adapter VM**, dit is een virtuele Windows-computer waarop de provider-services wordt uitgevoerd.
- **De resourceprovider zelf**, die inrichting verzoeken verwerkt en resources van de database zichtbaar gemaakt.
- **Servers die als host MySQL-Server fungeren**, die een capaciteit voor databases, Servers die als host fungeert aangeroepen.

Deze release maakt niet meer u een MySQL-exemplaar. U moet ze maken en/of toegang tot de externe SQL-exemplaren. Ga naar de [Azure Stack Snelstartgalerie](https://github.com/Azure/AzureStack-QuickStart-Templates/tree/master/mysql-standalone-server-windows) voor een van de voorbeeldsjabloon die kunt:
- een MySQL-server maken voor u
- downloaden en implementeren van een MySQL-Server in de Marketplace.

! [OPMERKING] Hosting-servers die zijn geïnstalleerd op een Azure-Stack met meerdere knooppunten moet worden gemaakt van een tenantabonnement. Ze kunnen niet worden gemaakt van het standaard Provider-abonnement. Met andere woorden, moeten ze worden gemaakt vanuit de tenantportal of vanuit een PowerShell-sessie met een juiste aanmelding. Alle hosting-servers toerekenbare VM's zijn en moeten de juiste licenties hebt. De servicebeheerder kan de eigenaar van het abonnement zijn.

### <a name="required-privileges"></a>Vereiste bevoegdheden
Het systeem-account moet hebben de volgende bevoegdheden:

1.  Database: Maken, verwijderen
2.  Aanmelding: Maken, instellen of verwijderen, toekennen, intrekken

## <a name="deploy-the-resource-provider"></a>De resourceprovider implementeren

1. Als u dit nog niet hebt gedaan, registreren van uw development kit en downloaden van de installatiekopie van het Windows Server 2016 Datacenter Core downloadbare via Marketplace-beheer. U kunt een installatiekopie van Windows Server 2016 Core moet gebruiken. U kunt ook een script maken van een [installatiekopie van Windows Server 2016](https://docs.microsoft.com/azure/azure-stack/azure-stack-add-default-image) -moet u de core-optie te selecteren. De runtime .NET 3.5 is niet langer vereist.


2. Aanmelden bij een host die toegang heeft tot de bevoegde Endpoint-virtuele machine.

    a. Op Azure Stack Development Kit (ASDK)-installaties, moet u zich aanmelden bij de fysieke host.

    b. Op systemen met meerdere knooppunten moet de host een systeem dat toegang heeft tot de bevoegde eindpunt.

3. [Download het bestand MySQL resource provider binaire bestanden](https://aka.ms/azurestackmysqlrp) en het zelfstandig uitpakken om op te halen van de inhoud naar een tijdelijke map uitvoeren.

4.  Het Azure-Stack-basiscertificaat wordt opgehaald uit het bevoegde eindpunt. Voor ASDK, een zelfondertekend certificaat gemaakt als onderdeel van dit proces. Voor meerdere knooppunten, moet u een geschikt certificaat opgeven.

    Als u moet uw eigen certificaat op te geven, moet u het volgende certificaat:

    Een jokertekencertificaat voor \*.dbadapter.\< regio\>.\< externe fqdn\>. Dit certificaat moet worden vertrouwd, zoals zou zijn uitgegeven door een certificeringsinstantie. Dat wil zeggen moet de keten van vertrouwensrelatie bestaan zonder tussenliggende certificaten. Een certificaat voor één site kan worden gebruikt met expliciete naam van de VM [mysqladapter] gebruikt tijdens de installatie.


5. Open een **nieuwe** (beheerdersrechten) PowerShell-console met verhoogde bevoegdheid en de wijzigingen in de map waar u de bestanden hebt uitgepakt. Een nieuw venster gebruiken om problemen die kunnen worden veroorzaakt door onjuiste PowerShell-modules die al is geladen, op het systeem te voorkomen.

6. [Installeer Azure PowerShell versie 1.2.11](azure-stack-powershell-install.md).

7. Voer het script DeploySqlProvider.ps1.

Het script voert de volgende stappen uit:

* Download het MySQL connector binaire bestand (dit kan worden opgegeven offline).
* De certificaten en andere artefacten uploaden naar een opslagaccount op uw Azure-Stack.
* Galerie pakketten publiceren zodat u de SQL-databases via de galerie kunt implementeren.
* Publiceren van een gallery-pakket voor het implementeren van Servers die als host fungeert
* Een virtuele machine met behulp van de installatiekopie van het Windows Server 2016 gemaakt in stap 1 implementeren en installeren van de resourceprovider.
* Een lokale DNS-record dat is toegewezen aan uw VM-resourceprovider registreren.
* Registreer uw resourceprovider met de lokale Azure Resource Manager (Tenant en beheer).


U kunt:
- Geef ten minste de vereiste parameters op de opdrachtregel
- of als u zonder parameters uitvoert, voert u ze als u wordt gevraagd.

Hier volgt een voorbeeld u vanuit het PowerShell uitvoeren kunt vragen (maar desgewenst wijzigen van de accountgegevens en wachtwoorden):


```
# Install the AzureRM.Bootstrapper module, set the profile, and install AzureRM and AzureStack modules
Install-Module -Name AzureRm.BootStrapper -Force
Use-AzureRmProfile -Profile 2017-03-09-profile
Install-Module -Name AzureStack -RequiredVersion 1.2.11 -Force

# Use the NetBIOS name for the Azure Stack domain. On ASDK, the default is AzureStack
$domain = 'AzureStack'
# Point to the directory where the RP installation files were extracted
$tempDir = 'C:\TEMP\MYSQLRP'

# The service admin account (can be AAD or ADFS)
$serviceAdmin = "admin@mydomain.onmicrosoft.com"
$AdminPass = ConvertTo-SecureString "P@ssw0rd1" -AsPlainText -Force
$AdminCreds = New-Object System.Management.Automation.PSCredential ($serviceAdmin, $AdminPass)

# Set the credentials for the Resource Provider VM
$vmLocalAdminPass = ConvertTo-SecureString "P@ssw0rd1" -AsPlainText -Force
$vmLocalAdminCreds = New-Object System.Management.Automation.PSCredential ("mysqlrpadmin", $vmLocalAdminPass)

# and the cloudadmin credential required for Privleged Endpoint access
$CloudAdminPass = ConvertTo-SecureString "P@ssw0rd1" -AsPlainText -Force
$CloudAdminCreds = New-Object System.Management.Automation.PSCredential ("$domain\cloudadmin", $CloudAdminPass)

# change the following as appropriate
$PfxPass = ConvertTo-SecureString "P@ssw0rd1" -AsPlainText -Force

# Run the installation script from the folder where you extracted the installation files
# Find the ERCS01 IP address first and make sure the certificate
# file is in the specified directory
.$tempDir\DeployMySQLProvider.ps1 -AzCredential $AdminCreds `
  -VMLocalCredential $vmLocalAdminCreds `
  -CloudAdminCredential $cloudAdminCreds `
  -PrivilegedEndpoint '10.10.10.10' `
  -DefaultSSLCertificatePassword $PfxPass -DependencyFilesLocalPath $tempDir\cert `
  -AcceptLicense

 ```

### <a name="deploymysqlproviderps1-parameters"></a>DeployMySqlProvider.ps1 parameters

U kunt deze parameters opgeven op de opdrachtregel. Als u dit niet doet, of parametervalidatie mislukt, wordt u gevraagd om de vereiste waarden.

| Parameternaam | Beschrijving | Opmerking of een standaardwaarde |
| --- | --- | --- |
| **CloudAdminCredential** | De referentie voor de beheerder van de cloud, nodig voor toegang tot het eindpunt Privleged. | _Vereist_ |
| **AzCredential** | Geef de referenties voor de servicebeheerder voor Azure-Stack-account. De dezelfde referenties gebruiken als u gebruikt voor het implementeren van Azure-Stack). | _Vereist_ |
| **VMLocalCredential** | Definieer de referenties voor het lokale administrator-account van de resourceprovider MySQL VM. | _Vereist_ |
| **PrivilegedEndpoint** | Geef het IP-adres of de DNS-naam van het bevoegde eindpunt. |  _Vereist_ |
| **DependencyFilesLocalPath** | Pad naar een lokale share met [mysql-connector-net-6.9.9.msi](https://dev.mysql.com/get/Downloads/Connector-Net/mysql-connector-net-6.9.9.msi). Als u een opgeeft, moet het certificaatbestand dat u in deze map ook worden geplaatst. | _optionele_ (_verplichte_ voor meerdere knooppunten) |
| **DefaultSSLCertificatePassword** | Het wachtwoord voor het pfx-certificaat | _Vereist_ |
| **MaxRetryCount** | Opgeven hoe vaak u wilt dat elke bewerking opnieuw als er een fout.| 2 |
| **RetryDuration** | Definieer de time-out tussen nieuwe pogingen, in seconden. | 120 |
| **Verwijderen** | Verwijder de resourceprovider en alle bijbehorende resources (Zie opmerkingen hieronder) | Nee |
| **Fouten opsporen-modus** | Voorkomt dat automatisch opschonen bij fout | Nee |
| **AcceptLicense** | Slaat de prompt voor het accepteren van de licentie GPL (http://www.gnu.org/licenses/old-licenses/gpl-2.0.html) | |


Afhankelijk van de snelheid van de prestaties en downloaden voor system, kan installatie slechts 20 minuten of als long als enkele uren duren. Als de blade MySQLAdapter niet beschikbaar is, vernieuwt u de beheerportal.

> [!NOTE]
> Als de installatie van meer dan 90 minuten duurt, mislukken mogelijk en ziet u een foutbericht op het scherm en in het logboekbestand. De implementatie wordt van de mislukte stap geprobeerd. Systemen die niet voldoen aan de aanbevolen specificaties voor geheugen en core kan mogelijk niet de MySQL RP implementeren.



## <a name="verify-the-deployment-using-the-azure-stack-portal"></a>Controleer of de implementatie met de Stack Azure Portal

> [!NOTE]
>  Nadat het script voor installatie is voltooid, moet u de portal om te zien van de blade admin vernieuwen.


1. Aanmelden bij de beheerportal als de servicebeheerder.

2. Controleer of de implementatie is voltooid. Bladeren naar **resourcegroepen** &gt;, klikt u op de **system.\< locatie\>.mysqladapter** resource groep en controleer of alle vier implementaties is voltooid.

      ![Controleer of de implementatie van de MySQL-RP](./media/azure-stack-mysql-rp-deploy/mysqlrp-verify.png)

## <a name="provide-capacity-by-connecting-to-a-mysql-hosting-server"></a>Een capaciteit door verbinding te maken met een MySQL-hostingserver

1. Aanmelden bij de Azure-Stack-portal als de beheerder van een service.

2. Blader naar **SERVERVIRTUALISATIE** &gt; **MySQL-Servers die als host fungeert** &gt; **+ toevoegen**.

    De **MySQL-Servers die als host fungeert** blade is waar kunt u de Bronprovider van MySQL Server verbinden met de werkelijke instanties van MySQL-Server die als back-end van de resourceprovider fungeert.

    ![Hosting-Servers](./media/azure-stack-mysql-rp-deploy/mysql-add-hosting-server-2.png)

3. Vul het formulier met de details van de verbinding van uw MySQL-Server-exemplaar. Geef de volledig gekwalificeerde domeinnaam (FQDN) of een geldig IPv4-adres en niet korte naam van de VM. Deze installatie niet langer biedt een standaardexemplaar MySQL. De opgegeven grootte helpt de resourceprovider voor het beheren van de capaciteit voor de database. Deze moet dicht bij de fysieke capaciteit van de database-server.

    > [!NOTE]
    > Zolang de MySQL-exemplaar kan worden geopend door de tenant en Azure Resource Manager-beheerder, kan deze worden geplaatst onder het beheer van de resourceprovider. Het exemplaar MySQL __moet__ worden toegewezen aan de RP uitsluitend.

4. Als u servers toevoegt, moet u ze toewijzen aan een nieuwe of bestaande SKU om toe te staan van differentiatie van serviceaanbiedingen.
  U kunt bijvoorbeeld hebben een enterprise-exemplaar leveren:
    - capaciteit voor de database
    - Automatische back-up
    - servers met hoge prestaties voor afzonderlijke afdelingen reserveren
    - enzovoort.
    De SKU-naam moet overeenkomen met de eigenschappen zodat tenants hun databases op de juiste wijze kunnen plaatsen. Alle hosting-servers in een SKU moeten dezelfde mogelijkheden hebben.

    ![Maken van een MySQL-SKU](./media/azure-stack-mysql-rp-deploy/mysql-new-sku.png)


>[!NOTE]
SKU's kunnen een uur duren zichtbaar in de portal. U kunt een database kan niet maken, tot de SKU is gemaakt.


## <a name="to-test-your-deployment-create-your-first-mysql-database"></a>Als u wilt testen van uw implementatie, uw eerste MySQL-database maken


1. Meld u aan bij de Azure-Stack-portal als servicebeheerder.

2. Klik op de **+ nieuw** knop &gt; **gegevens en opslag** &gt; **MySQL-Database**.

3. Vul in het formulier met de informatie over de database.

    ![Een test MySQL-database maken](./media/azure-stack-mysql-rp-deploy/mysql-create-db.png)

4. Selecteer een SKU.

    ![Selecteer een SKU](./media/azure-stack-mysql-rp-deploy/mysql-select-a-sku.png)

5. De instelling van een aanmelding maken. De instelling van de aanmelding opnieuw kan worden gebruikt of een nieuw gemaakt. Deze instelling bevat de gebruikersnaam en wachtwoord voor de database.

    ![Maak een nieuwe databaseaanmelding](./media/azure-stack-mysql-rp-deploy/create-new-login.png)

    De verbindingsreeks bevat de naam van de bestaande database-server. Kopieer de verbindingsreeks van de portal.

    ![De verbindingsreeks ophalen voor de MySQL-database](./media/azure-stack-mysql-rp-deploy/mysql-db-created.png)

> [!NOTE]
> De lengte van de gebruikersnamen kan niet groter zijn dan 32 tekens met MySQL 5.7 of 16 tekens in eerdere versies.


## <a name="add-capacity"></a>Toevoegen van capaciteit

Capaciteit toevoegen door het toevoegen van aanvullende MySQL-servers in de Stack van Azure-portal. Extra servers kunnen worden toegevoegd aan een nieuwe of bestaande SKU. Zorg ervoor dat de kenmerken van de server hetzelfde zijn.


## <a name="making-mysql-databases-available-to-tenants"></a>MySQL-databases maken beschikbaar voor tenants
Plannen en aanbiedingen MySQL-databases om beschikbaar te maken voor tenants maken. De service Microsoft.MySqlAdapter toevoegen, Voeg een quotum, enzovoort.

![Plannen en aanbiedingen voor zijn onder meer databases maken](./media/azure-stack-mysql-rp-deploy/mysql-new-plan.png)

## <a name="updating-the-administrative-password"></a>Het beheerderswachtwoord bijwerken
U kunt het wachtwoord wijzigen door eerst op de server-exemplaar van MySQL wijzigen. Blader naar **SERVERVIRTUALISATIE** &gt; **MySQL-Servers die als host fungeert** &gt; en klik op de hostserver. Klik op wachtwoord in het deelvenster instellingen.

![Het beheerderswachtwoord bijwerken](./media/azure-stack-mysql-rp-deploy/mysql-update-password.png)

## <a name="removing-the-mysql-adapter-resource-provider"></a>De Resourceprovider voor MySQL-Adapter verwijderen

Als u wilt verwijderen van de resourceprovider, is het essentieel voor Verwijder eerst alle afhankelijkheden.

1. Zorg ervoor dat u hebt het oorspronkelijke implementatiepakket dat u hebt gedownload voor deze versie van de Resourceprovider.

2. Alle tenant databases moeten worden verwijderd van de resourceprovider (dit wordt niet de gegevens verwijderd). Dit moet worden uitgevoerd door de huurder zelf.

3. Tenants moeten Hef de registratie van de naamruimte.

4. Beheerder moet de hosting-servers uit de MySQL-Adapter verwijderen

5. De beheerder moet de schema's die verwijzen naar de MySQL-Adapter verwijderen.

6. De beheerder moet geen quota's die zijn gekoppeld aan de MySQL-Adapter verwijderen.

7. Opnieuw uitvoeren van het script voor implementatie met de - parameter, Azure Resource Manager-eindpunten DirectoryTenantID en referenties voor de service administrator-account verwijderen.




## <a name="next-steps"></a>Volgende stappen


Probeer andere [PaaS services](azure-stack-tools-paas-services.md) zoals de [SQL Server-resourceprovider](azure-stack-sql-resource-provider-deploy.md) en de [resourceprovider App Services](azure-stack-app-service-overview.md).
