---
title: 'Kopiëren en plakken naar en van een virtuele machine: Azure bastionhost | Microsoft Docs'
description: In dit artikel leest hoe kopiëren en plakken naar en van een Azure-VM met behulp van de Bastionomgeving.
services: bastion
author: cherylmc
ms.service: bastion
ms.topic: conceptual
ms.date: 06/03/2019
ms.author: cherylmc
ms.openlocfilehash: 9d69d1a9fae258c9546a8c794fc0b0c68b952049
ms.sourcegitcommit: 156b313eec59ad1b5a820fabb4d0f16b602737fc
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/18/2019
ms.locfileid: "67191809"
---
# <a name="copy-and-paste-to-a-virtual-machine-azure-bastion-preview"></a>Kopieer en plak aan een virtuele machine: Azure bastionhost (Preview)

Dit artikel helpt u kopiëren en plakken van tekst naar en van virtuele machines bij het gebruik van Azure bastionhost. Voordat u met een virtuele machine werkt, controleert u of u de stappen hebt gevolgd [maken van een bastionhost](bastion-create-host-portal.md). Vervolgens verbinding maken met de virtuele machine die u werken wilt met behulp van [RDP](bastion-connect-vm-rdp.md) of [SSH](bastion-connect-vm-ssh.md).

> [!IMPORTANT]
> Deze openbare preview-versie wordt aangeboden zonder service level agreement en wordt niet aanbevolen voor productieworkloads. Bepaalde functies worden mogelijk niet ondersteund, zijn mogelijk beperkt of zijn mogelijk niet beschikbaar in alle Azure-locaties. Raadpleeg voor meer informatie de [aanvullende gebruiksrechtovereenkomst voor Microsoft Azure-previews](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).
>

Voor browsers die ondersteuning bieden voor de geavanceerde Klembord-API-toegang, kunt u kopiëren en plakken van tekst tussen uw lokale apparaat en de externe sessie op dezelfde manier als u kopiëren en plakken tussen toepassingen op uw lokale apparaat. Voor andere browsers, kunt u de werkset voor de Bastionomgeving Klembord toegang.

  ![Klembord toestaan](./media/bastion-vm-manage/allow.png)

Alleen tekst kopiëren/plakken wordt ondersteund. Voor directe kopiëren en plakken, uw browser wordt u mogelijk gevraagd voor Klembord toegang wanneer de sessie van de Bastionomgeving wordt geïnitialiseerd. **Toestaan dat** de webpagina wordt weergegeven voor toegang tot het Klembord.

## <a name="to"></a>Kopiëren naar een externe sessie

Nadat u verbinding met de virtuele machine met de [Azure-portal](https://aka.ms/BastionHost) voor de Preview-versie voor de Bastionomgeving:

1. Kopieer de tekstinhoud van het lokale apparaat naar lokale Klembord.
1. Tijdens de externe sessie start u de werkset voor de Bastionomgeving Klembord toegang door het selecteren van de twee pijlen. De pijlen bevinden zich op de links in het midden van de sessie.

    ![de werkset](./media/bastion-vm-manage/left.png)

    ![Klembord](./media/bastion-vm-manage/clipboard.png)

1. Normaal gesproken de gekopieerde tekst wordt automatisch weergegeven in het palet bastionhost kopiëren plakken. Als de tekst niet bevat, moet u vervolgens de tekst in het tekstgebied in het palet plakken.
1. Nadat de tekst in het tekstvak is, kunt u deze kunt plakken in de externe sessie.

    ![Plakken](./media/bastion-vm-manage/local.png)

## <a name="from"></a>Kopiëren van een externe sessie

Nadat u verbinding met de virtuele machine met de [Azure-portal](https://aka.ms/BastionHost) voor de Preview-versie voor de Bastionomgeving:

1. Kopieer de tekstinhoud van de externe sessie in externe Klembord (met Ctrl + C).

    ![de werkset](./media/bastion-vm-manage/remote.png)

1. Tijdens de externe sessie start u de werkset voor de Bastionomgeving Klembord toegang door het selecteren van de twee pijlen. De pijlen bevinden zich op de links in het midden van de sessie.

    ![Klembord](./media/bastion-vm-manage/clipboard2.png)

1. Normaal gesproken de gekopieerde tekst wordt automatisch weergegeven in het palet bastionhost kopiëren plakken. Als de tekst niet bevat, moet u vervolgens de tekst in het tekstgebied in het palet plakken.
1. Nadat de tekst in het tekstvak is, kunt u deze kunt plakken naar het lokale apparaat.

    ![Plakken](./media/bastion-vm-manage/local2.png)
 
## <a name="next-steps"></a>Volgende stappen

Lees de [bastionhost Veelgestelde vragen over](bastion-faq.md).