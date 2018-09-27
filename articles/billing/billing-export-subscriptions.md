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
ms.date: 06/15/2018
ms.author: cwatson
ms.openlocfilehash: 5c32b90c8a291ff744b4894af12f8d623cb95137
ms.sourcegitcommit: d1aef670b97061507dc1343450211a2042b01641
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/27/2018
ms.locfileid: "47391401"
---
# <a name="export-and-view-your-top-level-subscription-information"></a>Exporteren en uw abonnementsgegevens op het hoogste niveau weergeven
Als u wilt weergeven van de set van abonnement-id's die zijn gekoppeld aan uw gebruikersreferenties [downloaden van een .json-bestand met gegevens van uw abonnement van het Azure-Accountcentrum](http://account.azure.com/subscriptions/download).

[!INCLUDE [gdpr-dsr-and-stp-note](../../includes/gdpr-dsr-and-stp-note.md)]

De gedownloade .json-bestand bevat de volgende informatie:
- E-mailadres: Het e-mailadres dat is gekoppeld aan uw account.
- De PUID: De unieke id die is gekoppeld aan uw factureringsrekening.
- SubscriptionIds: Een lijst met abonnementen die deel uitmaken van uw account, geïnventariseerd door abonnements-ID.

### <a name="subscriptionsjson-sample"></a>Subscriptions.JSON voorbeeld
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
