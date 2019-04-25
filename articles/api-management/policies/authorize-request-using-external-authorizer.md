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
ms.openlocfilehash: 65ea8622187d0665e4680f4162ddff0bc01e6eb9
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/23/2019
ms.locfileid: "60306763"
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
