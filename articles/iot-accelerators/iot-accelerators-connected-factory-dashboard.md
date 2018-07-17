---
title: Gebruik het dashboard van de verbonden Factory - Azure | Microsoft Docs
description: Over het gebruik van functies van de verbonden Factory-dashboard.
author: dominicbetts
manager: timlt
ms.service: iot-accelerators
services: iot-accelerators
ms.topic: conceptual
ms.date: 07/10/2018
ms.author: dobett
ms.openlocfilehash: 4af93b5667181941585cc1ac89f5696379962026
ms.sourcegitcommit: 0b05bdeb22a06c91823bd1933ac65b2e0c2d6553
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/17/2018
ms.locfileid: "39076036"
---
# <a name="use-features-in-the-connected-factory-solution-accelerator-dashboard"></a>Gebruik de functies in het dashboard van Connected Factory solution accelerator

De [implementeren van een cloud-gebaseerde oplossing voor het beheren van mijn industriële IoT-apparaten](quickstart-connected-factory-deploy.md) Quick Start hebt u geleerd hoe u het dashboard te navigeren en reageren op waarschuwingen. In deze gebruiksaanwijzing ziet u enkele aanvullende dashboardfuncties die u gebruiken kunt om te controleren en beheren van uw industriële IoT-apparaten.

## <a name="apply-filters"></a>Filters toepassen

U kunt de informatie die wordt weergegeven op het dashboard ofwel filteren in de **Factorylijst** deelvenster of de **waarschuwingen** deelvenster:

1. Klik op de **trechter** om een lijst met beschikbare filters weer te geven in het paneel met factorylocaties of het paneel Waarschuwingen.

1. Het deelvenster filters wordt weergegeven:

    [![Verbonden Factory-oplossing accelerator filters](./media/iot-accelerators-connected-factory-dashboard/filterpanel-inline.png)](./media/iot-accelerators-connected-factory-dashboard/filterpanel-expanded.png#lightbox)

1. Kies het filter dat u nodig hebt en op **toepassen**. Het is ook vrije tekst in de filtervelden typen.

1. Het filter wordt vervolgens toegepast. De extra trechterpictogram geeft aan dat een filter wordt toegepast:

    [![Verbonden Factory-oplossing accelerator-filter toegepast](./media/iot-accelerators-connected-factory-dashboard/filterapplied-inline.png)](./media/iot-accelerators-connected-factory-dashboard/filterapplied-expanded.png#lightbox)

    > [!NOTE]
    > Een actief filter heeft geen invloed op de weergegeven OEE en KPI-waarden worden alleen de inhoud van de lijst gefilterd.

1. Als u wilt een filter wissen, klikt u op de trechter en klikt u op **wissen** in het filterdeelvenster.

## <a name="browse-an-opc-ua-server"></a>Door een OPC UA-server bladeren

Wanneer u de oplossingsversneller implementeert, voorziet u automatisch een set gesimuleerde OPC UA-servers die u vanuit het dashboard kunt bladeren. Gesimuleerde servers maken het gemakkelijk om te experimenteren met de oplossingsversnellers zonder de noodzaak voor het implementeren echte servers. Als u een echte OPC UA-server koppelen aan de oplossing wilt, raadpleegt u de [uw OPC UA-apparaat koppelen aan de oplossingsverbetering voor verbonden Factory](iot-accelerators-connected-factory-gateway-deployment.md) zelfstudie.

1. Klik op de **browserpictogram** in de navigatiebalk dashboard:

    [![Verbonden Factory-oplossing accelerator server-browser](./media/iot-accelerators-connected-factory-dashboard/browser-inline.png)](./media/iot-accelerators-connected-factory-dashboard/browser-expanded.png#lightbox)

1. Kies een van de servers in de lijst waarin de servers die u in de solution accelerator geïmplementeerd:

    [![Verbonden Factory-oplossing accelerator-serverlijst](./media/iot-accelerators-connected-factory-dashboard/serverlist-inline.png)](./media/iot-accelerators-connected-factory-dashboard/serverlist-expanded.png#lightbox)

1. Klik op **Verbinden**. Er wordt een beveiligingsdialoogvenster weergegeven. De simulatie is het veilig om op **doorgaan**.

1. Klik op een van de knooppunten in de serverstructuur om deze uit te vouwen. Knooppunten die telemetrie publiceren hebben een vinkje naast deze:

    [![Verbonden Factory-oplossing accelerator-serverstructuur](./media/iot-accelerators-connected-factory-dashboard/servertree-inline.png)](./media/iot-accelerators-connected-factory-dashboard/servertree-expanded.png#lightbox)

1. Klik met de rechtermuisknop op een item om het knooppunt te lezen, te schrijven, te publiceren of aan te roepen. Welke acties er beschikbaar zijn, is afhankelijk van uw machtigingen en de kenmerken van het knooppunt. Met de optie voor lezen wordt een contextpaneel weergegeven met de waarde van dat knooppunt. Met de optie voor schrijven wordt een contextpaneel weergegeven waarin u een nieuwe waarde kunt invoeren. Met de optie voor aanroepen wordt een knooppunt weergegeven waarin u de parameters voor het aanroepen kunt invoeren.

## <a name="publish-a-node"></a>Een knooppunt publiceren

Wanneer u door een *gesimuleerde OPC UA-server* bladert, kunt u ook nieuwe knooppunten publiceren. Vervolgens kunt u de telemetrie van deze knooppunten in de oplossing analyseren. Deze *gesimuleerde OPC UA-servers* gemakkelijk om te experimenteren met de oplossingsversnellers zonder echte apparaten te implementeren:

1. Blader naar een knooppunt in de browserstructuur van de OPC UA-server die u wilt publiceren.

1. Klik met de rechtermuisknop op het knooppunt. Klik op **publiceren**:

    [![Oplossingsverbetering voor verbonden Factory publiceert knooppunt](./media/iot-accelerators-connected-factory-dashboard/publishnode-inline.png)](./media/iot-accelerators-connected-factory-dashboard/publishnode-expanded.png#lightbox)

1. Er word een contextpaneel weergegeven waarin staat dat het publiceren is voltooid. Het knooppunt wordt weergegeven in de weergave op stationsniveau met een vinkje naast het:

    [![Succesvolle publicatie van oplossingsverbetering voor verbonden Factory](./media/iot-accelerators-connected-factory-dashboard/publishsuccess-inline.png)](./media/iot-accelerators-connected-factory-dashboard/publishsuccess-expanded.png#lightbox)

## <a name="command-and-control"></a>Opdracht en controle

Met Verbonden factory kunt u uw industriële apparaten rechtstreeks vanuit de cloud beheren en bedienen. U kunt deze functie gebruiken om te reageren op waarschuwingen die door het apparaat worden gegenereerd. Bijvoorbeeld, kan u een opdracht tot het apparaat opent u een overdrukventiel verzenden. U vindt de beschikbare opdrachten in het knooppunt **StationCommands** in de browserstructuur van de OPC UA-servers. In dit scenario opent u een overdrukventiel op de verzamelplaats van een productielijn in München. De opdracht en controle-functionaliteit wilt gebruiken, moet u zich in de **beheerder** rol voor de implementatie van de oplossing accelerator:

1. Blader naar de **StationCommands** knooppunt in de browserstructuur van de OPC UA-server voor de München, de productielijn 0, de verzamelplaats.

1. Kies de opdracht die u wilt gebruiken. Met de rechtermuisknop op de **OpenPressureReleaseValve** knooppunt. Klik op **aanroepen**:

    [![Verbonden Factory-oplossing accelerator-aanroepopdracht](./media/iot-accelerators-connected-factory-dashboard/callcommand-inline.png)](./media/iot-accelerators-connected-factory-dashboard/callcommand-expanded.png#lightbox)

1. Er wordt een contextpaneel weergegeven waarin wordt gemeld welke methode u aanroepen en de details van de parameter wilt. Klik op **aanroepen**:

    [![Verbonden Factory-oplossing accelerator call-parameters](./media/iot-accelerators-connected-factory-dashboard/callpanel-inline.png)](./media/iot-accelerators-connected-factory-dashboard/callpanel-expanded.png#lightbox)

1. Het contextpaneel wordt bijgewerkt met de melding dat het aanroepen van de methode is voltooid. U kunt controleren of de oproep is voltooid door de waarde van het drukknooppunt te lezen, die na de aanroep is bijgewerkt.

    [![Verbonden Factory-oplossing accelerator aanroepen voltooid](./media/iot-accelerators-connected-factory-dashboard/callsuccess-inline.png)](./media/iot-accelerators-connected-factory-dashboard/callsuccess-expanded.png#lightbox)

## <a name="behind-the-scenes"></a>Achter de schermen

Wanneer u een oplossingsversneller implementeert, maakt het implementatieproces meerdere resources in het door u geselecteerde Azure-abonnement. U kunt deze resources weergeven in de Azure [portal](https://portal.azure.com). Het implementatieproces maakt een **resourcegroep** met een naam die is gebaseerd op de naam die u voor uw oplossingsversneller hebt gekozen:

[![Verbonden Factory-oplossing accelerator-resourcegroep](./media/iot-accelerators-connected-factory-dashboard/resourcegroup-inline.png)](./media/iot-accelerators-connected-factory-dashboard/resourcegroup-expanded.png#lightbox)

U kunt de instellingen van elke resource weergeven door deze te selecteren in de lijst met resources in de resourcegroep.

U kunt ook weergeven met de broncode voor de oplossingsversnellers in de [azure-iot-connected-factory](https://github.com/Azure/azure-iot-connected-factory) GitHub-opslagplaats.

Wanneer u klaar bent, kunt u de oplossingsversneller verwijderen uit uw Azure-abonnement op de [azureiotsolutions.com](https://www.azureiotsolutions.com/Accelerators#dashboard) site. Met de site kunt u gemakkelijk alle resources verwijderen die zijn ingericht op het moment dat de oplossingsversneller werd gemaakt.

> [!NOTE]
> Om ervoor te zorgen dat u alles met betrekking tot de solution accelerator verwijdert, verwijdert u deze op de [azureiotsolutions.com](https://www.azureiotsolutions.com/Accelerators#dashboard) site. Verwijder de resourcegroep niet in de portal.

## <a name="next-steps"></a>Volgende stappen

Nu u een werkende oplossingsversneller hebt geïmplementeerd, kunt u gaan werken met de IoT-oplossingsversnellers door de volgende artikelen te lezen:

* [Verbonden Factory-oplossing accelerator-overzicht](iot-accelerators-connected-factory-sample-walkthrough.md)
* [Uw apparaat aansluiten op de oplossingsverbetering voor verbonden Factory](iot-accelerators-connected-factory-gateway-deployment.md)
* [Machtigingen op de site azureiotsolutions.com](iot-accelerators-permissions.md)
