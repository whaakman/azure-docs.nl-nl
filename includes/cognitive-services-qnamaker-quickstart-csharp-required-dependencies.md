---
title: bestand opnemen
description: bestand opnemen
services: cognitive-services
author: diberry
manager: cgronlun
ms.service: cognitive-services
ms.component: qna-maker
ms.topic: include
ms.custom: include file
ms.date: 09/27/2018
ms.author: diberry
ms.openlocfilehash: c3081dc89740e4b97f773265ad598112e77af65f
ms.sourcegitcommit: 7bc4a872c170e3416052c87287391bc7adbf84ff
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/02/2018
ms.locfileid: "48019046"
---
Boven in **Program.cs** vervangt u de enige _using_-instructie door de volgende regels. U voegt op die manier de benodigde afhankelijkheden toe aan het project:

```csharp
using System;
using System.Collections.Generic;
using System.Linq;
using System.Net.Http;
using System.Net.Http.Headers;
using System.Text;
using System.Threading;
using System.Threading.Tasks;

// NOTE: Install the Newtonsoft.Json NuGet package.
using Newtonsoft.Json;
```