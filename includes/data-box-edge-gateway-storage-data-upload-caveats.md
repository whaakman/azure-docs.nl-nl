---
author: alkohli
ms.service: databox
ms.topic: include
ms.date: 02/14/2019
ms.author: alkohli
ms.openlocfilehash: 8bc4b78d262cf5f30076e90b40b92c4f3237924a
ms.sourcegitcommit: d2329d88f5ecabbe3e6da8a820faba9b26cb8a02
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/16/2019
ms.locfileid: "56334045"
---
Volgende voorbehoud toepassen op gegevens zoals deze is verplaatst naar Azure.

- Het is raadzaam dat meer dan één apparaat niet tot dezelfde container moet schrijven.
- Hebt u een bestaande Azure-object (zoals een blob of een bestand) in de cloud met dezelfde naam als het object dat wordt gekopieerd, wordt het bestand in de cloud door apparaat overschreven.
- Een lege map voor de hiërarchie (zonder bestanden) gemaakt op basis van gedeelde mappen is niet geüpload naar de blob-containers.
- Voor grote bestanden, raden wij aan dat u robocopy omdat het opnieuw de kopieerbewerking op onregelmatige fouten probeert.
- Als de share die zijn gekoppeld aan de Azure storage-container geüpload blobs die niet overeenkomen met het type van de blobs die zijn gedefinieerd voor de share op het moment dat wordt gemaakt, worden klikt u vervolgens deze blobs niet bijgewerkt. U kunt bijvoorbeeld een blok-blob-share maken op het apparaat. Koppelt de share aan een bestaande cloudcontainer met pagina-blobs. Vernieuw deze share om de bestanden te downloaden. Sommige van de vernieuwde bestanden die al zijn opgeslagen als pagina-blobs in de cloud wijzigen. U ziet fouten uploaden.
