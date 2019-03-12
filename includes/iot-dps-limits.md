---
author: rothja
ms.service: billing
ms.topic: include
ms.date: 11/09/2018
ms.author: jroth
ms.openlocfilehash: 1ae6c6ee59b66e6c61714c0ece9f306f1a885096
ms.sourcegitcommit: bd15a37170e57b651c54d8b194e5a99b5bcfb58f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/07/2019
ms.locfileid: "57553352"
---
De volgende tabel bevat de limieten die voor Azure IoT Hub Device Provisioning Service-resources gelden.

| Resource | Limiet |
| --- | --- |
| Maximum aantal apparaat inrichten van services per Azure-abonnement | 10 |
| Maximum aantal inschrijvingen | 500,000 |
| Maximum aantal registraties | 500,000 |
| Maximum aantal Registratiegroepen | 100 |
| Maximum aantal CA 's | 25 |

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
