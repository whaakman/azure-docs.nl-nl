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
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "66162105"
---
De `objectIdType` (of **object-id-type**) verwijst naar het type van de identiteit die wordt toegewezen aan een rol. Naast de `DeviceId` en `UserDefinedFunctionId` typen, typen van object-id's komen overeen met de eigenschappen van Azure Active Directory-objecten.

De volgende tabel bevat de ondersteunde object-id-typen in Azure digitale dubbels:

| Type | Description |
| --- | --- |
| Gebruikers-id | Een rol aan een gebruiker toegewezen. |
| DeviceId | Een rol toewijst aan een apparaat. |
| Domeinnaam | Wijst een rol toe aan een domein. Elke gebruiker met de opgegeven domeinnaam is de toegangsrechten van de bijbehorende rol. |
| TenantId | Een rol toegewezen aan een tenant. Elke gebruiker die tot de opgegeven behoort Azure AD-tenant-ID heeft de toegangsrechten van de bijbehorende rol. |
| ServicePrincipalId | Wijst een rol toe aan een service-principal-object-ID. |
| UserDefinedFunctionId | Een rol toegewezen aan een gebruiker gedefinieerde functie (UDF's). |