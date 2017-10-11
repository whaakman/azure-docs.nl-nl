---
title: Hyper-V-machines repliceren naar Azure in de klassieke portal met PowerShell | Microsoft Docs
description: De replicatie van Hyper-V virtuele machines in VMM-clouds met Site Recovery en PowerShell in de klassieke portal automatiseren
services: site-recovery
documentationcenter: 
author: bsiva
manager: abhiag
editor: tysonn
ms.assetid: 9011f567-e0b4-4306-951a-b30da19f5db6
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/31/2017
ms.author: bsiva
ms.openlocfilehash: 581daaaa5cc0cf8be782f834c6bdb3f27ee413fb
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/29/2017
---
# <a name="replicate-hyper-v-vms-to-azure-with-powershell-in-the-classic-portal"></a>Hyper-V-machines repliceren naar Azure met PowerShell in de klassieke portal
> [!div class="op_single_selector"]
> * [Azure Portal](site-recovery-vmm-to-azure.md)
> * [PowerShell - Resource Manager](site-recovery-vmm-to-azure-powershell-resource-manager.md)
> * [Klassieke portal](site-recovery-vmm-to-azure-classic.md)
> * [PowerShell - Klassiek](site-recovery-deploy-with-powershell.md)
>
>

## <a name="overview"></a>Overzicht
Azure Site Recovery draagt bij aan uw strategie voor zakelijke continuïteit en noodherstel herstel (BCDR) door replicatie, failovers en herstel van virtuele machines in een aantal implementatiescenario's te organiseren. Zie voor een volledige lijst van de implementatie van scenario's de [Azure Site Recovery-overzicht](site-recovery-overview.md).

In dit artikel leest u hoe u PowerShell gebruikt voor het automatiseren van algemene taken die u uitvoeren moet bij het instellen van Azure Site Recovery Hyper-V virtuele machines in System Center VMM-clouds repliceren naar Azure-opslag.

Het artikel bevat vereisten voor het scenario en ziet u het instellen van een Site Recovery-kluis, de Azure Site Recovery Provider installeren op de bron-VMM-server, de server in de kluis registreren, Azure storage-account toevoegen en installeren van de Azure Recovery Services-agent op Hyper-V-hostservers Configureer beveiligingsinstellingen voor VMM-clouds die wordt toegepast op alle beveiligde virtuele machines en schakel vervolgens de beveiliging voor deze virtuele machines. Test tenslotte de failover om te controleren of alles naar verwachting werkt.

Als u problemen bij het instellen van dit scenario, stel uw vragen op de [Azure Recovery Services-Forum](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).

> [!NOTE]
> Azure heeft twee verschillende implementatiemodellen voor het maken en werken met resources: [Resource Manager en Classic](../azure-resource-manager/resource-manager-deployment-model.md). In dit artikel bevat informatie over met behulp van het klassieke implementatiemodel.
>
>

## <a name="before-you-start"></a>Voordat u begint
Zorg ervoor dat u deze vereisten hebt voldaan:

### <a name="azure-prerequisites"></a>Vereisten voor Azure
* U hebt een [Microsoft Azure](https://azure.microsoft.com/)-account nodig. U kunt beginnen met een [gratis proefversie](https://azure.microsoft.com/pricing/free-trial/).
* U hebt een Azure Storage-account nodig om gerepliceerde gegevens op te slaan. Het account moet geo-replicatie is ingeschakeld. Het moet zich in dezelfde regio bevinden als de Azure Site Recovery-kluis en worden gekoppeld aan hetzelfde abonnement. [Meer informatie over Azure storage](../storage/common/storage-introduction.md).
* U moet ervoor zorgen dat virtuele machines die u wilt beveiligen, voldoen aan [vereisten van de virtuele machine van Azure](site-recovery-support-matrix-to-azure.md#failed-over-azure-vm-requirements).

### <a name="vmm-prerequisites"></a>VMM-vereisten
* U moet de VMM-server waarop System Center 2012 R2.
* U moet ten minste één cloud op de VMM-beheerserver die u wilt beveiligen. De cloud moet bevatten:
  * Een of meer VMM-hostgroepen.
  * Een of meer Hyper-V-hostservers of -clusters in elke hostgroep.
  * Een of meer virtuele machines op de bronserver met Hyper-V.

### <a name="hyper-v-prerequisites"></a>Hyper-V-vereisten
* De Hyper-V-hostservers moeten ten minste draaien **Windows Server 2012** met Hyper-V-rol of **Microsoft Hyper-V Server 2012** en de meest recente updates hebt geïnstalleerd.
* Als u Hyper-V in een cluster uitvoert, wordt die clusterbroker niet automatisch gemaakt als u een cluster op basis van een statisch IP-adres hebt. U moet de clusterbroker handmatig configureren. Om dit te doen, in Serverbeheer > Failover Cluster Manager verbinding met het cluster, klikt u op **functie configureren** en selecteer **Hyper-V Replica Broker** in de **rol selecteren** scherm van de wizard maximale beschikbaarheid.
* Alle Hyper-V-hostserver of het cluster waarvoor u wilt beheren protection moet worden opgenomen in een VMM-cloud.

### <a name="network-mapping-prerequisites"></a>Vereisten voor netwerktoewijzing
Wanneer u virtuele machines in Azure beveiligt, zorgen de koppelingen van netwerktoewijzingen tussen VM-netwerken op de bron-VMM-server en doel-Azure-netwerken voor het volgende:

* Alle machines die op hetzelfde netwerk failover kunnen verbinden met elkaar, ongeacht welk herstelplan ze zich in.
* Als een netwerkgateway is ingesteld in het doel-Azure-netwerk, kunnen virtuele machines worden verbonden met andere on-premises virtuele machines.
* Als u geen netwerktoewijzing configureert, kunnen alleen virtuele machines met failover in hetzelfde herstelplan met elkaar worden verbonden na failover naar Azure.

Als u netwerktoewijzing wilt implementeren, hebt u het volgende nodig:

* De virtuele machines die u wilt beveiligen op de bron-VMM-server, moeten zijn verbonden met een VM-netwerk. Dit netwerk moet zijn gekoppeld aan een logisch netwerk dat is gekoppeld aan de cloud.
* Een Azure-netwerk waarmee gerepliceerde virtuele machines verbinding kunnen maken na failover. U selecteert dit netwerk op het moment van failover. Het netwerk moet zich in dezelfde regio bevinden als uw Azure Site Recovery-abonnement.

### <a name="powershell-prerequisites"></a>PowerShell-vereisten
Zorg ervoor dat u hebt Azure PowerShell klaar voor gebruik. Als u al van PowerShell gebruikmaakt, moet u een upgrade uitvoeren naar versie 0.8.10 of hoger. Zie voor meer informatie over het instellen van PowerShell [installeren en configureren van Azure PowerShell](/powershell/azureps-cmdlets-docs). Zodra u hebt ingesteld en PowerShell geconfigureerd, kunt u alle van de beschikbare cmdlets voor de service weergeven [hier](/powershell/azure/overview).

Zie voor meer informatie over tips kunt u de cmdlets, zoals hoe parameterwaarden, in- en uitgangen doorgaans worden uitgevoerd in Azure PowerShell gebruiken [aan de slag met Azure-Cmdlets](/powershell/azure/get-started-azureps).

## <a name="step-1-set-the-subscription"></a>Stap 1: Stel het abonnement
Voer deze cmdlets in PowerShell:

```
$UserName = "<user@live.com>"
$Password = "<password>"
$AzureSubscriptionName = "prod_sub1"

$SecurePassword = ConvertTo-SecureString -AsPlainText $Password -Force
$Cred = New-Object System.Management.Automation.PSCredential -ArgumentList $UserName, $securePassword
Add-AzureAccount -Credential $Cred;
$AzureSubscription = Select-AzureSubscription -SubscriptionName $AzureSubscriptionName

```

De elementen in de 'haken"vervangen door uw specifieke gegevens.

## <a name="step-2-create-a-site-recovery-vault"></a>Stap 2: Een Site Recovery-kluis maken
De elementen in de 'haken"vervangen door uw specifieke gegevens in PowerShell en voer deze opdrachten:

```

$VaultName = "<testvault123>"
$VaultGeo  = "<Southeast Asia>"
$OutputPathForSettingsFile = "<c:\>"

```


```
New-AzureSiteRecoveryVault -Location $VaultGeo -Name $VaultName;
$vault = Get-AzureSiteRecoveryVault -Name $VaultName;

```

## <a name="step-3-generate-a-vault-registration-key"></a>Stap 3: Een kluisregistratiesleutel genereren
Genereer een registratiesleutel in de kluis. Nadat u Azure Site Recovery Provider hebt gedownload en op de VMM-server hebt geïnstalleerd, gebruikt u deze sleutel om de VMM-server in de kluis te registreren.

1. Het bestand van de instelling kluis ophalen en instellen van de context:

   ```

   $VaultName = "<testvault123>"
   $VaultGeo  = "<Southeast Asia>"
   $OutputPathForSettingsFile = "<c:\>"

   $VaultSetingsFile = Get-AzureSiteRecoveryVaultSettingsFile -Location $VaultGeo -Name $VaultName -Path $OutputPathForSettingsFile;

   ```
2. De context van de kluis instellen met de volgende opdrachten:

   ```

   $VaultSettingFilePath = $vaultSetingsFile.FilePath
   $VaultContext = Import-AzureSiteRecoveryVaultSettingsFile -Path $VaultSettingFilePath -ErrorAction Stop

   ```

## <a name="step-4-install-the-azure-site-recovery-provider"></a>Stap 4: De Azure Site Recovery Provider installeren
1. Op de VMM-machine, maak een map met de volgende opdracht:

   ```

   pushd C:\ASR\

   ```
2. Pak de bestanden met de gedownloade provider door de volgende opdracht uit te voeren

   ```

   AzureSiteRecoveryProvider.exe /x:. /q

   ```
3. Installeer de provider met de volgende opdrachten:

   ```

   .\SetupDr.exe /i

   ```

   ```

   $installationRegPath = "hklm:\software\Microsoft\Microsoft System Center Virtual Machine Manager Server\DRAdapter"
   do
   {
     $isNotInstalled = $true;
     if(Test-Path $installationRegPath)
     {
         $isNotInstalled = $false;
     }
   }While($isNotInstalled)

   ```

   Wacht totdat de installatie is voltooid.
4. Registreer de server in de kluis die met de volgende opdracht:

   ```

   $BinPath = $env:SystemDrive+"\Program Files\Microsoft System Center 2012 R2\Virtual Machine Manager\bin"
   pushd $BinPath
   $encryptionFilePath = "C:\temp\"
   .\DRConfigurator.exe /r /Credentials $VaultSettingFilePath /vmmfriendlyname $env:COMPUTERNAME /dataencryptionenabled $encryptionFilePath /startvmmservice

   ```

## <a name="step-5-create-an-azure-storage-account"></a>Stap 5: Een Azure storage-account maken
Als u geen Azure storage-account hebt, kunt u een geo-replicatie ingeschakeld-account maken met de volgende opdracht:

```

$StorageAccountName = "teststorageacc1"
$StorageAccountGeo  = "Southeast Asia"

New-AzureStorageAccount -StorageAccountName $StorageAccountName -Label $StorageAccountName -Location $StorageAccountGeo;

```

Houd er rekening mee dat het opslagaccount moet zich in dezelfde regio bevinden als de Azure Site Recovery-service en gekoppeld aan hetzelfde abonnement worden.

## <a name="step-6-install-the-azure-recovery-services-agent"></a>Stap 6: De Azure Recovery Services-Agent installeren
Vanuit de Azure-portal door de Azure Recovery Services-agent te installeren op elke Hyper-V-hostserver die zich in de VMM-clouds die u wilt beveiligen.

Voer de volgende opdracht uit op alle VMM-hosts:

```

marsagentinstaller.exe /q /nu

```


## <a name="step-7-configure-cloud-protection-settings"></a>Stap 7: Cloud configureren beveiligingsinstellingen
1. Een profiel van de beveiliging cloud naar Azure maken met de volgende opdracht:

   ```

   $ReplicationFrequencyInSeconds = "300";
   $ProfileResult = New-AzureSiteRecoveryProtectionProfileObject -ReplicationProvider HyperVReplica -RecoveryAzureSubscription $AzureSubscriptionName -RecoveryAzureStorageAccount $StorageAccountName -ReplicationFrequencyInSeconds     $ReplicationFrequencyInSeconds;

   ```
2. Een beveiligingscontainer ophalen met de volgende opdrachten:

   ```

   $PrimaryCloud = "testcloud"
   $protectionContainer = Get-AzureSiteRecoveryProtectionContainer -Name $PrimaryCloud;

   ```
3. De koppeling van de beveiligingscontainer beginnen met de cloud:

   ```

   $associationJob = Start-AzureSiteRecoveryProtectionProfileAssociationJob -ProtectionProfile $profileResult -PrimaryProtectionContainer $protectionContainer;        

   ```
4. Nadat de taak is voltooid, voert u de volgende opdracht uit:

        $job = Get-AzureSiteRecoveryJob -Id $associationJob.JobId;
        if($job -eq $null -or $job.StateDescription -ne "Completed")
        {
        $isJobLeftForProcessing = $true;
        }


1. Wanneer de verwerking van de taak is voltooid, kunt u de volgende opdracht uitvoeren:

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



Volg de stappen in om te zien wanneer de bewerking is voltooid, [Monitoractiviteit](#monitor).

## <a name="step-8-configure-network-mapping"></a>Stap 8: Netwerktoewijzing configureren
Controleer voordat u met de netwerktoewijzing begint of de virtuele machines op de bron-VMM-server zijn verbonden met een VM-netwerk. Daarnaast maakt u een of meer virtuele netwerken in Azure. Er kunnen meerdere VM-netwerken worden toegewezen aan één Azure-netwerk.

Als het doelnetwerk meerdere subnetten heeft en een van deze subnetten dezelfde naam heeft als het subnet waarin de virtuele bronmachine zich bevindt, wordt de gerepliceerde virtuele machine na een failover verbonden met dat doelsubnet. Als er geen een Doelsubnet met een overeenkomende naam beschikbaar is, wordt de virtuele machine verbonden met het eerste subnet in het netwerk.

De eerste opdracht haalt servers voor de huidige Azure Site Recovery-kluis. De opdracht slaat de Microsoft Azure Site Recovery-servers in de $Servers matrixvariabele.

    $Servers = Get-AzureSiteRecoveryServer


De tweede opdracht wordt de site recovery-netwerk opgehaald voor de eerste server in de matrix $Servers. De opdracht worden de netwerken in de variabele $Networks opgeslagen.

    $Networks = Get-AzureSiteRecoveryNetwork -Server $Servers[0]

De derde opdracht uw Azure-abonnementen opgehaald met de cmdlet Get-AzureSubscription en slaat vervolgens die waarde in de variabele $Subscriptions.

    $Subscriptions = Get-AzureSubscription



De vierde opdracht haalt virtuele netwerken in Azure met behulp van de cmdlet Get-AzureVNetSite en dat de waarde in de variabele $AzureVmNetworks.

    $AzureVmNetworks = Get-AzureVNetSite



De laatste cmdlet maakt een toewijzing tussen het primaire netwerk en de virtuele machine van Azure-netwerk. De cmdlet geeft het primaire netwerk als het eerste element van $Networks. De cmdlet geeft een VM-netwerk als het eerste element van $AzureVmNetworks met behulp van de-ID. De opdracht bevat uw Azure-abonnement-ID.

    New-AzureSiteRecoveryNetworkMapping -PrimaryNetwork $Networks[0] -AzureSubscriptionId $Subscriptions[0].SubscriptionId -AzureVMNetworkId $AzureVmNetworks[0].Id


## <a name="step-9-enable-protection-for-virtual-machines"></a>Stap 9: Beveiliging voor virtuele machines inschakelen
Wanneer de servers, clouds en netwerken correct zijn geconfigureerd, kunt u beveiliging voor virtuele machines in de cloud inschakelen. Houd rekening met het volgende:

Virtuele machines moeten voldoen aan [vereisten van de virtuele machine van Azure](site-recovery-support-matrix-to-azure.md#failed-over-azure-vm-requirements).

Als u beveiliging wilt inschakelen, moeten het besturingssysteem en de schijfeigenschappen van het besturingssysteem zijn ingesteld voor de virtuele machine. Wanneer u een virtuele machine in VMM maakt met een sjabloon voor een virtuele machine, kunt u de eigenschap instellen. U kunt deze eigenschappen ook voor bestaande virtuele machines instellen op de tabbladen **Algemeen** en **Hardwareconfiguratie** van de eigenschappen van de virtuele machines. Als u deze eigenschappen niet in VMM instelt, kunt u ze configureren in de Azure Site Recovery-portal.

1. Als beveiliging wilt inschakelen, voer de volgende opdracht om op te halen van de beveiligingscontainer:

     $ProtectionContainer = get-AzureSiteRecoveryProtectionContainer-$CloudName naam
2. De entiteit beveiliging (VM) ophalen met de volgende opdracht:

        $protectionEntity = Get-AzureSiteRecoveryProtectionEntity -Name $VMName -ProtectionContainer $protectionContainer



1. De DR voor de virtuele machine inschakelen met de volgende opdracht:

        $jobResult = Set-AzureSiteRecoveryProtectionEntity -ProtectionEntity $protectionEntity     -Protection Enable -Force



## <a name="test-your-deployment"></a>Uw implementatie testen
Als uw implementatie wilt testen kunt u een testfailover voor één virtuele machine, uitvoeren of een herstelplan dat bestaat uit meerdere virtuele machines maken en een testfailover voor het plan uitvoeren. Met een testfailover wordt uw failover- en herstelmechanisme in een geïsoleerd netwerk gesimuleerd. Houd rekening met het volgende:

* Als u na de failover verbinding wilt maken met de virtuele machine in Azure met Extern bureaublad, schakelt u Verbinding met extern bureaublad in op de virtuele machine voordat u de testfailover uitvoert.
* Na een failover gebruikt u een openbaar IP-adres verbinding maken met de virtuele machine in Azure met extern bureaublad. Als u dit wilt doen, zorgt u ervoor dat u geen domeinbeleid hebt waarmee het verbinden met een virtuele machine via een openbaar adres wordt verhinderd.

Volg de stappen in om te zien wanneer de bewerking is voltooid, [Monitoractiviteit](#monitor).

### <a name="create-a-recovery-plan"></a>Een herstelplan maken
1. Maak een XML-bestand als een sjabloon voor het herstelplan met behulp van de onderstaande gegevens en vervolgens opslaan als 'C:\RPTemplatePath.xml'.
2. Wijzig de RecoveryPlan knooppunt-Id, naam, PrimaryServerId en SecondaryServerId.
3. Wijzigen van het knooppunt ProtectionEntity PrimaryProtectionEntityId (vmid uit VMM).
4. U kunt meer virtuele machines toevoegen door meer ProtectionEntity knooppunten toe te voegen.

        <#
        <?xml version="1.0" encoding="utf-16"?>
        <RecoveryPlan Id="d0323b26-5be2-471b-addc-0a8742796610" Name="rp-test"     PrimaryServerId="9350a530-d5af-435b-9f2b-b941b5d9fcd5"     SecondaryServerId="21a9403c-6ec1-44f2-b744-b4e50b792387" Description=""     Version="V2014_07">
          <Actions />
          <ActionGroups>
            <ShutdownAllActionGroup Id="ShutdownAllActionGroup">
              <PreActionSequence />
              <PostActionSequence />
            </ShutdownAllActionGroup>
            <FailoverAllActionGroup Id="FailoverAllActionGroup">
              <PreActionSequence />
              <PostActionSequence />
            </FailoverAllActionGroup>
            <BootActionGroup Id="DefaultActionGroup">
              <PreActionSequence />
              <PostActionSequence />
              <ProtectionEntity PrimaryProtectionEntityId="d4c8ce92-a613-4c63-9b03-    cf163cc36ef8" />
            </BootActionGroup>
          </ActionGroups>
          <ActionGroupSequence>
            <ActionGroup Id="ShutdownAllActionGroup" ActionId="ShutdownAllActionGroup"     Before="FailoverAllActionGroup" />
            <ActionGroup Id="FailoverAllActionGroup" ActionId="FailoverAllActionGroup"     After="ShutdownAllActionGroup" Before="DefaultActionGroup" />
            <ActionGroup Id="DefaultActionGroup" ActionId="DefaultActionGroup" After="FailoverAllActionGroup"/>
          </ActionGroupSequence>
        </RecoveryPlan>
        #>



1. Vul in de gegevens in de sjabloon:

        $TemplatePath = "C:\RPTemplatePath.xml";



1. De RecoveryPlan maken:

        $RPCreationJob = New-AzureSiteRecoveryRecoveryPlan -File $TemplatePath -WaitForCompletion;

### <a name="run-a-test-failover"></a>Een testfailover uitvoeren
1. Het object RecoveryPlan ophalen met de volgende opdracht:

     $RPObject = get-AzureSiteRecoveryRecoveryPlan-naam $RPName;
2. Start de testfailover door het uitvoeren van de volgende opdracht:

        $jobIDResult = Start-AzureSiteRecoveryTestFailoverJob -RecoveryPlan $RPObject -Direction PrimaryToRecovery;


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
[Lees meer](/powershell/azure/overview) over Azure Site Recovery PowerShell-cmdlets. </a>.
