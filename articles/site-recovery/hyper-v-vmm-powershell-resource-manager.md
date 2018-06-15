---
title: Virtuele Hyper-V-machines in Virtual Machine Manager-clouds repliceren naar een secundaire site met PowerShell (Azure Resource Manager) | Microsoft Docs
description: Hierin wordt beschreven hoe u Hyper-V-machines in Virtual Machine Manager-clouds repliceren naar een secundaire virtuele Machine Manager-site met behulp van PowerShell (Resource Manager)
services: site-recovery
author: sujaytalasila
manager: rochakm
ms.service: site-recovery
ms.topic: article
ms.date: 02/12/2018
ms.author: sutalasi
ms.openlocfilehash: 7c6af1b63d9e7904f5a397200c6950c62df08832
ms.sourcegitcommit: 59914a06e1f337399e4db3c6f3bc15c573079832
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/19/2018
ms.locfileid: "31598775"
---
# <a name="replicate-hyper-v-vms-to-a-secondary-site-by-using-powershell-resource-manager"></a>Hyper-V-machines repliceren naar een secundaire site met behulp van PowerShell (Resource Manager)

Dit artikel laat zien hoe u kunt de stappen automatiseren voor replicatie van Hyper-V-machines in System Center Virtual Machine Manager-clouds naar een cloud Virtual Machine Manager in een secundaire on-premises site met behulp van [Azure Site Recovery](site-recovery-overview.md).

## <a name="prerequisites"></a>Vereisten

- Controleer de [scenario-architectuur en onderdelen](hyper-v-vmm-architecture.md).
- Raadpleeg de [ondersteuningsvereisten](site-recovery-support-matrix-to-sec-site.md) voor alle onderdelen.
- Zorg ervoor dat Virtual Machine Manager-servers en Hyper-V-hosts voldoen [ondersteuningsvereisten](site-recovery-support-matrix-to-sec-site.md).
- Controleer of de virtuele machines die u wilt repliceren voldoen [gerepliceerde machine ondersteuning](site-recovery-support-matrix-to-sec-site.md).


## <a name="prepare-for-network-mapping"></a>Voorbereiden op netwerktoewijzing

[Netwerktoewijzing](hyper-v-vmm-network-mapping.md) toewijzingen tussen lokale Virtual Machine Manager VM-netwerken in de bron en doel-clouds. Toewijzing doet het volgende:

- Maakt verbinding met virtuele machines juiste doel VM-netwerken na een failover. 
- Replica-VM wordt optimaal wordt geplaatst op doelservers Hyper-V-host. 
- Als u geen netwerktoewijzing configureert, replica VMs niet verbonden met een VM-netwerk na een failover.

Virtual Machine Manager als volgt voorbereiden:

* Zorg ervoor dat u hebt [Virtual Machine Manager logische netwerken](https://docs.microsoft.com/system-center/vmm/network-logical) op de bron- en Virtual Machine Manager-servers:

    - Het logische netwerk op de bronserver moet worden gekoppeld aan de broncloud waarin Hyper-V-hosts zich bevinden.
    - Het logische netwerk op de doelserver moet worden gekoppeld aan de doelcloud.
* Zorg ervoor dat u hebt [VM-netwerken](https://docs.microsoft.com/system-center/vmm/network-virtual) op de bron- en Virtual Machine Manager-servers. VM-netwerken moeten worden gekoppeld aan het logische netwerk in elke vestiging.
* Verbinding maken met virtuele machines op de bron-Hyper-V-hosts met de bron-VM-netwerk. 

## <a name="prepare-for-powershell"></a>Voorbereiden voor PowerShell

Zorg ervoor dat u hebt Azure PowerShell gereed om te gaan:

- Als u al PowerShell bijwerken naar versie 0.8.10 of hoger gebruikt. [Meer informatie](/powershell/azureps-cmdlets-docs) over het instellen van PowerShell.
- Na het instellen en configureren van PowerShell, Controleer de [service cmdlets](/powershell/azure/overview).
- Lees voor meer informatie over het gebruik van parameterwaarden, invoer en uitvoer in PowerShell de [aan de slag](/powershell/azure/get-started-azureps) handleiding.

## <a name="set-up-a-subscription"></a>Een abonnement instellen
1. Vanuit PowerShell, moet u zich aanmelden bij uw Azure-account.

        $UserName = "<user@live.com>"
        $Password = "<password>"
        $SecurePassword = ConvertTo-SecureString -AsPlainText $Password -Force
        $Cred = New-Object System.Management.Automation.PSCredential -ArgumentList $UserName, $SecurePassword
        Connect-AzureRmAccount #-Credential $Cred
2. Een lijst van uw abonnementen, met de abonnement-id's ophalen. Noteer de ID van het abonnement waarin u wilt maken van de Recovery Services-kluis. 

        Get-AzureRmSubscription
3. Stel het abonnement voor de kluis.

        Set-AzureRmContext –SubscriptionID <subscriptionId>

## <a name="create-a-recovery-services-vault"></a>Een Recovery Services-kluis maken
1. Maak een Azure Resource Manager-resourcegroep als u niet hebt.

        New-AzureRmResourceGroup -Name #ResourceGroupName -Location #location
2. Maak een nieuwe Recovery Services-kluis. Het object kluis opslaan in een variabele voor later gebruik. 

        $vault = New-AzureRmRecoveryServicesVault -Name #vaultname -ResouceGroupName #ResourceGroupName -Location #location
   
    Nadat u deze met de cmdlet Get-AzureRMRecoveryServicesVault hebt gemaakt, kunt u de kluis-object ophalen.

## <a name="set-the-vault-context"></a>De context van de kluis instellen
1. Ophalen van een bestaande kluis.

       $vault = Get-AzureRmRecoveryServicesVault -Name #vaultname
2. Hiermee stelt u de context van de kluis.

       Set-AzureRmSiteRecoveryVaultSettings -ARSVault $vault

## <a name="install-the-site-recovery-provider"></a>De Site Recovery provider installeren
1. Maak een map met de volgende opdracht op de machine Virtual Machine Manager:

       New-Item c:\ASR -type directory
2. Pak de bestanden met behulp van het gedownloade provider setup-bestand.

       pushd C:\ASR\
       .\AzureSiteRecoveryProvider.exe /x:. /q
3. Installeer de provider en wacht totdat de installatie te voltooien.

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

## <a name="create-and-associate-a-replication-policy"></a>Maakt en koppelt u een beleid voor wachtwoordreplicatie
1. Maak een beleid voor wachtwoordreplicatie in dit geval voor Hyper-V 2012 R2 als volgt:

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
    > De cloud Virtual Machine Manager kan Hyper-V-hosts met verschillende versies van Windows Server bevatten, maar het replicatiebeleid is voor een specifieke versie van een besturingssysteem. Als u verschillende hosts die worden uitgevoerd op verschillende besturingssystemen hebt, afzonderlijke replicatiegroepen beleid maken voor elk systeem. Als er vijf hosts waarop Windows Server 2012 en drie hosts waarop Windows Server 2012 R2, bijvoorbeeld twee beleidsregels voor replicatie maken. U kunt één voor elk type besturingssysteem.

2. Ophalen van de primaire beveiligingscontainer (primaire cloud voor Virtual Machine Manager) en herstel de beveiligingscontainer (herstelserver cloud Virtual Machine Manager).

       $PrimaryCloud = "testprimarycloud"
       $primaryprotectionContainer = Get-AzureRmSiteRecoveryProtectionContainer -friendlyName $PrimaryCloud;  

       $RecoveryCloud = "testrecoverycloud"
       $recoveryprotectionContainer = Get-AzureRmSiteRecoveryProtectionContainer -friendlyName $RecoveryCloud;  
3. Ophalen van het replicatiebeleid dat u hebt gemaakt met de beschrijvende naam.

       $policy = Get-AzureRmSiteRecoveryPolicy -FriendlyName $policyname
4. De koppeling van de beveiligingscontainer (Virtual Machine Manager cloud) met het replicatiebeleid beginnen.

       $associationJob  = Start-AzureRmSiteRecoveryPolicyAssociationJob -Policy     $Policy -PrimaryProtectionContainer $primaryprotectionContainer -RecoveryProtectionContainer $recoveryprotectionContainer
5. Wachten op de koppeling beleid taak is voltooid. Om te zien als de taak is voltooid, gebruikt u het volgende PowerShell-fragment:

       $job = Get-AzureRmSiteRecoveryJob -Job $associationJob

       if($job -eq $null -or $job.StateDescription -ne "Completed")
       {
         $isJobLeftForProcessing = $true;
       }

6. Nadat de taak is voltooid voor verwerking, kunt u de volgende opdracht uitvoeren:

       if($isJobLeftForProcessing)
       {
         Start-Sleep -Seconds 60
       }
       }While($isJobLeftForProcessing)

Volg de stappen in om te zien wanneer de bewerking is voltooid, [activiteitsbewaking](#monitor-activity).

##  <a name="configure-network-mapping"></a>Netwerktoewijzing configureren
1. Deze opdracht gebruiken om op te halen servers voor de huidige kluis. De opdracht slaat de Site Recovery-servers in de $Servers matrixvariabele.

        $Servers = Get-AzureRmSiteRecoveryServer
2. Voer deze opdracht voor het ophalen van de netwerken voor de bron-Virtual Machine Manager-beheerserver en de doelserver voor Virtual Machine Manager.

        $PrimaryNetworks = Get-AzureRmSiteRecoveryNetwork -Server $Servers[0]        

        $RecoveryNetworks = Get-AzureRmSiteRecoveryNetwork -Server $Servers[1]

    > [!NOTE]
    > De bronserver voor Virtual Machine Manager kan het eerste of tweede certificaat in de server-matrix zijn. Controleer de namen van de Virtual Machine Manager-server en de netwerken op de juiste wijze opgehaald.


3. Deze cmdlet maakt een toewijzing tussen de primaire netwerk en het herstel. Dit geeft het primaire netwerk als het eerste element van $PrimaryNetworks. Dit geeft de herstel-netwerk als het eerste element van $RecoveryNetworks.

        New-AzureRmSiteRecoveryNetworkMapping -PrimaryNetwork $PrimaryNetworks[0] -RecoveryNetwork $RecoveryNetworks[0]


## <a name="enable-protection-for-vms"></a>Schakel de beveiliging voor virtuele machines
Nadat de servers, clouds en netwerken correct zijn geconfigureerd, schakel de beveiliging voor virtuele machines in de cloud.

1. Als beveiliging wilt inschakelen, voer de volgende opdracht voor het ophalen van de beveiligingscontainer:

          $PrimaryProtectionContainer = Get-AzureRmSiteRecoveryProtectionContainer -friendlyName $PrimaryCloudName
2. Haal de entiteit beveiliging (VM), als volgt:

           $protectionEntity = Get-AzureRmSiteRecoveryProtectionEntity -friendlyName $VMName -ProtectionContainer $PrimaryProtectionContainer
3. Replicatie inschakelen voor de virtuele machine.

          $jobResult = Set-AzureRmSiteRecoveryProtectionEntity -ProtectionEntity $protectionentity -Protection Enable -Policy $policy

## <a name="run-a-test-failover"></a>Een testfailover uitvoeren

Als u wilt testen van uw implementatie, moet u een testfailover voor één virtuele machine uitvoeren. U kunt ook een herstelplan met meerdere virtuele machines maken en een testfailover voor het plan uitvoeren. Met een testfailover wordt uw failover- en herstelmechanisme in een geïsoleerd netwerk gesimuleerd.

1. De virtuele machine waarin virtuele machines failover worden opgehaald.

       $Servers = Get-AzureRmSiteRecoveryServer
       $RecoveryNetworks = Get-AzureRmSiteRecoveryNetwork -Server $Servers[1]

2. Een testfailover uitvoeren.

   Voor een enkele VM:

        $protectionEntity = Get-AzureRmSiteRecoveryProtectionEntity -FriendlyName $VMName -ProtectionContainer $PrimaryprotectionContainer

        $jobIDResult =  Start-AzureRmSiteRecoveryTestFailoverJob -Direction PrimaryToRecovery -ProtectionEntity $protectionEntity -VMNetwork $RecoveryNetworks[1]
    
   Voor een herstelplan:

        $recoveryplanname = "test-recovery-plan"

        $recoveryplan = Get-AzureRmSiteRecoveryRecoveryPlan -FriendlyName $recoveryplanname

        $jobIDResult =  Start-AzureRmSiteRecoveryTestFailoverJob -Direction PrimaryToRecovery -Recoveryplan $recoveryplan -VMNetwork $RecoveryNetworks[1]

Volg de stappen in om te zien wanneer de bewerking is voltooid, [activiteitsbewaking](#monitor-activity).

## <a name="run-planned-and-unplanned-failovers"></a>Geplande en ongeplande failovers uitvoeren

1. Voer een geplande failover.

   Voor een enkele VM:

        $protectionEntity = Get-AzureRmSiteRecoveryProtectionEntity -Name $VMName -ProtectionContainer $PrimaryprotectionContainer

        $jobIDResult =  Start-AzureRmSiteRecoveryPlannedFailoverJob -Direction PrimaryToRecovery -ProtectionEntity $protectionEntity

   Voor een herstelplan:

        $recoveryplanname = "test-recovery-plan"

        $recoveryplan = Get-AzureRmSiteRecoveryRecoveryPlan -FriendlyName $recoveryplanname

        $jobIDResult =  Start-AzureRmSiteRecoveryPlannedFailoverJob -Direction PrimaryToRecovery -Recoveryplan $recoveryplan

2. Een niet-geplande failover uitvoeren.

   Voor een enkele VM:
        
        $protectionEntity = Get-AzureRmSiteRecoveryProtectionEntity -Name $VMName -ProtectionContainer $PrimaryprotectionContainer

        $jobIDResult =  Start-AzureRmSiteRecoveryUnPlannedFailoverJob -Direction PrimaryToRecovery -ProtectionEntity $protectionEntity

   Voor een herstelplan:

        $recoveryplanname = "test-recovery-plan"

        $recoveryplan = Get-AzureRmSiteRecoveryRecoveryPlan -FriendlyName $recoveryplanname

        $jobIDResult =  Start-AzureRmSiteRecoveryUnPlannedFailoverJob -Direction PrimaryToRecovery -ProtectionEntity $protectionEntity

## <a name="monitor-activity"></a>Voor monitoractiviteit
Gebruik de volgende opdrachten voor het bewaken van failover-activiteit. Wacht totdat de verwerking tussen taken zijn voltooid.

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
