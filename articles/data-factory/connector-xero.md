---
title: Kopiëren van gegevens uit Xero met behulp van Azure Data Factory (Preview) | Microsoft Docs
description: Informatie over het gegevens uit Xero kopiëren naar gegevensarchieven ondersteunde sink met behulp van een kopieeractiviteit in een Azure Data Factory-pijplijn.
services: data-factory
documentationcenter: ''
author: linda33wj
manager: craigg
ms.reviewer: douglasl
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 06/15/2018
ms.author: jingwang
ms.openlocfilehash: 0d2b55384484932cb243ec6b15a6b4944c716471
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/23/2018
ms.locfileid: "36337651"
---
# <a name="copy-data-from-xero-using-azure-data-factory-preview"></a>Gegevens uit Xero met behulp van Azure Data Factory (Preview) kopiëren

In dit artikel bevat een overzicht van het gebruik van de Kopieeractiviteit in Azure Data Factory om gegevens te kopiëren uit Xero. Dit is gebaseerd op de [activiteit overzicht kopiëren](copy-activity-overview.md) artikel met daarin een algemeen overzicht van de kopieeractiviteit.

> [!NOTE]
> Dit artikel is van toepassing op versie 2 van Data Factory, dat zich momenteel in de previewfase bevindt. Als u van versie 1 van de Data Factory-service gebruikmaakt (GA) is algemeen beschikbaar is, raadpleegt u [Kopieeractiviteit in V1](v1/data-factory-data-movement-activities.md).

> [!IMPORTANT]
> Deze connector is momenteel in preview. U kunt uitproberen en feedback geven. Neem contact op met de [ondersteuning van Azure](https://azure.microsoft.com/support/) als u een afhankelijkheid van preview-connectors wilt opnemen in uw oplossing.

## <a name="supported-capabilities"></a>Ondersteunde mogelijkheden

U kunt gegevens uit Xero kopiëren naar een ondersteunde sink-gegevensarchief. Zie voor een lijst van opgeslagen gegevens die worden ondersteund als bronnen/put door met de kopieerbewerking de [ondersteunde gegevensarchieven](copy-activity-overview.md#supported-data-stores-and-formats) tabel.

In het bijzonder ondersteunt deze connector Xero:

- Xero [persoonlijke toepassing](https://developer.xero.com/documentation/getting-started/api-application-types) maar niet openbaar toepassing.
- Alle Xero tabellen (API-eindpunten) behalve 'Rapporten'. 

Azure Data Factory biedt een ingebouwde stuurprogramma's zodat connectiviteit, dus u hoeft niet te gebruik van deze connector stuurprogramma handmatig installeren.

## <a name="getting-started"></a>Aan de slag

[!INCLUDE [data-factory-v2-connector-get-started](../../includes/data-factory-v2-connector-get-started.md)]

De volgende secties bevatten informatie over de eigenschappen die worden gebruikt voor het definiëren van Data Factory-entiteiten specifieke naar Xero-connector.

## <a name="linked-service-properties"></a>Eigenschappen van de gekoppelde service

De volgende eigenschappen worden ondersteund voor Xero gekoppelde service:

| Eigenschap | Beschrijving | Vereist |
|:--- |:--- |:--- |
| type | De eigenschap type moet worden ingesteld op: **Xero** | Ja |
| host | Het eindpunt van de server Xero (`api.xero.com`).  | Ja |
| consumerKey | De consumentsleutel die is gekoppeld aan de Xero. Dit veld markeren als een SecureString Bewaar deze zorgvuldig in Data Factory of [verwijzen naar een geheim dat is opgeslagen in Azure Key Vault](store-credentials-in-key-vault.md). | Ja |
| privateKey | De persoonlijke sleutel uit het .pem-bestand dat is gegenereerd voor uw persoonlijke toepassing Xero Raadpleeg [maken van een openbaar/persoonlijk sleutelpaar](https://developer.xero.com/documentation/api-guides/create-publicprivate-key). Houd er rekening mee te **genereren de privatekey.pem met numbits van 512** met `openssl genrsa -out privatekey.pem 512`; 1024 wordt niet ondersteund. De tekst van het .pem-bestand met inbegrip van de regel Unix endings(\n) omvatten, zie onderstaand voorbeeld.<br/><br/>Dit veld markeren als een SecureString Bewaar deze zorgvuldig in Data Factory of [verwijzen naar een geheim dat is opgeslagen in Azure Key Vault](store-credentials-in-key-vault.md). | Ja |
| useEncryptedEndpoints | Geeft aan of de eindpunten van de gegevensbron zijn versleuteld via HTTPS. De standaardwaarde is true.  | Nee |
| useHostVerification | Hiermee geeft u op of de hostnaam is vereist in het certificaat van de server overeenkomen met de hostnaam van de server om verbinding te maken via SSL. De standaardwaarde is true.  | Nee |
| usePeerVerification | Geeft aan of de identiteit van de server te verifiëren wanneer u verbinding maakt via SSL. De standaardwaarde is true.  | Nee |

**Voorbeeld:**

```json
{
    "name": "XeroLinkedService",
    "properties": {
        "type": "Xero",
        "typeProperties": {
            "host" : "api.xero.com",
            "consumerKey": {
                 "type": "SecureString",
                 "value": "<consumerKey>"
            },
            "privateKey": {
                 "type": "SecureString",
                 "value": "<privateKey>"
            }
        }
    }
}
```

**Persoonlijke sleutel Voorbeeldwaarde:**

Alle tekst uit het .pem-bestand met inbegrip van de regel Unix endings(\n) bevatten.

```
"-----BEGIN RSA PRIVATE KEY-----\nMII***************************************************P\nbu****************************************************s\nU/****************************************************B\nA*****************************************************W\njH****************************************************e\nsx*****************************************************l\nq******************************************************X\nh*****************************************************i\nd*****************************************************s\nA*****************************************************dsfb\nN*****************************************************M\np*****************************************************Ly\nK*****************************************************Y=\n-----END RSA PRIVATE KEY-----"
```

## <a name="dataset-properties"></a>Eigenschappen van gegevensset

Zie voor een volledige lijst met secties en de eigenschappen die beschikbaar zijn voor het definiëren van gegevenssets, de [gegevenssets](concepts-datasets-linked-services.md) artikel. Deze sectie bevat een lijst met eigenschappen die ondersteund worden door Xero dataset.

Stel de eigenschap type van de gegevensset om gegevens te kopiëren uit Xero, **XeroObject**. Er is geen aanvullende typespecifieke-eigenschap in dit type dataset.

**Voorbeeld**

```json
{
    "name": "XeroDataset",
    "properties": {
        "type": "XeroObject",
        "linkedServiceName": {
            "referenceName": "<Xero linked service name>",
            "type": "LinkedServiceReference"
        }
    }
}
```

## <a name="copy-activity-properties"></a>Eigenschappen van de kopieeractiviteit

Zie voor een volledige lijst met secties en de eigenschappen die beschikbaar zijn voor het definiëren van activiteiten, de [pijplijnen](concepts-pipelines-activities.md) artikel. Deze sectie bevat een lijst met eigenschappen die ondersteund worden door Xero bron.

### <a name="xero-as-source"></a>Xero als bron

Om gegevens te kopiëren uit Xero, stelt u het brontype in de kopieerbewerking naar **XeroSource**. De volgende eigenschappen worden ondersteund in de kopieerbewerking **bron** sectie:

| Eigenschap | Beschrijving | Vereist |
|:--- |:--- |:--- |
| type | De eigenschap type van de bron voor kopiëren-activiteit moet worden ingesteld op: **XeroSource** | Ja |
| query | Gebruik de aangepaste SQL-query om gegevens te lezen. Bijvoorbeeld: `"SELECT * FROM Contacts"`. | Ja |

**Voorbeeld:**

```json
"activities":[
    {
        "name": "CopyFromXero",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<Xero input dataset name>",
                "type": "DatasetReference"
            }
        ],
        "outputs": [
            {
                "referenceName": "<output dataset name>",
                "type": "DatasetReference"
            }
        ],
        "typeProperties": {
            "source": {
                "type": "XeroSource",
                "query": "SELECT * FROM Contacts"
            },
            "sink": {
                "type": "<sink type>"
            }
        }
    }
]
```

Let op het volgende bij het opgeven van de query Xero:

- Tabellen met complexe items gesplitst met meerdere tabellen. Transacties heeft bijvoorbeeld een complexe gegevensstructuur 'LineItems', zodat de gegevens van de banktransactie is toegewezen aan tabel `Bank_Transaction` en `Bank_Transaction_Line_Items`, met `Bank_Transaction_ID` als refererende sleutel ze om elkaar te koppelen.

- Xero gegevens is beschikbaar via twee schema's: `Minimal` (standaard) en `Complete`. Het volledige schema bevat een aanroep van de vereiste tabellen waarvoor extra gegevens (zoals kolom-ID) voordat u de gewenste query.

De volgende tabellen hebben dezelfde informatie in het schema van de minimale en voltooid. Als u het aantal API-aanroepen, minimale schema (standaard) te gebruiken.

- Bank_Transactions
- Contact_Groups 
- Contactpersonen 
- Contacts_Sales_Tracking_Categories 
- Contacts_Phones 
- Contacts_Addresses 
- Contacts_Purchases_Tracking_Categories 
- Credit_Notes 
- Credit_Notes_Allocations 
- Expense_Claims 
- Expense_Claim_Validation_Errors
- Facturen 
- Invoices_Credit_Notes
- Invoices_ vooruitbetalingen 
- Invoices_Overpayments 
- Manual_Journals 
- Te veel 
- Overpayments_Allocations 
- Vooruitbetalingen 
- Prepayments_Allocations 
- Ontvangstbevestigingen 
- Receipt_Validation_Errors 
- Tracking_Categories

De volgende tabellen kunnen alleen worden opgevraagd met volledige schema:

- Complete.Bank_Transaction_Line_Items 
- Complete.Bank_Transaction_Line_Item_Tracking 
- Complete.Contact_Group_Contacts 
- Complete.Contacts_Contact_ personen 
- Complete.Credit_Note_Line_Items 
- Complete.Credit_Notes_Line_Items_Tracking 
- Complete.Expense_Claim_ betalingen 
- Complete.Expense_Claim_Receipts 
- Complete.Invoice_Line_Items 
- Complete.Invoices_Line_Items_Tracking
- Complete.Manual_Journal_Lines 
- Complete.Manual_Journal_Line_Tracking 
- Complete.Overpayment_Line_Items 
- Complete.Overpayment_Line_Items_Tracking 
- Complete.Prepayment_Line_Items 
- Complete.Prepayment_Line_Item_Tracking 
- Complete.Receipt_Line_Items 
- Complete.Receipt_Line_Item_Tracking 
- Complete.Tracking_Category_Options

## <a name="next-steps"></a>Volgende stappen
Zie voor een lijst van ondersteunde gegevensarchieven door met de kopieerbewerking [ondersteunde gegevensarchieven](copy-activity-overview.md#supported-data-stores-and-formats).
