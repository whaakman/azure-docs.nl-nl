---
title: 'Azure API management-beleid steekproef: machtig aanvraag met behulp van externe authorizer | Microsoft Docs'
description: Azure API management-beleid - ziet u hoe autoriseren-aanvragen via externe authorizer encapsulating van een aangepaste of verouderde verificatie/autorisatie-logica.
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
ms.openlocfilehash: 7d172a40f2aad65b595026fc656634060a1f7193
ms.sourcegitcommit: d8ffb4a8cef3c6df8ab049a4540fc5e0fa7476ba
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/20/2018
ms.locfileid: "36284869"
---
# <a name="authorize-requests-using-external-authorizer"></a>-Aanvragen via externe authorizer autoriseren

Dit artikel ziet een Azure API management-beleid-voorbeeldtoepassing die u laat hoe u API-toegang beveiligen zien met behulp van een externe authorizer encapsulating aangepaste verificatie/autorisatie logica. Volg de stappen in wilt instellen of bewerken van een beleid code, [instellen of bewerken van een beleid](../set-edit-policies.md). Zie voor andere voorbeelden [beleid voorbeelden](../policy-samples.md).

## <a name="policy"></a>Beleid

Plak de code in de **inkomende** blok.

[!code-xml[Main](../../../api-management-policy-samples/examples/Authorize requests using external authorizer.policy.xml)]

## <a name="next-steps"></a>Volgende stappen

Meer informatie over APIM beleid:

+ [Beperkingen toegangsbeleid](../api-management-access-restriction-policies.md)
+ [Voorbeelden van beleid](../policy-samples.md)