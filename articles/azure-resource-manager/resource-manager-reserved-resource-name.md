---
title: Azure resource naam fouten gereserveerd | Microsoft Docs
description: Beschrijft hoe u fouten oplossen bij het opgeven van de naam van een resource met een gereserveerd woord.
services: azure-resource-manager
documentationcenter: 
author: tfitzmac
manager: timlt
editor: 
ms.service: azure-resource-manager
ms.workload: multiple
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: support-article
ms.date: 11/08/2017
ms.author: tomfitz
ms.openlocfilehash: 2f06b7bc65ea309bc5374b41f402f8e7bc4d2a38
ms.sourcegitcommit: adf6a4c89364394931c1d29e4057a50799c90fc0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/09/2017
---
# <a name="resolve-reserved-resource-name-errors"></a>Gereserveerde bron naam fouten oplossen

In dit artikel beschrijft de fout die u tegenkomt bij het implementeren van een resource met een gereserveerd woord in de naam ervan.

## <a name="symptom"></a>Symptoom

Bij het implementeren van een resource die beschikbaar is via een openbaar eindpunt, verschijnt de volgende fout:

```
Code=ReservedResourceName;
Message=The resource name <resource-name> or a part of the name is a trademarked or reserved word.
```

## <a name="cause"></a>Oorzaak

Resources met een openbaar eindpunt een gereserveerde woorden of merken in de naam niet gebruiken.

De volgende woorden zijn gereserveerd:

* TOEGANG
* AZURE
* BING
* BIZSPARK
* BIZTALK
* CORTANA
* DOOR DIRECTX
* DOTNET
* DYNAMICS
* EXCEL
* EXCHANGE
* FOREFRONT
* GROOVE
* HOLOLENS
* HYPER-V
* KINECT
* LYNC
* MSDN
* O365
* OFFICE
* OFFICE365
* ONEDRIVE
* ONENOTE
* OUTLOOK
* POWERPOINT
* SHAREPOINT
* SKYPE
* VISIO
* VISUALSTUDIO

De volgende tekst kunnen niet worden gebruikt als een woord of een subtekenreeks in de naam:

* AANMELDING
* MICROSOFT
* WINDOWS
* XBOX

## <a name="solution"></a>Oplossing

Geef een naam die geen van een van de gereserveerde woorden gebruikmaakt.