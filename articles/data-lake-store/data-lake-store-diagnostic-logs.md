---
title: Logboeken met diagnostische gegevens voor Azure Data Lake Store weergeven | Microsoft Docs
description: 'Begrijpen hoe instellen en toegang tot diagnoselogboeken voor Azure Data Lake Store '
services: data-lake-store
documentationcenter: 
author: nitinme
manager: jhubbard
editor: cgronlun
ms.assetid: f6e75eb1-d0ae-47cf-bdb8-06684b7c0a94
ms.service: data-lake-store
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 11/28/2017
ms.author: nitinme
ms.openlocfilehash: 5e1c3df24b0fc3e733981ab3f8814a9e6641f5f1
ms.sourcegitcommit: 9a8b9a24d67ba7b779fa34e67d7f2b45c941785e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/08/2018
---
# <a name="accessing-diagnostic-logs-for-azure-data-lake-store"></a>Toegang tot diagnoselogboeken voor Azure Data Lake Store
Informatie over het inschakelen van diagnostische logboekregistratie voor uw Data Lake Store-account en het weergeven van de logboeken die worden verzameld voor uw account.

Organisaties kunnen diagnostische logboekregistratie inschakelen voor hun Azure Data Lake Store-account voor het verzamelen van gegevens audittrails voor bestandstoegang, waarmee informatie, zoals de lijst van gebruikers die toegang tot de gegevens, hoe vaak de gegevens worden gebruikt, hoeveel gegevens worden opgeslagen in het account enz. Wanneer dit is ingeschakeld, worden de diagnostische gegevens en/of de aanvragen op basis van best-effort worden geregistreerd. Logboekvermeldingen zowel aanvragen en diagnostische gegevens worden alleen gemaakt als er aanvragen voor het service-eindpunt zijn.

## <a name="prerequisites"></a>Vereisten
* **Een Azure-abonnement**. Zie [Gratis proefversie van Azure ophalen](https://azure.microsoft.com/pricing/free-trial/).
* **Azure Data Lake Store-account**. Volg de instructies in [Aan de slag met Azure Data Lake Store met Azure Portal](data-lake-store-get-started-portal.md).

## <a name="enable-diagnostic-logging-for-your-data-lake-store-account"></a>Diagnostische logboekregistratie inschakelen voor uw Data Lake Store-account
1. Meld u aan bij de nieuwe [Azure Portal](https://portal.azure.com).
2. Open uw Data Lake Store-account en klik op de blade van het Data Lake Store-account **instellingen**, en klik vervolgens op **diagnostische logboeken**.
3. In de **diagnostische logboeken** blade, klikt u op **diagnostische gegevens inschakelen**.

    ![Diagnostische logboekregistratie inschakelen](./media/data-lake-store-diagnostic-logs/turn-on-diagnostics.png "logboeken met diagnostische gegevens inschakelen")

3. In de **diagnostische** blade Breng de volgende wijzigingen Diagnostische logboekregistratie configureren.
   
    ![Diagnostische logboekregistratie inschakelen](./media/data-lake-store-diagnostic-logs/enable-diagnostic-logs.png "logboeken met diagnostische gegevens inschakelen")
   
   * Voor **naam**, voer een waarde voor de configuratie van diagnostische logboeken.
   * U kunt kiezen om te verwerken/store de gegevens op verschillende manieren.
     
        * Selecteer de optie voor **archiveren naar een opslagaccount** voor het opslaan van Logboeken voor een Azure Storage-account. U gebruikt deze optie als u wilt archiveren van de gegevens die batch verwerkt op een later tijdstip worden. Als u deze optie selecteert, moet u een Azure Storage-account voor het opslaan van de logboeken naar opgeven.
        
        * Selecteer de optie voor **Stream naar een event hub** stroom logboek gegevens naar een Azure Event Hub. Zeer waarschijnlijk zal gebruik deze optie als u een pijplijn downstreamverwerking binnenkomende Logboeken in realtime analyseren. Als u deze optie selecteert, moet u de details opgeven voor de Azure Event Hub die u wilt gebruiken.

        * Selecteer de optie voor **verzenden met logboekanalyse** om de Azure Log Analytics-service gebruiken voor het analyseren van de gegenereerde logboekgegevens. Als u deze optie selecteert, moet u de details opgeven voor de Operations Management Suite-werkruimte dat zou u de logboekanalyse uitvoeren is. Zie [weergeven of gegevens die worden verzameld met logboekanalyse logboek search analyseren](../log-analytics/log-analytics-tutorial-viewdata.md) voor meer informatie over het gebruik van logboekanalyse.
     
   * Geef op of u wilt ophalen controlelogboeken of Logboeken aanvragen of beide.
   * Geef het aantal dagen waarvoor de gegevens moeten worden bewaard. Bewaartermijn is alleen van toepassing als u Azure storage-account gebruikt voor het archiveren van gegevens aan het logboek.
   * Klik op **Opslaan**.

Wanneer u de diagnostische instellingen hebt ingeschakeld, kunt u bekijken in de logboekbestanden in de **diagnostische logboeken** tabblad.

## <a name="view-diagnostic-logs-for-your-data-lake-store-account"></a>Diagnostische logboeken bekijken voor uw Data Lake Store-account
Er zijn twee manieren om de logboekgegevens voor uw Data Lake Store-account weer te geven.

* Vanuit de weergave-instellingen van Data Lake Store-account
* Vanuit het Azure-opslagaccount waar de gegevens worden opgeslagen

### <a name="using-the-data-lake-store-settings-view"></a>De weergave van Data Lake Store-instellingen
1. Van uw Data Lake Store-account **instellingen** blade, klikt u op **diagnostische logboeken**.
   
    ![Diagnostische logboekregistratie weergave](./media/data-lake-store-diagnostic-logs/view-diagnostic-logs.png "diagnostische logboeken bekijken") 
2. In de **diagnostische logboeken** blade ziet u de logboeken door gecategoriseerd **controlelogboeken** en **aanvragen logboeken**.
   
   * Logboeken aanvragen vastleggen elk verzoek API op het Data Lake Store-account.
   * Controlelogboeken zijn vergelijkbaar met Logboeken aanvragen, maar bieden een veel gedetailleerder verdeling van de bewerkingen die worden uitgevoerd op het Data Lake Store-account. Bijvoorbeeld, kan een API-aanroep voor één upload in Logboeken van de aanvraag leiden tot meerdere "Toevoegen" bewerkingen in de controlelogboeken.
3. De logboeken downloaden, klikt u op de **downloaden** link tegen elke logboekvermelding.

### <a name="from-the-azure-storage-account-that-contains-log-data"></a>Vanuit het Azure-opslagaccount waarin de logboekgegevens
1. Open de blade van Azure Storage-account die zijn gekoppeld met Data Lake Store voor logboekregistratie en klik op Blobs. De **Blob-service** blade bevat twee containers.
   
    ![Diagnostische logboekregistratie weergave](./media/data-lake-store-diagnostic-logs/view-diagnostic-logs-storage-account.png "diagnostische logboeken bekijken")
   
   * De container **insights-logboeken-controlegebeurtenissen** bevat de controlelogboeken.
   * De container **insights-logboeken-aanvragen** bevat de logboeken van de aanvraag.
2. De logboeken worden binnen deze containers in de volgende structuur opgeslagen.
   
    ![Diagnostische logboekregistratie weergave](./media/data-lake-store-diagnostic-logs/view-diagnostic-logs-storage-account-structure.png "diagnostische logboeken bekijken")
   
    Als u bijvoorbeeld kan het volledige pad naar een controlelogboek worden`https://adllogs.blob.core.windows.net/insights-logs-audit/resourceId=/SUBSCRIPTIONS/<sub-id>/RESOURCEGROUPS/myresourcegroup/PROVIDERS/MICROSOFT.DATALAKESTORE/ACCOUNTS/mydatalakestore/y=2016/m=07/d=18/h=04/m=00/PT1H.json`
   
    Daarnaast kan het volledige pad naar een logboek met aanvragen worden`https://adllogs.blob.core.windows.net/insights-logs-requests/resourceId=/SUBSCRIPTIONS/<sub-id>/RESOURCEGROUPS/myresourcegroup/PROVIDERS/MICROSOFT.DATALAKESTORE/ACCOUNTS/mydatalakestore/y=2016/m=07/d=18/h=14/m=00/PT1H.json`

## <a name="understand-the-structure-of-the-log-data"></a>De structuur van de logboekgegevens begrijpen
Logboeken van de audit en aanvraag zijn in een JSON-indeling. In deze sectie we kijken naar de JSON-structuur voor aanvraag en controlelogboeken.

### <a name="request-logs"></a>Logboeken aanvragen
Hier volgt een voorbeeldvermelding voor het in het logboek voor aanvraag JSON-indeling. Elke blob heeft een basis-object aangeroepen **records** die een matrix van logboek-objecten bevat.

    {
    "records": 
      [        
        . . . .
        ,
        {
             "time": "2016-07-07T21:02:53.456Z",
             "resourceId": "/SUBSCRIPTIONS/<subscription_id>/RESOURCEGROUPS/<resource_group_name>/PROVIDERS/MICROSOFT.DATALAKESTORE/ACCOUNTS/<data_lake_store_account_name>",
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

#### <a name="request-log-schema"></a>Schema voor aanvraag-logboek
| Naam | Type | Beschrijving |
| --- | --- | --- |
| tijd |Tekenreeks |De tijdstempel (in UTC) van het logboek |
| resourceId |Tekenreeks |De ID van de resource die bewerking vond plaats op |
| category |Tekenreeks |De logboek-categorie. Bijvoorbeeld: **aanvragen**. |
| operationName |Tekenreeks |De naam van de bewerking die wordt vastgelegd. Bijvoorbeeld: getfilestatus. |
| resultType |Tekenreeks |De status van de bewerking, bijvoorbeeld 200. |
| callerIpAddress |Tekenreeks |Het IP-adres van de client die de aanvraag |
| correlationId |Tekenreeks |De ID van het logboek dat kan worden gebruikt om een set van gerelateerde logboekvermeldingen groepen |
| identity |Object |De identiteit die door het logboek is gegenereerd |
| properties |JSON |Zie hieronder voor meer informatie |

#### <a name="request-log-properties-schema"></a>Aanvraag logboek eigenschappen schema
| Naam | Type | Beschrijving |
| --- | --- | --- |
| HttpMethod |Tekenreeks |De HTTP-methode gebruikt voor het opnieuw. Bijvoorbeeld, ophalen. |
| Pad |Tekenreeks |Het pad van de bewerking is uitgevoerd op |
| RequestContentLength |int |De lengte van de inhoud van de HTTP-aanvraag |
| clientRequestId |Tekenreeks |De ID die is uniek voor deze aanvraag |
| StartTime |Tekenreeks |Het tijdstip waarop de server de aanvraag ontvangen |
| Eindtijd |Tekenreeks |Het tijdstip waarop de server een antwoord verzonden |

### <a name="audit-logs"></a>Controlelogboeken
Hier wordt een voorbeeldvermelding voor het controlelogboek met JSON-indeling. Elke blob heeft een basis-object aangeroepen **records** die een matrix van logboek-objecten bevat

    {
    "records": 
      [        
        . . . .
        ,
        {
             "time": "2016-07-08T19:08:59.359Z",
             "resourceId": "/SUBSCRIPTIONS/<subscription_id>/RESOURCEGROUPS/<resource_group_name>/PROVIDERS/MICROSOFT.DATALAKESTORE/ACCOUNTS/<data_lake_store_account_name>",
             "category": "Audit",
             "operationName": "SeOpenStream",
             "resultType": "0",
             "correlationId": "381110fc03534e1cb99ec52376ceebdf;Append_BrEKAmg;25.66.9.145",
             "identity": "A9DAFFAF-FFEE-4BB5-A4A0-1B6CBBF24355",
             "properties": {"StreamName":"adl://<data_lake_store_account_name>.azuredatalakestore.net/logs.csv"}
        }
        ,
        . . . .
      ]
    }

#### <a name="audit-log-schema"></a>Audit log schema
| Naam | Type | Beschrijving |
| --- | --- | --- |
| tijd |Tekenreeks |De tijdstempel (in UTC) van het logboek |
| resourceId |Tekenreeks |De ID van de resource die bewerking vond plaats op |
| category |Tekenreeks |De logboek-categorie. Bijvoorbeeld: **Audit**. |
| operationName |Tekenreeks |De naam van de bewerking die wordt vastgelegd. Bijvoorbeeld: getfilestatus. |
| resultType |Tekenreeks |De status van de bewerking, bijvoorbeeld 200. |
| correlationId |Tekenreeks |De ID van het logboek dat kan worden gebruikt om een set van gerelateerde logboekvermeldingen groepen |
| identity |Object |De identiteit die door het logboek is gegenereerd |
| properties |JSON |Zie hieronder voor meer informatie |

#### <a name="audit-log-properties-schema"></a>Audit log eigenschappen schema
| Naam | Type | Beschrijving |
| --- | --- | --- |
| StreamName |Tekenreeks |Het pad van de bewerking is uitgevoerd op |

## <a name="samples-to-process-the-log-data"></a>Voorbeelden voor het verwerken van de logboekgegevens
Bij het verzenden van Logboeken van Azure Data Lake Store met Azure Log Analytics (Zie [weergeven of gegevens die worden verzameld met logboekanalyse logboek search analyseren](../log-analytics/log-analytics-tutorial-viewdata.md) voor meer informatie over het gebruik van logboekanalyse), de volgende query retourneert een tabel met een lijst van gebruikers namen van de tijd van de gebeurtenissen en het aantal gebeurtenissen weer voor de tijd van de gebeurtenis samen met een visuele grafiek. Kan eenvoudig worden gewijzigd om weer te geven van de gebruikers-GUID of andere kenmerken:

```
search *
| where ( Type == "AzureDiagnostics" )
| summarize count(TimeGenerated) by identity_s, TimeGenerated
```


Een voorbeeld van een biedt Azure Data Lake Store voor het verwerken en analyseren van de logboekgegevens. U vindt het voorbeeld op [https://github.com/Azure/AzureDataLake/tree/master/Samples/AzureDiagnosticsSample](https://github.com/Azure/AzureDataLake/tree/master/Samples/AzureDiagnosticsSample). 

## <a name="see-also"></a>Zie ook
* [Overzicht van Azure Data Lake Store](data-lake-store-overview.md)
* [Gegevens in Data Lake Store beveiligen](data-lake-store-secure-data.md)

