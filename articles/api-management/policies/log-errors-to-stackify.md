---
title: Azure API management-beleid voorbeeld - fouten verzenden Stackify voor logboekregistratie | Microsoft Docs
description: Azure API management-beleid-voorbeeld - ziet u hoe u een beleid voor fout logboekregistratie voor het verzenden van fouten naar Stackify voor logboekregistratie toevoegen...
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
ms.date: 10/13/2017
ms.author: apimpm
ms.openlocfilehash: 07cc83830fe2d467c611622bb66dfbb8c9429c2d
ms.sourcegitcommit: b0f39746412c93a48317f985a8365743e5fe1596
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/04/2018
ms.locfileid: "52864873"
---
# <a name="send-errors-to-stackify-for-logging"></a>Fouten verzenden naar Stackify voor logboekregistratie

In dit artikel bevat een voorbeeld van Azure API management-beleid die laat zien hoe u een beleid van de fout logboekregistratie voor het verzenden van fouten naar Stackify voor logboekregistratie toe te voegen. Als u wilt instellen of bewerken van een beleid voor code, volg de stappen [instellen of bewerken van een beleid](../set-edit-policies.md). Zie voor meer voorbeelden [beleid voorbeelden](../policy-samples.md).

## <a name="policy"></a>Beleid

Plak de code in de **bij fout** blokkeren.

[!code-xml[Main](../../../api-management-policy-samples/examples/Log errors to Stackify.policy.xml)]

## <a name="next-steps"></a>Volgende stappen

Meer informatie over de APIM-beleid:

+ [Transformatiebeleid](../api-management-transformation-policies.md)
+ [Voorbeelden van beleid](../policy-samples.md)

