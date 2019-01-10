---
title: Azure Analysis Services, uitbreidbare | Microsoft Docs
description: Azure Analysis Services-servers met scale-out repliceren
author: minewiskan
manager: kfile
ms.service: azure-analysis-services
ms.topic: conceptual
ms.date: 01/09/2019
ms.author: owend
ms.reviewer: minewiskan
ms.openlocfilehash: 775de554f39df8359c3852a2d7fa876fd12199d2
ms.sourcegitcommit: 63b996e9dc7cade181e83e13046a5006b275638d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/10/2019
ms.locfileid: "54190813"
---
# <a name="azure-analysis-services-scale-out"></a>Azure Analysis Services-uitschalen

Met scale-out-clientquery's kunnen worden verdeeld over meerdere *queryreplica* verminderen in een querypool responstijden tijdens hoge querywerkbelastingen. U kunt ook afzonderlijke verwerking van de querypool, ervoor te zorgen dat clientquery's niet nadelig worden beïnvloed door het verwerken van bewerkingen. Scale-out kan worden geconfigureerd in Azure portal of met behulp van de Analysis Services REST-API.

## <a name="how-it-works"></a>Hoe werkt het?

In een typische server-implementatie, wordt één server fungeert als zowel de verwerkingsserver en de queryserver. Als de client-query's voor modellen op uw server meer dan de Query verwerken eenheden (QPU) voor de planning van uw server, of modelverwerking uitgevoerd op hetzelfde moment als hoge querywerkbelastingen wordt, worden de prestaties kunnen afnemen. 

U kunt een querypool met maximaal zeven extra query replica resources (acht in totaal, met inbegrip van uw server) kunt maken met scale-out. U kunt het aantal query's worden afgestemd op QPU op kritieke perioden schalen en u kunt een verwerkingsserver van de querypool scheiden op elk gewenst moment. Alle query-replica's worden gemaakt in dezelfde regio als uw server.

Ongeacht het aantal queryreplica's in een querypool hebt, zijn de verwerkingsworkloads niet verdeeld over queryreplica's. Een enkele server fungeert als de verwerkingsserver. Queryreplica's fungeren alleen query's op de modellen gesynchroniseerd tussen de replica van elke query in de query-groep. 

Bij het uitschalen, worden nieuwe query-replica's toegevoegd aan de querypool incrementeel. Het kan tot vijf minuten voor nieuwe query replica resources moeten worden opgenomen in de query-groep duren. Wanneer alle nieuwe query-replica's zijn en wordt uitgevoerd, worden nieuwe clientverbindingen zijn verdeeld over alle resources in de query. Bestaande clientverbindingen zijn niet gewijzigd van de resource die momenteel zijn verbonden met.  Wanneer u, worden bestaande clientverbindingen met een query pool-resource die wordt verwijderd van de querypool beëindigd. Ze zijn verbonden aan een andere query pool resource wanneer de schaal in de bewerking is voltooid, wat tot vijf minuten kan duren.

Bij het verwerken van modellen, nadat de bewerkingen zijn voltooid, moet een synchronisatie tussen de verwerkingsserver en de replica van de query's worden uitgevoerd. Bij het automatiseren van bewerkingen voor de verwerking, is het belangrijk dat u een synchronisatiebewerking na voltooiing van de verwerkingen configureren. Synchronisatie kan handmatig worden uitgevoerd in de portal of met behulp van PowerShell of REST-API. 

### <a name="separate-processing-from-query-pool"></a>Afzonderlijk te verwerken van de querypool

Voor maximale prestaties voor verwerking en querybewerkingen, kunt u kiezen voor het scheiden van de verwerkingsserver van de querypool. Wanneer gescheiden, worden bestaande en nieuwe clientverbindingen zijn toegewezen aan de query-replica's in de groep van de query alleen. Als een korte periode alleen bewerkingen voor de verwerking nemen, kunt u voor het scheiden van uw verwerkingsserver van de querypool alleen voor de hoeveelheid tijd die nodig is het uitvoeren van bewerkingen voor verwerking en synchronisatie en neemt u deze terug naar de querypool. 

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

2. In **scheiden van de verwerkingsserver van de querypool**, selecteer Ja als u wilt uitsluiten van uw verwerkingsserver van queryservers. Verbindingen van clients met behulp van de standaard-verbindingsreeks (zonder: rw) worden omgeleid naar de replica's in de query-groep. 

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

`GET https://<region>.asazure.windows.net/servers/<servername>/models/<modelname>/sync`

### <a name="powershell"></a>PowerShell

Voordat u met behulp van PowerShell, [installeren of bijwerken van de meest recente AzureRM-module](https://github.com/Azure/azure-powershell/releases). 

Als u wilt dat het aantal query's, gebruikt u [Set-AzureRmAnalysisServicesServer](https://docs.microsoft.com/powershell/module/azurerm.analysisservices/set-azurermanalysisservicesserver). Geef de optionele `-ReadonlyReplicaCount` parameter.

Gebruiken om uit te voeren synchronisatie, [synchronisatie AzureAnalysisServicesInstance](https://docs.microsoft.com/powershell/module/azurerm.analysisservices/sync-azureanalysisservicesinstance).

## <a name="connections"></a>Verbindingen

Op de overzichtspagina van uw server zijn er twee servernamen. Als u scale-out voor een server nog niet hebt geconfigureerd, werken de namen van beide op dezelfde manier. Wanneer u scale-out voor een server configureert, moet u de naam van de juiste server, afhankelijk van het verbindingstype opgeven. 

Voor eindgebruikers-clientverbindingen, zoals Power BI Desktop, Excel en aangepaste apps gebruik **servernaam**. 

Voor SSMS, SSDT en verbindingsreeksen in PowerShell, Azure-functie-apps en AMO, gebruik **beheerservernaam**. Naam van de beheerserver bevat een speciaal `:rw` kwalificatie (lezen-schrijven). Alle bewerkingen voor de verwerking optreden op de beheerserver.

![Servernamen](media/analysis-services-scale-out/aas-scale-out-name.png)

## <a name="troubleshoot"></a>Problemen oplossen

**Probleem:** Gebruikers krijgen fout **kan geen server vinden '\<naam van de server >' exemplaar in de verbindingsmodus 'Alleen-lezen'.**

**Oplossing:** Bij het selecteren van de **scheiden van de verwerkingsserver van de querypool** optie, client-verbindingen met behulp van de standaard-verbindingsreeks (zonder: rw) worden omgeleid naar de pool queryreplica's. Als de replica's in de groep van de query nog niet zijn nog online omdat synchronisatie niet is voltooid, wordt omgeleid clientverbindingen kunnen mislukken. Om te voorkomen dat een mislukte verbindingen, kies niet voor het scheiden van de verwerkingsserver van de querypool totdat een bewerking waarbij de scale-out en synchroniseren zijn voltooid. U kunt de metrische gegevens over geheugen en QPU gebruiken voor het bewaken van de synchronisatiestatus.

## <a name="related-information"></a>Gerelateerde informatie

[Metrische servergegevens bewaken](analysis-services-monitor.md)   
[Azure analyseservices beheren](analysis-services-manage.md) 

