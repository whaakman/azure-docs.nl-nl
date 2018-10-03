---
title: Replicatie van virtuele Hyper-V-machines met PowerShell en Azure Resource Manager | Microsoft Docs
description: Automatiseer de replicatie van Hyper-V-machines naar Azure met Azure Site Recovery met behulp van PowerShell en Azure Resource Manager.
services: site-recovery
author: sujayt
manager: rochakm
ms.service: site-recovery
ms.topic: article
ms.date: 07/06/2018
ms.author: sutalasi
ms.openlocfilehash: b71c381c3ebc2b36c36b862c00de02144f94bd0f
ms.sourcegitcommit: 3856c66eb17ef96dcf00880c746143213be3806a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/02/2018
ms.locfileid: "48043574"
---
# <a name="set-up-disaster-recovery-to-azure-for-hyper-v-vms-using-powershell-and-azure-resource-manager"></a>Instellen van herstel na noodgevallen naar Azure voor Hyper-V-machines met behulp van PowerShell en Azure Resource Manager

[Azure Site Recovery](site-recovery-overview.md) draagt bij aan uw zakelijke continuïteit en noodherstelplan (BCDR) door het coördineren van replicatie, failover en herstel van virtuele Azure-machines (VM's) en on-premises machines en fysieke servers.

In dit artikel wordt beschreven hoe u Windows PowerShell, samen met Azure Resource Manager gebruiken voor het Hyper-V-machines repliceren naar Azure. Het voorbeeld dat wordt gebruikt in dit artikel ziet u hoe u kunt een enkele virtuele machine die wordt uitgevoerd op een Hyper-V-host naar Azure repliceren.

## <a name="azure-powershell"></a>Azure PowerShell

Azure PowerShell biedt voor het beheren van Azure met behulp van Windows PowerShell-cmdlets. Site Recovery PowerShell-cmdlets beschikbaar zijn met Azure PowerShell voor Azure Resource Manager kunt u beveiligen en herstellen van uw servers in Azure.

U hoeft te worden van een deskundige PowerShell te gebruiken in dit artikel, maar u hoeft te begrijpen basisconcepten, zoals modules, cmdlets en sessies. Lezen [aan de slag met Windows PowerShell](http://technet.microsoft.com/library/hh857337.aspx), en [met behulp van Azure PowerShell met Azure Resource Manager](../powershell-azure-resource-manager.md).

> [!NOTE]
> Microsoft-partners in het programma Cloud Solution Provider (CSP) kunnen configureren en beheren van beveiliging van de klant servers hun respectieve CSP-abonnementen (tenant-abonnementen).
>
>

## <a name="before-you-start"></a>Voordat u begint
Zorg ervoor dat u deze vereisten hebt voldaan:

* Een [Microsoft Azure](https://azure.microsoft.com/) account. U kunt beginnen met een [gratis proefversie](https://azure.microsoft.com/pricing/free-trial/). Bovendien meer over [prijzen voor Azure Site Recovery Manager](https://azure.microsoft.com/pricing/details/site-recovery/).
* Azure PowerShell 1.0. Zie voor meer informatie over deze release en hoe u deze installeert [Azure PowerShell 1.0.](https://azure.microsoft.com/)
* De [AzureRM.SiteRecovery](https://www.powershellgallery.com/packages/AzureRM.SiteRecovery/) en [AzureRM.RecoveryServices](https://www.powershellgallery.com/packages/AzureRM.RecoveryServices/) modules. Krijgt u de nieuwste versies van deze modules uit de [PowerShell gallery](https://www.powershellgallery.com/)

Het specifieke voorbeeld in dit artikel beschreven heeft bovendien de volgende vereisten:

* Een Hyper-V-host met Windows Server 2012 R2 of Microsoft Hyper-V Server 2012 R2 met een of meer VM's. Hyper-V-servers moeten zijn verbonden met Internet, rechtstreeks of via een proxy.
* De virtuele machines die u wilt repliceren, moeten voldoen aan [deze vereisten](hyper-v-azure-support-matrix.md#replicated-vms).

## <a name="step-1-sign-in-to-your-azure-account"></a>Stap 1: Aanmelden bij uw Azure-account

1. Open een PowerShell-console en voer deze opdracht uit om aan te melden bij uw Azure-account. De cmdlet wordt een webpagina vraagt u om uw accountreferenties: **Connect-AzureRmAccount**.
    - U kunt ook uw accountreferenties opnemen als een parameter in de **Connect-AzureRmAccount** cmdlet, met behulp van de **-referentie** parameter.
    - Als u de CSP-partner werken namens een tenant bent, geeft u de klant als een tenant met behulp van de naam van de primaire domeincontroller tenant-id of tenant. Bijvoorbeeld: **Connect-AzureRmAccount-Tenant "fabrikam.com"**
2. Koppel het abonnement dat u gebruiken met het account wilt, omdat een account kan meerdere abonnementen hebben:

    `Select-AzureRmSubscription -SubscriptionName $SubscriptionName`

3. Controleer of uw abonnement is geregistreerd voor het gebruik van de Azure-providers voor de Recovery Services- en Site Recovery, met behulp van deze opdrachten:

    `Get-AzureRmResourceProvider -ProviderNamespace  Microsoft.RecoveryServices` `Get-AzureRmResourceProvider -ProviderNamespace  Microsoft.SiteRecovery`

4. Controleer in de uitvoer van de opdracht, de **RegistrationState** is ingesteld op **geregistreerde**, kunt u doorgaan met stap 2. Als dat niet het geval is, moet u de ontbrekende provider registreren in uw abonnement, door het uitvoeren van deze opdrachten:

    `Register-AzureRmResourceProvider -ProviderNamespace Microsoft.SiteRecovery` `Register-AzureRmResourceProvider -ProviderNamespace Microsoft.RecoveryServices`

5. Controleer of de Providers geregistreerd is, met de volgende opdrachten:

    `Get-AzureRmResourceProvider -ProviderNamespace  Microsoft.RecoveryServices` `Get-AzureRmResourceProvider -ProviderNamespace  Microsoft.SiteRecovery`.

## <a name="step-2-set-up-the-vault"></a>Stap 2: De kluis instellen

1. Maak een Azure Resource Manager-resourcegroep waarin u wilt maken van de kluis of gebruik een bestaande resourcegroep. Als volgt te werk om een nieuwe resourcegroep te maken. De variabele $ResourceGroupName bevat de naam van de resourcegroep die u wilt maken en de variabele $Geo bevat de Azure-regio waarin u wilt maken van de resourcegroep (bijvoorbeeld ' Brazilië-Zuid").

    `New-AzureRmResourceGroup -Name $ResourceGroupName -Location $Geo` 

2. Om op te halen een lijst met resourcegroepen in uw abonnement uitvoeren de **Get-AzureRmResourceGroup** cmdlet.
2. Maak een nieuwe Azure Recovery Services-kluis als volgt:

        $vault = New-AzureRmRecoveryServicesVault -Name <string> -ResourceGroupName <string> -Location <string>

    U kunt een lijst met bestaande kluizen met ophalen van de **Get-AzureRmRecoveryServicesVault** cmdlet.


## <a name="step-3-set-the-recovery-services-vault-context"></a>Stap 3: De context van de Recovery Services-kluis instellen

De context van de kluis als volgt instellen:

`Set-AzureRmSiteRecoveryVaultSettings -ARSVault $vault`

## <a name="step-4-create-a-hyper-v-site"></a>Stap 4: Een Hyper-V-site maken

1. Maak een nieuwe Hyper-V-site als volgt:

        $sitename = "MySite"                #Specify site friendly name
        New-AzureRmSiteRecoverySite -Name $sitename

2. Deze cmdlet wordt een Site Recovery-taak voor het maken van de site wordt gestart en retourneert een taakobject Site Recovery. Wachten op de taak is voltooid en controleer of dat de taak is voltooid.
3. Gebruik de **cmdlet Get-AzureRmSiteRecoveryJob**, voor het ophalen van het taakobject en controleer de huidige status van de taak.
4. Genereren en downloaden van een registratiesleutel voor de site, als volgt:

    ```
    $SiteIdentifier = Get-AzureRmSiteRecoverySite -Name $sitename | Select -ExpandProperty SiteIdentifier
        Get-AzureRmRecoveryServicesVaultSettingsFile -Vault $vault -SiteIdentifier $SiteIdentifier -SiteFriendlyName $sitename -Path $Path
    ```

5. De gedownloade sleutel kopiëren naar de Hyper-V-host. U moet de sleutel voor het registreren van de Hyper-V-host naar de site.

## <a name="step-5-install-the-provider-and-agent"></a>Stap 5: Installeer de Provider en agent

1. Download het installatieprogramma voor de meest recente versie van de Provider van [Microsoft](https://aka.ms/downloaddra).
2. Voer het installatieprogramma op theHyper-V-host.
3. Aan het einde van de installatie doorgaan naar de registratiestap.
4. Wanneer u hierom wordt gevraagd, de gedownloade sleutel opgeven en voltooi de registratie van de Hyper-V-host.
5. Controleer of dat de Hyper-V-host is geregistreerd bij de site als volgt:

        $server =  Get-AzureRmSiteRecoveryServer -FriendlyName $server-friendlyname

## <a name="step-6-create-a-replication-policy"></a>Stap 6: Een replicatiebeleid maken

Houd er rekening mee dat het opgegeven opslagaccount moet zich in dezelfde Azure-regio als de kluis en geo-replicatie is ingeschakeld moet hebben voordat u begint.

1. Als volgt te werk om een replicatiebeleid te maken:

        $ReplicationFrequencyInSeconds = "300";        #options are 30,300,900
        $PolicyName = “replicapolicy”
        $Recoverypoints = 6                    #specify the number of recovery points
        $storageaccountID = Get-AzureRmStorageAccount -Name "mystorea" -ResourceGroupName "MyRG" | Select -ExpandProperty Id

        $PolicyResult = New-AzureRmSiteRecoveryPolicy -Name $PolicyName -ReplicationProvider “HyperVReplicaAzure” -ReplicationFrequencyInSeconds $ReplicationFrequencyInSeconds  -RecoveryPoints $Recoverypoints -ApplicationConsistentSnapshotFrequencyInHours 1 -RecoveryAzureStorageAccountId $storageaccountID

2. Controleer de geretourneerde taak om ervoor te zorgen dat het beleid voor replicatie maken is gelukt.

3. Ophalen van de beveiligingscontainer die overeenkomt met de site, als volgt:

        $protectionContainer = Get-AzureRmSiteRecoveryProtectionContainer
3. Koppel de beveiligingscontainer aan het replicatiebeleid als volgt:

     $Policy = get-AzureRmSiteRecoveryPolicy - FriendlyName $PolicyName $associationJob = Start AzureRmSiteRecoveryPolicyAssociationJob-beleid $Policy - PrimaryProtectionContainer $protectionContainer

4. Wachten op de koppeling-taak is voltooid.

## <a name="step-7-enable-vm-protection"></a>Stap 7: VM-beveiliging inschakelen

1. De beveiliging-entiteit die overeenkomt met de virtuele machine die u beveiligen wilt, als volgt ophalen:

        $VMFriendlyName = "Fabrikam-app"                    #Name of the VM
        $protectionEntity = Get-AzureRmSiteRecoveryProtectionEntity -ProtectionContainer $protectionContainer -FriendlyName $VMFriendlyName
2. De VM te beveiligen. Als de virtuele machine die u beveiligt meer dan één schijf is gekoppeld heeft, geeft u de besturingssysteemschijf met behulp van de *OSDiskName* parameter.

        $Ostype = "Windows"                                 # "Windows" or "Linux"
        $DRjob = Set-AzureRmSiteRecoveryProtectionEntity -ProtectionEntity $protectionEntity -Policy $Policy -Protection Enable -RecoveryAzureStorageAccountId $storageaccountID  -OS $OStype -OSDiskName $protectionEntity.Disks[0].Name

3. Wachten op de virtuele machines tot een beveiligde status na de initiële replicatie. Dit kan duren, afhankelijk van factoren zoals de hoeveelheid gegevens worden gerepliceerd en de beschikbare bandbreedte van de upstream naar Azure. Wanneer een beveiligde status ingesteld is, worden de taakstatus en StateDescription als volgt bijgewerkt: 

        PS C:\> $DRjob = Get-AzureRmSiteRecoveryJob -Job $DRjob

        PS C:\> $DRjob | Select-Object -ExpandProperty State
        Succeeded

        PS C:\> $DRjob | Select-Object -ExpandProperty StateDescription
        Completed
4. Eigenschappen van de recovery-(zoals de VM-rolgrootte) en het Azure-netwerk waarmee de VM NIC koppelen na een failover.

        PS C:\> $nw1 = Get-AzureRmVirtualNetwork -Name "FailoverNw" -ResourceGroupName "MyRG"

        PS C:\> $VMFriendlyName = "Fabrikam-App"

        PS C:\> $VM = Get-AzureRmSiteRecoveryVM -ProtectionContainer $protectionContainer -FriendlyName $VMFriendlyName

        PS C:\> $UpdateJob = Set-AzureRmSiteRecoveryVM -VirtualMachine $VM -PrimaryNic $VM.NicDetailsList[0].NicId -RecoveryNetworkId $nw1.Id -RecoveryNicSubnetName $nw1.Subnets[0].Name

        PS C:\> $UpdateJob = Get-AzureRmSiteRecoveryJob -Job $UpdateJob

        PS C:\> $UpdateJob

        Name             : b8a647e0-2cb9-40d1-84c4-d0169919e2c5
        ID               : /Subscriptions/a731825f-4bf2-4f81-a611-c331b272206e/resourceGroups/MyRG/providers/Microsoft.RecoveryServices/vault
                           s/MyVault/replicationJobs/b8a647e0-2cb9-40d1-84c4-d0169919e2c5
        Type             : Microsoft.RecoveryServices/vaults/replicationJobs
        JobType          : UpdateVmProperties
        DisplayName      : Update the virtual machine
        ClientRequestId  : 805a22a3-be86-441c-9da8-f32685673112-2015-12-10 17:55:51Z-P
        State            : Succeeded
        StateDescription : Completed
        StartTime        : 10-12-2015 17:55:53 +00:00
        EndTime          : 10-12-2015 17:55:54 +00:00
        TargetObjectId   : 289682c6-c5e6-42dc-a1d2-5f9621f78ae6
        TargetObjectType : ProtectionEntity
        TargetObjectName : Fabrikam-App
        AllowedActions   : {Restart}
        Tasks            : {UpdateVmPropertiesTask}
        Errors           : {}



## <a name="step-8-run-a-test-failover"></a>Stap 8: Een testfailover uitvoeren
1. Voer een testfailover als volgt uit:

        $nw = Get-AzureRmVirtualNetwork -Name "TestFailoverNw" -ResourceGroupName "MyRG" #Specify Azure vnet name and resource group

        $protectionEntity = Get-AzureRmSiteRecoveryProtectionEntity -FriendlyName $VMFriendlyName -ProtectionContainer $protectionContainer

        $TFjob = Start-AzureRmSiteRecoveryTestFailoverJob -ProtectionEntity $protectionEntity -Direction PrimaryToRecovery -AzureVMNetworkId $nw.Id
2. Controleer of de test virtuele machine is gemaakt in Azure. De test-failover-taak is onderbroken na het maken van de test-VM in Azure.
3. Als u wilt opschonen en voltooi de testfailover, voert u de volgende uit:

        $TFjob = Resume-AzureRmSiteRecoveryJob -Job $TFjob

## <a name="next-steps"></a>Volgende stappen
[Meer informatie](https://docs.microsoft.com/powershell/module/azurerm.siterecovery) over Azure Site Recovery met Azure Resource Manager PowerShell-cmdlets.
