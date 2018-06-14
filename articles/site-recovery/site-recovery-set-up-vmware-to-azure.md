---
title: Instellen van de bronomgeving (VMware naar Azure) | Microsoft Docs
description: In dit artikel wordt beschreven hoe uw on-premises-omgeving instellen voor virtuele VMware-machines repliceren naar Azure.
services: site-recovery
author: AnoopVasudavan
manager: gauravd
ms.service: site-recovery
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 02/22/2018
ms.author: anoopkv
ms.openlocfilehash: 2b6b0e5cc95f28b5596e7e898f5f035e3647d9c5
ms.sourcegitcommit: 0b02e180f02ca3acbfb2f91ca3e36989df0f2d9c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/07/2018
ms.locfileid: "29768436"
---
# <a name="set-up-the-source-environment-vmware-to-azure"></a>Instellen van de bronomgeving (VMware naar Azure)
> [!div class="op_single_selector"]
> * [VMware naar Azure](./site-recovery-set-up-vmware-to-azure.md)
> * [Fysieke naar Azure](./site-recovery-set-up-physical-to-azure.md)

In dit artikel wordt beschreven hoe de bron, on-premises omgeving instellen voor het repliceren van virtuele machines waarop VMware naar Azure. Dit omvat stappen voor het selecteren van uw replicatiescenario, instellen van een lokale machine als de configuratieserver Site Recovery en automatisch detecteren van on-premises virtuele machines. 

## <a name="prerequisites"></a>Vereisten

Het artikel wordt ervan uitgegaan dat u al hebt:
- [Resources instellen](tutorial-prepare-azure.md) in de [Azure-portal](http://portal.azure.com).
- [Instellen van lokale VMware](tutorial-prepare-on-premises-vmware.md), met inbegrip van een specifiek account voor automatische detectie.



## <a name="choose-your-protection-goals"></a>Uw beveiligingsdoelstellingen kiezen

1. In de Azure portal, gaat u naar de **Recovery Services** blade kluis en selecteer uw kluis.
2. In het menu van de bron van de kluis, gaat u naar **aan de slag** > **siteherstel** > **stap 1: infrastructuur voorbereiden**  >  **Beveiligingsdoel**.

    ![Doelstellingen kiezen](./media/site-recovery-set-up-vmware-to-azure/choose-goals.png)
3. In **beveiligingsdoel**, selecteer **naar Azure**, en kies **Ja, met VMware vSphere Hypervisor**. Klik vervolgens op **OK**.

    ![Doelstellingen kiezen](./media/site-recovery-set-up-vmware-to-azure/choose-goals2.png)

## <a name="set-up-the-configuration-server"></a>De configuratieserver instellen

U de configuratieserver instellen als een lokale VMware virtuele machine, een Open Virtualization Format OVF ()-sjabloon gebruiken. [Meer informatie](concepts-vmware-to-azure-architecture.md) over de onderdelen die worden geïnstalleerd op de VMware-VM. 

1. Meer informatie over de [vereisten](how-to-deploy-configuration-server.md#prerequisites) voor implementatie van configuratieserver. [Controleer capaciteit cijfers](how-to-deploy-configuration-server.md#capacity-planning) voor implementatie.
2. [Download](how-to-deploy-configuration-server.md#download-the-template) en [importeren](how-to-deploy-configuration-server.md#import-the-template-in-vmware) de OVF-sjabloon (how-naar-implementeren-configuratie-server.md) voor het instellen van een lokale VMware virtuele machine die wordt uitgevoerd van de configuratieserver.
3. Schakel op de VMware-VM en [registreren](how-to-deploy-configuration-server.md#register-the-configuration-server) in de Recovery Services-kluis.


## <a name="add-the-vmware-account-for-automatic-discovery"></a>De VMware-account voor automatische detectie toevoegen

[!INCLUDE [site-recovery-add-vcenter-account](../../includes/site-recovery-add-vcenter-account.md)]

## <a name="connect-to-the-vmware-server"></a>Verbinding maken met de VMware-server

U moet verbinding maken met de VMware vCenter-Server of vSphere ESXi-hosts met Site Recovery zodat Azure Site Recovery voor het detecteren van virtuele machines die worden uitgevoerd in uw on-premises omgeving.

Selecteer **+ vCenter** starten gebruikmaken van een VMware vCenter-server of een VMware vSphere ESXi-host.

[!INCLUDE [site-recovery-add-vcenter](../../includes/site-recovery-add-vcenter.md)]


## <a name="common-issues"></a>Algemene problemen
[!INCLUDE [site-recovery-vmware-to-azure-install-register-issues](../../includes/site-recovery-vmware-to-azure-install-register-issues.md)]


## <a name="next-steps"></a>Volgende stappen
[Instellen van uw doelomgeving](./site-recovery-prepare-target-vmware-to-azure.md) in Azure.
