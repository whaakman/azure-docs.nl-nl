---
title: 'Azure back-up: Back-upbeleid met behulp van REST-API maken'
description: Back-upbeleid (schema en de retentie) beheren met behulp van REST-API
services: backup
author: pvrk
manager: shivamg
keywords: REST-API; Azure VM back-up. Azure VM-herstel.
ms.service: backup
ms.topic: conceptual
ms.date: 08/21/2018
ms.author: pullabhk
ms.assetid: 5ffc4115-0ae5-4b85-a18c-8a942f6d4870
ms.openlocfilehash: 657a777da0e984a145c1c617a6194bf4ef56306e
ms.sourcegitcommit: 02ce0fc22a71796f08a9aa20c76e2fa40eb2f10a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/08/2018
ms.locfileid: "51289832"
---
# <a name="create-azure-recovery-services-backup-policies-using-rest-api"></a>Azure Recovery Services back-upbeleid met behulp van REST-API maken

De stappen voor het maken van een back-upbeleid voor een Azure Recovery Services-kluis worden beschreven in de [REST-API beleidsdocument](https://docs.microsoft.com/rest/api/backup/protectionpolicies/createorupdate). Laat het ons dit document als uitgangspunt gebruiken om een beleid voor Azure-VM back-up te maken.

## <a name="backup-policy-essentials"></a>Back-upbeleid essentials

- Een back-upbeleid is per kluis gemaakt.
- Een back-upbeleid kan voor de back-up van de volgende werkbelastingen worden gemaakt
  - Azure VM
  - SQL in Azure-VM
  - Azure-bestandsshare
- Een beleid kan worden toegewezen aan veel resources. Een Azure-VM back-upbeleid kan worden gebruikt om u te veel Azure-VM's beveiligen.
- Een beleid bestaat uit twee onderdelen
  - Schema: Wanneer u een back-up
  - Bewaarperiode: Hoe lang elke back-up moet worden bewaard.
- Schema kan worden gedefinieerd als 'dagelijks' of 'wekelijks' met een bepaald punt in tijd wordt opgelost.
- Retentie kan worden gedefinieerd voor 'dag', 'week', 'maand', 'jaarlijkse' back-uppunten.
- 'wekelijks' verwijst naar een back-up op een bepaalde dag een van de week, 'maand' betekent een back-up op een bepaalde dag een van de maand en "jaarlijkse" verwijst naar een back-up op een bepaalde dag van het jaar.
- Bewaarperiode voor back-uppunten van 'maand', "jaarlijkse" is aangeduid als 'LongTermRetention'.
- Wanneer een kluis is gemaakt, wordt een beleid voor Azure VM-back-ups met de naam 'DefaultPolicy' ook wordt gemaakt en kan worden gebruikt om back-up Azure VM's.

Als u wilt maken of bijwerken van een Azure Backup-beleid, gebruikt u de volgende *plaatsen* bewerking

```http
PUT https://management.azure.com/Subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.RecoveryServices/vaults/{vaultName}/backupPolicies/{policyName}?api-version=2016-12-01
```

De `{policyName}` en `{vaultName}` vindt u in de URI. Als u meer informatie vindt u in de aanvraagtekst.

## <a name="create-the-request-body"></a>Hoofdtekst van de aanvraag maken

Bijvoorbeeld, voor het maken van een beleid voor Azure VM backup, vindt hieronder u de onderdelen van de aanvraagtekst.

|Naam  |Vereist  |Type  |Beschrijving  |
|---------|---------|---------|---------|
|properties     |   True      |  ProtectionPolicy:[AzureIaaSVMProtectionPolicy](https://docs.microsoft.com/rest/api/backup/protectionpolicies/createorupdate#azureiaasvmprotectionpolicy)      | ProtectionPolicyResource eigenschappen        |
|tags     |         | Object        |  Resourcetags       |

Voor de volledige lijst met definities in de aanvraagtekst, raadpleegt u de [back-upbeleid REST API-document](https://docs.microsoft.com/rest/api/backup/protectionpolicies/createorupdate).

### <a name="example-request-body"></a>Voorbeeld van de aanvraag hoofdtekst

De hoofdtekst van de volgende aanvraag definieert een back-upbeleid voor back-ups van virtuele Azure-machine.

Het beleid zegt:

- Maak een wekelijkse back-up elke maandag, woensdag, donderdag om 10:00 uur Pacific (standaardtijd).
- De back-ups uitgevoerd op elke maandag, woensdag, donderdag voor één week behouden.
- De back-ups uitgevoerd op elke eerste dag, woensdag en derde donderdag van een maand gedurende twee maanden (onderdrukkingen de vorige voorwaarden van retentie, indien van toepassing) behouden.
- De back-ups uitgevoerd op de vierde maandag en vierde donderdag in februari en November vier jaar (onderdrukkingen de vorige voorwaarden van retentie, indien van toepassing) behouden.

```json
{
  "properties": {
    "backupManagementType": "AzureIaasVM",
    "timeZone": "Pacific Standard Time",
    "schedulePolicy": {
      "schedulePolicyType": "SimpleSchedulePolicy",
      "scheduleRunFrequency": "Weekly",
      "scheduleRunTimes": [
        "2018-01-24T10:00:00Z"
      ],
      "scheduleRunDays": [
        "Monday",
        "Wednesday",
        "Thursday"
      ]
    },
    "retentionPolicy": {
      "retentionPolicyType": "LongTermRetentionPolicy",
      "weeklySchedule": {
        "daysOfTheWeek": [
          "Monday",
          "Wednesday",
          "Thursday"
        ],
        "retentionTimes": [
          "2018-01-24T10:00:00Z"
        ],
        "retentionDuration": {
          "count": 1,
          "durationType": "Weeks"
        }
      },
      "monthlySchedule": {
        "retentionScheduleFormatType": "Weekly",
        "retentionScheduleWeekly": {
          "daysOfTheWeek": [
            "Wednesday",
            "Thursday"
          ],
          "weeksOfTheMonth": [
            "First",
            "Third"
          ]
        },
        "retentionTimes": [
          "2018-01-24T10:00:00Z"
        ],
        "retentionDuration": {
          "count": 2,
          "durationType": "Months"
        }
      },
      "yearlySchedule": {
        "retentionScheduleFormatType": "Weekly",
        "monthsOfYear": [
          "February",
          "November"
        ],
        "retentionScheduleWeekly": {
          "daysOfTheWeek": [
            "Monday",
            "Thursday"
          ],
          "weeksOfTheMonth": [
            "Fourth"
          ]
        },
        "retentionTimes": [
          "2018-01-24T10:00:00Z"
        ],
        "retentionDuration": {
          "count": 4,
          "durationType": "Years"
        }
      }
    }
  }
}
```

> [!IMPORTANT]
> De tijdnotatie voor planning en retentie ondersteunen alleen datum/tijd. Alleen tijdnotatie niet wordt ondersteund.

## <a name="responses"></a>Antwoorden

De back-upbeleid maken/bijwerken is een [asynchrone bewerking](https://docs.microsoft.com/azure/azure-resource-manager/resource-manager-async-operations). Dit betekent dat deze bewerking wordt gemaakt van een andere bewerking waardoor moet afzonderlijk worden bijgehouden.

Deze twee antwoorden retourneert: 202 (aanvaard) wanneer een andere bewerking wordt gemaakt en klik vervolgens op 200 (OK) wanneer deze bewerking is voltooid.

|Naam  |Type  |Beschrijving  |
|---------|---------|---------|
|200 OK     |    [Beveiliging PolicyResource](https://docs.microsoft.com/rest/api/backup/protectionpolicies/createorupdate#protectionpolicyresource)     |  OK       |
|202 geaccepteerd     |         |     Geaccepteerd    |

### <a name="example-responses"></a>Van de voorbeeldantwoorden

Zodra u de *plaatsen* van de aanvraag voor het maken van beleid of bijwerken en de reactietijd is 202 (aanvaard) met een location-header of de Azure-async-header.

```http
HTTP/1.1 202 Accepted
Pragma: no-cache
Retry-After: 60
Azure-AsyncOperation: https://management.azure.com/Subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/SwaggerTestRg/providers/Microsoft.RecoveryServices/vaults/testVault/backupPolicies/testPolicy1/operations/00000000-0000-0000-0000-000000000000?api-version=2016-06-01
X-Content-Type-Options: nosniff
x-ms-request-id: db785be0-bb20-4598-bc9f-70c9428b170b
x-ms-client-request-id: e1f94eef-9b2d-45c4-85b8-151e12b07d03; e1f94eef-9b2d-45c4-85b8-151e12b07d03
Strict-Transport-Security: max-age=31536000; includeSubDomains
x-ms-ratelimit-remaining-subscription-writes: 1199
x-ms-correlation-request-id: db785be0-bb20-4598-bc9f-70c9428b170b
x-ms-routing-request-id: SOUTHINDIA:20180521T073907Z:db785be0-bb20-4598-bc9f-70c9428b170b
Cache-Control: no-cache
Date: Mon, 21 May 2018 07:39:06 GMT
Location: https://management.azure.com/Subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/SwaggerTestRg/providers/Microsoft.RecoveryServices/vaults/testVault/backupPolicies/testPolicy1/operationResults/00000000-0000-0000-0000-000000000000?api-version=2016-06-01
X-Powered-By: ASP.NET
```

De resulterende bewerking met de location-header of de Azure-AsyncOperation koptekst met een eenvoudige vervolgens bijhouden *ophalen* opdracht.

```http
GET https://management.azure.com/Subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/SwaggerTestRg/providers/Microsoft.RecoveryServices/vaults/testVault/backupPolicies/testPolicy1/operationResults/00000000-0000-0000-0000-000000000000?api-version=2016-06-01
```

Nadat de bewerking is voltooid, wordt 200 (OK) geretourneerd met de inhoud van het beleid in de hoofdtekst van de reactie.

```json
{
  "id": "/Subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/SwaggerTestRg/providers/Microsoft.RecoveryServices/vaults/testVault/backupPolicies/testPolicy1",
  "name": "testPolicy1",
  "type": "Microsoft.RecoveryServices/vaults/backupPolicies",
  "properties": {
    "backupManagementType": "AzureIaasVM",
    "schedulePolicy": {
      "schedulePolicyType": "SimpleSchedulePolicy",
      "scheduleRunFrequency": "Weekly",
      "scheduleRunDays": [
        "Monday",
        "Wednesday",
        "Thursday"
      ],
      "scheduleRunTimes": [
        "2018-01-24T10:00:00Z"
      ],
      "scheduleWeeklyFrequency": 0
    },
    "retentionPolicy": {
      "retentionPolicyType": "LongTermRetentionPolicy",
      "weeklySchedule": {
        "daysOfTheWeek": [
          "Monday",
          "Wednesday",
          "Thursday"
        ],
        "retentionTimes": [
          "2018-01-24T10:00:00Z"
        ],
        "retentionDuration": {
          "count": 1,
          "durationType": "Weeks"
        }
      },
      "monthlySchedule": {
        "retentionScheduleFormatType": "Weekly",
        "retentionScheduleWeekly": {
          "daysOfTheWeek": [
            "Wednesday",
            "Thursday"
          ],
          "weeksOfTheMonth": [
            "First",
            "Third"
          ]
        },
        "retentionTimes": [
          "2018-01-24T10:00:00Z"
        ],
        "retentionDuration": {
          "count": 2,
          "durationType": "Months"
        }
      },
      "yearlySchedule": {
        "retentionScheduleFormatType": "Weekly",
        "monthsOfYear": [
          "February",
          "November"
        ],
        "retentionScheduleWeekly": {
          "daysOfTheWeek": [
            "Monday",
            "Thursday"
          ],
          "weeksOfTheMonth": [
            "Fourth"
          ]
        },
        "retentionTimes": [
          "2018-01-24T10:00:00Z"
        ],
        "retentionDuration": {
          "count": 4,
          "durationType": "Years"
        }
      }
    },
    "timeZone": "Pacific Standard Time",
    "protectedItemsCount": 0
  }
}
```

Als er al een beleid wordt gebruikt voor het beveiligen van een item, een update in het beleid resulteert in [beveiliging wijzigen](backup-azure-arm-userestapi-backupazurevms.md#changing-the-policy-of-protection) voor alle dergelijke objecten gekoppeld.

## <a name="next-steps"></a>Volgende stappen

[Schakel de beveiliging voor een niet-beveiligde Azure-VM](backup-azure-arm-userestapi-backupazurevms.md).

Raadpleeg de volgende documenten voor meer informatie over de REST API's van Azure back-up:

- [Azure Recovery Services-provider REST-API](/rest/api/recoveryservices/)
- [Aan de slag met REST API van Azure](/rest/api/azure/)