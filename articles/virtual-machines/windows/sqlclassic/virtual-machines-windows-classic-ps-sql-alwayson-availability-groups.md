---
title: De AlwaysOn-beschikbaarheidsgroep configureren op een virtuele machine in Azure met behulp van PowerShell | Microsoft Docs
description: Deze zelfstudie maakt gebruik van bronnen die zijn gemaakt met het klassieke implementatiemodel. PowerShell kunt u een AlwaysOn-beschikbaarheidsgroep maken in Azure.
services: virtual-machines-windows
documentationcenter: na
author: MikeRayMSFT
manager: jhubbard
editor: 
tags: azure-service-management
ms.assetid: a4e2f175-fe56-4218-86c7-a43fb916cc64
ms.service: virtual-machines-sql
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 03/17/2017
ms.author: mikeray
ms.openlocfilehash: b99cf767fb931d3f7fe14fcbe7990126244613ed
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2017
---
# <a name="configure-the-always-on-availability-group-on-an-azure-vm-with-powershell"></a>De AlwaysOn-beschikbaarheidsgroep configureren op een virtuele machine in Azure met PowerShell
> [!div class="op_single_selector"]
> * [Klassieke: UI](../classic/portal-sql-alwayson-availability-groups.md)
> * [Klassieke: PowerShell](../classic/ps-sql-alwayson-availability-groups.md)
<br/>

Voordat u begint, kunt u overwegen of u kunt deze taak in Azure resource manager-model voltooien. U wordt aangeraden Azure resource manager-model voor nieuwe implementaties. Zie [op virtuele machines in Azure SQL Server altijd op beschikbaarheidsgroepen](../sql/virtual-machines-windows-portal-sql-availability-group-overview.md).

> [!IMPORTANT]
> U wordt aangeraden de meeste nieuwe implementaties het Resource Manager-model gebruiken. Azure heeft twee verschillende implementatiemodellen voor het maken en werken met resources: [Resource Manager en classic](../../../azure-resource-manager/resource-manager-deployment-model.md). Dit artikel gaat over het gebruik van het klassieke implementatiemodel.

Azure virtuele machines (VM's) kunt databasebeheerders verlagen de kosten van een SQL Server-systeem voor hoge beschikbaarheid. Deze zelfstudie laat zien hoe u een beschikbaarheidsgroep implementeert met behulp van SQL Server Always On end-to-end binnen een Azure-omgeving. Uw oplossing SQL Server Always On in Azure wordt aan het einde van de zelfstudie bestaat uit de volgende elementen:

* Een virtueel netwerk met meerdere subnetten, met inbegrip van een front-end- en een back-end-subnet.
* Een domeincontroller met een Active Directory-domein.
* Twee SQL Server-VM's die zijn geïmplementeerd met het subnet voor back-end en toegevoegd aan het Active Directory-domein.
* Een drie-knooppunt Windows failover-cluster met Knooppuntmeerderheid quorummodel.
* Een beschikbaarheidsgroep met twee replica's voor synchrone doorvoer van een beschikbaarheidsdatabase.

Dit scenario is een goede keuze voor de eenvoud op Azure, niet voor de kosteneffectiviteit of andere factoren. U kunt bijvoorbeeld het aantal virtuele machines voor een twee-replica van een beschikbaarheidsgroep op te slaan op rekenuren in Azure met behulp van de domeincontroller als de bestandssharewitness in quorum in een failovercluster met twee knooppunten minimaliseren. Deze methode wordt het aantal VM's met één van de bovenstaande configuratie verlaagd.

Deze zelfstudie is bedoeld om u de stappen die nodig zijn voor het instellen van de beschreven oplossing hierboven, zonder de details van elke stap bespreekt weer te geven. Daarom in plaats van het bieden van de GUI-configuratiestappen, gebruikt het PowerShell-scripts waarmee u snel via elke stap. Deze zelfstudie wordt ervan uitgegaan:

* U hebt al een Azure-account aan het abonnement van de virtuele machine.
* U hebt geïnstalleerd het [Azure PowerShell-cmdlets](/powershell/azure/overview).
* U hebt al een goed begrip van AlwaysOn-beschikbaarheidsgroepen voor oplossingen on-premises. Zie voor meer informatie [altijd op beschikbaarheidsgroepen (SQL Server)](https://msdn.microsoft.com/library/hh510230.aspx).

## <a name="connect-to-your-azure-subscription-and-create-the-virtual-network"></a>Verbinding maken met uw Azure-abonnement en het virtuele netwerk maken
1. De Azure-module importeren in een PowerShell-venster op de lokale computer, downloaden van het bestand publicatie-instellingen naar uw computer en uw PowerShell-sessie verbinding met uw Azure-abonnement met het importeren van de gedownloade publicatie-instellingen.

        Import-Module "C:\Program Files (x86)\Microsoft SDKs\Azure\PowerShell\Azure\Azure.psd1"
        Get-AzurePublishSettingsFile
        Import-AzurePublishSettingsFile <publishsettingsfilepath>

    De **Get-AzurePublishSettingsFile** opdracht genereert u een beheercertificaat met Azure automatisch en downloadt u deze naar uw computer. Een browser wordt automatisch geopend en u wordt gevraagd de referenties van het Microsoft-account invoeren voor uw Azure-abonnement. De gedownloade **.publishsettings** bestand bevat alle de informatie die u nodig voor het beheren van uw Azure-abonnement. Na het opslaan van dit bestand naar een lokale map importeren met behulp van de **importeren AzurePublishSettingsFile** opdracht.

   > [!NOTE]
   > Het .publishsettings-bestand bevat uw referenties (ongecodeerde) die worden gebruikt voor het beheren van uw Azure-abonnementen en services. De aanbevolen beveiligingsprocedures voor dit bestand is tijdelijk buiten uw adreslijsten bron (bijvoorbeeld in de map Libraries\Documents) opslaan en vervolgens te verwijderen nadat het importeren is voltooid. Een kwaadwillende gebruiker die toegang tot de .publishsettings-bestand krijgt kunt bewerken, maken en verwijderen van uw Azure-services.

2. Definieer een reeks variabelen die u gebruiken gaat voor het maken van uw cloud IT-infrastructuur.

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

    Let op het volgende om ervoor te zorgen dat uw opdrachten later slaagt:

   * Variabelen **$storageAccountName** en **$dcServiceName** moeten uniek zijn omdat deze worden gebruikt voor het identificeren van uw cloud-account en cloud opslagserver, respectievelijk op het Internet.
   * De namen die u voor variabelen opgeeft **$affinityGroupName** en **$virtualNetworkName** zijn geconfigureerd in het configuratiebestand voor virtueel netwerk die u later gaat gebruiken.
   * **$sqlImageName** geeft de bijgewerkte naam van de VM-installatiekopie waarin SQL Server 2012 Service Pack 1 Enterprise Edition.
   * Voor het gemak, **Contoso! 000** is het wachtwoord dat wordt gebruikt in de hele zelfstudie.

3. Maak een affiniteitsgroep.

        New-AzureAffinityGroup `
            -Name $affinityGroupName `
            -Location $location `
            -Description $affinityGroupDescription `
            -Label $affinityGroupLabel

4. Een virtueel netwerk maken door een configuratiebestand te importeren.

        Set-AzureVNetConfig `
            -ConfigurationPath $networkConfigPath

    Het configuratiebestand bevat de volgende XML-document. Kort samengevat: Hiermee wordt een virtueel netwerk met de naam **ContosoNET** in de affiniteitsgroep aangeroepen **ContosoAG**. Dit is de adresruimte **10.10.0.0/16** en heeft twee subnetten **10.10.1.0/24** en **10.10.2.0/24**, die de front-subnet en back-subnet, respectievelijk zijn. De front-subnet is waarin u de clienttoepassingen, zoals Microsoft SharePoint kunt plaatsen. Het back-subnet is plaats voegt u de VM's van SQL Server. Als u wijzigt de **$affinityGroupName** en **$virtualNetworkName** variabelen eerder, moet u ook de desbetreffende namen hieronder wijzigen.

        <NetworkConfiguration xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns="http://schemas.microsoft.com/ServiceHosting/2011/07/NetworkConfiguration">
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

5. Maak een opslagaccount die is gekoppeld aan de affiniteitsgroep gemaakt en stel dit in als het huidige opslagaccount in uw abonnement.

        New-AzureStorageAccount `
            -StorageAccountName $storageAccountName `
            -Label $storageAccountLabel `
            -AffinityGroup $affinityGroupName
        Set-AzureSubscription `
            -SubscriptionName (Get-AzureSubscription).SubscriptionName `
            -CurrentStorageAccount $storageAccountName

6. Maak de domain controller-server in de nieuwe cloud-service en beschikbaarheid.

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

    Deze piped opdrachten doen het volgende:

   * **Nieuwe AzureVMConfig** maakt u een VM-configuratie.
   * **Voeg AzureProvisioningConfig** geeft de configuratieparameters van een zelfstandige Windows-server.
   * **Voeg AzureDataDisk** voegt de gegevensschijf die u gebruiken gaat voor het opslaan van Active Directory-gegevens met de cache in optie is ingesteld op None.
   * **Nieuwe AzureVM** maakt een nieuwe cloudservice en maakt u de nieuwe Azure-virtuele machine in de nieuwe cloudservice.

7. Wacht tot de nieuwe virtuele machine volledig is ingericht en download het externe bureaublad naar uw werkmap. Omdat de nieuwe virtuele machine in Azure een lange tijd om in te richten duren, de `while` lus blijft voor het pollen van de nieuwe virtuele machine totdat deze klaar voor gebruik.

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

De domain controller-server is nu is ingericht. Vervolgens configureert u het Active Directory-domein op deze domeincontrollerserver. Laat het PowerShell-venster geopend op uw lokale computer. U gaat deze later opnieuw gebruiken voor het maken van de twee SQL Server-VM's.

## <a name="configure-the-domain-controller"></a>De domeincontroller configureren
1. Verbinding maken met de domeincontrollerserver beschadigt door de extern bureaublad-bestand. Gebruik de gebruikersnaam AzureAdmin en het wachtwoord van de beheerder van de machine **Contoso! 000**, die u hebt opgegeven toen u de nieuwe virtuele machine hebt gemaakt.
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

    Nadat de opdracht is voltooid, wordt de virtuele machine automatisch opnieuw gestart.

4. Maakt verbinding met de domain controller-server opnieuw starten van de extern bureaublad-bestand. Deze tijd, meld u aan als **CORP\Administrator**.
5. Open een PowerShell-venster in de beheerdersmodus en de Active Directory PowerShell-module importeren met behulp van de volgende opdracht:

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

    **CORP\Install** wordt gebruikt voor het configureren van alles met betrekking tot de service-exemplaren van SQL Server, het failover-cluster en de beschikbaarheidsgroep. **CORP\SQLSvc1** en **CORP\SQLSvc2** worden gebruikt als de SQL Server-serviceaccounts voor de twee SQL Server-VM's.
7. Voer vervolgens de volgende opdrachten om te geven **CORP\Install** de machtigingen voor het maken van computerobjecten in het domein.

        Cd ad:
        $sid = new-object System.Security.Principal.SecurityIdentifier (Get-ADUser "Install").SID
        $guid = new-object Guid bf967a86-0de6-11d0-a285-00aa003049e2
        $ace1 = new-object System.DirectoryServices.ActiveDirectoryAccessRule $sid,"CreateChild","Allow",$guid,"All"
        $corp = Get-ADObject -Identity "DC=corp,DC=contoso,DC=com"
        $acl = Get-Acl $corp
        $acl.AddAccessRule($ace1)
        Set-Acl -Path "DC=corp,DC=contoso,DC=com" -AclObject $acl

    De GUID die hierboven is opgegeven, is de GUID voor het objecttype van de computer. De **CORP\Install** account moet de **alle eigenschappen lezen** en **Computerobjecten maken** machtiging voor het maken van de actieve Direct objecten voor de failover-cluster. De **alle eigenschappen lezen** machtiging is al toegewezen aan CORP\Install standaard, dus u hoeft niet expliciet verlenen. Zie voor meer informatie over de machtigingen die nodig zijn voor het failovercluster maakt, [stapsgewijze handleiding voor Failover-Cluster: Accounts configureren in Active Directory](https://technet.microsoft.com/library/cc731002%28v=WS.10%29.aspx).

    Nu dat u klaar bent met het configureren van Active Directory en de gebruikersobjecten, u maakt twee SQL Server-VM's en voeg ze toe aan dit domein.

## <a name="create-the-sql-server-vms"></a>De VM's van SQL Server maken
1. Blijven gebruiken van het PowerShell-venster is geopend op uw lokale computer. De volgende aanvullende variabelen definiëren:

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

    Het IP-adres **10.10.0.4** is doorgaans toegewezen aan de eerste virtuele machine die u maakt in de **10.10.0.0/16** subnet van uw virtuele Azure-netwerk. U moet bevestigen dat dit het adres van uw domeincontrollerserver door te voeren **IPCONFIG**.
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

    Houd rekening met het volgende met betrekking tot de bovenstaande opdracht:

   * **Nieuwe AzureVMConfig** maakt u een VM-configuratie met de naam van de gewenste beschikbaarheidsset. De volgende virtuele machines wordt gemaakt met dezelfde naam van de beschikbaarheid zodat ze zijn gekoppeld aan dezelfde beschikbaarheidsset.
   * **Voeg AzureProvisioningConfig** lid wordt van de virtuele machine aan Active Directory-domein dat u hebt gemaakt.
   * **Set-AzureSubnet** plaatst u de virtuele machine in het back-subnet.
   * **Nieuwe AzureVM** maakt een nieuwe cloudservice en maakt u de nieuwe Azure-virtuele machine in de nieuwe cloudservice. De **DnsSettings** parameter geeft u op dat de DNS-server voor de servers in de nieuwe cloudservice voor het IP-adres heeft **10.10.0.4**. Dit is het IP-adres van de domain controller-server. Deze parameter is vereist voor de nieuwe virtuele machines in de cloudservice om toe te voegen aan het Active Directory-domein is. Deze parameter niet opgeeft, moet u handmatig de IPv4-instellingen instellen in uw VM voor gebruik van de domain controller-server als de primaire DNS-server nadat de virtuele machine is ingericht en vervolgens de virtuele machine toevoegen aan het Active Directory-domein.
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

    Houd rekening met het volgende met betrekking tot de bovenstaande opdrachten:

   * **Nieuwe AzureVMConfig** als de domain controller-server dezelfde naam van de beschikbaarheid, en de installatiekopie van het SQL Server 2012 Service Pack 1 Enterprise Edition gebruikt in de galerie met virtuele machine. Ook wordt de besturingssysteemschijf ingesteld op lees-caching alleen (geen schrijfcache). U wordt aangeraden de databasebestanden migreren naar een afzonderlijke schijf die u aan de virtuele machine koppelen en worden geconfigureerd met geen lees- of schrijfcache. Het volgende beste is echter verwijderen schrijfcache op de besturingssysteemschijf omdat de lees-caching op de schijf van het besturingssysteem kan niet worden verwijderd.
   * **Voeg AzureProvisioningConfig** lid wordt van de virtuele machine aan Active Directory-domein dat u hebt gemaakt.
   * **Set-AzureSubnet** plaatst u de virtuele machine in het back-subnet.
   * **Voeg AzureEndpoint** toegang eindpunten toegevoegd zodat clienttoepassingen toegang deze services-exemplaren van SQL Server op het Internet tot. Andere poorten worden besteed aan ContosoSQL1 en ContosoSQL2.
   * **Nieuwe AzureVM** maakt de nieuwe virtuele machine een SQL-Server in dezelfde cloudservice als ContosoQuorum. Als u wilt dat ze zich in dezelfde beschikbaarheidsset, moet u de virtuele machines in dezelfde cloudservice plaatsen.
4. Wacht voor elke virtuele machine volledig is ingericht en voor elke virtuele machine de extern bureaublad-bestand te downloaden naar uw werkmap. De `for` lus doorlopen van de drie nieuwe virtuele machines en voert u de opdrachten binnen de site op het hoogste accolades voor elk van deze.

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

    De SQL Server-VM's nu zijn ingericht en wordt uitgevoerd, maar ze zijn geïnstalleerd met SQL Server met de standaardopties.

## <a name="initialize-the-failover-cluster-vms"></a>Initialiseren van het failover-cluster virtuele machines
In deze sectie moet u de drie servers die u in het failovercluster en de installatie van SQL Server gebruiken gaat wijzigen. Specifiek:

* Alle servers: U moet installeren de **Failoverclustering** functie.
* Alle servers: U moet toevoegen **CORP\Install** als de machine **beheerder**.
* ContosoSQL1 en alleen ContosoSQL2: U moet toevoegen **CORP\Install** als een **sysadmin** rol in de standaarddatabase.
* ContosoSQL1 en alleen ContosoSQL2: U moet toevoegen **NT AUTHORITY\System** als iemand zich aanmeldt met de volgende machtigingen:

  * Wijzigen van een beschikbaarheidsgroep
  * Verbinding maken met SQL
  * Status van de server weergeven
* ContosoSQL1 en alleen ContosoSQL2: de **TCP** protocol is al ingeschakeld op de virtuele machine van SQL Server. U moet nog steeds de firewall voor externe toegang van SQL Server te openen.

U bent nu klaar om te beginnen. Beginnen met **ContosoQuorum**, voer de volgende stappen uit:

1. Verbinding maken met **ContosoQuorum** door het starten van de extern bureaublad-bestanden. Gebruik de gebruikersnaam van de beheerder van de machine **AzureAdmin** en het wachtwoord **Contoso! 000**, die u hebt opgegeven tijdens het maken van de virtuele machines.
2. Controleer of dat de computers met succes hebt toegevoegd aan **corp.contoso.com**.
3. Wacht tot de SQL Server-installatie te voltooien uitgevoerd van de van de geautomatiseerde initialisatietaken voordat u doorgaat.
4. Open een PowerShell-venster in de beheerdersmodus.
5. Het onderdeel Windows Failover Clustering installeren.

        Import-Module ServerManager
        Add-WindowsFeature Failover-Clustering
6. Voeg **CORP\Install** als een lokale beheerder.

        net localgroup administrators "CORP\Install" /Add
7. Afmelden bij ContosoQuorum. U bent nu klaar met deze server.

        logoff.exe

Vervolgens initialiseren **ContosoSQL1** en **ContosoSQL2**. Volg de stappen hieronder, identiek voor beide VM's van SQL Server zijn.

1. Verbinding maken met de twee SQL Server-VM's met het starten van de extern bureaublad-bestanden. Gebruik de gebruikersnaam van de beheerder van de machine **AzureAdmin** en het wachtwoord **Contoso! 000**, die u hebt opgegeven tijdens het maken van de virtuele machines.
2. Controleer of dat de computers met succes hebt toegevoegd aan **corp.contoso.com**.
3. Wacht tot de SQL Server-installatie te voltooien uitgevoerd van de van de geautomatiseerde initialisatietaken voordat u doorgaat.
4. Open een PowerShell-venster in de beheerdersmodus.
5. Het onderdeel Windows Failover Clustering installeren.

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
9. Voeg **NT AUTHORITY\System** als iemand zich aanmeldt met de drie machtigingen die hierboven worden beschreven.

        Invoke-SqlCmd -Query "CREATE LOGIN [NT AUTHORITY\SYSTEM] FROM WINDOWS" -ServerInstance "."
        Invoke-SqlCmd -Query "GRANT ALTER ANY AVAILABILITY GROUP TO [NT AUTHORITY\SYSTEM] AS SA" -ServerInstance "."
        Invoke-SqlCmd -Query "GRANT CONNECT SQL TO [NT AUTHORITY\SYSTEM] AS SA" -ServerInstance "."
        Invoke-SqlCmd -Query "GRANT VIEW SERVER STATE TO [NT AUTHORITY\SYSTEM] AS SA" -ServerInstance "."
10. Open de firewall voor externe toegang van SQL Server.

         netsh advfirewall firewall add rule name='SQL Server (TCP-In)' program='C:\Program Files\Microsoft SQL Server\MSSQL11.MSSQLSERVER\MSSQL\Binn\sqlservr.exe' dir=in action=allow protocol=TCP
11. Afmelden bij beide virtuele machines.

         logoff.exe

U kunt ten slotte de beschikbaarheidsgroep configureren. U de SQL Server PowerShell-Provider voor het uitvoeren van al het werk op **ContosoSQL1**.

## <a name="configure-the-availability-group"></a>De beschikbaarheidsgroep configureren
1. Verbinding maken met **ContosoSQL1** opnieuw door het starten van de extern bureaublad-bestanden. In plaats van met de machineaccount aanmeldt, moet u zich aanmelden via **CORP\Install**.
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
5. De SQL Server-serviceaccount voor ContosoSQL1 om CORP\SQLSvc1 te wijzigen.

        $wmi1 = new-object ("Microsoft.SqlServer.Management.Smo.Wmi.ManagedComputer") $server1
        $wmi1.services | where {$_.Type -eq 'SqlServer'} | foreach{$_.SetServiceAccount($acct1,$password)}
        $svc1 = Get-Service -ComputerName $server1 -Name 'MSSQLSERVER'
        $svc1.Stop()
        $svc1.WaitForStatus([System.ServiceProcess.ServiceControllerStatus]::Stopped,$timeout)
        $svc1.Start();
        $svc1.WaitForStatus([System.ServiceProcess.ServiceControllerStatus]::Running,$timeout)
6. De SQL Server-serviceaccount voor ContosoSQL2 om CORP\SQLSvc2 te wijzigen.

        $wmi2 = new-object ("Microsoft.SqlServer.Management.Smo.Wmi.ManagedComputer") $server2
        $wmi2.services | where {$_.Type -eq 'SqlServer'} | foreach{$_.SetServiceAccount($acct2,$password)}
        $svc2 = Get-Service -ComputerName $server2 -Name 'MSSQLSERVER'
        $svc2.Stop()
        $svc2.WaitForStatus([System.ServiceProcess.ServiceControllerStatus]::Stopped,$timeout)
        $svc2.Start();
        $svc2.WaitForStatus([System.ServiceProcess.ServiceControllerStatus]::Running,$timeout)
7. Download **CreateAzureFailoverCluster.ps1** van [failovercluster maken voor AlwaysOn-beschikbaarheidsgroepen in Azure VM](http://gallery.technet.microsoft.com/scriptcenter/Create-WSFC-Cluster-for-7c207d3a) naar de lokale werkmap. U hebt dit script gebruiken voor het maken van een functionele failover-cluster. Zie voor meer informatie over hoe Windows Failover Clustering met de Azure-netwerk samenwerkt [hoge beschikbaarheid en herstel na noodgevallen voor SQL Server in Azure Virtual Machines](../sql/virtual-machines-windows-sql-high-availability-dr.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fsqlclassic%2ftoc.json).
8. Ga naar uw werkmap en het failovercluster maakt met het gedownloade script.

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
10. Maak een back-map en machtigingen voor de SQL Server-serviceaccounts. U gebruikt deze map voor het voorbereiden van de beschikbaarheidsdatabase op de secundaire replica.

         $backup = "C:\backup"
         New-Item $backup -ItemType directory
         net share backup=$backup "/grant:$acct1,FULL" "/grant:$acct2,FULL"
         icacls.exe "$backup" /grant:r ("$acct1" + ":(OI)(CI)F") ("$acct2" + ":(OI)(CI)F")
11. Een database maakt op **ContosoSQL1** aangeroepen **MyDB1**, Neem zowel een volledige back-up als een logboekback-up en herstel deze op **ContosoSQL2** met de **WITH NORECOVERY**  optie.

         Invoke-SqlCmd -Query "CREATE database $db"
         Backup-SqlDatabase -Database $db -BackupFile "$backupShare\db.bak" -ServerInstance $server1
         Backup-SqlDatabase -Database $db -BackupFile "$backupShare\db.log" -ServerInstance $server1 -BackupAction Log
         Restore-SqlDatabase -Database $db -BackupFile "$backupShare\db.bak" -ServerInstance $server2 -NoRecovery
         Restore-SqlDatabase -Database $db -BackupFile "$backupShare\db.log" -ServerInstance $server2 -RestoreAction Log -NoRecovery
12. De beschikbaarheid van de groep eindpunten maken op de SQL Server-VM's en stel de juiste machtigingen op de eindpunten.

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
U hebt nu geïmplementeerd SQL Server Always On door het maken van een beschikbaarheidsgroep in Azure. Zie voor het configureren van een listener voor deze beschikbaarheidsgroep [een ILB-listener voor AlwaysOn-beschikbaarheidsgroepen configureren in Azure](../classic/ps-sql-int-listener.md).

Zie voor meer informatie over het gebruik van SQL Server in Azure, [SQL Server op virtuele machines in Azure](../sql/virtual-machines-windows-sql-server-iaas-overview.md).
