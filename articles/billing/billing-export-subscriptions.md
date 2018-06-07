---
title: Uw Azure-abonnement bovenste niveau gegevens exporteren | Microsoft Docs
description: Hierin wordt beschreven hoe u alle Azure-abonnement id's die zijn gekoppeld aan uw account kunt bekijken.
keywords: ''
services: billing
documentationcenter: ''
author: adpick
manager: adpick
editor: ''
tags: billing
ms.service: billing
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 10/9/2017
ms.author: adpick
ms.openlocfilehash: b995b0c7154faab66a44bef1a7d74eeb8404e5c1
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/01/2018
ms.locfileid: "34654940"
---
# <a name="export-and-view-your-top-level-subscription-information"></a>Exporteren en uw abonnementsgegevens op het hoogste niveau weergeven
Als u wilt weergeven van de reeks abonnement-id's die zijn gekoppeld aan uw gebruikersreferenties [een .json-bestand met uw abonnementsgegevens downloaden van het Azure-Accountcentrum](http://account.azure.com/subscriptions/download).

[!INCLUDE [gdpr-dsr-and-stp-note](../../includes/gdpr-dsr-and-stp-note.md)]

Het gedownloade .json-bestand bevat de volgende informatie:
- E-mailadres: Het e-mailadres gekoppeld aan uw account.
- PUID: De unieke id die is gekoppeld aan uw factureringsrekening.
- SubscriptionIds: Een lijst met abonnementen die deel uitmaken van uw account, geïnventariseerd door abonnements-ID.

### <a name="subscriptionsjson-sample"></a>Voorbeeld van Subscriptions.JSON
~~~~
{
  "Email":"admin@contoso.com",
  "Puid":"00052xxxxxxxxxxx",
  "SubscriptionIds":[
    "38124d4d-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
    "7c8308f1-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
    "39a25f2b-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
    "52ec2489-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
    "e42384b2-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
    "90757cdc-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
  ]
}
~~~~
