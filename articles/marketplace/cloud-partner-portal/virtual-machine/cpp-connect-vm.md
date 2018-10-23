---
title: Verbinding maken met uw Microsoft Azure op basis van virtuele machine | Microsoft Docs
description: Wordt uitgelegd hoe u verbinding maken met de nieuwe virtuele machine is gemaakt op Azure.
services: Azure, Marketplace, Cloud Partner Portal,
documentationcenter: ''
author: v-miclar
manager: Patrick.Butler
editor: ''
ms.assetid: ''
ms.service: marketplace
ms.workload: ''
ms.tgt_pltfrm: ''
ms.devlang: ''
ms.topic: article
ms.date: 10/19/2018
ms.author: pbutlerm
ms.openlocfilehash: fd68846b9144c3efcc71dec369d64119427758a3
ms.sourcegitcommit: 17633e545a3d03018d3a218ae6a3e4338a92450d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/22/2018
ms.locfileid: "49639679"
---
# <a name="connect-to-your-azure-based-virtual-machine"></a>Verbinding maken met uw virtuele machine van Azure

In dit artikel wordt uitgelegd hoe u verbinding maken met en meld u aan de virtuele machines (VM's) die u hebt gemaakt in Azure.  Nadat u verbinding hebt gemaakt, kunt u werken met de virtuele machine als u lokaal zijn aangemeld bij de host-server. 

## <a name="connect-to-a-windows-based-vm"></a>Verbinding maken met een Windows-VM

U gebruikt de extern bureaublad-client verbinding maken met de virtuele machine wordt gehost in Azure op basis van Windows.  De meeste versies van Windows bevatten systeemeigen ondersteuning voor remote desktop protocol (RDP).  Voor andere virtuele machines, vindt u meer informatie over de clients in [extern bureaublad-clients](https://docs.microsoft.com/windows-server/remote/remote-desktop-services/clients/remote-desktop-clients).  

Het volgende artikel wordt uitgelegd hoe u de ingebouwde ondersteuning voor Windows-RDP verbinding maken met uw virtuele machine: [hoe u verbinding maken met en meld u aan een virtuele Azure-machine waarop Windows wordt uitgevoerd bij](../../../virtual-machines/windows/connect-logon.md).  

>[!TIP]
> Krijgt u mogelijk waarschuwingen tijdens het proces zo dat het RDP-bestand van een onbekende uitgever is of dat de referenties van de gebruiker kunnen niet worden geverifieerd.  Het is veilig negeren van deze waarschuwingen.


## <a name="connect-to-a-linux-based-vm"></a>Verbinding maken met een Linux-VM

Verbind de VM op basis van Linux, moet u een secure shell (SSH)-protocol-client.  Deze beschrijving wordt gebruik van de gratis [PuTTY](https://www.ssh.com/ssh/putty/) SHH-terminal.

1. In de **virtuele machines** blade van de [Azure-portal](https://ms.portal.azure.com), selecteer de virtuele machine die u wilt verbinden.  
2. **Start** de virtuele machine als deze niet al wordt uitgevoerd.
3. Klik op de naam van de virtuele machine en opent de **overzicht** pagina.
4. Houd er rekening mee de openbare IP-adres en DNS-naam van uw virtuele machine.  (Als deze waarden niet zijn ingesteld, dan u moet [een netwerkinterface maken](https://docs.microsoft.com/azure/virtual-network/virtual-network-network-interface#create-a-network-interface)

   ![Overzicht van VM-instellingen](./media/publishvm_019.png)
 
5. Open de PuTTY-toepassing.  
6. Voer het IP-adres of de DNS-naam van uw virtuele machine in het dialoogvenster PuTTY-configuratie. 

   ![Instellingen voor puTTY terminal](./media/publishvm_020.png)
 
7. Klik op **Open** een PuTTY terminal te openen.  
8. Wanneer u wordt gevraagd, voert u de accountnaam en het wachtwoord van uw Linux-VM-account. 

   Als u verbindingsproblemen ondervindt, raadpleegt u de documentatie van uw SSH-client, bijvoorbeeld [hoofdstuk 10: veelvoorkomende foutberichten](https://www.ssh.com/ssh/putty/putty-manuals/0.68/Chapter10.html#errors).

Zie voor meer informatie, waaronder het toevoegen van een bureaublad met een ingerichte Linux-VM, [installeren en configureren van extern bureaublad verbinding maken met een Linux-VM in Azure](../../../virtual-machines/linux/use-remote-desktop.md).


## <a name="stop-unused-vms"></a>Niet-gebruikte virtuele machines stoppen
Azure-facturen voor de virtuele machine die als host fungeert als een virtuele machine wordt uitgevoerd *of niet-actieve*.  Daarom is het aanbevolen om te stoppen van virtuele machines die op dit moment niet worden gebruikt.  Bijvoorbeeld, testen, back-up, of buiten gebruik gestelde VM's zijn kandidaten voor afsluiten. Als u wilt een virtuele machine afsluit, kunt u de volgende stappen uitvoeren:

1. Op de **virtuele machines** blade, selecteer de virtuele machine die u wilt stoppen. 
2. Klik in de werkbalk aan de bovenkant van de pagina op de **stoppen** knop.

   ![Een VM stoppen](./media/publishvm_018.png)

Azure gestopt snel de virtuele machine in een proces genaamd *toewijzing is opgeheven*, die niet alleen het besturingssysteem op de virtuele machine wordt afgesloten, maar kunnen ook de hardware- en netwerkbronnen die eerder zijn ingericht voor deze.

Als u een gestopte virtuele machine later opnieuw te activeren wilt, selecteert u deze en klikt u op de **Start** knop.


## <a name="next-steps"></a>Volgende stappen

Nadat u extern verbonden zijn, bent u klaar om te [configureren van uw virtuele machine](./cpp-configure-vm.md).
