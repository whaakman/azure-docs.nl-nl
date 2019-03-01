---
title: Ondersteuning voor Avere vFXT - Azure inschakelen
description: Het inschakelen van ondersteuning voor uploads van Avere vFXT voor Azure
author: ekpgh
ms.service: avere-vfxt
ms.topic: conceptual
ms.date: 10/31/2018
ms.author: v-erkell
ms.openlocfilehash: fe096b2e2a75cc89e3ce5ef905d8e4c347cc153a
ms.sourcegitcommit: f7f4b83996640d6fa35aea889dbf9073ba4422f0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/28/2019
ms.locfileid: "56992461"
---
# <a name="enable-support-uploads"></a>Ondersteuningsuploads inschakelen

De vFXT Avere voor Azure kunt ondersteuningsgegevens over uw cluster automatisch uploaden. Deze uploads laten ondersteunend personeel bieden de best mogelijke klantervaring-service.

## <a name="steps-to-enable-uploads"></a>Stappen voor het uploaden van bestanden inschakelen

Volg deze stappen in het Configuratiescherm Avere ondersteuning te activeren. (Lezen [toegang tot het cluster vFXT](avere-vfxt-cluster-gui.md) voor meer informatie over Avere het Configuratiescherm te openen.)

1. Navigeer naar de **instellingen** tabblad bovenaan.
1. Klik op de **ondersteuning** koppelen aan de linkerkant en accepteer het privacybeleid.

   ![Schermopname die laat zien Avere het Configuratiescherm en pop-upvenster met bevestigen om het privacybeleid accepteren](media/avere-vfxt-privacy-policy.png)

1. Klik op het driehoekje aan de linkerkant van **klantgegevens** om uit te breiden, de sectie.
1. Klik op de **Revalidate uploadinformatie** knop.
1. Naam van de ondersteuning van het cluster instellen in **unieke clusternaam** -Zorg ervoor dat deze unieke identificatie van uw cluster ter ondersteuning van personeel.
1. Schakel de selectievakjes voor **statistieken bewaking**, **algemene gegevens uploaden**, en **Crash gegevens uploaden**.
1. Klik op **Submit**

   ![Schermafbeelding met voltooide klant info-sectie van de pagina instellingen ondersteuning](media/avere-vfxt-support-info.png)

1. Klik op het driehoekje aan de linkerkant van **Secure proactieve ondersteuning (SP's)** om uit te breiden, de sectie.
1. Schakel het selectievakje voor **inschakelen SP's koppeling**.
1. Klik op **Submit**

   ![Schermafbeelding met voltooide proactieve ondersteuning beveiligde sectie op de instellingenpagina voor ondersteuning](media/avere-vfxt-support-sps.png)

## <a name="next-steps"></a>Volgende stappen

Als u toevoegen van een wilt on-premises of in de cloud opslagsysteem aan het cluster, volg de instructies in bestaande [opslag configureren](avere-vfxt-add-storage.md). 

Als u klaar om te beginnen clients koppelen aan het cluster bent, lezen [koppelen van het cluster van Avere vFXT](avere-vfxt-mount-clients.md).