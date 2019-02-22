---
author: alkohli
ms.service: databox
ms.topic: include
ms.date: 02/21/2019
ms.author: alkohli
ms.openlocfilehash: 8c87e14071b3bb40421ab655c172df739570e295
ms.sourcegitcommit: a4efc1d7fc4793bbff43b30ebb4275cd5c8fec77
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/21/2019
ms.locfileid: "56660020"
---
Volgende voorbehoud toepassen op gegevens zoals deze is verplaatst naar Azure.

- Het is raadzaam dat meer dan één apparaat niet tot dezelfde container moet schrijven.
- Hebt u een bestaande Azure-object (zoals een blob of een bestand) in de cloud met dezelfde naam als het object dat wordt gekopieerd, wordt het bestand in de cloud door apparaat overschreven.
- Een lege map voor de hiërarchie (zonder bestanden) gemaakt op basis van gedeelde mappen is niet geüpload naar de blob-containers.
- U kunt kopiëren van de gegevens met behulp van slepen en neerzetten met Verkenner of via de opdrachtregel. Als de cumulatieve grootte van bestanden worden gekopieerd groter dan 10 GB is, raden wij dat u een programma voor bulksgewijs kopiëren zoals Robocopy of rsync gebruiken. De hulpprogramma's voor bulksgewijs kopiëren opnieuw proberen de kopieerbewerking op onregelmatige fouten en extra flexibiliteit bieden.
- Als de share die zijn gekoppeld aan de Azure storage-container geüpload blobs die niet overeenkomen met het type van de blobs die zijn gedefinieerd voor de share op het moment dat wordt gemaakt, worden klikt u vervolgens deze blobs niet bijgewerkt. U kunt bijvoorbeeld een blok-blob-share maken op het apparaat. Koppelt de share aan een bestaande cloudcontainer met pagina-blobs. Vernieuw deze share om de bestanden te downloaden. Sommige van de vernieuwde bestanden die al zijn opgeslagen als pagina-blobs in de cloud wijzigen. U ziet fouten uploaden.
