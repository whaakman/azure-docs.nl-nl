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
ms.date: 10/10/2017
ms.author: JeffGo
ms.openlocfilehash: 6e65af68dcd2306aabda65efdf8fe056c0d9b4a4
ms.sourcegitcommit: 6a22af82b88674cd029387f6cedf0fb9f8830afd
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/11/2017
---
# <a name="use-sql-databases-on-microsoft-azure-stack"></a>SQL-databases op Microsoft Azure-Stack gebruiken

*Van toepassing op: Azure Stack geïntegreerde systemen en Azure Stack Development Kit*

Gebruik van de adapter van SQL Server resource provider voor SQL-databases weergeven als een service van [Azure Stack](azure-stack-poc.md). Na het installeren van de resourceprovider en verbinden met een of meer exemplaren van SQL Server, kunnen u en uw gebruikers maken:
- databases voor cloud-systeemeigen apps
- websites op basis van SQL
- werkbelastingen die zijn gebaseerd op SQL u beschikt niet over voor het inrichten van een virtuele machine (VM) die als host fungeert voor SQL Server elke keer.

De resourceprovider biedt geen ondersteuning voor alle database beheermogelijkheden van [Azure SQL Database](https://azure.microsoft.com/services/sql-database/). Bijvoorbeeld, pools voor elastische databases en de mogelijkheid om de prestaties van de database omhoog en omlaag automatisch bellen zijn niet beschikbaar. Echter, de resource provider biedt ondersteuning voor vergelijkbare maken, lezen, bijwerken en verwijderen (CRUD)-bewerkingen. De API is niet compatibel met SQL-database.

## <a name="sql-server-resource-provider-adapter-architecture"></a>Architectuur van SQL Server Resource Provider netwerkadapter
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

3. [Download het bestand SQL resource provider binaire bestanden](https://aka.ms/azurestacksqlrp) en het zelfstandig uitpakken om op te halen van de inhoud naar een tijdelijke map uitvoeren.

4. Het Azure-Stack-basiscertificaat wordt opgehaald uit het bevoegde eindpunt. Voor ASDK, een zelfondertekend certificaat gemaakt als onderdeel van dit proces. Voor meerdere knooppunten, moet u een geschikt certificaat opgeven.

    Als u moet uw eigen certificaat op te geven, moet u het volgende certificaat:

    Een jokertekencertificaat voor \*.dbadapter.\< regio\>.\< externe fqdn\>. Dit certificaat moet worden vertrouwd, zoals zou zijn uitgegeven door een certificeringsinstantie. Dat wil zeggen moet de keten van vertrouwensrelatie bestaan zonder tussenliggende certificaten. Een certificaat voor één site kan worden gebruikt met expliciete naam van de VM [sqladapter] gebruikt tijdens de installatie.


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

# Use the NetBIOS name for the Azure Stack domain. On ASDK, the default is AzureStack
$domain = "AzureStack"
# Point to the directory where the RP installation files were extracted
$tempDir = 'C:\TEMP\SQLRP'

# The service admin account (can be AAD or ADFS)
$serviceAdmin = "admin@mydomain.onmicrosoft.com"
$AdminPass = ConvertTo-SecureString "P@ssw0rd1" -AsPlainText -Force
$AdminCreds = New-Object System.Management.Automation.PSCredential ($serviceAdmin, $AdminPass)

# Set the credentials for the Resource Provider VM
$vmLocalAdminPass = ConvertTo-SecureString "P@ssw0rd1" -AsPlainText -Force
$vmLocalAdminCreds = New-Object System.Management.Automation.PSCredential ("sqlrpadmin", $vmLocalAdminPass)

# and the cloudadmin credential required for Privileged Endpoint access
$CloudAdminPass = ConvertTo-SecureString "P@ssw0rd1" -AsPlainText -Force
$CloudAdminCreds = New-Object System.Management.Automation.PSCredential ("$domain\cloudadmin", $CloudAdminPass)

# change the following as appropriate
$PfxPass = ConvertTo-SecureString "P@ssw0rd1" -AsPlainText -Force

# Change directory to the folder where you extracted the installation files
# and adjust the endpoints
.$tempDir\DeploySQLProvider.ps1 -AzCredential $AdminCreds -VMLocalCredential $vmLocalAdminCreds -CloudAdminCredential $cloudAdminCreds -PrivilegedEndpoint '10.10.10.10' -DefaultSSLCertificatePassword $PfxPass -DependencyFilesLocalPath $tempDir\cert
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


## <a name="verify-the-deployment-using-the-azure-stack-portal"></a>Controleer of de implementatie met de Stack Azure Portal

> [!NOTE]
>  Nadat het script voor installatie is voltooid, moet u de portal om te zien van de blade admin vernieuwen.


1. Aanmelden bij de beheerportal als de servicebeheerder.

2. Controleer of de implementatie is voltooid. Bladeren naar **resourcegroepen** &gt;, klikt u op de **system.\< locatie\>.sqladapter** resource groep en controleer of alle vier implementaties is voltooid.

      ![Controleer of de implementatie van de SQL-RP](./media/azure-stack-sql-rp-deploy/sqlrp-verify.png)





## <a name="removing-the-sql-adapter-resource-provider"></a>De Resourceprovider voor SQL-Adapter verwijderen

Als u wilt verwijderen van de resourceprovider, is het essentieel voor Verwijder eerst alle afhankelijkheden.

1. Zorg ervoor dat u hebt het oorspronkelijke implementatiepakket dat u hebt gedownload voor deze versie van de Resourceprovider.

2. Alle gebruikersdatabases moeten worden verwijderd van de resourceprovider (dit wordt niet de gegevens verwijderd). Dit moet worden uitgevoerd door de gebruikers zelf.

3. De beheerder moet de hosting-servers verwijderen van de SQL-Adapter

4. De beheerder moet de schema's die verwijzen naar de SQL-Adapter verwijderen.

5. Beheerder moet elk SKU's en quota die zijn gekoppeld aan de SQL-Adapter verwijderen.

6. Opnieuw uitvoeren van het script voor implementatie met de - parameter, Azure Resource Manager-eindpunten DirectoryTenantID en referenties voor de service administrator-account verwijderen.


## <a name="next-steps"></a>Volgende stappen


Probeer andere [PaaS services](azure-stack-tools-paas-services.md) zoals de [MySQL-Server resourceprovider](azure-stack-mysql-resource-provider-deploy.md) en de [resourceprovider App Services](azure-stack-app-service-overview.md).
