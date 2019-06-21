---
title: Verbinding maken met een Linux-VM met behulp van Azure bastionhost | Microsoft Docs
description: In dit artikel leert u hoe u verbinding maakt met Linux-VM met behulp van Azure bastionhost.
services: bastion
author: cherylmc
ms.service: bastion
ms.topic: conceptual
ms.date: 06/03/2019
ms.author: cherylmc
ms.openlocfilehash: 572043598c71a400e154c5c2e9e6c2f1e9b4ab49
ms.sourcegitcommit: 156b313eec59ad1b5a820fabb4d0f16b602737fc
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/18/2019
ms.locfileid: "67191796"
---
# <a name="connect-using-ssh-to-a-linux-virtual-machine-using-azure-bastion-preview"></a>Verbinding maken met SSH naar een virtuele Linux-machine met behulp van Azure bastionhost (Preview)

In dit artikel ziet u hoe u aan veilig en naadloos SSH verbinding met uw virtuele Linux-machines in een Azure-netwerk. U kunt rechtstreeks verbinding maken met een VM vanuit Azure Portal. Bij gebruik van Azure Bastion hebben VM's geen client, agent of aanvullende software nodig. Zie voor meer informatie over Azure bastionhost, de [overzicht](bastion-overview.md).

Azure bastionhost kunt u verbinding maken met een virtuele Linux-machine via SSH. U kunt zowel de gebruikersnaam en wachtwoord en de SSH-sleutels gebruiken voor verificatie. U kunt verbinding maken met uw virtuele machine met SSH-sleutels met behulp van:

* Een persoonlijke sleutel die u handmatig invoeren
* Een bestand met de gegevens van de persoonlijke sleutel

De persoonlijke SSH-sleutel moet zich in een indeling die met begint `"-----BEGIN RSA PRIVATE KEY-----"` en eindigt met `"-----END RSA PRIVATE KEY-----"`.

> [!IMPORTANT]
> Deze openbare preview-versie wordt aangeboden zonder service level agreement en wordt niet aanbevolen voor productieworkloads. Bepaalde functies worden mogelijk niet ondersteund, zijn mogelijk beperkt of zijn mogelijk niet beschikbaar in alle Azure-locaties. Raadpleeg voor meer informatie de [aanvullende gebruiksrechtovereenkomst voor Microsoft Azure-previews](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).
>

## <a name="before-you-begin"></a>Voordat u begint

Zorg ervoor dat u een Azure-bastionhost voor het virtuele netwerk waarin de virtuele machine zich bevindt hebt ingesteld. Zie voor meer informatie, [maken van een Azure-bastionhost](bastion-create-host-portal.md). Zodra de service van de Bastionomgeving is ingericht en geïmplementeerd in uw virtuele netwerk, kunt u deze kunt gebruiken om te verbinden met alle virtuele machines in dit virtuele netwerk. In dit voorbeeld, wanneer u verbinding hebt gemaakt, met bastionhost wordt ervan uitgegaan dat u van RDP gebruikmaakt verbinding maken met een Windows-VM en SSH verbinding maken met uw virtuele Linux-machines.

Als u wilt een verbinding maakt, zijn de volgende rollen zijn vereist:

* Rol van lezer voor de virtuele machine
* Rol van lezer voor de NIC met particuliere IP-adres van de virtuele machine
* Rol van lezer voor de Bastionomgeving voor Azure-resource

## <a name="username"></a>Verbinding maken: Met behulp van de gebruikersnaam en wachtwoord


1. In de [Azure-portal](https://aka.ms/BastionHost) voor de Bastionomgeving Preview-versie, gaat u naar de virtuele machine die u wilt verbinden, en klik vervolgens op **Connect**. De virtuele machine moet een virtuele Linux-machine bij het gebruik van een SSH-verbinding.
1. Nadat u op verbinden klikt, verschijnt er een zijbalk met drie tabbladen: RDP, SSH en bastionhost. Als de Bastionomgeving is ingericht voor het virtuele netwerk, is het tabblad bastionhost standaard actief. Als u nog niet hebt bastionhost voor het virtuele netwerk ingericht, Zie [configureren bastionhost](bastion-create-host-portal.md). Als u niet ziet **bastionhost** weergegeven, u hebt niet geopend de preview-portal. Open de portal via [deze koppeling](https://aka.ms/BastionHost).

      ![Virtuele machine verbinding maken](./media/bastion-connect-vm-ssh/bastion.png)

1. Voer de gebruikersnaam en het wachtwoord voor SSH naar uw virtuele machine.
1. Klik op **Connect** knop na het invoeren van de sleutel.

## <a name="privatekey"></a>Verbinding maken: Voer handmatig een persoonlijke sleutel

1. In de [Azure-portal](https://aka.ms/BastionHost) voor de Bastionomgeving Preview-versie, gaat u naar de virtuele machine die u wilt verbinden, en klik vervolgens op **Connect**. De virtuele machine moet een virtuele Linux-machine bij het gebruik van een SSH-verbinding.
1. Nadat u op verbinden klikt, verschijnt er een zijbalk met drie tabbladen: RDP, SSH en bastionhost. Als de Bastionomgeving is ingericht voor het virtuele netwerk, is het tabblad bastionhost standaard actief. Als u nog niet hebt bastionhost voor het virtuele netwerk ingericht, Zie [configureren bastionhost](bastion-create-host-portal.md). Als u niet ziet **bastionhost** weergegeven, u hebt niet geopend de preview-portal. Open de portal via [deze koppeling](https://aka.ms/BastionHost).

      ![Virtuele machine verbinding maken](./media/bastion-connect-vm-ssh/bastion.png)

1. Voer de gebruikersnaam in en selecteer **SSH privésleutel**.
1. Voer uw persoonlijke sleutel in het tekstgebied **SSH privésleutel** (of plakt u deze rechtstreeks).
1. Klik op **Connect** knop na het invoeren van de sleutel.

## <a name="ssh"></a>Verbinding maken: Met behulp van een bestand met persoonlijke sleutel

1. In de [Azure-portal](https://aka.ms/BastionHost) voor de Bastionomgeving Preview-versie, gaat u naar de virtuele machine die u wilt verbinden, en klik vervolgens op **Connect**. De virtuele machine moet een virtuele Linux-machine bij het gebruik van een SSH-verbinding.

    ![Virtuele machine verbinding maken](./media/bastion-connect-vm-ssh/connect.png)

1. Nadat u op verbinden klikt, verschijnt er een zijbalk met drie tabbladen: RDP, SSH en bastionhost. Als de Bastionomgeving is ingericht voor het virtuele netwerk, is het tabblad bastionhost standaard actief. Als u nog niet hebt bastionhost voor het virtuele netwerk ingericht, Zie [configureren bastionhost](bastion-create-host-portal.md). Als u niet ziet **bastionhost** weergegeven, u hebt niet geopend de preview-portal. Open de portal via [deze koppeling](https://aka.ms/BastionHost).

    ![Virtuele machine verbinding maken](./media/bastion-connect-vm-ssh/bastion.png)

1. Voer de gebruikersnaam in en selecteer **SSH persoonlijke-sleutel van het lokale bestand**.
1. Klik op de **Bladeren** knop (het mappictogram in het lokale bestand).
1. Bladeren naar het bestand en klik vervolgens op **Open**.
1. Klik op **Connect** verbinding maken met de virtuele machine. Nadat u op wordt rechtstreeks verbinding maken, SSH aan deze virtuele machine geopend in de Azure-portal. Deze verbinding is via het HTML5 met behulp van poort 443 van de Bastionomgeving-service via de privé IP-adres van uw virtuele machine.

## <a name="next-steps"></a>Volgende stappen

Lees de [bastionhost Veelgestelde vragen](bastion-faq.md)