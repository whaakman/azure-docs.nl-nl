---
title: Instellen van herstel na noodgevallen van Hyper-V-machines in VMM-clouds naar een secundaire site met Azure Site Recovery en PowerShell | Microsoft Docs
description: Beschrijft hoe u het instellen van herstel na noodgevallen van Hyper-V-machines in VMM-clouds naar een secundaire VMM-site met behulp van Azure Site Recovery en PowerShell.
services: site-recovery
author: sujayt
manager: rochakm
ms.service: site-recovery
ms.topic: article
ms.date: 07/06/2018
ms.author: sutalasi
ms.openlocfilehash: 8e1494594546c432123b8b1b98d646e8637eea99
ms.sourcegitcommit: 1f9e1c563245f2a6dcc40ff398d20510dd88fd92
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/14/2018
ms.locfileid: "51622836"
---
# <a name="set-up-disaster-recovery-of-hyper-v-vms-to-a-secondary-site-by-using-powershell-resource-manager"></a>Instellen van herstel na noodgevallen van Hyper-V-machines naar een secundaire site met behulp van PowerShell (Resource Manager)

Dit artikel wordt beschreven hoe u de stappen te automatiseren voor replicatie van Hyper-V virtuele machines in System Center Virtual Machine Manager-clouds naar een cloud Virtual Machine Manager in een secundaire on-premises site met behulp van [Azure Site Recovery](site-recovery-overview.md).

## <a name="prerequisites"></a>Vereisten

- De [architectuur en onderdelen voor dit scenario](hyper-v-vmm-architecture.md) doornemen.
- Raadpleeg de [ondersteuningsvereisten](site-recovery-support-matrix-to-sec-site.md) voor alle onderdelen.
- Zorg ervoor dat Virtual Machine Manager-servers en Hyper-V-hosts voldoen [ondersteuningsvereisten](site-recovery-support-matrix-to-sec-site.md).
- Controleer of de virtuele machines die u wilt repliceren voldoen aan [gerepliceerde machine ondersteuning](site-recovery-support-matrix-to-sec-site.md).


## <a name="prepare-for-network-mapping"></a>Voorbereiden op netwerktoewijzing

[Netwerktoewijzing](hyper-v-vmm-network-mapping.md) toewijzingen tussen on-premises Virtual Machine Manager-VM-netwerken in de bron- en clouds. Bij toewijzing gebeurt het volgende:

- Virtuele machines worden verbonden met de juiste VM-doelnetwerken na een failover. 
- Gerepliceerde VM's worden optimaal op Hyper-V-doelhostservers geplaatst. 
- Als u geen netwerktoewijzing configureert, replica-VM's niet verbonden met een VM-netwerk na een failover.

Virtual Machine Manager als volgt voorbereiden:

* Zorg ervoor dat u hebt [Virtual Machine Manager logische netwerken](https://docs.microsoft.com/system-center/vmm/network-logical) op de bron- en Virtual Machine Manager-servers:

    - Het logische netwerk op de bronserver moet worden gekoppeld aan de broncloud waarin de Hyper-V-hosts zich bevinden.
    - Het logische netwerk op de doelserver moet worden gekoppeld aan de doelcloud.
* Zorg ervoor dat u hebt [VM-netwerken](https://docs.microsoft.com/system-center/vmm/network-virtual) op de bron- en Virtual Machine Manager-servers. VM-netwerken moeten zijn gekoppeld aan het logische netwerk op elke locatie.
* Verbind VM's op de Hyper-V-bronhosts met het VM-bronnetwerk. 

## <a name="prepare-for-powershell"></a>Voorbereiden voor PowerShell

Zorg ervoor dat u Azure PowerShell kunt aan de slag:

- Als u PowerShell, voer een upgrade naar versie 0.8.10 of hoger gebruiken. [Meer informatie](/powershell/azureps-cmdlets-docs) over het instellen van PowerShell.
- Nadat u instellen en configureren van PowerShell, bekijkt u de [cmdlets-service](/powershell/azure/overview).
- Lees voor meer informatie over het gebruik van parameterwaarden, invoer en uitvoer in PowerShell, de [aan de slag](/powershell/azure/get-started-azureps) handleiding.

## <a name="set-up-a-subscription"></a>Instellen van een abonnement
1. Vanuit PowerShell, moet u zich aanmelden bij uw Azure-account.

        $UserName = "<user@live.com>"
        $Password = "<password>"
        $SecurePassword = ConvertTo-SecureString -AsPlainText $Password -Force
        $Cred = New-Object System.Management.Automation.PSCredential -ArgumentList $UserName, $SecurePassword
        Connect-AzureRmAccount #-Credential $Cred
2. Een lijst van uw abonnementen, met de abonnement-id's ophalen. Houd er rekening mee de ID van het abonnement waarin u wilt maken van de Recovery Services-kluis. 

        Get-AzureRmSubscription
3. Het abonnement voor de kluis instellen.

        Set-AzureRmContext –SubscriptionID <subscriptionId>

## <a name="create-a-recovery-services-vault"></a>Een Recovery Services-kluis maken
1. Maak een Azure Resource Manager-resourcegroep als u dit niet hebt.

        New-AzureRmResourceGroup -Name #ResourceGroupName -Location #location
2. Maak een nieuwe Recovery Services-kluis. Sla de object-kluis in een variabele moet later worden gebruikt. 

        $vault = New-AzureRmRecoveryServicesVault -Name #vaultname -ResourceGroupName #ResourceGroupName -Location #location
   
    Nadat u dit met behulp van de cmdlet Get-AzureRMRecoveryServicesVault hebt gemaakt, kunt u het object kluis ophalen.

## <a name="set-the-vault-context"></a>De kluiscontext instellen
1. Een bestaande kluis ophalen.

       $vault = Get-AzureRmRecoveryServicesVault -Name #vaultname
2. Stel de context van de kluis.

       Set-AzureRmSiteRecoveryVaultSettings -ARSVault $vault

## <a name="install-the-site-recovery-provider"></a>De Site Recovery-provider installeren
1. Maak een map met de volgende opdracht op de Virtual Machine Manager-machine:

       New-Item c:\ASR -type directory
2. Pak de bestanden met behulp van de gedownloade provider-installatiebestand.

       pushd C:\ASR\
       .\AzureSiteRecoveryProvider.exe /x:. /q
3. Installeer de provider en wachten op installatie is voltooid.

       .\SetupDr.exe /i
       $installationRegPath = "hklm:\software\Microsoft\Microsoft System Center Virtual Machine Manager Server\DRAdapter"
       do
       {
         $isNotInstalled = $true;
         if(Test-Path $installationRegPath)
         {
           $isNotInstalled = $false;
         }
       }While($isNotInstalled)

4. Registreer de server in de kluis.

       $BinPath = $env:SystemDrive+"\Program Files\Microsoft System Center 2012 R2\Virtual Machine Manager\bin"
       pushd $BinPath
       $encryptionFilePath = "C:\temp\".\DRConfigurator.exe /r /Credentials $VaultSettingFilePath /vmmfriendlyname $env:COMPUTERNAME /dataencryptionenabled $encryptionFilePath /startvmmservice

## <a name="create-and-associate-a-replication-policy"></a>Maken en koppelen van een replicatiebeleid
1. Maak een beleid voor wachtwoordreplicatie in dit geval voor Hyper-V 2012 R2, als volgt:

        $ReplicationFrequencyInSeconds = "300";        #options are 30,300,900
        $PolicyName = “replicapolicy”
        $RepProvider = HyperVReplica2012R2
        $Recoverypoints = 24                    #specify the number of hours to retain recovery pints
        $AppConsistentSnapshotFrequency = 4 #specify the frequency (in hours) at which app consistent snapshots are taken
        $AuthMode = "Kerberos"  #options are "Kerberos" or "Certificate"
        $AuthPort = "8083"  #specify the port number that will be used for replication traffic on Hyper-V hosts
        $InitialRepMethod = "Online" #options are "Online" or "Offline"

        $policyresult = New-AzureRmSiteRecoveryPolicy -Name $policyname -ReplicationProvider $RepProvider -ReplicationFrequencyInSeconds $Replicationfrequencyinseconds -RecoveryPoints $recoverypoints -ApplicationConsistentSnapshotFrequencyInHours $AppConsistentSnapshotFrequency -Authentication $AuthMode -ReplicationPort $AuthPort -ReplicationMethod $InitialRepMethod

    > [!NOTE]
    > De cloud Virtual Machine Manager kan Hyper-V-hosts met verschillende versies van Windows Server bevatten, maar het replicatiebeleid is voor een specifieke versie van een besturingssysteem. Als u verschillende hosts die worden uitgevoerd op verschillende besturingssystemen hebt, maakt u afzonderlijke beleidsregels voor elk systeem. Als u vijf hosts die worden uitgevoerd op Windows Server 2012 en drie hosts die worden uitgevoerd op Windows Server 2012 R2 hebt, maakt u bijvoorbeeld twee replicatiebeleidsregels voor. U maakt een voor elk type besturingssysteem.

2. Het ophalen van de primaire beveiligingscontainer (primaire Virtual Machine Manager cloud) en de beveiligingscontainer recovery (herstel cloud Virtual Machine Manager).

       $PrimaryCloud = "testprimarycloud"
       $primaryprotectionContainer = Get-AzureRmSiteRecoveryProtectionContainer -friendlyName $PrimaryCloud;  

       $RecoveryCloud = "testrecoverycloud"
       $recoveryprotectionContainer = Get-AzureRmSiteRecoveryProtectionContainer -friendlyName $RecoveryCloud;  
3. Het replicatiebeleid dat u hebt gemaakt met behulp van de beschrijvende naam worden opgehaald.

       $policy = Get-AzureRmSiteRecoveryPolicy -FriendlyName $policyname
4. Start de koppeling van de beveiligingscontainer (Virtual Machine Manager-cloud) met het replicatiebeleid.

       $associationJob  = Start-AzureRmSiteRecoveryPolicyAssociationJob -Policy     $Policy -PrimaryProtectionContainer $primaryprotectionContainer -RecoveryProtectionContainer $recoveryprotectionContainer
5. Wachten op de koppeling beleid taak is voltooid. Als u wilt controleren als de taak is voltooid, gebruikt u het volgende PowerShell-codefragment:

       $job = Get-AzureRmSiteRecoveryJob -Job $associationJob

       if($job -eq $null -or $job.StateDescription -ne "Completed")
       {
         $isJobLeftForProcessing = $true;
       }

6. Nadat de taak is voltooid voor verwerking, moet u de volgende opdracht uitvoeren:

       if($isJobLeftForProcessing)
       {
         Start-Sleep -Seconds 60
       }
       }While($isJobLeftForProcessing)

Volg de stappen in om te controleren of de bewerking is voltooid, [activiteiten](#monitor-activity).

##  <a name="configure-network-mapping"></a>Netwerktoewijzing configureren
1. Deze opdracht gebruiken om op te halen servers voor de huidige kluis. De opdracht slaat de Site Recovery-servers in de variabele van de matrix $Servers.

        $Servers = Get-AzureRmSiteRecoveryServer
2. Voer deze opdracht uit om op te halen van de netwerken voor de bron-Virtual Machine Manager-server en de doelserver van de Virtual Machine Manager.

        $PrimaryNetworks = Get-AzureRmSiteRecoveryNetwork -Server $Servers[0]        

        $RecoveryNetworks = Get-AzureRmSiteRecoveryNetwork -Server $Servers[1]

    > [!NOTE]
    > De bronserver van de Virtual Machine Manager kan het eerste of tweede item in de matrix van de server zijn. Controleer de namen van Virtual Machine Manager en de netwerken op de juiste wijze opgehaald.


3. Met deze cmdlet maakt een toewijzing tussen het primaire netwerk en het herstelnetwerk. Deze geeft het primaire netwerk als het eerste element van $PrimaryNetworks. Deze geeft het herstelnetwerk als het eerste element van $RecoveryNetworks.

        New-AzureRmSiteRecoveryNetworkMapping -PrimaryNetwork $PrimaryNetworks[0] -RecoveryNetwork $RecoveryNetworks[0]


## <a name="enable-protection-for-vms"></a>Schakel de beveiliging voor virtuele machines
Nadat de servers, clouds en netwerken correct zijn geconfigureerd, schakel de beveiliging voor virtuele machines in de cloud.

1. Als u wilt beveiliging inschakelt, voer de volgende opdracht om op te halen van de beveiligingscontainer:

          $PrimaryProtectionContainer = Get-AzureRmSiteRecoveryProtectionContainer -friendlyName $PrimaryCloudName
2. Haal de bescherming van entiteit (VM), als volgt:

           $protectionEntity = Get-AzureRmSiteRecoveryProtectionEntity -friendlyName $VMName -ProtectionContainer $PrimaryProtectionContainer
3. Schakel replicatie voor de virtuele machine.

          $jobResult = Set-AzureRmSiteRecoveryProtectionEntity -ProtectionEntity $protectionentity -Protection Enable -Policy $policy

## <a name="run-a-test-failover"></a>Een testfailover uitvoeren

Als u wilt testen van uw implementatie, moet u een testfailover voor één virtuele machine uitvoeren. U kunt ook een herstelplan met meerdere virtuele machines maken en uitvoeren van een testfailover uitvoeren voor het abonnement. Met een testfailover wordt uw failover- en herstelmechanisme in een geïsoleerd netwerk gesimuleerd.

1. Ophalen van de virtuele machine waarin virtuele machines een failover uitgevoerd.

       $Servers = Get-AzureRmSiteRecoveryServer
       $RecoveryNetworks = Get-AzureRmSiteRecoveryNetwork -Server $Servers[1]

2. Een testfailover uitvoeren.

   Voor een enkele virtuele machine:

        $protectionEntity = Get-AzureRmSiteRecoveryProtectionEntity -FriendlyName $VMName -ProtectionContainer $PrimaryprotectionContainer

        $jobIDResult =  Start-AzureRmSiteRecoveryTestFailoverJob -Direction PrimaryToRecovery -ProtectionEntity $protectionEntity -VMNetwork $RecoveryNetworks[1]
    
   Voor een plan voor herstel:

        $recoveryplanname = "test-recovery-plan"

        $recoveryplan = Get-AzureRmSiteRecoveryRecoveryPlan -FriendlyName $recoveryplanname

        $jobIDResult =  Start-AzureRmSiteRecoveryTestFailoverJob -Direction PrimaryToRecovery -Recoveryplan $recoveryplan -VMNetwork $RecoveryNetworks[1]

Volg de stappen in om te controleren of de bewerking is voltooid, [activiteiten](#monitor-activity).

## <a name="run-planned-and-unplanned-failovers"></a>Geplande en ongeplande failovers uitvoeren

1. Een geplande failover uitvoeren.

   Voor een enkele virtuele machine:

        $protectionEntity = Get-AzureRmSiteRecoveryProtectionEntity -Name $VMName -ProtectionContainer $PrimaryprotectionContainer

        $jobIDResult =  Start-AzureRmSiteRecoveryPlannedFailoverJob -Direction PrimaryToRecovery -ProtectionEntity $protectionEntity

   Voor een plan voor herstel:

        $recoveryplanname = "test-recovery-plan"

        $recoveryplan = Get-AzureRmSiteRecoveryRecoveryPlan -FriendlyName $recoveryplanname

        $jobIDResult =  Start-AzureRmSiteRecoveryPlannedFailoverJob -Direction PrimaryToRecovery -Recoveryplan $recoveryplan

2. Een niet-geplande failover uitvoeren.

   Voor een enkele virtuele machine:
        
        $protectionEntity = Get-AzureRmSiteRecoveryProtectionEntity -Name $VMName -ProtectionContainer $PrimaryprotectionContainer

        $jobIDResult =  Start-AzureRmSiteRecoveryUnPlannedFailoverJob -Direction PrimaryToRecovery -ProtectionEntity $protectionEntity

   Voor een plan voor herstel:

        $recoveryplanname = "test-recovery-plan"

        $recoveryplan = Get-AzureRmSiteRecoveryRecoveryPlan -FriendlyName $recoveryplanname

        $jobIDResult =  Start-AzureRmSiteRecoveryUnPlannedFailoverJob -Direction PrimaryToRecovery -ProtectionEntity $protectionEntity

## <a name="monitor-activity"></a>Activiteit controleren
Gebruik de volgende opdrachten voor het bewaken van failover-activiteit. Wacht tot de verwerking tussen taken zijn voltooid.

    Do
    {
        $job = Get-AzureSiteRecoveryJob -Id $associationJob.JobId;
        Write-Host "Job State:{0}, StateDescription:{1}" -f Job.State, $job.StateDescription;
        if($job -eq $null -or $job.StateDescription -ne "Completed")
        {
            $isJobLeftForProcessing = $true;
        }

    if($isJobLeftForProcessing)
        {
            Start-Sleep -Seconds 60
        }
    }While($isJobLeftForProcessing)



## <a name="next-steps"></a>Volgende stappen

[Meer informatie](/powershell/module/azurerm.recoveryservices.backup/#recovery) over Site Recovery met Resource Manager PowerShell-cmdlets.
