---
title: bestand opnemen
description: bestand opnemen
services: digital-twins
author: kingdomofends
ms.service: digital-twins
ms.topic: include
ms.date: 12/20/2018
ms.author: adgera
ms.custom: include file
ms.openlocfilehash: 40ab53c941a7ac619ebb09d381a4ae0450f26e8b
ms.sourcegitcommit: 295babdcfe86b7a3074fd5b65350c8c11a49f2f1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/27/2018
ms.locfileid: "53795882"
---
De `objectIdType` (of **object-id-type**) verwijst naar het type van de identiteit die wordt toegewezen aan een rol. Naast de `DeviceId` en `UserDefinedFunctionId` typen, typen van object-id's komen overeen met de eigenschappen van Azure Active Directory-objecten.

De volgende tabel bevat de ondersteunde object-id-typen in Azure digitale dubbels:

| Type | Description |
| --- | --- |
| UserId | Een rol aan een gebruiker toegewezen. |
| DeviceId | Een rol toewijst aan een apparaat. |
| Domeinnaam | Wijst een rol toe aan een domein. Elke gebruiker met de opgegeven domeinnaam is de toegangsrechten van de bijbehorende rol. |
| TenantId | Een rol toegewezen aan een tenant. Elke gebruiker die tot de opgegeven behoort Azure AD-tenant-ID heeft de toegangsrechten van de bijbehorende rol. |
| servicePrincipalId | Wijst een rol toe aan een service-principal-object-ID. |
| UserDefinedFunctionId | Een rol toegewezen aan een gebruiker gedefinieerde functie (UDF's). |