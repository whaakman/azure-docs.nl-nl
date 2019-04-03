---
title: Logboeken met diagnostische gegevens weergeven voor Azure Data Lake Storage Gen1 | Microsoft Docs
description: 'Meer informatie over het instellen en toegang krijgen tot diagnostische logboeken voor Azure Data Lake Storage Gen1 '
services: data-lake-store
documentationcenter: ''
author: twooley
manager: mtillman
editor: cgronlun
ms.assetid: f6e75eb1-d0ae-47cf-bdb8-06684b7c0a94
ms.service: data-lake-store
ms.devlang: na
ms.topic: conceptual
ms.date: 03/26/2018
ms.author: twooley
ms.openlocfilehash: d200f72b3c0e5634c3dca8f60a4754a14351110a
ms.sourcegitcommit: a60a55278f645f5d6cda95bcf9895441ade04629
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/03/2019
ms.locfileid: "58877955"
---
# <a name="accessing-diagnostic-logs-for-azure-data-lake-storage-gen1"></a>Diagnostische logboeken openen voor Azure Data Lake Storage Gen1
Leer hoe u Diagnostische logboekregistratie voor uw Azure Data Lake Storage Gen1-account en het weergeven van de logboeken die worden verzameld voor uw account inschakelen.

Organisaties kunnen diagnostische logboekregistratie inschakelen voor hun Gen1 van Azure Data Lake Storage-account voor het verzamelen van audittrails voor gegevenstoegang die informatie zoals de lijst van gebruikers met toegang tot de gegevens bevat, hoe vaak de gegevens worden gebruikt, hoeveel gegevens worden opgeslagen in de account, enzovoort. Wanneer dit is ingeschakeld, worden de diagnostische gegevens en/of de aanvragen worden geregistreerd op basis van best-effort. Vermeldingen in zowel de aanvragen als de diagnostische gegevens worden alleen als er aanvragen voor het service-eindpunt gemaakt.

## <a name="prerequisites"></a>Vereisten
* **Een Azure-abonnement**. Zie [Gratis proefversie van Azure ophalen](https://azure.microsoft.com/pricing/free-trial/).
* **Azure Data Lake Storage Gen1 account**. Volg de instructies op [aan de slag met Azure Data Lake Storage Gen1 met behulp van de Azure-Portal](data-lake-store-get-started-portal.md).

## <a name="enable-diagnostic-logging-for-your-data-lake-storage-gen1-account"></a>Diagnostische logboekregistratie inschakelen voor uw Gen1 van Data Lake Storage-account
1. Meld u aan bij de nieuwe [Azure Portal](https://portal.azure.com).
2. Open uw Gen1 van Data Lake Storage-account en in de blade van het Data Lake Storage Gen1-account op **diagnostische instellingen**.
3. In de **diagnostische instellingen** blade, klikt u op **diagnostische gegevens inschakelen**.

    ![Diagnostische logboekregistratie inschakelen](./media/data-lake-store-diagnostic-logs/turn-on-diagnostics.png "logboeken met diagnostische gegevens inschakelen")

3. In de **diagnostische instellingen** blade maken de volgende wijzigingen aan het vastleggen van diagnostische gegevens configureren.
   
    ![Diagnostische logboekregistratie inschakelen](./media/data-lake-store-diagnostic-logs/enable-diagnostic-logs.png "logboeken met diagnostische gegevens inschakelen")
   
   * Voor **naam**, voer een waarde voor de configuratie van diagnostische logboeken.
   * U kunt opslaan en verwerken de gegevens op verschillende manieren.
     
        * Selecteer de optie voor **archiveren naar een opslagaccount** voor het opslaan van logboeken naar een Azure Storage-account. U gebruikt deze optie als u wilt archiveren van de gegevens die batch verwerkt op een later tijdstip. Als u deze optie selecteert, moet u een Azure Storage-account om op te slaan van de logboeken te opgeven.
        
        * Selecteer de optie voor **Stream naar een event hub** logboek om gegevens te streamen naar een Azure Event Hub. Waarschijnlijk gebruikt u deze optie hebt u een pijplijn downstream-verwerkingen binnenkomende Logboeken in realtime analyseren. Als u deze optie selecteert, moet u de details opgeven voor de Azure Event Hub die u wilt gebruiken.

        * Selecteer de optie voor **verzenden naar Log Analytics** de service Azure Monitor gebruiken voor het analyseren van de gegenereerde logboekgegevens. Als u deze optie selecteert, moet u de details opgeven voor de Log Analytics-werkruimte die u de logboekanalyse uitvoeren gebruikt is. Zie [weergeven of analyseren van gegevens die zijn verzameld met Azure Monitor logboeken zoeken](../azure-monitor/learn/tutorial-viewdata.md) logboeken voor meer informatie over het gebruik van Azure Monitor.
     
   * Geef op of u wilt ophalen van de logboeken voor controle of aanvraag zich aanmeldt of beide.
   * Geef het aantal dagen waarvoor gegevens moeten worden bewaard. Bewaarperiode is alleen van toepassing als u Azure storage-account gebruikt voor het archiveren van gegevens aan het logboek.
   * Klik op **Opslaan**.

Als u diagnostische instellingen hebt ingeschakeld, kunt u bekijken in de logboekbestanden in de **diagnostische logboeken** tabblad.

## <a name="view-diagnostic-logs-for-your-data-lake-storage-gen1-account"></a>Diagnostische logboeken voor uw Gen1 van Data Lake Storage-account weergeven
Er zijn twee manieren om de logboekgegevens voor uw Gen1 van Data Lake Storage-account weer te geven.

* De Data Lake Storage Gen1 bekijken Accountinstellingen
* Vanuit de Azure Storage-account waar de gegevens worden opgeslagen

### <a name="using-the-data-lake-storage-gen1-settings-view"></a>Met behulp van de Data Lake-Opslaginstellingen Gen1-weergave
1. Uit uw account voor Data Lake Storage Gen1 **instellingen** blade, klikt u op **diagnostische logboeken**.
   
    ![Diagnostische logboeken bekijken](./media/data-lake-store-diagnostic-logs/view-diagnostic-logs.png "diagnostische logboeken weergeven") 
2. In de **diagnoselogboeken** blade ziet u de logboeken door gecategoriseerd **auditlogboeken** en **aanvragen logboeken**.
   
   * Logboeken van de aanvraag vastleggen elke API-aanvraag die is gemaakt op de Gen1 van Data Lake Storage-account.
   * Auditlogboeken zijn vergelijkbaar met het aanvragen van Logboeken, maar bieden een meer gedetailleerd overzicht van de bewerkingen die worden uitgevoerd op de Gen1 van Data Lake Storage-account. Een API-aanroep voor één upload in Logboeken van de aanvraag kan bijvoorbeeld leiden tot meerdere "Toevoegen" bewerkingen in de auditlogboeken.
3. De om Logboeken te downloaden, klikt u op de **downloaden** link tegen elke logboekvermelding.

### <a name="from-the-azure-storage-account-that-contains-log-data"></a>Vanuit de Azure Storage-account bevat die logboekgegevens
1. Open de blade voor Azure Storage-account die is gekoppeld aan de Data Lake Storage Gen1 voor logboekregistratie, en klik vervolgens op Blobs. De **Blob-service** blade een lijst met twee containers.
   
    ![Registratie in diagnoselogboek weergave](./media/data-lake-store-diagnostic-logs/view-diagnostic-logs-storage-account.png "diagnostische logboeken weergeven")
   
   * De container **insights-logs-audit** bevat de auditlogboeken.
   * De container **insights-logs-aanvragen** bevat de logboeken van de aanvraag.
2. In deze containers worden de logboeken worden opgeslagen onder de volgende structuur.
   
    ![Registratie in diagnoselogboek weergave](./media/data-lake-store-diagnostic-logs/view-diagnostic-logs-storage-account-structure.png "diagnostische logboeken weergeven")
   
    Als u bijvoorbeeld kan het volledige pad naar een auditlogboek worden `https://adllogs.blob.core.windows.net/insights-logs-audit/resourceId=/SUBSCRIPTIONS/<sub-id>/RESOURCEGROUPS/myresourcegroup/PROVIDERS/MICROSOFT.DATALAKESTORE/ACCOUNTS/mydatalakestorage/y=2016/m=07/d=18/h=04/m=00/PT1H.json`
   
    Op dezelfde manier kan het volledige pad naar een logboek van de aanvraag worden `https://adllogs.blob.core.windows.net/insights-logs-requests/resourceId=/SUBSCRIPTIONS/<sub-id>/RESOURCEGROUPS/myresourcegroup/PROVIDERS/MICROSOFT.DATALAKESTORE/ACCOUNTS/mydatalakestorage/y=2016/m=07/d=18/h=14/m=00/PT1H.json`

## <a name="understand-the-structure-of-the-log-data"></a>De structuur van de logboekgegevens begrijpen
Logboeken van de audit en -aanvraag zijn in een JSON-indeling. In deze sectie we kijken naar de structuur van de JSON voor de aanvraag en auditlogboeken.

### <a name="request-logs"></a>Logboeken van de aanvraag
Hier volgt een voorbeeldvermelding voor het in het logboek van de aanvraag voor JSON-indeling. Elke blob heeft een basis-object met de naam **records** die een matrix met objecten logboek bevat.

    {
    "records": 
      [        
        . . . .
        ,
        {
             "time": "2016-07-07T21:02:53.456Z",
             "resourceId": "/SUBSCRIPTIONS/<subscription_id>/RESOURCEGROUPS/<resource_group_name>/PROVIDERS/MICROSOFT.DATALAKESTORE/ACCOUNTS/<data_lake_storage_gen1_account_name>",
             "category": "Requests",
             "operationName": "GETCustomerIngressEgress",
             "resultType": "200",
             "callerIpAddress": "::ffff:1.1.1.1",
             "correlationId": "4a11c709-05f5-417c-a98d-6e81b3e29c58",
             "identity": "1808bd5f-62af-45f4-89d8-03c5e81bac30",
             "properties": {"HttpMethod":"GET","Path":"/webhdfs/v1/Samples/Outputs/Drivers.csv","RequestContentLength":0,"ClientRequestId":"3b7adbd9-3519-4f28-a61c-bd89506163b8","StartTime":"2016-07-07T21:02:52.472Z","EndTime":"2016-07-07T21:02:53.456Z"}
        }
        ,
        . . . .
      ]
    }

#### <a name="request-log-schema"></a>Schema voor het logboek van aanvraag
| Name | Type | Description |
| --- | --- | --- |
| time |String |De tijdstempel (in UTC) van het logboek |
| resourceId |String |De ID van de resource die bewerking duurde plaats op |
| category |String |De logboekcategorie. Bijvoorbeeld, **aanvragen**. |
| operationName |String |Naam van de bewerking die wordt vastgelegd. Bijvoorbeeld: getfilestatus. |
| resultType |String |De status van de bewerking, bijvoorbeeld 200. |
| callerIpAddress |String |Het IP-adres van de client die de aanvraag |
| correlationId |String |De ID van het logboek die kan worden gebruikt voor het groeperen van een set van gerelateerde vermeldingen |
| identity |Object |De identiteit die het logboek is gegenereerd |
| properties |JSON |Zie hieronder voor meer informatie |

#### <a name="request-log-properties-schema"></a>Schema voor eigenschappen van aanvraag-logboek
| Name | Type | Beschrijving |
| --- | --- | --- |
| HttpMethod |String |De HTTP-methode gebruikt voor de bewerking. Bijvoorbeeld, krijgen. |
| Pad |String |Het pad naar de bewerking is uitgevoerd op |
| RequestContentLength |int |De lengte van de inhoud van de HTTP-aanvraag |
| ClientRequestId |String |De ID die is uniek voor deze aanvraag |
| StartTime |String |Het tijdstip waarop de server de aanvraag ontvangen |
| EndTime |String |Het tijdstip waarop de server een antwoord verzonden |

### <a name="audit-logs"></a>Controlelogboeken
Hier volgt een voorbeeldvermelding voor het in het auditlogboek voor JSON-indeling. Elke blob heeft een basis-object met de naam **records** waarin een matrix met objecten log

    {
    "records": 
      [        
        . . . .
        ,
        {
             "time": "2016-07-08T19:08:59.359Z",
             "resourceId": "/SUBSCRIPTIONS/<subscription_id>/RESOURCEGROUPS/<resource_group_name>/PROVIDERS/MICROSOFT.DATALAKESTORE/ACCOUNTS/<data_lake_storage_gen1_account_name>",
             "category": "Audit",
             "operationName": "SeOpenStream",
             "resultType": "0",
             "resultSignature": "0",
             "correlationId": "381110fc03534e1cb99ec52376ceebdf;Append_BrEKAmg;25.66.9.145",
             "identity": "A9DAFFAF-FFEE-4BB5-A4A0-1B6CBBF24355",
             "properties": {"StreamName":"adl://<data_lake_storage_gen1_account_name>.azuredatalakestore.net/logs.csv"}
        }
        ,
        . . . .
      ]
    }

#### <a name="audit-log-schema"></a>Schema van auditlogboek
| Name | Type | Beschrijving |
| --- | --- | --- |
| time |String |De tijdstempel (in UTC) van het logboek |
| resourceId |String |De ID van de resource die bewerking duurde plaats op |
| category |String |De logboekcategorie. Bijvoorbeeld, **Audit**. |
| operationName |String |Naam van de bewerking die wordt vastgelegd. Bijvoorbeeld: getfilestatus. |
| resultType |String |De status van de bewerking, bijvoorbeeld 200. |
| resultSignature |String |Meer informatie over het opnieuw. |
| correlationId |String |De ID van het logboek die kan worden gebruikt voor het groeperen van een set van gerelateerde vermeldingen |
| identity |Object |De identiteit die het logboek is gegenereerd |
| properties |JSON |Zie hieronder voor meer informatie |

#### <a name="audit-log-properties-schema"></a>Audit log eigenschappen schema
| Name | Type | Description |
| --- | --- | --- |
| StreamName |String |Het pad naar de bewerking is uitgevoerd op |

## <a name="samples-to-process-the-log-data"></a>Voorbeelden voor het verwerken van de logboekgegevens
Bij het verzenden van Logboeken van Azure Data Lake Storage Gen1 naar Azure Monitor-Logboeken (Zie [weergeven of analyseren van gegevens die zijn verzameld met Azure Monitor logboeken zoeken](../azure-monitor/learn/tutorial-viewdata.md) logboeken voor meer informatie over het gebruik van Azure Monitor), de volgende query retourneert een tabel met een lijst van de gebruiker weergeven namen van de tijd van de gebeurtenissen en het aantal gebeurtenissen voor de tijd van de gebeurtenis, samen met een visuele grafiek. Kan eenvoudig worden gewijzigd om weer te geven van gebruikers-GUID of andere kenmerken:

```
search *
| where ( Type == "AzureDiagnostics" )
| summarize count(TimeGenerated) by identity_s, TimeGenerated
```


Azure Data Lake Storage Gen1 bevat een voorbeeld over het verwerken en analyseren van de logboekgegevens. U vindt het voorbeeld in [ https://github.com/Azure/AzureDataLake/tree/master/Samples/AzureDiagnosticsSample ](https://github.com/Azure/AzureDataLake/tree/master/Samples/AzureDiagnosticsSample). 

## <a name="see-also"></a>Zie ook
* [Overzicht van Azure Data Lake Storage Gen1](data-lake-store-overview.md)
* [Bescherm uw gegevens in Data Lake Storage Gen1](data-lake-store-secure-data.md)

