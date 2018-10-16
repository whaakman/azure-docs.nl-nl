---
title: Inzicht krijgen in digitale dubbels Azure apparaatconnectiviteit en -verificatie | Microsoft Docs
description: Gebruik Azure digitale dubbels om verbinding te verifiëren van apparaten
author: lyrana
manager: alinast
ms.service: digital-twins
services: digital-twins
ms.topic: conceptual
ms.date: 10/10/2018
ms.author: lyrana
ms.openlocfilehash: adfb4c369ea1b324da8562a5b0b245ebdecff602
ms.sourcegitcommit: 74941e0d60dbfd5ab44395e1867b2171c4944dbe
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/15/2018
ms.locfileid: "49324080"
---
# <a name="create-and-manage-role-assignments"></a>Maken en beheren van roltoewijzingen

Azure van digitale dubbels maakt gebruik van op rollen gebaseerd toegangsbeheer ([RBAC](./security-role-based-access-control.md)) voor het beheren van toegang tot bronnen.

De toewijzing van elke rol bevat:

* Een **object-id** (een Azure Active Directory-ID, een service-principal-object-ID of een domeinnaam).
* Een **object-id-type**.
* Een **roldefinitie-ID**.
* Een **ruimte pad**.
* (In de meeste gevallen) Azure Active Directory **tenant-ID**.

## <a name="role-definition-identifiers"></a>Rol van definitie-id 's

De onderstaande tabel ziet u wat kan worden verkregen door het opvragen van de sitesysteemrollen /-API:

| **Rol** | **ID** |
| --- | --- |
| Ruimte-beheerder | 98e44ad7-28d4-4007-853b-b9968ad132d1 |
| Gebruikerbeheerder| dfaac54c-f583-4dd2-b45d-8d4bbc0aa1ac |
| Apparaatbeheerder | 3cdfde07-bc16-40D9-bed3-66d49a8f52ae |
| Sleutel-beheerder | 5a0b1afc-e118-4068-969f-b50efb8e5da6 |
| Token-beheerder | 38a3bb21-5424-43b4-b0bf-78ee228840c3 |
| Gebruiker | b1ffdb77-c635-4E7E-ad25-948237d85b30 |
| Ondersteuning voor gespecialiseerde | 6e46958b-dc62-4e7c-990c-c3da2e030969 |
| Apparaat-installatieprogramma | b16dd9fe-4efe-467b-8c8c-720e2ff8817c |
| GatewayDevice | d4c69766-e9bd-4e61-BFC1-d8b6e686c7a8 |

## <a name="supported-objectidtypes"></a>Ondersteunde ObjectIdTypes

De ondersteunde `ObjectIdTypes` zijn:

* `UserId`
* `DeviceId`
* `DomainName`
* `TenantId`
* `ServicePrincipalId`
* `UserDefinedFunctionId`

## <a name="create-a-role-assignment"></a>Een roltoewijzing maken

```plaintext
HTTP POST /api/v1.0/roleassignments
```

| **Naam** | **Vereist** | **Type** | **Beschrijving** |
| --- | --- | --- | --- |
| Rol-id| Ja |tekenreeks | De definitie rol-id. Definities van gebruikersrollen en hun-id's kunnen worden gevonden door het opvragen van de systeem-API. |
| object-id | Ja |tekenreeks | De object-ID voor de roltoewijzing die moet worden opgemaakt op basis van het bijbehorende type. Voor de `DomainName` ObjectIdType, object-id moet beginnen met de `“@”` teken. |
| objectIdType | Ja |tekenreeks | Het type van de roltoewijzing. Moet een van de volgende rijen in deze tabel. |
| tenant-id | Varieert | tekenreeks |De tenant-id. Niet toegestaan voor `DeviceId` en `TenantId` ObjectIdTypes. Vereist voor `UserId` en `ServicePrincipalId` ObjectIdTypes. Dit is optioneel voor de domeinnaam ObjectIdType. |
| pad * | Ja | tekenreeks |De volledige toegang tot het pad naar de `Space` object. Bijvoorbeeld: `/{Guid}/{Guid}` opgeven als een id de roltoewijzing voor de hele grafiek moet, `"/"` (geeft de hoofdmap). Echter, dat wil zeggen met behulp van afgeraden en **u moet altijd volgen op het principe van minimale bevoegdheden**. |

## <a name="sample-configuration"></a>Voorbeeldconfiguratie

Een gebruiker moet beheerderstoegang tot een verdieping van een tenant-ruimte:

  ```JSON
    {
      "RoleId": "98e44ad7-28d4-4007-853b-b9968ad132d1",
      "ObjectId" : " 0fc863bb-eb51-4704-a312-7d635d70e599",
      "ObjectIdType" : "UserId",
      "TenantId": " a0c20ae6-e830-4c60-993d-a91ce6032724",
      "Path": "/ 091e349c-c0ea-43d4-93cf-6b57abd23a44/ d84e82e6-84d5-45a4-bd9d-006a118e3bab"
    }
  ```

Een toepassing die wordt uitgevoerd scenario's simuleren van apparaten en sensoren testen:

  ```JSON
    {
      "RoleId": "98e44ad7-28d4-4007-853b-b9968ad132d1",
      "ObjectId" : "cabf7acd-af0b-41c5-959a-ce2f4c26565b",
      "ObjectIdType" : "ServicePrincipalId",
      "TenantId": " a0c20ae6-e830-4c60-993d-a91ce6032724",
      "Path": "/"
    }
  ```

Alle gebruikers die deel uitmaken van een domein wordt leestoegang voor opslagruimten, sensoren en gebruikers, met inbegrip van hun bijbehorende verwante objecten:

  ```JSON
    {
      "RoleId": " b1ffdb77-c635-4e7e-ad25-948237d85b30",
      "ObjectId" : "@microsoft.com",
      "ObjectIdType" : "DomainName",
      "Path": "/091e349c-c0ea-43d4-93cf-6b57abd23a44"
    }
  ```

Een roltoewijzing ophalen:

```plaintext
HTTP GET /api/v1/roleassignments?path={path}
```

| **Naam** | **In** | **Vereist** |    **Type** |  **Beschrijving** |
| --- | --- | --- | --- | --- |
| Pad | Pad | True | Reeks | Het volledige pad naar de ruimte |

Een roltoewijzing verwijderen:

```plaintext
HTTP DELETE /api/v1/roleassignments/{id}
```

| **Naam** | **In** | **Vereist** | **Type** | **Beschrijving** |
| --- | --- | --- | --- | --- |
| Id | Pad | True | Reeks |   Roltoewijzings-ID |

## <a name="next-steps"></a>Volgende stappen

Lees meer over de beveiliging van Azure digitale dubbels [API-verificatie](./security-authenticating-apis.md).
