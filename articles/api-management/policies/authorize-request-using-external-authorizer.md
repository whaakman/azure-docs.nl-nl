---
title: Azure API management-beleid voorbeeld - aanvraag met behulp van externe authorizer autoriseren | Microsoft Docs
description: Azure API management-beleid-voorbeeld - laat zien hoe aanvragen via externe authorizer encapsulating van een aangepaste en verouderde verificatie/autorisatie-logica te autoriseren.
services: api-management
documentationcenter: ''
author: vladvino
manager: cfowler
editor: ''
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/06/2018
ms.author: apimpm
ms.openlocfilehash: 70f43a058cfd3818dae1ccfa4b222a7d0d740aee
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/24/2018
ms.locfileid: "46979400"
---
# <a name="authorize-requests-using-external-authorizer"></a>Toestaan van aanvragen met behulp van externe authorizer

In dit artikel wordt een Azure API management-beleid voorbeeld die laat hoe u API-toegang beveiligen zien met behulp van een externe authorizer encapsulating aangepaste verificatie/autorisatie logica. Als u wilt instellen of bewerken van een beleid voor code, volg de stappen [instellen of bewerken van een beleid](../set-edit-policies.md). Zie voor meer voorbeelden [beleid voorbeelden](../policy-samples.md).

## <a name="policy"></a>Beleid

Plak de code in de **inkomende** blokkeren.

[!code-xml[Main](../../../api-management-policy-samples/examples/Authorize requests using external authorizer.policy.xml)]

## <a name="next-steps"></a>Volgende stappen

Meer informatie over de APIM-beleid:

+ [Toegangsbeleid voor beperkingen](../api-management-access-restriction-policies.md)
+ [Voorbeelden van beleid](../policy-samples.md)
