---
author: rothja
ms.service: billing
ms.topic: include
ms.date: 11/09/2018
ms.author: jroth
ms.openlocfilehash: 610f5fcf80db8ffa0c7207d459b98087cbb4773b
ms.sourcegitcommit: 087ee51483b7180f9e897431e83f37b08ec890ae
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/31/2019
ms.locfileid: "66455141"
---
De volgende tabel bevat de limieten die voor Azure IoT Hub Device Provisioning Service-resources gelden.

| Resource | Limiet |
| --- | --- |
| Maximum aantal apparaat inrichten van services per Azure-abonnement | 10 |
| Maximum aantal inschrijvingen | 1.000.000 |
| Maximum aantal registraties | 1.000.000 |
| Maximum aantal Registratiegroepen | 100 |
| Maximum aantal CA 's | 25 |
| Maximale grootte van bericht | 96 KB|

> [!NOTE]
> Als u wilt het aantal exemplaren in uw abonnement verhogen, neem contact op met [Microsoft Support](https://azure.microsoft.com/support/options/).

> [!NOTE]
> Als u wilt vergroten het aantal registraties en registraties voor uw provisioning-service, neem contact op met [Microsoft Support](https://azure.microsoft.com/support/options/).

De Device Provisioning Service vertraagt aanvragen wanneer de volgende quota worden overschreden.

| Vertragen | Waarde per eenheid |
| --- | --- |
| Bewerkingen | 200/min/service |
| Apparaatregistraties | 200/min/service |
| Pollingbewerking van apparaat | 5/10 sec/apparaat |
