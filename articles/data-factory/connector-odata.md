---
title: Gegevens kopiëren van de OData-bronnen met behulp van Azure Data Factory | Microsoft Docs
description: Informatie over het kopiëren van gegevens uit OData-bronnen voor ondersteunde sink-gegevensopslag met behulp van een kopieeractiviteit in een Azure Data Factory-pijplijn.
services: data-factory
documentationcenter: ''
author: linda33wj
manager: craigg
ms.reviewer: douglasl
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 12/13/2018
ms.author: jingwang
ms.openlocfilehash: eeab0311519d7e3410ddb625c1801101ad63ad18
ms.sourcegitcommit: bd15a37170e57b651c54d8b194e5a99b5bcfb58f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/07/2019
ms.locfileid: "57550413"
---
# <a name="copy-data-from-an-odata-source-by-using-azure-data-factory"></a>Gegevens kopiëren van een OData-bron met behulp van Azure Data Factory

> [!div class="op_single_selector" title1="Select the version of Data Factory service you are using:"]
> * [Versie 1:](v1/data-factory-odata-connector.md)
> * [Huidige versie](connector-odata.md)

In dit artikel bevat een overzicht over het gebruik van de Kopieeractiviteit in Azure Data Factory om gegevens te kopiëren uit een OData-bron. Het artikel is gebaseerd op [Kopieeractiviteit in Azure Data Factory](copy-activity-overview.md), die een algemeen overzicht van Kopieeractiviteit geeft.

## <a name="supported-capabilities"></a>Ondersteunde mogelijkheden

U kunt gegevens uit een OData-bron kopiëren naar een ondersteunde sink-gegevensopslag. Zie voor een lijst met gegevens opslaat of Kopieeractiviteit als bronnen en sinks ondersteunt, [ondersteunde gegevensarchieven en indelingen](copy-activity-overview.md#supported-data-stores-and-formats).

Om precies ondersteunt deze OData-connector:

- OData versie 3.0 en 4.0.
- Kopiëren van gegevens met behulp van een van de volgende verificaties: **Anonieme**, **Basic**, **Windows**, **AAD-service-principal**, en **beheerde identiteiten voor een Azure-resources**.

## <a name="get-started"></a>Aan de slag

[!INCLUDE [data-factory-v2-connector-get-started](../../includes/data-factory-v2-connector-get-started.md)]

De volgende secties bevatten meer informatie over eigenschappen die u gebruiken kunt voor het definiëren van Data Factory-entiteiten die specifiek voor een OData-connector zijn.

## <a name="linked-service-properties"></a>Eigenschappen van de gekoppelde service

De volgende eigenschappen worden ondersteund voor een gekoppelde OData-service:

| Eigenschap | Description | Vereist |
|:--- |:--- |:--- |
| type | De **type** eigenschap moet worden ingesteld op **OData**. |Ja |
| url | De basis-URL van de OData-service. |Ja |
| authenticationType | Het type verificatie gebruikt voor verbinding met de OData-bron. Toegestane waarden zijn **anoniem**, **Basic**, **Windows**, **AadServicePrincipal**, en **ManagedServiceIdentity** . De gebruiker op basis van OAuth wordt niet ondersteund. | Ja |
| Gebruikersnaam | Geef **gebruikersnaam** als u basisverificatie of Windows-verificatie gebruikt. | Nee |
| wachtwoord | Geef **wachtwoord** voor de gebruiker-account u hebt opgegeven voor **gebruikersnaam**. Dit veld als markeert een **SecureString** type voor het veilig opslaan in Data Factory. U kunt ook [verwijzen naar een geheim opgeslagen in Azure Key Vault](store-credentials-in-key-vault.md). | Nee |
| servicePrincipalId | Geef de Azure Active Directory-toepassing client-ID. | Nee |
| aadServicePrincipalCredentialType | Geef het referentietype moet worden gebruikt voor verificatie van service-principal. Toegestane waarden zijn: `ServicePrincipalKey` of `ServicePrincipalCert`. | Nee |
| servicePrincipalKey | Geef de sleutel van de Azure Active Directory-toepassing. Dit veld als markeert een **SecureString** voor het veilig opslaan in de Data Factory of [verwijzen naar een geheim opgeslagen in Azure Key Vault](store-credentials-in-key-vault.md). | Nee |
| servicePrincipalEmbeddedCert | Geef de base64-gecodeerd certificaat van uw toepassing in Azure Active Directory geregistreerd. Dit veld als markeert een **SecureString** voor het veilig opslaan in de Data Factory of [verwijzen naar een geheim opgeslagen in Azure Key Vault](store-credentials-in-key-vault.md). | Nee |
| servicePrincipalEmbeddedCertPassword | Geef het wachtwoord van uw certificaat als het certificaat is beveiligd met een wachtwoord. Dit veld als markeert een **SecureString** voor het veilig opslaan in de Data Factory of [verwijzen naar een geheim opgeslagen in Azure Key Vault](store-credentials-in-key-vault.md).  | Nee|
| tenant | De tenantgegevens (domain name of tenant-ID) opgeven in uw toepassing zich bevindt. Deze ophalen door de muis in de rechterbovenhoek van de Azure-portal. | Nee |
| aadResourceId | Geef de AAD-resource die u hebt aangevraagd voor autorisatie.| Nee |
| connectVia | De [Integration Runtime](concepts-integration-runtime.md) gebruiken om te verbinden met het gegevensarchief. U kunt Azure Integration Runtime of een zelf-hostende Integration Runtime (als het gegevensarchief bevindt zich in een particulier netwerk). Indien niet opgegeven, wordt de standaard Azure Integration Runtime wordt gebruikt. |Nee |

**Voorbeeld 1: Met behulp van anonieme verificatie**

```json
{
    "name": "ODataLinkedService",
    "properties": {
        "type": "OData",
        "typeProperties": {
            "url": "https://services.odata.org/OData/OData.svc",
            "authenticationType": "Anonymous"
        },
        "connectVia": {
            "referenceName": "<name of Integration Runtime>",
            "type": "IntegrationRuntimeReference"
        }
    }
}
```

**Voorbeeld 2: Met behulp van basisverificatie**

```json
{
    "name": "ODataLinkedService",
    "properties": {
        "type": "OData",
        "typeProperties": {
            "url": "<endpoint of OData source>",
            "authenticationType": "Basic",
            "userName": "<user name>",
            "password": {
                "type": "SecureString",
                "value": "<password>"
            }
        },
        "connectVia": {
            "referenceName": "<name of Integration Runtime>",
            "type": "IntegrationRuntimeReference"
        }
    }
}
```

**Voorbeeld 3: Met behulp van Windows-verificatie**

```json
{
    "name": "ODataLinkedService",
    "properties": {
        "type": "OData",
        "typeProperties": {
            "url": "<endpoint of OData source>",
            "authenticationType": "Windows",
            "userName": "<domain>\\<user>",
            "password": {
                "type": "SecureString",
                "value": "<password>"
            }
        },
        "connectVia": {
            "referenceName": "<name of Integration Runtime>",
            "type": "IntegrationRuntimeReference"
        }
    }
}
```

**Voorbeeld 4: Met behulp van verificatie van service-principal-sleutel**

```json
{
    "name": "ODataLinkedService",
    "properties": {
        "type": "OData",
        "typeProperties": {
            "url": "<endpoint of OData source>",
            "authenticationType": "AadServicePrincipal",
            "servicePrincipalId": "<service principal id>",
            "aadServicePrincipalCredentialType": "ServicePrincipalKey",
            "servicePrincipalKey": {
                "type": "SecureString",
                "value": "<service principal key>"
            },
            "tenant": "<tenant info, e.g. microsoft.onmicrosoft.com>",
            "aadResourceId": "<AAD resource URL>"
        }
    },
    "connectVia": {
        "referenceName": "<name of Integration Runtime>",
        "type": "IntegrationRuntimeReference"
    }
}
```

**Voorbeeld 5: Met behulp van service-principal cert-verificatie**

```json
{
    "name": "ODataLinkedService",
    "properties": {
        "type": "OData",
        "typeProperties": {
            "url": "<endpoint of OData source>",
            "authenticationType": "AadServicePrincipal",
            "servicePrincipalId": "<service principal id>",
            "aadServicePrincipalCredentialType": "ServicePrincipalCert",
            "servicePrincipalEmbeddedCert": { 
                "type": "SecureString", 
                "value": "<base64 encoded string of (.pfx) certificate data>"
            },
            "servicePrincipalEmbeddedCertPassword": { 
                "type": "SecureString", 
                "value": "<password of your certificate>"
            },
            "tenant": "<tenant info, e.g. microsoft.onmicrosoft.com>",
            "aadResourceId": "<AAD resource e.g. https://tenant.sharepoint.com>"
        }
    },
    "connectVia": {
        "referenceName": "<name of Integration Runtime>",
        "type": "IntegrationRuntimeReference"
    }
}
```

## <a name="dataset-properties"></a>Eigenschappen van gegevensset

Deze sectie bevat een lijst met eigenschappen die ondersteuning biedt voor de OData-gegevensset.

Zie voor een volledige lijst van eigenschappen die beschikbaar zijn voor het definiëren van gegevenssets en secties, [gegevenssets en gekoppelde services](concepts-datasets-linked-services.md). 

Om gegevens te kopiëren uit OData-, stel de **type** eigenschap van de gegevensset in **ODataResource**. De volgende eigenschappen worden ondersteund:

| Eigenschap | Description | Vereist |
|:--- |:--- |:--- |
| type | De **type** eigenschap van de gegevensset moet worden ingesteld op **ODataResource**. | Ja |
| pad | Het pad naar de OData-resource. | Ja |

**Voorbeeld**

```json
{
    "name": "ODataDataset",
    "properties":
    {
        "type": "ODataResource",
        "linkedServiceName": {
            "referenceName": "<OData linked service name>",
            "type": "LinkedServiceReference"
        },
        "typeProperties":
        {
            "path": "Products"
        }
    }
}
```

## <a name="copy-activity-properties"></a>Eigenschappen van de kopieeractiviteit

Deze sectie bevat een lijst met eigenschappen die ondersteuning biedt voor de OData-bron.

Zie voor een volledige lijst van eigenschappen die beschikbaar zijn voor het definiëren van activiteiten en secties, [pijplijnen](concepts-pipelines-activities.md). 

### <a name="odata-as-source"></a>OData als bron

Om gegevens te kopiëren uit OData-, stel de **bron** type in de Kopieeractiviteit naar **RelationalSource**. De volgende eigenschappen worden ondersteund in de Kopieeractiviteit **bron** sectie:

| Eigenschap | Description | Vereist |
|:--- |:--- |:--- |
| type | De **type** eigenschap van de Kopieeractiviteit-bron moet worden ingesteld op **RelationalSource**. | Ja |
| query | OData-queryopties voor het filteren van gegevens. Voorbeeld: `"?$select=Name,Description&$top=5"`.<br/><br/>**Opmerking**: De OData-connector worden gegevens gekopieerd van de gecombineerde URL: `[URL specified in linked service]/[path specified in dataset][query specified in copy activity source]`. Zie voor meer informatie, [OData-URL-onderdelen](https://www.odata.org/documentation/odata-version-3-0/url-conventions/). | Nee |

**Voorbeeld**

```json
"activities":[
    {
        "name": "CopyFromOData",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<OData input dataset name>",
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
                "type": "RelationalSource",
                "query": "?$select=Name,Description&$top=5"
            },
            "sink": {
                "type": "<sink type>"
            }
        }
    }
]
```

## <a name="data-type-mapping-for-odata"></a>Toewijzing voor OData-gegevenstype

Wanneer u gegevens uit OData-kopieert, worden de volgende toewijzingen tussen de OData-gegevenstypen en Azure Data Factory tussentijdse gegevenstypen gebruikt. Zie voor meer informatie hoe Copy Activity in het schema en de gegevens van een brontype toegewezen aan de sink [Schema en gegevens typt toewijzingen](copy-activity-schema-and-type-mapping.md).

| OData-gegevenstype | Data Factory tussentijdse gegevenstype |
|:--- |:--- |
| Edm.Binary | Byte[] |
| Edm.Boolean | Bool |
| Edm.Byte | Byte[] |
| Edm.DateTime | DateTime |
| Edm.Decimal | Decimal |
| Edm.Double | Double |
| Edm.Single | Single |
| Edm.Guid | Guid |
| Edm.Int16 | Int16 |
| Edm.Int32 | Int32 |
| Edm.Int64 | Int64 |
| Edm.SByte | Int16 |
| Edm.String | String |
| Edm.Time | TimeSpan |
| Edm.DateTimeOffset | DateTimeOffset |

> [!NOTE]
> Complexe gegevenstypen voor OData (zoals **Object**) worden niet ondersteund.


## <a name="next-steps"></a>Volgende stappen

Zie voor een lijst met gegevensarchieven die Kopieeractiviteit ondersteunt als bronnen en sinks in Azure Data Factory, [ondersteunde gegevensarchieven en indelingen](copy-activity-overview.md##supported-data-stores-and-formats).