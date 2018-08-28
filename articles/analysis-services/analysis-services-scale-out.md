---
title: Azure Analysis Services, uitbreidbare | Microsoft Docs
description: Azure Analysis Services-servers met scale-out repliceren
author: minewiskan
manager: kfile
ms.service: azure-analysis-services
ms.topic: conceptual
ms.date: 08/27/2018
ms.author: owend
ms.reviewer: minewiskan
ms.openlocfilehash: f89a6bdbe906d490231725cf528396928faebe47
ms.sourcegitcommit: f6e2a03076679d53b550a24828141c4fb978dcf9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/27/2018
ms.locfileid: "43092091"
---
# <a name="azure-analysis-services-scale-out"></a>Azure Analysis Services-uitschalen

Met scale-out-clientquery's kunnen worden verdeeld over meerdere *queryreplica* verminderen in een querypool responstijden tijdens hoge querywerkbelastingen. U kunt ook afzonderlijke verwerking van de querypool, ervoor te zorgen dat clientquery's niet nadelig worden beïnvloed door het verwerken van bewerkingen. Scale-out kan worden geconfigureerd in Azure portal of met behulp van de Analysis Services REST-API.

## <a name="how-it-works"></a>Hoe werkt het?

In een typische server-implementatie, wordt één server fungeert als zowel de verwerkingsserver en de queryserver. Als de client-query's voor modellen op uw server meer dan de Query verwerken eenheden (QPU) voor de planning van uw server, of modelverwerking uitgevoerd op hetzelfde moment als hoge querywerkbelastingen wordt, worden de prestaties kunnen afnemen. 

U kunt een querypool met maximaal zeven extra queryreplica's (acht in totaal, met inbegrip van uw server) kunt maken met scale-out. U kunt het aantal query's worden afgestemd op QPU op kritieke perioden schalen en u kunt een verwerkingsserver van de querypool scheiden op elk gewenst moment. Alle query-replica's worden gemaakt in dezelfde regio als uw server.

Ongeacht het aantal queryreplica's in een querypool hebt, zijn de verwerkingsworkloads niet verdeeld over queryreplica's. Een enkele server fungeert als de verwerkingsserver. Queryreplica's fungeren alleen query's op de modellen gesynchroniseerd tussen elke replica in de query-groep. 

Wanneer bewerkingen voor de verwerking zijn voltooid, moet een synchronisatie tussen de verwerkingsserver en de query-replica-servers worden uitgevoerd. Bij het automatiseren van bewerkingen voor de verwerking, is het belangrijk dat u een synchronisatiebewerking na voltooiing van de verwerkingen configureren. Synchronisatie kan handmatig worden uitgevoerd in de portal of met behulp van PowerShell of REST-API.

> [!NOTE]
> Scale-out is beschikbaar voor servers in de prijscategorie Standard. Elke query-replica wordt hetzelfde tarief als uw server gefactureerd.

> [!NOTE]
> Scale-out verhoogt niet de hoeveelheid beschikbaar geheugen voor uw server. Om geheugen te maken, moet u uw abonnement upgraden.

## <a name="region-limits"></a>Regio-limieten

Het aantal query's die kunt u configureren worden beperkt door het uw server bevindt zich in de regio. Zie voor meer informatie, [beschikbaarheid per regio](analysis-services-overview.md#availability-by-region).

## <a name="monitor-qpu-usage"></a>QPU-gebruik bewaken

 Om te bepalen als scale-out voor de server nodig is, moet u de server in Azure portal controleren met behulp van metrische gegevens. Als uw QPU regelmatig loadniveau, betekent dit dat het aantal query's op uw modellen overschrijdt de limiet QPU voor uw abonnement. De Query pool taak wachtrij lengte metriek verhoogt ook wanneer het aantal query's in de querywachtrij van de thread-pool overschrijdt de beschikbare QPU. Zie [Monitor server metrics](analysis-services-monitor.md) (Metrische servergegevens bewaken) voor meer informatie.

## <a name="configure-scale-out"></a>Uitschalen configureren

### <a name="in-azure-portal"></a>In Azure portal

1. Klik in de portal op **Scale-out**. Gebruik de schuifregelaar om te selecteren van het aantal query-replicaservers. Het aantal replica's die u kiest is naast uw bestaande server.

2. In **scheiden van de verwerkingsserver van de querypool**, selecteer Ja als u wilt uitsluiten van uw verwerkingsserver van queryservers.

   ![Schuifregelaar voor scale-out](media/analysis-services-scale-out/aas-scale-out-slider.png)

3. Klik op **opslaan** voor het inrichten van uw nieuwe query-replicaservers. 

Tabellaire modellen op de primaire server worden gesynchroniseerd met de replicaservers. Wanneer synchronisatie voltooid is, begint de querypool binnenkomende query's tussen de replicaservers distribueren. 


## <a name="synchronization"></a>Synchronisatie 

Wanneer u een nieuwe query-replica's inricht, worden uw modellen automatisch gerepliceerd met Azure Analysis Services in alle replica's. U kunt ook een handmatige synchronisatie uitvoeren met behulp van de portal of REST-API. Wanneer u uw modellen verwerkt, moet u een synchronisatie uitvoeren, zodat de updates zijn gesynchroniseerd tussen uw queryreplica's.

### <a name="in-azure-portal"></a>In Azure portal

In **overzicht** > model > **synchroniseren model**.

![Schuifregelaar voor scale-out](media/analysis-services-scale-out/aas-scale-out-sync.png)

### <a name="rest-api"></a>REST-API
Gebruik de **synchronisatie** bewerking.

#### <a name="synchronize-a-model"></a>Een model synchroniseren   
`POST https://<region>.asazure.windows.net/servers/<servername>:rw/models/<modelname>/sync`

#### <a name="get-sync-status"></a>Synchronisatiestatus ophalen  
`GET https://<region>.asazure.windows.net/servers/<servername>:rw/models/<modelname>/sync`

### <a name="powershell"></a>PowerShell
Voordat u met behulp van PowerShell, [installeren of bijwerken van de meest recente AzureRM-module](https://github.com/Azure/azure-powershell/releases). 

Als u wilt dat het aantal query's, gebruikt u [Set-AzureRmAnalysisServicesServer](https://docs.microsoft.com/powershell/module/azurerm.analysisservices/set-azurermanalysisservicesserver). Geef de optionele `-ReadonlyReplicaCount` parameter.

Gebruiken om uit te voeren synchronisatie, [synchronisatie AzureAnalysisServicesInstance](https://docs.microsoft.com/powershell/module/azurerm.analysisservices/sync-azureanalysisservicesinstance).



## <a name="connections"></a>Verbindingen

Op de overzichtspagina van uw server zijn er twee servernamen. Als u scale-out voor een server nog niet hebt geconfigureerd, werken de namen van beide op dezelfde manier. Wanneer u scale-out voor een server configureert, moet u de naam van de juiste server, afhankelijk van het verbindingstype opgeven. 

Voor eindgebruikers-clientverbindingen, zoals Power BI Desktop, Excel en aangepaste apps gebruik **servernaam**. 

Voor SSMS, SSDT en verbindingsreeksen in PowerShell, Azure-functie-apps en AMO, gebruik **beheerservernaam**. Naam van de beheerserver bevat een speciaal `:rw` kwalificatie (lezen-schrijven). Alle bewerkingen voor de verwerking optreden op de beheerserver.

![Servernamen](media/analysis-services-scale-out/aas-scale-out-name.png)

## <a name="related-information"></a>Gerelateerde informatie

[Metrische servergegevens bewaken](analysis-services-monitor.md)   
[Azure analyseservices beheren](analysis-services-manage.md) 

