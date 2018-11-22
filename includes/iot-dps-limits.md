---
author: rothja
ms.service: billing
ms.topic: include
ms.date: 11/09/2018
ms.author: jroth
ms.openlocfilehash: 62f5c1a1f6184feddad7f2b57bcfc1c4bb5bc339
ms.sourcegitcommit: 8d88a025090e5087b9d0ab390b1207977ef4ff7c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/21/2018
ms.locfileid: "52279563"
---
De volgende tabel bevat de limieten die betrekking hebben op IoT Hub Device Provisioning Service-resources:

| Resource | Limiet |
| --- | --- |
| Maximale Device Provisioning Services per Azure-abonnement | 10 |
| Maximum aantal inschrijvingen | 500,000 |
| Maximum aantal registraties | 500,000 |
| Maximum aantal Registratiegroepen | 100 |
| Maximum aantal CA 's | 25 |

> [!NOTE]
> U kunt contact opnemen met [Microsoft Support](https://azure.microsoft.com/support/options/) te verhogen van het aantal exemplaren in uw abonnement.

> [!NOTE]
> U kunt contact opnemen met [Microsoft Support](https://azure.microsoft.com/support/options/) om het aantal registraties en rapporten op uw provisioning-service te vergroten.

De Device Provisioning Service vertraagt aanvragen wanneer de volgende quota worden overschreden:

| Vertragen | Waarde per eenheid |
| --- | --- |
| Bewerkingen | 200/min/service |
| Apparaatregistraties | 200/min/service |
| Pollingbewerking van apparaat | 5 / 10sec/apparaat |
