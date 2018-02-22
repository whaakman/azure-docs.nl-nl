---
title: Hyper-V-machines in VMM-clouds repliceren naar een secundaire site met PowerShell (Azure Resource Manager) | Microsoft Docs
description: Hierin wordt beschreven hoe u Hyper-V-machines in VMM-clouds repliceren naar een secundaire VMM-site met behulp van PowerShell (Resource Manager)
services: site-recovery
author: sujaytalasila
manager: rochakm
ms.service: site-recovery
ms.topic: article
ms.date: 02/12/2018
ms.author: sutalasi
ms.openlocfilehash: a5e36546494223b20844303f3f76782746658411
ms.sourcegitcommit: d87b039e13a5f8df1ee9d82a727e6bc04715c341
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/21/2018
---
# <a name="replicate-hyper-v-vms-to-a-secondary-site-using-powershell-resource-manager"></a>Virtuele Hyper-V-machines repliceren naar een secundaire site met behulp van PowerShell (Resource Manager)

In dit artikel ziet u de stappen voor de replicatie van Hyper-V-machines in System Center Virtual Machine Manager (VMM)-clouds naar een VMM-cloud in een secundaire site van de on-premises automatiseren met behulp van [Azure Site Recovery](site-recovery-overview.md).

## <a name="prerequisites"></a>Vereisten

- Controleer de [scenario-architectuur en onderdelen](hyper-v-vmm-architecture.md).
- Controleer de [ondersteuningsvereisten](site-recovery-support-matrix-to-sec-site.md) voor alle onderdelen.
- Zorg ervoor dat VMM-servers en Hyper-V-hosts voldoen [ondersteuningsvereisten](site-recovery-support-matrix-to-sec-site.md).
- Controleer of de virtuele machines die u wilt repliceren voldoen [gerepliceerde machine-ondersteuning](site-recovery-support-matrix-to-sec-site.md#support-for-replicated-machine-os-versions)


## <a name="prepare-for-network-mapping"></a>Voorbereiden op netwerktoewijzing

[Netwerktoewijzing](hyper-v-vmm-network-mapping.md) toewijzingen tussen lokale VMM VM-netwerken in de bron en doel-clouds. Toewijzing doet het volgende:

- Maakt verbinding met virtuele machines juiste doel VM-netwerken na een failover. 
- Replica-VM wordt optimaal wordt geplaatst op doelservers Hyper-V-host. 
- Als u geen netwerktoewijzing configureert, replica VMs niet verbonden met een VM-netwerk na een failover.

VMM als volgt voorbereiden:

1. Zorg ervoor dat u hebt [logische netwerken VMM](https://docs.microsoft.com/system-center/vmm/network-logical) op de bron en doel-VMM-servers.
    - Het logische netwerk op de bronserver moet worden gekoppeld aan de broncloud waarin Hyper-V-hosts zich bevinden.
    - Het logische netwerk op de doelserver moet worden gekoppeld aan de doelcloud.
1. Zorg ervoor dat u hebt [VM-netwerken](https://docs.microsoft.com/system-center/vmm/network-virtual) op de bron en doel-VMM-servers. VM-netwerken moeten worden gekoppeld aan het logische netwerk in elke vestiging.
2. Verbinding maken met virtuele machines op de bron-Hyper-V-hosts met de bron-VM-netwerk. 

## <a name="prepare-for-powershell"></a>Voorbereiden voor PowerShell

Zorg ervoor dat u hebt Azure PowerShell klaar voor gebruik.

- Als u al van PowerShell gebruikmaakt, moet u een upgrade uitvoeren naar versie 0.8.10 of hoger.  [Meer informatie](/powershell/azureps-cmdlets-docs) over het instellen van PowerShell.
- Nadat u hebt ingesteld en geconfigureerd PowerShell, controleren de [service cmdlets](/powershell/azure/overview).
- Lees meer informatie over het gebruik van parameterwaarden, invoer en uitvoer in Azure PowerShell, de [aan de slag](/powershell/azure/get-started-azureps) handleiding.

## <a name="set-up-a-subscription"></a>Een abonnement instellen
1. Meld u aan bij uw Azure-account van Azure PowerShell:

        $UserName = "<user@live.com>"
        $Password = "<password>"
        $SecurePassword = ConvertTo-SecureString -AsPlainText $Password -Force
        $Cred = New-Object System.Management.Automation.PSCredential -ArgumentList $UserName, $SecurePassword
        Login-AzureRmAccount #-Credential $Cred
2. Een lijst van uw abonnementen, met de abonnement-id's ophalen. Noteer de ID van het abonnement waarin u wilt maken van de Recovery Services-kluis.   

        Get-AzureRmSubscription
3. Het abonnement voor de kluis instellen:

        Set-AzureRmContext –SubscriptionID <subscriptionId>

## <a name="create-a-recovery-services-vault"></a>Een Recovery Services-kluis maken
1. Maak een Azure Resource Manager-resourcegroep als u niet hebt.

        New-AzureRmResourceGroup -Name #ResourceGroupName -Location #location
2. Maak een nieuwe Recovery Services-kluis en slaat u het object kluis in een variabele die later worden gebruikt: 

        $vault = New-AzureRmRecoveryServicesVault -Name #vaultname -ResouceGroupName #ResourceGroupName -Location #location
   
    Nadat u deze hebt gemaakt, met de cmdlet Get-AzureRMRecoveryServicesVault, kunt u de kluis-object ophalen.

## <a name="set-the-vault-context"></a>De context van de kluis instellen
1. Ophalen van een bestaande kluis:

       $vault = Get-AzureRmRecoveryServicesVault -Name #vaultname
2. Stel de context van de kluis:

       Set-AzureRmSiteRecoveryVaultSettings -ARSVault $vault

## <a name="install-the-azure-site-recovery-provider"></a>De Azure Site Recovery Provider installeren
1. Op de VMM-machine, maak een map met de volgende opdracht:

       New-Item c:\ASR -type directory
2. Pak de bestanden met behulp van het gedownloade installatiebestand van Provider: pushd C:\ASR\
       .\AzureSiteRecoveryProvider.exe /x:. /q
3. Installeer de provider en wacht totdat de installatie te voltooien:

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

4. De server in de kluis registreren:

       $BinPath = $env:SystemDrive+"\Program Files\Microsoft System Center 2012 R2\Virtual Machine Manager\bin"
       pushd $BinPath
       $encryptionFilePath = "C:\temp\".\DRConfigurator.exe /r /Credentials $VaultSettingFilePath /vmmfriendlyname $env:COMPUTERNAME /dataencryptionenabled $encryptionFilePath /startvmmservice

## <a name="create-and-associate-a-replication-policy"></a>Maakt en koppelt u een beleid voor wachtwoordreplicatie
1. Maak een beleid voor wachtwoordreplicatie (in dit geval voor Hyper-V 2012 R2), als volgt:

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
    > De VMM-cloud kan Hyper-V-hosts met verschillende versies van Windows Server bevatten, maar het replicatiebeleid is voor een specifieke versie van een besturingssysteem. Als u verschillende hosts die worden uitgevoerd op verschillende besturingssystemen worden uitgevoerd hebt, maakt u afzonderlijke replicatiegroepen beleid voor elk systeem. Als er vijf hosts die worden uitgevoerd op Windows-Servers 2012 en drie op Windows Server 2012 R2, bijvoorbeeld twee beleidsregels voor replicatie: één voor elk type besturingssysteem maken.

2. Ophalen van de primaire beveiligingscontainer (primaire VMM c) en herstel de beveiligingscontainer (herstelserver VMM-cloud):

       $PrimaryCloud = "testprimarycloud"
       $primaryprotectionContainer = Get-AzureRmSiteRecoveryProtectionContainer -friendlyName $PrimaryCloud;  

       $RecoveryCloud = "testrecoverycloud"
       $recoveryprotectionContainer = Get-AzureRmSiteRecoveryProtectionContainer -friendlyName $RecoveryCloud;  
2. Ophalen van het replicatiebeleid dat u hebt gemaakt met de beschrijvende naam:

       $policy = Get-AzureRmSiteRecoveryPolicy -FriendlyName $policyname
3. De koppeling van de beveiligingscontainer (VMM-Cloud) met het replicatiebeleid starten:

       $associationJob  = Start-AzureRmSiteRecoveryPolicyAssociationJob -Policy     $Policy -PrimaryProtectionContainer $primaryprotectionContainer -RecoveryProtectionContainer $recoveryprotectionContainer
4. Wacht totdat de taak van de koppeling beleid om te voltooien. U kunt controleren als de taak is voltooid met behulp van de volgende PowerShell-fragment:

       $job = Get-AzureRmSiteRecoveryJob -Job $associationJob

       if($job -eq $null -or $job.StateDescription -ne "Completed")
       {
         $isJobLeftForProcessing = $true;
       }

   Wanneer de verwerking van de taak is voltooid, kunt u de volgende opdracht uitvoeren:

       if($isJobLeftForProcessing)
       {
         Start-Sleep -Seconds 60
       }
       }While($isJobLeftForProcessing)

Volg de stappen in om te zien wanneer de bewerking is voltooid, [activiteitsbewaking](#monitor-activity).

##  <a name="configure-network-mapping"></a>Netwerktoewijzing configureren
1. Deze opdracht gebruiken om op te halen servers voor de huidige kluis. De opdracht slaat de Azure Site Recovery-servers in de $Servers matrixvariabele.

        $Servers = Get-AzureRmSiteRecoveryServer
2. Deze opdracht uitvoeren om te retrievet de netwerken voor de bron-VMM-beheerserver en de doel-VMM-server.

        $PrimaryNetworks = Get-AzureRmSiteRecoveryNetwork -Server $Servers[0]        

        $RecoveryNetworks = Get-AzureRmSiteRecoveryNetwork -Server $Servers[1]

    > [!NOTE]
    > De bron-VMM-server kan het eerste of tweede voorbeeld in de matrix servers zijn. Controleer de namen van de VMM-server en de netwerken op de juiste wijze opgehaald.


3. Deze cmdlet maakt een toewijzing tussen de primaire netwerk en het herstel. Het geeft primaire netwerk als het eerste element van $PrimaryNetworks en het netwerk herstel als het eerste element van $RecoveryNetworks.

        New-AzureRmSiteRecoveryNetworkMapping -PrimaryNetwork $PrimaryNetworks[0] -RecoveryNetwork $RecoveryNetworks[0]


## <a name="enable-protection-for-vms"></a>Schakel de beveiliging voor virtuele machines
Nadat de servers, clouds en netwerken correct zijn geconfigureerd, kunt u beveiliging voor VM's in de cloud inschakelen.

1. Als beveiliging wilt inschakelen, voer de volgende opdracht voor het ophalen van de beveiligingscontainer:

          $PrimaryProtectionContainer = Get-AzureRmSiteRecoveryProtectionContainer -friendlyName $PrimaryCloudName
2. Haal de entiteit beveiliging (VM) als volgt:

           $protectionEntity = Get-AzureRmSiteRecoveryProtectionEntity -friendlyName $VMName -ProtectionContainer $PrimaryProtectionContainer
3. Replicatie voor de virtuele machine inschakelen:

          $jobResult = Set-AzureRmSiteRecoveryProtectionEntity -ProtectionEntity $protectionentity -Protection Enable -Policy $policy

## <a name="run-a-test-failover"></a>Een testfailover uitvoeren

Voor het testen van uw implementatie kunt u een testfailover voor één virtuele machine uitvoeren of maken van een herstelplan met meerdere virtuele machines, en en een testfailover voor het plan uitvoeren. Met een testfailover wordt uw failover- en herstelmechanisme in een geïsoleerd netwerk gesimuleerd.

1. De virtuele machine waarin virtuele machines failover ophalen:

       $Servers = Get-AzureRmSiteRecoveryServer
       $RecoveryNetworks = Get-AzureRmSiteRecoveryNetwork -Server $Servers[1]
2. Een testfailover als volgt uitvoeren:
    - Voor een enkele VM

        $protectionEntity = Get-AzureRmSiteRecoveryProtectionEntity -FriendlyName $VMName -ProtectionContainer $PrimaryprotectionContainer

        $jobIDResult =  Start-AzureRmSiteRecoveryTestFailoverJob -Direction PrimaryToRecovery -ProtectionEntity $protectionEntity -VMNetwork $RecoveryNetworks[1]
    
    - Voor een herstelplan:

        $recoveryplanname = 'test--herstelplan'

        $recoveryplan = Get-AzureRmSiteRecoveryRecoveryPlan -FriendlyName $recoveryplanname

        $jobIDResult =  Start-AzureRmSiteRecoveryTestFailoverJob -Direction PrimaryToRecovery -Recoveryplan $recoveryplan -VMNetwork $RecoveryNetworks[1]

Volg de stappen in om te zien wanneer de bewerking is voltooid, [activiteitsbewaking](#monitor-activity).

## <a name="run-planned-and-unplanned-failovers"></a>Geplande en ongeplande failovers uitvoeren

1. Voer een geplande failover als volgt:
    -  Voor een enkele VM:

        $protectionEntity = Get-AzureRmSiteRecoveryProtectionEntity -Name $VMName -ProtectionContainer $PrimaryprotectionContainer

        $jobIDResult = start AzureRmSiteRecoveryPlannedFailoverJob-richting PrimaryToRecovery - ProtectionEntity $protectionEntity
    - Voor een herstelplan

        $recoveryplanname = 'test--herstelplan'

        $recoveryplan = Get-AzureRmSiteRecoveryRecoveryPlan -FriendlyName $recoveryplanname

        $jobIDResult = start AzureRmSiteRecoveryPlannedFailoverJob-richting PrimaryToRecovery - Recoveryplan $recoveryplan
2. Voer een niet-geplande failover als volgt uit:
    - Voor een enkele VM
        
        $protectionEntity = Get-AzureRmSiteRecoveryProtectionEntity -Name $VMName -ProtectionContainer $PrimaryprotectionContainer

        $jobIDResult = start AzureRmSiteRecoveryUnPlannedFailoverJob-richting PrimaryToRecovery - ProtectionEntity $protectionEntity

    - Voor een herstelplan:

        $recoveryplanname = 'test--herstelplan'

        $recoveryplan = Get-AzureRmSiteRecoveryRecoveryPlan -FriendlyName $recoveryplanname

        $jobIDResult = start AzureRmSiteRecoveryUnPlannedFailoverJob-richting PrimaryToRecovery - ProtectionEntity $protectionEntity

## <a name="monitor-activity"></a>Voor monitoractiviteit
Gebruik de volgende opdrachten failver activiteit bewaken. Houd er rekening mee dat u wachten tussen taken voor de verwerking moet te voltooien.

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

[Meer informatie](/powershell/module/azurerm.recoveryservices.backup/#recovery) over Site Recovery met Azure Resource Manager PowerShell-cmdlets.
