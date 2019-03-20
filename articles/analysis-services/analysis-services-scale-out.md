---
title: Azure Analysis Services, uitbreidbare | Microsoft Docs
description: Azure Analysis Services-servers met scale-out repliceren
author: minewiskan
manager: kfile
ms.service: azure-analysis-services
ms.topic: conceptual
ms.date: 03/18/2019
ms.author: owend
ms.reviewer: minewiskan
ms.openlocfilehash: eae1569cf6f7ada89f64b96fe81b154b84932a12
ms.sourcegitcommit: 5839af386c5a2ad46aaaeb90a13065ef94e61e74
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/19/2019
ms.locfileid: "58182843"
---
# <a name="azure-analysis-services-scale-out"></a>Azure Analysis Services-uitschalen

Met scale-out-clientquery's kunnen worden verdeeld over meerdere *queryreplica* in een *pool query*, responstijden tijdens hoge querywerkbelastingen verminderen. U kunt ook afzonderlijke verwerking van de querypool, ervoor te zorgen dat clientquery's niet nadelig worden beïnvloed door het verwerken van bewerkingen. Scale-out kan worden geconfigureerd in Azure portal of met behulp van de Analysis Services REST-API.

Scale-out is beschikbaar voor servers in de prijscategorie Standard. Elke query-replica wordt hetzelfde tarief als uw server gefactureerd. Alle query-replica's worden gemaakt in dezelfde regio als uw server. Het aantal query's die kunt u configureren worden beperkt door het uw server bevindt zich in de regio. Zie voor meer informatie, [beschikbaarheid per regio](analysis-services-overview.md#availability-by-region). Scale-out verhoogt niet de hoeveelheid beschikbaar geheugen voor uw server. Om geheugen te maken, moet u uw abonnement upgraden. 

## <a name="why-scale-out"></a>Waarom scale-out?

In een typische server-implementatie, wordt één server fungeert als zowel de verwerkingsserver en de queryserver. Als de client-query's voor modellen op uw server meer dan de Query verwerken eenheden (QPU) voor de planning van uw server, of modelverwerking uitgevoerd op hetzelfde moment als hoge querywerkbelastingen wordt, worden de prestaties kunnen afnemen. 

Met scale-out, kunt u een querypool met maximaal zeven extra query replica resources (acht totaal, met inbegrip van uw *primaire* server). U kunt het aantal replica's in de query-groep worden afgestemd op QPU op kritieke perioden schalen en u kunt een verwerkingsserver van de querypool scheiden op elk gewenst moment. 

Ongeacht het aantal queryreplica's in een querypool hebt, zijn de verwerkingsworkloads niet verdeeld over queryreplica's. De primaire server fungeert als de verwerkingsserver. Queryreplica's fungeren alleen query's op de model-databases gesynchroniseerd tussen de primaire server en elke replica in de query-groep. 

Bij het uitschalen, kan het tot vijf minuten voor nieuwe query-replica's stapsgewijs worden toegevoegd aan de querypool duren. Wanneer alle nieuwe query-replica's zijn en wordt uitgevoerd, worden nieuwe clientverbindingen zijn verdeeld over resources in de querypool. Bestaande clientverbindingen zijn niet gewijzigd van de resource die momenteel zijn verbonden met. Wanneer u, worden bestaande clientverbindingen met een query pool-resource die wordt verwijderd van de querypool beëindigd. Clients kunnen opnieuw verbinding met een andere query pool resource.

## <a name="how-it-works"></a>Hoe werkt het?

Wanneer u scale-out voor het eerst configureert, model-databases op uw primaire server zijn *automatisch* gesynchroniseerd met de nieuwe replica's in een nieuwe query-groep. Automatische synchronisatie plaatsvindt slechts één keer. Tijdens automatische synchronisatie zijn van de primaire server gegevens (versleuteld in rust in blob-opslag) gekopieerd naar een tweede locatie, ook versleuteld in rust in blob-opslag. Replica's in de groep query worden vervolgens *gehydrateerd* met gegevens uit de tweede set bestanden. 

Tijdens een automatische synchronisatie wordt uitgevoerd alleen wanneer u scale-out een server voor de eerste keer, kunt u ook een handmatige synchronisatie uitvoeren. Zorgen voor synchroniseren van gegevens op de replica's in de groep query overeenkomen met die van de primaire server. Bij het verwerken van modellen op de primaire server (vernieuwen), een synchronisatie moet worden uitgevoerd *nadat* bewerkingen zijn voltooid. Deze synchronisatie kopieert bijgewerkte gegevens van de primaire server-bestanden in blob-opslag in de tweede set bestanden. Replica's in de query-groep wordt gehydrateerd met bijgewerkte gegevens van de tweede set met bestanden in blob storage. 

Wanneer u de volgende scale-out-bewerking uitvoert, bijvoorbeeld zijn het aantal replica's in de querypool van twee tot vijf, verhoogt de nieuwe replica's gehydrateerd met gegevens uit de tweede set met bestanden in blob storage. Er is geen synchronisatie. Als u vervolgens uitvoeren wordt een synchronisatie na het uitschalen, de nieuwe replica's in de groep query gehydrateerd tweemaal - een redundante hydration. Wanneer u de volgende scale-out-bewerking uitvoert, is het belangrijk rekening moet houden:

* Voer een synchronisatie uit *voordat de bewerking scale-out* om te voorkomen dat redundante hydration van de toegevoegde replica's.

* Bij het automatiseren van beide verwerking *en* scale-out-bewerkingen, het is belangrijk dat de gegevens op de primaire server eerst te verwerken en vervolgens een synchronisatie uitvoeren en vervolgens voert u de scale-out-bewerking. Deze volgorde zorgt ervoor dat minimale gevolgen voor de QPU-en geheugenresources.

* Synchronisatie is toegestaan, zelfs wanneer er zich geen replica's in de query-groep. Als u horizontaal van nul tot een of meer replica's met nieuwe gegevens uit een verwerking op de primaire server schalen worden, eerst de synchronisatie wordt uitgevoerd met geen replica's in de query-groep, en vervolgens scale-out. Synchroniseren voordat horizontaal schalen, voorkomt u redundante hydration van de zojuist toegevoegde replica's.

* Bij het verwijderen van een modeldatabase van de primaire server, deze wordt niet automatisch worden verwijderd uit replica's in de query-groep. U moet een synchronisatiebewerking die Hiermee verwijdert u het bestand/s voor die database van de gedeelde blob-opslaglocatie van de replica en verwijdert vervolgens de modeldatabase op de replica's in de groep query uitvoeren.

### <a name="separate-processing-from-query-pool"></a>Afzonderlijk te verwerken van de querypool

Voor maximale prestaties voor verwerking en querybewerkingen, kunt u kiezen voor het scheiden van de verwerkingsserver van de querypool. Wanneer gescheiden, worden bestaande en nieuwe clientverbindingen zijn toegewezen aan de query-replica's in de groep van de query alleen. Als een korte periode alleen bewerkingen voor de verwerking nemen, kunt u voor het scheiden van uw verwerkingsserver van de querypool alleen voor de hoeveelheid tijd die nodig is het uitvoeren van bewerkingen voor verwerking en synchronisatie en neemt u deze terug naar de querypool. 

## <a name="monitor-qpu-usage"></a>QPU-gebruik bewaken

Om te bepalen als scale-out voor de server nodig is, moet u de server in Azure portal controleren met behulp van metrische gegevens. Als uw QPU regelmatig loadniveau, betekent dit dat het aantal query's op uw modellen overschrijdt de limiet QPU voor uw abonnement. De Query pool taak wachtrij lengte metriek verhoogt ook wanneer het aantal query's in de querywachtrij van de thread-pool overschrijdt de beschikbare QPU. 

Een andere goede meetwaarde om te kijken is de gemiddelde QPU door ServerResourceType. Met deze metriek wordt de gemiddelde QPU voor de primaire server met die van de querypool vergeleken. 

### <a name="to-configure-qpu-by-serverresourcetype"></a>QPU door ServerResourceType configureren
1. Klik in een lijndiagram metrische gegevens op **metrische waarde toevoegen**. 
2. In **RESOURCE**, selecteer uw server, klikt u vervolgens **METRIEK NAAMRUIMTE**, selecteer **standaard metrische gegevens van Analysis Services**, klik dan in **metrische gegevens**, Selecteer **QPU**, en klik vervolgens in **aggregatie**, selecteer **Avg**. 
3. Klik op **toepassen splitsen**. 
4. In **waarden**, selecteer **ServerResourceType**.  

Zie [Monitor server metrics](analysis-services-monitor.md) (Metrische servergegevens bewaken) voor meer informatie.

## <a name="configure-scale-out"></a>Uitschalen configureren

### <a name="in-azure-portal"></a>In Azure portal

1. Klik in de portal op **Scale-out**. Gebruik de schuifregelaar om te selecteren van het aantal query-replicaservers. Het aantal replica's die u kiest is naast uw bestaande server.

2. In **scheiden van de verwerkingsserver van de querypool**, selecteer Ja als u wilt uitsluiten van uw verwerkingsserver van queryservers. Client [verbindingen](#connections) met behulp van de standaard-verbindingsreeks (zonder `:rw`) worden omgeleid naar de replica's in de query-groep. 

   ![Schuifregelaar voor scale-out](media/analysis-services-scale-out/aas-scale-out-slider.png)

3. Klik op **opslaan** voor het inrichten van uw nieuwe query-replicaservers. 

Wanneer u scale-out voor een server voor het eerst configureert, worden automatisch modellen op de primaire server gesynchroniseerd met replica's in de query-groep. Automatische synchronisatie treedt alleen op als wanneer u scale-out op een of meer replica's voor het eerst configureert. Toekomstige wijzigingen aan het aantal replica's op dezelfde server *wordt niet geactiveerd voor een andere automatische synchronisatie*. Automatische synchronisatie wordt niet opnieuw uitgevoerd zelfs als u de server instellen op nul replica's en nog een keer scale-out naar een willekeurig aantal replica's. 

## <a name="synchronize"></a>Synchroniseren 

Synchronisatiebewerkingen moeten worden uitgevoerd, handmatig of via de REST-API.

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

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

Voordat u met behulp van PowerShell, [installeren of bijwerken van de meest recente Azure PowerShell-module](/powershell/azure/install-az-ps). 

Gebruiken om uit te voeren synchronisatie, [synchronisatie AzAnalysisServicesInstance](https://docs.microsoft.com/powershell/module/az.analysisservices/sync-AzAnalysisServicesinstance).

Als u wilt dat het aantal query's, gebruikt u [Set AzAnalysisServicesServer](https://docs.microsoft.com/powershell/module/az.analysisservices/set-azanalysisservicesserver). Geef de optionele `-ReadonlyReplicaCount` parameter.

## <a name="connections"></a>Verbindingen

Op de overzichtspagina van uw server zijn er twee servernamen. Als u scale-out voor een server nog niet hebt geconfigureerd, werken de namen van beide op dezelfde manier. Wanneer u scale-out voor een server configureert, moet u de naam van de juiste server, afhankelijk van het verbindingstype opgeven. 

Voor eindgebruikers-clientverbindingen, zoals Power BI Desktop, Excel en aangepaste apps gebruik **servernaam**. 

Voor SSMS, SSDT en verbindingsreeksen in PowerShell, Azure-functie-apps en AMO, gebruik **beheerservernaam**. Naam van de beheerserver bevat een speciaal `:rw` kwalificatie (lezen-schrijven). Alle bewerkingen voor de verwerking optreden op de (primaire) beheerserver.

![Servernamen](media/analysis-services-scale-out/aas-scale-out-name.png)

## <a name="troubleshoot"></a>Problemen oplossen

**Probleem:** Gebruikers krijgen fout **kan geen server vinden '\<naam van de server >' exemplaar in de verbindingsmodus 'Alleen-lezen'.**

**Oplossing:** Bij het selecteren van de **scheiden van de verwerkingsserver van de querypool** optie, client-verbindingen met behulp van de standaard-verbindingsreeks (zonder `:rw`) worden omgeleid naar de pool queryreplica's. Als de replica's in de groep van de query nog niet zijn nog online omdat synchronisatie niet is voltooid, wordt omgeleid clientverbindingen kunnen mislukken. Om te voorkomen dat een mislukte verbindingen, moet er ten minste twee servers in de query-groep bij het uitvoeren van een synchronisatie. Elke server afzonderlijk gesynchroniseerd terwijl anderen online blijven. Als u niet beschikt over de verwerkingsserver in de query van toepassingen tijdens de verwerking, kunt u de te verwijderen uit de groep voor verwerking en nadat de verwerking is voltooid, maar voordat de synchronisatie toevoegen terug naar de pool. Metrische gegevens over geheugen en QPU gebruiken voor het bewaken van de synchronisatiestatus.

## <a name="related-information"></a>Gerelateerde informatie

[Metrische servergegevens bewaken](analysis-services-monitor.md)   
[Azure analyseservices beheren](analysis-services-manage.md) 
