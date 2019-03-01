---
title: Aanpassen van de oplossing voor verbonden Factory - Azure | Microsoft Docs
description: Een beschrijving van hoe u het gedrag van de oplossingsversneller voor verbonden Factory aanpassen.
author: dominicbetts
manager: timlt
ms.service: iot-accelerators
services: iot-accelerators
ms.devlang: csharp
ms.topic: conceptual
ms.date: 12/14/2017
ms.author: dobett
ms.openlocfilehash: 7d4e95d066e191e1d5b6d083ede65843dbe73f31
ms.sourcegitcommit: 15e9613e9e32288e174241efdb365fa0b12ec2ac
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/28/2019
ms.locfileid: "57010069"
---
# <a name="customize-how-the-connected-factory-solution-displays-data-from-your-opc-ua-servers"></a>Hoe de gegevens van uw OPC UA-servers worden weergegeven in de oplossing voor verbonden Factory aanpassen

De oplossing voor verbonden Factory verzamelt en gegevens van de OPC UA-servers die zijn verbonden met de oplossing worden weergegeven. U kunt bladeren en opdrachten worden verzonden naar de OPC UA-servers in uw oplossing. Zie de [veelgestelde vragen over verbonden factory's](iot-accelerators-faq-cf.md) voor meer informatie over OPC UA.

Voorbeelden van cumulatieve gegevens in de oplossing zijn de totale Equipment Efficiency (OEE) en Key Performance Indicators (KPI's) die u in het dashboard op de factory, de regel en het station niveaus bekijken kunt. De volgende schermafbeelding ziet u de OEE en KPI-waarden voor de **Assembly** station op **productielijn 1**, in de **München** factory:

![Voorbeeld van de OEE en KPI-waarden in de oplossing][img-oee-kpi]

De oplossing kunt u gedetailleerde informatie weergeven van specifieke items uit de OPC UA-servers met de naam *stations*. De volgende schermafbeelding ziet u grafieken van het aantal geproduceerde items uit een specifiek station:

![Grafieken van het aantal geproduceerde artikelen][img-manufactured-items]

Als u op een of meer grafieken, kunt u de gegevens verder met Time Series Insights (TSI) verkennen:

![Gegevens met behulp van Time Series Insights verkennen][img-tsi]

Dit artikel wordt beschreven:

- Hoe de gegevens beschikbaar worden gesteld aan de verschillende weergaven in de oplossing.
- Hoe u de manier waarop de oplossing kunt aanpassen, worden de gegevens weergegeven.

## <a name="data-sources"></a>Gegevensbronnen

De oplossing voor verbonden Factory worden gegevens uit de OPC UA-servers die zijn verbonden met de oplossing. De standaard-installatie omvat verschillende OPC UA-servers waarop een factory simulatie wordt uitgevoerd. U kunt uw eigen OPC UA-servers toevoegen die [verbinding maken via een gateway] [ lnk-connect-cf] aan uw oplossing.

De gegevensitems die een verbonden OPC UA-server naar uw oplossing in het dashboard verzenden kunt, kunt u bladeren:

1. Kies **Browser** om te navigeren naar de **selecteert een OPC UA-server** weergeven:

    ![Navigeer naar de selecteert een OPC UA-server weergeven][img-select-server]

1. Selecteer een server en klikt u op **Connect**. Klik op **doorgaan** wanneer de beveiligingswaarschuwing wordt weergegeven.

    > [!NOTE]
    > Deze waarschuwing wordt eenmaal weergegeven voor elke server alleen en brengt een vertrouwensrelatie tussen het oplossingsdashboard en de server.

1. U kunt nu de gegevensitems die aan de oplossing kan de server verzenden bladeren. Items die worden verzonden naar de oplossing is ingeschakeld:

    ![Gepubliceerde items][img-published]

1. Als u een *beheerder* in de oplossing, kunt u kiezen voor het publiceren van een gegevensitem zodat deze beschikbaar zijn in de oplossing Connected Factory. Als beheerder, kunt u ook de waarde van gegevensitems wijzigen en methoden aanroepen in de OPC UA-server.

## <a name="map-the-data"></a>Gegevenstoewijzing

De oplossing voor verbonden Factory-kaarten en de gepubliceerde-gegevensitems van de OPC UA-server naar de verschillende weergaven in de oplossing combineert. De oplossing voor verbonden Factory's implementeert naar uw Azure-account wanneer u de oplossing inricht. Een JSON-bestand in de Visual Studio verbonden Factory-oplossing slaat deze toewijzingsinformatie. U kunt weergeven en wijzigen van dit JSON-configuratiebestand in de verbonden Factory Visual Studio-oplossing. Nadat u een wijziging aanbrengt, kunt u de oplossing opnieuw implementeren.

U kunt het configuratiebestand te gebruiken:

- Bewerk de bestaande gesimuleerde factory's, de productielijnen en stations.
- Gegevens uit de echte OPC UA-servers die u verbinding met de oplossing maken worden toegewezen.

Zie voor meer informatie over de toewijzing en samenvoeging van de gegevens om te voldoen aan uw specifieke vereisten, [de oplossingsverbetering voor verbonden Factory configureren ](iot-accelerators-connected-factory-configure.md).

## <a name="deploy-the-changes"></a>De wijzigingen te implementeren

Wanneer u klaar bent met aanbrengen van wijzigingen aan de **ContosoTopologyDescription.json** -bestand, moet u de oplossing voor verbonden Factory opnieuw implementeren naar uw Azure-account.

De **azure-iot-connected-factory** opslagplaats bevat een **build.ps1** PowerShell-script die u gebruiken kunt om te bouwen en implementeren van de oplossing.

## <a name="next-steps"></a>Volgende stappen

Meer informatie over de oplossingsverbetering voor verbonden Factory door het lezen van de volgende artikelen:

* [Kennismaken met de oplossingsversneller Verbonden factory][lnk-rm-walkthrough]
* [Een gateway implementeren voor verbonden Factory][lnk-connect-cf]
* [Machtigingen op de site azureiotsolutions.com][lnk-permissions]
* [Veelgestelde vragen over Verbonden Factory](iot-accelerators-faq-cf.md)
* [FAQ][lnk-faq]


[img-oee-kpi]: ./media/iot-accelerators-connected-factory-customize/oeenadkpi.png
[img-manufactured-items]: ./media/iot-accelerators-connected-factory-customize/manufactured.png
[img-tsi]: ./media/iot-accelerators-connected-factory-customize/tsi.png
[img-select-server]: ./media/iot-accelerators-connected-factory-customize/selectserver.png
[img-published]: ./media/iot-accelerators-connected-factory-customize/published.png


[lnk-rm-walkthrough]:iot-accelerators-connected-factory-sample-walkthrough.md
[lnk-connect-cf]:iot-accelerators-connected-factory-gateway-deployment.md
[lnk-permissions]: iot-accelerators-permissions.md
[lnk-faq]: iot-accelerators-faq.md