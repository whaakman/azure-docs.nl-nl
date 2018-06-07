---
title: bestand opnemen
description: bestand opnemen
services: functions
author: tdykstra
manager: cfowler
ms.service: functions
ms.topic: include
ms.date: 05/17/2018
ms.author: tdykstra
ms.custom: include file
ms.openlocfilehash: 97c56b07833f7d93541bb0b3747889f5a50a8203
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/01/2018
ms.locfileid: "34675272"
---
### <a name="azure-storage-sdk-version-in-functions-1x"></a>Azure-opslag-SDK versie in functies 1.x

In de functies 1.x, de opslag triggers en bindingen versie 7.2.1 van de Azure-opslag-SDK gebruiken ([WindowsAzure.Storage](https://www.nuget.org/packages/WindowsAzure.Storage/7.2.1) NuGet-pakket). Als u verwijst naar een andere versie van de opslag-SDK en u op het type van de opslag-SDK in uw functiehandtekening verbonden, kan de runtime van Functions melden dat het type kan niet binden. De oplossing is om te controleren of uw projectverwijzingen [WindowsAzure.Storage 7.2.1](https://www.nuget.org/packages/WindowsAzure.Storage/7.2.1).
