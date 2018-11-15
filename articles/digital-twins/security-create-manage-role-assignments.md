---
title: Informatie over Azure digitale dubbele apparaat verbinding en verificatie | Microsoft Docs
description: Digitale dubbels van Azure gebruiken om te verbinden en verificatie van apparaten
author: lyrana
manager: alinast
ms.service: digital-twins
services: digital-twins
ms.topic: conceptual
ms.date: 11/13/2018
ms.author: lyrana
ms.openlocfilehash: f032e3ebf6a10411057cd6d41df0cad6248f328b
ms.sourcegitcommit: 542964c196a08b83dd18efe2e0cbfb21a34558aa
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/14/2018
ms.locfileid: "51636234"
---
# <a name="create-and-manage-role-assignments"></a>Roltoewijzingen maken en beheren

Azure van digitale dubbels maakt gebruik van op rollen gebaseerd toegangsbeheer ([RBAC](./security-role-based-access-control.md)) voor het beheren van toegang tot bronnen.

De toewijzing van elke rol bevat:

* **Object-id**: een Azure Active Directory-ID, service-principal-object-ID of domeinnaam
* **Object-id-type**
* **Roldefinitie-ID**
* **Pad van de ruimte**
* **Tenant-ID**: In de meeste gevallen tenant Azure Active Directory-ID

[!INCLUDE [Digital Twins Management API](../../includes/digital-twins-management-api.md)]

## <a name="role-definition-identifiers"></a>Rol van definitie-id 's

De volgende tabel ziet u wat kan worden verkregen door het opvragen van de system/rollen API.

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
| Gateway-apparaat | d4c69766-e9bd-4e61-BFC1-d8b6e686c7a8 |

## <a name="supported-objectidtypes"></a>Ondersteunde ObjectIdTypes

De ondersteunde `ObjectIdTypes`:

* `UserId`
* `DeviceId`
* `DomainName`
* `TenantId`
* `ServicePrincipalId`
* `UserDefinedFunctionId`

## <a name="create-a-role-assignment"></a>Een roltoewijzing maken

```plaintext
HTTP POST YOUR_MANAGEMENT_API_URL/roleassignments
```

| **Naam** | **Vereist** | **Type** | **Beschrijving** |
| --- | --- | --- | --- |
| Rol-id| Ja |Reeks | De definitie rol-id. Definities van gebruikersrollen en hun-id vinden door het opvragen van de systeem-API. |
| object-id | Ja |Reeks | De object-ID voor de roltoewijzing die moet worden opgemaakt op basis van het bijbehorende type. Voor de `DomainName` ObjectIdType, object-id moet beginnen met de `“@”` teken. |
| objectIdType | Ja |Reeks | Het type van de roltoewijzing. Moet een van de volgende rijen in deze tabel. |
| tenant-id | Varieert | Reeks |De tenant-id. Niet toegestaan voor `DeviceId` en `TenantId` ObjectIdTypes. Vereist voor `UserId` en `ServicePrincipalId` ObjectIdTypes. Dit is optioneel voor de domeinnaam ObjectIdType. |
| pad * | Ja | Reeks |De volledige toegang tot het pad naar de `Space` object. Een voorbeeld is `/{Guid}/{Guid}`. Als een id de roltoewijzing voor de hele grafiek moet, geef `"/"`. Dit teken wordt aangegeven dat de hoofdmap, maar het gebruik ervan wordt afgeraden. Voer altijd het principe van minimale bevoegdheden. |

## <a name="sample-configuration"></a>Voorbeeldconfiguratie

In dit voorbeeld moet een gebruiker met beheerdersrechten toegang tot een verdieping van een tenant-ruimte.

  ```JSON
    {
      "RoleId": "98e44ad7-28d4-4007-853b-b9968ad132d1",
      "ObjectId" : " 0fc863bb-eb51-4704-a312-7d635d70e599",
      "ObjectIdType" : "UserId",
      "TenantId": " a0c20ae6-e830-4c60-993d-a91ce6032724",
      "Path": "/ 091e349c-c0ea-43d4-93cf-6b57abd23a44/ d84e82e6-84d5-45a4-bd9d-006a118e3bab"
    }
  ```

In dit voorbeeld wordt een toepassing uitgevoerd Testscenario's, apparaten en sensoren te simuleren.

  ```JSON
    {
      "RoleId": "98e44ad7-28d4-4007-853b-b9968ad132d1",
      "ObjectId" : "cabf7acd-af0b-41c5-959a-ce2f4c26565b",
      "ObjectIdType" : "ServicePrincipalId",
      "TenantId": " a0c20ae6-e830-4c60-993d-a91ce6032724",
      "Path": "/"
    }
  ```

Alle gebruikers die deel van een domein uitmaken ontvangen leestoegang voor opslagruimten, sensoren en gebruikers. Deze toegang bevat hun bijbehorende verwante objecten.

  ```JSON
    {
      "RoleId": " b1ffdb77-c635-4e7e-ad25-948237d85b30",
      "ObjectId" : "@microsoft.com",
      "ObjectIdType" : "DomainName",
      "Path": "/091e349c-c0ea-43d4-93cf-6b57abd23a44"
    }
  ```

Gebruiken om op te halen van een roltoewijzing ophalen

```plaintext
HTTP GET YOUR_MANAGEMENT_API_URL/roleassignments?path=YOUR_PATH
```

| **Naam** | **In** | **Vereist** |    **Type** |  **Beschrijving** |
| --- | --- | --- | --- | --- |
| YOUR_PATH | Pad | True | Reeks |    Het volledige pad naar de ruimte |

Gebruik verwijderen om te verwijderen van een roltoewijzing.

```plaintext
HTTP DELETE YOUR_MANAGEMENT_API_URL/roleassignments/YOUR_ROLE_ID
```

| **Naam** | **In** | **Vereist** | **Type** | **Beschrijving** |
| --- | --- | --- | --- | --- |
| YOUR_ROLE_ID | Pad | True | Reeks | Roltoewijzings-ID |

## <a name="next-steps"></a>Volgende stappen

Lees meer over de beveiliging van Azure digitale dubbels [API-verificatie](./security-authenticating-apis.md).
