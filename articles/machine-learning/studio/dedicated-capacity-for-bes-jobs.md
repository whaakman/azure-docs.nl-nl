---
title: Toegewezen capaciteit voor Machine Learning uitvoering Service batchtaken | Microsoft Docs
description: Overzicht van Azure Batch-services voor Machine Learning-taken.
services: machine-learning
documentationcenter: 
author: garyericson
manager: raymondl
editor: 
ms.service: machine-learning
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/19/2017
ms.author: v-donglo
ms.openlocfilehash: 7f7498c63db89a77121d33afc9d48a4132b1a51d
ms.sourcegitcommit: 42ee5ea09d9684ed7a71e7974ceb141d525361c9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/09/2017
---
# <a name="azure-batch-service-for-machine-learning-jobs"></a>Azure Batch-service voor Machine Learning-taken

Machine Learning Batch-Pool verwerking biedt door de klant beheerd schaal voor de Service Azure Machine Learning Batch worden uitgevoerd. Klassieke batchverwerking voor machine learning vindt plaats in een omgeving met meerdere tenants, waardoor het aantal gelijktijdige taken die u kunt verzenden en taken in de wachtrij op basis van de eerste in first out. Deze onzekerheid betekent dat valt niet nauwkeurig te voorspellen wanneer de taak wordt uitgevoerd.

Batch-Pool verwerking kunt u groepen waarop u kunt indienen maken batchtaken. Bepalen van de grootte van de groep en aan welke groep de taak wordt verzonden. Uw BES-taak wordt uitgevoerd in een eigen verwerking ruimte biedt voorspelbare verwerking en de mogelijkheid te maken van resourcegroepen die overeenkomen met de verwerkingsbelasting die u verzendt.

## <a name="how-to-use-batch-pool-processing"></a>Het gebruik van de verwerking van de Batch-Pool

Configuratie van de verwerking van de Batch-Pool is momenteel niet beschikbaar via de Azure portal. Voor het gebruik van de verwerking van de Batch-Pool, moet u het volgende doen:

-   Aanroepen van CSS voor het maken van een Batch-Pool-Account en de URL van een Pool-Service en een autorisatiesleutel verkrijgen
-   Een nieuwe Resource Manager gebaseerde web-service en een abonnement maken

Maken van uw account, bellen Microsoft Customer Service and Support (CSS) en geef uw abonnement-ID. CSS geschikt is voor u om te bepalen van de juiste capaciteit voor uw scenario. CSS configureert vervolgens een account met het maximum aantal groepen die u kunt maken en het maximum aantal virtuele machines (VM's) die u kunt in elke groep van toepassingen plaatsen. Wanneer uw account is geconfigureerd, kunt u de URL van de Pool-Service en een autorisatiesleutel zijn opgegeven.

Nadat uw account is gemaakt, kunt u de sleutel Pool Service-URL en autorisatie van toepassingen management bewerkingen uitvoeren op uw Batch-Pool.

![Architectuur van batch pool-service.](./media/dedicated-capacity-for-bes-jobs/pool-architecture.png)

U maken groepen door het aanroepen van de groep maken-bewerking op de URL van de groep van toepassingen die CSS aan u worden verstrekt. Wanneer u een pool maakt, geef dat het aantal virtuele machines en de URL van de swagger.json van een nieuwe Resource Manager op basis van Machine Learning-webservice. Deze webservice is opgegeven om de facturering koppeling te maken. De Batch-Pool-service gebruikt de swagger.json voor de pool koppelen aan een abonnement. U zowel nieuwe Resource Manager gebaseerde webservice kunt uitvoeren voor elke BES en klassieke, die u kiest in de groep.

U kunt een nieuwe Resource Manager gebaseerde webservice gebruiken, maar houd er rekening mee dat de facturering voor de taken worden in rekening gebracht op basis van het abonnement dat is gekoppeld aan die service. U kunt een web-service en een nieuw abonnement specifiek voor het uitvoeren van taken in Batch-Pool maken.

Zie voor meer informatie over het maken van webservices [Azure Machine Learning-webservice implementeren](publish-a-machine-learning-web-service.md).

Nadat u een groep hebt gemaakt, kunt u de BES taak voor de webservice met behulp van de URL van de Batch-aanvragen indienen. U kunt verzenden naar een groep of klassieke batchverwerking. Als u een taak voor het verwerken van Batch-Pool, kunt u de volgende parameter toevoegen aan de hoofdtekst van de aanvraag van de taak verzenden:

'AzureBatchPoolId': '&lt;ID-groep&gt;'

Als u de parameter niet toevoegt, wordt de taak wordt uitgevoerd in de klassieke batch proces-omgeving. Als de groep beschikbare bronnen heeft, wordt de taak wordt onmiddellijk uitgevoerd. Als de groep momenteel geen gratis resources, uw taak in de wachtrij staat tot een resource beschikbaar is.

Als u dat u de capaciteit van uw pools regelmatig bereiken en u de grotere capaciteit moet vinden, kunt u CSS aanroepen en werken met een medewerker van de quota verhogen.

Voorbeeld van de aanvraag:

https://ussouthcentral.Services.azureml.NET/Subscriptions/80c77c7674ba4c8c82294c3b2957990c/Services/9fe659022c9747e3b9b7b923c3830623/Jobs?API-Version=2.0

```json
{

    "Input":{
    
        "ConnectionString":"DefaultEndpointsProtocol=https;BlobEndpoint=https://sampleaccount.blob.core.windows.net/;TableEndpoint
        =https://sampleaccount.table.core.windows.net/;QueueEndpoint=https://sampleaccount.queue.core.windows.net/;FileEndpoint=https://zhguim
        l.file.core.windows.net/;AccountName=sampleaccount;AccountKey=&lt;Key&gt;;",
        
        "BaseLocation":null,
        
        "RelativeLocation":"testint/AdultCensusIncomeBinaryClassificationDataset.csv",
        
        "SasBlobToken":null
    
    },
    
    "GlobalParameters":{ },
    
    "Outputs":{
    
        "output1":{
        
            "ConnectionString":"DefaultEndpointsProtocol=https;BlobEndpoint=https://sampleaccount.blob.core.windows.net/;TableEndpo
            int=https://sampleaccount.table.core.windows.net/;QueueEndpoint=https://sampleaccount.queue.core.windows.net/;FileEndpoint=https://sampleaccount.file.core.windows.net/;AccountName=sampleaccount;AccountKey=&lt;Key&gt;",
            "BaseLocation":null,
            "RelativeLocation":"testintoutput/testint\_results.csv",
            
            "SasBlobToken":null
        
        }
    
    },
    
    "AzureBatchPoolId":"8dfc151b0d3e446497b845f3b29ef53b"

}
```

## <a name="considerations-when-using-batch-pool-processing"></a>Overwegingen bij het gebruik van de verwerking van de Batch-Pool

Batch-Pool verwerking is een factureerbare service altijd op en waarvoor u deze koppelen aan een Resource Manager op basis van abonnement is vereist. U wordt alleen gefactureerd voor het aantal rekenuren die de pool wordt uitgevoerd. ongeacht het aantal taken gedurende die tijd van toepassingen worden uitgevoerd. Als u een pool maakt, wordt u gefactureerd voor de rekenuren van elke virtuele machine in de groep totdat de pool wordt verwijderd, zelfs als er geen batchtaken worden uitgevoerd in de groep. Facturering voor de virtuele machines wordt gestart wanneer ze klaar zijn met het inrichten en stopt wanneer ze zijn verwijderd. U kunt een van de abonnementen gevonden op de [Machine Learning-prijzen pagina](https://azure.microsoft.com/pricing/details/machine-learning/).

Facturering voorbeeld:

Als u een Batch-Pool met 2 virtuele machines maken en die na 24 uur die uw abonnement is 48 gedebiteerd rekenuren; verwijderen ongeacht hoeveel taken zijn uitgevoerd tijdens deze periode.

Als u een Batch-Pool met 4 virtuele machines maken en na 12 uur verwijderen, wordt uw abonnement is ook gedebiteerd 48 rekenuren.

Het is raadzaam om het pollen van de status van de taak om te bepalen wanneer taken zijn voltooid. Wanneer alle uw taken uitgevoerd hebt, moet u de bewerking vergroten of verkleinen van toepassingen voor het instellen van het aantal virtuele machines in de groep op nul aanroepen. Als u op de resources in de korte en u een nieuwe groep, bijvoorbeeld moet gefactureerd tegen een ander abonnement maken kunt u de groep in plaats daarvan verwijderen wanneer alle uw taken zijn voltooid.


| **Batch-Pool verwerking als gebruiken**    | **Klassieke batchverwerking wanneer gebruiken**  |
|---|---|
|U moet een groot aantal taken uitvoeren<br>of<br/>U moet weten dat uw taken onmiddellijk worden uitgevoerd<br/>of<br/>U moet gegarandeerde doorvoer. Bijvoorbeeld, moet u een aantal taken uitvoeren in een bepaalde periode en wilt uw rekenresources om te voldoen aan uw behoeften uitbreiden.    | U kunt een paar taken worden uitgevoerd<br/>En<br/> U hoeft niet de taken onmiddellijk uitvoeren |
