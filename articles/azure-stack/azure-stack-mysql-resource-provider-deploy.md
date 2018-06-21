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
ms.date: 06/20/2018
ms.author: jeffgilb
ms.reviewer: jeffgo
ms.openlocfilehash: 061d9431890622dafafa8c180027b8df735c100e
ms.sourcegitcommit: 1438b7549c2d9bc2ace6a0a3e460ad4206bad423
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/20/2018
ms.locfileid: "36295754"
---
# <a name="use-mysql-databases-on-microsoft-azure-stack"></a>MySQL-database gebruiken op Microsoft Azure-Stack
MySQL-Server van Azure-Stack van resourceprovider te MySQL-databases weergeven als een service van Azure-Stack gebruiken. De MySQL resource provider-service wordt uitgevoerd op virtuele machine een Windows Server core-virtuele machine is van de MySQL-resourceprovider.

## <a name="deploy-the-resource-provider"></a>De resourceprovider implementeren 
1. Als u dit nog niet hebt gedaan, registreren van uw development kit en downloaden van de installatiekopie van het Windows Server 2016 Datacenter Core downloadbare via Marketplace-beheer. U kunt een installatiekopie van Windows Server 2016 Core moet gebruiken. U kunt ook een script maken van een [installatiekopie van Windows Server 2016](https://docs.microsoft.com/azure/azure-stack/azure-stack-add-default-image). (Zorg ervoor dat de core-optie te selecteren.) 

2. Meld u bij een host die toegang heeft tot de bevoegde eindpunt VM:
    - Bij installaties van de Azure SDK, moet u zich aanmelden bij de fysieke host.  
    - Op geïntegreerde systemen moet de host een systeem dat toegang heeft tot de bevoegde eindpunt. 
    
    >[!NOTE] 
    > Het systeem waarop het script wordt uitgevoerd *moet* worden van een Windows 10 of Windows Server 2016-systeem met de meest recente versie van de .NET runtime geïnstalleerd. Anders mislukt de installatie. De Azure-Stack ASDK host voldoet aan dit criterium. 
    
3. Download de binaire MySQL resourceprovider. Voer vervolgens de zelfstandig uitpakken om de inhoud naar een tijdelijke map te pakken. 
    >[!NOTE]  
    > De resourceprovider heeft een minimale bijbehorende Azure-Stack bouwen. Zorg ervoor dat het juiste binaire bestand voor de versie van Azure-Stack met downloaden.

    | Azure Stack-versie | MySQL RP-versie| 
    | --- | --- | 
    | Versie 1804 (1.0.180513.1)|[MySQL RP versie 1.1.24.0](https://aka.ms/azurestackmysqlrp1804) | 
    | Versie 1802 (1.0.180302.1) | [MySQL RP versie 1.1.18.0](https://aka.ms/azurestackmysqlrp1802) | 
    | Versie 1712 (1.0.180102.3 of 1.0.180106.1 (geïntegreerde systemen)) | [MySQL RP versie 1.1.14.0](https://aka.ms/azurestackmysqlrp1712) | 

4.  Voor de ASDK een zelfondertekend certificaat gemaakt als onderdeel van dit proces. Voor geïntegreerde systemen, moet u een geschikt certificaat opgeven. Als u moet uw eigen certificaat op te geven, plaatst u een .pfx-bestand in de **DependencyFilesLocalPath** die voldoet aan de volgende criteria: 
    - Een jokertekencertificaat voor \*.dbadapter.\< regio\>.\< externe fqdn\> of een certificaat voor één site met een algemene naam van mysqladapter.dbadapter.\< regio\>.\< externe fqdn\>. 
    - Dit certificaat moet worden vertrouwd. Dat wil zeggen moet de keten van vertrouwensrelatie bestaan zonder tussenliggende certificaten. 
    - Een enkele certificaatbestand bestaat in de DependencyFilesLocalPath. 
    - De bestandsnaam mag geen eventuele speciale tekens of spaties bevatten. 

5. Open een **nieuwe** (beheerdersrechten) PowerShell-console met verhoogde bevoegdheid. Wijzig in de map waar u de bestanden hebt uitgepakt. Een nieuw venster gebruiken om problemen die mogelijk worden veroorzaakt door onjuiste PowerShell-modules die al zijn geladen in het systeem te voorkomen. 

6. Voer de **DeployMySqlProvider.ps1** script. Het script voert de volgende stappen uit: 
    - Downloadt het MySQL connector binaire bestand (dit kan worden opgegeven offline). 
    - De certificaten en andere artefacten uploadt naar een opslagaccount op Azure-Stack. 
    - Publiceert galerie pakketten zodat u de SQL-databases via de galerie kunt implementeren. 
    - Publiceert een gallery-pakket voor het implementeren van de hosting-servers. 
    - Een VM die gebruikmaakt van een Azure-Stack van Windows Server 2016 marketplace-installatiekopie implementeert en de resourceprovider wordt geïnstalleerd. 
    - Registreert een lokale DNS-record dat is toegewezen aan uw VM-resourceprovider. 
    - Registreert uw resourceprovider met de lokale Azure Resource Manager (tenant en beheer). 

    U kunt door de vereiste parameters op de opdrachtregel of Voer het script zonder parameters en voer ze wanneer u wordt gevraagd. 

    Hier volgt een voorbeeld die u vanuit de PowerShell-prompt uitvoeren kunt. Zorg ervoor dat de accountgegevens en wachtwoorden zo nodig wijzigen: 

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
    # Find the ERCS01 IP address first, and make sure the certificate 
    # file is in the specified directory. 
    $tempDir\DeployMySQLProvider.ps1 -AzCredential $AdminCreds ` 
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
| **Fouten opsporen-modus** | Voorkomt dat automatisch opschonen bij fouten. | Nee | 
| **AcceptLicense** | Slaat de prompt voor het accepteren van de licentie GPL.  (http://www.gnu.org/licenses/old-licenses/gpl-2.0.html) | | 

> [!NOTE] 
> SKU's kunnen een uur duren zichtbaar in de portal. U kunt een database kan niet maken, tot de SKU is gemaakt. 

## <a name="verify-the-deployment-by-using-the-azure-stack-portal"></a>Controleer of de implementatie met behulp van de Stack van Azure-portal 
> [!NOTE] 
>  Nadat het script voor installatie is voltooid, moet u de portal om te zien van de blade admin vernieuwen. 

1. Aanmelden bij de beheerportal als de servicebeheerder. 
2. Controleer of de implementatie is voltooid. Ga naar **resourcegroepen**, en selecteer vervolgens de **system.\< locatie\>.mysqladapter** resourcegroep. Controleer of alle vier implementaties is voltooid:

      ![Controleer of de implementatie van de MySQL RP](./media/azure-stack-mysql-rp-deploy/mysqlrp-verify.png) 


## <a name="next-steps"></a>Volgende stappen
[Hosting-servers toevoegen](azure-stack-mysql-resource-provider-hosting-servers.md)
