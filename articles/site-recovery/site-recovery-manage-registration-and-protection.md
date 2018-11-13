---
title: Servers verwijderen en beveiliging uitschakelen | Microsoft Docs
description: Dit artikel wordt beschreven hoe u de registratie ongedaan maken-servers vanaf een Site Recovery-kluis en schakel de beveiliging voor virtuele machines en fysieke servers.
author: rayne-wiselman
manager: carmonm
ms.service: site-recovery
ms.topic: conceptual
ms.date: 10/29/2018
ms.author: raynew
ms.openlocfilehash: da9319934068709d5635352fdbd52c3ca6ac49be
ms.sourcegitcommit: 6b7c8b44361e87d18dba8af2da306666c41b9396
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/12/2018
ms.locfileid: "51568883"
---
# <a name="remove-servers-and-disable-protection"></a>Servers verwijderen en beveiliging uitschakelen

Dit artikel wordt beschreven hoe u registratie van de servers van Recovery Services-kluis en hoe u beveiliging voor machines die zijn beveiligd door Site Recovery uitschakelen.


## <a name="unregister-a--configuration-server"></a>Registratie van een configuratieserver

Als u virtuele VMware-machines of fysieke Windows/Linux-servers naar Azure repliceren, kunt u de registratie van een niet-verbonden configuratieserver in een kluis als volgt ongedaan maken:

1. [Schakel de beveiliging van virtuele machines](#disable-protection-for-a-vmware-vm-or-physical-server-vmware-to-azure).
2. [Ontkoppelen of verwijderen](vmware-azure-set-up-replication.md#disassociate-or-delete-a-replication-policy) replicatiebeleid.
3. [De configuratieserver verwijderen](vmware-azure-manage-configuration-server.md#delete-or-unregister-a-configuration-server)

## <a name="unregister-a-vmm-server"></a>Registratie van een VMM-server

1. Stoppen met het repliceren van virtuele machines in de clouds op de VMM-server die u wilt verwijderen.
2. Verwijder alle Netwerktoewijzingen die worden gebruikt door clouds op de VMM-server die u wilt verwijderen. In **Site Recovery-infrastructuur** > **voor System Center VMM** > **netwerktoewijzing**, met de rechtermuisknop op de netwerktoewijzing > **Verwijderen**.
3. Houd er rekening mee de ID van de VMM-server.
4. Maak replicatiebeleid voor van clouds op de VMM-server die u wilt verwijderen.  In **Site Recovery-infrastructuur** > **voor System Center VMM** >  **replicatiebeleid**, dubbelklik op het bijbehorende beleid. Met de rechtermuisknop op de cloud > **koppeling verbreken**.
5. Verwijder de VMM-server of het actieve knooppunt. In **Site Recovery-infrastructuur** > **voor System Center VMM** > **VMM-Servers**, met de rechtermuisknop op de server > **verwijderen** .
6. Als de VMM-server zich in een niet-verbonden status, downloadt en voert u de [script voor opschoning](https://aka.ms/asr-cleanup-script-vmm) op de VMM-server. Open PowerShell met de **als Administrator uitvoeren** optie, voor het wijzigen van het uitvoeringsbeleid voor het bereik van standaard (LocalMachine /). Geef de ID van de VMM-server die u wilt verwijderen in het script. Het script Hiermee verwijdert u de registratie en cloudkoppeling bij van de server.
5. Het script voor opschoning uitvoeren op een secundaire VMM-server.
6. Het script voor opschoning uitvoeren op een andere passieve VMM-clusterknooppunten die de Provider geïnstalleerd hebben.
7. De Provider op de VMM-server handmatig verwijderen. Als u een cluster hebt, verwijderen van alle knooppunten.
8. Als uw virtuele machines zijn gerepliceerd naar Azure, moet u de Microsoft Recovery Services-agent verwijderen van Hyper-V-hosts in de verwijderde clouds.

## <a name="unregister-a-hyper-v-host-in-a-hyper-v-site"></a>Registratie van een Hyper-V-host in een Hyper-V-Site

Hyper-V-hosts die niet worden beheerd door VMM worden verzameld in een Hyper-V-site. Een host in een Hyper-V-site als volgt verwijderen:

1. Schakel de replicatie voor Hyper-V VM's op de host.
2. Maak beleid voor de Hyper-V-site. In **Site Recovery-infrastructuur** > **voor Hyper-V-Sites** >  **replicatiebeleid**, dubbelklik op het bijbehorende beleid. Met de rechtermuisknop op de site > **koppeling verbreken**.
3. Verwijder de Hyper-V-hosts. In **Site Recovery-infrastructuur** > **voor Hyper-V-Sites** > **Hyper-V-Hosts**, met de rechtermuisknop op de server > **verwijderen** .
4. De Hyper-V-site verwijderen nadat alle hosts zijn verwijderd uit het. In **Site Recovery-infrastructuur** > **voor Hyper-V-Sites** > **Hyper-V-Sites**, met de rechtermuisknop op de site > **verwijderen** .
5. Als uw Hyper-V-host zich in een **verbroken** status en vervolgens op het volgende script uitvoeren op elke Hyper-V-host die u hebt verwijderd. Het script ruimt instellingen op de server, en de registratie van het uit de kluis.



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



## <a name="disable-protection-for-a-vmware-vm-or-physical-server-vmware-to-azure"></a>Schakel de beveiliging voor een VM met VMware of fysieke server (VMware naar Azure)

1. In **beveiligde Items** > **gerepliceerde Items**, met de rechtermuisknop op de machine > **replicatie uitschakelen**.
2. In **replicatie uitschakelen** pagina, selecteert u een van deze opties:
    - **Schakelt u replicatie verwijderen (aanbevolen)** : deze optie wordt het gerepliceerde item verwijderd uit Azure Site Recovery en de replicatie voor de machine is gestopt. De replicatieconfiguratie op de configuratieserver wordt opgeschoond en Site Recovery-facturering voor deze beveiligde server is gestopt.
    - **Verwijder** -deze optie moet alleen worden gebruikt als de bronomgeving is verwijderd of niet toegankelijk (niet verbonden). Dit wordt het gerepliceerde item verwijderd uit Azure Site Recovery (de facturering wordt gestopt). De replicatieconfiguratie op de configuratieserver **niet** worden opgeschoond. 

> [!NOTE]
> In zowel de opties van de mobility-service wordt niet verwijderd van de beveiligde servers, moet u handmatig verwijderen. Als u van plan bent om de server opnieuw met dezelfde configuratieserver te beveiligen, kunt u doorgaan met het verwijderen van de mobility-service.

## <a name="disable-protection-for-a-hyper-v-virtual-machine-hyper-v-to-azure"></a>Schakel de beveiliging voor een Hyper-V virtuele machine (Hyper-V naar Azure)

> [!NOTE]
> Gebruik deze procedure als u Hyper-V-machines naar Azure zonder een VMM-server repliceert. Als u uw virtuele machines met repliceert de **System Center VMM naar Azure** scenario, volg de instructies [Schakel de beveiliging voor een Hyper-V virtuele machine die wordt gerepliceerd met behulp van de System Center VMM voor Azure-scenario](#disable-protection-for-a-hyper-v-virtual-machine-replicating-using-the-system-centet-vmm-to-azure-scenario)

1. In **beveiligde Items** > **gerepliceerde Items**, met de rechtermuisknop op de machine > **replicatie uitschakelen**.
2. In **replicatie uitschakelen**, kunt u de volgende opties:
     - **Schakelt u replicatie verwijderen (aanbevolen)** : deze optie wordt het gerepliceerde item verwijderd uit Azure Site Recovery en de replicatie voor de machine is gestopt. De replicatieconfiguratie op de on-premises virtuele machine worden opgeschoond en Site Recovery-facturering voor deze beveiligde server is gestopt.
    - **Verwijder** -deze optie moet alleen worden gebruikt als de bronomgeving is verwijderd of niet toegankelijk (niet verbonden). Dit wordt het gerepliceerde item verwijderd uit Azure Site Recovery (de facturering wordt gestopt). De replicatieconfiguratie op de on-premises virtuele machine **niet** worden opgeschoond. 

    > [!NOTE]
    > Als u ervoor kiest de **verwijderen** optie voert u de volgende set van scripts voor het opschonen van de replicatie-instellingen on-premises Hyper-V-Server.
1. Op de bron-Hyper-V-hostserver, replicatie voor de virtuele machine verwijderen. SQLVM1 vervangen door de naam van uw virtuele machine en voer het script uit vanuit een administratief PowerShell


    
    $vmName = "SQLVM1" $vm = Get-WmiObject - Namespace "root\virtualization\v2"-Query ' selecteren * van Msvm_ComputerSystem waar ElementName '$vmName' = ' $replicationService = Get-WmiObject - Namespace "root\virtualization\v2"-"Selecteer * uit Msvm_ Query ReplicationService"$replicationService.RemoveReplicationRelationship($vm.__PATH)
    

## <a name="disable-protection-for-a-hyper-v-virtual-machine-replicating-to-azure-using-the-system-center-vmm-to-azure-scenario"></a>Schakel de beveiliging voor een Hyper-V virtuele machine repliceren naar Azure met behulp van de System Center VMM naar Azure

1. In **beveiligde Items** > **gerepliceerde Items**, met de rechtermuisknop op de machine > **replicatie uitschakelen**.
2. In **replicatie uitschakelen**, selecteer een van de volgende opties:

    - **Schakelt u replicatie verwijderen (aanbevolen)** : deze optie wordt het gerepliceerde item verwijderd uit Azure Site Recovery en de replicatie voor de machine is gestopt. De replicatieconfiguratie op de on-premises virtuele machine wordt opgeschoond en Site Recovery-facturering voor deze beveiligde server is gestopt.
    - **Verwijder** -deze optie moet alleen worden gebruikt als de bronomgeving is verwijderd of niet toegankelijk (niet verbonden). Dit wordt het gerepliceerde item verwijderd uit Azure Site Recovery (de facturering wordt gestopt). De replicatieconfiguratie op de on-premises virtuele machine **niet** worden opgeschoond. 

    > [!NOTE]
    > Als u ervoor kiest de **verwijderen** optie, en vervolgens de volgende scripts voor het opschonen van de replicatie-instellingen tun lokale VMM-Server.
3. Voer dit script op de bronserver VMM, met behulp van PowerShell (administrator-bevoegdheden nodig) van de VMM-console. Vervang de tijdelijke aanduiding **SQLVM1** met de naam van uw virtuele machine.

        $vm = get-scvirtualmachine -Name "SQLVM1"
        Set-SCVirtualMachine -VM $vm -ClearDRProtection
4. De bovenstaande stappen schakelt u de replicatie-instellingen op de VMM-server. Als u wilt stoppen van replicatie voor de virtuele machine die wordt uitgevoerd op de Hyper-V-host-server, moet u dit script uitvoeren. SQLVM1 vervangen door de naam van uw virtuele machine en host01.contoso.com met de naam van de Hyper-V-hostserver.

    
    $vmName = "SQLVM1" $hostName = "host01.contoso.com" $vm = Get-WmiObject - Namespace "root\virtualization\v2"-Query "selecteren * van Msvm_ComputerSystem waar ElementName = '$vmName'" - computername $hostName $replicationService = Get-WmiObject - Namespace "root\virtualization\v2"-'Selecteer * uit Msvm_ReplicationService' - computername $hostName $replicationService.RemoveReplicationRelationship($vm.__PATH) Query
    
       
 
## <a name="disable-protection-for-a-hyper-v-virtual-machine-replicating-to-secondary-vmm-server-using-the-system-center-vmm-to-vmm-scenario"></a>Schakel de beveiliging voor een Hyper-V virtuele machine repliceren naar een secundaire VMM-Server met behulp van de System Center VMM naar VMM-scenario

1. In **beveiligde Items** > **gerepliceerde Items**, met de rechtermuisknop op de machine > **replicatie uitschakelen**.
2. In **replicatie uitschakelen**, selecteer een van de volgende opties:

    - **Schakelt u replicatie verwijderen (aanbevolen)** : deze optie wordt het gerepliceerde item verwijderd uit Azure Site Recovery en de replicatie voor de machine is gestopt. De replicatieconfiguratie op de on-premises virtuele machine wordt opgeschoond en Site Recovery-facturering voor deze beveiligde server is gestopt.
    - **Verwijder** -deze optie moet alleen worden gebruikt als de bronomgeving is verwijderd of niet toegankelijk (niet verbonden). Dit wordt het gerepliceerde item verwijderd uit Azure Site Recovery (de facturering wordt gestopt). De replicatieconfiguratie op de on-premises virtuele machine **niet** worden opgeschoond. Voer de volgende reeks scripts voor het opschonen van de replicatie-instellingen on-premises virtuele machines.
> [!NOTE]
> Als u ervoor kiest de **verwijderen** optie, en vervolgens de volgende scripts voor het opschonen van de replicatie-instellingen tun lokale VMM-Server.

3. Voer dit script op de bronserver VMM, met behulp van PowerShell (administrator-bevoegdheden nodig) van de VMM-console. Vervang de tijdelijke aanduiding **SQLVM1** met de naam van uw virtuele machine.

         $vm = get-scvirtualmachine -Name "SQLVM1"
         Set-SCVirtualMachine -VM $vm -ClearDRProtection
4. Voer dit script voor het opschonen van de instellingen voor de secundaire virtuele machine op de secundaire VMM-server:

        $vm = get-scvirtualmachine -Name "SQLVM1"
        Remove-SCVirtualMachine -VM $vm -Force
5. Vernieuwen op de secundaire VMM-server, de virtuele machines op de Hyper-V-host-server, zodat de secundaire virtuele machine opnieuw wordt gedetecteerd in de VMM-console.
6. De bovenstaande stappen schakelt u de replicatie-instellingen op de VMM-server. Als u wilt om replicatie voor de virtuele machine te stoppen, voer het volgende script opgeleverd de primaire en secundaire VM's. SQLVM1 vervangen door de naam van uw virtuele machine.

        Remove-VMReplication –VMName “SQLVM1”




