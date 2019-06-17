---
title: Voor Azure gereserveerde resource naam fouten | Microsoft Docs
description: Beschrijft hoe u fouten oplossen bij het opgeven van de naam van een resource met een gereserveerd woord.
services: azure-resource-manager
documentationcenter: ''
author: tfitzmac
manager: timlt
editor: ''
ms.service: azure-resource-manager
ms.workload: multiple
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: troubleshooting
ms.date: 11/08/2017
ms.author: tomfitz
ms.openlocfilehash: 922389b7c6c1bb7ad1d9b8f6ec35ccc1c5656723
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "64683940"
---
# <a name="resolve-reserved-resource-name-errors"></a>Gereserveerde bron naam fouten oplossen

Dit artikel beschrijft de fout die u ondervindt bij het implementeren van een resource met een gereserveerd woord in de naam ervan.

## <a name="symptom"></a>Symptoom

Bij het implementeren van een resource die is beschikbaar via een openbaar eindpunt, wordt de volgende fout:

```
Code=ReservedResourceName;
Message=The resource name <resource-name> or a part of the name is a trademarked or reserved word.
```

## <a name="cause"></a>Oorzaak

Resources met een openbaar eindpunt gereserveerde woorden of handelsmerken in de naam niet gebruiken.

De volgende woorden zijn gereserveerd:

* TOEGANG
* AZURE
* BING
* BIZSPARK
* BIZTALK
* CORTANA
* DIRECTX
* DOTNET
* DYNAMICS
* EXCEL
* EXCHANGE
* FOREFRONT
* GROOVE
* HOLOLENS
* HYPERV
* KINECT
* LYNC
* MSDN
* O365
* OFFICE
* OFFICE 365
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