---
title: 'Azure Backup: Recovery Services kluizen maken met behulp van REST API'
description: back-up-en herstel bewerkingen van Azure VM-back-ups beheren met REST API
author: pvrk
manager: shivamg
keywords: REST API; Back-ups van Azure-VM'S; Azure VM herstellen;
ms.service: backup
ms.topic: conceptual
ms.date: 08/21/2018
ms.author: pullabhk
ms.assetid: e54750b4-4518-4262-8f23-ca2f0c7c0439
ms.openlocfilehash: 0373098dd344df79be79871227f20c8a995958fa
ms.sourcegitcommit: c72ddb56b5657b2adeb3c4608c3d4c56e3421f2c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/24/2019
ms.locfileid: "68466942"
---
# <a name="create-azure-recovery-services-vault-using-rest-api"></a>Azure Recovery Services-kluis maken met behulp van REST API

De stappen voor het maken van een Azure Recovery Services kluis met behulp van REST API worden beschreven in de documentatie voor het [maken van kluis rest API](https://docs.microsoft.com/rest/api/recoveryservices/vaults/createorupdate) . Laat ons dit document gebruiken als referentie voor het maken van een kluis met de naam ' testVault ' in ' vs-West '.

Als u een Azure Recovery Services kluis wilt maken of bijwerken, gebruikt u de volgende *put* -bewerking.

```http
PUT https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.RecoveryServices/vaults/{vaultName}?api-version=2016-06-01
```

## <a name="create-a-request"></a>Een aanvraag maken

Voor het maken van de *put* - `{subscription-id}` aanvraag is de para meter vereist. Als u meerdere abonnementen hebt, raadpleegt u [werken met meerdere abonnementen](/cli/azure/manage-azure-subscriptions-azure-cli?view=azure-cli-latest). U definieert een `{resourceGroupName}` en `{vaultName}` voor uw resources, samen met de `api-version` para meter. In dit artikel `api-version=2016-06-01`wordt gebruikgemaakt van.

De volgende headers zijn vereist:

| Aanvraag header   | Description |
|------------------|-----------------|
| *Inhouds type:*  | Vereist. Ingesteld op `application/json`. |
| *Authorization:* | Vereist. Ingesteld op een geldig `Bearer` [toegangs token](https://docs.microsoft.com/rest/api/azure/#authorization-code-grant-interactive-clients). |

Zie [onderdelen van een rest API aanvraag/antwoord](/rest/api/azure/#components-of-a-rest-api-requestresponse)voor meer informatie over het maken van de aanvraag.

## <a name="create-the-request-body"></a>De aanvraag tekst maken

De volgende algemene definities worden gebruikt voor het bouwen van een aanvraag tekst:

|Name  |Vereist  |Type  |Description  |
|---------|---------|---------|---------|
|eTag     |         |   Reeks      |  Optionele eTag       |
|location     |  true       |Tekenreeks         |   Resourcelocatie      |
|properties     |         | [VaultProperties](https://docs.microsoft.com/rest/api/recoveryservices/vaults/createorupdate#vaultproperties)        |  Eigenschappen van de kluis       |
|sku     |         |  [SKU](https://docs.microsoft.com/rest/api/recoveryservices/vaults/createorupdate#sku)       |    Identificeert de unieke systeem-id voor elke Azure-resource     |
|codes     |         | Object        |     Resourcetags    |

Houd er rekening mee dat de naam van de kluis en de resource groep is opgenomen in de PUT-URI. De hoofd tekst van de aanvraag definieert de locatie.

## <a name="example-request-body"></a>Voorbeeld aanvraag tekst

De volgende voorbeeld tekst wordt gebruikt voor het maken van een kluis in ' vs-West '. Geef de locatie op. De SKU is altijd ' standaard '.

```json
{
  "properties": {},
  "sku": {
    "name": "Standard"
  },
  "location": "West US"
}
```

## <a name="responses"></a>Responses

Er zijn twee geslaagde reacties voor de bewerking om een Recovery Services kluis te maken of bij te werken:

|Name  |Type  |Description  |
|---------|---------|---------|
|200 OK     |   [Vault](https://docs.microsoft.com/rest/api/recoveryservices/vaults/createorupdate#vault)      | OK        |
|201 gemaakt     | [Vault](https://docs.microsoft.com/rest/api/recoveryservices/vaults/createorupdate#vault)        |   Gemaakt      |

Zie voor meer informatie over REST API reacties [het antwoord bericht verwerken](/rest/api/azure/#process-the-response-message).

### <a name="example-response"></a>Voorbeeld van een antwoord

Een versmalded *201* -antwoord dat is gemaakt op basis van de aanvraag tekst van het vorige voor beeld toont een *id* die is toegewezen en de *provisioningState* is *voltooid*:

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

[Maak een back-upbeleid voor het maken van een back-up van een virtuele Azure-machine in deze kluis](backup-azure-arm-userestapi-createorupdatepolicy.md).

Raadpleeg de volgende documenten voor meer informatie over de REST-Api's van Azure:

- [REST API Azure Recovery Services provider](/rest/api/recoveryservices/)
- [Aan de slag met Azure REST API](/rest/api/azure/)
