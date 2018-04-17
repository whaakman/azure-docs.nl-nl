---
title: On-premises Hyper-V-server voorbereiden op herstel na noodgevallen van Hyper-V-machines naar Azure | Microsoft Docs
description: Informatie over het voorbereiden van de lokale Hyper-V-machines niet worden beheerd door System Center VMM voor noodherstel naar Azure met de Azure Site Recovery-service.
services: site-recovery
author: rayne-wiselman
ms.service: site-recovery
ms.topic: article
ms.date: 03/15/2018
ms.author: raynew
ms.custom: MVC
ms.openlocfilehash: 7e0219a662483ef123bdc2889a43dd3d93d23ac2
ms.sourcegitcommit: 9cdd83256b82e664bd36991d78f87ea1e56827cd
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/16/2018
---
# <a name="prepare-on-premises-hyper-v-servers-for-disaster-recovery-to-azure"></a>On-premises Hyper-V-servers voorbereiden op herstel na noodgevallen naar Azure

Deze zelfstudie laat zien hoe u uw on-premises Hyper-V-infrastructuur voorbereidt voor Hyper-V-machines repliceren naar Azure, ten behoeve van herstel na noodgevallen bij. Hyper-V-hosts kunnen worden beheerd door System Center Virtual Machine Manager (VMM), maar dit is niet vereist.  In deze zelfstudie leert u het volgende:

> [!div class="checklist"]
> * Vereisten voor Hyper-V en VMM-vereisten, indien van toepassing controleren.
> * Voorbereiden van VMM, indien van toepassing
> * Controleer of u toegang tot internet voor Azure-locaties
> * Virtuele machines voorbereiden, zodat u er toegang toe na een failover naar Azure

Dit is de tweede zelfstudie in de reeks. Zorg ervoor dat u [de Azure-onderdelen hebt ingesteld](tutorial-prepare-azure.md) zoals beschreven in de vorige zelfstudie.



## <a name="review-requirements-and-prerequisites"></a>Vereisten controleren en vereisten

Zorg ervoor dat Hyper-V-hosts en virtuele machines die voldoen aan de vereisten.

1. [Controleer of](hyper-v-azure-support-matrix.md#on-premises-servers) on-premises serververeisten.
2. [Controleer de](hyper-v-azure-support-matrix.md#replicated-vms) voor Hyper-V virtuele machines die u wilt repliceren naar Azure.
3. Controleer de Hyper-V-host [netwerken](hyper-v-azure-support-matrix.md#hyper-v-network-configuration); en de host en de Gast [opslag](hyper-v-azure-support-matrix.md#hyper-v-host-storage) ondersteuning voor lokale Hyper-V-hosts.
4. Controleer wat er na failover wordt ondersteund voor [Azure-netwerken](hyper-v-azure-support-matrix.md#azure-vm-network-configuration-after-failover), [-opslag](hyper-v-azure-support-matrix.md#azure-storage) en [-rekenkracht](hyper-v-azure-support-matrix.md#azure-compute-features).
5. De on-premises VM's die u voor Azure repliceert, moeten overeenstemmen met [Azure VM-vereisten](hyper-v-azure-support-matrix.md#azure-vm-requirements).


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

1. Voor de doeleinden van de zelfstudie is de eenvoudigste configuratie voor de Hyper-V-hosts en VMM-server direct toegang hebben tot internet zonder gebruik van een proxy. 
2. Zorg ervoor dat Hyper-V-hosts en de VMM-server, indien van toepassing, toegang tot deze URL's: 

    [!INCLUDE [site-recovery-URLS](../../includes/site-recovery-URLS.md)]
    
3. Als u bent toegangsbeheer op basis van IP-adres, zorg ervoor dat:
    - IP-adressen gebaseerde firewallregels verbinding kunnen maken met [Azure Datacenter IP-adresbereiken](https://www.microsoft.com/download/confirmation.aspx?id=41653), en de poort voor HTTPS (443).
    - IP-adresbereiken voor de Azure-regio van uw abonnement en voor VS-West (gebruikt voor beheer en de identiteit van toegangsbeheer) toestaan.


## <a name="prepare-to-connect-to-azure-vms-after-failover"></a>Voorbereiden op het verbinden met virtuele Azure-machines na een failover

Tijdens een failover-scenario mogelijk u wilt verbinding maken met uw gerepliceerde on-premises netwerk.

Als u wilt verbinding maken met Windows-VM's met RDP na een failover, als volgt toegang toestaan:

1. Om toegang te krijgen via het internet, schakelt u voor de failover RDP in op de on-premises VM. Zorg ervoor dat TCP- en UDP-regels zijn toegevoegd voor het profiel **Openbaar** en dat RDP is toegestaan in **Windows Firewall** > **Toegestane apps** voor alle profielen.
2. Voor toegang via VPN tussen sites schakelt u RDP in op de on-premises computer. RDP moet toegestaan zijn in de **Windows Firewall** -> **Toegestane apps en onderdelen** voor **Domein en Privé**-netwerken.
   Controleer of het SAN-beleid van het besturingssysteem is ingesteld op **OnlineAll**. [Meer informatie](https://support.microsoft.com/kb/3031135). Er mogen geen Windows-updates in behandeling zijn op de VM wanneer u een failover activeert. Als die er zijn, kunt u pas weer bij de virtuele machine inloggen als de update is voltooid.
3. Controleer na een failover **Diagnostische gegevens over opstarten** op de Windows Azure VM om een schermopname van de VM weer te geven. Als u geen verbinding kunt maken, controleer dan of de VM actief is en bekijk deze [tips voor het oplossen van problemen](http://social.technet.microsoft.com/wiki/contents/articles/31666.troubleshooting-remote-desktop-connection-after-failover-using-asr.aspx).

Na een failover, kunt u toegang tot virtuele Azure-machines met hetzelfde IP-adres als de gerepliceerde lokale virtuele machine of een ander IP-adres. [Meer informatie](concepts-on-premises-to-azure-networking.md) over het instellen van IP-adressering voor failover.

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Herstel na noodgevallen naar Azure instellen voor Hyper-V-machines](tutorial-hyper-v-to-azure.md)
> [herstel na noodgevallen naar Azure instellen voor Hyper-V-machines in VMM-clouds](tutorial-hyper-v-vmm-to-azure.md)
