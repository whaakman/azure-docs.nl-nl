---
title: Visualizing Azure Netwerkbeveiligingsgroep stroom logboeken met Power BI | Microsoft Docs
description: Deze pagina wordt beschreven hoe NSG-logboeken stroom met Power BI te visualiseren.
services: network-watcher
documentationcenter: na
author: jimdial
manager: timlt
editor: 
ms.assetid: 1e4f95fa-f5f0-4e03-bc25-008fbfc4934c
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/22/2017
ms.author: jdial
ms.openlocfilehash: 19bd7ed4bab915d7918a192a046653666cfaa498
ms.sourcegitcommit: c7215d71e1cdeab731dd923a9b6b6643cee6eb04
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/17/2017
---
# <a name="visualizing-network-security-group-flow-logs-with-power-bi"></a>Netwerkbeveiligingsgroep visualizing stroom logboeken met Power BI

Netwerkbeveiligingsgroep stroom logboeken kunnen u informatie weergeven over inkomende en uitgaande IP-verkeer op Netwerkbeveiligingsgroepen. Deze stromen logboeken weergeven binnenkomend en uitgaand overdrachten op basis van een per regel, de NIC de stroom is van toepassing op 5-tuple informatie met uitleg over de stroom (IP bron/het doel, bron/het doel-poort Protocol) en als het verkeer is toegestaan of geweigerd.

Het kan lastig zijn om inzicht te krijgen in stroom logboekregistratie gegevens door te zoeken in de logboekbestanden handmatig. In dit artikel bieden we een oplossing voor het visualiseren van uw meest recente stroom-logboeken en meer informatie over verkeer op uw netwerk.

## <a name="scenario"></a>Scenario

In het volgende scenario we verbinding maken met Power BI desktop naar het opslagaccount die we voor onze gegevens NSG stromen logboekregistratie hebt geconfigureerd als de sink. Nadat we verbinding met onze storage-account maken, worden de Power BI downloadt en parseert de logboeken om te bieden een visuele representatie van het verkeer dat is vastgelegd door netwerkbeveiligingsgroepen.

Met behulp van de visuele elementen opgegeven in de sjabloon die u kunt controleren:

* Bovenste Talkers
* Tijd stromen reeksgegevens richting en regel beschikking
* Stromen door Network Interface-MAC-adres
* Stromen door het NSG en de regel
* Stromen door doelpoort

De opgegeven sjabloon kan worden bewerkt zodat u kunt wijzigen om de nieuwe gegevens, visuele elementen, toevoegen of bewerken van query's aan uw behoeften.

## <a name="setup"></a>Instellen

Voordat u begint, kunt u Network Security groep stromen-logboekregistratie is ingeschakeld op een of meer Netwerkbeveiligingsgroepen in uw account moet hebben. Voor instructies over het inschakelen van netwerkbeveiliging stromen Logboeken, raadpleegt u het volgende artikel: [Inleiding tot registratie van de stroom voor Netwerkbeveiligingsgroepen](network-watcher-nsg-flow-logging-overview.md).

U moet ook de Power BI Desktop client is geïnstalleerd op uw computer en voldoende vrije ruimte op uw computer te downloaden en laden van de logboekgegevens die in uw opslagaccount voorkomt.

![Visio-diagram][1]

### <a name="steps"></a>Stappen

1. Download en opent u de volgende Power BI-sjabloon in de toepassing van Power BI Desktop [netwerk-Watcher Power BI-stroom logboeken sjabloon](https://aka.ms/networkwatcherpowerbiflowlogstemplate)
1. Geef de vereiste parameters voor Query
    1. **StorageAccountName** – Hiermee geeft u op de naam van het opslagaccount met de NSG stroom-logboeken die u wilt laden en visualiseren.
    1. **NumberOfLogFiles** – Hiermee geeft u het nummer van de logboekbestanden die u wilt downloaden en visualiseren in Power BI. Bijvoorbeeld, als 50 is opgegeven, de meest recente 50 logboekbestanden. FF hebben we 2 nsg's ingeschakeld en geconfigureerd NSG stroom om Logboeken te verzenden naar dit account en vervolgens de afgelopen 25 uur van Logboeken kunnen worden weergegeven.

    ![Power BI main][2]

1. Voer de toegangssleutel voor uw opslagaccount. U kunt geldig toegangstoetsen vinden door te navigeren naar uw opslagaccount in de Azure portal en selecteert **toegangstoetsen** in het menu instellingen. Klik op **Connect** past u wijzigingen toe.

    ![Toegangstoetsen][3]

    ![toegangssleutel 2][4]

4.  Uw logboeken worden gedownload en geparseerd en kunt u nu gebruikmaken van de vooraf gemaakte visuele elementen.

## <a name="understanding-the-visuals"></a>Inzicht in de visuele elementen

Opgegeven in de sjabloon zijn een vaste set van visuele elementen waarmee zinvol zijn het NSG stromen logboekgegevens. De volgende afbeeldingen ziet een voorbeeld van hoe het dashboard eruit wanneer gevuld met gegevens. Hieronder wordt elke visual met meer details onderzoeken 

![Power BI][5]
 
De Top Talkers visual ziet u de IP-adressen die de meeste verbindingen hebt geïnitieerd gedurende de periode is opgegeven. De grootte van de vakken correspondeert met het relatieve aantal verbindingen. 

![toptalkers][6]

De volgende keer reeks grafieken zien hoeveel stromen gedurende de periode. De bovenste grafiek wordt gesegmenteerd op de stroomrichting en de laagste wordt gesegmenteerd op de beslissing (toestaan of weigeren). Met dit visuele element, kunt u onderzoeken van uw verkeer trends na verloop van tijd en eventuele abnormale pieken herkennen of daling van verkeer of verkeer segmentering.

![flowsoverperiod][7]

De volgende diagrammen aangegeven de stromen per netwerkinterface met de bovenste gesegmenteerd op stroomrichting en hoe lager gesegmenteerd op beslissingen. Met deze informatie krijgt u inzicht in welke van uw virtuele machines de meeste ten opzichte van andere gecommuniceerd en als het verkeer naar een specifieke virtuele machine wordt toegestaan of geweigerd.

![flowspernic][8]

De volgende ring wheel-diagram toont een overzicht van de stromen door doelpoort. Met deze informatie kunt u de meest gebruikte doelpoorten gebruikt binnen de opgegeven periode weergeven.

![Ring][9]

De volgende staafdiagram ziet u de stroom door het NSG en de regel. Met deze informatie kunt u het nsg's die verantwoordelijk is voor de meeste verkeer en de uitsplitsing van verkeer op een NSG zien door de regel.

![barchart][10]
 
De volgende informatieve grafieken weergegeven informatie over het nsg's aanwezig zijn in de logboeken, het aantal overdrachten vastgelegd gedurende de periode en de datum van het eerste logboek vastgelegd. Deze informatie hebt u een idee van welke nsg's zijn geregistreerd en het datumbereik van stromen.

![infochart1][11]

![infochart2][12]

Deze sjabloon bevat de volgende slicers zodat u alleen de gegevens die u meest geïnteresseerd bent in weergeven. U kunt filteren op uw resourcegroepen, nsg's en regels. U kunt ook filteren op 5-tuple informatie, besluit en de tijd die is geschreven in het logboek.

![slicers][13]

## <a name="conclusion"></a>Conclusie

In dit scenario beschreven dat via het netwerk beveiliging groep overgebracht logboeken die worden geleverd door Power BI en netwerk-Watcher we kunnen visualiseren en het verkeer te begrijpen. Met de opgegeven sjabloon, Power BI de logboeken rechtstreeks uit storage downloadt en verwerkt deze lokaal. Tijd die nodig is om de sjabloon wordt geladen, is afhankelijk van het aantal bestanden die zijn aangevraagd en de totale grootte van bestanden gedownload.

U kunt deze sjabloon voor uw behoeften passen. Er zijn veel verschillende manieren u Power BI kunt gebruiken met het netwerk groep stromen beveiligingslogboeken. 

## <a name="notes"></a>Opmerkingen

* Logboeken standaard worden opgeslagen in`https://{storageAccountName}.blob.core.windows.net/insights-logs-networksecuritygroupflowevent/`

    * Als andere gegevens in een andere directory bestaat, kan deze query's naar pull-abonnementen en proces de gegevens moeten worden gewijzigd.

* De opgegeven sjabloon wordt niet aanbevolen voor gebruik met meer dan 1 GB van Logboeken.

* Als u een grote hoeveelheid Logboeken hebt, raden wij onderzoeken van een oplossing met behulp van een ander gegevensarchief zoals Data Lake of SQL server.

## <a name="next-steps"></a>Volgende stappen

Meer informatie over het visualiseren van uw NSG-logboeken stroom met de Elastick Stack in via [visualiseren Azure-netwerk-Watcher NSG stroom logboeken met open-source hulpprogramma's](network-watcher-visualize-nsg-flow-logs-open-source-tools.md)

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
