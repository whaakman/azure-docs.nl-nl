---
title: Replicatie van Hyper-V-machines met PowerShell en Azure Resource Manager | Microsoft Docs
description: De replicatie van Hyper-V-machines naar Azure met Azure Site Recovery met PowerShell en Azure Resource Manager automatiseren.
services: site-recovery
documentationcenter: 
author: bsiva
manager: abhiag
editor: 
ms.assetid: 05e0d76e-c3f5-4845-8052-094019b6d102
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 10/19/2017
ms.author: bsiva
ms.openlocfilehash: d93be3b958f85cd2892f92d84ed68996909a5d6b
ms.sourcegitcommit: 76a3cbac40337ce88f41f9c21a388e21bbd9c13f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/25/2017
---
# <a name="set-up-disaster-recovery-to-azure-for-hyper-v-vms-using-powershell-and-azure-resource-manager"></a>Herstel na noodgevallen naar Azure instellen voor Hyper-V-machines met behulp van PowerShell en Azure Resource Manager

[Azure Site Recovery](site-recovery-overview.md) bijdraagt aan uw zakelijke continuïteit en noodherstelplan (BCDR) door te organiseren replicatie, failovers en herstel van virtuele Azure-machines (VM's) en on-premises virtuele machines en fysieke servers.

In dit artikel wordt beschreven hoe u Windows PowerShell, samen met Azure Resource Manager, gebruikt Hyper-V-machines repliceren naar Azure. Het voorbeeld gebruikt in dit artikel leest u hoe een enkele virtuele machine worden uitgevoerd op een Hyper-V-host naar Azure repliceren.

## <a name="overview"></a>Overzicht

Azure PowerShell biedt voor het beheren van Azure met behulp van Windows PowerShell-cmdlets. Site Recovery PowerShell-cmdlets beschikbaar met Azure PowerShell voor Azure Resource Manager helpen u bij het beveiligen en herstellen van uw servers in Azure.

U hoeft niet te worden van een deskundige PowerShell te gebruiken in dit artikel, maar u hoeft te begrijpen basisconcepten, zoals modules, cmdlets en sessies. Lees [aan de slag met Windows PowerShell](http://technet.microsoft.com/library/hh857337.aspx), en [Azure PowerShell gebruiken met Azure Resource Manager](../powershell-azure-resource-manager.md).

> [!NOTE]
> Microsoft-partners in het programma Cloud Solution Provider (CSP) kunnen configureren en beheren van de beveiliging van servers van de klant aan hun respectieve CSP-abonnementen (tenant-abonnementen).
>
>

## <a name="before-you-start"></a>Voordat u begint
Zorg ervoor dat u deze vereisten hebt voldaan:

* Een [Microsoft Azure](https://azure.microsoft.com/) account. U kunt beginnen met een [gratis proefversie](https://azure.microsoft.com/pricing/free-trial/). U kunt bovendien lezen over [prijzen van Azure Site Recovery Manager](https://azure.microsoft.com/pricing/details/site-recovery/).
* Azure PowerShell 1.0. Zie voor meer informatie over deze release en hoe u deze installeert [Azure PowerShell 1.0.](https://azure.microsoft.com/)
* De [AzureRM.SiteRecovery](https://www.powershellgallery.com/packages/AzureRM.SiteRecovery/) en [AzureRM.RecoveryServices](https://www.powershellgallery.com/packages/AzureRM.RecoveryServices/) modules. U kunt de nieuwste versies van deze modules van de [PowerShell gallery](https://www.powershellgallery.com/)

Het specifieke voorbeeld dat wordt beschreven in dit artikel heeft bovendien de volgende vereisten:

* Een Hyper-V-host waarop Windows Server 2012 R2 of Microsoft Hyper-V Server 2012 R2 met een of meer virtuele machines. Hyper-V-servers moeten worden verbonden met Internet, rechtstreeks of via een proxy.
* De virtuele machines die u wilt repliceren, moeten voldoen aan [deze vereisten](site-recovery-support-matrix-to-azure.md#failed-over-azure-vm-requirements).

## <a name="step-1-sign-in-to-your-azure-account"></a>Stap 1: Meld u aan bij uw Azure-account

1. Open een PowerShell-console en voer deze opdracht aan te melden bij uw Azure-account. De cmdlet wordt een webpagina wordt u gevraagd uw accountreferenties: **Login-AzureRmAccount**.
    - Ook kunt u uw accountreferenties opnemen als een parameter in de **Login-AzureRmAccount** cmdlet, met behulp van de **-referentie** parameter.
    - Als u CSP partner werken namens een tenant de klant opgeven als een tenant met behulp van de naam van de primaire domeincontroller tenantID of tenant. Bijvoorbeeld: **Login-AzureRmAccount-Tenant 'fabrikam.com'**
2. Het abonnement dat u gebruiken met de acount wilt omdat een account kan meerdere abonnementen hebben koppelen:

    `Select-AzureRmSubscription -SubscriptionName $SubscriptionName`

3. Controleer of uw abonnement is geregistreerd voor het gebruik van de Azure-providers voor de Recovery Services- en Site is hersteld, met de volgende opdrachten:

    `Get-AzureRmResourceProvider -ProviderNamespace  Microsoft.RecoveryServices` `Get-AzureRmResourceProvider -ProviderNamespace  Microsoft.SiteRecovery`

4. Controleer in de opdrachtuitvoer de **RegistrationState** is ingesteld op **geregistreerde**, kunt u doorgaan met stap 2. Als dat niet het geval is, moet u de ontbrekende provider registreren in uw abonnement, door deze opdrachten uit te voeren:

    `Register-AzureRmResourceProvider -ProviderNamespace Microsoft.SiteRecovery` `Register-AzureRmResourceProvider -ProviderNamespace Microsoft.RecoveryServices`

5. Controleer of de Providers geregistreerd is, met de volgende opdrachten:

    `Get-AzureRmResourceProvider -ProviderNamespace  Microsoft.RecoveryServices` `Get-AzureRmResourceProvider -ProviderNamespace  Microsoft.SiteRecovery`.

## <a name="step-2-set-up-the-vault"></a>Stap 2: De kluis instellen

1. Maak een Azure Resource Manager-resourcegroep in te maken van de kluis of gebruik een bestaande resourcegroep. Als volgt te werk om een nieuwe resourcegroep te maken. De variabele $ResourceGroupName bevat de naam van de resourcegroep die u wilt maken, en de variabele $Geo bevat de Azure-regio waarin u wilt maken van de resourcegroep (bijvoorbeeld ' Brazilië-Zuid').

    `New-AzureRmResourceGroup -Name $ResourceGroupName -Location $Geo` 

2. Verkrijgen van een lijst met resourcegroepen in uw abonnement uitvoeren de **Get-AzureRmResourceGroup** cmdlet.
2. Maak een nieuwe Azure Recovery Services-kluis als volgt:

        $vault = New-AzureRmRecoveryServicesVault -Name <string> -ResourceGroupName <string> -Location <string>

    U kunt een lijst met bestaande kluizen met ophalen de **Get-AzureRmRecoveryServicesVault** cmdlet.


## <a name="step-3-set-the-recovery-services-vault-context"></a>Stap 3: Stel de context van Recovery Services-kluis

De context van de kluis als volgt instellen:

`Set-AzureRmSiteRecoveryVaultSettings -ARSVault $vault`

## <a name="step-4-create-a-hyper-v-site"></a>Stap 4: Een Hyper-V-site maken

1. Maak een nieuwe Hyper-V-site als volgt:

        $sitename = "MySite"                #Specify site friendly name
        New-AzureRmSiteRecoverySite -Name $sitename

2. Deze cmdlet wordt gestart van een Site Recovery-taak voor het maken van de site en retourneert een taakobject Site Recovery. Wacht u totdat de taak is voltooid en controleer of de taak is voltooid.
3. Gebruik de **cmdlet Get-AzureRmSiteRecoveryJob**, voor het ophalen van het taakobject en controleer de huidige status van de taak.
4. Genereren en download een registratiesleutel voor de site, als volgt:

    ```
    $SiteIdentifier = Get-AzureRmSiteRecoverySite -Name $sitename | Select -ExpandProperty SiteIdentifier
        Get-AzureRmRecoveryServicesVaultSettingsFile -Vault $vault -SiteIdentifier $SiteIdentifier -SiteFriendlyName $sitename -Path $Path
    ```

5. Kopieer het gedownloade sleutel met de Hyper-V-host. U moet de sleutel voor het registreren van de Hyper-V-host naar de site.

## <a name="step-5-install-the-provider-and-agent"></a>Stap 5: De Provider en de agent installeren

1. Het installatieprogramma downloaden voor de nieuwste versie van de Provider van de [Microsoft](https://aka.ms/downloaddra).
2. Het installatieprogramma uitvoeren op theHyper-V-host.
3. Blijven de registratiestap aan het einde van de installatie.
4. Wanneer u wordt gevraagd, de gedownloade sleutel opgeven en voltooien van de registratie van de Hyper-V-host.
5. Controleer of dat de Hyper-V-host is geregistreerd bij de site als volgt:

        $server =  Get-AzureRmSiteRecoveryServer -FriendlyName $server-friendlyname

## <a name="step-6-create-a-replication-policy"></a>Stap 6: Een beleid voor wachtwoordreplicatie maken

Voordat u begint, houd er rekening mee dat het opgegeven opslagaccount moet zich in dezelfde Azure-regio als de kluis en moet geo-replicatie is ingeschakeld.

1. Maak een beleid voor wachtwoordreplicatie als volgt:

        $ReplicationFrequencyInSeconds = "300";        #options are 30,300,900
        $PolicyName = “replicapolicy”
        $Recoverypoints = 6                    #specify the number of recovery points
        $storageaccountID = Get-AzureRmStorageAccount -Name "mystorea" -ResourceGroupName "MyRG" | Select -ExpandProperty Id

        $PolicyResult = New-AzureRmSiteRecoveryPolicy -Name $PolicyName -ReplicationProvider “HyperVReplicaAzure” -ReplicationFrequencyInSeconds $ReplicationFrequencyInSeconds  -RecoveryPoints $Recoverypoints -ApplicationConsistentSnapshotFrequencyInHours 1 -RecoveryAzureStorageAccountId $storageaccountID

2. Controleer de geretourneerde taak om ervoor te zorgen dat de replicatie beleid aanmaak slaagt.

3. Ophalen van de beveiligingscontainer die overeenkomt met de site, als volgt:

        $protectionContainer = Get-AzureRmSiteRecoveryProtectionContainer
3. De beveiligingscontainer als volgt aan het replicatiebeleid koppelen:

     $Policy = get-AzureRmSiteRecoveryPolicy - FriendlyName $PolicyName $associationJob = Start AzureRmSiteRecoveryPolicyAssociationJob-beleid $Policy - PrimaryProtectionContainer $protectionContainer

4. Wachten op de koppeling taak te voltooien.

## <a name="step-7-enable-vm-protection"></a>Stap 7: Schakel de beveiliging voor VM

1. Ophalen van de beveiliging-entiteit die overeenkomt met de virtuele machine die u beveiligen wilt, als volgt:

        $VMFriendlyName = "Fabrikam-app"                    #Name of the VM
        $protectionEntity = Get-AzureRmSiteRecoveryProtectionEntity -ProtectionContainer $protectionContainer -FriendlyName $VMFriendlyName
2. Beveiligen van de VM. Als de virtuele machine die u beveiligt meer dan één schijf zijn gekoppeld heeft, de besturingssysteemschijf opgeeft via de *OSDiskName* parameter.

        $Ostype = "Windows"                                 # "Windows" or "Linux"
        $DRjob = Set-AzureRmSiteRecoveryProtectionEntity -ProtectionEntity $protectionEntity -Policy $Policy -Protection Enable -RecoveryAzureStorageAccountId $storageaccountID  -OS $OStype -OSDiskName $protectionEntity.Disks[0].Name

3. Wacht tot de virtuele machines een beveiligde status bereiken nadat de initiële replicatie. Dit kan even duren, afhankelijk van factoren zoals de hoeveelheid gegevens worden gerepliceerd en de beschikbare bandbreedte van de upstream naar Azure. Wanneer een beveiligde status aanwezig is, worden de taakstatus en StateDescription als volgt bijgewerkt: 

        PS C:\> $DRjob = Get-AzureRmSiteRecoveryJob -Job $DRjob

        PS C:\> $DRjob | Select-Object -ExpandProperty State
        Succeeded

        PS C:\> $DRjob | Select-Object -ExpandProperty StateDescription
        Completed
4. Eigenschappen van de update-herstel (zoals de grootte VM-rol,) en het Azure-netwerk waarmee de VM-NIC koppelen na een failover.

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
1. Een testfailover als volgt uitvoeren:

        $nw = Get-AzureRmVirtualNetwork -Name "TestFailoverNw" -ResourceGroupName "MyRG" #Specify Azure vnet name and resource group

        $protectionEntity = Get-AzureRmSiteRecoveryProtectionEntity -FriendlyName $VMFriendlyName -ProtectionContainer $protectionContainer

        $TFjob = Start-AzureRmSiteRecoveryTestFailoverJob -ProtectionEntity $protectionEntity -Direction PrimaryToRecovery -AzureVMNetworkId $nw.Id
2. Controleer of de test VM is gemaakt in Azure. De test failover-taak is onderbroken na het maken van de virtuele testmachine in Azure.
3. Als u wilt opruimen en voltooi de testfailover, uitvoeren:

        $TFjob = Resume-AzureRmSiteRecoveryJob -Job $TFjob

## <a name="next-steps"></a>Volgende stappen
[Meer informatie](https://msdn.microsoft.com/library/azure/mt637930.aspx) over Azure Site Recovery met Azure Resource Manager PowerShell-cmdlets.
