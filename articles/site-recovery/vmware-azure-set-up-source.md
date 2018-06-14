---
title: Instellen van de bronomgeving voor VMware naar Azure replicatie met Azure Site Recovery | Microsoft Docs
description: In dit artikel wordt beschreven hoe uw on-premises-omgeving instellen voor virtuele VMware-machines repliceren naar Azure met Azure Site Recovery.
services: site-recovery
author: AnoopVasudavan
manager: gauravd
ms.service: site-recovery
ms.topic: article
ms.date: 03/05/2018
ms.author: anoopkv
ms.openlocfilehash: b2c564e8d49e39d9cdc09d3fe168388d579de70e
ms.sourcegitcommit: 168426c3545eae6287febecc8804b1035171c048
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/08/2018
ms.locfileid: "29812654"
---
# <a name="set-up-the-source-environment-for-vmware-to-azure-replication"></a>Instellen van de bronomgeving voor VMware naar Azure replicatie

In dit artikel wordt beschreven hoe uw on-premises-omgeving van bron instellen voor virtuele VMware-machines repliceren naar Azure. Dit omvat stappen voor het selecteren van uw replicatiescenario, instellen van een lokale machine als de configuratieserver Site Recovery en automatisch detecteren van on-premises virtuele machines. 

## <a name="prerequisites"></a>Vereisten

Het artikel wordt ervan uitgegaan dat u al hebt:
- [Resources instellen](tutorial-prepare-azure.md) in de [Azure-portal](http://portal.azure.com).
- [Instellen van lokale VMware](vmware-azure-tutorial-prepare-on-premises.md), met inbegrip van een specifiek account voor automatische detectie.



## <a name="choose-your-protection-goals"></a>Uw beveiligingsdoelstellingen kiezen

1. In de Azure portal, gaat u naar de **Recovery Services** blade kluis en selecteer uw kluis.
2. In het menu van de bron van de kluis, gaat u naar **aan de slag** > **siteherstel** > **stap 1: infrastructuur voorbereiden**  >  **Beveiligingsdoel**.

    ![Doelstellingen kiezen](./media/vmware-azure-set-up-source/choose-goals.png)
3. In **beveiligingsdoel**, selecteer **naar Azure**, en kies **Ja, met VMware vSphere Hypervisor**. Klik vervolgens op **OK**.

    ![Doelstellingen kiezen](./media/vmware-azure-set-up-source/choose-goals2.png)

## <a name="set-up-the-configuration-server"></a>De configuratieserver instellen

U de configuratieserver instellen als een lokale VMware virtuele machine, een Open Virtualization Format OVF ()-sjabloon gebruiken. [Meer informatie](concepts-vmware-to-azure-architecture.md) over de onderdelen die worden geïnstalleerd op de VMware-VM. 

1. Meer informatie over de [vereisten](vmware-azure-deploy-configuration-server.md#prerequisites) voor implementatie van configuratieserver.
2. [Controleer capaciteit cijfers](vmware-azure-deploy-configuration-server.md#capacity-planning) voor implementatie.
3. [Download](vmware-azure-deploy-configuration-server.md#download-the-template) en [importeren](vmware-azure-deploy-configuration-server.md#import-the-template-in-vmware) de OVF-sjabloon (how-naar-implementeren-configuratie-server.md) voor het instellen van een lokale VMware virtuele machine die wordt uitgevoerd van de configuratieserver.
4. Schakel op de VMware-VM en [registreren](vmware-azure-deploy-configuration-server.md#register-the-configuration-server) in de Recovery Services-kluis.


## <a name="add-the-vmware-account-for-automatic-discovery"></a>De VMware-account voor automatische detectie toevoegen

[!INCLUDE [site-recovery-add-vcenter-account](../../includes/site-recovery-add-vcenter-account.md)]

## <a name="connect-to-the-vmware-server"></a>Verbinding maken met de VMware-server

U moet verbinding maken met de VMware vCenter-Server of vSphere ESXi-hosts met Site Recovery zodat Azure Site Recovery voor het detecteren van virtuele machines die worden uitgevoerd in uw on-premises omgeving.

Selecteer **+ vCenter** starten gebruikmaken van een VMware vCenter-server of een VMware vSphere ESXi-host.

[!INCLUDE [site-recovery-add-vcenter](../../includes/site-recovery-add-vcenter.md)]


## <a name="common-issues"></a>Algemene problemen
[!INCLUDE [site-recovery-vmware-to-azure-install-register-issues](../../includes/site-recovery-vmware-to-azure-install-register-issues.md)]


## <a name="next-steps"></a>Volgende stappen
[Instellen van uw doelomgeving](./vmware-azure-set-up-target.md) in Azure.
