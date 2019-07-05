---
title: Verbinding maken met een Windows-VM met behulp van Azure bastionhost | Microsoft Docs
description: In dit artikel leert u hoe u verbinding maakt met een virtuele Machine in Azure waarop Windows wordt uitgevoerd met behulp van Azure bastionhost.
services: bastion
author: cherylmc
ms.service: bastion
ms.topic: conceptual
ms.date: 06/03/2019
ms.author: cherylmc
ms.openlocfilehash: 376b7042a513dd50647dc8f88bf1de70f65bb21c
ms.sourcegitcommit: f811238c0d732deb1f0892fe7a20a26c993bc4fc
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/29/2019
ms.locfileid: "67478408"
---
# <a name="connect-to-a-windows-virtual-machine-using-azure-bastion-preview"></a>Verbinding maken met een Windows virtuele machine met behulp van Azure bastionhost (Preview)

In dit artikel leest u hoe veilig en naadloos RDP naar uw Windows-VM's in een virtuele Azure-netwerk wilt met behulp van Azure bastionhost. U kunt rechtstreeks verbinding maken met een VM vanuit Azure Portal. Bij gebruik van Azure Bastion hebben VM's geen client, agent of aanvullende software nodig. Zie voor meer informatie over Azure bastionhost, de [overzicht](bastion-overview.md).

> [!IMPORTANT]
> Deze openbare preview-versie wordt aangeboden zonder service level agreement en wordt niet aanbevolen voor productieworkloads. Bepaalde functies worden mogelijk niet ondersteund, zijn mogelijk beperkt of zijn mogelijk niet beschikbaar in alle Azure-locaties. Raadpleeg voor meer informatie de [aanvullende gebruiksrechtovereenkomst voor Microsoft Azure-previews](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).
>

## <a name="before-you-begin"></a>Voordat u begint

Zorg ervoor dat u een Azure-bastionhost voor het virtuele netwerk waarin de virtuele machine zich bevindt hebt ingesteld. Zie voor meer informatie, [maken van een Azure-bastionhost](bastion-create-host-portal.md). Zodra de service van de Bastionomgeving is ingericht en geïmplementeerd in uw virtuele netwerk, kunt u deze kunt gebruiken om te verbinden met alle virtuele machines in dit virtuele netwerk. In deze Preview-versie bastionhost wordt ervan uitgegaan dat u van RDP gebruikmaakt verbinding maken met een Windows-VM en SSH verbinding maken met uw virtuele Linux-machines. Zie voor meer informatie over de verbinding met een Linux VM [verbinding maken met een VM - Linux](bastion-connect-vm-ssh.md).

Als u wilt een verbinding maakt, zijn de volgende rollen zijn vereist:

* Rol van lezer voor de virtuele machine
* Rol van lezer voor de NIC met particuliere IP-adres van de virtuele machine
* Rol van lezer voor de Bastionomgeving voor Azure-resource

## <a name="rdp"></a>Verbinding maken met behulp van RDP

1. Gebruik [deze koppeling](https://aka.ms/BastionHost) de preview-portal-pagina voor Azure bastionhost openen. Navigeer naar de virtuele machine die u verbinding maken wilt met en klik vervolgens op **Connect**. De virtuele machine moet een Windows virtuele machine bij het gebruik van een RDP-verbinding.

    ![Virtuele machine verbinding maken](./media/bastion-connect-vm-rdp/connect.png)

1. Nadat u op verbinden klikt, verschijnt er een zijbalk met drie tabbladen: RDP, SSH en bastionhost. Als de Bastionomgeving is ingericht voor het virtuele netwerk, is het tabblad bastionhost standaard actief. Als u nog niet hebt bastionhost voor het virtuele netwerk ingericht, kunt u klikken op de koppeling voor het configureren van de Bastionomgeving. Zie voor configuratie-instructies, [configureren bastionhost](bastion-create-host-portal.md). Als u niet ziet **bastionhost** weergegeven, u hebt niet geopend de preview-portal. Open de portal voor gebruik van deze [preview koppeling](https://aka.ms/BastionHost).

    ![Virtuele machine verbinding maken](./media/bastion-connect-vm-rdp/bastion.png)

1. Klik op het tabblad bastionhost, de gebruikersnaam en het wachtwoord voor uw virtuele machine, **Connect**. De RDP-verbinding met deze virtuele machine via bastionhost wordt rechtstreeks in Azure portal (via HTML5) met behulp van poort 443 en de service van de Bastionomgeving geopend.

    ![Virtuele machine verbinding maken](./media/bastion-connect-vm-rdp/443rdp.png)
 
## <a name="next-steps"></a>Volgende stappen

Lees de [bastionhost Veelgestelde vragen](bastion-faq.md)
