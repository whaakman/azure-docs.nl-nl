---
title: Gegevens kopiëren van en naar Dynamics CRM- of Dynamics 365 (Common Data Service) met behulp van Azure Data Factory | Microsoft Docs
description: Informatie over het kopiëren van gegevens uit Microsoft Dynamics CRM of Microsoft Dynamics 365 (Common Data Service) ondersteund sink-gegevensopslag, of van bron-gegevensarchieven aan Dynamics CRM- of Dynamics 365, wordt ondersteund met behulp van een kopieeractiviteit in een data factory-pijplijn.
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
ms.openlocfilehash: 3f45f9337a5522f490c268bbdae3ef1a41205175
ms.sourcegitcommit: 0b4da003fc0063c6232f795d6b67fa8101695b61
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/05/2018
ms.locfileid: "37859371"
---
# <a name="copy-data-from-and-to-dynamics-365-common-data-service-or-dynamics-crm-by-using-azure-data-factory"></a>Gegevens kopiëren van en naar Dynamics 365 (Common Data Service) of Dynamics CRM met behulp van Azure Data Factory

In dit artikel bevat een overzicht over het gebruik van de Kopieeractiviteit in Azure Data Factory om gegevens te kopiëren van en naar Microsoft Dynamics 365 of Microsoft Dynamics CRM. Dit is gebaseerd op de [overzicht van Kopieeractiviteit](copy-activity-overview.md) artikel met daarin een algemeen overzicht van de Kopieeractiviteit.

## <a name="supported-capabilities"></a>Ondersteunde mogelijkheden

U kunt gegevens uit Dynamics 365 (Common Data Service) of Dynamics CRM kopiëren naar een ondersteunde sink-gegevensopslag. U kunt ook gegevens uit een ondersteund brongegevensarchief kopiëren naar Dynamics 365 (Common Data Service) of Dynamics CRM. Zie voor een lijst met gegevensarchieven die worden ondersteund als gegevensbronnen of PUT voor de kopieeractiviteit, de [ondersteunde gegevensarchieven](copy-activity-overview.md#supported-data-stores-and-formats) tabel.

Deze Dynamics-connector ondersteunt de volgende versies van Dynamics en de verificatietypen. (IFD is kort voor internetgerichte-implementatie.)

| Dynamics-versies | Verificatietypen | Voorbeelden van de gekoppelde service |
|:--- |:--- |:--- |
| Dynamics 365 online <br> Dynamics CRM Online | Office365 | [Dynamics online en Office 365-verificatie](#dynamics-365-and-dynamics-crm-online) |
| Dynamics 365 on-premises met IFD <br> Dynamics CRM 2016 on-premises met IFD <br> Dynamics CRM 2015 on-premises met IFD | IFD | [Dynamics on-premises met IFD + IFD auth](#dynamics-365-and-dynamics-crm-on-premises-with-ifd) |

Voor Dynamics 365 met name worden de volgende typen ondersteund:

- Dynamics 365 for Sales
- Dynamics 365 for Customer Service
- Dynamics 365 for Field Service
- Dynamics 365 voor Project Service Automation
- Dynamics 365 voor Marketing

Andere toepassing van het type bijvoorbeeld Operations en financiën, Talent, enzovoort worden niet ondersteund.

## <a name="get-started"></a>Aan de slag

[!INCLUDE [data-factory-v2-connector-get-started](../../includes/data-factory-v2-connector-get-started.md)]

De volgende secties bevatten meer informatie over eigenschappen die worden gebruikt voor het definiëren van Data Factory-entiteiten specifieke en Dynamics.

## <a name="linked-service-properties"></a>Eigenschappen van de gekoppelde service

De volgende eigenschappen worden ondersteund voor de gekoppelde service van Dynamics.

### <a name="dynamics-365-and-dynamics-crm-online"></a>Dynamics 365 en Dynamics CRM Online

| Eigenschap | Beschrijving | Vereist |
|:--- |:--- |:--- |
| type | De eigenschap type moet worden ingesteld op **Dynamics**. | Ja |
| deploymentType | Het implementatietype van de Dynamics-instantie. Het moet **'Online'** voor Dynamics online. | Ja |
| serviceUri | De service-URL van uw Dynamics-exemplaar, bijvoorbeeld `https://adfdynamics.crm.dynamics.com`. | Ja |
| authenticationType | Het verificatietype voor verbinding met een Dynamics-server. Geef **'Office 365'** voor Dynamics online. | Ja |
| gebruikersnaam | Geef de naam van de gebruiker verbinding maken met Dynamics. | Ja |
| wachtwoord | Geef het wachtwoord voor het gebruikersaccount dat u hebt opgegeven voor de gebruikersnaam. Dit veld markeren als een SecureString Bewaar deze zorgvuldig in Data Factory, of [verwijzen naar een geheim opgeslagen in Azure Key Vault](store-credentials-in-key-vault.md). | Ja |
| connectVia | De [integratieruntime](concepts-integration-runtime.md) moet worden gebruikt verbinding maken met het gegevensarchief. Als niet is opgegeven, wordt de standaard Azure Integration Runtime. | Niet voor bron, Ja voor sink als de bron gekoppelde service beschikt niet over een integratieruntime |

>[!IMPORTANT]
>Wanneer u gegevens naar Dynamics kopiëren, kan de standaard Azure Integration Runtime kan niet worden gebruikt voor het uitvoeren van de kopie. Met andere woorden, als uw bron gekoppelde service beschikt niet over een opgegeven integration-runtime expliciet [maken van een Azure Integration Runtime](create-azure-integration-runtime.md#create-azure-ir) met een locatie in de buurt van uw Dynamics-exemplaar. Koppel deze in de gekoppelde service Dynamics zoals in het volgende voorbeeld.

>[!NOTE]
>De Dynamics-connector gebruikt voor het optionele "organisatienaam" eigenschap gebruiken om uw Dynamics CRM/365 Online exemplaar te identificeren. Terwijl deze werken blijft, is het worden voorgesteld om op te geven van de eigenschap voor de nieuwe 'serviceUri' in plaats daarvan te krijgen van betere prestaties voor detectie.

**Voorbeeld: Office 365-verificatie met behulp van Dynamics online**

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

*De aanvullende eigenschappen die zich tot Dynamics online verhouden zijn 'hostnaam' en 'poort'.*

| Eigenschap | Beschrijving | Vereist |
|:--- |:--- |:--- |
| type | De eigenschap type moet worden ingesteld op **Dynamics**. | Ja |
| deploymentType | Het implementatietype van de Dynamics-instantie. Het moet **"OnPremisesWithIfd"** voor Dynamics on-premises met IFD.| Ja |
| hostName | De hostnaam van de on-premises Dynamics-server. | Ja |
| poort | De poort van de on-premises Dynamics-server. | Nee, standaard is dit 443 |
| Organisatienaam | De naam van de organisatie van de Dynamics-instantie. | Ja |
| authenticationType | Het verificatietype voor verbinding met de Dynamics-server. Geef **"Ifd"** voor Dynamics on-premises met IFD. | Ja |
| gebruikersnaam | Geef de naam van de gebruiker verbinding maken met Dynamics. | Ja |
| wachtwoord | Geef het wachtwoord voor het gebruikersaccount dat u hebt opgegeven voor de gebruikersnaam. U kunt kiezen voor dit veld markeren als een SecureString veilig opslaan in ADF of wachtwoord opslaan in Azure Key Vault en laat de copy-activiteit ophalen van daaruit bij het uitvoeren van het kopiëren van gegevens: meer informatie [referenties Store in Key Vault](store-credentials-in-key-vault.md). | Ja |
| connectVia | De [integratieruntime](concepts-integration-runtime.md) moet worden gebruikt verbinding maken met het gegevensarchief. Als niet is opgegeven, wordt de standaard Azure Integration Runtime. | Nee voor bron, Ja voor sink |

>[!IMPORTANT]
>Gegevens te kopiëren naar Dynamics, expliciet [maken van een Azure Integration Runtime](create-azure-integration-runtime.md#create-azure-ir) door de locatie in de buurt van uw Dynamics-exemplaar. Koppel deze in de gekoppelde service zoals in het volgende voorbeeld.

**Voorbeeld: Dynamics on-premises met IFD IFD-verificatie**

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

Zie voor een volledige lijst van de secties en eigenschappen die beschikbaar zijn voor het definiëren van gegevenssets, de [gegevenssets](concepts-datasets-linked-services.md) artikel. Deze sectie bevat een lijst met eigenschappen die worden ondersteund door Dynamics-gegevensset.

Als u wilt kopiëren van gegevens van en naar Dynamics, stel de eigenschap type van de gegevensset in **DynamicsEntity**. De volgende eigenschappen worden ondersteund.

| Eigenschap | Beschrijving | Vereist |
|:--- |:--- |:--- |
| type | De eigenschap type van de gegevensset moet worden ingesteld op **DynamicsEntity**. |Ja |
| entityName | De logische naam van de entiteit om op te halen. | Nee voor bron (als 'query' in de bron van de activiteit is opgegeven), Ja voor sink |

> [!IMPORTANT]
>- Wanneer u gegevens van Dynamics kopieert, wordt de sectie "structuur" is vereist in de Dynamics-gegevensset. Hiermee wordt de kolom naam en het gegevenstype voor de Dynamics-gegevens die u kopiëren wilt via gedefinieerd. Zie voor meer informatie, [gegevenssetstructuur](concepts-datasets-linked-services.md#dataset-structure) en [gegevenstypetoewijzing voor Dynamics](#data-type-mapping-for-dynamics).
>- Wanneer u gegevens naar Dynamics kopiëren, is de sectie "structuur" optioneel in de Dynamics-gegevensset. Welke kolommen u wilt kopiëren naar wordt bepaald door het schema van de bron. Als de bron is een CSV-bestand zonder header voor de 'structuur"in de invoergegevensset opgeven met de kolom naam en het gegevenstype. Deze worden toegewezen aan de velden in het CSV-bestand één voor één in volgorde.

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

Zie voor een volledige lijst van de secties en eigenschappen die beschikbaar zijn voor het definiëren van activiteiten, de [pijplijnen](concepts-pipelines-activities.md) artikel. Deze sectie bevat een lijst met eigenschappen die worden ondersteund door Dynamics-bron en sink-typen.

### <a name="dynamics-as-a-source-type"></a>Dynamics als een brontype

Om gegevens te kopiëren van Dynamics, stelt u het brontype in de kopieeractiviteit naar **DynamicsSource**. De volgende eigenschappen worden ondersteund in de kopieeractiviteit **bron** sectie.

| Eigenschap | Beschrijving | Vereist |
|:--- |:--- |:--- |
| type | De eigenschap type van de bron voor kopiëren-activiteit moet worden ingesteld op **DynamicsSource**. | Ja |
| query | FetchXML is een eigen querytaal die wordt gebruikt in Dynamics (online en on-premises). Zie het volgende voorbeeld Zie voor meer informatie, [query's met FeachXML](https://msdn.microsoft.com/library/gg328332.aspx). | Nee (als 'entityName' in de gegevensset is opgegeven) |

>[!NOTE]
>De kolom PK wordt altijd uit worden gekopieerd, zelfs als de kolom projectie die u in de FetchXML-query configureert niet het bevat.

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

### <a name="sample-fetchxml-query"></a>Voorbeeld FetchXML-query

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

### <a name="dynamics-as-a-sink-type"></a>Dynamics als een sink-type

Om gegevens te kopiëren naar Dynamics, stelt u het sink-type in de kopieeractiviteit naar **DynamicsSink**. De volgende eigenschappen worden ondersteund in de kopieeractiviteit **sink** sectie.

| Eigenschap | Beschrijving | Vereist |
|:--- |:--- |:--- |
| type | De eigenschap type van de kopie-activiteit-sink moet worden ingesteld op **DynamicsSink**. | Ja |
| writeBehavior | Het gedrag van het schrijven van de bewerking.<br/>Toegestane waarde is **"Upsert"**. | Ja |
| writeBatchSize | Het aantal rijen van de gegevens die naar Dynamics zijn geschreven in elke batch. | Nee (de standaardwaarde is 10) |
| ignoreNullValues | Hiermee wordt aangegeven of null-waarden van invoergegevens (met uitzondering van velden voor sleutels) tijdens een schrijfactie negeren.<br/>Toegestane waarden zijn **waar** en **false**.<br>- **De waarde True**: laat de gegevens in het doelobject ongewijzigd wanneer u een bewerking upsert/bijwerken. Voeg een gedefinieerde standaardwaarde wanneer u een insert-bewerking.<br/>- **De waarde False**: de gegevens in het doelobject op NULL bijwerken wanneer u een bewerking upsert/bijwerken. Voeg een NULL-waarde als u een insert-bewerking. | Nee (de standaardinstelling is false) |

>[!NOTE]
>De standaardwaarde van de sink "**writeBatchSize**'en de kopieeractiviteit'**[parallelCopies](copy-activity-performance.md#parallel-copy)**' zijn beide 10 voor de Dynamics-sink. 100 records worden daarom verzonden naar Dynamics het gelijktijdig.

Voor Dynamics 365 online, er geldt een limiet van [2 gelijktijdige batch-aanroepen per organisatie](https://msdn.microsoft.com/en-us/library/jj863631.aspx#Run-time%20limitations). Als deze limiet wordt overschreden, wordt een 'Server bezet'-fout is opgetreden voordat de eerste aanvraag ooit wordt uitgevoerd. Houden 'writeBatchSize' kleiner dan of gelijk aan 10 wilt voorkomen dat dergelijke beperking van gelijktijdige aanroepen.

De optimale combinatie van '**writeBatchSize**'en'**parallelCopies**"is afhankelijk van het schema van uw entiteit bijvoorbeeld het aantal kolommen, rijgrootte, aantal plugins/werkstromen/workflow-activiteiten die zijn gekoppeld deze aanroepen, enzovoort. De standaardinstelling van 10 writeBatchSize * 10 parallelCopies is de aanbeveling op basis van Dynamics-service, die zou moeten werken voor de meeste Dynamics-entiteiten echter mogelijk niet optimale prestaties. U kunt de prestaties afstemmen door aan te passen van de combinatie van argumenten in de instellingen voor de kopieeractiviteit.

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

## <a name="data-type-mapping-for-dynamics"></a>Toewijzing voor Dynamics-gegevenstype

Wanneer u gegevens van Dynamics kopieert, worden de volgende toewijzingen uit Dynamics-gegevenstypen gebruikt om Data Factory tussentijdse gegevenstypen. Zie voor meer informatie hoe de kopieerbewerking het schema en de gegevens van een brontype toegewezen aan de sink [Schema en gegevens typt toewijzingen](copy-activity-schema-and-type-mapping.md).

Configureer het bijbehorende gegevenstype voor de Data Factory in de gegevenssetstructuur van een op basis van uw gegevensbron Dynamics-gegevenstype met behulp van de volgende toewijzingstabel.

| Dynamics-gegevenstype | Data Factory tussentijdse gegevenstype | Ondersteund als bron | Ondersteund als sink |
|:--- |:--- |:--- |:--- |
| AttributeTypeCode.BigInt | Lang | ✓ | ✓ |
| AttributeTypeCode.Boolean | Booleaans | ✓ | ✓ |
| AttributeType.Customer | GUID | ✓ | | 
| AttributeType.DateTime | Datum en tijd | ✓ | ✓ |
| AttributeType.Decimal | Decimaal | ✓ | ✓ |
| AttributeType.Double | Double-waarde | ✓ | ✓ |
| AttributeType.EntityName | Reeks | ✓ | ✓ |
| AttributeType.Integer | Int32 | ✓ | ✓ |
| AttributeType.Lookup | GUID | ✓ | ✓ (met één type dat is gekoppeld) |
| AttributeType.ManagedProperty | Booleaans | ✓ | |
| AttributeType.Memo | Reeks | ✓ | ✓ |
| AttributeType.Money | Decimaal | ✓ | ✓ |
| AttributeType.Owner | GUID | ✓ | |
| AttributeType.Picklist | Int32 | ✓ | ✓ |
| AttributeType.Uniqueidentifier | GUID | ✓ | ✓ |
| AttributeType.String | Reeks | ✓ | ✓ |
| AttributeType.State | Int32 | ✓ | ✓ |
| AttributeType.Status | Int32 | ✓ | ✓ |


> [!NOTE]
> De gegevenstypen Dynamics AttributeType.CalendarRules en AttributeType.PartyList worden niet ondersteund.

## <a name="next-steps"></a>Volgende stappen
Zie voor een lijst met gegevensarchieven die worden ondersteund als bronnen en sinks door de kopieeractiviteit in Data Factory, [ondersteunde gegevensarchieven](copy-activity-overview.md#supported-data-stores-and-formats).
