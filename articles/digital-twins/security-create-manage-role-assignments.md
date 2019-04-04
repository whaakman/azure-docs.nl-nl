---
title: Maken en beheren van roltoewijzingen in Azure, digitale dubbels | Microsoft Docs
description: Maken en beheren van roltoewijzingen in digitale dubbels van Azure.
author: lyrana
manager: alinast
ms.service: digital-twins
services: digital-twins
ms.topic: conceptual
ms.date: 12/26/2018
ms.author: lyrana
ms.custom: seodec18
ms.openlocfilehash: 72155799971760e9ddc93746dceafb1ea554d88b
ms.sourcegitcommit: 9f4eb5a3758f8a1a6a58c33c2806fa2986f702cb
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/03/2019
ms.locfileid: "58905304"
---
# <a name="create-and-manage-role-assignments-in-azure-digital-twins"></a>Maken en beheren van roltoewijzingen in Azure, digitale dubbels

Azure van digitale dubbels maakt gebruik van op rollen gebaseerd toegangsbeheer ([RBAC](./security-role-based-access-control.md)) voor het beheren van toegang tot bronnen.


[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="role-assignments-overview"></a>Overzicht van de rol toewijzingen

De toewijzing van elke rol voldoet aan de definitie van de volgende:

```JSON
{
  "roleId": "00e00ad7-00d4-4007-853b-b9968ad000d1",
  "objectId": "be2c6daa-a3a0-0c0a-b0da-c000000fbc5f",
  "objectIdType": "ServicePrincipalId",
  "path": "/",
  "tenantId": "00f000bf-86f1-00aa-91ab-2d7cd000db47"
}
```

De onderstaande tabel wordt elk kenmerk beschreven:

| Kenmerk | Name | Vereist | Type | Description |
| --- | --- | --- | --- | --- |
| roleId | Rol-id | Ja | String | De unieke ID van de gewenste roltoewijzing. Definities van gebruikersrollen en hun-id vinden door de systeem-API uitvoeren van query's of controleren van de onderstaande tabel. |
| object-id | Object-id | Ja | String | Een Azure Active Directory-ID, service-principal-object-ID of domeinnaam. Wat of wie de toewijzing van rol is toegewezen aan. De roltoewijzing moet zijn geformatteerd volgens het bijbehorende type. Voor de `DomainName` objectIdType, object-id moet beginnen met de `“@”` teken. |
| objectIdType | Object-id-type | Ja | String | Het soort Object-id die wordt gebruikt. Zie **ObjectIdTypes ondersteund** hieronder. |
| pad | Pad van de ruimte | Ja | String | De volledige toegang tot het pad naar de `Space` object. Een voorbeeld is `/{Guid}/{Guid}`. Als een id de roltoewijzing voor de hele grafiek moet, geef `"/"`. Dit teken wordt aangegeven dat de hoofdmap, maar het gebruik ervan wordt afgeraden. Voer altijd het principe van minimale bevoegdheden. |
| tenantId | Tenant-id | Varieert | String | In de meeste gevallen een Azure Active Directory-tenant-id. Niet toegestaan voor `DeviceId` en `TenantId` ObjectIdTypes. Vereist voor `UserId` en `ServicePrincipalId` ObjectIdTypes. Dit is optioneel voor de domeinnaam ObjectIdType. |

### <a name="supported-role-definition-identifiers"></a>Ondersteunde rol definitie-id 's

Een roldefinitie koppelt elke roltoewijzing aan een entiteit in uw omgeving digitale dubbels van Azure.

[!INCLUDE [digital-twins-roles](../../includes/digital-twins-roles.md)]

### <a name="supported-object-identifier-types"></a>Id van ondersteunde objecttypen

Voorheen was de **objectIdType** kenmerk is geïntroduceerd.

[!INCLUDE [digital-twins-object-types](../../includes/digital-twins-object-id-types.md)]

## <a name="role-assignment-operations"></a>Rol toewijzingsbewerkingen

Azure van digitale dubbels biedt volledige ondersteuning voor *maken*, *lezen*, en *verwijderen* bewerkingen voor roltoewijzingen. *UPDATE* bewerkingen worden verwerkt door het toevoegen van roltoewijzingen, roltoewijzingen verwijderen of wijzigen van de [ruimtelijke Intelligence Graph](./concepts-objectmodel-spatialgraph.md) knooppunten die roltoewijzingen geven toegang tot.

![Toewijzing van roleindpunten][1]

Naslagdocumentatie over de opgegeven Swagger bevat meer informatie over alle beschikbare API-eindpunten, bewerkingen en definities.

[!INCLUDE [Digital Twins Swagger](../../includes/digital-twins-swagger.md)]

[!INCLUDE [Digital Twins Management API](../../includes/digital-twins-management-api.md)]

<div id="grant"></div>

### <a name="grant-permissions-to-your-service-principal"></a>Machtigingen verlenen voor uw service-principal

Machtigingen verlenen aan uw service-principal is vaak een van de eerste stappen die u neemt bij het werken met Azure digitale dubbels. Dit omvat:

1. Meld u aan bij uw Azure-instantie via PowerShell.
1. Ophalen van de gegevens van uw service-principal.
1. De gewenste rol toewijzen aan uw service-principal.

ID van uw toepassing wordt geleverd aan u in Azure Active Directory. Lees voor meer informatie over het configureren en inrichten van een Azure digitale Twins in Active Directory via de [snelstartgids](./quickstart-view-occupancy-dotnet.md).

Zodra u de toepassings-ID hebt, voert u de volgende PowerShell-opdrachten uit:

```shell
Login-AzAccount
Get-AzADServicePrincipal -ApplicationId  <ApplicationId>
```

Een gebruiker met de **Admin** rol kunt vervolgens de ruimte-beheerdersrol aan een gebruiker toewijzen door het maken van een geverifieerde HTTP POST-aanvraag naar de URL:

```plaintext
YOUR_MANAGEMENT_API_URL/roleassignments
```

Met de volgende JSON-hoofdtekst:

```JSON
{
  "roleId": "98e44ad7-28d4-4007-853b-b9968ad132d1",
  "objectId": "YOUR_SERVICE_PRINCIPLE_OBJECT_ID",
  "objectIdType": "ServicePrincipalId",
  "path": "YOUR_PATH",
  "tenantId": "YOUR_TENANT_ID"
}
```

<div id="all"></div>

### <a name="retrieve-all-roles"></a>Alle functies ophalen

![Sitesysteemrollen][2]

Als u alle beschikbare rollen (roldefinities), moet u een geverifieerde HTTP GET-aanvraag naar:

```plaintext
YOUR_MANAGEMENT_API_URL/system/roles
```

Een geslaagde aanvraag retourneert een JSON-matrix met items voor elke rol die kunnen worden toegewezen:

```JSON
[
    {
        "id": "3cdfde07-bc16-40d9-bed3-66d49a8f52ae",
        "name": "DeviceAdministrator",
        "permissions": [
            {
                "notActions": [],
                "actions": [
                    "Read",
                    "Create",
                    "Update",
                    "Delete"
                ],
                "condition": "@Resource.Type Any_of {'Device', 'DeviceBlobMetadata', 'DeviceExtendedProperty', 'Sensor', 'SensorBlobMetadata', 'SensorExtendedProperty'} || ( @Resource.Type == 'ExtendedType' && (!Exists @Resource.Category || @Resource.Category Any_of { 'DeviceSubtype', 'DeviceType', 'DeviceBlobType', 'DeviceBlobSubtype', 'SensorBlobSubtype', 'SensorBlobType', 'SensorDataSubtype', 'SensorDataType', 'SensorDataUnitType', 'SensorPortType', 'SensorType' } ) )"
            },
            {
                "notActions": [],
                "actions": [
                    "Read"
                ],
                "condition": "@Resource.Type == 'Space' && @Resource.Category == 'WithoutSpecifiedRbacResourceTypes' || @Resource.Type Any_of {'ExtendedPropertyKey', 'SpaceExtendedProperty', 'SpaceBlobMetadata', 'SpaceResource', 'Matcher'}"
            }
        ],
        "accessControlPath": "/system",
        "friendlyPath": "/system",
        "accessControlType": "System"
    }
]
```

<div id="check"></div>

### <a name="check-a-specific-role-assignment"></a>Controleren op een specifieke roltoewijzing

Om te controleren of een specifieke roltoewijzing, moet u een geverifieerde HTTP GET-aanvraag naar:

```plaintext
YOUR_MANAGEMENT_API_URL/roleassignments/check?userId=YOUR_USER_ID&path=YOUR_PATH&accessType=YOUR_ACCESS_TYPE&resourceType=YOUR_RESOURCE_TYPE
```

| **Parameterwaarde** | **Vereist** |  **Type** |  **Beschrijving** |
| --- | --- | --- | --- |
| YOUR_USER_ID |  True | String |   De object-id voor de gebruikers-id objectIdType. |
| YOUR_PATH | True | String |   Het gekozen pad om te controleren of de toegang voor. |
| YOUR_ACCESS_TYPE |  True | String |   Het toegangstype om te controleren. |
| YOUR_RESOURCE_TYPE | True | String |  De bron om te controleren. |

Een geslaagde aanvraag retourneert een Booleaanse waarde `true` of `false` om aan te geven of het toegangstype is toegewezen aan de gebruiker voor het opgegeven pad en de resource.

### <a name="get-role-assignments-by-path"></a>Roltoewijzingen ophalen door pad

Als u alle roltoewijzingen voor een pad, moet u een geverifieerde HTTP GET-aanvraag naar:

```plaintext
YOUR_MANAGEMENT_API_URL/roleassignments?path=YOUR_PATH
```

| Value | Vervangen door |
| --- | --- |
| YOUR_PATH | Het volledige pad naar de ruimte |

Een geslaagde aanvraag retourneert een JSON-matrix met de toewijzing van elke rol die is gekoppeld aan de geselecteerde **pad** parameter:

```JSON
[
    {
        "id": "0000c484-698e-46fd-a3fd-c12aa11e53a1",
        "roleId": "98e44ad7-28d4-4007-853b-b9968ad132d1",
        "objectId": "0de38846-1aa5-000c-a46d-ea3d8ca8ee5e",
        "objectIdType": "UserId",
        "path": "/"
    }
]
```

### <a name="revoke-a-permission"></a>Een machtiging intrekken

Als u wilt een machtigingen van een ontvanger intrekken, de roltoewijzing te verwijderen door het maken van een geverifieerde HTTP DELETE-aanvraag:

```plaintext
YOUR_MANAGEMENT_API_URL/roleassignments/YOUR_ROLE_ASSIGNMENT_ID
```

| Parameter | Vervangen door |
| --- | --- |
| *YOUR_ROLE_ASSIGNMENT_ID* | De **id** van de roltoewijzing verwijderen |

Een geslaagde DELETE-aanvraag retourneert een 204 antwoordstatus. Controleer of het verwijderen van de roltoewijzing door [controleren](#check) of de roltoewijzing nog bevat.

### <a name="create-a-role-assignment"></a>Een roltoewijzing maken

Als u wilt een roltoewijzing maken, moet u een geverifieerde HTTP POST-aanvraag voor de URL:

```plaintext
YOUR_MANAGEMENT_API_URL/roleassignments
```

Controleer of dat de JSON-hoofdtekst aan de volgende schema voldoet:

```JSON
{
  "roleId": "YOUR_ROLE_ID",
  "objectId": "YOUR_OBJECT_ID",
  "objectIdType": "YOUR_OBJECT_ID_TYPE",
  "path": "YOUR_PATH",
  "tenantId": "YOUR_TENANT_ID"
}
```

Een geslaagde aanvraag resulteert in een status 201-antwoord samen met de **id** van de toewijzing van de zojuist gemaakte rol:

```JSON
"d92c7823-6e65-41d4-aaaa-f5b32e3f01b9"
```

## <a name="configuration-examples"></a>Voorbeelden van configuratie

De volgende voorbeelden laten zien hoe u uw JSON-hoofdtekst in verschillende scenario's voor frequent roltoewijzing configureren.

* **Voorbeeld**: Een gebruiker moet beheerderstoegang tot een verdieping van een tenant-ruimte.

   ```JSON
   {
    "roleId": "98e44ad7-28d4-4007-853b-b9968ad132d1",
    "objectId" : " 0fc863aa-eb51-4704-a312-7d635d70e000",
    "objectIdType" : "UserId",
    "tenantId": " a0c20ae6-e830-4c60-993d-a00ce6032724",
    "path": "/ 000e349c-c0ea-43d4-93cf-6b00abd23a44/ d84e82e6-84d5-45a4-bd9d-006a000e3bab"
   }
   ```

* **Voorbeeld**: Een toepassing wordt uitgevoerd voor Testscenario's, apparaten en sensoren te simuleren.

   ```JSON
   {
    "roleId": "98e44ad7-28d4-0007-853b-b9968ad132d1",
    "objectId" : "cabf7aaa-af0b-41c5-000a-ce2f4c20000b",
    "objectIdType" : "ServicePrincipalId",
    "tenantId": " a0c20ae6-e000-4c60-993d-a91ce6000724",
    "path": "/"
   }
    ```

* **Voorbeeld**: Alle gebruikers die deel van een domein uitmaken ontvangen leestoegang voor opslagruimten, sensoren en gebruikers. Deze toegang bevat hun bijbehorende verwante objecten.

   ```JSON
   {
    "roleId": " b1ffdb77-c635-4e7e-ad25-948237d85b30",
    "objectId" : "@microsoft.com",
    "objectIdType" : "DomainName",
    "path": "/000e349c-c0ea-43d4-93cf-6b00abd23a00"
   }
   ```

## <a name="next-steps"></a>Volgende stappen

- Als u wilt controleren Azure digitale dubbels rol-toegangsbeheer op basis van--, Lees [rol-base-access-control](./security-authenticating-apis.md).

- Lees voor meer informatie over Azure digitale dubbels API-verificatie, [API-verificatie](./security-authenticating-apis.md).

<!-- Images -->
[1]: media/security-roles/roleassignments.png
[2]: media/security-roles/system.png
