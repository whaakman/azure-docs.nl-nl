---
title: Instellen van de bronomgeving (VMware naar Azure) | Microsoft Docs
description: In dit artikel wordt beschreven hoe uw on-premises-omgeving instellen voor virtuele VMware-machines repliceren naar Azure.
services: site-recovery
documentationcenter: 
author: AnoopVasudavan
manager: gauravd
editor: 
ms.assetid: 
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 11/23/2017
ms.author: anoopkv
ms.openlocfilehash: 32a3f7498d3c8891178818436e33221f91ae2f8f
ms.sourcegitcommit: b5c6197f997aa6858f420302d375896360dd7ceb
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/21/2017
---
# <a name="set-up-the-source-environment-vmware-to-azure"></a>Instellen van de bronomgeving (VMware naar Azure)
> [!div class="op_single_selector"]
> * [VMware naar Azure](./site-recovery-set-up-vmware-to-azure.md)
> * [Fysieke naar Azure](./site-recovery-set-up-physical-to-azure.md)

Dit artikel wordt beschreven hoe u uw on-premises omgeving instelt om te repliceren van virtuele machines waarop VMware naar Azure.

## <a name="prerequisites"></a>Vereisten

Het artikel wordt ervan uitgegaan dat u al hebt gemaakt:
- Een Recovery Services-kluis in de [Azure-portal](http://portal.azure.com "Azure-portal").
- Een speciaal account in uw VMware vCenter die kan worden gebruikt voor [automatische detectie](./site-recovery-vmware-to-azure.md).
- Een virtuele machine waarop de configuratieserver te installeren.

## <a name="configuration-server-minimum-requirements"></a>Minimale vereisten voor configuratie-server
De volgende tabel bevat de minimale hardware, software en netwerkvereisten voor een configuratieserver.

> [!IMPORTANT]
> Bij het implementeren van een configuratie-Server voor het beveiligen van virtuele VMware-machines, raden wij aan dat u als implementeren een **maximaal beschikbare (HA)** virtuele machine.

[!INCLUDE [site-recovery-configuration-server-requirements](../../includes/site-recovery-configuration-and-scaleout-process-server-requirements.md)]

> [!NOTE]
> HTTPS gebaseerde proxy-servers worden niet ondersteund door de configuratieserver.

## <a name="choose-your-protection-goals"></a>Uw beveiligingsdoelstellingen kiezen

1. In de Azure portal, gaat u naar de **Recovery Services** blade kluis en selecteer uw kluis.
2. In het menu van de bron van de kluis, gaat u naar **aan de slag** > **siteherstel** > **stap 1: infrastructuur voorbereiden**  >  **Beveiligingsdoel**.

    ![Doelstellingen kiezen](./media/site-recovery-set-up-vmware-to-azure/choose-goals.png)
3. In **beveiligingsdoel**, selecteer **naar Azure**, en kies **Ja, met VMware vSphere Hypervisor**. Klik vervolgens op **OK**.

    ![Doelstellingen kiezen](./media/site-recovery-set-up-vmware-to-azure/choose-goals2.png)

## <a name="set-up-the-source-environment"></a>De bronomgeving instellen
Instellen van de bronomgeving omvat twee belangrijkste activiteiten:

- Installeren en registreren van een server configureren met Site Recovery.
- Uw on-premises virtuele machines detecteren door verbinding te maken van Site Recovery op uw lokale VMware vCenter of vSphere EXSi-hosts.

### <a name="step-1-install-and-register-a-configuration-server"></a>Stap 1: Installeren en registreren van een configuratieserver

1. Klik op **stap 1: infrastructuur voorbereiden** > **bron**. In **bron voorbereiden**, als u een configuratieserver, klikt u op geen **+ configuratieserver** een toe te voegen.

    ![Bron instellen](./media/site-recovery-set-up-vmware-to-azure/set-source1.png)
2. Op de **Server toevoegen** blade, controleert u of **configuratieserver** wordt weergegeven in **servertype**.
4. Download het installatiebestand van de Site Recovery Unified Setup.
5. Download de kluisregistratiesleutel. Wanneer u Setup Unified uitvoert moet u de registratiesleutel. De sleutel blijft vijf dagen na het genereren ervan geldig.

    ![Bron instellen](./media/site-recovery-set-up-vmware-to-azure/set-source2.png)
6. Voer op de computer die u als de configuratieserver **Azure Site Recovery Unified Setup** voor het installeren van de configuratieserver, de processerver en de hoofddoelserver.

#### <a name="run-azure-site-recovery-unified-setup"></a>Voer Azure Site Recovery Unified Setup

> [!TIP]
> Registratie van de configuratie-server mislukt als de tijd op de systeemklok van uw computer van de lokale tijd van meer dan vijf minuten verschilt. Synchroniseren van uw systeemklok met een [tijdserver](https://technet.microsoft.com/windows-server-docs/identity/ad-ds/get-started/windows-time-service/windows-time-service) voordat u de installatie start.

[!INCLUDE [site-recovery-add-configuration-server](../../includes/site-recovery-add-configuration-server.md)]

> [!NOTE]
> De configuratieserver kan worden geïnstalleerd via de opdrachtregel. Zie voor meer informatie [installeren van de configuratieserver met opdrachtregelprogramma's](http://aka.ms/installconfigsrv).

#### <a name="add-the-vmware-account-for-automatic-discovery"></a>De VMware-account voor automatische detectie toevoegen

[!INCLUDE [site-recovery-add-vcenter-account](../../includes/site-recovery-add-vcenter-account.md)]

### <a name="step-2-add-a-vcenter"></a>Stap 2: Een vCenter toevoegen
U moet verbinding maken met de VMware vCenter-Server of vSphere ESXi-hosts met Site Recovery zodat Azure Site Recovery voor het detecteren van virtuele machines die worden uitgevoerd in uw on-premises omgeving.

Selecteer **+ vCenter** starten gebruikmaken van een VMware vCenter-server of een VMware vSphere ESXi-host.

[!INCLUDE [site-recovery-add-vcenter](../../includes/site-recovery-add-vcenter.md)]


## <a name="common-issues"></a>Algemene problemen
[!INCLUDE [site-recovery-vmware-to-azure-install-register-issues](../../includes/site-recovery-vmware-to-azure-install-register-issues.md)]


## <a name="next-steps"></a>Volgende stappen
[Instellen van uw doelomgeving](./site-recovery-prepare-target-vmware-to-azure.md) in Azure.
