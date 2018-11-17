---
title: Logboeken van de stroom met Power BI visualizing Azure Network Security Group | Microsoft Docs
description: Deze pagina wordt beschreven hoe u NSG-stroomlogboeken met Power BI visualiseren.
services: network-watcher
documentationcenter: na
author: mattreatMSFT
manager: vitinnan
editor: ''
ms.assetid: 1e4f95fa-f5f0-4e03-bc25-008fbfc4934c
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/22/2017
ms.author: mareat
ms.openlocfilehash: 1f8807094c6e93cf55467e76e0fa2b617ec3d5fb
ms.sourcegitcommit: 8899e76afb51f0d507c4f786f28eb46ada060b8d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/16/2018
ms.locfileid: "51822871"
---
# <a name="visualizing-network-security-group-flow-logs-with-power-bi"></a>Stroomlogboeken van Netwerkbeveiligingsgroep visualizing met Power BI

Stroomlogboeken van Netwerkbeveiligingsgroep kunnen u informatie weergeven over inkomende en uitgaande IP-verkeer op Netwerkbeveiligingsgroepen. Deze stroom logboeken weergeven binnenkomende en uitgaande stromen op basis van per regel, de NIC van de stroom is van toepassing op, 5-tuple-informatie over de stroom (bron-/ doel-IP, poort van de bron-/ doel, Protocol), en als het verkeer is toegestaan of geweigerd.

Het kan lastig zijn om inzicht te krijgen in gegevens voor logboekregistratie van stroom door handmatig te zoeken naar de logboekbestanden. In dit artikel krijgt u een oplossing voor uw meest recente stroomlogboeken visualiseren en meer informatie over verkeer in uw netwerk.

> [!Warning]  
> De volgende stappen werken met flow logboeken versie 1. Zie voor meer informatie, [Inleiding tot stroomlogboeken voor netwerkbeveiligingsgroepen](network-watcher-nsg-flow-logging-overview.md). De volgende instructies werkt niet met versie 2 van de logboekbestanden, zonder aanpassingen.

## <a name="scenario"></a>Scenario

In het volgende scenario, we verbinding maken met Power BI desktop naar het opslagaccount dat we voor onze gegevens NSG Flow logboekregistratie hebt geconfigureerd als de sink. Nadat we verbinding met onze storage-account maken, wordt in Power BI wordt gedownload en parseert de logboeken voor een visuele representatie van het verkeer dat is geregistreerd door netwerkbeveiligingsgroepen.

Met behulp van de visuele elementen opgegeven in de sjabloon die u kunt controleren:

* Belangrijke gebruikers
* Time Series-stroomgegevens op richting en op regelbesluit
* Stromen op Network Interface-MAC-adres
* Stromen op Netwerkbeveiligingsgroep en regel
* Stromen op doelpoort

De opgegeven sjabloon kan worden bewerkt zodat u kunt wijzigen als u wilt toevoegen van nieuwe gegevens, visuele elementen, of query's om aan uw behoeften te bewerken.

## <a name="setup"></a>Instellen

Voordat u begint, kunt u Network Security Group Flow-logboekregistratie is ingeschakeld op een of meer Netwerkbeveiligingsgroepen in uw account moet hebben. Voor instructies over het inschakelen van netwerkbeveiliging flow Logboeken, raadpleegt u het volgende artikel: [Inleiding tot stroomlogboeken voor Netwerkbeveiligingsgroepen](network-watcher-nsg-flow-logging-overview.md).

U moet ook de Power BI Desktop-client geïnstalleerd op uw computer en voldoende vrije ruimte op uw computer te downloaden en het laden van de logboekgegevens die deel uitmaakt van uw storage-account hebben.

![Visio-diagram][1]

### <a name="steps"></a>Stappen

1. Downloaden en openen van de volgende Power BI-sjabloon in de Power BI Desktop-toepassing [Power BI netwerk-Watcher-stroomlogboeken sjabloon](https://aka.ms/networkwatcherpowerbiflowlogstemplate)
1. Voer de vereiste parameters voor de Query
    1. **StorageAccountName** – Hiermee geeft u op de naam van het opslagaccount met de NSG-stroomlogboeken die u wilt laden en visualiseren.
    1. **NumberOfLogFiles** – Hiermee geeft u het nummer van de logboekbestanden die u wilt downloaden en visualiseren in Power BI. Bijvoorbeeld, als 50 is opgegeven, de meest recente 50 logboekbestanden. Als we 2 nsg's ingeschakeld en geconfigureerd voor het verzenden van NSG-stroomlogboeken aan dit account hebt, kan de laatste 25 minuten aan logboeken worden weergegeven.

    ![hoofdvenster van Power BI][2]

1. Voer de toegangssleutel voor uw opslagaccount. U kunt geldige toegangssleutels vinden door te navigeren naar uw opslagaccount in Azure portal en vervolgens **toegangssleutels** in het menu instellingen. Klik op **Connect** wijzigingen vervolgens toepassen.

    ![Toegangssleutels][3]

    ![toegang tot de sleutel 2][4]

4.  Uw logboeken worden gedownload en geparseerd en u kunt nu gebruikmaken van de vooraf gemaakte visuele elementen.

## <a name="understanding-the-visuals"></a>Inzicht krijgen in de visuele elementen

Opgegeven in de sjabloon zijn een set van visuele elementen waarmee zinvol in de Flow-logboekgegevens van NSG. De volgende afbeeldingen ziet een voorbeeld van hoe het dashboard uitziet als ingevuld met gegevens. Hieronder bekijken we elk visuele element in meer detail 

![Power BI][5]
 
De bovenste Talkers visual ziet u de IP-adressen die de meeste verbindingen hebt gestart in de periode is opgegeven. De grootte van de vakken komt overeen met het relatieve aantal verbindingen. 

![toptalkers][6]

De volgende tijd reeks grafieken ziet u het aantal stromen in de periode. De bovenste grafiek wordt gesegmenteerd op de stroomrichting en des te lager is gesegmenteerd op de beslissing (toestaan of weigeren). Met het visuele element, kunt u uw verkeer trends na verloop van tijd, onderzoeken en abnormale pieken vertoont herkennen of weigeren in verkeer of verkeer segmentering.

![flowsoverperiod][7]

De volgende grafieken ziet u de stromen per netwerkinterface, met de bovenste gesegmenteerd op stroomrichting en des te lager gesegmenteerd op beslissing heeft genomen. Met deze informatie, krijgt u inzicht in welke van uw VM's de meeste ten opzichte van andere gecommuniceerd, en als er verkeer naar een specifieke virtuele machine wordt toegestaan of geweigerd.

![flowspernic][8]

De volgende wheel ringdiagram bevat een overzicht van de stromen op doelpoort. Met deze informatie vindt u de meest gebruikte doelpoorten gebruikt binnen de opgegeven periode.

![ringdiagram][9]

De volgende staafdiagram ziet u de stroom op Netwerkbeveiligingsgroep en regel. Met deze informatie kunt u de nsg's die verantwoordelijk is voor het meeste verkeer en de verdeling van verkeer op een NSG zien door regel.

![barchart][10]
 
De volgende informatie grafieken weergegeven informatie over de nsg's aanwezig zijn in de logboeken, het aantal stromen die zijn vastgelegd over de periode en de datum van het eerste logboek vastgelegd. Deze informatie geeft u een beter beeld van welke nsg's zijn, worden geregistreerd en het datumbereik van stromen.

![infochart1][11]

![infochart2][12]

Deze sjabloon bevat de volgende slicers zodat u tot alleen de gegevens die u het meest geïnteresseerd zijn. U kunt filteren op uw resourcegroepen, nsg's en regels. U kunt ook filteren op 5-tuple-informatie, besluit en de tijd die het logboek is geschreven.

![slicers][13]

## <a name="conclusion"></a>Conclusie

We hebben laten zien in dit scenario dat met behulp van Network Security Group Flow-logboeken die zijn opgegeven door de Network Watcher en Power BI, kan worden te visualiseren en inzicht in het verkeer. Met de opgegeven sjabloon, Power BI de logboeken rechtstreeks uit storage downloadt en lokaal worden verwerkt. Gebruikte tijd voor het laden van de sjabloon is afhankelijk van het aantal bestanden die zijn aangevraagd en de totale grootte van de bestanden gedownload.

U kunt deze sjabloon voor uw behoeften aanpassen. Er zijn veel verschillende manieren dat u Power BI met Flow logboeken van Netwerkbeveiligingsgroepen gebruiken kunt. 

## <a name="notes"></a>Opmerkingen

* Logboeken standaard worden opgeslagen in `https://{storageAccountName}.blob.core.windows.net/insights-logs-networksecuritygroupflowevent/`

    * Als andere gegevens in een andere directory bestaat moeten ze de query's naar de pull-abonnementen en proces de gegevens worden gewijzigd.

* De opgegeven sjabloon wordt niet aanbevolen voor gebruik met meer dan 1 GB aan logboeken.

* Als u een grote hoeveelheid Logboeken hebt, wordt u aangeraden dat u een oplossing met behulp van een ander gegevensarchief zoals Data Lake of SQL server onderzoeken.

## <a name="next-steps"></a>Volgende stappen

Leer hoe u uw NSG-stroomlogboeken visualiseren met de Elastick Stack recentst [visualiseren Azure Network Watcher NSG-stroomlogboeken met behulp van open-sourcehulpprogramma's](network-watcher-visualize-nsg-flow-logs-open-source-tools.md)

[1]: ./media/network-watcher-visualize-nsg-flow-logs-power-bi/figure1.png
[2]: ./media/network-watcher-visualize-nsg-flow-logs-power-bi/figure2.png
[3]: ./media/network-watcher-visualize-nsg-flow-logs-power-bi/figure3.png
[4]: ./media/network-watcher-visualize-nsg-flow-logs-power-bi/figure4.png
[5]: ./media/network-watcher-visualize-nsg-flow-logs-power-bi/figure5.png
[6]: ./media/network-watcher-visualize-nsg-flow-logs-power-bi/figure6.png
[7]: ./media/network-watcher-visualize-nsg-flow-logs-power-bi/figure7.png
[8]: ./media/network-watcher-visualize-nsg-flow-logs-power-bi/figure8.png
[9]: ./media/network-watcher-visualize-nsg-flow-logs-power-bi/figure9.png
[10]: ./media/network-watcher-visualize-nsg-flow-logs-power-bi/figure10.png
[11]: ./media/network-watcher-visualize-nsg-flow-logs-power-bi/figure11.png
[12]: ./media/network-watcher-visualize-nsg-flow-logs-power-bi/figure12.png
[13]: ./media/network-watcher-visualize-nsg-flow-logs-power-bi/figure13.png
