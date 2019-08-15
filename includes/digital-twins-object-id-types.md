---
title: bestand opnemen
description: bestand opnemen
services: digital-twins
author: kingdomofends
ms.service: digital-twins
ms.topic: include
ms.date: 08/12/2019
ms.author: v-adgera
ms.custom: include file
ms.openlocfilehash: 94baeb3d459b700cc95d88fb82f995957640aab6
ms.sourcegitcommit: 5b76581fa8b5eaebcb06d7604a40672e7b557348
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/13/2019
ms.locfileid: "69012107"
---
Het `objectIdType` (of **object-ID-type**) verwijst naar het type identiteit dat wordt gegeven aan een rol. Naast het- `DeviceId` en `UserDefinedFunctionId` -type komen object-id-typen overeen met de eigenschappen van Azure Active Directory-objecten.

De volgende tabel bevat de ondersteunde object-id-typen in azure Digital Apparaatdubbels:

| type | Description |
| --- | --- |
| UserId | Hiermee wijst u een rol toe aan een gebruiker. |
| DeviceId | Hiermee wijst u een rol toe aan een apparaat. |
| Naam | Hiermee wijst u een rol toe aan een domein naam. Elke gebruiker met de opgegeven domein naam heeft de toegangs rechten van de bijbehorende rol. |
| TenantId | Hiermee wijst u een rol toe aan een Tenant. Elke gebruiker die lid is van de opgegeven Azure AD-Tenant-ID heeft de toegangs rechten van de bijbehorende rol. |
| ServicePrincipalId | Hiermee wijst u een rol toe aan een Service-Principal object-ID. |
| UserDefinedFunctionId | Hiermee wijst u een rol toe aan een door de gebruiker gedefinieerde functie (UDF). |