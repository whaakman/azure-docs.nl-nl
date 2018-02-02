---
title: Met behulp van SQL-databases op Azure-Stack | Microsoft Docs
description: Meer informatie over hoe u SQL-databases kan implementeren als een service op Azure-Stack en de snelle stappen voor het implementeren van de SQL Server resource provider-adapter.
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
ms.date: 01/09/2018
ms.author: JeffGo
ms.openlocfilehash: bf52ed4986b4e0930b57721c0e38bbf748045a36
ms.sourcegitcommit: 9d317dabf4a5cca13308c50a10349af0e72e1b7e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/01/2018
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

U moet een (of meer) intances van SQL Server maken en/of toegang tot de externe SQL Server-exemplaren.

## <a name="deploy-the-resource-provider"></a>De resourceprovider implementeren

1. Als u dit nog niet hebt gedaan, registreren van uw development kit en downloaden van de installatiekopie van het Windows Server 2016 Datacenter Core downloadbare via Marketplace-beheer. U kunt een installatiekopie van Windows Server 2016 Core moet gebruiken. U kunt ook een script maken van een [installatiekopie van Windows Server 2016](https://docs.microsoft.com/azure/azure-stack/azure-stack-add-default-image). (Zorg ervoor dat de core-optie). De runtime .NET 3.5 is niet langer vereist.

2. Aanmelden bij een host die toegang heeft tot de bevoegde VM-eindpunt.

    - Op Azure Stack Development Kit-installaties, moet u zich aanmelden bij de fysieke host.

    - Op systemen met meerdere knooppunten moet de host een systeem dat toegang heeft tot de bevoegde eindpunt.
    
    >[!NOTE]
    > Het systeem waarop het script wordt uitgevoerd *moet* worden van een Windows 10 of Windows Server 2016-systeem met de meest recente versie van de .NET runtime geïnstalleerd. Anders mislukt de installatie. De Stack-SDK van Azure-host voldoet aan dit criterium.


3. De SQL-resourceprovider binaire downloaden. Voer vervolgens de zelfstandig uitpakken om de inhoud naar een tijdelijke map te pakken.

    >[!NOTE] 
    > De resource provider-build correspondeert met de Azure-Stack builds. Zorg ervoor dat het juiste binaire bestand voor de versie van Azure-Stack met downloaden.

    | Azure Stack build | SQL resource provider-installatieprogramma |
    | --- | --- |
    |1.0.180102.3, 1.0.180103.2 of 1.0.180106.1 (met meerdere knooppunten) | [SQL RP versie 1.1.14.0](https://aka.ms/azurestacksqlrp1712) |
    | 1.0.171122.1 | [SQL RP versie 1.1.12.0](https://aka.ms/azurestacksqlrp1711) |
    | 1.0.171028.1 | [SQL RP versie 1.1.8.0](https://aka.ms/azurestacksqlrp1710) |
  

4. Het Azure-Stack-basiscertificaat wordt opgehaald uit het bevoegde eindpunt. Voor de Stack Azure SDK een zelfondertekend certificaat gemaakt als onderdeel van dit proces. Voor meerdere knooppunten, moet u een geschikt certificaat opgeven.

   Voor uw eigen certificaat, plaatst u een .pfx-bestand in de **DependencyFilesLocalPath** als volgt:

    - Een jokertekencertificaat voor \*.dbadapter.\< regio\>.\< externe fqdn\> of een certificaat voor één site met een algemene naam van sqladapter.dbadapter.\< regio\>.\< externe fqdn\>.

    - Dit certificaat moet worden vertrouwd. Dat wil zeggen moet de keten van vertrouwensrelatie bestaan zonder tussenliggende certificaten.

    - Een enkele certificaatbestand bestaat in de DependencyFilesLocalPath.

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

> [!NOTE]
> Als de installatie van meer dan 90 minuten duurt, kan het mislukken. Als dit mislukt, wordt er een foutbericht op het scherm en in het logboekbestand, maar de implementatie van de mislukte stap wordt geprobeerd. Systemen die niet voldoen aan de aanbevolen specificaties voor geheugen en vCPU niet mogelijk voor het implementeren van de provider voor SQL resoure.
>

Hier volgt een voorbeeld die u vanuit de PowerShell-prompt uitvoeren kunt. (Zorg ervoor dat de accountgegevens en -wachtwoorden zo nodig wijzigen.)

```
# Install the AzureRM.Bootstrapper module, set the profile, and install the AzureRM and AzureStack modules.
Install-Module -Name AzureRm.BootStrapper -Force
Use-AzureRmProfile -Profile 2017-03-09-profile
Install-Module -Name AzureStack -RequiredVersion 1.2.11 -Force

# Use the NetBIOS name for the Azure Stack domain. On the Azure Stack SDK, the default is AzureStack and the default prefix is AzS.
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

# Set credentials for the new Resource Provider VM.
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
Wanneer de Azure-Stack-build wordt bijgewerkt, wordt een nieuwe SQL-resource provider adapter uitgebracht. De bestaande adapter mogelijk blijven werken. We raden echter bijwerken naar de laatste build zo snel mogelijk nadat de Azure-Stack is bijgewerkt. 

Het updateproces is vergelijkbaar met het installatieproces dat eerder beschreven. U kunt een nieuwe virtuele machine maken met de meest recente resource provider-code. Bovendien migreren u instellingen naar deze nieuwe instantie met inbegrip van de database en het hosten van servergegevens. U kunt ook de benodigde DNS-record migreren.

Het script UpdateSQLProvider.ps1 gebruiken met dezelfde argumenten die we eerder beschreven. Geef hier het certificaat ook.

> [!NOTE]
> Dit updateproces wordt alleen ondersteund op systemen met meerdere knooppunten.

```
# Install the AzureRM.Bootstrapper module, set the profile, and install the AzureRM and AzureStack modules.
Install-Module -Name AzureRm.BootStrapper -Force
Use-AzureRmProfile -Profile 2017-03-09-profile
Install-Module -Name AzureStack -RequiredVersion 1.2.11 -Force

# Use the NetBIOS name for the Azure Stack domain. On the Azure Stack SDK, the default is AzureStack and the default prefix is AzS.
# For integrated systems, the domain and the prefix are the same.
$domain = "AzureStack"
$prefix = "AzS"
$privilegedEndpoint = "$prefix-ERCS01"

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
