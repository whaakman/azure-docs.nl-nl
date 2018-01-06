---
title: "Gegevens kopiëren van/naar Dynamics CRM en 365 met behulp van Azure Data Factory | Microsoft Docs"
description: "Informatie over het kopiëren van gegevens tot Dynamics CRM en 365 naar gegevensarchieven ondersteunde sink (of) uit gegevensarchieven ondersteunde bron tot Dynamics CRM en 365 met behulp van een kopieeractiviteit in een Azure Data Factory-pijplijn."
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
ms.date: 01/05/2018
ms.author: jingwang
ms.openlocfilehash: b9b7091a8cb1de3eefcce77cbf82eedfcb33c787
ms.sourcegitcommit: 1d423a8954731b0f318240f2fa0262934ff04bd9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/05/2018
---
# <a name="copy-data-fromto-dynamics-365dynamics-crm-using-azure-data-factory"></a>Gegevens kopiëren van/naar Dynamics 365 Dynamics CRM met Azure Data Factory

In dit artikel bevat een overzicht van het gebruik van de Kopieeractiviteit in Azure Data Factory om gegevens te kopiëren van en naar Dynamics 365 Dynamics CRM. Dit is gebaseerd op de [activiteit overzicht kopiëren](copy-activity-overview.md) artikel met daarin een algemeen overzicht van de kopieeractiviteit.

> [!NOTE]
> Dit artikel is van toepassing op versie 2 van Data Factory, dat zich momenteel in de previewfase bevindt. Als u van versie 1 van de Data Factory-service gebruikmaakt (GA) is algemeen beschikbaar is, raadpleegt u [Kopieeractiviteit in V1](v1/data-factory-data-movement-activities.md).

## <a name="supported-capabilities"></a>Ondersteunde mogelijkheden

U kunt gegevens van Dynamics 365 Dynamics CRM kopiëren naar een ondersteunde sink-gegevensarchief of gegevens kopiëren van een ondersteunde brongegevensarchief naar Dynamics 365 Dynamics CRM. Zie voor een lijst met gegevensarchieven als bronnen/put wordt ondersteund door de kopieeractiviteit, de [ondersteunde gegevensarchieven](copy-activity-overview.md#supported-data-stores-and-formats) tabel.

Biedt ondersteuning voor deze connector Dynamics hieronder Dynamics versies en verificatietypen (*IFD wordt kort voor internetgerichte implementatie*):

| Dynamics-versies | Verificatietypen | Voorbeelden van de gekoppelde service |
|:--- |:--- |:--- |
| Dynamics 365 online <br> Dynamics CRM online | Office365 | [Dynamics Online + Office365 auth](#dynamics-365-and-dynamics-crm-online) |
| Dynamics 365 on-premises met IFD <br> Dynamics CRM-2016 met on-premises IFD <br> Dynamics CRM-2015 met on-premises IFD | IFD | [Dynamics On-premises met IFD + IFD auth](#dynamics-365-and-dynamics-crm-on-premises-with-ifd) |

Voor de Dynamics 365 met name worden de volgende typen ondersteund:

- Dynamics 365 voor verkoop
- Dynamics 365 voor klantenservice
- Dynamics 365 voor veld Service
- Dynamics 365 voor Project Service Automation
- Dynamics 365 voor Marketing

> [!NOTE]
> Gebruik Dynamics connector moet uw wachtwoord opslaan in Azure Sleutelkluis en de kopie vizier pull van daaruit laat bij het uitvoeren van de gegevens opnieuw te kopiëren. Informatie over het configureren van in [gekoppelde service-eigenschappen](#linked-service-properties) sectie.

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
| wachtwoord | Wachtwoord voor het gebruikersaccount dat u hebt opgegeven voor de gebruikersnaam opgeven. U moet het wachtwoord in de Azure Sleutelkluis te plaatsen en het wachtwoord als een 'AzureKeyVaultSecret' configureren. Klik hier als u meer wilt weten van [referenties opgeslagen in de Sleutelkluis](store-credentials-in-key-vault.md). | Ja |
| connectVia | De [integratie Runtime](concepts-integration-runtime.md) moeten worden gebruikt voor het verbinding maken met het gegevensarchief. Als niet wordt opgegeven, wordt de standaardwaarde Azure integratie Runtime. | Niet voor de gegevensbron, Ja voor sink als bron gekoppeld-service heeft geen IR |

>[!IMPORTANT]
>Als u gegevens **in** Dynamics, standaard Azure integratie Runtime kan niet worden gebruikt om uit te voeren kopiëren. In een ander woord als de bron gekoppeld service beschikt niet over een opgegeven IR expliciet [maken van een Azure-IR](create-azure-integration-runtime.md#create-azure-ir) met een locatie in de buurt van uw Dynamics en koppelen in de gekoppelde service Dynamics als in het volgende voorbeeld.

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
                "secretName": "<secret name in AKV>",
                "store":{
                    "referenceName": "<Azure Key Vault linked service>",
                    "type": "LinkedServiceReference"
                }
            }
        },
        "connectVia": {
            "referenceName": "<name of Integration Runtime>",
            "type": "IntegrationRuntimeReference"
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
| wachtwoord | Wachtwoord voor het gebruikersaccount dat u hebt opgegeven voor de gebruikersnaam opgeven. Houd er rekening mee hebt tot het wachtwoord in de Azure Sleutelkluis te plaatsen en het wachtwoord als een 'AzureKeyVaultSecret' configureren. Klik hier als u meer wilt weten van [referenties opgeslagen in de Sleutelkluis](store-credentials-in-key-vault.md). | Ja |
| connectVia | De [integratie Runtime](concepts-integration-runtime.md) moeten worden gebruikt voor het verbinding maken met het gegevensarchief. Als niet wordt opgegeven, wordt de standaardwaarde Azure integratie Runtime. | Er is geen voor bron Ja voor sink |

>[!IMPORTANT]
>Om gegevens te kopiëren naar Dynamics, expliciet [maken van een Azure-IR](create-azure-integration-runtime.md#create-azure-ir) met de locatie in de buurt van uw Dynamics en koppelen in de gekoppelde service als het volgende voorbeeld.

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
                "secretName": "<secret name in AKV>",
                "store":{
                    "referenceName": "<Azure Key Vault linked service>",
                    "type": "LinkedServiceReference"
                }
            }
        },
        "connectVia": {
            "referenceName": "<name of Integration Runtime>",
            "type": "IntegrationRuntimeReference"
        }
    }
}
```

## <a name="dataset-properties"></a>Eigenschappen van gegevensset

Zie voor een volledige lijst met secties en de eigenschappen die beschikbaar zijn voor het definiëren van gegevenssets, de [gegevenssets](concepts-datasets-linked-services.md) artikel. Deze sectie bevat een lijst met eigenschappen die ondersteund worden door Dynamics dataset.

Om gegevens te kopiëren van/naar Dynamics, stel de eigenschap type van de gegevensset **DynamicsEntity**. De volgende eigenschappen worden ondersteund:

| Eigenschap | Beschrijving | Vereist |
|:--- |:--- |:--- |
| type | De eigenschap type van de gegevensset moet worden ingesteld op: **DynamicsEntity** |Ja |
| entityName | De logische naam van de entiteit ophalen. | Er is geen voor bron (als 'vragen' in de activiteitbron is opgegeven), maar wel voor sink |

> [!IMPORTANT]
>- **De sectie 'structuur' is vereist als het kopiëren van gegevens uit Dynamics** in Dynamics gegevensset, definieert een naam en het gegevenstype kolomtype voor Dynamics gegevens die u kopiëren wilt via. Klik hier als u meer wilt weten van [gegevenssetstructuur](concepts-datasets-linked-services.md#dataset-structure) en de [gegevenstypetoewijzing voor Dynamics](#data-type-mapping-for-dynamics).
>- **Bij het kopiëren van gegevens naar Dynamics, de sectie "structuur" is optioneel** in Dynamics gegevensset. Welke kolom(men) kopiëren naar wordt bepaald door het schema van de bron. Als de bron CSV-bestand zonder header is, 'de structuur' in de invoergegevensset opgeven met de naam en het gegevenstype kolomtype dat wordt toegewezen aan de velden in CSV-bestand, één voor één in volgorde.

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

Zie voor een volledige lijst met secties en de eigenschappen die beschikbaar zijn voor het definiëren van activiteiten, de [pijplijnen](concepts-pipelines-activities.md) artikel. Deze sectie bevat een lijst met eigenschappen die worden ondersteund door Dynamics bron- en sink.

### <a name="dynamics-as-source"></a>Dynamics als bron

Om gegevens te kopiëren van Dynamics, stelt u het brontype in de kopieerbewerking naar **DynamicsSource**. De volgende eigenschappen worden ondersteund in de kopieerbewerking **bron** sectie:

| Eigenschap | Beschrijving | Vereist |
|:--- |:--- |:--- |
| type | De eigenschap type van de bron voor kopiëren-activiteit moet worden ingesteld op: **DynamicsSource**  | Ja |
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

### <a name="dynamics-as-sink"></a>Dynamics als sink

Om gegevens te kopiëren naar Dynamics, stelt u het sink-type in de kopieerbewerking naar **DynamicsSink**. De volgende eigenschappen worden ondersteund in de kopieerbewerking **sink** sectie:

| Eigenschap | Beschrijving | Vereist |
|:--- |:--- |:--- |
| type | De eigenschap type van de activiteit kopiëren sink moet worden ingesteld op: **DynamicsSink**  | Ja |
| WriteBehavior | Het gedrag van het schrijven van de bewerking.<br/>De waarde is toegestaan: **"Upsert"**. | Ja |
| writeBatchSize | Het aantal rijen van de gegevens die naar Dynamics zijn geschreven in elke batch. | Nee (de standaardwaarde is 10) |
| ignoreNullValues | Geeft aan of voor het negeren van null-waarden van invoergegevens (met uitzondering van sleutelvelden) tijdens de schrijfbewerking.<br/>Toegestane waarden zijn: **true**, en **false**.<br>-true: laat de gegevens in de doel-object niet worden gewijzigd bij het uitvoeren van bewerking upsert/bijwerken en voeg standaardwaarde gedefinieerd bij het uitvoeren van insert-bewerking.<br/>-false: de gegevens in het doelobject op NULL bijwerken bij het uitvoeren van bewerking upsert/bijwerken en voeg NULL-waarde bij het uitvoeren van insert-bewerking.  | Nee (de standaardwaarde is ONWAAR) |

>[!NOTE]
>De standaardwaarde van de activiteit writeBatchSize en kopieer sink [parallelCopies](copy-activity-performance.md#parallel-copy) voor Dynamics sink zijn beide 10, wat betekent dat 100 records worden verzonden naar Dynamics gelijktijdig.

**Voorbeeld:**

```json
"activities":[
    {
        "name": "CopyToDynamics",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<input dataset>",
                "type": "DatasetReference"
            }
        ],
        "outputs": [
            {
                "referenceName": "<Dynamics output dataset>",
                "type": "DatasetReference"
            }
        ],
        "typeProperties": {
            "source": {
                "type": "<source type>"
            },
            "sink": {
                "type": "DynamicsSink",
                "writeBehavior": "Upsert",
                "writeBatchSize": 10,
                "ignoreNullValues": true
            }
        }
    }
]
```

## <a name="data-type-mapping-for-dynamics"></a>Gegevenstype toewijzing voor de Dynamics

Bij het kopiëren van gegevens uit Dynamics, worden de volgende toewijzingen van Dynamics gegevenstypen gebruikt voor Azure Data Factory tussentijdse gegevenstypen. Zie [Schema en de gegevens typt toewijzingen](copy-activity-schema-and-type-mapping.md) voor meer informatie over hoe het brontype schema en de gegevens in kopieeractiviteit worden toegewezen aan de sink.

Configureren van het bijbehorende Data Factory-gegevenstype in de gegevenssetstructuur op basis van uw Dynamics brongegevens typen met behulp van onderstaande toewijzingstabel:

| Het gegevenstype Dynamics | Data factory tussentijdse gegevenstype | Ondersteund als bron | Als sink ondersteund |
|:--- |:--- |:--- |:--- |
| AttributeTypeCode.BigInt | Lang | ✓ | ✓ |
| AttributeTypeCode.Boolean | Boole-waarde | ✓ | ✓ |
| AttributeType.Customer | GUID | ✓ |  |
| AttributeType.DateTime | Datum en tijd | ✓ | ✓ |
| AttributeType.Decimal | Decimale | ✓ | ✓ |
| AttributeType.Double | dubbele | ✓ | ✓ |
| AttributeType.EntityName | Tekenreeks | ✓ | ✓ |
| AttributeType.Integer | Int32 | ✓ | ✓ |
| AttributeType.Lookup | GUID | ✓ |  |
| AttributeType.ManagedProperty | Boole-waarde | ✓ |  |
| AttributeType.Memo | Tekenreeks | ✓ | ✓ |
| AttributeType.Money | Decimale | ✓ | ✓ |
| AttributeType.Owner | GUID | ✓ | |
| AttributeType.Picklist | Int32 | ✓ | ✓ |
| AttributeType.Uniqueidentifier | GUID | ✓ | ✓ |
| AttributeType.String | Tekenreeks | ✓ | ✓ |
| AttributeType.State | Int32 | ✓ | ✓ |
| AttributeType.Status | Int32 | ✓ | ✓ |


> [!NOTE]
> Het gegevenstype Dynamics AttributeType.CalendarRules en AttributeType.PartyList worden niet ondersteund.

## <a name="next-steps"></a>Volgende stappen
Zie voor een lijst met gegevensarchieven als bronnen en put wordt ondersteund door de kopieeractiviteit in Azure Data Factory, [ondersteunde gegevensarchieven](copy-activity-overview.md#supported-data-stores-and-formats).