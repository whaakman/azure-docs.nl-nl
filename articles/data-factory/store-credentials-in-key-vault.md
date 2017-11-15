---
title: Referenties opslaan in Azure Sleutelkluis | Microsoft Docs
description: Informatie over het opslaan van referenties voor gegevensopslag gebruikt in een Azure sleutelkluis die Azure Data Factory automatisch tijdens runtime ophalen kunnen.
services: data-factory
author: linda33wj
manager: jhubbard
editor: 
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/09/2017
ms.author: jingwang
ms.openlocfilehash: f7604e251bd62ec382ac9ace3de058e345abb863
ms.sourcegitcommit: bc8d39fa83b3c4a66457fba007d215bccd8be985
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/10/2017
---
# <a name="store-credential-in-azure-key-vault"></a>Opslaan van referenties in Azure Sleutelkluis

Slaat u referenties voor gegevensopslag in een [Azure Key Vault](../key-vault/key-vault-whatis.md). Azure Data Factory haalt de referenties bij het uitvoeren van een activiteit die gebruikmaakt van het gegevensarchief. Op dit moment alleen [Dynamics connector](connector-dynamics-crm-office-365.md) en [Salesforce-connector](connector-salesforce.md) deze functie ondersteunen.

> [!NOTE]
> Dit artikel is van toepassing op versie 2 van Data Factory, dat zich momenteel in de previewfase bevindt. Als u van versie 1 van de Data Factory-service gebruikmaakt (GA) is algemeen beschikbaar is, raadpleegt u [documentatie voor Data Factory version1 gedefinieerd](v1/data-factory-introduction.md).

## <a name="prerequisites"></a>Vereisten

Deze functie is afhankelijk van de data factory-service-identiteit. Meer informatie over hoe het werkt van [Data factory-service-identiteit](data-factory-service-identity.md) en controleert u of uw gegevensfactory een gekoppeld.

## <a name="steps"></a>Stappen

Om te verwijzen naar een referentie die zijn opgeslagen in Azure Sleutelkluis, moet u:

1. [Ophalen van de data factory-service-identiteit](data-factory-service-identity.md#retrieve-service-identity) door de waarde van 'SERVICE identiteit TOEPASSINGS-ID gegenereerd samen met uw gegevensfactory.
2. De service-identiteit toegang verlenen aan uw Azure Sleutelkluis. In de sleutelkluis -> Access control -> toevoegen -> Zoek deze servicetoepassing identity-ID om toe te voegen ten minste **lezer** machtiging. Hierdoor kan de aangewezen fabriek voor toegang tot geheim in de sleutelkluis.
3. Maak een gekoppelde service verwijst naar uw Azure Sleutelkluis. Raadpleeg [gekoppelde service van Azure Sleutelkluis](#azure-key-vault-linked-service).
4. Gegevens store gekoppelde service, in welke verwijzing naar de bijbehorende opgeslagen in geheime sleutel-kluis maken. Raadpleeg [verwijzen naar referenties opgeslagen in de sleutelkluis](#reference-credential-stored-in-key-vault).

## <a name="azure-key-vault-linked-service"></a>Azure Sleutelkluis gekoppelde service

De volgende eigenschappen worden ondersteund voor Azure Sleutelkluis gekoppelde service:

| Eigenschap | Beschrijving | Vereist |
|:--- |:--- |:--- |
| type | De eigenschap type moet worden ingesteld op: **AzureKeyVault**. | Ja |
| BaseUrl | Geef de URL van Azure Sleutelkluis. | Ja |

**Voorbeeld:**

```json
{
    "name": "AzureKeyVaultLinkedService",
    "properties": {
        "type": "AzureKeyVault",
        "typeProperties": {
            "baseUrl": "https://<azureKeyVaultName>.vault.azure.net"
        }
    }
}
```

## <a name="reference-credential-stored-in-key-vault"></a>Verwijzing naar referenties opgeslagen in de sleutelkluis

De volgende eigenschappen worden ondersteund wanneer u een veld in de gekoppelde service verwijst naar een geheim sleutelkluis configureren:

| Eigenschap | Beschrijving | Vereist |
|:--- |:--- |:--- |
| type | De eigenschap type van het veld moet worden ingesteld op: **AzureKeyVaultSecret**. | Ja |
| secretName | De naam van het geheim in azure sleutelkluis. | Ja |
| secretVersion | De versie van het geheim in azure sleutelkluis.<br/>Als niet wordt opgegeven, gebruikt deze altijd de nieuwste versie van het geheim.<br/>Indien opgegeven, klikt u vervolgens blijven deze hangen naar de opgegeven versie.| Nee |
| opslaan | Verwijst naar een Azure Key Vault gekoppelde service die u gebruikt voor het opslaan van de referentie. | Ja |

**Voorbeeld: (Zie de sectie 'password')**

```json
{
    "name": "DynamicsLinkedService",
    "properties": {
        "type": "Dynamics",
        "typeProperties": {
            "deploymentType": "<>",
            "organizationName": "<>",
            "authenticationType": "<>",
            "username": "<>",
            "password": {
                "type": "AzureKeyVaultSecret",
                "secretName": "<secret name in AKV>",
                "store":{
                    "referenceName": "<Azure Key Vault linked service>",
                    "type": "LinkedServiceReference"
                }
            }
        }
    }
}
```

## <a name="next-steps"></a>Volgende stappen
Zie voor een lijst met gegevensarchieven als bronnen en put wordt ondersteund door de kopieeractiviteit in Azure Data Factory, [ondersteunde gegevensarchieven](copy-activity-overview.md#supported-data-stores-and-formats).