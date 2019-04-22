---
author: alkohli
ms.service: databox
ms.topic: include
ms.date: 04/16/2019
ms.author: alkohli
ms.openlocfilehash: 23587e617c62cfe4aa24256330c5f7673dd1c674
ms.sourcegitcommit: c3d1aa5a1d922c172654b50a6a5c8b2a6c71aa91
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/17/2019
ms.locfileid: "59684418"
---
Uw apparaat is gekoppeld aan een opslagaccount dat wordt gebruikt als een doel voor uw gegevens in Azure. Toegang tot het opslagaccount dat wordt bepaald door het abonnement en twee 512-bits opslag toegang tot sleutels die zijn gekoppeld aan dit account.

Een van de sleutels wordt gebruikt voor verificatie wanneer de gegevens in het Edge-apparaat toegang heeft tot het opslagaccount. De andere sleutel is ondergebracht in reserveren, zodat u kunt de sleutels periodiek draaien.

Uit veiligheidsoverwegingen vereisen veel datacenters rouleren van de sleutel. U wordt aangeraden dat u deze aanbevolen procedures voor sleutelroulatie volgt:

- De sleutel van uw opslagaccount is vergelijkbaar met het hoofdwachtwoord voor uw opslagaccount. Uw accountsleutel zorgvuldig te beschermen. Niet distribueren van het wachtwoord aan andere gebruikers, hard foutcode of sla deze ergens als tekst zonder opmaak die toegankelijk is voor anderen.
- [Accountsleutel](../articles/storage/common/storage-account-manage.md#regenerate-access-keys) met behulp van de Azure-portal als u denkt het dat mogelijk zijn aangetast.
- Uw Azure-beheerder moet periodiek, wijzigen of de primaire of secundaire sleutel opnieuw genereren met behulp van de sectie voor opslag van de Azure-portal voor rechtstreekse toegang tot het opslagaccount.