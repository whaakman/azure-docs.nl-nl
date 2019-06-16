---
author: alkohli
ms.service: databox
ms.topic: include
ms.date: 04/16/2019
ms.author: alkohli
ms.openlocfilehash: 8a6ceee0f7db4b7b368dce1a28c8edc0b2b11180
ms.sourcegitcommit: 778e7376853b69bbd5455ad260d2dc17109d05c1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/23/2019
ms.locfileid: "67120563"
---
Voor gegevens in rust:

- BitLocker XTS-AES 256-bits versleuteling wordt gebruikt om lokale gegevens te beveiligen.
- Toegang tot gegevens die zijn opgeslagen in bestandsshares wordt beperkt.

    - SMB-clients die toegang hebben tot gegevens delen, moet de referenties van de gebruiker die is gekoppeld aan de share. Deze referenties worden gedefinieerd als de share is gemaakt.
    - IP-adressen van de NFS-clients die toegang hebben tot een share moeten worden toegevoegd wanneer de share is gemaakt.