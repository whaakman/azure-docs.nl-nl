---
title: De AlwaysOn-beschikbaarheidsgroep configureren op een Azure-VM met behulp van PowerShell | Microsoft Docs
description: In deze zelfstudie maakt gebruik van resources die zijn gemaakt met het klassieke implementatiemodel. U PowerShell gebruiken voor het maken van een AlwaysOn-beschikbaarheidsgroep in Azure.
services: virtual-machines-windows
documentationcenter: na
author: MikeRayMSFT
manager: craigg
editor: ''
tags: azure-service-management
ms.assetid: a4e2f175-fe56-4218-86c7-a43fb916cc64
ms.service: virtual-machines-sql
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 03/17/2017
ms.author: mikeray
ms.openlocfilehash: a6d8326afa3bcf13234ab072a2cd2909a864738b
ms.sourcegitcommit: 2d0fb4f3fc8086d61e2d8e506d5c2b930ba525a7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/18/2019
ms.locfileid: "58002847"
---
# <a name="configure-the-always-on-availability-group-on-an-azure-vm-with-powershell"></a>De AlwaysOn-beschikbaarheidsgroep configureren op een Azure-VM met PowerShell
> [!div class="op_single_selector"]
> * [Klassiek: GEBRUIKERSINTERFACE](../classic/portal-sql-alwayson-availability-groups.md)
> * [Klassiek: PowerShell](../classic/ps-sql-alwayson-availability-groups.md)
<br/>

Voordat u begint, kunt u dat u deze taak in Azure resource manager-model kunt voltooien. Het wordt aangeraden de Azure resource manager-model voor nieuwe implementaties. Zie [SQL Server Always On-beschikbaarheidsgroepen op Azure virtual machines](../sql/virtual-machines-windows-portal-sql-availability-group-overview.md).

> [!IMPORTANT]
> We raden aan dat de meeste nieuwe implementaties het Resource Manager-model gebruiken. Azure heeft twee verschillende implementatiemodellen voor het maken van en werken met resources: [Resource Manager en het klassieke model](../../../azure-resource-manager/resource-manager-deployment-model.md). Dit artikel gaat over het gebruik van het klassieke implementatiemodel.

Virtuele Azure-machines (VM's) kunt databasebeheerders worden de kosten van een maximaal beschikbare SQL Server-systeem. Deze zelfstudie leert u hoe u een beschikbaarheidsgroep toevoegen met behulp van SQL Server Always On end-to-end binnen een Azure-omgeving implementeert. Uw SQL Server Always On-oplossing in Azure wordt aan het einde van de zelfstudie bestaan uit de volgende elementen:

* Een virtueel netwerk met meerdere subnetten, met inbegrip van een front-end en een back-end-subnet.
* Een domeincontroller met een Active Directory-domein.
* Twee SQL Server-VM's die zijn geïmplementeerd op de back-end-subnet en gekoppeld aan het Active Directory-domein.
* Een Windows failovercluster met drie knooppunten met het model van de Quorumconfiguratie Knooppuntmeerderheid.
* Een beschikbaarheidsgroep toevoegen met twee replica's met synchrone doorvoer van een beschikbaarheidsdatabase.

Dit scenario is een goede keuze voor de eenvoud van Azure, niet voor de kosteneffectiviteit of andere factoren. U kunt bijvoorbeeld het aantal virtuele machines voor een twee-replica van een beschikbaarheidsgroep op te slaan op rekenuren in Azure met behulp van de domeincontroller als de quorumconfiguratie bestandsshare-witness in een failovercluster met twee knooppunten minimaliseren. Deze methode vermindert het aantal VM's door een van de bovenstaande configuratie.

Deze zelfstudie is bedoeld om u de stappen die nodig zijn voor het instellen van de oplossing wordt beschreven, zonder bespreekt de details van elke stap weer te geven. Dus in plaats van de GUI-configuratiestappen geven, maakt gebruik van PowerShell-scripts voor het snel begeleid door elke stap. In deze zelfstudie wordt ervan uitgegaan:

* U hebt al een Azure-account aan het abonnement van de virtuele machine.
* U hebt geïnstalleerd het [Azure PowerShell-cmdlets](/powershell/azure/overview).
* U hebt al een goed begrip van AlwaysOn availability groups voor on-premises oplossingen. Zie voor meer informatie, [AlwaysOn-beschikbaarheidsgroepen (SQL Server)](https://msdn.microsoft.com/library/hh510230.aspx).

## <a name="connect-to-your-azure-subscription-and-create-the-virtual-network"></a>Verbinding maken met uw Azure-abonnement en het virtuele netwerk maken
1. In een PowerShell-venster op uw lokale computer, de Azure-module importeren en downloaden van het bestand publicatie-instellingen op de computer verbinding maken met de PowerShell-sessie met uw Azure-abonnement door het importeren van de gedownloade publicatie-instellingen.

        Import-Module "C:\Program Files (x86)\Microsoft SDKs\Azure\PowerShell\Azure\Azure.psd1"
        Get-AzurePublishSettingsFile
        Import-AzurePublishSettingsFile <publishsettingsfilepath>

    De **Get-AzurePublishSettingsFile** opdracht automatisch genereert een certificaat voor beheer met Azure en downloadt deze naar uw computer. Een browser wordt automatisch geopend en u wordt gevraagd de referenties van het Microsoft-account invoeren voor uw Azure-abonnement. De gedownloade **.publishsettings** bestand bevat alle informatie die u nodig hebt voor het beheren van uw Azure-abonnement. Na het opslaan van dit bestand naar een lokale map, importeren met behulp van de **importeren AzurePublishSettingsFile** opdracht.

   > [!NOTE]
   > Het .publishsettings-bestand bevat uw referenties (ongecodeerd) die worden gebruikt voor het beheren van uw Azure-abonnementen en services. De aanbevolen beveiligingsprocedures voor dit bestand is tijdelijk opgeslagen buiten uw bron-mappen (bijvoorbeeld in de map Libraries\Documents) en deze te verwijderen nadat het importeren is voltooid. Een kwaadwillende gebruiker die toegang tot de .publishsettings-bestand krijgt kunt bewerken, maken en verwijderen van uw Azure-services.

2. Definieer een reeks variabelen die u gebruiken gaat om uw cloud-IT-infrastructuur te maken.

        $location = "West US"
        $affinityGroupName = "ContosoAG"
        $affinityGroupDescription = "Contoso SQL HADR Affinity Group"
        $affinityGroupLabel = "IaaS BI Affinity Group"
        $networkConfigPath = "C:\scripts\Network.netcfg"
        $virtualNetworkName = "ContosoNET"
        $storageAccountName = "<uniquestorageaccountname>"
        $storageAccountLabel = "Contoso SQL HADR Storage Account"
        $storageAccountContainer = "https://" + $storageAccountName + ".blob.core.windows.net/vhds/"
        $winImageName = (Get-AzureVMImage | where {$_.Label -like "Windows Server 2008 R2 SP1*"} | sort PublishedDate -Descending)[0].ImageName
        $sqlImageName = (Get-AzureVMImage | where {$_.Label -like "SQL Server 2012 SP1 Enterprise*"} | sort PublishedDate -Descending)[0].ImageName
        $dcServerName = "ContosoDC"
        $dcServiceName = "<uniqueservicename>"
        $availabilitySetName = "SQLHADR"
        $vmAdminUser = "AzureAdmin"
        $vmAdminPassword = "Contoso!000"
        $workingDir = "c:\scripts\"

    Let op het volgende om ervoor te zorgen dat uw opdrachten later worden uitgevoerd:

   * Variabelen **$storageAccountName** en **$dcServiceName** moet uniek zijn omdat ze worden gebruikt voor het identificeren van uw cloud storage-account en cloud-server, respectievelijk op het Internet.
   * De namen die u voor variabelen opgeeft **$affinityGroupName** en **$virtualNetworkName** zijn geconfigureerd in het configuratiebestand voor virtueel netwerk die u later gaat gebruiken.
   * **$sqlImageName** Hiermee geeft u de bijgewerkte naam van de VM-installatiekopie met SQL Server 2012 Service Pack 1 Enterprise Edition.
   * Voor het gemak **Contoso! 000** is het wachtwoord dat wordt gebruikt in de volledige zelfstudie.

3. Maak een affiniteitsgroep.

        New-AzureAffinityGroup `
            -Name $affinityGroupName `
            -Location $location `
            -Description $affinityGroupDescription `
            -Label $affinityGroupLabel

4. Een virtueel netwerk maken door het importeren van een configuratiebestand.

        Set-AzureVNetConfig `
            -ConfigurationPath $networkConfigPath

    Het configuratiebestand bevat de volgende XML-document. In het kort het Hiermee geeft u een virtueel netwerk met de naam **ContosoNET** in de affiniteitsgroep met de naam **ContosoAG**. Dit is de adresruimte **10.10.0.0/16** en twee subnetten gemaakt, heeft **10.10.1.0/24** en **10.10.2.0/24**, die respectievelijk de front-subnet en de back-subnet, zijn. De front-subnet is waar u clienttoepassingen, zoals Microsoft SharePoint kunt plaatsen. De back-subnet is plaatst u de SQL Server-VM's. Als u wijzigt de **$affinityGroupName** en **$virtualNetworkName** variabelen eerder gebruikt, moet u ook de bijbehorende namen hieronder wijzigen.

        <NetworkConfiguration xmlns:xsi="https://www.w3.org/2001/XMLSchema-instance" xmlns:xsd="https://www.w3.org/2001/XMLSchema" xmlns="http://schemas.microsoft.com/ServiceHosting/2011/07/NetworkConfiguration">
          <VirtualNetworkConfiguration>
            <Dns />
            <VirtualNetworkSites>
              <VirtualNetworkSite name="ContosoNET" AffinityGroup="ContosoAG">
                <AddressSpace>
                  <AddressPrefix>10.10.0.0/16</AddressPrefix>
                </AddressSpace>
                <Subnets>
                  <Subnet name="Front">
                    <AddressPrefix>10.10.1.0/24</AddressPrefix>
                  </Subnet>
                  <Subnet name="Back">
                    <AddressPrefix>10.10.2.0/24</AddressPrefix>
                  </Subnet>
                </Subnets>
              </VirtualNetworkSite>
            </VirtualNetworkSites>
          </VirtualNetworkConfiguration>
        </NetworkConfiguration>

5. Maak een opslagaccount dat is gekoppeld aan de affiniteitsgroep dat u hebt gemaakt en deze als de huidige storage-account in uw abonnement instellen.

        New-AzureStorageAccount `
            -StorageAccountName $storageAccountName `
            -Label $storageAccountLabel `
            -AffinityGroup $affinityGroupName
        Set-AzureSubscription `
            -SubscriptionName (Get-AzureSubscription).SubscriptionName `
            -CurrentStorageAccount $storageAccountName

6. Maak de domain controller-server in de nieuwe cloud service en beschikbaarheid.

        New-AzureVMConfig `
            -Name $dcServerName `
            -InstanceSize Medium `
            -ImageName $winImageName `
            -MediaLocation "$storageAccountContainer$dcServerName.vhd" `
            -DiskLabel "OS" |
            Add-AzureProvisioningConfig `
                -Windows `
                -DisableAutomaticUpdates `
                -AdminUserName $vmAdminUser `
                -Password $vmAdminPassword |
                New-AzureVM `
                    -ServiceName $dcServiceName `
                    –AffinityGroup $affinityGroupName `
                    -VNetName $virtualNetworkName

    Deze piped opdrachten doen de volgende dingen:

   * **Nieuwe AzureVMConfig** maakt u een VM-configuratie.
   * **Toevoegen-AzureProvisioningConfig** geeft de parameters voor de configuratie van een zelfstandige Windows-server.
   * **Voeg AzureDataDisk** voegt de gegevensschijf die u gebruiken gaat voor het opslaan van Active Directory-gegevens, met de cache optie is ingesteld op None.
   * **New-AzureVM** maakt een nieuwe cloudservice en maakt u de nieuwe Azure-VM in de nieuwe cloudservice.

7. Wacht tot de nieuwe virtuele machine volledig is ingericht en een bestand met de extern bureaublad downloaden naar uw werkmap. Omdat de nieuwe Azure-VM kan lang duren om in te richten, de `while` lus blijft op te vragen voor de nieuwe virtuele machine totdat deze klaar voor gebruik is.

        $VMStatus = Get-AzureVM -ServiceName $dcServiceName -Name $dcServerName

        While ($VMStatus.InstanceStatus -ne "ReadyRole")
        {
            write-host "Waiting for " $VMStatus.Name "... Current Status = " $VMStatus.InstanceStatus
            Start-Sleep -Seconds 15
            $VMStatus = Get-AzureVM -ServiceName $dcServiceName -Name $dcServerName
        }

        Get-AzureRemoteDesktopFile `
            -ServiceName $dcServiceName `
            -Name $dcServerName `
            -LocalPath "$workingDir$dcServerName.rdp"

De domain controller-server is nu ingericht. Vervolgens configureert u het Active Directory-domein op deze domain controller-server. Laat het PowerShell-venster geopend op uw lokale computer. U gebruikt deze later opnieuw te maken van de twee SQL Server-VM's.

## <a name="configure-the-domain-controller"></a>De domeincontroller configureren
1. Verbinding maken met de domain controller-server met het starten van een bestand met de extern bureaublad. Gebruik de gebruikersnaam AzureAdmin en het wachtwoord van de beheerder van de machine **Contoso! 000**, dat u hebt opgegeven tijdens het maken van de nieuwe virtuele machine.
2. Open een PowerShell-venster in de beheerdersmodus.
3. Voer de volgende **DCPROMO. EXE** opdracht voor het instellen van de **corp.contoso.com** domein met de gegevensmappen op station M.

        dcpromo.exe `
            /unattend `
            /ReplicaOrNewDomain:Domain `
            /NewDomain:Forest `
            /NewDomainDNSName:corp.contoso.com `
            /ForestLevel:4 `
            /DomainNetbiosName:CORP `
            /DomainLevel:4 `
            /InstallDNS:Yes `
            /ConfirmGc:Yes `
            /CreateDNSDelegation:No `
            /DatabasePath:"C:\Windows\NTDS" `
            /LogPath:"C:\Windows\NTDS" `
            /SYSVOLPath:"C:\Windows\SYSVOL" `
            /SafeModeAdminPassword:"Contoso!000"

    Nadat de opdracht is voltooid, wordt de virtuele machine automatisch opnieuw opgestart.

4. Maakt verbinding met de domain controller-server opnieuw starten van een bestand met de extern bureaublad. Deze keer aanmelden als **CORP\Administrator**.
5. Open een PowerShell-venster in de beheerdersmodus en de Active Directory PowerShell-module importeren met behulp van de volgende opdracht uit:

        Import-Module ActiveDirectory

6. Voer de volgende opdrachten drie gebruikers toevoegen aan het domein.

        $pwd = ConvertTo-SecureString "Contoso!000" -AsPlainText -Force
        New-ADUser `
            -Name 'Install' `
            -AccountPassword  $pwd `
            -PasswordNeverExpires $true `
            -ChangePasswordAtLogon $false `
            -Enabled $true
        New-ADUser `
            -Name 'SQLSvc1' `
            -AccountPassword  $pwd `
            -PasswordNeverExpires $true `
            -ChangePasswordAtLogon $false `
            -Enabled $true
        New-ADUser `
            -Name 'SQLSvc2' `
            -AccountPassword  $pwd `
            -PasswordNeverExpires $true `
            -ChangePasswordAtLogon $false `
            -Enabled $true

    **CORP\Install** wordt gebruikt voor het configureren van alles met betrekking tot de SQL Server-service-exemplaren, het failover-cluster en de beschikbaarheidsgroep. **CORP\SQLSvc1** en **CORP\SQLSvc2** worden gebruikt als de SQL Server-serviceaccounts voor de twee SQL Server-VM's.
7. Voer vervolgens de volgende opdrachten geven **CORP\Install** de machtigingen voor het maken van computerobjecten in het domein.

        Cd ad:
        $sid = new-object System.Security.Principal.SecurityIdentifier (Get-ADUser "Install").SID
        $guid = new-object Guid bf967a86-0de6-11d0-a285-00aa003049e2
        $ace1 = new-object System.DirectoryServices.ActiveDirectoryAccessRule $sid,"CreateChild","Allow",$guid,"All"
        $corp = Get-ADObject -Identity "DC=corp,DC=contoso,DC=com"
        $acl = Get-Acl $corp
        $acl.AddAccessRule($ace1)
        Set-Acl -Path "DC=corp,DC=contoso,DC=com" -AclObject $acl

    De GUID die hierboven is opgegeven, is de GUID voor het objecttype van de computer. De **CORP\Install** account moet de **alle eigenschappen lezen** en **Computerobjecten maken** machtiging voor het maken van de actieve directe objecten voor de failover-cluster. De **alle eigenschappen lezen** machtiging is al toegewezen aan CORP\Install standaard, dus u hoeft het expliciet te geven. Zie voor meer informatie over de machtigingen die nodig zijn om de failovercluster te maken, [stapsgewijze handleiding voor Failover-Cluster: Configureren van Accounts in Active Directory](https://technet.microsoft.com/library/cc731002%28v=WS.10%29.aspx).

    Nu dat u klaar bent met het configureren van Active Directory en de gebruikersobjecten, u twee SQL Server-VM's maken en voeg ze toe aan dit domein.

## <a name="create-the-sql-server-vms"></a>De SQL Server-VM's maken
1. Echter ook doorgaan met het PowerShell-venster is geopend op uw lokale computer. De volgende aanvullende variabelen definiëren:

        $domainName= "corp"
        $FQDN = "corp.contoso.com"
        $subnetName = "Back"
        $sqlServiceName = "<uniqueservicename>"
        $quorumServerName = "ContosoQuorum"
        $sql1ServerName = "ContosoSQL1"
        $sql2ServerName = "ContosoSQL2"
        $availabilitySetName = "SQLHADR"
        $dataDiskSize = 100
        $dnsSettings = New-AzureDns -Name "ContosoBackDNS" -IPAddress "10.10.0.4"

    Het IP-adres **10.10.0.4** is doorgaans toegewezen aan de eerste virtuele machine die u maakt in de **10.10.0.0/16** subnet van uw Azure-netwerk. U moet bevestigen dat dit het adres van de domain controller-server door te voeren **IPCONFIG**.
2. Voer de volgende opdrachten voor het maken van de eerste virtuele machine in het failovercluster, met de naam doorgesluisd **ContosoQuorum**:

        New-AzureVMConfig `
            -Name $quorumServerName `
            -InstanceSize Medium `
            -ImageName $winImageName `
            -MediaLocation "$storageAccountContainer$quorumServerName.vhd" `
            -AvailabilitySetName $availabilitySetName `
            -DiskLabel "OS" |
            Add-AzureProvisioningConfig `
                -WindowsDomain `
                -AdminUserName $vmAdminUser `
                -Password $vmAdminPassword `
                -DisableAutomaticUpdates `
                -Domain $domainName `
                -JoinDomain $FQDN `
                -DomainUserName $vmAdminUser `
                -DomainPassword $vmAdminPassword |
                Set-AzureSubnet `
                    -SubnetNames $subnetName |
                    New-AzureVM `
                        -ServiceName $sqlServiceName `
                        –AffinityGroup $affinityGroupName `
                        -VNetName $virtualNetworkName `
                        -DnsSettings $dnsSettings

    Let op het volgende met betrekking tot de bovenstaande opdracht:

   * **Nieuwe AzureVMConfig** maakt u een VM-configuratie met de naam van de gewenste beschikbaarheidsset. De volgende virtuele machines wordt gemaakt met de naam van de dezelfde beschikbaarheidsset zodat ze gekoppeld aan dezelfde beschikbaarheidsset.
   * **Toevoegen-AzureProvisioningConfig** lid wordt van de virtuele machine aan het Active Directory-domein dat u hebt gemaakt.
   * **Set-AzureSubnet** plaatst u de virtuele machine in het back-subnet.
   * **New-AzureVM** maakt een nieuwe cloudservice en maakt u de nieuwe Azure-VM in de nieuwe cloudservice. De **DnsSettings** parameter geeft u op dat de DNS-server voor de servers in de nieuwe cloudservice het IP-adres heeft **10.10.0.4**. Dit is het IP-adres van de domain controller-server. Deze parameter is vereist voor de nieuwe virtuele machines in de cloudservice om toe te voegen aan het Active Directory-domein is. Deze parameter niet opgeeft, moet u handmatig de IPv4-instellingen instellen in uw virtuele machine de domain controller-server gebruiken als de primaire DNS-server nadat de virtuele machine is ingericht en vervolgens de virtuele machine toevoegen aan het Active Directory-domein.
3. Voer de volgende opdrachten voor het maken van de SQL Server-VM's, met de naam doorgesluisd **ContosoSQL1** en **ContosoSQL2**.

        # Create ContosoSQL1...
        New-AzureVMConfig `
            -Name $sql1ServerName `
            -InstanceSize Large `
            -ImageName $sqlImageName `
            -MediaLocation "$storageAccountContainer$sql1ServerName.vhd" `
            -AvailabilitySetName $availabilitySetName `
            -HostCaching "ReadOnly" `
            -DiskLabel "OS" |
            Add-AzureProvisioningConfig `
                -WindowsDomain `
                -AdminUserName $vmAdminUser `
                -Password $vmAdminPassword `
                -DisableAutomaticUpdates `
                -Domain $domainName `
                -JoinDomain $FQDN `
                -DomainUserName $vmAdminUser `
                -DomainPassword $vmAdminPassword |
                Set-AzureSubnet `
                    -SubnetNames $subnetName |
                    Add-AzureEndpoint `
                        -Name "SQL" `
                        -Protocol "tcp" `
                        -PublicPort 1 `
                        -LocalPort 1433 |
                        New-AzureVM `
                            -ServiceName $sqlServiceName

        # Create ContosoSQL2...
        New-AzureVMConfig `
            -Name $sql2ServerName `
            -InstanceSize Large `
            -ImageName $sqlImageName `
            -MediaLocation "$storageAccountContainer$sql2ServerName.vhd" `
            -AvailabilitySetName $availabilitySetName `
            -HostCaching "ReadOnly" `
            -DiskLabel "OS" |
            Add-AzureProvisioningConfig `
                -WindowsDomain `
                -AdminUserName $vmAdminUser `
                -Password $vmAdminPassword `
                -DisableAutomaticUpdates `
                -Domain $domainName `
                -JoinDomain $FQDN `
                -DomainUserName $vmAdminUser `
                -DomainPassword $vmAdminPassword |
                Set-AzureSubnet `
                    -SubnetNames $subnetName |
                    Add-AzureEndpoint `
                        -Name "SQL" `
                        -Protocol "tcp" `
                        -PublicPort 2 `
                        -LocalPort 1433 |
                        New-AzureVM `
                            -ServiceName $sqlServiceName

    Let op het volgende met betrekking tot de bovenstaande opdrachten:

   * **Nieuwe AzureVMConfig** maakt gebruik van de dezelfde naam van de beschikbaarheidsset als de domain controller-server en maakt gebruik van de installatiekopie van het SQL Server 2012 Service Pack 1 Enterprise Edition in de galerie met virtuele machines. Ook wordt de besturingssysteemschijf voor lees-caching alleen (geen schrijfbewerkingen voor de cache). U wordt aangeraden dat u de databasebestanden migreren naar een afzonderlijke schijf die u aan de virtuele machine koppelen en deze met geen lees- of schrijfbewerkingen voor de cache configureren. Het bijna net zo goed is echter verwijderen van schrijfbewerkingen voor de cache op de besturingssysteemschijf omdat Lees-caching op de besturingssysteemschijf kan niet worden verwijderd.
   * **Toevoegen-AzureProvisioningConfig** lid wordt van de virtuele machine aan het Active Directory-domein dat u hebt gemaakt.
   * **Set-AzureSubnet** plaatst u de virtuele machine in het back-subnet.
   * **Toevoegen-AzureEndpoint** toegangseindpunten toegevoegd zodat clienttoepassingen toegang hebben tot deze services-exemplaren van SQL Server op het Internet. Andere poorten worden besteed aan ContosoSQL1 en ContosoSQL2.
   * **New-AzureVM** wordt de nieuwe SQL Server-VM gemaakt in dezelfde cloudservice als ContosoQuorum. Als u wilt dat ze zich in dezelfde beschikbaarheidsset bevinden, moet u de virtuele machines in dezelfde cloudservice plaatsen.
4. Wacht voor elke virtuele machine volledig is ingericht en voor elke virtuele machine om een bestand met de extern bureaublad downloaden naar uw werkmap. De `for` lus bladeren door de drie nieuwe virtuele machines en voert u de opdrachten binnen de accolades voor op het hoogste niveau voor elk van deze.

        Foreach ($VM in $VMs = Get-AzureVM -ServiceName $sqlServiceName)
        {
            write-host "Waiting for " $VM.Name "..."

            # Loop until the VM status is "ReadyRole"
            While ($VM.InstanceStatus -ne "ReadyRole")
            {
                write-host "  Current Status = " $VM.InstanceStatus
                Start-Sleep -Seconds 15
                $VM = Get-AzureVM -ServiceName $VM.ServiceName -Name $VM.InstanceName
            }

            write-host "  Current Status = " $VM.InstanceStatus

            # Download remote desktop file
            Get-AzureRemoteDesktopFile -ServiceName $VM.ServiceName -Name $VM.InstanceName -LocalPath "$workingDir$($VM.InstanceName).rdp"
        }

    De SQL Server-VM's worden nu ingericht en die wordt uitgevoerd, maar ze zijn geïnstalleerd met SQL Server met de standaardopties.

## <a name="initialize-the-failover-cluster-vms"></a>Initialiseren van de failovercluster-VM 's
In deze sectie die u wilt wijzigen van de drie servers die u in het failover-cluster en de installatie van SQL Server gebruikt. Specifiek:

* Alle servers: U moet installeren de **Failover Clustering** functie.
* Alle servers: U moet toevoegen **CORP\Install** als de machine **beheerder**.
* ContosoSQL1 en alleen ContosoSQL2: U moet toevoegen **CORP\Install** als een **sysadmin** rol in de standaarddatabase.
* ContosoSQL1 en alleen ContosoSQL2: U moet toevoegen **NT AUTHORITY\System** als een aanmelding met de volgende machtigingen:

  * Wijzigen van een beschikbaarheidsgroep
  * Verbinding maken met SQL
  * Status van de server weergeven
* ContosoSQL1 en alleen ContosoSQL2: De **TCP** protocol is al ingeschakeld op de SQL Server-VM. U moet wel openen van de firewall voor externe toegang van SQL Server.

U bent nu klaar om te beginnen. Beginnen met **ContosoQuorum**, volg de onderstaande stappen:

1. Verbinding maken met **ContosoQuorum** door het starten van de remote desktop-bestanden. De gebruikersnaam van de beheerder van de machine **AzureAdmin** en het wachtwoord **Contoso! 000**, dat u hebt opgegeven tijdens het maken van de virtuele machines.
2. Controleer of dat de computers met succes zijn gekoppeld aan **corp.contoso.com**.
3. Wacht tot de SQL Server-installatie te voltooien voor het uitvoeren van de van geautomatiseerde initialisatietaken voordat u doorgaat.
4. Open een PowerShell-venster in de beheerdersmodus.
5. De functie Windows Failover Clustering installeren.

        Import-Module ServerManager
        Add-WindowsFeature Failover-Clustering
6. Voeg **CORP\Install** als een lokale beheerder.

        net localgroup administrators "CORP\Install" /Add
7. Meld u af ContosoQuorum bij. U bent nu klaar met deze server.

        logoff.exe

Vervolgens initialiseren **ContosoSQL1** en **ContosoSQL2**. Volg de onderstaande stappen, die zijn identiek voor zowel SQL Server-VM's.

1. Verbinding maken met de twee SQL Server-VM's met het starten van de remote desktop-bestanden. De gebruikersnaam van de beheerder van de machine **AzureAdmin** en het wachtwoord **Contoso! 000**, dat u hebt opgegeven tijdens het maken van de virtuele machines.
2. Controleer of dat de computers met succes zijn gekoppeld aan **corp.contoso.com**.
3. Wacht tot de SQL Server-installatie te voltooien voor het uitvoeren van de van geautomatiseerde initialisatietaken voordat u doorgaat.
4. Open een PowerShell-venster in de beheerdersmodus.
5. De functie Windows Failover Clustering installeren.

        Import-Module ServerManager
        Add-WindowsFeature Failover-Clustering
6. Voeg **CORP\Install** als een lokale beheerder.

        net localgroup administrators "CORP\Install" /Add
7. Importeer de PowerShell-Provider van de SQL-Server.

        Set-ExecutionPolicy -Execution RemoteSigned -Force
        Import-Module -Name "sqlps" -DisableNameChecking
8. Voeg **CORP\Install** als de rol sysadmin voor het standaardexemplaar van SQL Server.

        net localgroup administrators "CORP\Install" /Add
        Invoke-SqlCmd -Query "EXEC sp_addsrvrolemember 'CORP\Install', 'sysadmin'" -ServerInstance "."
9. Voeg **NT AUTHORITY\System** als een aanmelding met de drie machtigingen die hierboven worden beschreven.

        Invoke-SqlCmd -Query "CREATE LOGIN [NT AUTHORITY\SYSTEM] FROM WINDOWS" -ServerInstance "."
        Invoke-SqlCmd -Query "GRANT ALTER ANY AVAILABILITY GROUP TO [NT AUTHORITY\SYSTEM] AS SA" -ServerInstance "."
        Invoke-SqlCmd -Query "GRANT CONNECT SQL TO [NT AUTHORITY\SYSTEM] AS SA" -ServerInstance "."
        Invoke-SqlCmd -Query "GRANT VIEW SERVER STATE TO [NT AUTHORITY\SYSTEM] AS SA" -ServerInstance "."
10. Open de firewall voor externe toegang van SQL Server.

         netsh advfirewall firewall add rule name='SQL Server (TCP-In)' program='C:\Program Files\Microsoft SQL Server\MSSQL11.MSSQLSERVER\MSSQL\Binn\sqlservr.exe' dir=in action=allow protocol=TCP
11. Afmelden bij beide VM's.

         logoff.exe

Ten slotte u kunt de beschikbaarheidsgroep configureren. U gebruikt de SQL Server PowerShell-Provider om uit te voeren al het werk op **ContosoSQL1**.

## <a name="configure-the-availability-group"></a>De beschikbaarheidsgroep configureren
1. Verbinding maken met **ContosoSQL1** opnieuw door het starten van de remote desktop-bestanden. In plaats van aanmelden met behulp van de machineaccount, moet u zich aanmelden met behulp van **CORP\Install**.
2. Open een PowerShell-venster in de beheerdersmodus.
3. Definieer de volgende variabelen:

        $server1 = "ContosoSQL1"
        $server2 = "ContosoSQL2"
        $serverQuorum = "ContosoQuorum"
        $acct1 = "CORP\SQLSvc1"
        $acct2 = "CORP\SQLSvc2"
        $password = "Contoso!000"
        $clusterName = "Cluster1"
        $timeout = New-Object System.TimeSpan -ArgumentList 0, 0, 30
        $db = "MyDB1"
        $backupShare = "\\$server1\backup"
        $quorumShare = "\\$server1\quorum"
        $ag = "AG1"
4. Importeer de PowerShell-Provider van de SQL-Server.

        Set-ExecutionPolicy RemoteSigned -Force
        Import-Module "sqlps" -DisableNameChecking
5. Wijzig de SQL Server-serviceaccount voor ContosoSQL1 in CORP\SQLSvc1.

        $wmi1 = new-object ("Microsoft.SqlServer.Management.Smo.Wmi.ManagedComputer") $server1
        $wmi1.services | where {$_.Type -eq 'SqlServer'} | foreach{$_.SetServiceAccount($acct1,$password)}
        $svc1 = Get-Service -ComputerName $server1 -Name 'MSSQLSERVER'
        $svc1.Stop()
        $svc1.WaitForStatus([System.ServiceProcess.ServiceControllerStatus]::Stopped,$timeout)
        $svc1.Start();
        $svc1.WaitForStatus([System.ServiceProcess.ServiceControllerStatus]::Running,$timeout)
6. Wijzig de SQL Server-serviceaccount voor ContosoSQL2 in CORP\SQLSvc2.

        $wmi2 = new-object ("Microsoft.SqlServer.Management.Smo.Wmi.ManagedComputer") $server2
        $wmi2.services | where {$_.Type -eq 'SqlServer'} | foreach{$_.SetServiceAccount($acct2,$password)}
        $svc2 = Get-Service -ComputerName $server2 -Name 'MSSQLSERVER'
        $svc2.Stop()
        $svc2.WaitForStatus([System.ServiceProcess.ServiceControllerStatus]::Stopped,$timeout)
        $svc2.Start();
        $svc2.WaitForStatus([System.ServiceProcess.ServiceControllerStatus]::Running,$timeout)
7. Download **CreateAzureFailoverCluster.ps1** van [Failover-Cluster voor Always On Availability Groups in virtuele Azure-machine maken](https://gallery.technet.microsoft.com/scriptcenter/Create-WSFC-Cluster-for-7c207d3a) naar de lokale werkmap. U gebruikt dit script voor het maken van een functionele failover-cluster. Zie voor meer informatie over hoe Windows Failover Clustering met het Azure-netwerk communiceert [hoge beschikbaarheid en herstel na noodgevallen voor SQL Server in Azure Virtual Machines](../sql/virtual-machines-windows-sql-high-availability-dr.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fsqlclassic%2ftoc.json).
8. Ga naar uw werkmap en het failover-cluster maken met het gedownloade script.

        Set-ExecutionPolicy Unrestricted -Force
        .\CreateAzureFailoverCluster.ps1 -ClusterName "$clusterName" -ClusterNode "$server1","$server2","$serverQuorum"
9. Schakel AlwaysOn-beschikbaarheidsgroepen voor de standaard SQL Server-exemplaren op **ContosoSQL1** en **ContosoSQL2**.

        Enable-SqlAlwaysOn `
            -Path SQLSERVER:\SQL\$server1\Default `
            -Force
        Enable-SqlAlwaysOn `
            -Path SQLSERVER:\SQL\$server2\Default `
            -NoServiceRestart
        $svc2.Stop()
        $svc2.WaitForStatus([System.ServiceProcess.ServiceControllerStatus]::Stopped,$timeout)
        $svc2.Start();
        $svc2.WaitForStatus([System.ServiceProcess.ServiceControllerStatus]::Running,$timeout)
10. Maak een back-upmap en machtigingen verlenen voor de SQL Server-service-accounts. U zult deze directory gebruiken voor het voorbereiden van de beschikbaarheidsdatabase op de secundaire replica.

         $backup = "C:\backup"
         New-Item $backup -ItemType directory
         net share backup=$backup "/grant:$acct1,FULL" "/grant:$acct2,FULL"
         icacls.exe "$backup" /grant:r ("$acct1" + ":(OI)(CI)F") ("$acct2" + ":(OI)(CI)F")
11. Maak een database in **ContosoSQL1** met de naam **MyDB1**, een volledige back-up-en een logboekback-up en herstellen op **ContosoSQL2** met de **WITH NORECOVERY**  optie.

         Invoke-SqlCmd -Query "CREATE database $db"
         Backup-SqlDatabase -Database $db -BackupFile "$backupShare\db.bak" -ServerInstance $server1
         Backup-SqlDatabase -Database $db -BackupFile "$backupShare\db.log" -ServerInstance $server1 -BackupAction Log
         Restore-SqlDatabase -Database $db -BackupFile "$backupShare\db.bak" -ServerInstance $server2 -NoRecovery
         Restore-SqlDatabase -Database $db -BackupFile "$backupShare\db.log" -ServerInstance $server2 -RestoreAction Log -NoRecovery
12. De beschikbaarheid van groep eindpunten op de SQL Server-VM's maken en de juiste machtigingen instellen voor de eindpunten.

         $endpoint =
             New-SqlHadrEndpoint MyMirroringEndpoint `
             -Port 5022 `
             -Path "SQLSERVER:\SQL\$server1\Default"
         Set-SqlHadrEndpoint `
             -InputObject $endpoint `
             -State "Started"
         $endpoint =
             New-SqlHadrEndpoint MyMirroringEndpoint `
             -Port 5022 `
             -Path "SQLSERVER:\SQL\$server2\Default"
         Set-SqlHadrEndpoint `
             -InputObject $endpoint `
             -State "Started"

         Invoke-SqlCmd -Query "CREATE LOGIN [$acct2] FROM WINDOWS" -ServerInstance $server1
         Invoke-SqlCmd -Query "GRANT CONNECT ON ENDPOINT::[MyMirroringEndpoint] TO [$acct2]" -ServerInstance $server1
         Invoke-SqlCmd -Query "CREATE LOGIN [$acct1] FROM WINDOWS" -ServerInstance $server2
         Invoke-SqlCmd -Query "GRANT CONNECT ON ENDPOINT::[MyMirroringEndpoint] TO [$acct1]" -ServerInstance $server2
13. De beschikbaarheid van replica's maken.

         $primaryReplica =
             New-SqlAvailabilityReplica `
             -Name $server1 `
             -EndpointURL "TCP://$server1.corp.contoso.com:5022" `
             -AvailabilityMode "SynchronousCommit" `
             -FailoverMode "Automatic" `
             -Version 11 `
             -AsTemplate
         $secondaryReplica =
             New-SqlAvailabilityReplica `
             -Name $server2 `
             -EndpointURL "TCP://$server2.corp.contoso.com:5022" `
             -AvailabilityMode "SynchronousCommit" `
             -FailoverMode "Automatic" `
             -Version 11 `
             -AsTemplate
14. Ten slotte de beschikbaarheidsgroep maken en de secundaire replica toevoegen aan de beschikbaarheidsgroep.

         New-SqlAvailabilityGroup `
             -Name $ag `
             -Path "SQLSERVER:\SQL\$server1\Default" `
             -AvailabilityReplica @($primaryReplica,$secondaryReplica) `
             -Database $db
         Join-SqlAvailabilityGroup `
             -Path "SQLSERVER:\SQL\$server2\Default" `
             -Name $ag
         Add-SqlAvailabilityDatabase `
             -Path "SQLSERVER:\SQL\$server2\Default\AvailabilityGroups\$ag" `
             -Database $db

## <a name="next-steps"></a>Volgende stappen
U hebt nu is geïmplementeerd SQL Server Always On met het maken van een beschikbaarheidsgroep in Azure. Zie configureren van een listener voor deze beschikbaarheidsgroep [een ILB-listener voor AlwaysOn-beschikbaarheidsgroepen configureren in Azure](../classic/ps-sql-int-listener.md).

Zie voor meer informatie over het gebruik van SQL Server in Azure, [SQL Server op Azure virtual machines](../sql/virtual-machines-windows-sql-server-iaas-overview.md).
