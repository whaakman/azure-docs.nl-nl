---
title: De bronomgeving instellen voor VMware naar Azure-replicatie met Azure Site Recovery | Microsoft Docs
description: In dit artikel wordt beschreven hoe u uw on-premises-omgeving instellen voor virtuele VMware-machines repliceren naar Azure met Azure Site Recovery.
services: site-recovery
author: Rajeswari-Mamilla
manager: rochakm
ms.service: site-recovery
ms.topic: article
ms.date: 07/06/2018
ms.author: ramamill
ms.openlocfilehash: 1380c1bc820a815fae317a86fcd0ee4f46dd9aa5
ms.sourcegitcommit: a1e1b5c15cfd7a38192d63ab8ee3c2c55a42f59c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/10/2018
ms.locfileid: "37952651"
---
# <a name="set-up-the-source-environment-for-vmware-to-azure-replication"></a>De bronomgeving instellen voor VMware naar Azure-replicatie

Dit artikel wordt beschreven hoe u voor het instellen van uw bronomgeving on-premises virtuele VMware-machines repliceren naar Azure. Dit omvat stappen voor het selecteren van de replicatiescenario, instellen van een on-premises machine als de Site Recovery-configuratieserver en automatisch detecteren van on-premises VM's. 

## <a name="prerequisites"></a>Vereisten

Het artikel wordt ervan uitgegaan dat u al hebt:
- [Instellen van resources](tutorial-prepare-azure.md) in de [Azure-portal](http://portal.azure.com).
- [Instellen van on-premises VMware](vmware-azure-tutorial-prepare-on-premises.md), met inbegrip van een specifiek account voor automatische detectie.



## <a name="choose-your-protection-goals"></a>Uw beveiligingsdoelstellingen kiezen

1. In de Azure-portal, gaat u naar de **herstelservices** blade kluis en selecteer uw kluis.
2. In het resourcemenu van de kluis, gaat u naar **aan de slag** > **siteherstel** > **stap 1: infrastructuur voorbereiden**  >  **Beveiligingsdoel**.

    ![Doelstellingen kiezen](./media/vmware-azure-set-up-source/choose-goals.png)
3. In **beveiligingsdoel**, selecteer **naar Azure**, en kies **Ja, met VMware vSphere Hypervisor**. Klik vervolgens op **OK**.

    ![Doelstellingen kiezen](./media/vmware-azure-set-up-source/choose-goals2.png)

## <a name="set-up-the-configuration-server"></a>Instellen van de configuratieserver

U kunt de configuratieserver instellen als een on-premises virtuele VMware-machine via een sjabloon van de Open Virtualization-toepassing (OVA). [Meer informatie](concepts-vmware-to-azure-architecture.md) over de onderdelen die worden geïnstalleerd op de VMware-VM.

1. Meer informatie over de [vereisten](vmware-azure-deploy-configuration-server.md#prerequisites) voor implementatie van configuratieserver.
2. [Controleer capaciteit](vmware-azure-deploy-configuration-server.md#capacity-planning) voor implementatie.
3. [Download](vmware-azure-deploy-configuration-server.md#download-the-template) en [importeren](vmware-azure-deploy-configuration-server.md#import-the-template-in-vmware) het OVA-sjabloon voor het instellen van een on-premises virtuele VMware-machine waarop de configuratieserver wordt uitgevoerd. Het certificaat dat is opgegeven met de sjabloon is een evaluatie-licentie en is geldig gedurende 180 dagen. Na deze periode klant nodig heeft om de windows met een geleverde certificaat te activeren.
4. Schakel op de VMware-VM, en [registreren](vmware-azure-deploy-configuration-server.md#register-the-configuration-server-with-azure-site-recovery-services) in de Recovery Services-kluis.


## <a name="add-the-vmware-account-for-automatic-discovery"></a>Toevoegen van de VMware-account voor automatische detectie

[!INCLUDE [site-recovery-add-vcenter-account](../../includes/site-recovery-add-vcenter-account.md)]

## <a name="connect-to-the-vmware-server"></a>Verbinding maken met de VMware-server

Als u wilt toestaan dat Azure Site Recovery voor het detecteren van virtuele machines die worden uitgevoerd in uw on-premises-omgeving, moet u verbinding maken met de VMware vCenter-Server of vSphere ESXi-hosts met Site Recovery.

Selecteer **+ vCenter** om te beginnen verbinding te maken van een VMware vCenter-server of VMware vSphere ESXi-host.

[!INCLUDE [site-recovery-add-vcenter](../../includes/site-recovery-add-vcenter.md)]


## <a name="common-issues"></a>Algemene problemen
[!INCLUDE [site-recovery-vmware-to-azure-install-register-issues](../../includes/site-recovery-vmware-to-azure-install-register-issues.md)]


## <a name="next-steps"></a>Volgende stappen
[De doelomgeving instellen](./vmware-azure-set-up-target.md) in Azure.
