---
title: Replicatie van Hyper-V virtuele machines in VMM-clouds met Azure Site Recovery en PowerShell (Resource Manager) | Microsoft Docs
description: Hyper-V virtuele machines in VMM-clouds met Azure Site Recovery en PowerShell repliceren
services: site-recovery
documentationcenter: 
author: Rajani-Janaki-Ram
manager: rochakm
editor: raynew
ms.assetid: 6ac509ad-5024-43d8-b621-d8fec019b9a9
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/15/2017
ms.author: rajanaki
ms.openlocfilehash: 28f35498593c896a5ad1fe7030c96c38bd4a48a9
ms.sourcegitcommit: 1d423a8954731b0f318240f2fa0262934ff04bd9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/05/2018
---
# <a name="replicate-hyper-v-virtual-machines-in-vmm-clouds-to-azure-using-powershell-and-azure-resource-manager"></a>Hyper-V virtuele machines in VMM-clouds repliceren naar Azure met behulp van PowerShell en Azure Resource Manager
> [!div class="op_single_selector"]
> * [Azure-portal](site-recovery-vmm-to-azure.md)
> * [PowerShell - Resource Manager](site-recovery-vmm-to-azure-powershell-resource-manager.md)
> * [PowerShell - Klassiek](site-recovery-deploy-with-powershell.md)
>
>

## <a name="overview"></a>Overzicht
Azure Site Recovery draagt bij aan uw strategie voor zakelijke continuïteit en noodherstel herstel (BCDR) door replicatie, failovers en herstel van virtuele machines in een aantal implementatiescenario's te organiseren. Zie voor een volledige lijst van de implementatie van scenario's de [Azure Site Recovery-overzicht](site-recovery-overview.md).

In dit artikel leest u hoe u PowerShell gebruikt voor het automatiseren van algemene taken die u uitvoeren moet bij het instellen van Azure Site Recovery Hyper-V virtuele machines in System Center VMM-clouds repliceren naar Azure-opslag.

Het artikel bevat vereisten voor het scenario en laat zien u

* Het instellen van een Recovery Services-kluis
* De Azure Site Recovery Provider installeren op de bron-VMM-server
* Registreer de server in de kluis, een Azure storage-account toevoegen
* Installeer de Azure Recovery Services-agent op Hyper-V-hostservers
* Configureer beveiligingsinstellingen voor VMM-clouds, die wordt toegepast op alle beveiligde virtuele machines
* Schakel de beveiliging voor deze virtuele machines.
* Test de failover om te controleren of dat alles werkt zoals verwacht.

Als u problemen bij het instellen van dit scenario, stel uw vragen op de [Azure Recovery Services-Forum](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).

> [!NOTE]
> Azure heeft twee verschillende implementatiemodellen voor het maken en werken met resources: [Resource Manager en Classic](../azure-resource-manager/resource-manager-deployment-model.md). In dit artikel bevat informatie over met behulp van de Resource Manager-implementatiemodel.
>
>

## <a name="before-you-start"></a>Voordat u begint
Zorg ervoor dat u deze vereisten hebt voldaan:

### <a name="azure-prerequisites"></a>Vereisten voor Azure
* U hebt een [Microsoft Azure](https://azure.microsoft.com/)-account nodig. Als u niet hebt, beginnen met een [gratis account](https://azure.microsoft.com/free). U kunt bovendien lezen over de [prijzen van Azure Site Recovery Manager](https://azure.microsoft.com/pricing/details/site-recovery/).
* Als u om de replicatie voor een scenario CSP-abonnement probeert, hebt u een CSP-abonnement nodig. Meer informatie over het programma CSP in [het registreren in het CSP-programma](https://msdn.microsoft.com/library/partnercenter/mt156995.aspx).
* U hebt een Azure-v2-opslag (Resource Manager)-account voor het opslaan van gegevens gerepliceerd naar Azure. Het account moet geo-replicatie is ingeschakeld. Het bestand moet zich in dezelfde regio bevinden als de Azure Site Recovery-service en worden gekoppeld aan hetzelfde abonnement of de CSP-abonnement. Zie voor meer informatie over het instellen van Azure storage, de [Inleiding tot Microsoft Azure Storage](../storage/common/storage-introduction.md) ter referentie.
* U moet ervoor zorgen dat virtuele machines die u wilt beveiligen, voldoen aan de [vereisten van de virtuele machine van Azure](site-recovery-support-matrix-to-azure.md#failed-over-azure-vm-requirements).

> [!NOTE]
> Er zijn momenteel alleen VM niveau bewerkingen mogelijk via Powershell. Ondersteuning voor niveau herstelbewerkingen plan zal binnenkort worden gesteld.  Nu bent u beperkt tot het uitvoeren van failovers van datacenters alleen op een granulatie 'beveiligde virtuele machine' en niet op een herstelplan-niveau.
>
>

### <a name="vmm-prerequisites"></a>VMM-vereisten
* U moet de VMM-server waarop System Center 2012 R2.
* De VMM-servers met virtuele machines die u wilt beveiligen, moet de Azure Site Recovery Provider worden uitgevoerd. Dit wordt geïnstalleerd tijdens de implementatie van de Azure Site Recovery.
* U moet ten minste één cloud op de VMM-beheerserver die u wilt beveiligen. De cloud moet bevatten:
  * Een of meer VMM-hostgroepen.
  * Een of meer Hyper-V-hostservers of -clusters in elke hostgroep.
  * Een of meer virtuele machines op de bronserver met Hyper-V.
* Meer informatie over het instellen van VMM-clouds:
  * Meer informatie over VMM-privéclouds in [What's New in een Privécloud met System Center 2012 R2 VMM](http://go.microsoft.com/fwlink/?LinkId=324952) en in [VMM 2012 en clouds](http://go.microsoft.com/fwlink/?LinkId=324956).
  * Meer informatie over [configureren van de VMM fabric cloud](https://msdn.microsoft.com/library/azure/dn469075.aspx#BKMK_Fabric)
  * Nadat uw cloud fabric-elementen voldaan zijn informatie over het maken van privéclouds in [maken van een privécloud in VMM](http://go.microsoft.com/fwlink/p/?LinkId=324953) en in [overzicht: privéclouds maken met System Center 2012 SP1 VMM](http://go.microsoft.com/fwlink/p/?LinkId=324954).

### <a name="hyper-v-prerequisites"></a>Hyper-V-vereisten
* De Hyper-V-hostservers moeten ten minste draaien **Windows Server 2012** met Hyper-V-rol of **Microsoft Hyper-V Server 2012** en de meest recente updates hebt geïnstalleerd.
* Als u Hyper-V in een cluster uitvoert, wordt die clusterbroker niet automatisch gemaakt als u een cluster op basis van een statisch IP-adres hebt. U moet de clusterbroker handmatig configureren. Voor
* Zie voor instructies [Hyper-V Replica Broker configureren hoe](http://blogs.technet.com/b/haroldwong/archive/2013/03/27/server-virtualization-series-hyper-v-replica-broker-explained-part-15-of-20-by-yung-chou.aspx).
* Alle Hyper-V-hostserver of het cluster waarvoor u wilt beheren protection moet worden opgenomen in een VMM-cloud.

### <a name="network-mapping-prerequisites"></a>Vereisten voor netwerktoewijzing
Als u virtuele machines in Azure worden de koppelingen van Netwerktoewijzingen VM-op de bron-VMM-server netwerken beveiligen en gericht op Azure-netwerken voor het inschakelen van de volgende:

* Alle machines die op hetzelfde netwerk failover kunnen verbinden met elkaar, ongeacht welk herstelplan ze zich in.
* Als een netwerkgateway is ingesteld in het doel-Azure-netwerk, kunnen virtuele machines worden verbonden met andere on-premises virtuele machines.
* Als u geen netwerktoewijzing configureert, mag alleen de virtuele machines met failover in hetzelfde herstelplan na failover naar Azure met elkaar verbinden.

Als u netwerktoewijzing wilt implementeren, hebt u het volgende nodig:

* De virtuele machines die u wilt beveiligen op de bron-VMM-server, moeten zijn verbonden met een VM-netwerk. Dit netwerk moet zijn gekoppeld aan een logisch netwerk dat is verbonden met de cloud.
* Een Azure-netwerk waarmee gerepliceerde virtuele machines verbinding na failover maken kunt. Dit netwerk selecteert u op het moment van failover. Het netwerk moet zich in dezelfde regio bevinden als uw Azure Site Recovery-abonnement.

Meer informatie over de netwerktoewijzing in

* [Het configureren van logische netwerken in VMM](http://go.microsoft.com/fwlink/p/?LinkId=386307)
* [Het VM-netwerken en gateways configureren in VMM](http://go.microsoft.com/fwlink/p/?LinkId=386308)
* [Het configureren en controleren van de virtuele netwerken in Azure](https://azure.microsoft.com/documentation/services/virtual-network/)

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
1. Een resourcegroep maken in Azure Resource Manager als u nog niet hebt

        New-AzureRmResourceGroup -Name #ResourceGroupName -Location #location
2. Maak een nieuwe Recovery Services-kluis en het gemaakte ASR kluis object opslaan in een variabele (later worden gebruikt). U kunt ook de ASR kluis post maken van het object met de cmdlet Get-AzureRMRecoveryServicesVault ophalen:-

        $vault = New-AzureRmRecoveryServicesVault -Name #vaultname -ResouceGroupName #ResourceGroupName -Location #location

## <a name="step-3-set-the-recovery-services-vault-context"></a>Stap 3: Stel de context van de Recovery Services-kluis

De context van de kluis ingesteld door de onderstaande opdracht.

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

## <a name="step-5-create-an-azure-storage-account"></a>Stap 5: Een Azure storage-account maken

Als u geen Azure storage-account hebt, een geo-replicatie ingeschakeld account maken in de dezelfde geo als de kluis met de volgende opdracht:

        $StorageAccountName = "teststorageacc1"    #StorageAccountname
        $StorageAccountGeo  = "Southeast Asia"     
        $ResourceGroupName =  “myRG”             #ResourceGroupName
        $RecoveryStorageAccount = New-AzureRmStorageAccount -ResourceGroupName $ResourceGroupName -Name $StorageAccountName -Type “StandardGRS” -Location $StorageAccountGeo

Houd er rekening mee dat het opslagaccount moet zich in dezelfde regio bevinden als de Azure Site Recovery-service en gekoppeld aan hetzelfde abonnement worden.

## <a name="step-6-install-the-azure-recovery-services-agent"></a>Stap 6: De Azure Recovery Services-Agent installeren
1. Download de Azure Recovery Services-agent op [http://aka.ms/latestmarsagent](http://aka.ms/latestmarsagent) en deze installeren op elke Hyper-V-hostserver die zich in de VMM-clouds die u wilt beveiligen.
2. Voer de volgende opdracht uit op alle VMM-hosts:

       marsagentinstaller.exe /q /nu

## <a name="step-7-configure-cloud-protection-settings"></a>Stap 7: Cloud configureren beveiligingsinstellingen
1. Maak een replicatiebeleid naar Azure met de volgende opdracht:

        $ReplicationFrequencyInSeconds = "300";        #options are 30,300,900
        $PolicyName = “replicapolicy”
        $Recoverypoints = 6                    #specify the number of recovery points

        $policryresult = New-AzureRmSiteRecoveryPolicy -Name $policyname -ReplicationProvider HyperVReplicaAzure -ReplicationFrequencyInSeconds $replicationfrequencyinseconds -RecoveryPoints $recoverypoints -ApplicationConsistentSnapshotFrequencyInHours 1 -RecoveryAzureStorageAccountId "/subscriptions/q1345667/resourceGroups/test/providers/Microsoft.Storage/storageAccounts/teststorageacc1"

1. Een beveiligingscontainer ophalen met de volgende opdrachten:

       $PrimaryCloud = "testcloud"
       $protectionContainer = Get-AzureRmSiteRecoveryProtectionContainer -friendlyName $PrimaryCloud;  
2. De details van het beleid aan een variabele met de taak die is gemaakt en houd de beschrijvende naam krijgen:

       $policy = Get-AzureRmSiteRecoveryPolicy -FriendlyName $policyname
3. De koppeling van de beveiligingscontainer beginnen met het replicatiebeleid:

       $associationJob  = Start-AzureRmSiteRecoveryPolicyAssociationJob -Policy     $Policy -PrimaryProtectionContainer $protectionContainer  
4. Nadat de taak is voltooid, voert u de volgende opdracht uit:

       $job = Get-AzureRmSiteRecoveryJob -Job $associationJob

       if($job -eq $null -or $job.StateDescription -ne "Completed")
       {
         $isJobLeftForProcessing = $true;
       }
5. Wanneer de verwerking van de taak is voltooid, kunt u de volgende opdracht uitvoeren:

       if($isJobLeftForProcessing)
       {
         Start-Sleep -Seconds 60
       }
       }While($isJobLeftForProcessing)

Volg de stappen in om te zien wanneer de bewerking is voltooid, [Monitoractiviteit](#monitor).

## <a name="step-8-configure-network-mapping"></a>Stap 8: Netwerktoewijzing configureren
Controleer voordat u met de netwerktoewijzing begint of de virtuele machines op de bron-VMM-server zijn verbonden met een VM-netwerk. Daarnaast maakt u een of meer virtuele netwerken in Azure.

Meer informatie over het maken van een virtueel netwerk met Azure Resource Manager en PowerShell in [een virtueel netwerk maken met een site-naar-site VPN-verbinding met Azure Resource Manager en PowerShell](../vpn-gateway/vpn-gateway-create-site-to-site-rm-powershell.md)

Houd er rekening mee dat meerdere netwerken voor virtuele Machine kunnen worden toegewezen aan één Azure-netwerk. Als het doelnetwerk meerdere subnetten heeft en een van deze subnetten heeft dezelfde naam als het subnet waarop de virtuele bronmachine zich bevindt, klikt u vervolgens de replica virtuele machine verbonden met dat Doelsubnet na failover. Als er geen Doelsubnet met een overeenkomende naam, wordt de virtuele machine verbonden met het eerste subnet in het netwerk.

1. De eerste opdracht haalt servers voor de huidige Azure Site Recovery-kluis. De opdracht slaat de Microsoft Azure Site Recovery-servers in de $Servers matrixvariabele.

        $Servers = Get-AzureRmSiteRecoveryServer
2. De tweede opdracht wordt de site recovery-netwerk opgehaald voor de eerste server in de matrix $Servers. De opdracht worden de netwerken in de variabele $Networks opgeslagen.

        $Networks = Get-AzureRmSiteRecoveryNetwork -Server $Servers[0]

1. De derde opdracht wordt virtuele Azure-netwerken en dat de waarde in de variabele $AzureVmNetworks opgehaald.

        $AzureVmNetworks =  Get-AzureRmVirtualNetwork
2. De laatste cmdlet maakt een toewijzing tussen het primaire netwerk en de virtuele machine van Azure-netwerk. De cmdlet geeft het primaire netwerk als het eerste element van $Networks. De cmdlet geeft een VM-netwerk als het eerste element van $AzureVmNetworks.

        New-AzureRmSiteRecoveryNetworkMapping -PrimaryNetwork $Networks[0] -AzureVMNetworkId $AzureVmNetworks[0]

## <a name="step-9-enable-protection-for-virtual-machines"></a>Stap 9: Beveiliging voor virtuele machines inschakelen
Nadat de servers, clouds en netwerken correct zijn geconfigureerd, kunt u beveiliging voor virtuele machines in de cloud inschakelen.

 Houd rekening met het volgende:

* Virtuele machines moeten voldoen aan de Azure-vereisten. Controleer deze in [vereisten en ondersteuning](site-recovery-support-matrix-to-azure.md#failed-over-azure-vm-requirements) in de planning guide.
* Als u wilt inschakelen, beveiliging, het besturingssysteem en het besturingssysteem moeten schijfeigenschappen worden ingesteld voor de virtuele machine. Wanneer u een virtuele machine in VMM maakt met een sjabloon voor een virtuele machine, kunt u de eigenschap instellen. U kunt deze eigenschappen ook voor bestaande virtuele machines instellen op de tabbladen **Algemeen** en **Hardwareconfiguratie** van de eigenschappen van de virtuele machines. Als u deze eigenschappen niet in VMM instelt, kunt u ze configureren in de Azure Site Recovery-portal.

1. Als beveiliging wilt inschakelen, voer de volgende opdracht om op te halen van de beveiligingscontainer:

          $ProtectionContainer = Get-AzureRmSiteRecoveryProtectionContainer -friendlyName $CloudName
2. De entiteit beveiliging (VM) ophalen met de volgende opdracht:

           $protectionEntity = Get-AzureRmSiteRecoveryProtectionEntity -friendlyName $VMName -ProtectionContainer $protectionContainer
3. De DR voor de virtuele machine inschakelen met de volgende opdracht:

          $jobResult = Set-AzureRmSiteRecoveryProtectionEntity -ProtectionEntity $protectionentity -Protection Enable –Force -Policy $policy -RecoveryAzureStorageAccountId  $storageID "/subscriptions/217653172865hcvkchgvd/resourceGroups/rajanirgps/providers/Microsoft.Storage/storageAccounts/teststorageacc1

## <a name="test-your-deployment"></a>Uw implementatie testen
Als uw implementatie wilt testen kunt u een test failover voor een enkele virtuele machine uitvoeren of een herstelplan dat bestaat uit meerdere virtuele machines maken en een test failover voor het plan uitvoeren. Failover van de test wordt uw failover- en herstelmechanisme in een geïsoleerd netwerk gesimuleerd. Houd rekening met het volgende:

* Als u wilt verbinding maken met de virtuele machine in Azure met extern bureaublad na de failover, moet u verbinding met extern bureaublad inschakelen op de virtuele machine voordat u de testfailover uitvoert.
* Na een failover gebruikt u een openbaar IP-adres verbinding maken met de virtuele machine in Azure met extern bureaublad. Als u dit wilt doen, zorgt u ervoor dat u geen domeinbeleid hebt waarmee het verbinden met een virtuele machine via een openbaar adres wordt verhinderd.

Volg de stappen in om te zien wanneer de bewerking is voltooid, [Monitoractiviteit](#monitor).

### <a name="run-a-test-failover"></a>Een testfailover uitvoeren
- Start de testfailover door het uitvoeren van de volgende opdracht:

       $protectionEntity = Get-AzureRmSiteRecoveryProtectionEntity -Name $VMName -ProtectionContainer $protectionContainer

       $jobIDResult =  Start-AzureRmSiteRecoveryTestFailoverJob -Direction PrimaryToRecovery -ProtectionEntity $protectionEntity -AzureVMNetworkId <string>  

### <a name="run-a-planned-failover"></a>Een geplande failover uitvoeren
- Start de geplande failover door het uitvoeren van de volgende opdracht:

        $protectionEntity = Get-AzureRmSiteRecoveryProtectionEntity -Name $VMName -ProtectionContainer $protectionContainer

        $jobIDResult =  Start-AzureRmSiteRecoveryPlannedFailoverJob -Direction PrimaryToRecovery -ProtectionEntity $protectionEntity -AzureVMNetworkId <string>  

### <a name="run-an-unplanned-failover"></a>Een niet-geplande failover uitvoeren
- Start de niet-geplande failover door het uitvoeren van de volgende opdracht:

        $protectionEntity = Get-AzureRmSiteRecoveryProtectionEntity -Name $VMName -ProtectionContainer $protectionContainer

        $jobIDResult =  Start-AzureRmSiteRecoveryUnPlannedFailoverJob -Direction PrimaryToRecovery -ProtectionEntity $protectionEntity -AzureVMNetworkId <string>  

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
