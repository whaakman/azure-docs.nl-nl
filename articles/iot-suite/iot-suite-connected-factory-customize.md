---
title: De oplossing verbonden Factory - Azure aanpassen | Microsoft Docs
description: Een beschrijving van hoe u het gedrag van de Factory verbonden oplossingsverbetering aanpassen.
services: iot-suite
suite: iot-suite
documentationcenter: ''
author: dominicbetts
manager: timlt
editor: ''
ms.assetid: ''
ms.service: iot-suite
ms.devlang: c#
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 12/14/2017
ms.author: dobett
ms.openlocfilehash: 5d074a5cf0dd5191b5d94531068341ad1b953391
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/07/2018
---
# <a name="customize-how-the-connected-factory-solution-displays-data-from-your-opc-ua-servers"></a>Aanpassen hoe de gegevens van uw OPC UA-servers worden weergegeven in de verbonden Factory-oplossing

De oplossing verbonden Factory samenvoegt en gegevens uit de OPC UA-servers die is verbonden met de oplossing worden weergegeven. U kunt bladeren en opdrachten verzenden naar de OPC UA-servers in uw oplossing. Zie voor meer informatie over OPC UA de [verbonden Factory Veelgestelde vragen over het](iot-suite-faq-cf.md).

Voorbeelden van geaggregeerde gegevens in de oplossing zijn de algehele apparatuur efficiëntie (OEE) en de Key Performance Indicators (KPI's) die u in het dashboard op de factory-, lijn- en station niveaus bekijken kunt. De volgende schermafbeelding ziet u de waarden OEE en KPI's voor de **Assembly** station op **productie-regel 1**, in de **München** factory:

![Voorbeeld van OEE en KPI waarden in de oplossing][img-oee-kpi]

De oplossing kunt u gedetailleerde informatie weergeven vanaf specifieke gegevensitems uit de OPC UA-servers, zogenaamde *stations*. De volgende schermafbeelding ziet curve van het aantal geproduceerde items van een specifiek station:

![Curve van het aantal geproduceerde artikelen][img-manufactured-items]

Als u klikt op een van de grafieken, vindt u de gegevens verder met Time Series Insights (TSI):

![Gegevens met behulp van de Time Series Insights verkennen][img-tsi]

Dit artikel wordt beschreven:

- Hoe de gegevens beschikbaar worden gesteld aan de verschillende weergaven in de oplossing.
- Hoe u de manier waarop de oplossing kunt aanpassen, worden de gegevens weergegeven.

## <a name="data-sources"></a>Gegevensbronnen

De verbonden Factory-oplossing worden gegevens uit de OPC UA-servers die is verbonden met de oplossing. De standaardinstallatie bevat verschillende OPC UA-servers met een factory simulatie. U kunt uw eigen servers OPC UA toevoegen die [verbinding maken via een gateway] [ lnk-connect-cf] aan uw oplossing.

U kunt de gegevensitems die een verbonden OPC UA-server om uw oplossing in het dashboard te verzenden kunt bladeren:

1. Kies **Browser** om te navigeren naar de **selecteert u een server OPC UA** weergeven:

    ![Navigeer naar het selecteren van een weergave OPC UA-server][img-select-server]

1. Selecteer een server en klik op **Connect**. Klik op **doorgaan** wanneer de beveiligingswaarschuwing wordt weergegeven.

    > [!NOTE]
    > Deze waarschuwing alleen verschijnt eenmaal voor elke server en er wordt een vertrouwensrelatie tussen het dashboard van de oplossing en de server.

1. U kunt nu de gegevensitems die de server naar de oplossing verzenden kan bladeren. Items die worden verzonden naar de oplossing zijn ingeschakeld:

    ![Gepubliceerde items][img-published]

1. Als u een *beheerder* in de oplossing kunt u kiezen voor het publiceren van een gegevensitem zodat deze beschikbaar zijn in de verbonden Factory-oplossing. U kunt ook de waarde van gegevensitems wijzigen en methoden aanroepen op de OPC UA-server als beheerder.

## <a name="map-the-data"></a>De gegevens moeten worden toegewezen

De oplossing verbonden Factory toegewezen en de gepubliceerde-gegevensitems van de OPC UA-server samenvoegt met de verschillende weergaven in de oplossing. De Factory verbonden oplossing implementeert in uw Azure-account wanneer u de oplossing inricht. Een JSON-bestand in de fabriek verbonden Visual Studio-oplossing slaat deze informatie over de Identiteitstoewijzing. U kunt weergeven en wijzigen van dit JSON-configuratiebestand in de verbonden Factory Visual Studio-oplossing. U kunt de oplossing opnieuw implementeren nadat u een wijziging aanbrengt.

U kunt het configuratiebestand te gebruiken:

- Bewerk de bestaande gesimuleerde fabrieken, de regels voor productie en de stations.
- Gegevens van echte OPC UA-servers waarop u verbinding met de oplossing maken toewijzen.

Zie voor meer informatie over toewijzing en samenvoeging van de gegevens om te voldoen aan uw specifieke vereisten [het configureren van de Factory verbonden oplossingsverbetering ](iot-suite-connected-factory-configure.md).

## <a name="deploy-the-changes"></a>De wijzigingen te implementeren

Wanneer u klaar bent met het aanbrengen van wijzigingen naar de **ContosoTopologyDescription.json** -bestand, moet u de oplossing verbonden Factory opnieuw implementeren bij uw Azure-account.

De **azure-iot-verbonden-factory** opslagplaats bevat een **build.ps1** PowerShell-script die u gebruiken kunt om te bouwen en implementeren van de oplossing.

## <a name="next-steps"></a>Volgende stappen

Meer informatie over de oplossingsverbetering Factory verbonden door te lezen van de volgende artikelen:

* [Verbonden accelerator overzicht van de Factory-oplossing][lnk-rm-walkthrough]
* [Implementeer een gateway voor Factory verbonden][lnk-connect-cf]
* [Machtigingen op de site azureiotsuite.com][lnk-permissions]
* [Verbonden Factory Veelgestelde vragen](iot-suite-faq-cf.md)
* [FAQ][lnk-faq]


[img-oee-kpi]: ./media/iot-suite-connected-factory-customize/oeenadkpi.png
[img-manufactured-items]: ./media/iot-suite-connected-factory-customize/manufactured.png
[img-tsi]: ./media/iot-suite-connected-factory-customize/tsi.png
[img-select-server]: ./media/iot-suite-connected-factory-customize/selectserver.png
[img-published]: ./media/iot-suite-connected-factory-customize/published.png
[img-munich]: ./media/iot-suite-connected-factory-customize/munich.png
[img-server-uris]: ./media/iot-suite-connected-factory-customize/serveruris.png
[lnk-kpi]: ./media/iot-suite-connected-factory-customize/kpidisplay.png

[lnk-rm-walkthrough]: iot-suite-connected-factory-sample-walkthrough.md
[lnk-connect-cf]: iot-suite-connected-factory-gateway-deployment.md
[lnk-permissions]: iot-suite-v1-permissions.md
[lnk-faq]: iot-suite-v1-faq.md