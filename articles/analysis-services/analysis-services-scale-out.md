---
title: Azure Analysis Services scale-out | Microsoft Docs
description: Azure Analysis Services-servers met scale-out repliceren
services: analysis-services
documentationcenter: 
author: minewiskan
manager: erikre
editor: 
ms.assetid: 
ms.service: analysis-services
ms.workload: data-management
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/06/2017
ms.author: owend
ms.openlocfilehash: 14bdbf3dd6d940cc3f4b665658f0c789916a2597
ms.sourcegitcommit: b07d06ea51a20e32fdc61980667e801cb5db7333
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/08/2017
---
# <a name="azure-analysis-services-scale-out"></a>Azure Analysis Services scale-out

Met scale-out clientquery's kunnen worden verdeeld over meerdere *query uitvoeren op replica's* in een pool query verminderen reactietijden tijdens hoge query werkbelastingen. U kunt ook scheiden van de groep van de query verwerken, er wordt gezorgd clientaanvragen niet nadelig worden beïnvloed door de verwerking van bewerkingen. Scale-out kan worden geconfigureerd in Azure portal of met behulp van de REST-API van Analysis Services.

## <a name="how-it-works"></a>Hoe werkt het?

In een implementatie voor typische server fungeert één server als zowel de verwerking server en de queryserver. Als het aantal clientaanvragen op modellen op uw server de Query verwerken eenheden (QPU) voor uw server plan overschrijdt of modelverwerking vindt plaats op hetzelfde moment als hoge query werkbelastingen, worden de prestaties kunnen afnemen. 

Met scale-out, kunt u een query-pool met maximaal zeven extra query replica's (acht totaal, met inbegrip van uw server). U kunt het aantal replica's query om te voldoen aan de eisen van QPU op essentiële tijdstippen schalen en u kunt een server voor verwerking van de query-groep op elk gewenst moment scheiden. 

Ongeacht het aantal query-replica's die u in de groep van een query hebt zijn niet verwerkingsbelastingen verdeeld over query replica's. Eén server fungeert als de server voor verwerking. Query-replica's dienen alleen query's op de modellen gesynchroniseerd tussen elke replica in de query-groep. 

Wanneer bewerkingen zijn voltooid, moet u een synchronisatie uitgevoerd tussen de server verwerking en de query-replicaservers. Wanneer u bewerkingen voor de verwerking, is het is belangrijk voor het configureren van een synchronisatiebewerking na voltooiing van de verwerking van bewerkingen. Synchronisatie kan handmatig worden uitgevoerd in de portal of met behulp van PowerShell of REST-API.

> [!NOTE]
> Scale-out is beschikbaar voor servers in de Standard-prijscategorie. Elke replica query wordt gefactureerd op dezelfde snelheid als uw server.

> [!NOTE]
> Scale-out vergroot de hoeveelheid beschikbaar geheugen voor de server niet. Om geheugen te maken, moet u uw abonnement upgraden.

## <a name="monitor-qpu-usage"></a>Monitor QPU gebruik

 Om te bepalen of scale-out voor de server nodig is, door de server in Azure-portal met behulp van de metrische gegevens te controleren. Als uw QPU regelmatig maximaliseert, betekent dit dat het aantal query's op uw modellen overschrijdt de limiet QPU voor uw abonnement. De Query groep taak wachtrij lengte metriek verhoogt ook wanneer het aantal query's in de wachtrij voor de query thread groep beschikbaar QPU overschrijdt. Zie voor meer informatie, [metrische servergegevens controleren](analysis-services-monitor.md).

## <a name="configure-scale-out"></a>Configureren van scale-out

### <a name="in-azure-portal"></a>In Azure-portal

1. Klik in de portal op **Scale-out**. Gebruik de schuifregelaar om het aantal query-replicaservers te selecteren. Het aantal replica's die u kiest is naast de bestaande server.

2. In **scheiden van de server verwerking van de groep opvragen**, selecteer Ja als u wilt uitsluiten van uw server voor de verwerking van queryservers.

   ![Scale-out schuifregelaar](media/analysis-services-scale-out/aas-scale-out-slider.png)

3. Klik op **opslaan** uw nieuwe query replicaservers inrichten. 

Modellen in tabelvorm op uw primaire server worden gesynchroniseerd met de replicaservers. Wanneer synchronisatie voltooid is, begint de query-groep voor het distribueren van binnenkomende query's tussen de replicaservers. 


## <a name="synchronization"></a>Synchronisatie 

Wanneer u een nieuwe query-replica's inricht, repliceert Azure Analysis Services uw modellen automatisch op alle replica's. U kunt ook een handmatige synchronisatie uitvoeren met behulp van de portal of REST-API. Wanneer u uw modellen verwerkt, moet u een synchronisatie uitvoeren, zodat de updates zijn gesynchroniseerd tussen de replica van uw query's.

### <a name="in-azure-portal"></a>In Azure-portal

In **overzicht** > model > **synchroniseren model**.

![Scale-out schuifregelaar](media/analysis-services-scale-out/aas-scale-out-sync.png)

### <a name="rest-api"></a>REST API
Gebruik de **sync** bewerking.

#### <a name="synchronize-a-model"></a>Een model synchroniseren   
`POST https://<region>.asazure.windows.net/servers/<servername>:rw/models/<modelname>/sync`

#### <a name="get-sync-status"></a>Synchronisatiestatus ophalen  
`GET https://<region>.asazure.windows.net/servers/<servername>:rw/models/<modelname>/sync`

### <a name="powershell"></a>PowerShell
Synchronisatie uitvoeren van PowerShell, [bijwerken naar de meest recente](https://github.com/Azure/azure-powershell/releases) 5.01 of hoger AzureRM-module. Gebruik [Sync AzureAnalysisServicesInstance](https://docs.microsoft.com/powershell/module/azurerm.analysisservices/sync-azureanalysisservicesinstance).

## <a name="connections"></a>Verbindingen

Op de pagina overzicht van uw server zijn er twee servernamen. Als u scale-out voor een server nog niet hebt geconfigureerd, werken beide servernamen hetzelfde. Wanneer u scale-out voor een server configureert, moet u de naam van de juiste server afhankelijk van het verbindingstype opgeven. 

Voor eindgebruikers clientverbindingen zoals Power BI Desktop-, Excel- en aangepaste apps gebruik **servernaam**. 

SSMS SSDT en verbindingsreeksen in PowerShell, Azure-functie apps en AMO, gebruik voor **beheerservernaam**. Naam van de beheerserver omvat een speciale `:rw` kwalificatie (lezen / schrijven). Alle bewerkingen voor de verwerking optreden op de beheerserver.

![Namen van server](media/analysis-services-scale-out/aas-scale-out-name.png)

## <a name="related-information"></a>Gerelateerde informatie

[Monitor server metrische gegevens](analysis-services-monitor.md)   
[Azure analyseservices beheren](analysis-services-manage.md) 

