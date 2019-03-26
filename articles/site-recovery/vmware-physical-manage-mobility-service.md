---
title: Beheren van de Mobility-agent op servers voor herstel na noodgevallen van virtuele VMware-machines en fysieke servers met Azure Site Recovery | Microsoft Docs
description: Mobility Service-agent voor herstel na noodgevallen van virtuele VMware-machines en fysieke servers naar Azure met behulp van de Azure Site Recovery-service beheren.
author: Rajeswari-Mamilla
manager: rochakm
ms.service: site-recovery
ms.topic: conceptual
ms.date: 03/25/2019
ms.author: ramamill
ms.openlocfilehash: 441b58e60bf8dfd5f164ac24d746b9791158ade2
ms.sourcegitcommit: 72cc94d92928c0354d9671172979759922865615
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/25/2019
ms.locfileid: "58420111"
---
# <a name="manage-mobility-agent-on-protected-machines"></a>Mobility-agent op beveiligde machines beheren

Instellen van mobility-agent op uw server wanneer u Azure Site Recovery voor herstel na noodgevallen van virtuele VMware-machines en fysieke servers naar Azure gebruiken. Mobility-agent coördineert de communicatie tussen de beveiligde computer, configuratie van server-/ uitbreidbare processerver en beheert de gegevensreplicatie. In dit artikel bevat een overzicht van algemene taken voor het beheren van de mobility-agent nadat deze geïmplementeerd.

## <a name="update-mobility-service-from-azure-portal"></a>Bijwerken van de mobiliteitsservice van Azure-portal

1. Zorg ervoor dat de configuratieserver en scale-out processervers hoofddoelservers die deel van uw implementatie uitmaken worden bijgewerkt voordat u de Mobility-Service op beveiligde machines bijwerken voordat u begint.
2. Open de kluis in de portal > **gerepliceerde items**.
3. Als de configuratieserver de meest recente versie is, ziet u een melding die leest "de nieuwe agent bijwerken van Azure Site recovery-replicatie is beschikbaar. Klik om te installeren."

     ![Venster gerepliceerde items](./media/vmware-azure-install-mobility-service/replicated-item-notif.png)

4. Klik op de melding en **agentupdate**, selecteert u de machines waarop u wilt upgraden van de Mobility-service. Klik vervolgens op **OK**.

     ![Lijst met items voor VM gerepliceerd](./media/vmware-azure-install-mobility-service/update-okpng.png)

5. De Mobility-Service-Update-taak wordt gestart voor elk van de geselecteerde computers.

## <a name="update-mobility-service-through-powershell-script-on-windows-server"></a>Bijwerken van de Mobility-service via powershell-script op Windows server

Gebruik de volgende script om te upgraden van de mobility-service op een server via power shell-cmdlet

```azurepowershell
Update-AzureRmRecoveryServicesAsrMobilityService -ReplicationProtectedItem $rpi -Account $fabric.fabricSpecificDetails.RunAsAccounts[0]
```

## <a name="update-account-used-for-push-installation-of-mobility-service"></a>Account voor push-installatie van mobiliteitsservice bijwerken

U hebt opgegeven een account dat gebruikmaakt van de processerver van Site Recovery voor toegang tot de machines en de service te installeren wanneer replicatie is ingeschakeld voor de machine tijdens de implementatie van Site Recovery om in te schakelen van push-installatie van de Mobility-service. Als u wilt dat de referenties voor dit account bijwerken, voert u de [deze instructies](vmware-azure-manage-configuration-server.md#modify-credentials-for-mobility-service-installation).

## <a name="uninstall-mobility-service"></a>Verwijderen van de Mobility-service

### <a name="on-a-windows-machine"></a>Op een Windows-machine

Verwijderen van de gebruikersinterface of via een opdrachtprompt.

- **Vanuit de gebruikersinterface**: Selecteer in het Configuratiescherm van de machine, **programma's**. Selecteer **Microsoft Azure Site Recovery Mobility Service/Master Target server** > **verwijderen**.
- **Vanaf een opdrachtprompt**: Open een opdrachtpromptvenster als beheerder op de machine. Voer de volgende opdracht uit: 
    ```
    MsiExec.exe /qn /x {275197FC-14FD-4560-A5EB-38217F80CBD1} /L+*V "C:\ProgramData\ASRSetupLogs\UnifiedAgentMSIUninstall.log"
    ```

### <a name="on-a-linux-machine"></a>Op een Linux-machine
1. Op de Linux-machine, moet u zich aanmelden als een **hoofdmap** gebruiker.
2. Ga naar /user/local/ASR in een terminal.
3. Voer de volgende opdracht uit:
    ```
    uninstall.sh -Y

## Install Site Recovery VSS provider on source machine

Azure Site Recovery VSS provider is required on the source machine to generate application consistency points. If the installation of the provider didn't succeed through push installation, follow the below given guidelines to install it manually.

1. Open admin cmd window.
2. Navigate to the mobility service installation location. (Eg - C:\Program Files (x86)\Microsoft Azure Site Recovery\agent)
3. Run the script InMageVSSProvider_Uninstall.cmd . This will uninstall the service if it already exists.
4. Run the script InMageVSSProvider_Install.cmd to install the VSS provider manually.

## Next steps

- [Set up disaster recovery for VMware VMs](vmware-azure-tutorial.md)
- [Set up disaster recovery for physical servers](physical-azure-disaster-recovery.md)
