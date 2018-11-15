---
title: Aangepaste metrische gegevens voor een Azure-resource verzenden naar de Azure Monitor metrische store met behulp van een REST-API
description: Aangepaste metrische gegevens voor een Azure-resource verzenden naar de Azure Monitor metrische store met behulp van een REST-API
author: anirudhcavale
services: azure-monitor
ms.service: azure-monitor
ms.topic: howto
ms.date: 09/24/2018
ms.author: ancav
ms.component: metrics
ms.openlocfilehash: ccfe108b895450e97df1639f6c47eb01823ce5f4
ms.sourcegitcommit: 542964c196a08b83dd18efe2e0cbfb21a34558aa
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/14/2018
ms.locfileid: "51636761"
---
# <a name="send-custom-metrics-for-an-azure-resource-to-the-azure-monitor-metric-store-by-using-a-rest-api"></a>Aangepaste metrische gegevens voor een Azure-resource verzenden naar de Azure Monitor metrische store met behulp van een REST-API

In dit artikel wordt beschreven hoe u aangepaste metrische gegevens voor Azure-resources verzenden naar het archief met Azure Monitor metrische gegevens via een REST-API. Nadat de metrische gegevens in Azure Monitor zijn, kunt u alle dingen met hen die u met standaard metrische gegevens doen doen. Voorbeelden zijn voor grafieken, waarschuwingen en deze doorsturen naar andere externe hulpprogramma's.  

>[!NOTE]  
>De REST-API wordt alleen toegestaan voor het verzenden van aangepaste metrische gegevens voor Azure-resources. Voor het verzenden van metrische gegevens voor resources in verschillende omgevingen of on-premises, kunt u [Application Insights](../application-insights/app-insights-api-custom-events-metrics.md).    


## <a name="create-and-authorize-a-service-principal-to-emit-metrics"></a>Maken en een service-principal te verzenden van metrische gegevens 

Een service-principal maken in uw Azure Active Directory-tenant met behulp van de instructies op [maken van een service-principal](../active-directory/develop/howto-create-service-principal-portal.md). 

Let op het volgende terwijl u dit proces doorlopen: 

- U kunt een URL invoeren voor de URL van de aanmelding.  
- Maak een nieuwe clientgeheim voor deze app.  
- Sla de sleutel en de client-ID voor gebruik in latere stappen.  

Geef de app gemaakt als onderdeel van stap 1, bewaking metrische gegevens over uitgever en machtigingen voor de resource die u wilt verzenden van metrische gegevens tegen. Als u van plan bent de app om te verzenden van aangepaste metrische gegevens op veel resources gebruiken, kunt u deze machtigingen op het niveau van de resource-groep of abonnement verlenen. 

## <a name="get-an-authorization-token"></a>Een verificatietoken ophalen
Open een opdrachtprompt en voer de volgende opdracht uit:

```shell
curl -X POST https://login.microsoftonline.com/<yourtenantid>/oauth2/token -F "grant_type=client_credentials" -F "client_id=<insert clientId from earlier step> " -F "client_secret=<insert client secret from earlier step>" -F "resource=https://monitoring.azure.com/"
```
Sla het toegangstoken op uit het antwoord.

![Toegangstoken](./media/metrics-store-custom-rest-api/accesstoken.png)

## <a name="emit-the-metric-via-the-rest-api"></a>Verzenden van de metrische gegevens via de REST-API 

1. Plak de volgende JSON in een bestand en sla het bestand als **custommetric.json** op uw lokale computer. Update voor de parameter time in het JSON-bestand: 
    
    ```json
    { 
        "time": "2018-09-13T16:34:20", 
        "data": { 
            "baseData": { 
                "metric": "QueueDepth", 
                "namespace": "QueueProcessing", 
                "dimNames": [ 
                  "QueueName", 
                  "MessageType" 
                ], 
                "series": [ 
                  { 
                    "dimValues": [ 
                      "ImagesToProcess", 
                      "JPEG" 
                    ], 
                    "min": 3, 
                    "max": 20, 
                    "sum": 28, 
                    "count": 3 
                  } 
                ] 
            } 
        } 
    } 
    ``` 

1. In het venster Opdrachtprompt plaatsen de metrische gegevens: 
    - **Azureregio**. Moet overeenkomen met de implementatieregio van de resource die u bij het verzenden van metrische gegevens voor. 
    - **resourceID**.  Resource-ID van de Azure-resource die u de metrische gegevens tegen bijhoudt.  
    - **AccessToken**. Plak het token dat u eerder hebt verkregen.

    ```Shell 
    curl -X POST curl -X POST https://<azureRegion>.monitoring.azure.com/<resourceId>/metrics -H "Content-Type: application/json" -H "Authorization: Bearer <AccessToken>" -d @custommetric.json 
    ```
1. Wijzig de tijdstempel en de waarden in het JSON-bestand. 
1. Herhaal de vorige twee stappen een paar keer, zodat u gegevens voor enkele minuten hebt.

## <a name="troubleshooting"></a>Problemen oplossen 
Als u een foutbericht weergegeven met een deel van het proces wordt, kunt u de volgende informatie voor probleemoplossing:

1. U kunt geen metrische gegevens op basis van een abonnement of resourcegroep uitgeven als uw Azure-resource. 
1. U kunt geen een metrische waarde plaatsen in het archief dat is meer dan 20 minuten. De metrische store is geoptimaliseerd voor waarschuwingen en realtime voor grafieken. 
2. Het aantal dimensienamen moet overeenkomen met de waarden en vice versa. Controleer de waarden. 
2. Mogelijk dat u verzendt metrische gegevens op basis van een gebied dat biedt geen ondersteuning voor aangepaste metrische gegevens. Zie [ondersteunde regio's](metrics-custom-overview.md#supported-regions). 



## <a name="view-your-metrics"></a>Bekijk uw metrische gegevens 

1. Meld u aan bij Azure Portal. 

1. Selecteer in het menu links **Monitor**. 

1. Op de **Monitor** weergeeft, schakelt **metrische gegevens**. 

   ![Metrische gegevens selecteren](./media/metrics-store-custom-rest-api/metrics.png) 

1. Wijzig de aggregatie periode **afgelopen 30 minuten**.  

1. In de **resource** vervolgkeuzelijst, selecteer de resource die u de metrische gegevens tegen verzonden.  

1. In de **naamruimten** vervolgkeuzelijst in het menu **QueueProcessing**. 

1. In de **metrische gegevens** vervolgkeuzelijst in het menu **QueueDepth**.  

 
## <a name="next-steps"></a>Volgende stappen
- Meer informatie over [aangepaste metrische gegevens](metrics-custom-overview.md).
