---
title: bestand opnemen
description: bestand opnemen
services: active-directory
documentationcenter: dev-center-name
author: jmprieur
manager: mtillman
editor: ''
ms.service: active-directory
ms.devlang: na
ms.topic: include
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 09/17/2018
ms.author: jmprieur
ms.custom: include file
ms.openlocfilehash: dd74736734caddfcfb32a74a073e649c144abeb3
ms.sourcegitcommit: 0f54b9dbcf82346417ad69cbef266bc7804a5f0e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/26/2018
ms.locfileid: "50142467"
---
## <a name="add-the-applications-registration-information-to-your-app"></a>De registratiegegevens van de toepassing toevoegen aan uw app

In deze stap moet u de toepassings-Id toevoegen aan uw project.

1. Open `App.xaml.cs` en vervang de regel die de `ClientId` met:

```csharp
private static string ClientId = "[Enter the application Id here]";
```
