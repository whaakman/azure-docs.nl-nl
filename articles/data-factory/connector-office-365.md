---
title: Gegevens kopiëren van Office 365 met Azure Data Factory (Preview) | Microsoft Docs
description: Leer hoe u gegevens van Office 365 kopiëren naar een ondersteunde sink-gegevensopslag met behulp van de kopieeractiviteit in een Azure Data Factory-pijplijn.
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
ms.date: 09/19/2018
ms.author: jingwang
ms.openlocfilehash: 3783bc6ccda7a559e749a84005f20d860ab56755
ms.sourcegitcommit: ce526d13cd826b6f3e2d80558ea2e289d034d48f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/19/2018
ms.locfileid: "46368704"
---
# <a name="copy-data-from-office-365-into-azure-using-azure-data-factory-preview"></a>Gegevens uit Office 365 kopiëren naar Azure met behulp van Azure Data Factory (Preview) 

Azure Data Factory kunt u de uitgebreide gegevens van de organisatie in uw Office 365-tenant in Azure op een schaalbare manier en bouwen van toepassingen voor gegevensanalyse en inzichten op basis van deze waardevolle activa. Integratie met Privileged Access Management biedt beveiligd toegangsbeheer voor de waardevolle gegevens in Office 365.  Raadpleeg voor meer informatie over beheerde toegang tot Microsoft Graph in Microsoft Azure Preview [deze koppeling](https://github.com/OfficeDev/ManagedAccessMSGraph/wiki).

In dit artikel bevat een overzicht over het gebruik van de Kopieeractiviteit in Azure Data Factory om gegevens te kopiëren van Office 365. Dit is gebaseerd op de [overzicht kopieeractiviteit](copy-activity-overview.md) artikel met daarin een algemeen overzicht van de kopieeractiviteit.

## <a name="supported-capabilities"></a>Ondersteunde mogelijkheden

Op dit moment binnen een enkel kopieeractiviteit kunt u alleen **gegevens kopiëren van Office 365 in [Azure Blob Storage](connector-azure-blob-storage.md), [Azure Data Lake Storage Gen1](connector-azure-data-lake-store.md), en [Azure Data Lake Storage Gen2 ( Preview-versie)](connector-azure-data-lake-storage.md) in JSON-indeling** (type setOfObjects). Als u laden van Office 365 in andere typen van gegevensarchieven of in andere indelingen wilt, u kunt koppelen aan de eerste kopieeractiviteit met een latere copy activity in meer gegevens laden in een van de [ondersteund ADF doelarchieven](copy-activity-overview.md#supported-data-stores-and-formats) (Zie" ondersteund als een sink"kolom in de tabel 'Ondersteunde gegevensarchieven en indelingen').

>[!IMPORTANT]
>- Het Azure-abonnement met de data factory en de sink-gegevensopslag moet zich in dezelfde Azure Active Directory (Azure AD)-tenant als Office 365-tenant.
>- Zorg ervoor dat de Integration Runtime van Azure-regio die wordt gebruikt voor de kopieeractiviteit, evenals de bestemming zich in dezelfde regio waar de Office 365-tenantgebruikers postvak dat zich bevindt. Raadpleeg [hier](concepts-integration-runtime.md#integration-runtime-location) om te begrijpen hoe de locatie van de Azure IR wordt bepaald. Raadpleeg [tabel hier](https://github.com/OfficeDev/ManagedAccessMSGraph/wiki/Capabilities#data-regions) voor een lijst van ondersteunde Office-regio's en bijbehorende Azure-regio's.
>-  Als u het laden van gegevens in Office 365 **Azure Blob Storage** als doel, zorg ervoor dat u gebruikmaakt van **[service-principal verificatie](connector-azure-blob-storage.md#service-principal-authentication)** bij het definiëren van de gekoppelde Naar Azure Blob Storage-service en niet met behulp van [accountsleutel](connector-azure-blob-storage.md#account-key-authentication), [handtekening voor gedeelde toegang](connector-azure-blob-storage.md#shared-access-signature-authentication) of [beheerde service-identiteit ](connector-azure-blob-storage.md#managed-service-identity-authentication) verificaties.
>-  Als u het laden van gegevens in Office 365 **Azure Data Lake Storage Gen1** als doel, zorg ervoor dat u gebruikmaakt van [ **service-principal verificatie** ](connector-azure-data-lake-store.md#using-service-principal-authentication) bij het definiëren van de Gekoppelde Service Azure Data Lake Storage Gen1 en niet met behulp van [managed service identity-verificatie](connector-azure-data-lake-store.md#using-managed-service-identity-authentication).

## <a name="prerequisites"></a>Vereisten

Als u wilt gegevens uit Office 365 kopiëren naar Azure, moet u de volgende vereiste stappen uit:

- Uw Office 365-tenantbeheerder bepalen acties moet uitvoeren, zoals wordt beschreven [hier](https://github.com/OfficeDev/ManagedAccessMSGraph/wiki/On-boarding).
- Maken en configureren van een Azure AD-webtoepassing in Azure Active Directory.  Zie voor instructies [maken van een Azure AD-toepassing](../azure-resource-manager/resource-group-create-service-principal-portal.md#create-an-azure-active-directory-application).
- Noteer de volgende waarden, die u gebruiken wilt voor het definiëren van de gekoppelde service voor Office 365:
    - Tenant-ID.  Zie voor instructies [tenant-ID ophalen](../azure-resource-manager/resource-group-create-service-principal-portal.md#get-tenant-id).
    - Toepassings-ID en de toepassing de sleutel.  Zie voor instructies [Get-ID en -verificatie Toepassingssleutel](../azure-resource-manager/resource-group-create-service-principal-portal.md#get-application-id-and-authentication-key).
- De identiteit van de gebruiker die de aanvraag voor de toegang tot gegevens als eigenaar van de Azure AD-webtoepassing brengen zullen toevoegen (van de Azure AD-webtoepassing > Instellingen > eigenaren > eigenaar toevoegen).
- _(Aanbevolen)_  [Voor Azure toewijzen](../azure-policy/assign-policy-definition.md) voor versleuteling van gegevens naar uw gegevensarchieven. Informatie over de naleving van beleid worden als onderdeel van de gegevensaanvraag naar de goedkeurders gegevens weergegeven. Nadat u de toewijzing van configuratiebeleid wordt gemaakt, voor elke activiteit kopiëren die wordt uitgevoerd, controleert ADF Zorg ervoor dat de beleidstoewijzing wordt afgedwongen. Raadpleeg [hier](https://github.com/OfficeDev/ManagedAccessMSGraph/wiki/Capabilities#policies) voor een volledige lijst van ondersteunde beleidsregels.

## <a name="approving-new-data-access-requests"></a>Toegang aanvragen goedkeuren van nieuwe gegevens

Als dit de eerste keer dat u gegevens voor deze context aanvraagt (een combinatie van welke gegevens worden tabel toegang, welke bestemming-account is de gegevens worden geladen in en welke gebruikers-id is, zodat de gegevens toegangsaanvraag), ziet u de copy-activiteit status als 'Wordt uitgevoerd' en alleen wanneer u klikt op in ['Details' koppeling onder acties](copy-activity-overview.md#monitoring) wordt de status weergegeven als 'RequestingConsent'.  Een lid van de groep data access goedkeurder nodig voor het goedkeuren van de aanvraag in het Privileged Access Management voordat de gegevensextractie verder kunt gaan.

Raadpleeg [hier](https://github.com/OfficeDev/ManagedAccessMSGraph/wiki/Approving-a-data-access-request) op hoe de fiatteur kan goedkeuren de gegevens toegang tot aanvraag en Raadpleeg [hier](https://github.com/OfficeDev/ManagedAccessMSGraph/wiki/On-boarding) voor een uitleg over de algehele integratie met Privileged Access Management, waaronder over het instellen van de gegevens groep voor toegang tot goedkeurder.

## <a name="getting-started"></a>Aan de slag

U kunt een pijplijn maken met copy activity in .NET SDK, Python-SDK, Azure PowerShell, REST-API of Azure Resource Manager-sjabloon. Zie [zelfstudie Kopieeractiviteit](quickstart-create-data-factory-dot-net.md) voor stapsgewijze instructies voor het maken van een pijplijn met een kopieeractiviteit.

De volgende secties bevatten meer informatie over eigenschappen die worden gebruikt voor het definiëren van Data Factory-entiteiten die specifiek voor Office 365-connector.

## <a name="linked-service-properties"></a>Eigenschappen van de gekoppelde service

De volgende eigenschappen worden ondersteund voor Office 365 gekoppelde service:

| Eigenschap | Beschrijving | Vereist |
|:--- |:--- |:--- |
| type | De eigenschap type moet worden ingesteld op: **Office 365** | Ja |
| office365TenantId | Azure-tenant-ID waartoe de Office 365-account behoort. | Ja |
| servicePrincipalTenantId | Geef informatie over de tenant die uw Azure AD-webtoepassing zich bevindt. | Ja |
| servicePrincipalId | Opgeven van de toepassing client-ID. | Ja |
| servicePrincipalKey | Geef de sleutel van de toepassing. Dit veld markeert als een SecureString Bewaar deze zorgvuldig in Data Factory. | Ja |
| connectVia | De Integratieruntime moet worden gebruikt verbinding maken met het gegevensarchief.  Als niet is opgegeven, wordt de standaard Azure Integration Runtime. | Nee |

>[!NOTE]
> Het verschil tussen **office365TenantId** en **servicePrincipalTenantId** en de bijbehorende waarde om te bieden:
>- Als u een enterprise-ontwikkelaar bent ontwikkelen van een toepassing op basis van Office 365-gegevens voor eigen gebruik van uw organisatie, en vervolgens u dezelfde tenant moet opgeven-ID voor beide eigenschappen die van uw organisatie AAD-tenant-id.
>- Als u een ISV-ontwikkelaar ontwikkelen van een toepassing voor uw klanten wordt office365TenantId van uw klant (het installatieprogramma van toepassing) AAD-tenant-ID en servicePrincipalTenantId worden van uw bedrijf AAD-tenant-ID.

**Voorbeeld:**

```json
{
    "name": "Office365LinkedService",
    "properties": {
        "type": "Office365",
        "typeProperties": {
            "office365TenantId": "<Office 365 tenant id>",
            "servicePrincipalTenantId": "<AAD app service principal tenant id>",
            "servicePrincipalId": "<AAD app service principal id>",
            "servicePrincipalKey": {
                "type": "SecureString",
                "value": "<AAD app service principal key>"
            }
        }
    }
}
```

## <a name="dataset-properties"></a>Eigenschappen van gegevensset

Zie voor een volledige lijst van de secties en eigenschappen die beschikbaar zijn voor het definiëren van gegevenssets, de [gegevenssets](concepts-datasets-linked-services.md) artikel. Deze sectie bevat een lijst met eigenschappen die worden ondersteund door Office 365-gegevensset.

Als u wilt kopiëren van gegevens van Office 365, zijn de volgende eigenschappen worden ondersteund:

| Eigenschap | Beschrijving | Vereist |
|:--- |:--- |:--- |
| type | De eigenschap type van de gegevensset moet worden ingesteld op: **Office365Table** | Ja |
| tableName | Naam van de gegevensset om op te halen van Office 365. Raadpleeg hier voor een lijst van beschikbare voor het ophalen van Office 365-gegevenssets. | Ja |
| Predikaat | Een predicaatexpressie die kan worden gebruikt voor het filteren van de specifieke rijen om uit te pakken uit Office 365.  Raadpleeg hier als u wilt weten welke kolommen kunnen worden gebruikt voor predicaatfiltrering voor elke tabel en de indeling van de expressie filter. | Nee<br>(Als er geen predicaat wordt opgegeven, de standaardwaarde is om gegevens te extraheren voor de afgelopen 30 dagen) |

**Voorbeeld**

```json
{
    "name": "Office365Table1",
    "properties": {
        "type": "Office365Table",
        "linkedServiceName": {
            "referenceName": "<Office 365 linked service name>",
            "type": "LinkedServiceReference"
        },
        "structure": [
            {
                "name": "ReceivedDateTime",
                "type": "datetime"
            },
            {
                "name": "SentDateTime",
                "type": "datetime"
            },
            {
                "name": "HasAttachments",
                "type": "boolean"
            },
            {
                "name": "InternetMessageId",
                "type": "string"
            },
            {
                "name": "Subject",
                "type": "string"
            },
            {
                "name": "Importance",
                "type": "string"
            },
            {
                "name": "ParentFolderId",
                "type": "string"
            },
            {
                "name": "Sender",
                "type": "string"
            },
            {
                "name": "From",
                "type": "string"
            },
            {
                "name": "ToRecipients",
                "type": "string"
            },
            {
                "name": "CcRecipients",
                "type": "string"
            },
            {
                "name": "BccRecipients",
                "type": "string"
            },
            {
                "name": "ReplyTo",
                "type": "string"
            },
            {
                "name": "ConversationId",
                "type": "string"
            },
            {
                "name": "UniqueBody",
                "type": "string"
            },
            {
                "name": "IsDeliveryReceiptRequested",
                "type": "boolean"
            },
            {
                "name": "IsReadReceiptRequested",
                "type": "boolean"
            },
            {
                "name": "IsRead",
                "type": "boolean"
            },
            {
                "name": "IsDraft",
                "type": "boolean"
            },
            {
                "name": "WebLink",
                "type": "string"
            },
            {
                "name": "CreatedDateTime",
                "type": "datetime"
            },
            {
                "name": "LastModifiedDateTime",
                "type": "datetime"
            },
            {
                "name": "ChangeKey",
                "type": "string"
            },
            {
                "name": "Categories",
                "type": "string"
            },
            {
                "name": "Id",
                "type": "string"
            },
            {
                "name": "Attachments",
                "type": "string"
            }
        ],
        "typeProperties": {
            "tableName": "BasicDataSet_v0.Contact_v0",
            "predicate": "CreatedDateTime >= 2018-07-11T16:00:00.000Z AND CreatedDateTime <= 2018-07-18T16:00:00.000Z"
        }
    }
}
```

## <a name="copy-activity-properties"></a>Eigenschappen van de kopieeractiviteit

Zie voor een volledige lijst van de secties en eigenschappen die beschikbaar zijn voor het definiëren van activiteiten, de [pijplijnen](concepts-pipelines-activities.md) artikel. Deze sectie bevat een lijst met eigenschappen die worden ondersteund door Office 365-bron.

### <a name="office-365-as-source"></a>Office 365 als bron

Om gegevens te kopiëren van Office 365, stelt u het brontype in de kopieeractiviteit naar **Office365Source**. Er zijn geen extra eigenschappen worden ondersteund in de kopieeractiviteit **bron** sectie.

**Voorbeeld:**

```json
"activities": [
    {
        "name": "CopyFromO365ToBlob",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<Office 365 input dataset name>",
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
                "type": "Office365Source"
            },
            "sink": {
                "type": "BlobSink"
            }
        }
    }
]
```

## <a name="next-steps"></a>Volgende stappen
Zie voor een lijst met gegevensarchieven die worden ondersteund als bronnen en sinks door de kopieeractiviteit in Azure Data Factory, [ondersteunde gegevensarchieven](copy-activity-overview.md#supported-data-stores-and-formats).
