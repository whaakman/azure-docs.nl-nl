---
author: alkohli
ms.service: databox
ms.topic: include
ms.date: 02/07/2019
ms.author: alkohli
ms.openlocfilehash: ad7ceffed61665a729d4391b5f0ff2935375c253
ms.sourcegitcommit: d1c5b4d9a5ccfa2c9a9f4ae5f078ef8c1c04a3b4
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/08/2019
ms.locfileid: "55967298"
---
Volgende voorbehoud toepassen op gegevens zoals deze is verplaatst naar Azure.

- Het is raadzaam dat meer dan één apparaat niet tot dezelfde container moet schrijven.
- Hebt u een bestaande Azure-object (zoals een blob of een bestand) in de cloud met dezelfde naam als het object dat wordt gekopieerd, wordt het bestand in de cloud door apparaat overschreven.
- Een lege map voor de hiërarchie (zonder bestanden) gemaakt op basis van gedeelde mappen is niet geüpload naar de blob-containers.
- Voor grote bestanden, raden wij aan dat u robocopy omdat het opnieuw de kopieerbewerking op onregelmatige fouten probeert.
