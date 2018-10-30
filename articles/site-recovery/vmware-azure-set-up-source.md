---
title: De bronomgeving voor herstel na noodgevallen van virtuele VMware-machines naar Azure met Azure Site Recovery instellen | Microsoft-Docs
description: Dit artikel wordt beschreven hoe u uw on-premises omgeving voor herstel na noodgevallen van virtuele VMware-machines naar Azure met Azure Site Recovery kunt instellen.
services: site-recovery
author: Rajeswari-Mamilla
manager: rochakm
ms.service: site-recovery
ms.topic: article
ms.date: 10/29/2018
ms.author: ramamill
ms.openlocfilehash: 44b1cac461fa49b4bea19b7cf98f6038eb264492
ms.sourcegitcommit: fbdfcac863385daa0c4377b92995ab547c51dd4f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/30/2018
ms.locfileid: "50230723"
---
# <a name="set-up-the-source-environment-for-disaster-recovery-of-vmware-vms-to-azure"></a>De bronomgeving voor herstel na noodgevallen van virtuele VMware-machines naar Azure instellen

Dit artikel wordt beschreven hoe u voor het instellen van uw bronomgeving on-premises virtuele VMware-machines repliceren naar Azure. Dit omvat stappen voor het selecteren van de replicatiescenario, instellen van een on-premises machine als de Site Recovery-configuratieserver en automatisch detecteren van on-premises VM's. 

## <a name="prerequisites"></a>Vereisten

Het artikel wordt ervan uitgegaan dat u al hebt:

- Uw implementatie met behulp van geplande [Azure Site Recovery Deployment Planner](site-recovery-deployment-planner.md). Zo kunt u voldoende bandbreedte, op basis van uw dagelijkse gegevens wijzigen, om te voldoen aan uw gewenste beoogde herstelpunt (RPO) toewijzen.
- [Instellen van resources](tutorial-prepare-azure.md) in de [Azure-portal](http://portal.azure.com).
- [Instellen van on-premises VMware](vmware-azure-tutorial-prepare-on-premises.md), met inbegrip van een specifiek account voor automatische detectie.

## <a name="choose-your-protection-goals"></a>Uw beveiligingsdoelstellingen kiezen

1. Selecteer de naam van de kluis in **Recovery Services-kluizen**. We gebruiken **ContosoVMVault** voor dit scenario.
2. Selecteer in **Aan de slag** Site Recovery. Selecteer vervolgens **Infrastructuur voorbereiden**.
3. In **Beveiligingsdoel** > **Waar bevinden de machines zich**, selecteert u **On-premises**.
4. In **Waarnaartoe wilt u de machines repliceren** selecteert u **Naar Azure**.
5. In **Zijn de machines gevirtualiseerd** selecteert **Ja, met VMware vSphere Hypervisor**. Selecteer vervolgens **OK**.

## <a name="set-up-the-configuration-server"></a>Instellen van de configuratieserver

U kunt de configuratieserver instellen als een on-premises virtuele VMware-machine via een sjabloon van de Open Virtualization-toepassing (OVA). [Meer informatie](concepts-vmware-to-azure-architecture.md) over de onderdelen die worden geïnstalleerd op de VMware-VM.

1. Meer informatie over de [vereisten](vmware-azure-deploy-configuration-server.md#prerequisites) voor implementatie van configuratieserver.
2. [Controleer capaciteit](vmware-azure-deploy-configuration-server.md#capacity-planning) voor implementatie.
3. [Download](vmware-azure-deploy-configuration-server.md#download-the-template) en [importeren](vmware-azure-deploy-configuration-server.md#import-the-template-in-vmware) het OVA-sjabloon voor het instellen van een on-premises virtuele VMware-machine waarop de configuratieserver wordt uitgevoerd. Het certificaat dat is opgegeven met de sjabloon is een evaluatie-licentie en is geldig gedurende 180 dagen. Na deze periode klant nodig heeft om de windows met een geleverde certificaat te activeren.
4. Schakel op de VMware-VM, en [registreren](vmware-azure-deploy-configuration-server.md#register-the-configuration-server-with-azure-site-recovery-services) in de Recovery Services-kluis.

## <a name="azure-site-recovery-folder-exclusions-from-antivirus-program"></a>Azure Site Recovery-uitsluitingen van antivirusprogramma 's

### <a name="if-antivirus-software-is-active-on-source-machine"></a>Als er antivirussoftware is actief op de bronmachine

Als de bronmachine heeft een actieve antivirusprogramma's, kan de installatiemap moet worden uitgesloten. Dus uitgesloten map *C:\ProgramData\ASR\agent* voor goede replicatie.

### <a name="if-antivirus-software-is-active-on-configuration-server"></a>Als de antivirussoftware op de configuratieserver actief is

De volgende mappen van antivirussoftware voor goede replicatie en om te voorkomen dat problemen met de netwerkverbinding uitsluiten

 1. C:\Program Files\Microsoft Azure Recovery Services-Agent.
 2. C:\Program Files\Microsoft Azure Site Recovery Provider
 3. C:\Program Files\Microsoft Azure Site Recovery Configuration Manager 
 4. C:\Program Files\Microsoft Azure Site Recovery fout verzameling Tool 
 5. C:\thirdparty
 6. C:\Temp
 7. C:\strawberry
 8. C:\ProgramData\MySQL
 9. C:\Program bestanden (x86) \MySQL
 10. C:\ProgramData\ASR
 11. C:\ProgramData\Microsoft Azure Site Recovery
 12. C:\ProgramData\ASRLogs
 13. C:\ProgramData\ASRSetupLogs
 14. C:\ProgramData\LogUploadServiceLogs
 15. C:\Inetpub
 16. Installatiemap van ASR. Bijvoorbeeld: E:\Program bestanden (x86) \Microsoft Azure Site Recovery

### <a name="if-antivirus-software-is-active-on-scale-out-process-servermaster-target"></a>Als er antivirussoftware is actief op scale-out server/Master Target verwerken

De volgende mappen van antivirussoftware uitsluiten

1. C:\Program Files\Microsoft Azure Recovery Services-Agent
2. C:\ProgramData\ASR
3. C:\ProgramData\ASRLogs
4. C:\ProgramData\ASRSetupLogs
5. C:\ProgramData\LogUploadServiceLogs
6. C:\ProgramData\Microsoft Azure Site Recovery
7. De installatiemap proces, voorbeeld met gelijke taakverdeling ASR: C:\Program Files (x86) \Microsoft Azure Site Recovery

## <a name="common-issues"></a>Algemene problemen
[!INCLUDE [site-recovery-vmware-to-azure-install-register-issues](../../includes/site-recovery-vmware-to-azure-install-register-issues.md)]

## <a name="next-steps"></a>Volgende stappen
[De doelomgeving instellen](./vmware-azure-set-up-target.md) in Azure.
