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
ms.date: 06/25/2018
ms.author: jeffgilb
ms.reviewer: jeffgo
ms.openlocfilehash: e1505761a0bd1ea9dabdd0b2cbab7af902198311
ms.sourcegitcommit: 828d8ef0ec47767d251355c2002ade13d1c162af
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/25/2018
ms.locfileid: "36938329"
---
# <a name="deploy-the-sql-server-resource-provider-on-azure-stack"></a>Implementeert de bronprovider van SQL Server op Azure-Stack

De resourceprovider Azure Stack SQL Server gebruiken om SQL-databases als een Azure-Stack-service weer te geven. De SQL-resourceprovider als een service wordt uitgevoerd op een Windows Server 2016 Server Core-virtuele machine (VM).

## <a name="prerequisites"></a>Vereisten

Er zijn verschillende vereisten die worden voldaan moet voordat u de Azure-Stack SQL-resourceprovider kunt implementeren. Om te voldoen aan deze vereisten, voert u de volgende stappen uit op een computer die toegang heeft tot de bevoegde eindpunt VM:

- Als u dit nog niet hebt gedaan, [registreren Azure Stack](.\azure-stack-registration.md) met Azure zodat u via Azure marketplace-items downloaden kunt.
- De vereiste Windows Server core VM toevoegen aan de Stack van Azure marketplace downloaden de **Windows Server 2016 Datacenter - Server Core** installatiekopie. U kunt ook een script maken van een [installatiekopie van Windows Server 2016](https://docs.microsoft.com/azure/azure-stack/azure-stack-add-default-image). Zorg ervoor dat u de core-optie selecteert wanneer u het script uitvoert.

  >[!NOTE]
  >Als u een update installeert moet, kunt u één MSU-pakket in het pad van de lokale afhankelijkheid plaatsen. Als meer dan één MSU-bestand wordt gevonden, mislukt de installatie van de provider resource SQL.

- De binaire SQL resourceprovider downloaden en voer vervolgens de zelfstandig uitpakken om op te halen van de inhoud naar een tijdelijke map. De resourceprovider heeft een minimale bijbehorende Azure-Stack bouwen. Zorg ervoor dat u het juiste binaire bestand voor de versie van Azure-Stack die u gebruikt downloaden.

    |Azure Stack-versie|SQL RP-versie|
    |-----|-----|
    |Versie 1804 (1.0.180513.1)|[SQL RP versie 1.1.24.0](https://aka.ms/azurestacksqlrp1804)
    |Versie 1802 (1.0.180302.1)|[SQL RP versie 1.1.18.0](https://aka.ms/azurestacksqlrp1802)|
    |Versie 1712 (1.0.180102.3, 1.0.180103.2 of 1.0.180106.1 (geïntegreerde systemen))|[SQL RP versie 1.1.14.0](https://aka.ms/azurestacksqlrp1712)|
    |     |     |

### <a name="certificates"></a>Certificaten

Voor geïntegreerde systemen installaties. U moet de SQL PaaS PKI-certificaat dat wordt beschreven in de sectie optionele PaaS certificaten, van opgeven [Azure Stack PKI implementatievereisten](.\azure-stack-pki-certs.md#optional-paas-certificates). Plaats het .pfx-bestand in de locatie die is opgegeven door de **DependencyFilesLocalPath** parameter.

## <a name="deploy-the-sql-resource-provider"></a>De SQL-resourceprovider implementeren

Nadat u de vereisten die zijn geïnstalleerd hebt, voert u de **DeploySqlProvider.ps1** script voor het implementeren van de SQL-resourceprovider. Het script DeploySqlProvider.ps1 wordt opgehaald als onderdeel van het binaire SQL resource provider die u hebt gedownload voor uw versie van Azure-Stack.

> [!IMPORTANT]
> Het systeem dat u het script uitvoert op moet een Windows 10 of Windows Server 2016-systeem met de meest recente versie van de .NET runtime geïnstalleerd.

Voor het implementeren van de SQL-resourceprovider, opent u een **nieuwe** verhoogde PowerShell consolevenster en Ga naar de map waar u de binaire bestanden voor SQL resource provider hebt uitgepakt. U wordt aangeraden met behulp van een nieuwe PowerShell-venster om te voorkomen dat potentiële problemen veroorzaakt door een PowerShell-modules die al zijn geladen.

Voer het script DeploySqlProvider.ps1, die bestaat uit de volgende taken:

- De certificaten en andere artefacten uploadt naar een opslagaccount op Azure-Stack.
- Publiceert galerie pakketten zodat u de SQL-databases met de galerie kunt implementeren.
- Publiceert een gallery-pakket voor het implementeren van de hosting-servers.
- Een virtuele machine met behulp van de core installatiekopie van het Windows Server 2016 u hebt gedownload en installeert de SQL-resourceprovider implementeert.
- Registreert een lokale DNS-record dat is toegewezen aan uw VM-resourceprovider.
- Met de lokale Azure Resource Manager voor de operator en gebruikersaccounts voor de resourceprovider geregistreerd.
- Desgewenst installeert één Windows Server update tijdens de installatie van de resource provider.

> [!NOTE]
> Wanneer de SQL resource provider-implementatie wordt gestart, wordt de **system.local.sqladapter** resourcegroep wordt gemaakt. Duurt maximaal 75 minuten in beslag vier vereiste implementaties voor deze resourcegroep.

### <a name="deploysqlproviderps1-parameters"></a>DeploySqlProvider.ps1 parameters

U kunt de volgende parameters vanaf de opdrachtregel opgeven. Als u dit niet, of als een parametervalidatie mislukt, wordt u gevraagd de vereiste parameters bevatten.

| Parameternaam | Beschrijving | Opmerking of de standaardwaarde |
| --- | --- | --- |
| **CloudAdminCredential** | De referentie voor de beheerder van de cloud, nodig voor toegang tot de bevoegde eindpunt. | _Vereist_ |
| **AzCredential** | De referenties voor de Azure-Stack-admin-serviceaccount. Gebruik dezelfde referenties die u gebruikt voor de implementatie van Azure-Stack. | _Vereist_ |
| **VMLocalCredential** | De referenties voor het lokale administrator-account van de SQL-resourceprovider VM. | _Vereist_ |
| **PrivilegedEndpoint** | De IP-adres of de DNS-naam van het bevoegde eindpunt. |  _Vereist_ |
| **DependencyFilesLocalPath** | Het PFX-certificaatbestand moet in deze map ook worden geplaatst. | _Optionele_ (_verplichte_ voor geïntegreerde systemen) |
| **DefaultSSLCertificatePassword** | Het wachtwoord voor het pfx-certificaat. | _Vereist_ |
| **MaxRetryCount** | Het aantal keren dat u wilt opnieuw proberen aan elke als er een storing optreedt.| 2 |
| **RetryDuration** | De time-interval tussen nieuwe pogingen, in seconden. | 120 |
| **Verwijderen** | Hiermee verwijdert u de resourceprovider en alle bijbehorende resources (Zie de volgende opmerkingen). | Nee |
| **Fouten opsporen-modus** | Voorkomt dat automatisch opschonen bij fouten. | Nee |

>[!NOTE]
> SKU's kunnen een uur duren zichtbaar in de portal. U kunt een database kan niet maken, totdat de SKU geïmplementeerd en wordt uitgevoerd is.

## <a name="deploy-the-sql-resource-provider-using-a-custom-script"></a>De SQL-resourceprovider met een aangepast script implementeren

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
$tempDir = 'C:\TEMP\SQLRP'

# The service admin account can be Azure Active Directory or Active Directory Federation Services.
$serviceAdmin = "admin@mydomain.onmicrosoft.com"
$AdminPass = ConvertTo-SecureString "P@ssw0rd1" -AsPlainText -Force
$AdminCreds = New-Object System.Management.Automation.PSCredential ($serviceAdmin, $AdminPass)

# Set credentials for the new resource provider VM local administrator account.
$vmLocalAdminPass = ConvertTo-SecureString "P@ssw0rd1" -AsPlainText -Force
$vmLocalAdminCreds = New-Object System.Management.Automation.PSCredential ("sqlrpadmin", $vmLocalAdminPass)

# Add the cloudadmin credential that's required for privileged endpoint access.
$CloudAdminPass = ConvertTo-SecureString "P@ssw0rd1" -AsPlainText -Force
$CloudAdminCreds = New-Object System.Management.Automation.PSCredential ("$domain\cloudadmin", $CloudAdminPass)

# Change the following as appropriate.
$PfxPass = ConvertTo-SecureString "P@ssw0rd1" -AsPlainText -Force

# Change to the directory If folder where you extracted the installation files.
# Then adjust the endpoints.
. $tempDir\DeploySQLProvider.ps1 `
    -AzCredential $AdminCreds `
    -VMLocalCredential $vmLocalAdminCreds `
    -CloudAdminCredential $cloudAdminCreds `
    -PrivilegedEndpoint $privilegedEndpoint `
    -DefaultSSLCertificatePassword $PfxPass `
    -DependencyFilesLocalPath $tempDir\cert

 ```

Wanneer het script resource provider voor installatie is voltooid, vernieuw uw browser om te controleren of dat u de meest recente updates kunt zien.

## <a name="verify-the-deployment-using-the-azure-stack-portal"></a>Controleer of de implementatie met de Stack van Azure-portal

U kunt de volgende stappen controleren of de SQL-resourceprovider is geïmplementeerd.

1. Aanmelden bij de beheerportal als de servicebeheerder.
2. Selecteer **resourcegroepen**.
3. Selecteer de **system.\< locatie\>.sqladapter** resourcegroep.
4. Het bericht onder **implementaties**, in de volgende schermopname wordt weergegeven, moet **4 geslaagd**.

      ![Controleer of de implementatie van de SQL-resourceprovider](./media/azure-stack-sql-rp-deploy/sqlrp-verify.png)

5. Krijgt u meer gedetailleerde informatie over de implementatie van de resource provider onder **instellingen**. Selecteer **implementaties** ophalen van informatie, zoals: STATUS, TIMESTAMP en duur voor elke implementatie.

## <a name="next-steps"></a>Volgende stappen

[Hosting-servers toevoegen](azure-stack-sql-resource-provider-hosting-servers.md)
