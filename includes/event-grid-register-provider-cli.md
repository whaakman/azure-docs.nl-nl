---
title: bestand opnemen
description: bestand opnemen
services: event-grid
author: tfitzmac
ms.service: event-grid
ms.topic: include
ms.date: 07/05/2018
ms.author: tomfitz
ms.custom: include file
ms.openlocfilehash: a7ed21d8246b618149aa0d116070a14b033d5370
ms.sourcegitcommit: ab3b2482704758ed13cccafcf24345e833ceaff3
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/06/2018
ms.locfileid: "37869641"
---
## <a name="enable-event-grid-resource-provider"></a>Event Grid-resourceprovicer inschakelen

Als u Event Grid in uw Azure-abonnement nog niet eerder hebt gebruikt, moet u mogelijk de Event Grid-resourceprovider registreren. Voer de volgende opdracht uit:

```azurecli-interactive
az provider register --namespace Microsoft.EventGrid
```

Het kan even duren voordat de registratie is voltooid. Voer de volgende opdracht uit om de status te controleren:

```azurecli-interactive
az provider show -n Microsoft.EventGrid --query "registrationState"
```

Wanneer `registrationState` `Registered` is, bent u klaar om door te gaan.