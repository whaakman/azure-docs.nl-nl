---
title: Met behulp van MySQL-databases op Azure-Stack | Microsoft Docs
description: Meer informatie over hoe u MySQL-databases kan implementeren als een service op Azure-Stack en de snelle stappen voor het implementeren van de resource provider-adapter van MySQL-Server.
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
ms.date: 06/25/2018
ms.author: jeffgilb
ms.reviewer: jeffgo
ms.openlocfilehash: e4c3eb1d7dfd4894576d5fbed52cf4de5fed9e44
ms.sourcegitcommit: 828d8ef0ec47767d251355c2002ade13d1c162af
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/25/2018
ms.locfileid: "36938109"
---
# <a name="deploy-the-mysql-resource-provider-on-azure-stack"></a>De resourceprovider MySQL op Azure-Stack implementeren

De resourceprovider MySQL-Server te kunnen stellen MySQL-databases als een Azure-Stack-service gebruiken. De MySQL-resourceprovider als een service wordt uitgevoerd op een Windows Server 2016 Server Core-virtuele machine (VM).

## <a name="prerequisites"></a>Vereisten

Er zijn verschillende vereisten die worden voldaan moet voordat u de Azure-Stack MySQL-resourceprovider kunt implementeren. Om te voldoen aan deze vereisten, de stappen in dit artikel op een computer die toegang heeft tot de bevoegde VM-eindpunt.

* Als u dit nog niet hebt gedaan, [registreren Azure Stack](.\azure-stack-registration.md) met Azure zodat u via Azure marketplace-items downloaden kunt.
* De vereiste Windows Server core VM toevoegen aan de Stack van Azure marketplace downloaden de **Windows Server 2016 Datacenter - Server Core** installatiekopie. U kunt ook een script maken van een [installatiekopie van Windows Server 2016](https://docs.microsoft.com/azure/azure-stack/azure-stack-add-default-image). Zorg ervoor dat u de core-optie selecteert wanneer u het script uitvoert.

  >[!NOTE]
  >Als u een update installeert moet, kunt u één plaatsen. MSU-pakket in het pad van de lokale afhankelijkheid. Als meer dan één. MSU-bestand wordt gevonden, mislukt de installatie van de MySQL resource provider.

* De binaire MySQL resourceprovider downloaden en voer vervolgens de zelfstandig uitpakken om de inhoud naar een tijdelijke map te pakken.

  >[!NOTE]
  >Voor het implementeren van de MySQL-provider op een systeem dat geen toegang tot Internet, kopieert u de [mysql-connector-net-6.10.5.msi](https://dev.mysql.com/get/Download/sConnector-Net/mysql-connector-net-6.10.5.msi) bestand naar een lokale share. Geef de sharenaam wanneer u wordt gevraagd om deze. U moet de Azure en Azure Stack PowerShell-modules installeren.

* De resourceprovider heeft een minimale bijbehorende Azure-Stack bouwen. Zorg ervoor dat u het juiste binaire bestand voor de versie van Azure-Stack die u gebruikt downloaden.

    | Azure Stack-versie | MySQL RP-versie|
    | --- | --- |
    | Versie 1804 (1.0.180513.1)|[MySQL RP versie 1.1.24.0](https://aka.ms/azurestackmysqlrp1804) |
    | Versie 1802 (1.0.180302.1) | [MySQL RP versie 1.1.18.0](https://aka.ms/azurestackmysqlrp1802) |
    | Versie 1712 (1.0.180102.3 of 1.0.180106.1 (geïntegreerde systemen)) | [MySQL RP versie 1.1.14.0](https://aka.ms/azurestackmysqlrp1712) |

### <a name="certificates"></a>Certificaten

Voor de ASDK een zelfondertekend certificaat gemaakt als onderdeel van het installatieproces. Voor een Azure-Stack geïntegreerd, moet u een geschikt certificaat opgeven. Als u moet uw eigen certificaat op te geven, plaatst u een .pfx-bestand in de **DependencyFilesLocalPath** die voldoet aan de volgende criteria:

* Een jokertekencertificaat voor \*.dbadapter.\< regio\>.\< externe fqdn\> of een certificaat voor één site met een algemene naam van mysqladapter.dbadapter.\< regio\>.\< externe fqdn\>.
* Het certificaat moet worden vertrouwd. De vertrouwensketen moet zonder tussenliggende certificaten bestaan.
* Een enkele certificaatbestand bestaat in de DependencyFilesLocalPath.
* De bestandsnaam kan niet hebben speciale tekens of spaties.

## <a name="deploy-the-resource-provider"></a>De resourceprovider implementeren

Nadat u de vereisten die zijn geïnstalleerd hebt, voert u de **DeployMySqlProvider.ps1** script voor het implementeren van de MYSQL-resourceprovider. Het script DeployMySqlProvider.ps1 wordt opgehaald als onderdeel van het binaire MySQL resource provider die u hebt gedownload voor uw versie van Azure-Stack.

> [!IMPORTANT]
> Het systeem dat u het script uitvoert op moet een Windows 10 of Windows Server 2016-systeem met de meest recente versie van de .NET runtime geïnstalleerd.

Een nieuwe verhoogde PowerShell-consolevenster openen voor het implementeren van de resourceprovider MySQL en Ga naar de map waar u de binaire bestanden van MySQL resource provider hebt uitgepakt. U wordt aangeraden met behulp van een nieuwe PowerShell-venster om te voorkomen dat potentiële problemen veroorzaakt door een PowerShell-modules die al zijn geladen.

Voer de **DeployMySqlProvider.ps1** script, dat de volgende taken:

* De certificaten en andere artefacten uploadt naar een opslagaccount op Azure-Stack.
* Publiceert galerie pakketten zodat u kunt implementeren met de galerie MySQL-databases.
* Publiceert een gallery-pakket voor het implementeren van de hosting-servers.
* Een virtuele machine met behulp van de installatiekopie van de belangrijkste Windows Server 2016 u hebt gedownload en installeert vervolgens de MySQL-resourceprovider implementeert.
* Registreert een lokale DNS-record dat is toegewezen aan uw VM-resourceprovider.
* Met de lokale Azure Resource Manager voor de operator en gebruikersaccounts voor de resourceprovider geregistreerd.
* Desgewenst installeert één Windows Server update tijdens de installatie van de resource provider.

> [!NOTE]
> Wanneer de MySQL resource provider-implementatie wordt gestart, wordt de **system.local.mysqladapter** resourcegroep wordt gemaakt. Het kan voltooien van de vier implementaties die zijn vereist voor deze resourcegroep 75 minuten duren.

### <a name="deploymysqlproviderps1-parameters"></a>DeployMySqlProvider.ps1 parameters

U kunt deze parameters vanaf de opdrachtregel opgeven. Als u dit niet, of als een parametervalidatie mislukt, wordt u gevraagd de vereiste parameters bevatten.

| Parameternaam | Beschrijving | Opmerking of de standaardwaarde |
| --- | --- | --- |
| **CloudAdminCredential** | De referentie voor de beheerder van de cloud, nodig voor toegang tot de bevoegde eindpunt. | _Vereist_ |
| **AzCredential** | De referenties voor de Azure-Stack-admin-serviceaccount. Gebruik dezelfde referenties die u gebruikt voor de implementatie van Azure-Stack. | _Vereist_ |
| **VMLocalCredential** | De referenties voor het lokale administrator-account van de resourceprovider MySQL VM. | _Vereist_ |
| **PrivilegedEndpoint** | De IP-adres of de DNS-naam van het bevoegde eindpunt. |  _Vereist_ |
| **DependencyFilesLocalPath** | Pad naar een lokale share met [mysql-connector-net-6.10.5.msi](https://dev.mysql.com/get/Downloads/Connector-Net/mysql-connector-net-6.10.5.msi). Als u een van deze paden opgeeft, moet het certificaatbestand dat u in deze map ook worden geplaatst. | _Optionele_ voor één knooppunt _verplichte_ voor meerdere knooppunten. |
| **DefaultSSLCertificatePassword** | Het wachtwoord voor het pfx-certificaat. | _Vereist_ |
| **MaxRetryCount** | Het aantal keren dat u wilt opnieuw proberen aan elke als er een storing optreedt.| 2 |
| **RetryDuration** | De time-interval tussen nieuwe pogingen, in seconden. | 120 |
| **Verwijderen** | Hiermee verwijdert u de resourceprovider en alle bijbehorende resources (Zie de volgende opmerkingen). | Nee |
| **Fouten opsporen-modus** | Voorkomt dat automatisch opschonen bij fouten. | Nee |
| **AcceptLicense** | Slaat de prompt voor het accepteren van de licentie GPL.  <http://www.gnu.org/licenses/old-licenses/gpl-2.0.html> | |

> [!NOTE]
> SKU's kunnen een uur duren zichtbaar in de portal. U kunt een database kan niet maken, totdat de SKU geïmplementeerd en wordt uitgevoerd is.

## <a name="deploy-the-mysql-resource-provider-using-a-custom-script"></a>Implementeert de bronprovider van MySQL met een aangepast script

Om te voorkomen handmatige configuratie bij het implementeren van de resourceprovider, kunt u het volgende script aanpassen. Wijzig de standaard-accountgegevens en -wachtwoorden voor uw Azure-Stack-implementatie.

```powershell
# Install the AzureRM.Bootstrapper module and set the profile.
Install-Module -Name AzureRm.BootStrapper -Force
Use-AzureRmProfile -Profile 2017-03-09-profile

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
# Find the ERCS01 IP address first, and make sure the certificate file is in the specified directory.
. $tempDir\DeployMySQLProvider.ps1 `
    -AzCredential $AdminCreds `
    -VMLocalCredential $vmLocalAdminCreds `
    -CloudAdminCredential $cloudAdminCreds `
    -PrivilegedEndpoint $privilegedEndpoint `
    -DefaultSSLCertificatePassword $PfxPass `
    -DependencyFilesLocalPath $tempDir\cert `
    -AcceptLicense

```

Wanneer het script resource provider voor installatie is voltooid, vernieuw uw browser om te controleren of dat u de meest recente updates kunt zien.

## <a name="verify-the-deployment-by-using-the-azure-stack-portal"></a>Controleer of de implementatie met behulp van de Stack van Azure-portal

1. Aanmelden bij de beheerportal als de servicebeheerder.
2. Selecteer **resourcegroepen**
3. Selecteer de **system.\< locatie\>.mysqladapter** resourcegroep.
4. Groeperen op de pagina overzicht voor Resource overzicht van het bericht onder **implementaties** moet **3 geslaagd**.
5. Krijgt u meer gedetailleerde informatie over de implementatie van de resource provider onder **instellingen**. Selecteer **implementaties** ophalen van informatie, zoals: STATUS, TIMESTAMP en duur voor elke implementatie.

## <a name="next-steps"></a>Volgende stappen

[Hosting-servers toevoegen](azure-stack-mysql-resource-provider-hosting-servers.md)
