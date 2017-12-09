---
title: Azure API management-beleid voorbeeld - Shared Access Signature genereren | Microsoft Docs
description: Azure API management-beleid-voorbeeld - laat zien hoe u genereren Shared Access Signature expressies gebruiken en de aanvraag doorsturen naar Azure storage met herschrijven-uri-beleid...
services: api-management
documentationcenter: 
author: juliako
manager: cfowler
editor: 
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/13/2017
ms.author: apimpm
ms.openlocfilehash: 9b0d37e4f7930389d3399e51de905db2b2ce8c27
ms.sourcegitcommit: b07d06ea51a20e32fdc61980667e801cb5db7333
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/08/2017
---
# <a name="generate-shared-access-signature"></a>Shared Access Signature genereren

In dit artikel wordt een Azure API management-beleid-voorbeeldtoepassing die u laat hoe u zien voor het genereren van [Shared Access Signature](https://docs.microsoft.com/azure/storage/storage-dotnet-shared-access-signature-part-1) expressies gebruiken en de aanvraag doorsturen naar Azure storage met herschrijven-uri-beleid. Volg de stappen in wilt instellen of bewerken van een beleid code, [instellen of bewerken van een beleid](../set-edit-policies.md). Zie voor andere voorbeelden [beleid voorbeelden](../policy-samples.md).

## <a name="policy"></a>Beleid

Plak de code in de **inkomende** blok.

[!code-xml[Main](../../../api-management-policy-samples/Snippets/Generate Shared Access Signature and forward request to Azure storage.policy.xml)]

## <a name="next-steps"></a>Volgende stappen

Meer informatie over APIM beleid:

+ [Transformatiebeleid](../api-management-transformation-policies.md)
+ [Voorbeelden van beleid](../policy-samples.md)

