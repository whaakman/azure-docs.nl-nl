---
title: Met behulp van SQL-databases op Azure-Stack | Microsoft Docs
description: Meer informatie over hoe u SQL-databases kan implementeren als een service op Azure-Stack en de snelle stappen voor het implementeren van de SQL Server resource provider-adapter
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
ms.date: 12/14/2017
ms.author: JeffGo
ms.openlocfilehash: 111b6274f4a3633fa4dd367866bf4e4e72d6e2df
ms.sourcegitcommit: 3fca41d1c978d4b9165666bb2a9a1fe2a13aabb6
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/15/2017
---
# <a name="use-sql-databases-on-microsoft-azure-stack"></a>SQL-databases op Microsoft Azure-Stack gebruiken

*Van toepassing op: Azure Stack geïntegreerde systemen en Azure Stack Development Kit*

Gebruik van de adapter van SQL Server resource provider voor SQL-databases weergeven als een service van [Azure Stack](azure-stack-poc.md). Na het installeren van de resourceprovider en verbinden met een of meer exemplaren van SQL Server, kunnen u en uw gebruikers maken:
- databases voor cloud-systeemeigen apps
- websites op basis van SQL
- werkbelastingen die zijn gebaseerd op SQL u beschikt niet over voor het inrichten van een virtuele machine (VM) die als host fungeert voor SQL Server elke keer.

De resourceprovider biedt geen ondersteuning voor alle database beheermogelijkheden van [Azure SQL Database](https://azure.microsoft.com/services/sql-database/). Bijvoorbeeld, pools voor elastische databases en de mogelijkheid om de prestaties van de database omhoog en omlaag automatisch bellen zijn niet beschikbaar. Echter, de resource provider biedt ondersteuning voor vergelijkbare maken, lezen, bijwerken en verwijderen (CRUD)-bewerkingen. De API is niet compatibel met SQL-database.

## <a name="sql-resource-provider-adapter-architecture"></a>SQL Resource Provider Adapter-architectuur
De resourceprovider bestaat uit drie onderdelen:

- **De SQL resource provider adapter VM**, dit is een virtuele Windows-computer waarop de provider-services wordt uitgevoerd.
- **De resourceprovider zelf**, die inrichting verzoeken verwerkt en resources van de database zichtbaar gemaakt.
- **Servers waarop SQL Server worden gehost**, die een capaciteit voor databases, Servers die als host fungeert aangeroepen.

U moet een (of meer) SQL-servers maken en/of toegang tot de externe SQL-exemplaren.

## <a name="deploy-the-resource-provider"></a>De resourceprovider implementeren

1. Als u dit nog niet hebt gedaan, registreren van uw development kit en downloaden van de installatiekopie van het Windows Server 2016 Datacenter Core downloadbare via Marketplace-beheer. U kunt een installatiekopie van Windows Server 2016 Core moet gebruiken. U kunt ook een script maken van een [installatiekopie van Windows Server 2016](https://docs.microsoft.com/azure/azure-stack/azure-stack-add-default-image) -moet u de core-optie te selecteren. De runtime .NET 3.5 is niet langer vereist.

2. Aanmelden bij een host die toegang heeft tot de bevoegde Endpoint-virtuele machine.

    a. Op Azure Stack Development Kit (ASDK)-installaties, moet u zich aanmelden bij de fysieke host.

    b. Op systemen met meerdere knooppunten moet de host een systeem dat toegang heeft tot de bevoegde eindpunt. 
    
    >[!NOTE]
    > Het systeem waarop het script wordt uitgevoerd *moet* worden van een Windows 10 of Windows Server 2016-systeem met de meest recente versie van de .NET runtime geïnstalleerd. Installatie mislukt anders. De host ASDK voldoet aan deze criteria.


3. De binaire SQL resourceprovider downloaden en uitvoeren van de zelfstandig uitpakken om de inhoud naar een tijdelijke map te pakken.

    >[!NOTE] 
    > De resource provider-build correspondeert met de Azure-Stack builds. U moet het juiste binaire bestand voor de versie van Azure-Stack met downloaden.

    | Azure Stack Build | SQL RP-installatieprogramma |
    | --- | --- |
    | 1.0.171122.1 | [SQL RP versie 1.1.12.0](https://aka.ms/azurestacksqlrp) |
    | 1.0.171028.1 | [SQL RP versie 1.1.8.0](https://aka.ms/azurestacksqlrp1710) |
    | 1.0.170928.3 | [SQL RP versie 1.1.3.0](https://aka.ms/azurestacksqlrp1709) |
   

4. Het Azure-Stack-basiscertificaat wordt opgehaald uit het bevoegde eindpunt. Voor ASDK, een zelfondertekend certificaat gemaakt als onderdeel van dit proces. Voor meerdere knooppunten, moet u een geschikt certificaat opgeven.

    Als u moet uw eigen certificaat op te geven, moet u een PFX-bestand geplaatst de **DependencyFilesLocalPath** (Zie hieronder) als volgt:

    - Een jokertekencertificaat voor \*.dbadapter.\< regio\>.\< externe fqdn\> of een certificaat voor één site met een algemene naam van sqladapter.dbadapter.\< regio\>.\< externe FQDN-naam\>
    - Dit certificaat moet worden vertrouwd, zoals zou zijn uitgegeven door een certificeringsinstantie. Dat wil zeggen moet de keten van vertrouwensrelatie bestaan zonder tussenliggende certificaten.
    - Een enkele certificaatbestand bestaat in de DependencyFilesLocalPath.
    - De bestandsnaam moet de speciale tekens niet bevatten.


5. Open een **nieuwe** (beheerdersrechten) PowerShell-console met verhoogde bevoegdheid en de wijzigingen in de map waar u de bestanden hebt uitgepakt. Een nieuw venster gebruiken om problemen die kunnen worden veroorzaakt door onjuiste PowerShell-modules die al is geladen, op het systeem te voorkomen.

6. [Installeer Azure PowerShell versie 1.2.11](azure-stack-powershell-install.md).

7. Voer het script DeploySqlProvider.ps1 met de onderstaande parameters.

Het script voert de volgende stappen uit:

- De certificaten en andere artefacten uploaden naar een opslagaccount op uw Azure-Stack.
- Galerie pakketten publiceren zodat u de SQL-databases via de galerie kunt implementeren.
- Publiceren van een gallery-pakket voor het implementeren van Servers die als host fungeert
- Een virtuele machine met behulp van de installatiekopie van het Windows Server 2016 gemaakt in stap 1 implementeren en installeren van de resourceprovider.
- Een lokale DNS-record dat is toegewezen aan uw VM-resourceprovider registreren.
- Registreer uw resourceprovider met de lokale Azure Resource Manager (gebruiker en beheer).

> [!NOTE]
> Als de installatie van meer dan 90 minuten duurt, mislukken mogelijk en er een foutbericht op het scherm en in het logboekbestand, maar de implementatie van de mislukte stap wordt geprobeerd. Systemen die niet voldoen aan de aanbevolen specificaties voor geheugen en vCPU kan mogelijk niet de SQL RP implementeren.
>

Hier volgt een voorbeeld u vanuit het PowerShell uitvoeren kunt vragen (maar desgewenst wijzigen van de accountgegevens en wachtwoorden):

```
# Install the AzureRM.Bootstrapper module, set the profile, and install AzureRM and AzureStack modules
Install-Module -Name AzureRm.BootStrapper -Force
Use-AzureRmProfile -Profile 2017-03-09-profile
Install-Module -Name AzureStack -RequiredVersion 1.2.11 -Force

# Use the NetBIOS name for the Azure Stack domain. On ASDK, the default is AzureStack and the default prefix is AzS
# For integrated systems, the domain and the prefix will be the same.
$domain = "AzureStack"
$prefix = "AzS"
$privilegedEndpoint = "$prefix-ERCS01"

# Point to the directory where the RP installation files were extracted
$tempDir = 'C:\TEMP\SQLRP'

# The service admin account (can be AAD or ADFS)
$serviceAdmin = "admin@mydomain.onmicrosoft.com"
$AdminPass = ConvertTo-SecureString "P@ssw0rd1" -AsPlainText -Force
$AdminCreds = New-Object System.Management.Automation.PSCredential ($serviceAdmin, $AdminPass)

# Set credentials for the new Resource Provider VM
$vmLocalAdminPass = ConvertTo-SecureString "P@ssw0rd1" -AsPlainText -Force
$vmLocalAdminCreds = New-Object System.Management.Automation.PSCredential ("sqlrpadmin", $vmLocalAdminPass)

# and the cloudadmin credential required for Privileged Endpoint access
$CloudAdminPass = ConvertTo-SecureString "P@ssw0rd1" -AsPlainText -Force
$CloudAdminCreds = New-Object System.Management.Automation.PSCredential ("$domain\cloudadmin", $CloudAdminPass)

# change the following as appropriate
$PfxPass = ConvertTo-SecureString "P@ssw0rd1" -AsPlainText -Force

# Change directory to the folder where you extracted the installation files
# and adjust the endpoints
. $tempDir\DeploySQLProvider.ps1 -AzCredential $AdminCreds `
  -VMLocalCredential $vmLocalAdminCreds `
  -CloudAdminCredential $cloudAdminCreds `
  -PrivilegedEndpoint $privilegedEndpoint `
  -DefaultSSLCertificatePassword $PfxPass `
  -DependencyFilesLocalPath $tempDir\cert
 ```

### <a name="deploysqlproviderps1-parameters"></a>DeploySqlProvider.ps1 parameters
U kunt deze parameters opgeven op de opdrachtregel. Als u dit niet doet, of parametervalidatie mislukt, wordt u gevraagd om de vereiste waarden.

| Parameternaam | Beschrijving | Opmerking of een standaardwaarde |
| --- | --- | --- |
| **CloudAdminCredential** | De referentie voor de beheerder van de cloud, nodig voor toegang tot de bevoegde eindpunt. | _Vereist_ |
| **AzCredential** | Geef de referenties voor de servicebeheerder voor Azure-Stack-account. De dezelfde referenties gebruiken als u gebruikt voor het implementeren van Azure-Stack). | _Vereist_ |
| **VMLocalCredential** | Definieer de referenties voor het lokale administrator-account van de SQL-resourceprovider VM. | _Vereist_ |
| **PrivilegedEndpoint** | Geef het IP-adres of de DNS-naam van het eindpunt Privleged. |  _Vereist_ |
| **DependencyFilesLocalPath** | Het PFX-certificaatbestand moet in deze map ook worden geplaatst. | _optionele_ (_verplichte_ voor meerdere knooppunten) |
| **DefaultSSLCertificatePassword** | Het wachtwoord voor het pfx-certificaat | _Vereist_ |
| **MaxRetryCount** | Opgeven hoe vaak u wilt dat elke bewerking opnieuw als er een fout.| 2 |
| **RetryDuration** | Definieer de time-out tussen nieuwe pogingen, in seconden. | 120 |
| **Verwijderen** | Verwijder de resourceprovider en alle bijbehorende resources (Zie opmerkingen hieronder) | Nee |
| **Fouten opsporen-modus** | Voorkomt dat automatisch opschonen bij fout | Nee |


## <a name="verify-the-deployment-using-the-azure-stack-portal"></a>Controleer of de implementatie met de Stack van Azure-portal

> [!NOTE]
>  Nadat het script voor installatie is voltooid, moet u de portal om te zien van de blade admin vernieuwen.


1. Aanmelden bij de beheerportal als de servicebeheerder.

2. Controleer of de implementatie is voltooid. Bladeren naar **resourcegroepen** &gt;, klikt u op de **system.\< locatie\>.sqladapter** resource groep en controleer of alle vier implementaties is voltooid.

      ![Controleer of de implementatie van de SQL-RP](./media/azure-stack-sql-rp-deploy/sqlrp-verify.png)


## <a name="remove-the-sql-resource-provider-adapter"></a>Verwijder de Adapter SQL Resource Provider

Als u wilt verwijderen van de resourceprovider, is het essentieel voor Verwijder eerst alle afhankelijkheden.

1. Zorg ervoor dat u hebt het oorspronkelijke implementatiepakket dat u voor deze versie van de Adapter SQL Resource Provider hebt gedownload.

2. Alle gebruikersdatabases moeten worden verwijderd van de resourceprovider (dit wordt niet de gegevens verwijderd). Dit moet worden uitgevoerd door de gebruikers zelf.

3. Beheerder moet de hosting-servers verwijderen van de SQL Resource Provider-Adapter

4. Beheerder moet de schema's die verwijzen naar de SQL Resource Provider-Adapter verwijderen.

5. Beheerder moet een SKU's en quota die zijn gekoppeld aan de SQL Resource Provider-Adapter verwijderen.

6. Opnieuw uitvoeren van het script voor implementatie met de - parameter, Azure Resource Manager-eindpunten DirectoryTenantID en referenties voor de service administrator-account verwijderen.


## <a name="next-steps"></a>Volgende stappen

[Toevoegen van Servers die als host fungeert](azure-stack-sql-resource-provider-hosting-servers.md) en [maken van databases](azure-stack-sql-resource-provider-databases.md).

Probeer andere [PaaS services](azure-stack-tools-paas-services.md) zoals de [MySQL-Server resourceprovider](azure-stack-mysql-resource-provider-deploy.md) en de [resourceprovider App Services](azure-stack-app-service-overview.md).
