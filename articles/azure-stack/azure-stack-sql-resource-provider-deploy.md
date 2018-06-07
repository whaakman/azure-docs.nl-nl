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
ms.date: 05/24/2018
ms.author: jeffgilb
ms.reviewer: jeffgo
ms.openlocfilehash: 8643e75a24ff7840b71dfaceae9934cdda566d30
ms.sourcegitcommit: 680964b75f7fff2f0517b7a0d43e01a9ee3da445
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/01/2018
ms.locfileid: "34604417"
---
# <a name="use-sql-databases-on-microsoft-azure-stack"></a>SQL-databases op Microsoft Azure-Stack gebruiken
De resourceprovider Azure Stack SQL Server gebruiken om SQL-databases als een service van Azure-Stack weer te geven. De SQL resource provider-service wordt uitgevoerd op de SQL-resourceprovider virtuele machine een virtuele machine van Windows Server core is.

## <a name="prerequisites"></a>Vereisten
Er zijn verschillende vereisten die worden voldaan moet voordat u de Azure-Stack SQL-resourceprovider kunt implementeren. De volgende stappen uitvoeren op een computer die toegang heeft tot de bevoegde eindpunt VM:

- Als u nog niet hebt gedaan, [registreren Azure Stack](.\azure-stack-registration.md) met Azure zodat u via Azure marketplace-items downloaden kunt.
- De vereiste Windows Server core VM toevoegen aan de Stack van Azure marketplace downloaden de **Windows Server 2016 Server core** installatiekopie. Als u een update installeert moet, kunt u één plaatsen. MSU-pakket in het pad van de lokale afhankelijkheid. Als meer dan één. MSU-bestand wordt gevonden, mislukt de installatie van de SQL resource provider.
- SQL-resourceprovider binaire downloaden en voer vervolgens de zelfstandig uitpakken om de inhoud naar een tijdelijke map te pakken. De resourceprovider heeft een minimale bijbehorende Azure-Stack bouwen. Zorg ervoor dat het juiste binaire bestand voor de versie van Azure-Stack die u gebruikt downloaden:

    |Azure Stack-versie|SQL RP-versie|
    |-----|-----|
    |Versie 1804 (1.0.180513.1)|[SQL RP versie 1.1.24.0](https://aka.ms/azurestacksqlrp1804)
    |Versie 1802 (1.0.180302.1)|[SQL RP versie 1.1.18.0](https://aka.ms/azurestacksqlrp1802)|
    |Versie 1712 (1.0.180102.3, 1.0.180103.2 of 1.0.180106.1 (geïntegreerde systemen))|[SQL RP versie 1.1.14.0](https://aka.ms/azurestacksqlrp1712)|
    |     |     |
- Voor geïntegreerde systemen installaties, moet u het SQL-PaaS-PKI-certificaat opgeven, zoals beschreven in de sectie optionele PaaS certificaten, van [Azure Stack PKI implementatievereisten](.\azure-stack-pki-certs.md#optional-paas-certificates), door het plaatsen van het pfx-bestand op de locatie opgegeven door de **DependencyFilesLocalPath** parameter.

## <a name="deploy-the-sql-resource-provider"></a>De SQL-resourceprovider implementeren
Nadat u hebt voorbereid voor het installeren van de SQL-resourceprovider door voldoen aan alle vereisten, kunt u nu uitvoeren de **DeploySqlProvider.ps1** script voor het implementeren van de SQL-resourceprovider. Het script DeploySqlProvider.ps1 wordt opgehaald als onderdeel van de SQL-resourceprovider binaire dat u hebt gedownload overeenkomt met uw Azure-Stack-versie. 

> [!IMPORTANT]
> Het systeem waarop het script wordt uitgevoerd, moet een Windows 10 of Windows Server 2016-systeem met de meest recente versie van de .NET runtime geïnstalleerd.


Voor het implementeren van de SQL-resourceprovider, open een nieuw verhoogde (beheerdersrechten) PowerShell-console en Ga naar de map waar u de binaire bestanden voor SQL resource provider hebt uitgepakt.

> [!NOTE]
> Een nieuw venster van de PowerShell-console gebruiken om problemen die mogelijk worden veroorzaakt door onjuiste PowerShell-modules die al zijn geladen in het systeem te voorkomen.

Voer het script DeploySqlProvider.ps1, waarmee de volgende stappen uit:
- De certificaten en andere artefacten uploadt naar een opslagaccount op Azure-Stack.
- Publiceert galerie pakketten zodat u de SQL-databases via de galerie kunt implementeren.
- Publiceert een gallery-pakket voor het implementeren van de hosting-servers.
- Een virtuele machine implementeert met behulp van de installatiekopie van het Windows Server 2016 die is gemaakt in stap 1 en vervolgens de resourceprovider wordt geïnstalleerd.
- Registreert een lokale DNS-record dat is toegewezen aan uw VM-resourceprovider.
- Registreert uw resourceprovider met de lokale Azure Resource Manager (gebruiker en beheer).
- Eventueel installeert één Windows update tijdens de RP-installatie.

SQL resource provider-implementatie begint en de resourcegroep system.local.sqladapter maakt. Het kan voltooien van de vier vereiste implementaties aan deze resourcegroep 75 minuten duren.

### <a name="deploysqlproviderps1-parameters"></a>DeploySqlProvider.ps1 parameters
U kunt deze parameters opgeven op de opdrachtregel. Als u dit niet doet, of als er parametervalidatie mislukt, wordt u gevraagd de vereiste parameters bevatten.

| Parameternaam | Beschrijving | Opmerking of de standaardwaarde |
| --- | --- | --- |
| **CloudAdminCredential** | De referentie voor de beheerder van de cloud, nodig voor toegang tot de bevoegde eindpunt. | _Vereist_ |
| **AzCredential** | De referenties voor de Azure-Stack-admin-serviceaccount. Dezelfde referenties gebruiken dat u gebruikt voor het implementeren van Azure-Stack. | _Vereist_ |
| **VMLocalCredential** | De referenties voor het lokale administrator-account van de SQL-resourceprovider VM. | _Vereist_ |
| **PrivilegedEndpoint** | De IP-adres of de DNS-naam van het bevoegde eindpunt. |  _Vereist_ |
| **DependencyFilesLocalPath** | Het PFX-certificaatbestand moet in deze map ook worden geplaatst. | _Optionele_ (_verplichte_ voor geïntegreerde systemen) |
| **DefaultSSLCertificatePassword** | Het wachtwoord voor het pfx-certificaat. | _Vereist_ |
| **MaxRetryCount** | Het aantal keren dat u wilt opnieuw proberen aan elke als er een storing optreedt.| 2 |
| **RetryDuration** | De time-interval tussen nieuwe pogingen, in seconden. | 120 |
| **Verwijderen** | Hiermee verwijdert u de resourceprovider en alle bijbehorende resources (Zie de volgende opmerkingen). | Nee |
| **Fouten opsporen-modus** | Voorkomt dat automatisch opschonen bij fouten. | Nee |

>[!NOTE]
> SKU's kunnen een uur duren zichtbaar in de portal. U kunt een database kan niet maken, tot de SKU is gemaakt.


## <a name="deploy-the-sql-resource-provider-using-a-custom-script"></a>De SQL-resourceprovider met een aangepast script implementeren
Om te voorkomen dat de vereiste informatie handmatig worden ingevoerd wanneer het script DeploySqlProvider.ps1 wordt uitgevoerd, kunt u het volgende scriptvoorbeeld aanpassen door de standaard-accountgegevens en wachtwoorden naar behoefte te wijzigen:

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
$tempDir\DeploySQLProvider.ps1 `
    -AzCredential $AdminCreds `
    -VMLocalCredential $vmLocalAdminCreds `
    -CloudAdminCredential $cloudAdminCreds `
    -PrivilegedEndpoint $privilegedEndpoint `
    -DefaultSSLCertificatePassword $PfxPass `
    -DependencyFilesLocalPath $tempDir\cert
 ```

## <a name="verify-the-deployment-using-the-azure-stack-portal"></a>Controleer of de implementatie met de Stack van Azure-portal
De stappen in deze sectie kunnen worden gebruikt om ervoor te zorgen dat de SQL-resourceprovider is geïmplementeerd.

> [!NOTE]
>  Nadat het script voor installatie is voltooid, moet u de portal om de beheerder blade en galerie-items weer te vernieuwen.

1. Aanmelden bij de beheerportal als de servicebeheerder.

2. Controleer of de implementatie is voltooid. Ga naar **resourcegroepen**. Selecteer vervolgens de **system.\< locatie\>.sqladapter** resourcegroep. Controleer of alle vier implementaties is voltooid.

      ![Controleer of de implementatie van de SQL-resourceprovider](./media/azure-stack-sql-rp-deploy/sqlrp-verify.png)


## <a name="next-steps"></a>Volgende stappen

[Hosting-servers toevoegen](azure-stack-sql-resource-provider-hosting-servers.md)
