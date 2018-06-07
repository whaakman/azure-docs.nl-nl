---
title: Gegevens kopiëren van en naar Dynamics CRM of Dynamics 365 (algemene gegevensservice) met behulp van Azure Data Factory | Microsoft Docs
description: Informatie over het kopiëren van gegevens uit Microsoft Dynamics CRM of Microsoft Dynamics 365 (algemene gegevensservice) worden ondersteund sink gegevensarchieven of ondersteund van bron gegevensarchieven Dynamics CRM of Dynamics 365, met behulp van een kopieeractiviteit in een data factory-pijplijn.
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
ms.date: 05/02/2018
ms.author: jingwang
ms.openlocfilehash: 848b571dd2664d4910d669faa4dce1aad16cd528
ms.sourcegitcommit: 6cf20e87414dedd0d4f0ae644696151e728633b6
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/06/2018
ms.locfileid: "34808424"
---
# <a name="copy-data-from-and-to-dynamics-365-common-data-service-or-dynamics-crm-by-using-azure-data-factory"></a>Gegevens kopiëren van en naar Dynamics 365 (algemene gegevensservice) of Dynamics CRM met behulp van Azure Data Factory

In dit artikel bevat een overzicht van het gebruik van de Kopieeractiviteit in Azure Data Factory om gegevens te kopiëren van en naar Microsoft Dynamics 365 of Microsoft Dynamics CRM. Dit is gebaseerd op de [Kopieeractiviteit overzicht](copy-activity-overview.md) artikel met daarin een algemeen overzicht van de Kopieeractiviteit.

> [!NOTE]
> Dit artikel is van toepassing op versie 2 van Data Factory, dat zich momenteel in de previewfase bevindt. Als u versie 1 van de Data Factory, die in het algemeen beschikbaar is, Zie [Kopieeractiviteit in versie 1](v1/data-factory-data-movement-activities.md).

## <a name="supported-capabilities"></a>Ondersteunde mogelijkheden

U kunt gegevens uit Dynamics 365 (algemene gegevensservice) of Dynamics CRM kopiëren naar een ondersteunde sink-gegevensarchief. U kunt ook gegevens uit alle ondersteunde brongegevensarchief kopiëren naar Dynamics 365 (algemene gegevensservice) of Dynamics CRM. Zie voor een lijst met gegevensarchieven als bronnen of PUT wordt ondersteund door de kopieeractiviteit, de [ondersteunde gegevensarchieven](copy-activity-overview.md#supported-data-stores-and-formats) tabel.

Deze connector Dynamics ondersteunt de volgende versies van Dynamics en verificatietypen. (IFD is kort voor internet facing deployment).

| Dynamics-versies | Verificatietypen | Voorbeelden van de gekoppelde service |
|:--- |:--- |:--- |
| Dynamics 365 online <br> Dynamics CRM Online | Office365 | [Dynamics online + Office365 auth](#dynamics-365-and-dynamics-crm-online) |
| Dynamics 365 on-premises met IFD <br> Dynamics CRM-2016 met on-premises IFD <br> Dynamics CRM-2015 met on-premises IFD | IFD | [Dynamics on-premises met IFD + IFD auth](#dynamics-365-and-dynamics-crm-on-premises-with-ifd) |

Voor de Dynamics 365 met name worden de volgende typen ondersteund:

- Dynamics 365 voor verkoop
- Dynamics 365 voor klantenservice
- Dynamics 365 voor veld Service
- Dynamics 365 voor Project Service Automation
- Dynamics 365 voor Marketing

Andere toepassing zoals Operations en Finance, Talent typen, enzovoort worden niet ondersteund.

## <a name="get-started"></a>Aan de slag

[!INCLUDE [data-factory-v2-connector-get-started-2](../../includes/data-factory-v2-connector-get-started-2.md)]

De volgende secties bevatten informatie over de eigenschappen die worden gebruikt voor het definiëren van Data Factory-entiteiten specifieke naar Dynamics.

## <a name="linked-service-properties"></a>Eigenschappen van de gekoppelde service

De volgende eigenschappen worden ondersteund voor de Dynamics gekoppelde service.

### <a name="dynamics-365-and-dynamics-crm-online"></a>Dynamics 365 en Dynamics CRM Online

| Eigenschap | Beschrijving | Vereist |
|:--- |:--- |:--- |
| type | De eigenschap type moet worden ingesteld op **Dynamics**. | Ja |
| deploymentType | Het implementatietype van de Dynamics-exemplaar. Dit moet **'Online'** voor Dynamics online. | Ja |
| serviceUri | De service-URL van uw Dynamics exemplaar, bijvoorbeeld `https://adfdynamics.crm.dynamics.com`. | Ja |
| authenticationType | Het verificatietype dat verbinding maken met een Dynamics-server. Geef **'Office365'** voor Dynamics online. | Ja |
| gebruikersnaam | Geef de naam van de gebruiker verbinding maken met Dynamics. | Ja |
| wachtwoord | Geef het wachtwoord voor het gebruikersaccount dat u hebt opgegeven voor de gebruikersnaam. Dit veld markeren als een SecureString Bewaar deze zorgvuldig in Data Factory of [verwijzen naar een geheim dat is opgeslagen in Azure Key Vault](store-credentials-in-key-vault.md). | Ja |
| connectVia | De [integratie runtime](concepts-integration-runtime.md) moeten worden gebruikt voor het verbinding maken met het gegevensarchief. Als niet wordt opgegeven, wordt de standaardwaarde Azure integratie Runtime. | Niet voor de gegevensbron, Ja voor sink als de bron gekoppeld service beschikt niet over een integratie-runtime |

>[!IMPORTANT]
>Wanneer u gegevens naar Dynamics kopiëren, kan niet de standaard Azure integratie Runtime worden gebruikt om uit te voeren kopiëren. Met andere woorden, als de bron gekoppeld service beschikt niet over een runtime opgegeven integratie expliciet [maken van een Azure-integratie Runtime](create-azure-integration-runtime.md#create-azure-ir) met een locatie in de buurt van uw Dynamics-exemplaar. Koppelen in de gekoppelde service Dynamics zoals in het volgende voorbeeld.

>[!NOTE]
>De Dynamics-connector gebruikt optionele 'organisatienaam' als eigenschap wilt gebruiken om uw Dynamics CRM/365 Online-instantie te identificeren. Terwijl deze werken blijft, is het worden voorgesteld naar de nieuwe 'serviceUri'-eigenschap in plaats daarvan opgeven voor het verkrijgen van betere prestaties voor instantie detectie.

**Voorbeeld: Office365 verificatie met behulp van Dynamics online**

```json
{
    "name": "DynamicsLinkedService",
    "properties": {
        "type": "Dynamics",
        "description": "Dynamics online linked service using Office365 authentication",
        "typeProperties": {
            "deploymentType": "Online",
            "serviceUri": "https://adfdynamics.crm.dynamics.com",
            "authenticationType": "Office365",
            "username": "test@contoso.onmicrosoft.com",
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

### <a name="dynamics-365-and-dynamics-crm-on-premises-with-ifd"></a>Dynamics 365 en Dynamics CRM on-premises met IFD

*Aanvullende eigenschappen die met Dynamics online vergelijken zijn 'hostnaam' en 'poort'.*

| Eigenschap | Beschrijving | Vereist |
|:--- |:--- |:--- |
| type | De eigenschap type moet worden ingesteld op **Dynamics**. | Ja |
| deploymentType | Het implementatietype van de Dynamics-exemplaar. Dit moet **'OnPremisesWithIfd'** voor Dynamics met on-premises IFD.| Ja |
| hostName | De hostnaam van de on-premises Dynamics-server. | Ja |
| poort | De poort van de on-premises Dynamics-server. | Nee, standaard is dit 443 |
| Organisatienaam | De naam van de organisatie van de Dynamics-exemplaar. | Ja |
| authenticationType | Het verificatietype dat verbinding maken met de Dynamics-server. Geef **'Ifd'** voor Dynamics met on-premises IFD. | Ja |
| gebruikersnaam | Geef de naam van de gebruiker verbinding maken met Dynamics. | Ja |
| wachtwoord | Geef het wachtwoord voor het gebruikersaccount dat u hebt opgegeven voor de gebruikersnaam. U kunt kiezen voor dit veld markeren als een SecureString veilig opslaan in ADF of wachtwoord worden opgeslagen in Azure Sleutelkluis en de kopieeractiviteit pull daar bij het uitvoeren van de gegevens opnieuw te kopiëren: meer informatie kunt [referenties opgeslagen in de Sleutelkluis](store-credentials-in-key-vault.md). | Ja |
| connectVia | De [integratie runtime](concepts-integration-runtime.md) moeten worden gebruikt voor het verbinding maken met het gegevensarchief. Als niet wordt opgegeven, wordt de standaardwaarde Azure integratie Runtime. | Er is geen voor bron Ja voor sink |

>[!IMPORTANT]
>Om gegevens te kopiëren naar Dynamics, expliciet [maken van een Azure-integratie Runtime](create-azure-integration-runtime.md#create-azure-ir) met de locatie in de buurt van uw Dynamics-exemplaar. Koppelen in de gekoppelde service zoals in het volgende voorbeeld.

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

## <a name="dataset-properties"></a>Eigenschappen van gegevensset

Zie voor een volledige lijst met secties en de eigenschappen die beschikbaar zijn voor het definiëren van gegevenssets, de [gegevenssets](concepts-datasets-linked-services.md) artikel. Deze sectie bevat een lijst met eigenschappen die ondersteund worden door Dynamics dataset.

Om gegevens te kopiëren van en naar Dynamics, stel de eigenschap type van de gegevensset **DynamicsEntity**. De volgende eigenschappen worden ondersteund.

| Eigenschap | Beschrijving | Vereist |
|:--- |:--- |:--- |
| type | De eigenschap type van de gegevensset moet worden ingesteld op **DynamicsEntity**. |Ja |
| entityName | De logische naam van de entiteit ophalen. | Er is geen voor bron (als 'query' in de activiteitbron is opgegeven), maar wel voor sink |

> [!IMPORTANT]
>- Als u gegevens uit Dynamics kopieert, wordt de sectie 'structuur' in de gegevensset Dynamics vereist. Hiermee definieert u het kolomtype naam en het gegevenstype voor de Dynamics-gegevens die u kopiëren wilt via. Zie voor meer informatie, [gegevenssetstructuur](concepts-datasets-linked-services.md#dataset-structure) en [gegevenstypetoewijzing voor Dynamics](#data-type-mapping-for-dynamics).
>- Wanneer u gegevens naar Dynamics kopiëren, is de sectie "structuur" optioneel in de gegevensset Dynamics. Kolommen die u wilt kopiëren naar wordt bepaald door het schema van de bron. Als de bron een CSV-bestand zonder header is 'de structuur' in de invoergegevensset opgeven met de naam en het gegevenstype kolomtype. Ze worden toegewezen aan de velden in het CSV-bestand één voor één in volgorde.

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
        "typeProperties": {
            "entityName": "account"
        },
        "linkedServiceName": {
            "referenceName": "<Dynamics linked service name>",
            "type": "linkedservicereference"
        }
    }
}
```

## <a name="copy-activity-properties"></a>Eigenschappen van de kopieeractiviteit

Zie voor een volledige lijst met secties en de eigenschappen die beschikbaar zijn voor het definiëren van activiteiten, de [pijplijnen](concepts-pipelines-activities.md) artikel. Deze sectie bevat een lijst met eigenschappen die ondersteund worden door Dynamics bron- en sink-typen.

### <a name="dynamics-as-a-source-type"></a>Dynamics als een type gegevensbron

Om gegevens te kopiëren van Dynamics, stelt u het brontype in de kopieerbewerking naar **DynamicsSource**. De volgende eigenschappen worden ondersteund in de kopieerbewerking **bron** sectie.

| Eigenschap | Beschrijving | Vereist |
|:--- |:--- |:--- |
| type | De eigenschap type van de bron voor kopiëren-activiteit moet worden ingesteld op **DynamicsSource**. | Ja |
| query | FetchXML is een eigen querytaal die wordt gebruikt in Dynamics (online en on-premises). Zie het volgende voorbeeld Zie voor meer informatie, [opbouwen van query's met FeachXML](https://msdn.microsoft.com/library/gg328332.aspx). | Nee (als het 'entityName in de gegevensset is opgegeven) |

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

### <a name="dynamics-as-a-sink-type"></a>Dynamics als sink-type

Om gegevens te kopiëren naar Dynamics, stelt u het sink-type in de kopieerbewerking naar **DynamicsSink**. De volgende eigenschappen worden ondersteund in de kopieerbewerking **sink** sectie.

| Eigenschap | Beschrijving | Vereist |
|:--- |:--- |:--- |
| type | De eigenschap type van de activiteit kopiëren sink moet worden ingesteld op **DynamicsSink**. | Ja |
| WriteBehavior | Het gedrag van het schrijven van de bewerking.<br/>De waarde is toegestaan **"Upsert"**. | Ja |
| writeBatchSize | Het aantal rijen van de gegevens die naar Dynamics zijn geschreven in elke batch. | Nee (de standaardwaarde is 10) |
| ignoreNullValues | Hiermee wordt aangegeven of null-waarden van invoergegevens (met uitzondering van sleutelvelden) tijdens een schrijfactie negeren.<br/>Toegestane waarden zijn **true** en **false**.<br>- **De waarde True**: laat de gegevens in het doelobject ongewijzigd wanneer u een bewerking upsert of bij te werken. Plaats een gedefinieerde standaardwaarde als u een insert-bewerking doet.<br/>- **ONWAAR**: de gegevens in het doelobject op NULL bijwerken wanneer u een bewerking upsert of bij te werken. Plaats een NULL-waarde als u een insert-bewerking doet. | Nee (de standaardwaarde is ONWAAR) |

>[!NOTE]
>De standaardwaarde van de sink '**writeBatchSize**'en de kopieeractiviteit'**[parallelCopies](copy-activity-performance.md#parallel-copy)**' zijn beide 10 voor de Dynamics sink. 100 records worden daarom ingediend bij Dynamics het gelijktijdig.

Voor de Dynamics 365 online, is een limiet van [2 gelijktijdige batch-aanroepen per organisatie](https://msdn.microsoft.com/en-us/library/jj863631.aspx#Run-time%20limitations). Als deze limiet wordt overschreden, wordt een 'Server bezet' fout gegenereerd voordat de eerste aanvraag ooit wordt uitgevoerd. Houden 'writeBatchSize' kleiner dan of gelijk aan 10 zou voorkomen dat dergelijke beperking van gelijktijdige aanroepen.

De optimale combinatie van '**writeBatchSize**'en'**parallelCopies**' is afhankelijk van het schema van de entiteit bijvoorbeeld aantal kolommen, rijgrootte, aantal werkstromen/plugins/workflow-activiteiten die zijn aangesloten naar deze aanroepen, enzovoort. De standaardinstelling van 10 writeBatchSize * 10 parallelCopies is de aanbeveling volgens Dynamics-service, die voor de meeste Dynamics entiteiten echter mogelijk niet optimale prestaties zou werken. Door de combinatie in de instellingen van de activiteit exemplaar aan te passen, kunt u de prestaties afstemmen.

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

Wanneer u gegevens uit Dynamics kopieert, worden de volgende toewijzingen van Dynamics gegevenstypen gebruikt voor Data Factory tussentijdse gegevenstypen. Zie voor meer informatie over hoe het brontype schema en de gegevens in de kopieerbewerking wordt toegewezen aan de sink, [Schema en de gegevens typt toewijzingen](copy-activity-schema-and-type-mapping.md).

Configureer het bijbehorende Data Factory-gegevenstype in de gegevenssetstructuur van een op basis van de bron-Dynamics-gegevenstype met behulp van de volgende toewijzingstabel.

| Het gegevenstype Dynamics | Data Factory tussentijdse gegevenstype | Ondersteund als bron | Als sink ondersteund |
|:--- |:--- |:--- |:--- |
| AttributeTypeCode.BigInt | Lang | ✓ | ✓ |
| AttributeTypeCode.Boolean | Boole-waarde | ✓ | ✓ |
| AttributeType.Customer | GUID | ✓ | | 
| AttributeType.DateTime | Datum en tijd | ✓ | ✓ |
| AttributeType.Decimal | Decimale | ✓ | ✓ |
| AttributeType.Double | dubbele | ✓ | ✓ |
| AttributeType.EntityName | Reeks | ✓ | ✓ |
| AttributeType.Integer | Int32 | ✓ | ✓ |
| AttributeType.Lookup | GUID | ✓ | ✓ |
| AttributeType.ManagedProperty | Boole-waarde | ✓ | |
| AttributeType.Memo | Reeks | ✓ | ✓ |
| AttributeType.Money | Decimale | ✓ | ✓ |
| AttributeType.Owner | GUID | ✓ | |
| AttributeType.Picklist | Int32 | ✓ | ✓ |
| AttributeType.Uniqueidentifier | GUID | ✓ | ✓ |
| AttributeType.String | Reeks | ✓ | ✓ |
| AttributeType.State | Int32 | ✓ | ✓ |
| AttributeType.Status | Int32 | ✓ | ✓ |


> [!NOTE]
> De gegevenstypen Dynamics AttributeType.CalendarRules en AttributeType.PartyList worden niet ondersteund.

## <a name="next-steps"></a>Volgende stappen
Zie voor een lijst van gegevensarchieven als bronnen en put wordt ondersteund door de kopieeractiviteit in Gegevensfactory [ondersteunde gegevensarchieven](copy-activity-overview.md#supported-data-stores-and-formats).
