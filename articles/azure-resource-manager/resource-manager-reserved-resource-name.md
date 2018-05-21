---
title: Azure resource naam fouten gereserveerd | Microsoft Docs
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
ms.openlocfilehash: b91a53d17d64afb0a56f745505f10e8cabbc22cc
ms.sourcegitcommit: b6319f1a87d9316122f96769aab0d92b46a6879a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/20/2018
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