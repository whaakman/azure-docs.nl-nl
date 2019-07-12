---
title: bestand opnemen
description: bestand opnemen
services: functions
author: ggailey777
manager: cfowler
ms.service: azure-functions
ms.topic: include
ms.date: 05/17/2018
ms.author: tdykstra
ms.custom: include file
ms.openlocfilehash: c2fff707dcaafac69efcad3dbf33446a7b797396
ms.sourcegitcommit: f10ae7078e477531af5b61a7fe64ab0e389830e8
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/05/2019
ms.locfileid: "67608162"
---
### <a name="azure-storage-sdk-version-in-functions-1x"></a>Azure Storage-SDK-versie in de functies 1.x

In de functies 1.x, de opslag-triggers en bindingen versie 7.2.1 van de Azure Storage SDK gebruiken ([WindowsAzure.Storage](https://www.nuget.org/packages/WindowsAzure.Storage/7.2.1) NuGet-pakket). Als u naar een andere versie van de Storage-SDK verwijzen, en u op het type van de Storage-SDK in uw functiehandtekening verbindt, kan de Functions-runtime melden dat het kan geen binding met dat type. De oplossing is om te controleren of uw projectverwijzingen [WindowsAzure.Storage 7.2.1](https://www.nuget.org/packages/WindowsAzure.Storage/7.2.1).
