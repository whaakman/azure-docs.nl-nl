---
title: Aan de slag met Azure AD v2 Windows-bureau blad-config | Microsoft Docs
description: Hoe een Windows Desktop .NET (XAML)-toepassing een toegangs token kan ophalen en een API kan aanroepen die wordt beveiligd door Azure Active Directory v2-eind punt.
services: active-directory
documentationcenter: dev-center-name
author: andretms
manager: CelesteDG
editor: ''
ms.assetid: 820acdb7-d316-4c3b-8de9-79df48ba3b06
ms.service: active-directory
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 05/09/2017
ms.author: ryanwi
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: a058024f8d6bdf7399e222c134f9f24c4ddffee8
ms.sourcegitcommit: bc3a153d79b7e398581d3bcfadbb7403551aa536
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/06/2019
ms.locfileid: "68835556"
---
# <a name="add-the-applications-registration-information-to-your-app"></a>De registratie gegevens van de toepassing toevoegen aan uw app
In deze stap moet u de toepassings-id toevoegen aan uw project.

1.  Open `App.xaml.cs` en vervang de regel met het `ClientId` volgende:

```csharp
private static string ClientId = "[Enter the application Id here]";
```

### <a name="what-is-next"></a>Wat is de volgende?

[!INCLUDE [Test and Validate](../../../../includes/active-directory-develop-guidedsetup-windesktop-test.md)]
