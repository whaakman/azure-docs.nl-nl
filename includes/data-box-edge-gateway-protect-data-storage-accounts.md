---
author: alkohli
ms.service: databox
ms.topic: include
ms.date: 04/16/2019
ms.author: alkohli
ms.openlocfilehash: 653c175a559f5c0b7dc551b396e91276332df20a
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/23/2019
ms.locfileid: "60754245"
---
Uw apparaat is gekoppeld aan een opslagaccount dat wordt gebruikt als een doel voor uw gegevens in Azure. Toegang tot het opslagaccount dat wordt bepaald door het abonnement en twee 512-bits opslag toegang tot sleutels die zijn gekoppeld aan dit account.

Een van de sleutels wordt gebruikt voor verificatie wanneer de gegevens in het Edge-apparaat toegang heeft tot het opslagaccount. De andere sleutel is ondergebracht in reserveren, zodat u kunt de sleutels periodiek draaien.

Uit veiligheidsoverwegingen vereisen veel datacenters rouleren van de sleutel. U wordt aangeraden dat u deze aanbevolen procedures voor sleutelroulatie volgt:

- De sleutel van uw opslagaccount is vergelijkbaar met het hoofdwachtwoord voor uw opslagaccount. Uw accountsleutel zorgvuldig te beschermen. Niet distribueren van het wachtwoord aan andere gebruikers, hard foutcode of sla deze ergens in tekst zonder opmaak die toegankelijk is voor anderen.
- [Accountsleutel](../articles/storage/common/storage-account-manage.md#regenerate-access-keys) via de Azure portal als u denkt deze dat kan worden aangetast.
- Uw Azure-beheerder moet periodiek wijzigen of de primaire of secundaire sleutel opnieuw genereren met behulp van de sectie voor opslag van Azure portal rechtstreeks toegang hebben tot het opslagaccount.