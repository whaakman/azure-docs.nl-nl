---
title: Met behulp van de MySQL-databases in Azure Stack | Microsoft Docs
description: Meer informatie over hoe u MySQL-databases kunt implementeren als een service op Azure Stack en de snelle stappen voor het implementeren van de MySQL-Server resource provider-adapter.
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
ms.date: 07/13/2018
ms.author: jeffgilb
ms.reviewer: jeffgo
ms.openlocfilehash: 2f5661ddac16a3024335bd633623f7ada2fc5870
ms.sourcegitcommit: 387d7edd387a478db181ca639db8a8e43d0d75f7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/10/2018
ms.locfileid: "42057193"
---
# <a name="deploy-the-mysql-resource-provider-on-azure-stack"></a>De resourceprovider van MySQL in Azure Stack implementeren

De resourceprovider van MySQL-Server gebruiken om MySQL-databases als een Azure Stack-service zichtbaar te maken. De MySQL-resourceprovider wordt uitgevoerd als een service op een Windows Server 2016 Server Core-machine (VM).

## <a name="prerequisites"></a>Vereisten

Er zijn verschillende vereisten die worden voldaan moet voordat u kunt de Azure Stack-MySQL-resourceprovider implementeren. Om te voldoen aan deze vereisten, de stappen in dit artikel op een computer die toegang de beschermde VM-eindpunt tot.

* Als u dit nog niet hebt gedaan, [registreren Azure Stack](.\azure-stack-registration.md) met Azure, zodat u kunt Azure marketplace-items downloaden.
* U moet de Azure- en PowerShell voor Azure Stack-modules installeren op het systeem waar u deze installatie wordt uitgevoerd. Dat systeem moet een installatiekopie van Windows 10 of Windows Server 2016 met de meest recente versie van de .NET runtime. Zie [PowerShell voor Azure Stack installeren](.\azure-stack-powershell-install.md).
* De vereiste Windows Server-core VM toevoegen aan de Azure Stack marketplace door te downloaden de **Windows Server 2016 Datacenter - Server Core** installatiekopie.

* De MySQL-resourceprovider binaire downloaden en voer vervolgens de zelfstandige extractor om de inhoud uitpakken naar een tijdelijke map.

  >[!NOTE]
  >Voor het implementeren van de MySQL-provider op een computer die geen toegang tot Internet, Kopieer de [mysql-connector-net-6.10.5.msi](https://dev.mysql.com/get/Downloads/Connector-Net/mysql-connector-net-6.10.5.msi) bestand naar een lokaal pad. Geef het pad de naam met behulp de **DependencyFilesLocalPath** parameter.

* De resourceprovider heeft een minimale bijbehorende Azure Stack bouwen. Zorg ervoor dat u het juiste binaire bestand voor de versie van Azure-Stack die u gebruikt downloaden:

    | Azure Stack-versie | MySQL RP-versie|
    | --- | --- |
    | Versie 1804 (1.0.180513.1)|[MySQL RP versie 1.1.24.0](https://aka.ms/azurestackmysqlrp1804) |
    | Versie 1802 (1.0.180302.1) | [MySQL RP versie 1.1.18.0](https://aka.ms/azurestackmysqlrp1802)|
    |     |     |

- Zorg ervoor dat de datacenter-integratie vereisten wordt voldaan:

    |Vereiste|Referentie|
    |-----|-----|
    |Voorwaardelijk doorsturen van DNS is correct ingesteld.|[Datacenter-integratie Azure Stack - DNS](azure-stack-integrate-dns.md)|
    |Poorten voor inkomend verkeer voor resourceproviders zijn geopend.|[Azure Stack-datacenter-integratie - eindpunten publiceren](azure-stack-integrate-endpoints.md#ports-and-protocols-inbound)|
    |PKI-certificaatonderwerp en SAN zijn juist ingesteld.|[Azure Stack verplichte PKI vereisten voor implementatie](azure-stack-pki-certs.md#mandatory-certificates)<br>[De vereisten PaaS-certificaat voor Azure Stack-implementatie](azure-stack-pki-certs.md#optional-paas-certificates)|
    |     |     |

### <a name="certificates"></a>Certificaten

_Geïntegreerde systemen alleen voor installaties_. Moet u de SQL-PaaS-PKI-certificaat dat is beschreven in de sectie met optionele PaaS certificaten van [Azure Stack-implementatievereisten PKI](.\azure-stack-pki-certs.md#optional-paas-certificates). Plaats het pfx-bestand in de locatie die is opgegeven door de **DependencyFilesLocalPath** parameter. Bieden een certificaat voor ASDK systemen.

## <a name="deploy-the-resource-provider"></a>De resourceprovider implementeren

Nadat u alle vereisten die zijn geïnstalleerd hebt, voert u de **DeployMySqlProvider.ps1** script voor het implementeren van de MYSQL-resourceprovider. Het script DeployMySqlProvider.ps1 wordt opgehaald als onderdeel van de MySQL-resource provider binaire bestand dat u hebt gedownload voor uw versie van Azure Stack.

Voor het implementeren van de MySQL-resourceprovider, open een nieuw verhoogde PowerShell-venster (niet PowerShell ISE) en Ga naar de map waar u de binaire bestanden voor MySQL resource provider hebt uitgepakt. We raden u aan met behulp van een nieuwe PowerShell-venster om te voorkomen van potentiële problemen veroorzaakt door een PowerShell-modules die al zijn geladen.

Voer de **DeployMySqlProvider.ps1** script, dat bestaat uit de volgende taken:

* De certificaten en andere artefacten worden geüpload naar een opslagaccount in Azure Stack.
* Publiceert galeriepakketten zodat u MySQL-databases met behulp van de galerie kunt implementeren.
* Hiermee geeft u een galerijpakket voor het implementeren van hostingservers mogelijk.
* Een virtuele machine met behulp van de Windows Server 2016 core u hebt gedownload en vervolgens installeert hij de MySQL-resourceprovider implementeert.
* Hiermee wordt een lokale DNS-record die wordt toegewezen aan uw VM-resourceprovider geregistreerd.
* Hiermee wordt de resourceprovider geregistreerd met de lokale Azure Resource Manager voor de operator-account.

> [!NOTE]
> Wanneer de implementatie van MySQL resource provider wordt gestart, wordt de **system.local.mysqladapter** resourcegroep wordt gemaakt. Het duurt maximaal 75 minuten aan de implementaties die zijn vereist om deze resourcegroep te voltooien.

### <a name="deploymysqlproviderps1-parameters"></a>DeployMySqlProvider.ps1 parameters

U kunt deze parameters vanaf de opdrachtregel opgeven. Als u dit niet doet, of als een parametervalidatie mislukt, wordt u gevraagd de vereiste parameters opgeven.

| Parameternaam | Beschrijving | Opmerking of de standaardwaarde |
| --- | --- | --- |
| **CloudAdminCredential** | De referentie voor de beheerder van de cloud, die nodig zijn voor toegang tot het eindpunt van de bevoegdheden. | _Vereist_ |
| **AzCredential** | De referenties voor de beheerdersaccount van de Azure Stack-service. Gebruik de dezelfde referenties die u gebruikt voor het implementeren van Azure Stack. | _Vereist_ |
| **VMLocalCredential** | De referenties voor het lokale administrator-account van de MySQL-resourceprovider VM. | _Vereist_ |
| **PrivilegedEndpoint** | De IP-adres of de DNS-naam van het eindpunt van de bevoegdheden. |  _Vereist_ |
| **DependencyFilesLocalPath** | Voor alleen geïntegreerde systemen, moet uw certificaat-pfx-bestand in deze map worden geplaatst. Download voor niet-verbonden enviroments [mysql-connector-net-6.10.5.msi](https://dev.mysql.com/get/Downloads/Connector-Net/mysql-connector-net-6.10.5.msi) naar deze map. U kunt eventueel een pakket voor Windows Update MSU hier kopiëren. | _Optionele_ (_verplichte_ voor geïntegreerde systemen of niet-verbonden omgevingen) |
| **DefaultSSLCertificatePassword** | Het wachtwoord voor het pfx-certificaat. | _Vereist_ |
| **MaxRetryCount** | Het aantal keren dat die u wilt dat elke bewerking wordt uitgevoerd als er een fout is.| 2 |
| **RetryDuration** | De time-outinterval tussen nieuwe pogingen in seconden. | 120 |
| **Verwijderen** | Hiermee verwijdert u de resourceprovider en alle bijbehorende resources (Zie de opmerkingen bij de volgende). | Nee |
| **Fouten opsporen-modus** | Hiermee voorkomt u dat bij fout automatisch op te schonen. | Nee |
| **AcceptLicense** | Hiermee slaat u de prompt om de GPL-licentie te accepteren.  <http://www.gnu.org/licenses/old-licenses/gpl-2.0.html> | |

## <a name="deploy-the-mysql-resource-provider-using-a-custom-script"></a>De MySQL-resourceprovider met behulp van een aangepast script implementeren

Om te voorkomen handmatige configuratie bij het implementeren van de resourceprovider, kunt u het volgende script aanpassen. Wijzig de standaard-accountgegevens en -wachtwoorden voor uw Azure Stack-implementatie.

```powershell
# Install the AzureRM.Bootstrapper module, set the profile and install the AzureStack module
Install-Module -Name AzureRm.BootStrapper -Force
Use-AzureRmProfile -Profile 2017-03-09-profile
Install-Module -Name AzureStack -RequiredVersion 1.4.0

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

# Change to the directory folder where you extracted the installation files. Do not provide a certificate on ASDK!
. $tempDir\DeployMySQLProvider.ps1 `
    -AzCredential $AdminCreds `
    -VMLocalCredential $vmLocalAdminCreds `
    -CloudAdminCredential $cloudAdminCreds `
    -PrivilegedEndpoint $privilegedEndpoint `
    -DefaultSSLCertificatePassword $PfxPass `
    -DependencyFilesLocalPath $tempDir\cert `
    -AcceptLicense

```

Wanneer het script voor resource provider-installatie is voltooid, vernieuw uw browser om ervoor te zorgen dat u de meest recente updates kunt zien.

## <a name="verify-the-deployment-by-using-the-azure-stack-portal"></a>Controleer of de implementatie met behulp van de Azure Stack-portal

1. Meld u aan de admin-portal als de servicebeheerder.
2. Selecteer **resourcegroepen**
3. Selecteer de **system.\< locatie\>.mysqladapter** resourcegroep.
4. Op de overzichtspagina voor de resourcegroep overzicht, mogen er geen mislukte implementaties.

## <a name="next-steps"></a>Volgende stappen

[Hosting-servers toevoegen](azure-stack-mysql-resource-provider-hosting-servers.md)
