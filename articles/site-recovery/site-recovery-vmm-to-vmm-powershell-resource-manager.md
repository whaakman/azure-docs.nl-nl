---
title: Hyper-V-machines in VMM repliceren naar een secundaire site met PowerShell (Azure Resource Manager) | Microsoft Docs
description: Hierin wordt beschreven hoe u Azure Site Recovery implementeert om replicatie, failovers en herstel van Hyper-V-machines in VMM-clouds naar een secundaire VMM-site met behulp van PowerShell (Resource Manager) indelen
services: site-recovery
documentationcenter: 
author: sujaytalasila
manager: rochakm
editor: raynew
ms.assetid: 9d38e9c3-217c-4e44-830c-575e9a4141f2
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/19/2017
ms.author: sutalasi
ms.openlocfilehash: c978c2e31e775f56824d765491f6d7b73648b8ae
ms.sourcegitcommit: 76a3cbac40337ce88f41f9c21a388e21bbd9c13f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/25/2017
---
# <a name="replicate-hyper-v-virtual-machines-in-vmm-clouds-to-a-secondary-vmm-site-using-powershell-resource-manager"></a>Hyper-V virtuele machines in VMM-clouds repliceren naar een secundaire VMM-site met behulp van PowerShell (Resource Manager)

In dit artikel leest u hoe u PowerShell gebruikt voor het automatiseren van algemene taken bij het instellen van Azure Site Recovery op Hyper-V virtuele machines in System Center VMM-clouds repliceren naar System Center VMM-clouds in een secundaire site.



## <a name="on-premises-prerequisites"></a>Vereisten voor on-premises
Dit is wat u nodig hebt in de primaire en secundaire on-premises sites voor het implementeren van dit scenario:

| **Vereisten** | **Details** |
| --- | --- |
| **VMM** |Het is raadzaam om de implementatie van een VMM-server in de primaire site en een VMM-server in de secundaire site.<br/><br/> U kunt ook repliceren tussen clouds op één VMM-server. Hiervoor moet u ten minste twee clouds die zijn geconfigureerd op de VMM-server.<br/><br/> VMM-servers waarop ten minste System Center 2012 SP1 met de nieuwste updates.<br/><br/> Elke VMM-server moet op een of meer clouds zijn geconfigureerd en alle clouds moeten het Hyper-V-capaciteitsprofiel instellen. <br/><br/>Clouds moeten een of meer VMM-hostgroepen bevatten. VMM-servers moeten toegang tot internet hebben. |
| **Hyper-V** |Hyper-V-servers moeten ten minste draaien op Windows Server 2012 met de Hyper-V-rol en de meest recente updates hebt geïnstalleerd.<br/><br/> Een Hyper-V-server moet een of meer virtuele machines bevatten.<br/><br/>  Hyper-V-hostservers moeten zich in hostgroepen in de primaire en secundaire VMM-clouds.<br/><br/> Als u Hyper-V in een cluster op Windows Server 2012 R2 moet u eerst installeren [2961977 bijwerken](https://support.microsoft.com/kb/2961977)<br/><br/> Als u werkt met Hyper-V in een cluster op Windows Server 2012-opmerking die clusterbroker niet automatisch gemaakt als u een statische IP-adressen gebaseerde cluster hebt. U moet de clusterbroker handmatig configureren. [Meer informatie](http://social.technet.microsoft.com/wiki/contents/articles/18792.configure-replica-broker-role-cluster-to-cluster-replication.aspx). |
| **Provider** |Tijdens de implementatie van Site Recovery kunt u de Azure Site Recovery Provider installeren op VMM-servers. De Provider communiceert met Site Recovery via HTTPS 443 naar de organisatie van replicatie. Gegevens replicatie tussen de primaire en secundaire Hyper-V-servers via het LAN over of een VPN-verbinding.<br/><br/> De Provider die wordt uitgevoerd op de VMM-beheerserver moet toegang tot deze URL's: *. hypervrecoverymanager.windowsazure.com; *. accesscontrol.windows.net; *. backup.windowsazure.com; *. blob.core.windows.net; *. store.core.windows.net.<br/><br/> Bovendien toestaan firewall communicatie van de VMM-servers voor de [Azure datacenter IP-adresbereiken](https://www.microsoft.com/download/confirmation.aspx?id=41653) en het protocol HTTPS (443) toestaan. |

### <a name="network-mapping-prerequisites"></a>Vereisten voor netwerktoewijzing
Koppelingen van Netwerktoewijzingen tussen VMM VM-netwerken op de primaire en secundaire VMM-servers voor het volgende:

* Replica-VM optimaal op secundaire Hyper-V-hosts plaatsen na een failover.
* Verbinding met het maken van replica-VM met de juiste VM-netwerken.
* Als u geen netwerk toewijzing replica die VMS niet na een failover met een netwerk verbonden configureert.
* Als u wilt instellen netwerk is toewijzen tijdens het siteherstel implementatie hier wat u nodig hebt:

  * Zorg ervoor dat de virtuele machines op de Hyper-V-bronhostserver zijn verbonden met een VMM-netwerk met virtuele machines. Dit netwerk moet zijn gekoppeld aan een logisch netwerk dat is verbonden met de cloud.
  * Controleer of de secundaire cloud die u voor herstel gebruiken gaat heeft een bijbehorende VM-netwerk is geconfigureerd. Deze VM-netwerk moet zijn gekoppeld aan een logisch netwerk dat is gekoppeld aan de secundaire cloud.

Meer informatie over het configureren van netwerken in VMM de onderstaande artikelen

* [Het configureren van logische netwerken in VMM](http://go.microsoft.com/fwlink/p/?LinkId=386307)
* [Het VM-netwerken en gateways configureren in VMM](http://go.microsoft.com/fwlink/p/?LinkId=386308)


### <a name="powershell-prerequisites"></a>PowerShell-vereisten
Zorg ervoor dat u hebt Azure PowerShell klaar voor gebruik. Als u al van PowerShell gebruikmaakt, moet u een upgrade uitvoeren naar versie 0.8.10 of hoger. Zie voor meer informatie over het instellen van PowerShell, de [handleiding voor het installeren en configureren van Azure PowerShell](/powershell/azureps-cmdlets-docs). Zodra u hebt ingesteld en PowerShell geconfigureerd, kunt u alle van de beschikbare cmdlets voor de service weergeven [hier](/powershell/azure/overview).

Zie voor meer informatie over tips kunt u de cmdlets, zoals hoe parameterwaarden, in- en uitgangen doorgaans worden uitgevoerd in Azure PowerShell gebruiken de [handleiding aan de slag met Azure-Cmdlets](/powershell/azure/get-started-azureps).

## <a name="step-1-set-the-subscription"></a>Stap 1: Stel het abonnement
1. Van Azure powershell, meld u aan bij uw Azure-account: met de volgende cmdlets

        $UserName = "<user@live.com>"
        $Password = "<password>"
        $SecurePassword = ConvertTo-SecureString -AsPlainText $Password -Force
        $Cred = New-Object System.Management.Automation.PSCredential -ArgumentList $UserName, $SecurePassword
        Login-AzureRmAccount #-Credential $Cred
2. Een lijst van uw abonnementen ophalen. Hiermee worden de subscriptionIDs vermeld voor elk van de abonnementen. Noteer de abonnements-id van het abonnement waarin u wilt de recovery services-kluis maken    

        Get-AzureRmSubscription
3. Het abonnement waarmee de recovery services-kluis worden gemaakt door de vermelding van de abonnements-ID is ingesteld

        Set-AzureRmContext –SubscriptionID <subscriptionId>

## <a name="step-2-create-a-recovery-services-vault"></a>Stap 2: Maak een Recovery Services-kluis
1. Een Azure Resource Manager-resourcegroep maken als u nog niet hebt

        New-AzureRmResourceGroup -Name #ResourceGroupName -Location #location
2. Maak een nieuwe Recovery Services-kluis en de Recovery Services-kluis object opslaan in een variabele (later worden gebruikt). U kunt ook de kluis post maken van het object met de cmdlet Get-AzureRMRecoveryServicesVault ophalen:-

        $vault = New-AzureRmRecoveryServicesVault -Name #vaultname -ResouceGroupName #ResourceGroupName -Location #location

## <a name="step-3-set-the-recovery-services-vault-context"></a>Stap 3: Stel de context van Recovery Services-kluis
1. Als u een kluis hebt gemaakt hebt, voert u de onderstaande opdracht om op te halen van de kluis.

       $vault = Get-AzureRmRecoveryServicesVault -Name #vaultname
2. De context van de kluis ingesteld door de onderstaande opdracht.

       Set-AzureRmSiteRecoveryVaultSettings -ARSVault $vault

## <a name="step-4-install-the-azure-site-recovery-provider"></a>Stap 4: De Azure Site Recovery Provider installeren
1. Op de VMM-machine, maak een map met de volgende opdracht:

       New-Item c:\ASR -type directory
2. Pak de bestanden met de gedownloade provider door de volgende opdracht uit te voeren

       pushd C:\ASR\
       .\AzureSiteRecoveryProvider.exe /x:. /q
3. Installeer de provider met de volgende opdrachten:

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

   Wacht totdat de installatie is voltooid.
4. Registreer de server in de kluis die met de volgende opdracht:

       $BinPath = $env:SystemDrive+"\Program Files\Microsoft System Center 2012 R2\Virtual Machine Manager\bin"
       pushd $BinPath
       $encryptionFilePath = "C:\temp\".\DRConfigurator.exe /r /Credentials $VaultSettingFilePath /vmmfriendlyname $env:COMPUTERNAME /dataencryptionenabled $encryptionFilePath /startvmmservice

## <a name="step-5-create-and-associate-a-replication-policy"></a>Stap 5: Maken en koppelen van een beleid voor wachtwoordreplicatie
1. Maak een beleid van de replicatie Hyper-V 2012 R2 met de volgende opdracht:

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
    > De VMM-cloud Hyper-V-hosts met verschillende versies van Windows Server (zoals vermeld in de Hyper-V-vereisten) kan bevatten, maar het replicatiebeleid specifieke versie van het besturingssysteem is. Als u andere hosts die worden uitgevoerd op verschillende besturingssysteemversies hebt, maakt u afzonderlijke replicatiegroepen beleid voor elk type van de versie van het besturingssysteem. Als er vijf hosts die worden uitgevoerd op Windows-Servers 2012 en drie op Windows Server 2012 R2, bijvoorbeeld twee beleidsregels voor replicatie: één voor elk type besturingssysteemversies maken.

1. De primaire beveiligingscontainer (primaire VMM-Cloud) en herstel de beveiligingscontainer (herstelserver VMM-Cloud) ophalen met de volgende opdrachten:

       $PrimaryCloud = "testprimarycloud"
       $primaryprotectionContainer = Get-AzureRmSiteRecoveryProtectionContainer -friendlyName $PrimaryCloud;  

       $RecoveryCloud = "testrecoverycloud"
       $recoveryprotectionContainer = Get-AzureRmSiteRecoveryProtectionContainer -friendlyName $RecoveryCloud;  
2. Ophalen van het beleid dat u hebt gemaakt in stap 1 met de beschrijvende naam van het beleid

       $policy = Get-AzureRmSiteRecoveryPolicy -FriendlyName $policyname
3. De koppeling van de beveiligingscontainer (VMM-Cloud) met het replicatiebeleid starten:

       $associationJob  = Start-AzureRmSiteRecoveryPolicyAssociationJob -Policy     $Policy -PrimaryProtectionContainer $primaryprotectionContainer -RecoveryProtectionContainer $recoveryprotectionContainer
4. Wacht totdat de taak van de koppeling beleid om te voltooien. U kunt controleren als de taak is voltooid met behulp van de volgende PowerShell-codefragment.

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

Volg de stappen in om te zien wanneer de bewerking is voltooid, [Monitoractiviteit](#monitor).

## <a name="step-6-configure-network-mapping"></a>Stap 6: Netwerktoewijzing configureren
1. De eerste opdracht haalt servers voor de huidige Azure Site Recovery-kluis. De opdracht slaat de Microsoft Azure Site Recovery-servers in de $Servers matrixvariabele.

        $Servers = Get-AzureRmSiteRecoveryServer
2. De volgende opdracht uit het netwerk met Site Recovery voor de bron-VMM-server en de doel-VMM-beheerserver niet ophalen.

        $PrimaryNetworks = Get-AzureRmSiteRecoveryNetwork -Server $Servers[0]        

        $RecoveryNetworks = Get-AzureRmSiteRecoveryNetwork -Server $Servers[1]

    > [!NOTE]
    > De bron-VMM-server kan niet het eerste beheerpunt of in het tweede voorbeeld in de matrix servers. Controleer de namen van de VMM-servers en de netwerken op de juiste wijze ophalen


1. De laatste cmdlet maakt een toewijzing tussen de primaire netwerk en het herstel. De cmdlet geeft het primaire netwerk als het eerste element van $PrimaryNetworks en het herstel netwerk als het eerste element van $RecoveryNetworks.

        New-AzureRmSiteRecoveryNetworkMapping -PrimaryNetwork $PrimaryNetworks[0] -RecoveryNetwork $RecoveryNetworks[0]

## <a name="step-7-configure-storage-mapping"></a>Stap 7: De toewijzing van opslag configureren
1. De onderstaande opdracht de lijst met opslagclassificaties in $storageclassifications-variabele ophalen.

        $storageclassifications = Get-AzureRmSiteRecoveryStorageClassification
2. De volgende opdrachten Lees het classificatie in de variabele $SourceClassificaion bron en doel-indeling in $TargetClassification variabele.

        $SourceClassificaion = $storageclassifications[0]

        $TargetClassification = $storageclassifications[1]

    > [!NOTE]
    > De bron en doel-classificaties kunnen elk element in de matrix zijn. Raadpleeg de uitvoer van de onderstaande opdracht om te bepalen van de index van de bron en doel classificaties in $storageclassifications matrix.

    > Get-AzureRmSiteRecoveryStorageClassification | Select-Object - eigenschap FriendlyName, Id | Tabel opmaken


1. De onderstaande cmdlet een toewijzing gemaakt tussen de bron-indeling en de doel-classificatie.

        New-AzureRmSiteRecoveryStorageClassificationMapping -PrimaryStorageClassification $SourceClassificaion -RecoveryStorageClassification $TargetClassification

## <a name="step-8-enable-protection-for-virtual-machines"></a>Stap 8: Beveiliging voor virtuele machines inschakelen
Nadat de servers, clouds en netwerken correct zijn geconfigureerd, kunt u beveiliging voor virtuele machines in de cloud inschakelen.

1. Als beveiliging wilt inschakelen, voer de volgende opdracht om op te halen van de beveiligingscontainer:

          $PrimaryProtectionContainer = Get-AzureRmSiteRecoveryProtectionContainer -friendlyName $PrimaryCloudName
2. De entiteit beveiliging (VM) ophalen met de volgende opdracht:

           $protectionEntity = Get-AzureRmSiteRecoveryProtectionEntity -friendlyName $VMName -ProtectionContainer $PrimaryProtectionContainer
3. Replicatie voor de virtuele machine inschakelen met de volgende opdracht:

          $jobResult = Set-AzureRmSiteRecoveryProtectionEntity -ProtectionEntity $protectionentity -Protection Enable -Policy $policy

## <a name="test-your-deployment"></a>Uw implementatie testen
Als uw implementatie wilt testen kunt u een testfailover voor één virtuele machine, uitvoeren of een herstelplan dat bestaat uit meerdere virtuele machines maken en een testfailover voor het plan uitvoeren. Met een testfailover wordt uw failover- en herstelmechanisme in een geïsoleerd netwerk gesimuleerd.

> [!NOTE]
> U kunt een herstelplan maken voor uw toepassing in Azure-portal.
>
>

Volg de stappen in om te zien wanneer de bewerking is voltooid, [Monitoractiviteit](#monitor).

### <a name="run-a-test-failover"></a>Een testfailover uitvoeren
1. Voer de onderstaande cmdlets voor het ophalen van het VM-netwerk waarnaar u wilt testen van failover uw virtuele machines te.

       $Servers = Get-AzureRmSiteRecoveryServer
       $RecoveryNetworks = Get-AzureRmSiteRecoveryNetwork -Server $Servers[1]
2. Voer een testfailover uitgevoerd van een virtuele machine als volgt:

       $protectionEntity = Get-AzureRmSiteRecoveryProtectionEntity -FriendlyName $VMName -ProtectionContainer $PrimaryprotectionContainer

       $jobIDResult =  Start-AzureRmSiteRecoveryTestFailoverJob -Direction PrimaryToRecovery -ProtectionEntity $protectionEntity -VMNetwork $RecoveryNetworks[1]
3. Voer een testfailover uitgevoerd van een herstelplan als volgt:

       $recoveryplanname = "test-recovery-plan"

       $recoveryplan = Get-AzureRmSiteRecoveryRecoveryPlan -FriendlyName $recoveryplanname

       $jobIDResult =  Start-AzureRmSiteRecoveryTestFailoverJob -Direction PrimaryToRecovery -Recoveryplan $recoveryplan -VMNetwork $RecoveryNetworks[1]

### <a name="run-a-planned-failover"></a>Een geplande failover uitvoeren
1. Voer een geplande failover van een virtuele machine als volgt:

        $protectionEntity = Get-AzureRmSiteRecoveryProtectionEntity -Name $VMName -ProtectionContainer $PrimaryprotectionContainer

        $jobIDResult =  Start-AzureRmSiteRecoveryPlannedFailoverJob -Direction PrimaryToRecovery -ProtectionEntity $protectionEntity
2. Voer een geplande failover van een herstelplan als volgt:

        $recoveryplanname = "test-recovery-plan"

        $recoveryplan = Get-AzureRmSiteRecoveryRecoveryPlan -FriendlyName $recoveryplanname

        $jobIDResult =  Start-AzureRmSiteRecoveryPlannedFailoverJob -Direction PrimaryToRecovery -Recoveryplan $recoveryplan

### <a name="run-an-unplanned-failover"></a>Een niet-geplande failover uitvoeren
1. Voer een niet-geplande failover van een virtuele machine als volgt:

        $protectionEntity = Get-AzureRmSiteRecoveryProtectionEntity -Name $VMName -ProtectionContainer $PrimaryprotectionContainer

        $jobIDResult =  Start-AzureRmSiteRecoveryUnPlannedFailoverJob -Direction PrimaryToRecovery -ProtectionEntity $protectionEntity

2. Voer een niet-geplande failover van een herstelplan als volgt:

        $recoveryplanname = "test-recovery-plan"

        $recoveryplan = Get-AzureRmSiteRecoveryRecoveryPlan -FriendlyName $recoveryplanname

        $jobIDResult =  Start-AzureRmSiteRecoveryUnPlannedFailoverJob -Direction PrimaryToRecovery -ProtectionEntity $protectionEntity

## <a name=monitor></a>Voor Monitoractiviteit
Gebruik de volgende opdrachten voor het bewaken van de activiteit. Houd er rekening mee dat u wachten tussen taken voor de verwerking moet te voltooien.

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
[Lees meer](/powershell/module/azurerm.recoveryservices.backup/#recovery) over Azure Site Recovery met Azure Resource Manager PowerShell-cmdlets.
