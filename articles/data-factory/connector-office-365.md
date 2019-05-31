---
title: Gegevens kopiëren van Office 365 met Azure Data Factory | Microsoft Docs
description: Leer hoe u gegevens van Office 365 kopiëren naar een ondersteunde sink-gegevensopslag met behulp van de kopieeractiviteit in een Azure Data Factory-pijplijn.
services: data-factory
documentationcenter: ''
author: linda33wj
manager: craigg
ms.reviewer: douglasl
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 05/07/2019
ms.author: jingwang
ms.openlocfilehash: 80ef8870bafa00f3debda99db299018a39d42a82
ms.sourcegitcommit: 509e1583c3a3dde34c8090d2149d255cb92fe991
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/27/2019
ms.locfileid: "66245040"
---
# <a name="copy-data-from-office-365-into-azure-using-azure-data-factory"></a>Gegevens uit Office 365 kopiëren naar Azure met behulp van Azure Data Factory

Azure Data Factory kunt u de uitgebreide gegevens van de organisatie in uw Office 365-tenant in Azure op een schaalbare manier en bouwen van toepassingen voor gegevensanalyse en inzichten op basis van deze waardevolle activa. Integratie met Privileged Access Management biedt beveiligd toegangsbeheer voor de waardevolle gegevens in Office 365.  Voor meer informatie over Microsoft Graph-gegevens verbinden, Raadpleeg [deze koppeling](https://docs.microsoft.com/graph/data-connect-concept-overview).

In dit artikel bevat een overzicht over het gebruik van de Kopieeractiviteit in Azure Data Factory om gegevens te kopiëren van Office 365. Dit is gebaseerd op de [overzicht kopieeractiviteit](copy-activity-overview.md) artikel met daarin een algemeen overzicht van de kopieeractiviteit.

## <a name="supported-capabilities"></a>Ondersteunde mogelijkheden

Op dit moment binnen een enkel kopieeractiviteit kunt u alleen **gegevens kopiëren van Office 365 in [Azure Blob Storage](connector-azure-blob-storage.md), [Azure Data Lake Storage Gen1](connector-azure-data-lake-store.md), en [Azure Data Lake Storage Gen2 ](connector-azure-data-lake-storage.md) in JSON-indeling** (type setOfObjects). Als u laden van Office 365 in andere typen van gegevensarchieven of in andere indelingen wilt, u kunt koppelen aan de eerste kopieeractiviteit met een latere copy activity in meer gegevens laden in een van de [ondersteund ADF doelarchieven](copy-activity-overview.md#supported-data-stores-and-formats) (Zie" ondersteund als een sink"kolom in de tabel 'Ondersteunde gegevensarchieven en indelingen').

>[!IMPORTANT]
>- Het Azure-abonnement met de data factory en de sink-gegevensopslag moet zich in dezelfde Azure Active Directory (Azure AD)-tenant als Office 365-tenant.
>- Zorg ervoor dat de Integration Runtime van Azure-regio die wordt gebruikt voor de kopieeractiviteit, evenals de bestemming zich in dezelfde regio waar de Office 365-tenantgebruikers postvak dat zich bevindt. Raadpleeg [hier](concepts-integration-runtime.md#integration-runtime-location) om te begrijpen hoe de locatie van de Azure IR wordt bepaald. Raadpleeg [tabel hier](https://docs.microsoft.com/graph/data-connect-datasets#regions) voor een lijst van ondersteunde Office-regio's en bijbehorende Azure-regio's.
>- Service-Principal-verificatie is het enige verificatiemechanisme ondersteund voor Azure Blob Storage, Azure Data Lake Storage Gen1 en Gen2 Azure Data Lake-opslag als doelarchieven.

## <a name="prerequisites"></a>Vereisten

Als u wilt gegevens uit Office 365 kopiëren naar Azure, moet u de volgende vereiste stappen uit:

- Uw Office 365-tenantbeheerder bepalen acties moet uitvoeren, zoals wordt beschreven [hier](https://docs.microsoft.com/graph/data-connect-get-started).
- Maken en configureren van een Azure AD-webtoepassing in Azure Active Directory.  Zie voor instructies [maken van een Azure AD-toepassing](../active-directory/develop/howto-create-service-principal-portal.md#create-an-azure-active-directory-application).
- Noteer de volgende waarden, die u gebruiken wilt voor het definiëren van de gekoppelde service voor Office 365:
    - Tenant-ID. Zie voor instructies [tenant-ID ophalen](../active-directory/develop/howto-create-service-principal-portal.md#get-values-for-signing-in).
    - Toepassings-ID en de toepassing de sleutel.  Zie voor instructies [Get-ID en -verificatie Toepassingssleutel](../active-directory/develop/howto-create-service-principal-portal.md#get-values-for-signing-in).
- De identiteit van de gebruiker die de aanvraag voor de toegang tot gegevens als eigenaar van de Azure AD-webtoepassing brengen zullen toevoegen (van de Azure AD-webtoepassing > Instellingen > eigenaren > eigenaar toevoegen). 
    - De identiteit van de gebruiker moet zich in de Office 365-organisatie u ophalen van gegevens uit zijn en mag geen een gastgebruiker.

## <a name="approving-new-data-access-requests"></a>Toegang aanvragen goedkeuren van nieuwe gegevens

Als dit de eerste keer dat u gegevens voor deze context aanvraagt (een combinatie van welke gegevens worden tabel toegang, welke bestemming-account is de gegevens worden geladen in en welke gebruikers-id is, zodat de gegevens toegangsaanvraag), ziet u de copy-activiteit status als 'Wordt uitgevoerd' en alleen wanneer u klikt op in ['Details' koppeling onder acties](copy-activity-overview.md#monitoring) wordt de status weergegeven als 'RequestingConsent'.  Een lid van de groep data access goedkeurder nodig voor het goedkeuren van de aanvraag in het Privileged Access Management voordat de gegevensextractie verder kunt gaan.

Raadpleeg [hier](https://docs.microsoft.com/graph/data-connect-tips#approve-pam-requests-via-office-365-admin-portal) op hoe de fiatteur kan goedkeuren de gegevens toegang tot aanvraag en Raadpleeg [hier](https://docs.microsoft.com/graph/data-connect-pam) voor een uitleg over de algehele integratie met Privileged Access Management, waaronder over het instellen van de gegevens groep voor toegang tot goedkeurder.

## <a name="policy-validation"></a>Validatie van het gebruikersbeleid

Als ADF is gemaakt als onderdeel van een beheerde app en Azure-beleid-toewijzingen worden aangebracht in resources binnen de resourcegroep voor beheer, wordt klikt u vervolgens voor elke activiteit kopiëren die wordt uitgevoerd, ADF gecontroleerd om ervoor te zorgen dat de toewijzingen van beleid worden afgedwongen. Raadpleeg [hier](https://docs.microsoft.com/graph/data-connect-policies#policies) voor een lijst van ondersteunde beleidsregels.

## <a name="getting-started"></a>Aan de slag

>[!TIP]
>Zie voor een overzicht van het gebruik van Office 365-connector, [laden van gegevens uit Office 365](load-office-365-data.md) artikel.

U kunt een pijplijn maken met de kopieeractiviteit met behulp van een van de volgende hulpprogramma's of de SDK's. Selecteer een koppeling naar een zelfstudie met stapsgewijze instructies voor het maken van een pijplijn met een kopieeractiviteit. 

- [Azure Portal](quickstart-create-data-factory-portal.md)
- [.NET SDK](quickstart-create-data-factory-dot-net.md)
- [Python SDK](quickstart-create-data-factory-python.md)
- [Azure PowerShell](quickstart-create-data-factory-powershell.md)
- [REST API](quickstart-create-data-factory-rest-api.md)
- [Azure Resource Manager-sjabloon](quickstart-create-data-factory-resource-manager-template.md). 

De volgende secties bevatten meer informatie over eigenschappen die worden gebruikt voor het definiëren van Data Factory-entiteiten die specifiek voor Office 365-connector.

## <a name="linked-service-properties"></a>Eigenschappen van de gekoppelde service

De volgende eigenschappen worden ondersteund voor Office 365 gekoppelde service:

| Eigenschap | Description | Vereist |
|:--- |:--- |:--- |
| type | De eigenschap type moet worden ingesteld op: **Office365** | Ja |
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

| Eigenschap | Description | Vereist |
|:--- |:--- |:--- |
| type | De eigenschap type van de gegevensset moet worden ingesteld op: **Office365Table** | Ja |
| tableName | Naam van de gegevensset om op te halen van Office 365. Raadpleeg [hier](https://docs.microsoft.com/graph/data-connect-datasets#datasets) voor een lijst van beschikbare voor het ophalen van Office 365-gegevenssets. | Ja |
| allowedGroups | Groep selectie predikaat.  Gebruik deze eigenschap te selecteren van groepen van maximaal 10 gebruikers voor wie de gegevens worden opgehaald.  Er zijn geen groepen zijn opgegeven, worden gegevens worden geretourneerd voor de hele organisatie. | Nee |
| userScopeFilterUri | Wanneer `allowedGroups` eigenschap niet is opgegeven, kunt u een predicaat-expressie die wordt toegepast op de gehele tenant voor het filteren van de specifieke rijen om uit te pakken uit Office 365. Het predicaat indeling moet de queryindeling van Microsoft Graph-API's, bijvoorbeeld overeen `https://graph.microsoft.com/v1.0/users?$filter=Department eq 'Finance'`. | Nee |
| dateFilterColumn | De naam van de datum/tijd-filterkolom. Deze eigenschap gebruiken om te beperken van het tijdsbereik voor welke Office 365-gegevens worden geëxtraheerd. | Ja als gegevensset een of meer datum/tijd-kolommen heeft. Raadpleeg [hier](https://docs.microsoft.com/graph/data-connect-filtering#filtering) voor een lijst met gegevenssets waarvoor deze datum/tijd-filter. |
| startTime | Datum/tijd-waarde om te filteren op starten. | Ja als `dateFilterColumn` is opgegeven |
| endTime | Datum/tijd-waarde om te filteren op beëindigen. | Ja als `dateFilterColumn` is opgegeven |

**Voorbeeld**

```json
{
    "name": "DS_May2019_O365_Message",
    "properties": {
        "type": "Office365Table",
        "linkedServiceName": {
            "referenceName": "<Office 365 linked service name>",
            "type": "LinkedServiceReference"
        },
        "structure": [
            {
                "name": "Id",
                "type": "String",
                "description": "The unique identifier of the event."
            },
            {
                "name": "CreatedDateTime",
                "type": "DateTime",
                "description": "The date and time that the event was created."
            },
            {
                "name": "LastModifiedDateTime",
                "type": "DateTime",
                "description": "The date and time that the event was last modified."
            },
            {
                "name": "ChangeKey",
                "type": "String",
                "description": "Identifies the version of the event object. Every time the event is changed, ChangeKey changes as well. This allows Exchange to apply changes to the correct version of the object."
            },
            {
                "name": "Categories",
                "type": "String",
                "description": "The categories associated with the event. Format: ARRAY<STRING>"
            },
            {
                "name": "OriginalStartTimeZone",
                "type": "String",
                "description": "The start time zone that was set when the event was created. See DateTimeTimeZone for a list of valid time zones."
            },
            {
                "name": "OriginalEndTimeZone",
                "type": "String",
                "description": "The end time zone that was set when the event was created. See DateTimeTimeZone for a list of valid time zones."
            },
            {
                "name": "ResponseStatus",
                "type": "String",
                "description": "Indicates the type of response sent in response to an event message. Format: STRUCT<Response: STRING, Time: STRING>"
            },
            {
                "name": "iCalUId",
                "type": "String",
                "description": "A unique identifier that is shared by all instances of an event across different calendars."
            },
            {
                "name": "ReminderMinutesBeforeStart",
                "type": "Int32",
                "description": "The number of minutes before the event start time that the reminder alert occurs."
            },
            {
                "name": "IsReminderOn",
                "type": "Boolean",
                "description": "Set to true if an alert is set to remind the user of the event."
            },
            {
                "name": "HasAttachments",
                "type": "Boolean",
                "description": "Set to true if the event has attachments."
            },
            {
                "name": "Subject",
                "type": "String",
                "description": "The text of the event's subject line."
            },
            {
                "name": "Body",
                "type": "String",
                "description": "The body of the message associated with the event.Format: STRUCT<ContentType: STRING, Content: STRING>"
            },
            {
                "name": "Importance",
                "type": "String",
                "description": "The importance of the event: Low, Normal, High."
            },
            {
                "name": "Sensitivity",
                "type": "String",
                "description": "Indicates the level of privacy for the event: Normal, Personal, Private, Confidential."
            },
            {
                "name": "Start",
                "type": "String",
                "description": "The start time of the event. Format: STRUCT<DateTime: STRING, TimeZone: STRING>"
            },
            {
                "name": "End",
                "type": "String",
                "description": "The date and time that the event ends. Format: STRUCT<DateTime: STRING, TimeZone: STRING>"
            },
            {
                "name": "Location",
                "type": "String",
                "description": "Location information of the event. Format: STRUCT<DisplayName: STRING, Address: STRUCT<Street: STRING, City: STRING, State: STRING, CountryOrRegion: STRING, PostalCode: STRING>, Coordinates: STRUCT<Altitude: DOUBLE, Latitude: DOUBLE, Longitude: DOUBLE, Accuracy: DOUBLE, AltitudeAccuracy: DOUBLE>>"
            },
            {
                "name": "IsAllDay",
                "type": "Boolean",
                "description": "Set to true if the event lasts all day. Adjusting this property requires adjusting the Start and End properties of the event as well."
            },
            {
                "name": "IsCancelled",
                "type": "Boolean",
                "description": "Set to true if the event has been canceled."
            },
            {
                "name": "IsOrganizer",
                "type": "Boolean",
                "description": "Set to true if the message sender is also the organizer."
            },
            {
                "name": "Recurrence",
                "type": "String",
                "description": "The recurrence pattern for the event. Format: STRUCT<Pattern: STRUCT<Type: STRING, `Interval`: INT, Month: INT, DayOfMonth: INT, DaysOfWeek: ARRAY<STRING>, FirstDayOfWeek: STRING, Index: STRING>, `Range`: STRUCT<Type: STRING, StartDate: STRING, EndDate: STRING, RecurrenceTimeZone: STRING, NumberOfOccurrences: INT>>"
            },
            {
                "name": "ResponseRequested",
                "type": "Boolean",
                "description": "Set to true if the sender would like a response when the event is accepted or declined."
            },
            {
                "name": "ShowAs",
                "type": "String",
                "description": "The status to show: Free, Tentative, Busy, Oof, WorkingElsewhere, Unknown."
            },
            {
                "name": "Type",
                "type": "String",
                "description": "The event type: SingleInstance, Occurrence, Exception, SeriesMaster."
            },
            {
                "name": "Attendees",
                "type": "String",
                "description": "The collection of attendees for the event. Format: ARRAY<STRUCT<EmailAddress: STRUCT<Name: STRING, Address: STRING>, Status: STRUCT<Response: STRING, Time: STRING>, Type: STRING>>"
            },
            {
                "name": "Organizer",
                "type": "String",
                "description": "The organizer of the event. Format: STRUCT<EmailAddress: STRUCT<Name: STRING, Address: STRING>>"
            },
            {
                "name": "WebLink",
                "type": "String",
                "description": "The URL to open the event in Outlook Web App."
            },
            {
                "name": "Attachments",
                "type": "String",
                "description": "The FileAttachment and ItemAttachment attachments for the message. Navigation property. Format: ARRAY<STRUCT<LastModifiedDateTime: STRING, Name: STRING, ContentType: STRING, Size: INT, IsInline: BOOLEAN, Id: STRING>>"
            },
            {
                "name": "BodyPreview",
                "type": "String",
                "description": "The preview of the message associated with the event. It is in text format."
            },
            {
                "name": "Locations",
                "type": "String",
                "description": "The locations where the event is held or attended from. The location and locations properties always correspond with each other. Format:  ARRAY<STRUCT<DisplayName: STRING, Address: STRUCT<Street: STRING, City: STRING, State: STRING, CountryOrRegion: STRING, PostalCode: STRING>, Coordinates: STRUCT<Altitude: DOUBLE, Latitude: DOUBLE, Longitude: DOUBLE, Accuracy: DOUBLE, AltitudeAccuracy: DOUBLE>, LocationEmailAddress: STRING, LocationUri: STRING, LocationType: STRING, UniqueId: STRING, UniqueIdType: STRING>>"
            },
            {
                "name": "OnlineMeetingUrl",
                "type": "String",
                "description": "A URL for an online meeting. The property is set only when an organizer specifies an event as an online meeting such as a Skype meeting"
            },
            {
                "name": "OriginalStart",
                "type": "DateTime",
                "description": "The start time that was set when the event was created in UTC time."
            },
            {
                "name": "SeriesMasterId",
                "type": "String",
                "description": "The ID for the recurring series master item, if this event is part of a recurring series."
            }
        ],
        "typeProperties": {
            "tableName": "BasicDataSet_v0.Event_v1",
            "dateFilterColumn": "CreatedDateTime",
            "startTime": "2019-04-28T16:00:00.000Z",
            "endTime": "2019-05-05T16:00:00.000Z",
            "userScopeFilterUri": "https://graph.microsoft.com/v1.0/users?$filter=Department eq 'Finance'"
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
