---
title: 'Azure Backup: Recovery Services-kluizen met behulp van REST-API maken'
description: beheren van back-up en het terugzetten van Azure VM Backup met behulp van REST-API
services: backup
author: pvrk
manager: shivamg
keywords: REST-API; Azure VM back-up. Azure VM-herstel.
ms.service: backup
ms.topic: conceptual
ms.date: 08/21/2018
ms.author: pullabhk
ms.assetid: e54750b4-4518-4262-8f23-ca2f0c7c0439
ms.openlocfilehash: 4f18b10ee3f4148badc8e53a9660c9f5c998aef7
ms.sourcegitcommit: 3aa0fbfdde618656d66edf7e469e543c2aa29a57
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/05/2019
ms.locfileid: "55734331"
---
# <a name="create-azure-recovery-services-vault-using-rest-api"></a>Met behulp van REST-API van Azure Recovery Services-kluis maken

De stappen voor het maken van een Azure Recovery Services-kluis met behulp van REST-API worden beschreven in [REST-API-kluis maken](https://docs.microsoft.com/rest/api/recoveryservices/vaults/createorupdate) documentatie. Laat het ons dit document als uitgangspunt gebruiken om te maken van een sleutelkluis met de naam 'testVault' in 'VS-West'.

Als u wilt maken of bijwerken van een Azure Recovery Services-kluis, gebruikt u de volgende *plaatsen* bewerking.

```http
PUT https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.RecoveryServices/vaults/{vaultName}?api-version=2016-06-01
```

## <a name="create-a-request"></a>Een aanvraag maken

Maakt de *plaatsen* aanvraag, de `{subscription-id}` parameter is vereist. Als u meerdere abonnementen hebt, raadpleegt u [werken met meerdere abonnementen](/cli/azure/manage-azure-subscriptions-azure-cli?view=azure-cli-latest). U definieert een `{resourceGroupName}` en `{vaultName}` voor uw resources samen met de `api-version` parameter. In dit artikel wordt gebruikgemaakt van `api-version=2016-06-01`.

De volgende headers zijn vereist:

| Aanvraagheader   | Description |
|------------------|-----------------|
| *Content-Type:*  | Vereist. Ingesteld op `application/json`. |
| *Autorisatie:* | Vereist. Ingesteld op een geldige `Bearer` [toegangstoken](https://docs.microsoft.com/rest/api/azure/#authorization-code-grant-interactive-clients). |

Zie voor meer informatie over het maken van de aanvraag [onderdelen van een REST-API-aanvraag/antwoord](/rest/api/azure/#components-of-a-rest-api-requestresponse).

## <a name="create-the-request-body"></a>Hoofdtekst van de aanvraag maken

De volgende algemene definities worden gebruikt voor het bouwen van een aanvraagtekst:

|Name  |Vereist  |Type  |Description  |
|---------|---------|---------|---------|
|eTag     |         |   String      |  Optionele eTag       |
|location     |  true       |String         |   Resourcelocatie      |
|properties     |         | [VaultProperties](https://docs.microsoft.com/rest/api/recoveryservices/vaults/createorupdate#vaultproperties)        |  Eigenschappen van de kluis       |
|sku     |         |  [SKU](https://docs.microsoft.com/rest/api/recoveryservices/vaults/createorupdate#sku)       |    De unieke systeem-id voor elke Azure-resource     |
|tags     |         | Object        |     Resourcetags    |

Houd er rekening mee dat de kluisnaam van de en de naam van resourcegroep u in de URI plaatsen vindt. De aanvraagtekst definieert de locatie.

## <a name="example-request-body"></a>Voorbeeld van de aanvraag hoofdtekst

De hoofdtekst van het volgende voorbeeld wordt gebruikt voor het maken van een kluis in 'VS-West'. Geef de locatie. De SKU is altijd 'Standaard'.

```json
{
  "properties": {},
  "sku": {
    "name": "Standard"
  },
  "location": "West US"
}
```

## <a name="responses"></a>Antwoorden

Er zijn twee gelukt-antwoorden voor de bewerking voor het maken of bijwerken van een Recovery Services-kluis:

|Name  |Type  |Description  |
|---------|---------|---------|
|200 OK     |   [Vault](https://docs.microsoft.com/rest/api/recoveryservices/vaults/createorupdate#vault)      | OK        |
|201-gemaakt     | [Vault](https://docs.microsoft.com/rest/api/recoveryservices/vaults/createorupdate#vault)        |   Gemaakt      |

Zie voor meer informatie over de REST-API-reacties [verwerken van het antwoordbericht](/rest/api/azure/#process-the-response-message).

### <a name="example-response"></a>Voorbeeld van een antwoord

Een verkorte *201-gemaakt* reactie van het vorige van de voorbeeldaanvraag hoofdtekst bevat een *id* is toegewezen en de *provisioningState* is *geslaagd* :

```json
{
  "location": "westus",
  "name": "testVault",
  "properties": {
    "provisioningState": "Succeeded"
  },
  "id": "/subscriptions/77777777-b0c6-47a2-b37c-d8e65a629c18/resourceGroups/Default-RecoveryServices-ResourceGroup/providers/Microsoft.RecoveryServices/vaults/testVault",
  "type": "Microsoft.RecoveryServices/vaults",
  "sku": {
    "name": "Standard"
  }
}
```

## <a name="next-steps"></a>Volgende stappen

[Maak een back-upbeleid voor de back-ups van een Azure-VM in deze kluis](backup-azure-arm-userestapi-createorupdatepolicy.md).

Zie de volgende documenten voor meer informatie over de Azure REST API's:

- [Azure Recovery Services-provider REST-API](/rest/api/recoveryservices/)
- [Aan de slag met REST API van Azure](/rest/api/azure/)
