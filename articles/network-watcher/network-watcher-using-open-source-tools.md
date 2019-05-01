---
title: Netwerkverkeerspatronen met Azure Network Watcher en open-sourcehulpprogramma's visualiseren | Microsoft Docs
description: Deze pagina wordt beschreven hoe u van Network Watcher packet-capture met Capanalysis voor het visualiseren van patronen in het netwerkverkeer naar en van uw virtuele machines.
services: network-watcher
documentationcenter: na
author: KumudD
manager: twooley
editor: ''
ms.assetid: 936d881b-49f9-4798-8e45-d7185ec9fe89
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/22/2017
ms.author: kumud
ms.openlocfilehash: 3a0ae782d3fe97752ca8b9e786c3c2672f554277
ms.sourcegitcommit: c53a800d6c2e5baad800c1247dce94bdbf2ad324
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/30/2019
ms.locfileid: "64936012"
---
# <a name="visualize-network-traffic-patterns-to-and-from-your-vms-using-open-source-tools"></a>Netwerkverkeerspatronen naar en van uw virtuele machines met behulp van open-source hulpprogramma's visualiseren

Pakketopnamen bevatten gegevens van het netwerk waarmee u kunt netwerk forensisch onderzoek en grondige inspecties van pakketten. Er zijn veel open source-hulpprogramma's die u gebruiken kunt voor het analyseren van pakketopnamen om inzicht te verkrijgen over uw netwerk. Een van deze programma is CapAnalysis, een hulpprogramma voor gegevensvisualisatie vastleggen open-source-pakket. Pakket vastleggen van gegevens te visualiseren is een waardevolle manier voor het afleiden van snel inzicht te krijgen in patronen en afwijkingen in uw netwerk. Visualisaties bieden ook een mogelijkheid van dergelijke inzichten delen op een manier die eenvoudig worden gebruikt.

Van Azure Network Watcher biedt u de mogelijkheid om vast te leggen met de mogelijkheid om uit te voeren van pakketopnamen in uw netwerk. In dit artikel biedt een Walkthrough van het visualiseren en inzichten verkrijgen van het pakket wordt vastgelegd met behulp van CapAnalysis met Network Watcher.

## <a name="scenario"></a>Scenario

U hebt een eenvoudige web-App geïmplementeerd op een virtuele machine in Azure wilt gebruiken van open-source hulpprogramma's voor het visualiseren van het netwerkverkeer voor patronen van verkeersstromen en eventuele mogelijke afwijkingen snel kunt identificeren. U kunt met Network Watcher, een pakketopname van uw netwerkomgeving verkrijgen en bewaar deze rechtstreeks in uw storage-account. CapAnalysis kunt vervolgens de pakketopname rechtstreeks vanuit de opslag-blob opnemen en visualiseren van de inhoud ervan.

![scenario][1]

## <a name="steps"></a>Stappen

### <a name="install-capanalysis"></a>CapAnalysis installeren

Als u wilt installeren CapAnalysis op een virtuele machine, kunt u verwijzen naar de officiële instructies hier https://www.capanalysis.net/ca/how-to-install-capanalysis.
De toegangsstatus CapAnalysis op afstand, moet u poort 9877 op de virtuele machine door toe te voegen een nieuwe inkomende beveiligingsregel te openen. Raadpleeg voor meer informatie over het maken van regels in Netwerkbeveiligingsgroepen, [-regels maken in een bestaande NSG](../virtual-network/manage-network-security-group.md#create-a-security-rule). Nadat de regel is toegevoegd, moet u toegang hebben tot CapAnalysis uit `http://<PublicIP>:9877`

### <a name="use-azure-network-watcher-to-start-a-packet-capture-session"></a>Gebruik Azure Network Watcher packet capture sessie starten

Network Watcher kunt u voor het vastleggen van pakketten voor het volgen van verkeer voor een virtuele machine. U kunt verwijzen naar de instructies op de [beheren pakket worden vastgelegd met Network Watcher](network-watcher-packet-capture-manage-portal.md) een packet capture-sessie te starten. Een pakketopname kan worden opgeslagen in een opslag-blob worden geopend door CapAnalysis.

### <a name="upload-a-packet-capture-to-capanalysis"></a>Een pakketopname uploaden naar CapAnalysis
Een pakketopname genomen door de netwerk-watcher gebruik het tabblad 'Importeren vanaf URL' en het geven van een koppeling naar de blob storage waar de pakketopname is opgeslagen, kunt u rechtstreeks uploaden.

Als u een koppeling naar CapAnalysis, zorg ervoor dat een SAS-token toevoegen aan de URL van de opslag-blob.  Om dit te doen, gaat u naar de gedeelde-toegangshandtekening van het opslagaccount, de toegestane machtigingen aanwijzen en druk op de knop SAS genereren om een token te maken. U kunt vervolgens het SAS-token toevoegen aan de URL van de packet capture opslag-blob.

De resulterende URL ziet er ongeveer als de volgende URL: http://storageaccount.blob.core.windows.net/container/location?addSASkeyhere


### <a name="analyzing-packet-captures"></a>Analyseren van pakket wordt vastgelegd

CapAnalysis biedt verschillende opties voor het visualiseren van uw pakketopname, elke bieden analyse vanuit een ander perspectief. Met deze visuele samenvattingen, kunt u inzicht in uw netwerkverkeer trends en snel herkent geen ongebruikelijke activiteiten. Enkele van deze functies worden weergegeven in de volgende lijst:

1. Stroom tabellen

    Deze tabel geeft de lijst met stromen in de pakketgegevens, de tijdstempel die is gekoppeld aan de stromen en de verschillende protocollen die zijn gekoppeld aan de stroom, evenals de bron en doel-IP.

    ![capanalysis stroom pagina][5]

1. Overzicht van-Protocol

    Dit deelvenster kunt u snel de distributie van het netwerkverkeer zien over de verschillende protocollen en de verschillende geografische gebieden.

    ![overzicht van capanalysis-protocol][6]

1. statistieken

    Dit deelvenster kunt u weergeven van verkeer netwerkstatistieken – bytes verzonden en ontvangen van de bron- en IP-adressen, stromen voor elk van de bron en doel-IP-adressen,-protocol gebruikt voor verschillende stromen en de duur van stromen.

    ![capanalysis statistieken][7]

1. Geomap

    Dit deelvenster geeft u een weergave van uw netwerkverkeer met kleuren aan het volume van netwerkverkeer van elk land/regio te schalen. U kunt geselecteerde landen/regio's om weer te geven aanvullende stroom statistische gegevens zoals het deel van de gegevens verzonden en ontvangen van IP-adressen in dat land/regio selecteren.

    ![geomap][8]

1. Filters

    CapAnalysis biedt een set met filters voor een snelle analyse van specifieke pakketten. U kunt bijvoorbeeld de gegevens filteren door-protocol voor specifieke inzicht op deze subset van verkeer.

    ![filters][11]

    Ga naar [ https://www.capanalysis.net/ca/#about ](https://www.capanalysis.net/ca/#about) voor meer informatie over alle CapAnalysis mogelijkheden.

## <a name="conclusion"></a>Conclusie

Functie van Network Watcher packet-capture kunt u de gegevens om netwerk forensische gegevens en meer informatie over het netwerkverkeer vast te leggen. In dit scenario, we hebben het laten zien hoe pakketopnamen van Network Watcher kunnen eenvoudig worden geïntegreerd met visualisatie van de open-source hulpprogramma's. U kunt met behulp van open-source hulpprogramma's zoals CapAnalysis voor het visualiseren van pakketten opnamen, grondige pakketinspecties uitvoeren en Identificeer snel trends in uw netwerkverkeer.

## <a name="next-steps"></a>Volgende stappen

Voor meer informatie over NSG-stroomlogboeken, gaat u naar [NSG-stroomlogboeken](network-watcher-nsg-flow-logging-overview.md)

Meer informatie over het visualiseren van uw NSG-stroomlogboeken met Power BI recentst [visualiseren NSG-stromen logboeken met Power BI](network-watcher-visualize-nsg-flow-logs-power-bi.md)
<!--Image references-->

[1]: ./media/network-watcher-using-open-source-tools/figure1.png
[2]: ./media/network-watcher-using-open-source-tools/figure2.png
[3]: ./media/network-watcher-using-open-source-tools/figure3.png
[4]: ./media/network-watcher-using-open-source-tools/figure4.png
[5]: ./media/network-watcher-using-open-source-tools/figure5.png
[6]: ./media/network-watcher-using-open-source-tools/figure6.png
[7]: ./media/network-watcher-using-open-source-tools/figure7.png
[8]: ./media/network-watcher-using-open-source-tools/figure8.png
[9]: ./media/network-watcher-using-open-source-tools/figure9.png
[10]: ./media/network-watcher-using-open-source-tools/figure10.png
[11]: ./media/network-watcher-using-open-source-tools/figure11.png
