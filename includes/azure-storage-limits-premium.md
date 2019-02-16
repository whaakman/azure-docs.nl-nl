---
title: bestand opnemen
description: bestand opnemen
services: storage
author: roygara
ms.service: storage
ms.topic: include
ms.date: 01/22/2019
ms.author: rogarana
ms.custom: include file
ms.openlocfilehash: 3d100ec08b7b6d70366e605daf9c67edc6ab3bf3
ms.sourcegitcommit: d2329d88f5ecabbe3e6da8a820faba9b26cb8a02
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/16/2019
ms.locfileid: "56331065"
---
Premium storage-accounts hebben de volgende schaalbaarheidsdoelen:

| Totale capaciteit | Totale bandbreedte voor een account voor lokaal redundante opslag |
| --- | --- | 
| Capaciteit van de schijf: 35 TB <br>Snapshot-capaciteit: 10 TB | Omhoog naar 50 gigabits per seconde voor binnenkomende<sup>1</sup> + uitgaande<sup>2</sup> |

<sup>1</sup> alle gegevens (aanvragen) die worden verzonden naar een opslagaccount

<sup>2</sup> alle gegevens (reacties) die worden ontvangen van een storage-account

Als u premium storage-accounts voor niet-beheerde schijven gebruikt en uw toepassing is groter dan de schaalbaarheidsdoelen van een enkel opslagaccount, is het raadzaam om te migreren naar managed disks. Als u niet migreren naar managed disks wilt, moet u uw toepassing gebruik meerdere opslagaccounts maken. Partities voor uw gegevens vervolgens over de storage-accounts. Bijvoorbeeld, als u koppelen 51-TB schijven op meerdere virtuele machines wilt, verdeeld over deze twee opslagaccounts. 35 TB is de limiet voor één premium storage-account. Zorg ervoor dat één premium storage-account nooit meer dan 35 TB ingerichte schijven heeft.