---
title: Ondersteuning voor Avere vFXT - Azure inschakelen
description: Het inschakelen van ondersteuning voor uploads van Avere vFXT voor Azure
author: ekpgh
ms.service: avere-vfxt
ms.topic: conceptual
ms.date: 10/31/2018
ms.author: v-erkell
ms.openlocfilehash: bc21e06b704bfe3d25132092efbbf23f342acb14
ms.sourcegitcommit: 6135cd9a0dae9755c5ec33b8201ba3e0d5f7b5a1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/31/2018
ms.locfileid: "50633908"
---
# <a name="enable-support-uploads"></a>Inschakelen van ondersteuning voor uploads

De vFXT Avere voor Azure kunt ondersteuningsgegevens over uw cluster automatisch uploaden. Deze uploads laten ondersteunend personeel bieden de best mogelijke klantervaring-service.

## <a name="steps-to-enable-uploads"></a>Stappen voor het uploaden van bestanden inschakelen

Volg deze stappen in het Configuratiescherm Avere ondersteuning te activeren. (Lezen [toegang tot het cluster vFXT](avere-vfxt-cluster-gui.md) voor meer informatie over Avere het Configuratiescherm te openen.)

1. Navigeer naar de **instellingen** tabblad bovenaan.
1. Klik op de **ondersteuning** koppelen aan de linkerkant en accepteer het privacybeleid.

   ![Schermafbeelding voor het bevestigen van de acceptatie van privacybeleid](media/avere-vfxt-privacy-policy.png)
1. Klik op het driehoekje aan de linkerkant van **klantgegevens** om uit te breiden, de sectie.
1. Naam van de ondersteuning van het cluster instellen in **unieke clusternaam** -Zorg ervoor dat deze unieke identificatie van uw cluster ter ondersteuning van personeel.
1. Schakel de selectievakjes voor **statistieken bewaking**, **algemene gegevens uploaden**, en **Crash gegevens uploaden**.
1. Klik op de **valideren uploadinformatie** knop.
1. Klik op **indienen**.
1. Klik op het driehoekje aan de linkerkant van **Secure proactieve ondersteuning (SP's)** om uit te breiden, de sectie.
1. Schakel het selectievakje voor **inschakelen SP's koppeling**.
1. Klik op **indienen**.

   ![Schermafbeelding met de stappen voor het inschakelen van ondersteuning](media/avere-vfxt-support-info-steps.png)


## <a name="next-steps"></a>Volgende stappen

Als u een on-premises opslag-systeem toevoegen aan het cluster wilt, volg de instructies in [opslag configureren](avere-vfxt-add-storage.md). 

Als u klaar om te beginnen clients koppelen aan het cluster bent, lezen [koppelen van het cluster van Avere vFXT](avere-vfxt-mount-clients.md).