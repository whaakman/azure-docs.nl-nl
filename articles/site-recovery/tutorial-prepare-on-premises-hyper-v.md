---
title: On-premises Hyper-V-server voorbereiden op herstel na noodgevallen van Hyper-V-machines naar Azure | Microsoft Docs
description: Informatie over het voorbereiden van de lokale Hyper-V-machines niet worden beheerd door System Center VMM voor noodherstel naar Azure met de Azure Site Recovery-service.
services: site-recovery
author: rayne-wiselman
ms.service: site-recovery
ms.topic: article
ms.date: 12/31/2017
ms.author: raynew
ms.custom: MVC
ms.openlocfilehash: 6e2837341e16f5077cfff18d4a34c097c165ef09
ms.sourcegitcommit: 9ea2edae5dbb4a104322135bef957ba6e9aeecde
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/03/2018
---
# <a name="prepare-on-premises-hyper-v-servers-for-disaster-recovery-to-azure"></a>On-premises Hyper-V-servers voorbereiden op herstel na noodgevallen naar Azure

Deze zelfstudie laat zien hoe u uw on-premises Hyper-V-infrastructuur voorbereidt voor Hyper-V-machines repliceren naar Azure, ten behoeve van herstel na noodgevallen bij. Hyper-V-hosts kunnen worden beheerd door System Center Virtual Machine Manager (VMM), maar dit is niet vereist.  In deze zelfstudie leert u het volgende:

> [!div class="checklist"]
> * Vereisten voor Hyper-V en VMM-vereisten, indien van toepassing controleren.
> * Voorbereiden van VMM, indien van toepassing
> * Controleer of u toegang tot internet voor Azure-locaties
> * Virtuele machines voorbereiden, zodat u er toegang toe na een failover naar Azure

Dit is de tweede zelfstudie in de reeks. Zorg ervoor dat u hebt [instellen van de Azure-onderdelen](tutorial-prepare-azure.md) zoals beschreven in de vorige zelfstudie.



## <a name="review-server-requirements"></a>Overzicht van serververeisten

Zorg ervoor dat Hyper-V-hosts aan de volgende vereisten voldoen. Als u de hosts in System Center Virtual Machine Manager (VMM)-clouds beheert, Controleer of de VMM-vereisten.


**Onderdeel** | **Hyper-V wordt beheerd door VMM** | **Hyper-V zonder VMM**
--- | --- | ---
**Hyper-V-host-besturingssysteem** | WindowsServer 2016, 2012 R2 | N.v.t.
**VMM** | VMM 2012 VMM 2012 R2 | N.v.t.


## <a name="review-hyper-v-vm-requirements"></a>Overzicht van Hyper-V-machine-vereisten

Zorg ervoor dat de virtuele machine aan vereisten samengevat in de tabel voldoet.

**VM-vereiste** | **Details**
--- | ---
**Gastbesturingssysteem** | Een gastbesturingssysteem [ondersteund door Azure](https://technet.microsoft.com/library/cc794868.aspx).
**Azure-vereisten** | Een on-premises Hyper-V-machines moeten voldoen aan de virtuele machine van Azure requirements(site-recovery-support-matrix-to-azure.md).

## <a name="prepare-vmm-optional"></a>Voorbereiden van VMM (optioneel)

Als Hyper-V-hosts worden beheerd door VMM, moet u de lokale VMM-server voorbereiden. 

- Zorg ervoor dat de VMM-server een minimaal één cloud, met een of meer hostgroepen. De virtuele machines waarop Hyper-V-host moet zich in de cloud.
- De VMM-server voorbereiden op netwerktoewijzing.

### <a name="prepare-vmm-for-network-mapping"></a>VMM voorbereiden op netwerktoewijzing

Als u VMM, [netwerktoewijzing](site-recovery-network-mapping.md) toewijzingen tussen on-premises VMM VM-netwerken en virtuele netwerken in Azure. Toewijzing zorgt ervoor dat de virtuele Azure-machines zijn verbonden met het juiste netwerk wanneer ze zijn gemaakt na een failover.

VMM voorbereiden voor netwerktoewijzing als volgt:

1. Zorg ervoor dat u hebt een [logisch netwerk van VMM](https://docs.microsoft.com/system-center/vmm/network-logical) die is gekoppeld aan de cloud waarin de Hyper-V-hosts zich bevinden.
2. Zorg ervoor dat u hebt een [VM-netwerk](https://docs.microsoft.com/system-center/vmm/network-virtual) gekoppeld aan het logische netwerk.
3. In VMM door de virtuele machines verbinding te maken met het VM-netwerk.

## <a name="verify-internet-access"></a>Controleer of u toegang tot internet

1. Voor de doeleinden van de zelfstudie is de eenvoudigste configuratie voor de Hyper-V-hosts en VMM-server, indien van toepassing, direct toegang hebben tot internet zonder gebruik van een proxy. 
2. Zorg ervoor dat Hyper-V-hosts en de VMM-server, indien van toepassing, toegang tot deze URL's: 

    [!INCLUDE [site-recovery-URLS](../../includes/site-recovery-URLS.md)]
    
3. Zorg ervoor dat:
    - Alle IP-adressen gebaseerde firewallregels moeten communicatie met Azure toestaan.
    - Sta de [IP-adresbereiken voor Azure Datacenter](https://www.microsoft.com/download/confirmation.aspx?id=41653) en de HTTPS-poort (443) toe.
    - IP-adresbereiken voor de Azure-regio van uw abonnement en voor VS-West (gebruikt voor beheer en de identiteit van toegangsbeheer) toestaan.


## <a name="prepare-to-connect-to-azure-vms-after-failover"></a>Voorbereiden op het verbinden met virtuele Azure-machines na een failover

Tijdens een failover-scenario mogelijk u wilt verbinding maken met uw gerepliceerde on-premises netwerk.

Als u wilt verbinding maken met Windows-VM's met RDP na een failover, het volgende doen:

1. Als u wilt openen via het internet, schakelt u RDP op de lokale virtuele machine voordat failover wordt uitgevoerd. Zorg ervoor dat TCP en UDP-regels worden toegevoegd voor de **openbare** profiel, en dat RDP is toegestaan in **Windows Firewall** > **Apps toegestaan** voor alle profielen.
2. Als u wilt openen via site-naar-site VPN, schakelt u RDP in op de on-premises machine. RDP moet worden toegestaan de **Windows Firewall** -> **toegestane apps en functies** voor **domein- en persoonlijke** netwerken.
   Controleer of de SAN-beleid van het besturingssysteem is ingesteld op **OnlineAll**. [Meer informatie](https://support.microsoft.com/kb/3031135). Er moet geen Windows-updates in behandeling op de virtuele machine wanneer u een failover activeren. Als er, kunt u niet aanmelden met de virtuele machine totdat de update is voltooid.
3. Controleer op de Windows Azure virtuele machine na een failover **opstarten diagnostics** om een schermopname van de virtuele machine weer te geven. Als u geen verbinding maken, Controleer of de virtuele machine wordt uitgevoerd en deze [tips voor probleemoplossing](http://social.technet.microsoft.com/wiki/contents/articles/31666.troubleshooting-remote-desktop-connection-after-failover-using-asr.aspx).


## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Herstel na noodgevallen naar Azure instellen voor Hyper-V-machines](tutorial-hyper-v-to-azure.md)
> [herstel na noodgevallen naar Azure instellen voor Hyper-V-machines in VMM-clouds](tutorial-hyper-v-vmm-to-azure.md)
