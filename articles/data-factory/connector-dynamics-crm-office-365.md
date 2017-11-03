---
title: "Gegevens kopiëren van Dynamics CRM en 365 met behulp van Azure Data Factory | Microsoft Docs"
description: "Informatie over het kopiëren van gegevens tot Dynamics CRM en 365 naar gegevensarchieven ondersteunde sink met behulp van een kopieeractiviteit in een Azure Data Factory-pijplijn."
services: data-factory
documentationcenter: 
author: linda33wj
manager: jhubbard
editor: spelluru
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/10/2017
ms.author: jingwang
ms.openlocfilehash: 74a4fca2ff5a0f913b5cb195f42482339e6200cb
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="copy-data-from-dynamics-365dynamics-crm-using-azure-data-factory"></a>Gegevens kopiëren van Dynamics 365 Dynamics CRM met Azure Data Factory
In dit artikel bevat een overzicht van het gebruik van de Kopieeractiviteit in Azure Data Factory om gegevens te kopiëren van Dynamics 365 Dynamics CRM. Dit is gebaseerd op de [activiteit overzicht kopiëren](copy-activity-overview.md) artikel met daarin een algemeen overzicht van de kopieeractiviteit.

## <a name="supported-scenarios"></a>Ondersteunde scenario's

U kunt gegevens uit Dynamics 365 Dynamics CRM kopiëren naar een ondersteunde sink-gegevensarchief. Zie voor een lijst met gegevensarchieven als bronnen/put wordt ondersteund door de kopieeractiviteit, de [ondersteunde gegevensarchieven](copy-activity-overview.md#supported-data-stores-and-formats) tabel.

In het bijzonder ondersteunt deze connector Dynamics hieronder Dynamics versies en verificatietypen:

| Dynamics-versies | Verificatietypen | Voorbeelden van de gekoppelde service |
|:--- |:--- |:--- |
| Dynamics 365 online <br> Dynamics CRM online | Office365 | [Dynamics Online + Office365 auth](#dynamics-365-and-dynamics-crm-online) |
| Dynamics 365 on-premises met IFD <br> Dynamics CRM-2016 met on-premises IFD <br> Dynamics CRM-2015 met on-premises IFD | IFD | [Dynamics On-premises met IFD + IFD auth](#dynamics-365-and-dynamics-crm-on-premises-with-ifd) |

*IFD is kort voor internetgerichte implementatie.*

> [!NOTE]
> Gebruik Dynamics connector moet uw wachtwoord opslaan in Azure Sleutelkluis en laat ADF kopie vizier pull daar bij het uitvoeren van de gegevens opnieuw te kopiëren. Informatie over het configureren van in [gekoppelde service-eigenschappen](#linked-service-properties) sectie.

## <a name="getting-started"></a>Aan de slag
U kunt een pijplijn maken met de kopieeractiviteit middels de .NET SDK, Python SDK, Azure PowerShell, REST-API of Azure Resource Manager-sjabloon. Zie [kopie activiteit zelfstudie](quickstart-create-data-factory-dot-net.md) voor stapsgewijze instructies voor een pijplijn maken met een kopieeractiviteit.

De volgende secties bevatten informatie over de eigenschappen die worden gebruikt voor het definiëren van Data Factory-entiteiten specifieke naar Dynamics.

## <a name="linked-service-properties"></a>Eigenschappen van de gekoppelde service

De volgende eigenschappen worden ondersteund voor de Dynamics gekoppelde service:

### <a name="dynamics-365-and-dynamics-crm-online"></a>Dynamics 365 en Dynamics CRM Online

| Eigenschap | Beschrijving | Vereist |
|:--- |:--- |:--- |
| type | De eigenschap type moet worden ingesteld op: **Dynamics**. | Ja |
| deploymentType | Het implementatietype van de Dynamics-exemplaar. Moet **'Online'** voor Dynamics Online. | Ja |
| Organisatienaam | De naam van de organisatie van de Dynamics-exemplaar. | Nee, moet opgeven wanneer er meer dan één Dynamics-exemplaren die zijn gekoppeld aan de gebruiker. |
| authenticationType | Het verificatietype dat verbinding maken met Dynamics-server. Geef **'Office365'** voor Dynamics Online. | Ja |
| gebruikersnaam | Geef de gebruikersnaam voor verbinding maken met de Dynamics. | Ja |
| wachtwoord | Wachtwoord voor het gebruikersaccount dat u hebt opgegeven voor de gebruikersnaam opgeven. U moet het wachtwoord in de Azure Sleutelkluis te plaatsen en het wachtwoord als een 'AzureKeyVaultSecret' configureren. Zie voor meer informatie de [referenties opgeslagen in de Sleutelkluis](store-credentials-in-key-vault.md) artikel. | Ja |

**Voorbeeld: Office365 verificatie met behulp van Dynamics online**

```json
{
    "name": "DynamicsLinkedService",
    "properties": {
        "type": "Dynamics",
        "description": "Dynamics online linked service using Office365 authentication",
        "typeProperties": {
            "deploymentType": "Online",
            "organizationName": "orga02d9c75",
            "authenticationType": "Office365",
            "username": "test@contoso.onmicrosoft.com",
            "password": {
                "type": "AzureKeyVaultSecret",
                "secretName": "mySecret",
                "store":{
                    "linkedServiceName": "<Azure Key Vault linked service>",
                    "type": "LinkedServiceReference"
                }
            }
        }
    }
}
```

### <a name="dynamics-365-and-dynamics-crm-on-premises-with-ifd"></a>Dynamics 365 en Dynamics CRM on-premises met IFD

*Aanvullende eigenschappen vergelijken met het Dyanmics online zijn 'hostnaam' en 'poort'.*

| Eigenschap | Beschrijving | Vereist |
|:--- |:--- |:--- |
| type | De eigenschap type moet worden ingesteld op: **Dynamics**. | Ja |
| deploymentType | Het implementatietype van de Dynamics-exemplaar. Moet **'OnPremisesWithIfd'** voor Dynamics met on-premises IFD.| Ja |
| **Hostnaam** | De hostnaam van de on-premises Dynamics-server. | Ja |
| **poort** | De poort van de on-premises Dynamics-server. | Nee, standaard is dit 443 |
| Organisatienaam | De naam van de organisatie van de Dynamics-exemplaar. | Ja |
| authenticationType | Het verificatietype dat verbinding maken met Dynamics-server. Geef **'Ifd'** voor Dynamics met on-premises IFD. | Ja |
| gebruikersnaam | Geef de gebruikersnaam voor verbinding maken met de Dynamics. | Ja |
| wachtwoord | Wachtwoord voor het gebruikersaccount dat u hebt opgegeven voor de gebruikersnaam opgeven. Houd er rekening mee hebt tot het wachtwoord in de Azure Sleutelkluis te plaatsen en het wachtwoord als een 'AzureKeyVaultSecret' configureren. Zie voor meer informatie de [referenties opgeslagen in de Sleutelkluis](store-credentials-in-key-vault.md) artikel. | Ja |

**Voorbeeld: Dynamics met on-premises IFD IFD-verificatie**

```json
{
    "name": "DynamicsLinkedService",
    "properties": {
        "type": "Dynamics",
        "description": "Dynamics on-premises with IFD linked service using IFD authentication",
        "typeProperties": {
            "deploymentType": "OnPremisesWithIFD",
            "hostName": "contosodynamicsserver.contoso.com",
            "port": 443,
            "organizationName": "admsDynamicsTest",
            "authenticationType": "Ifd",
            "username": "test@contoso.onmicrosoft.com",
            "password": {
                "type": "AzureKeyVaultSecret",
                "secretName": "mySecret",
                "store":{
                    "linkedServiceName": "<Azure Key Vault linked service>",
                    "type": "LinkedServiceReference"
                }
            }
        }
    }
}
```

## <a name="dataset-properties"></a>Eigenschappen van gegevensset

Zie voor een volledige lijst met secties en de eigenschappen die beschikbaar zijn voor het definiëren van gegevenssets, de [gegevenssets](concepts-datasets-linked-services.md) artikel. Deze sectie bevat een lijst met eigenschappen die ondersteund worden door Dynamics dataset.

Om gegevens te kopiëren van Dynamics, stel de eigenschap type van de gegevensset **DynamicsEntity**. De volgende eigenschappen worden ondersteund:

| Eigenschap | Beschrijving | Vereist |
|:--- |:--- |:--- |
| type | De eigenschap type van de gegevensset moet worden ingesteld op: **DynamicsEntity** |Ja |
| entityName | De logische naam van de entiteit ophalen. | Nee (als 'query' in de activiteitbron is opgegeven) |

> [!IMPORTANT]
> **De sectie 'structuur' in de gegevensset is vereist voor de Dynamics**, definieert een naam en het gegevenstype kolomtype voor Dynamics gegevens die u kopiëren wilt via. Klik hier als u meer wilt weten van [gegevenssetstructuur](concepts-datasets-linked-services.md#dataset-structure) en de [gegevenstypetoewijzing voor Dynamics](#data-type-mapping-for-dynamics).

**Voorbeeld:**

```json
{
    "name": "DynamicsDataset",
    "properties": {
        "type": "DynamicsEntity",
        "structure": [
            {
                "name": "accountid",
                "type": "Guid"
            },
            {
                "name": "name",
                "type": "String"
            },
            {
                "name": "marketingonly",
                "type": "Boolean"
            },
            {
                "name": "modifiedon",
                "type": "Datetime"
            }
        ],
        "typePoperties": {
            "entityName": "account"
        },
        "linkedServiceName": {
            "referenceName": "<Dynamics linked service name>",
            "type": "linkedservicereference"
        }
    }
}
```

## <a name="copy-activity-properties"></a>Eigenschappen van de activiteit kopiëren

Zie voor een volledige lijst met secties en de eigenschappen die beschikbaar zijn voor het definiëren van activiteiten, de [pijplijnen](concepts-pipelines-activities.md) artikel. Deze sectie bevat een lijst met eigenschappen die ondersteund worden door Dynamics bron.

### <a name="dynamics-as-source"></a>Dynamics als bron

Om gegevens te kopiëren van Dynamics, stelt u het brontype in de kopieerbewerking naar **DynamicsSource**. De volgende eigenschappen worden ondersteund in de kopieerbewerking **bron** sectie:

| Eigenschap | Beschrijving | Vereist |
|:--- |:--- |:--- |
| type | De eigenschap type moet worden ingesteld op: **DynamicsSource**  | Ja |
| query  | FetchXML is een eigen querytaal die wordt gebruikt in Microsoft Dynamics (online & lokale). Verderop in dit voorbeeld en meer informatie van [opbouwen van query's met FeachXML](https://msdn.microsoft.com/en-us/library/gg328332.aspx). | Nee (als het 'entityName in de gegevensset is opgegeven)  |

**Voorbeeld:**

```json
"activities":[
    {
        "name": "CopyFromDynamics",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<Dynamics input dataset>",
                "type": "DatasetReference"
            }
        ],
        "outputs": [
            {
                "referenceName": "<output dataset>",
                "type": "DatasetReference"
            }
        ],
        "typeProperties": {
            "source": {
                "type": "DynamicsSource",
                "query": "<FetchXML Query>"
            },
            "sink": {
                "type": "<sink type>"
            }
        }
    }
]
```

### <a name="sample-fetchxml-query"></a>Voorbeeldquery FetchXML

```xml
<fetch>
  <entity name="account">
    <attribute name="accountid" />
    <attribute name="name" />
    <attribute name="marketingonly" />
    <attribute name="modifiedon" />
    <order attribute="modifiedon" descending="false" />
    <filter type="and">
      <condition attribute ="modifiedon" operator="between">
        <value>2017-03-10 18:40:00z</value>
        <value>2017-03-12 20:40:00z</value>
      </condition>
    </filter>
  </entity>
</fetch>
```

## <a name="data-type-mapping-for-dynamics"></a>Gegevenstype toewijzing voor de Dynamics

Bij het kopiëren van gegevens uit Dynamics, worden de volgende toewijzingen van Dynamics gegevenstypen gebruikt voor Azure Data Factory tussentijdse gegevenstypen. Zie [Schema en de gegevens typt toewijzingen](copy-activity-schema-and-type-mapping.md) voor meer informatie over hoe het brontype schema en de gegevens in kopieeractiviteit worden toegewezen aan de sink.

Configureren van het bijbehorende ADF-gegevenstype in de gegevenssetstructuur op basis van uw Dynamics brongegevens typen met behulp van onderstaande toewijzingstabel:

| Het gegevenstype Dynamics | Data factory tussentijdse gegevenstype |
|:--- |:--- |
| AttributeTypeCode.BigInt | Lang |
| AttributeTypeCode.Boolean | Booleaanse waarde |
| AttributeType.Customer | GUID |
| AttributeType.DateTime | Datum/tijd |
| AttributeType.Decimal | Decimale |
| AttributeType.Double | dubbele |
| AttributeType.EntityName | Tekenreeks |
| AttributeType.Integer | Int32 |
| AttributeType.Lookup | GUID |
| AttributeType.ManagedProperty | Booleaanse waarde |
| AttributeType.Memo | Tekenreeks |
| AttributeType.Money | Decimale |
| AttributeType.Owner | GUID |
| AttributeType.Picklist | Int32 |
| AttributeType.Uniqueidentifier | GUID |
| AttributeType.String | Tekenreeks |
| AttributeType.State | Int32 |
| AttributeType.Status | Int32 |

> [!NOTE]
> Het gegevenstype Dynamics AttributeType.CalendarRules en AttributeType.PartyList worden niet ondersteund.


## <a name="next-steps"></a>Volgende stappen
Zie voor een lijst met gegevensarchieven als bronnen en put wordt ondersteund door de kopieeractiviteit in Azure Data Factory, [ondersteunde gegevensarchieven](copy-activity-overview.md##supported-data-stores-and-formats).