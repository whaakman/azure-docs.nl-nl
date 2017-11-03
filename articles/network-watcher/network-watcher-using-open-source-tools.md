---
title: Patronen in het netwerkverkeer met Azure-netwerk-Watcher en open-source hulpprogramma's visualiseren | Microsoft Docs
description: Deze pagina beschrijft het gebruik van netwerk-Watcher pakketopname met Capanalysis voor het visualiseren van patronen in het netwerkverkeer naar en van uw virtuele machines.
services: network-watcher
documentationcenter: na
author: jimdial
manager: timlt
editor: 
ms.assetid: 936d881b-49f9-4798-8e45-d7185ec9fe89
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/22/2017
ms.author: jdial
ms.openlocfilehash: 61abda6053fe743e294f309df3a6e1041052ec6e
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="visualize-network-traffic-patterns-to-and-from-your-vms-using-open-source-tools"></a>Visualiseren patronen in het netwerkverkeer naar en van uw virtuele machines met open-source hulpprogramma 's

Pakket opnamen bevatten gegevens van het netwerk waarmee u kunt het netwerk forensische en grondige pakketinspecties uitvoeren. Er zijn veel wordt geopend, source hulpprogramma's die u gebruiken kunt voor het analyseren van opnamen van pakket voor het verkrijgen van inzicht in uw netwerk. Een van deze programma is CapAnalysis, een visualisatie pakket open-source hulpprogramma. Pakket vastleggen gegevens te visualiseren is een waardevolle manier voor het afleiden van snel inzicht op patronen en afwijkingen in uw netwerk. Visualisaties bieden ook een manier om dergelijke insights te delen op een manier eenvoudig worden gebruikt.

Azure netwerk-Watcher biedt u de mogelijkheid voor het vastleggen van deze waardevolle gegevens doordat u voor het pakket opnamen uitvoeren op uw netwerk. In dit artikel bieden we een overzicht van het visualiseren en Verkrijg inzicht in het pakket worden vastgelegd met behulp van CapAnalysis met netwerk-Watcher.

## <a name="scenario"></a>Scenario

U hebt een eenvoudige webtoepassing geïmplementeerd op een virtuele machine in Azure wilt gebruiken van open-source hulpprogramma's voor het visualiseren van het netwerkverkeer om snel te identificeren stroom patronen en eventuele mogelijke afwijkingen. U kunt met de netwerk-Watcher verkrijgen van een pakketopname van uw netwerkomgeving en bewaar deze rechtstreeks van uw opslagaccount. CapAnalysis kunnen vervolgens de pakketopname rechtstreeks van de storage-blob voor opnemen en visualiseren van de inhoud ervan.

![scenario][1]

## <a name="steps"></a>Stappen

### <a name="install-capanalysis"></a>CapAnalysis installeren

Als u wilt installeren CapAnalysis op een virtuele machine, kunt u hier de officiële instructies https://www.capanalysis.net/ca/how-to-install-capanalysis verwijzen.
In volgorde extern toegang tot CapAnalysis, moeten we open poort 9877 op de virtuele machine door een nieuwe beveiligingsregel voor binnenkomende toe te voegen. Raadpleeg voor meer informatie over het maken van regels in Netwerkbeveiligingsgroepen [regels maken in een bestaande NSG](../virtual-network/virtual-networks-create-nsg-arm-pportal.md#create-rules-in-an-existing-nsg). Wanneer de regel is toegevoegd, moet u kunnen toegang krijgen tot CapAnalysis van`http://<PublicIP>:9877`

### <a name="use-azure-network-watcher-to-start-a-packet-capture-session"></a>Gebruik Azure netwerk-Watcher pakket vastleggen sessie starten

Netwerk-Watcher kunt u pakketten om bij te houden van verkeer van en naar een virtuele machine vastleggen. U kunt verwijzen naar de instructies op de [beheren pakket worden vastgelegd met de netwerk-Watcher](network-watcher-packet-capture-manage-portal.md) pakket vastleggen sessie starten. Deze pakketopname kan worden opgeslagen in een blob storage toegankelijk door CapAnalysis.

### <a name="upload-a-packet-capture-to-capanalysis"></a>Een pakketopname uploaden naar CapAnalysis
U kunt rechtstreeks uploaden een pakketopname die door de netwerk-watcher via het tabblad 'Importeren van URL' en het geven van een koppeling naar de blob storage waar het vastleggen van het pakket wordt opgeslagen.

Wanneer een koppeling biedt naar CapAnalysis, zorg er dan voor dat een SAS-token toevoegen aan de URL van de opslag-blob.  Hiervoor gaat u naar de gedeelde-toegangshandtekening van het opslagaccount, de toegestane machtigingen aanwijzen en druk op de knop SAS genereren voor het maken van een token. Vervolgens kunt u deze SAS-token toevoegen aan de URL van de pakket vastleggen opslag-blob.

De resulterende URL ziet er ongeveer als volgt: http://storageaccount.blob.core.windows.net/container/location?addSASkeyhere


### <a name="analyzing-packet-captures"></a>Analyseren van pakket worden vastgelegd

CapAnalysis biedt verschillende opties voor het visualiseren van uw pakketopname, elke verstrekken analyse vanuit een ander perspectief. U kunt met deze visual samenvattingen inzicht in uw netwerkverkeer trends en snel een ongewone activiteit te herkennen. Enkele van deze functies worden weergegeven in de volgende lijst:

1. Stroom tabellen

    Deze tabel geeft de lijst met stromen in de pakketgegevens, de tijdstempel die is gekoppeld aan de stromen en de verschillende protocollen die zijn gekoppeld aan de stroom, evenals de bron en doel-IP.

    ![capanalysis stroom pagina][5]

1. Overzicht van Protocol

    Dit deelvenster kunt u snel zien wat de distributie van netwerkverkeer via de verschillende protocollen en -locaties.

    ![overzicht van capanalysis protocol][6]

1. statistieken

    Dit deelvenster kunt u weergeven netwerkverkeer statistics – bytes verzonden en ontvangen van de bron en doel-IP-adressen, stromen voor elk van de bron en doel-IP-adressen,-protocol gebruikt voor verschillende stromen en de duur van stromen.

    ![capanalysis statistieken][7]

1. geomap

    Dit deelvenster bevat een overzichtsweergave van het netwerkverkeer met kleuren schalen van het volume van verkeer vanuit elk land. Gemarkeerde landen extra stroom statistieken zoals het deel van de gegevens worden verzonden en ontvangen van IP-adressen in dat land weergeven, kunt u selecteren.

    ![geomap][8]

1. Filters

    CapAnalysis biedt een set met filters voor snelle analyse van specifieke pakketten. U kunt bijvoorbeeld de gegevens filteren per protocol om specifieke inzicht in deze subset van verkeer te krijgen.

    ![filters][11]

    Ga naar [https://www.capanalysis.net/ca/#about](https://www.capanalysis.net/ca/#about) voor meer informatie over alle CapAnalysis mogelijkheden.

## <a name="conclusion"></a>Conclusie

Netwerk-Watcher pakket vastleggen functie kunt u voor het vastleggen van de gegevens die nodig zijn om netwerk forensische uitvoeren en het netwerkverkeer beter te begrijpen. In dit scenario wordt beschreven hoe pakket schermopnamen van netwerk-Watcher eenvoudig worden geïntegreerd met open-source hulpprogramma's. U kunt met open-source hulpprogramma's zoals CapAnalysis pakketten opnamen visualiseren, grondige pakketinspecties uitvoeren en snel trends te identificeren binnen het netwerkverkeer.

## <a name="next-steps"></a>Volgende stappen

Ga voor meer informatie over het NSG stroom logboeken naar [stroom NSG-Logboeken](network-watcher-nsg-flow-logging-overview.md)

Meer informatie over het visualiseren van uw NSG stroom logboeken met Power BI in via [visualiseren NSG loopt logboeken met Power BI](network-watcher-visualize-nsg-flow-logs-power-bi.md)
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
