---
author: alkohli
ms.service: databox
ms.topic: include
ms.date: 04/16/2019
ms.author: alkohli
ms.openlocfilehash: d8570c0be6b64d4e289575ce3f3f1721c4a65074
ms.sourcegitcommit: c3d1aa5a1d922c172654b50a6a5c8b2a6c71aa91
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/17/2019
ms.locfileid: "59684428"
---
Voor de data-at-rest:

- Voor de data-at-rest, BitLocker XTS-AES-256-codering gebruikt om de lokale gegevens te beveiligen.
- De gegevens die zich in shares bevinden, is de toegang tot de shares beperkt.

    - Voor SMB-clients die toegang hebben tot de gegevens delen, moeten ze de referenties van de gebruiker die is gekoppeld aan de share. Deze referenties worden gedefinieerd op het moment dat de share gemaakt.
    - Voor NFS-clients die toegang hebben tot de shares, moeten de IP-adressen van de clients op het moment dat de share gemaakt worden toegevoegd.