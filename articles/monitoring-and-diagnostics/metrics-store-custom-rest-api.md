---
title: Aangepaste metrische gegevens voor een Azure-resource in de Azure Monitor-metriek opslaan met een REST-API's verzenden
description: Aangepaste metrische gegevens voor een Azure-resource in de Azure Monitor-metriek opslaan met een REST-API's verzenden
author: anirudhcavale
services: azure-monitor
ms.service: azure-monitor
ms.topic: howto
ms.date: 09/24/2018
ms.author: ancav
ms.component: metrics
ms.openlocfilehash: d36697e6b5765ecf35ed9b3add45cff6c33823a5
ms.sourcegitcommit: 5c00e98c0d825f7005cb0f07d62052aff0bc0ca8
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/24/2018
ms.locfileid: "49958215"
---
# <a name="send-custom-metrics-for-an-azure-resource-to-the-azure-monitor-metric-store-using-a-rest-api"></a>Aangepaste metrische gegevens voor een Azure-resource in de Azure Monitor-metriek opslaan met een REST-API's verzenden

In dit artikel wordt beschreven hoe u aangepaste metrische gegevens voor Azure-resources verzenden naar het archief met Azure Monitor metrische gegevens via een REST-API.  Zodra de metrische gegevens in Azure Monitor zijn, kunt u doen alles wat met hen die u met standaard metrische gegevens doen, zoals grafieken, waarschuwingen, routering ze naar andere externe hulpprogramma's, enzovoort.  

>[!NOTE] 
>De REST-API wordt alleen toegestaan voor het verzenden van aangepaste metrische gegevens voor Azure-resources. Voor het verzenden van metrische gegevens voor resources in verschillende omgevingen of on-premises, kunt u [Application Insights](../application-insights/app-insights-api-custom-events-metrics.md).    


## <a name="create-and-authorize-a-service-principal-to-emit-metrics"></a>Maken en een service-principal te verzenden van metrische gegevens 

Een service-principal maken in uw Azure Active Directory-tenant met behulp van de instructies op [maken van een service-principal](../active-directory/develop/howto-create-service-principal-portal.md). 

Let op het volgende tijdens dit proces te doorlopen: 

- U kunt opnemen in een URL voor de aanmeldings-URL.  
- Nieuwe clientgeheim voor deze app maken  
- Sla de sleutel en de client-ID voor gebruik in latere stappen.  

Geef de app gemaakt als onderdeel van stap 1 'Bewaking van metrische gegevens Publisher'-machtigingen voor de resource die u wilt verzenden van metrische gegevens tegen. Als u van plan bent de app om te verzenden van aangepaste metrische gegevens op veel resources gebruiken, kunt u deze machtigingen op het niveau van de resource-groep of abonnement verlenen. 

## <a name="get-an-authorization-token"></a>Een verificatietoken ophalen
Open een opdrachtprompt en voer de volgende opdracht uit
```shell
curl -X POST https://login.microsoftonline.com/<yourtenantid>/oauth2/token -F "grant_type=client_credentials" -F "client_id=<insert clientId from earlier step> " -F "client_secret=<insert client secret from earlier step>" -F "resource=https://monitoring.azure.com/"
```
Het toegangstoken van het antwoord opslaan

![Toegangstoken](./media/metrics-store-custom-rest-api/accesstoken.png)

## <a name="emit-the-metric-via-the-rest-api"></a>Verzenden van de metrische gegevens via de REST-API 

1. Plak de volgende JSON naar een bestand en sla deze op als custommetric.json op uw lokale computer. De parameter time in het JSON-bestand bijwerken. 
    
    ```json
    { 
        "time": "2018-09-13T16:34:20”, 
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

1. In het venster Opdrachtprompt plaatsen de metrische gegevens 
    - Azure-regio, moet overeenkomen met de implementatieregio van de resource die u bij het verzenden van metrische gegevens voor. 
    - ResourceID: Resource-ID van de Azure-resource houdt u de metriek op.  
    - Toegangstoken: plak het token dat is eerder verkregen

    ```Shell 
    curl -X POST curl -X POST https://<azureRegion>.monitoring.azure.com/<resourceId> /metrics -H "Content-Type: application/json" -H "Authorization: Bearer <AccessToken>" -d @custommetric.json 
    ```
1. Wijzig de tijdstempel en de waarden in het JSON-bestand. 
1. Vorige twee stappen een paar keer herhalen, zodat u gegevens hebt die meerdere minuten.

## <a name="troubleshooting"></a>Problemen oplossen 
Als u een fout opgetreden bij een deel van het proces, ontvangt de volgende

1. U kunt geen metrische gegevens op basis van een abonnement of resourcegroep uitgeven als uw Azure-resource. 
1. U kunt geen een metrische waarde plaatsen in het archief dat is meer dan 20 minuten. De metrische store is geoptimaliseerd voor waarschuwingen en realtime voor grafieken. 
2. Het aantal dimensienamen moet overeenkomen met de waarden en vice versa. Controleer de waarden. 
2. Mogelijk dat u verzendt metrische gegevens op basis van de regio die biedt geen ondersteuning voor aangepaste metrische gegevens. Zie [ondersteunde regio's voor aangepaste metrische gegevens (preview)](metrics-custom-overview.md#supported-regions) 



## <a name="view-your-metrics"></a>Bekijk uw metrische gegevens 

1. Aanmelden bij Azure Portal 

1. Klik in het menu links op **Monitor** 

1. Klik op de pagina Monitor **metrische gegevens**. 

   ![Toegangstoken](./media/metrics-store-custom-rest-api/metrics.png) 

1. Wijzig de aggregatie periode **afgelopen 30 minuten**.  

1. In de *resource* vervolgkeuzelijst, selecteer de resource die u de metrische gegevens tegen verzonden.  

1. In de *naamruimten* vervolgkeuzelijst, selecteer **QueueProcessing** 

1. In de *metrische gegevens* vervolgkeuzelijst, selecteer **QueueDepth**.  

 
## <a name="next-steps"></a>Volgende stappen
- Meer informatie over [aangepaste metrische gegevens](metrics-custom-overview.md).