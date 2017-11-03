---
title: Verwijderen van servers en schakel de beveiliging | Microsoft Docs
description: Dit artikel wordt beschreven hoe u servers vanaf een Site Recovery-kluis registratie en beveiliging voor virtuele machines en fysieke servers uit te schakelen.
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: cfreeman
editor: 
ms.assetid: ef1f31d5-285b-4a0f-89b5-0123cd422d80
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 10/3/2017
ms.author: raynew
ms.openlocfilehash: 471d68742668e2b1b1c72579cee9dd493f1bd042
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="remove-servers-and-disable-protection"></a>Servers verwijderen en beveiliging uitschakelen
Dit artikel wordt beschreven hoe registratie servers vanaf een Recovery Services-kluis en hoe de beveiliging voor computers die zijn beveiligd door Site Recovery uit te schakelen.


## <a name="unregister-a--configuration-server"></a>Hef de registratie van een configuratieserver

Als u virtuele VMware-machines of fysieke Windows of Linux-servers naar Azure repliceren, kunt u de registratie van een niet-verbonden configuratieserver uit een kluis als volgt ongedaan maken:

1. [Schakel de beveiliging van virtuele machines](#disable-protection-for-a-vmware-vm-or-physical-server-vmware-to-azure).
2. [Loskoppelen](site-recovery-setup-replication-settings-vmware.md#dissociate-a-configuration-server-from-a-replication-policy) en [verwijderen](site-recovery-setup-replication-settings-vmware.md#delete-a-replication-policy) alle beleidsregels voor replicatie
3. [De configuratieserver verwijderen](site-recovery-vmware-to-azure-manage-configuration-server.md#delete-or-unregister-a-configuration-server)

## <a name="unregister-a-vmm-server"></a>Hef de registratie van een VMM-server

1. Stoppen met het repliceren van virtuele machines in clouds op de VMM-server die u wilt verwijderen.
2. Verwijder eventuele Netwerktoewijzingen gebruikt door clouds op de VMM-server die u wilt verwijderen. In **Site Recovery-infrastructuur** > **voor System Center VMM** > **netwerktoewijzing**, met de rechtermuisknop op de netwerktoewijzing > **verwijderen**.
3. Noteer de ID van de VMM-server.
4. Loskoppelen van beleidsregels voor replicatie van clouds op de VMM-server die u wilt verwijderen.  In **Site Recovery-infrastructuur** > **voor System Center VMM** >  **replicatiebeleid**, dubbelklik op het bijbehorende beleid. Met de rechtermuisknop op de cloud > **koppeling verbreken**.
5. Verwijder de VMM-server of het actieve knooppunt. In **Site Recovery-infrastructuur** > **voor System Center VMM** > **VMM-Servers**, met de rechtermuisknop op de server > **verwijderen**.
6. Als de VMM-server verbroken is, downloadt en voert u de [script voor opschoning](http://aka.ms/asr-cleanup-script-vmm) op de VMM-server. Open PowerShell met de **als Administrator uitvoeren** optie, het uitvoeringsbeleid voor het standaardbereik (LocalMachine) wijzigen. Geef in het script wordt de ID van de VMM-beheerserver die u wilt verwijderen. Het script wordt verwijderd voor registratie en cloud koppelen van gegevens van de server.
5. Het script voor opschoning uitvoeren op elke secundaire VMM-server.
6. Het script voor opschoning uitvoeren op een andere passieve VMM-clusterknooppunten die de Provider geïnstalleerd hebben.
7. De Provider op de VMM-server handmatig verwijderen. Als u een cluster hebt, verwijderen van alle knooppunten.
8. Als uw virtuele machines zijn naar Azure repliceer, moet u de Microsoft Recovery Services-agent verwijderen van Hyper-V-hosts in de verwijderde clouds.

## <a name="unregister-a-hyper-v-host-in-a-hyper-v-site"></a>Hef de registratie van een Hyper-V-host in een Hyper-V-Site

Hyper-V-hosts die niet worden beheerd door VMM worden verzameld in een Hyper-V-site. Verwijder een host in een Hyper-V-site als volgt:

1. Schakel replicatie voor Hyper-V virtuele machines zich bevinden op de host uit.
2. Maak beleid voor de Hyper-V-site. In **Site Recovery-infrastructuur** > **voor Hyper-V-Sites** >  **replicatiebeleid**, dubbelklik op het bijbehorende beleid. Met de rechtermuisknop op de site > **koppeling verbreken**.
3. Verwijder de Hyper-V-hosts. In **Site Recovery-infrastructuur** > **voor Hyper-V-Sites** > **Hyper-V-Hosts**, met de rechtermuisknop op de server > **verwijderen** .
4. De Hyper-V-site verwijderen nadat alle hosts uit deze zijn verwijderd. In **Site Recovery-infrastructuur** > **voor Hyper-V-Sites** > **Hyper-V-Sites**, met de rechtermuisknop op de site > **verwijderen** .
5. Als de Hyper-V-host zich in een **verbroken** status en vervolgens op het volgende script uitvoeren op elke Hyper-V-host die u hebt verwijderd. Het script ruimt instellingen op de server en het deregistreren bij de kluis.



        pushd .
        try
        {
             $windowsIdentity=[System.Security.Principal.WindowsIdentity]::GetCurrent()
             $principal=new-object System.Security.Principal.WindowsPrincipal($windowsIdentity)
             $administrators=[System.Security.Principal.WindowsBuiltInRole]::Administrator
             $isAdmin=$principal.IsInRole($administrators)
             if (!$isAdmin)
             {
                "Please run the script as an administrator in elevated mode."
                $choice = Read-Host
                return;       
             }

            $error.Clear()    
            "This script will remove the old Azure Site Recovery Provider related properties. Do you want to continue (Y/N) ?"
            $choice =  Read-Host

            if (!($choice -eq 'Y' -or $choice -eq 'y'))
            {
            "Stopping cleanup."
            return;
            }

            $serviceName = "dra"
            $service = Get-Service -Name $serviceName
            if ($service.Status -eq "Running")
            {
                "Stopping the Azure Site Recovery service..."
                net stop $serviceName
            }

            $asrHivePath = "HKLM:\SOFTWARE\Microsoft\Azure Site Recovery"
            $registrationPath = $asrHivePath + '\Registration'
            $proxySettingsPath = $asrHivePath + '\ProxySettings'
            $draIdvalue = 'DraID'

            if (Test-Path $asrHivePath)
            {
                if (Test-Path $registrationPath)
                {
                    "Removing registration related registry keys."    
                    Remove-Item -Recurse -Path $registrationPath
                }

                if (Test-Path $proxySettingsPath)
            {
                    "Removing proxy settings"
                    Remove-Item -Recurse -Path $proxySettingsPath
                }

                $regNode = Get-ItemProperty -Path $asrHivePath
                if($regNode.DraID -ne $null)
                {            
                    "Removing DraId"
                    Remove-ItemProperty -Path $asrHivePath -Name $draIdValue
                }
                "Registry keys removed."
            }

            # First retrive all the certificates to be deleted
            $ASRcerts = Get-ChildItem -Path cert:\localmachine\my | where-object {$_.friendlyname.startswith('ASR_SRSAUTH_CERT_KEY_CONTAINER') -or $_.friendlyname.startswith('ASR_HYPER_V_HOST_CERT_KEY_CONTAINER')}
            # Open a cert store object
            $store = New-Object System.Security.Cryptography.X509Certificates.X509Store("My","LocalMachine")
            $store.Open('ReadWrite')
            # Delete the certs
            "Removing all related certificates"
            foreach ($cert in $ASRcerts)
            {
                $store.Remove($cert)
            }
        }catch
        {    
            [system.exception]
            Write-Host "Error occured" -ForegroundColor "Red"
            $error[0]
            Write-Host "FAILED" -ForegroundColor "Red"
        }
        popd



## <a name="disable-protection-for-a-vmware-vm-or-physical-server-vmware-to-azure"></a>Schakel de beveiliging voor een VMware-VM of de fysieke server (VMware naar Azure)

1. In **beveiligde Items** > **gerepliceerde Items**, met de rechtermuisknop op de machine > **schakelt u replicatie uit**.
2. In **schakelt u replicatie uit** pagina, selecteert u een van de volgende opties:
    - **Schakel replicatie en verwijderen (aanbevolen)** : deze optie het gerepliceerde item verwijderen uit Azure Site Recovery en de replicatie voor de machine is gestopt. Configuratie van de replicatie op de configuratieserver wordt opgeschoond en facturering van Site Recovery voor deze beveiligde server is gestopt.
    - **Verwijder** -deze optie moet alleen worden gebruikt als de bronomgeving wordt verwijderd of niet toegankelijk (niet verbonden). Hiermee verwijdert u het gerepliceerde item uit Azure Site Recovery (facturering is gestopt). Configuratie van de replicatie op de configuratieserver **niet** worden opgeschoond. 

> [!NOTE]
> In beide de mobility-service wordt niet verwijderd van de beveiligde servers opties, moet u handmatig verwijderen. Als u van plan bent om de server opnieuw met dezelfde configuratieserver te beveiligen, kunt u doorgaan met het verwijderen van de mobility-service.

## <a name="disable-protection-for-a-hyper-v-virtual-machine-hyper-v-to-azure"></a>Schakel de beveiliging voor een Hyper-V virtuele machine (Hyper-V naar Azure)

> [!NOTE]
> Gebruik deze procedure als u Hyper-V-machines naar Azure zonder een VMM-server repliceert. Als u uw virtuele machines met repliceert de **System Center VMM naar Azure** scenario en volg de instructies [Schakel de beveiliging voor een Hyper-V virtuele machine Bezig met repliceren met behulp van de System Center VMM voor Azure-scenario](#disable-protection-for-a-hyper-v-virtual-machine-replicating-using-the-system-centet-vmm-to-azure-scenario)

1. In **beveiligde Items** > **gerepliceerde Items**, met de rechtermuisknop op de machine > **schakelt u replicatie uit**.
2. In **schakelt u replicatie uit**, kunt u de volgende opties selecteren:
     - **Schakel replicatie en verwijderen (aanbevolen)** : deze optie het gerepliceerde item verwijderen uit Azure Site Recovery en de replicatie voor de machine is gestopt. Configuratie van de replicatie op de lokale virtuele machine opgeschoond en facturering van Site Recovery voor deze beveiligde server is gestopt.
    - **Verwijder** -deze optie moet alleen worden gebruikt als de bronomgeving wordt verwijderd of niet toegankelijk (niet verbonden). Hiermee verwijdert u het gerepliceerde item uit Azure Site Recovery (facturering is gestopt). Configuratie van de replicatie op de lokale virtuele machine **niet** worden opgeschoond. 

    > [!NOTE]
    > Als u ervoor kiest de **verwijderen** optie voert u de volgende set van scripts voor het opschonen van de replicatie-instellingen op de lokale Hyper-V Server.
1. Op de bron-Hyper-V-hostserver, replicatie voor de virtuele machine verwijderen. SQLVM1 vervangen door de naam van uw virtuele machine en voer het script uit een administratieve PowerShell


    
    $vmName = 'SQLVM1' $vm = Get-WmiObject - Namespace 'root\virtualization\v2'-Query ' Selecteer * van Msvm_ComputerSystem waar ElementName = '$vmName' ' $replicationService 'root\virtualization\v2' Get-WmiObject - Namespace =-Query "Selecteer * van Msvm_ ReplicationService' $replicationService.RemoveReplicationRelationship($vm.__PATH)
    

## <a name="disable-protection-for-a-hyper-v-virtual-machine-replicating-to-azure-using-the-system-center-vmm-to-azure-scenario"></a>Schakel de beveiliging voor een Hyper-V virtuele machine repliceren naar Azure met behulp van de System Center VMM voor Azure scenario

1. In **beveiligde Items** > **gerepliceerde Items**, met de rechtermuisknop op de machine > **schakelt u replicatie uit**.
2. In **schakelt u replicatie uit**, selecteer een van de volgende opties:

    - **Schakel replicatie en verwijderen (aanbevolen)** : deze optie het gerepliceerde item verwijderen uit Azure Site Recovery en de replicatie voor de machine is gestopt. Configuratie van de replicatie op de lokale virtuele machine wordt opgeschoond en facturering van Site Recovery voor deze beveiligde server is gestopt.
    - **Verwijder** -deze optie moet alleen worden gebruikt als de bronomgeving wordt verwijderd of niet toegankelijk (niet verbonden). Hiermee verwijdert u het gerepliceerde item uit Azure Site Recovery (facturering is gestopt). Configuratie van de replicatie op de lokale virtuele machine **niet** worden opgeschoond. 

    > [!NOTE]
    > Als u ervoor kiest de **verwijderen** optie, en vervolgens de volgende scripts om de replicatie-instellingen op te schonen tun lokale VMM-Server.
3. Dit script uitvoert op de bronserver VMM, met behulp van PowerShell (previleges beheerder vereist) van de VMM-console. Vervang de tijdelijke aanduiding **SQLVM1** met de naam van uw virtuele machine.

        $vm = get-scvirtualmachine -Name "SQLVM1"
        Set-SCVirtualMachine -VM $vm -ClearDRProtection
4. De bovenstaande stappen schakelt u de replicatie-instellingen op de VMM-server. Om replicatie stoppen voor de virtuele machine op de Hyper-V-hostserver, moet u dit script uitvoert. SQLVM1 vervangen door de naam van uw virtuele machine en host01.contoso.com met de naam van de Hyper-V-hostserver.

    
    $vmName = 'SQLVM1' $hostName 'host01.contoso.com' $vm = 'root\virtualization\v2' Get-WmiObject - Namespace =-Query ' Selecteer * van Msvm_ComputerSystem waar ElementName '$vmName' = ' - computername $hostName $replicationService = Get-WmiObject - Namespace "root\virtualization\v2"-'Selecteer * van Msvm_ReplicationService' - computername $hostName $replicationService.RemoveReplicationRelationship($vm.__PATH) Query
    
       
 
## <a name="disable-protection-for-a-hyper-v-virtual-machine-replicating-to-secondary-vmm-server-using-the-system-center-vmm-to-vmm-scenario"></a>Schakel de beveiliging voor een Hyper-V virtuele machine repliceren naar de secundaire VMM-Server met de System Center VMM voor VMM-scenario

1. In **beveiligde Items** > **gerepliceerde Items**, met de rechtermuisknop op de machine > **schakelt u replicatie uit**.
2. In **schakelt u replicatie uit**, selecteer een van de volgende opties:

    - **Schakel replicatie en verwijderen (aanbevolen)** : deze optie het gerepliceerde item verwijderen uit Azure Site Recovery en de replicatie voor de machine is gestopt. Configuratie van de replicatie op de lokale virtuele machine wordt opgeschoond en facturering van Site Recovery voor deze beveiligde server is gestopt.
    - **Verwijder** -deze optie moet alleen worden gebruikt als de bronomgeving wordt verwijderd of niet toegankelijk (niet verbonden). Hiermee verwijdert u het gerepliceerde item uit Azure Site Recovery (facturering is gestopt). Configuratie van de replicatie op de lokale virtuele machine **niet** worden opgeschoond. Voer de volgende reeks scripts om de replicatie-instellingen op de lokale virtuele machines op te schonen.
> [!NOTE]
> Als u ervoor kiest de **verwijderen** optie, en vervolgens de volgende scripts om de replicatie-instellingen op te schonen tun lokale VMM-Server.

3. Dit script uitvoert op de bronserver VMM, met behulp van PowerShell (previleges beheerder vereist) van de VMM-console. Vervang de tijdelijke aanduiding **SQLVM1** met de naam van uw virtuele machine.

         $vm = get-scvirtualmachine -Name "SQLVM1"
         Set-SCVirtualMachine -VM $vm -ClearDRProtection
4. Op de secundaire VMM-beheerserver, voer dit script om de instellingen voor de secundaire virtuele machine op te schonen:

        $vm = get-scvirtualmachine -Name "SQLVM1"
        Remove-SCVirtualMachine -VM $vm -Force
5. Vernieuwen op de secundaire VMM-beheerserver, de virtuele machines op de Hyper-V-hostserver, zodat de secundaire virtuele machine opnieuw wordt gedetecteerd in de VMM-console.
6. De bovenstaande stappen wissen om de replicatie-instellingen op de VMM-server. Als u wilt om replicatie stoppen voor de virtuele machine, voer het volgende script geselecteerd de primaire en secundaire virtuele machines. SQLVM1 vervangen door de naam van de virtuele machine.

        Remove-VMReplication –VMName “SQLVM1”




