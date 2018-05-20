---
title: Externe controle gegevens visualiseren met Power BI - Azure | Microsoft Docs
description: Deze zelfstudie maakt gebruik van Power BI Desktop- en Cosmos DB integerate gegevens van een oplossing voor externe controle in een aangepaste visualisatie. Deze manier waarop gebruikers kunnen hun eigen aangepaste dashboards bouwen en computers delen niet op de oplossing van gebruikers.
services: iot-suite
suite: iot-suite
author: asdonald
manager: hegate
ms.author: asdonald
ms.service: iot-suite
ms.date: 05/01/2018
ms.topic: article
ms.devlang: NA
ms.tgt_pltfrm: NA
ms.workload: NA
ms.openlocfilehash: 45b91241223b1286cc412ab609e60aeadef51b85
ms.sourcegitcommit: 688a394c4901590bbcf5351f9afdf9e8f0c89505
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/17/2018
---
# <a name="visualize-remote-monitoring-data-using-power-bi"></a>Externe controle gegevens visualiseren met Power BI

Deze zelfstudie begeleidt u stapsgewijs door van hoe u aan te brengen in uw oplossing voor externe controle gegevens van CosmosDB naar Power BI. Met deze verbinding tot stand gebracht, kunt u uw eigen aangepaste dashboards maken en toe te voegen terug op het dashboard van de oplossing voor externe controle. Deze workstream kunt u meer gespecialiseerde grafieken worden gemaakt, naast die buiten het vak. Vervolgens kunt u deze zelfstudie integreren met andere gegevensstromen of aangepaste dashboards om te worden verbruikt buiten uw oplossing voor externe controle wilt bouwen. Opbouwen van dashboards in Power BI betekent dat u elk deelvenster met elkaar communiceren, terwijl u specifieke stukken selecteert ook kunt maken. Bijvoorbeeld, u een filter dat u alleen informatie over uw gesimuleerde vrachtwagens kunnen hebben en elk onderdeel van uw dashboard zodat u alleen gesimuleerde vrachtwagen informatie zou werken. Als u gebruiken dan Power BI wilt, kunt u deze stappen voor het gebruik van uw visualisatie hulpprogramma naar keuze en in de Cosmos-Database, of de aangepaste database koppelen als u een hebt ingesteld ook uitbreiden. 

## <a name="prerequisites"></a>Vereisten

- Hebt u een oplossing voor externe controle momenteel wordt uitgevoerd
- U moet toegang hebben tot [Azure Portal](https://portal.azure.com) en uw abonnement op waarop de IoT Hub en de oplossing worden uitgevoerd
- U moet hebben [van Power BI desktop](https://powerbi.microsoft.com) is geïnstalleerd, wordt de doet een willekeurige versie


## <a name="information-needed-from-azure-portal"></a>Informatie die nodig is via Azure-Portal

1. Navigeer naar [Azure Portal](https://portal.azure.com) en meld u indien nodig

2. Klik in het deelvenster links op resourcegroepen

    ![Zijde Configuratiescherm Nav](media/iot-suite-remote-monitoring-powerbi/side_panel.png)

3. Navigeer naar de resourcegroep die uw Iot-oplossing wordt uitgevoerd op en klik hier om te worden uitgevoerd om de pagina overzicht van die resourcegroep. 

4. Op die overzichtspagina klikt u op het item, dat het type 'Azure Cosmos DB Account' is, klikt u vervolgens gaat u naar de overzichtspagina van de Cosmos-DB-stroom voor deze IoT-oplossing.

    ![Resourcegroep](media/iot-suite-remote-monitoring-powerbi/resource_groups.png)

5. In het paneel aan de linkerkant, klik op de sectie 'Sleutels' en noteer de volgende waarden worden gebruikt in Power BI:

    - URI
    - Primaire sleutel

    ![keys](media/iot-suite-remote-monitoring-powerbi/keys.png)

## <a name="setting-up-the-stream-in-power-bi"></a>Instellen van de stroom in Power BI
  
1. Opent u de Power BI desktop-app en klikt u op "Get-Data" in de linkerbovenhoek. 

    ![Gegevens ophalen](media/iot-suite-remote-monitoring-powerbi/get_data.png)

2. Wanneer u wordt gevraagd de gegevens in te voeren, ervoor kiezen om te zoeken naar "DB met Azure Cosmos" en selecteert u deze connector. Deze connector in wezen haalt gegevens rechtstreeks uit de database van de cosmos van uw Azure-IoT-oplossing
  
    ![Cosmos DB](media/iot-suite-remote-monitoring-powerbi/cosmos_db.png)
  
3. Voer de gegevens die u hierboven hebt genoteerd:

    * URI
    * Primaire sleutel

4. Selecteer de tabellen om te importeren naar Power BI. Deze actie wordt ere van het laden van de gegevens. Hoe langer uw oplossing actief is geweest, hoe langer kan duren om de gegevens te laden (maximaal een paar uur). 

    ![Tabellen importeren](media/iot-suite-remote-monitoring-powerbi/import_tables.png)

5. Nadat de gegevens is voltooid wordt geladen, klik op 'Bewerken query's ' op de eerste rij van Power BI en alle tabellen uitvouwen door te klikken op de pijlen in de gele balk voor elke tabel. Dit wordt in wezen uitgebreid alle kolommen weergeven. U ziet hoe gegevens voor dingen zoals vochtigheid, versnellen, etc. zijn niet van het juiste type.

    ![Nieuwe kolom](media/iot-suite-remote-monitoring-powerbi/new_column.png)
  
    De gegevens die afkomstig zijn bij Power BI is bijvoorbeeld gewijzigd in UNIX tijd wanneer deze is geleverd door de connector. Als u wilt aanpassen voor deze conversie, kunt voortaan u een nieuwe kolom maken en deze vergelijking gebruik om deze naar datum-tijdnotatie: 

    ```text
    #datetime(1970, 1, 1, 0, 0, 0) + #duration(0, 0, 0, [Document.device.msg.received]/1000)
    ```

    ![Bijgewerkte tabel](media/iot-suite-remote-monitoring-powerbi/updated_table.png)
  
    Document.Device.msg.Received is slechts een van de kolommen met UNIX-opmaak en kunnen worden vervangen door anderen die conversie nodig. 
  
    Andere gegevenspunten zijn geconverteerd naar type die String kan worden gewijzigd in verdubbelt of Int waar zoals hierboven juiste met behulp van dezelfde stappen.

## <a name="creating-a-dashboard"></a>Een dashboard maken

Zodra de stroom is verbonden, bent u klaar om uw persoonlijke dashboards te maken. Het dashboard Hieronder volgt een voorbeeld rekening gehouden met de telemetrie wordt immmited door onze gesimuleerde apparaten en het weergeven van verschillende gedraaid omheen, zoals: 

* Locatie van het apparaat op een kaart (rechts)
* Apparaten met hun status en ernst. (linksboven)
* Apparaten met regels aanwezig is, en als er alarmen gaat uitschakelen voor hen (linksonder)

![Visualisatie van Power BI](media/iot-suite-remote-monitoring-powerbi/visual_data.png)

## <a name="publishing-the-dashboard-and-refreshing-the-data"></a>Publiceren van het dashboard en de gegevens vernieuwen

Nadat u uw dashboards hebt gemaakt, raden we u [publiceren van uw Power BI-dashboards](https://docs.microsoft.com/en-us/power-bi/desktop-upload-desktop-files) met anderen kunt delen.

Ook moet u [de gegevens vernieuwen](https://docs.microsoft.com/en-us/power-bi/refresh-data) op het gepubliceerde-dashboard om ervoor te zorgen dat u de meest recente gegevensset hebt.

## <a name="next-steps"></a>Volgende stappen

In dit artikel hebt u geleerd over het externe controle gegevens visualiseren met Power BI

Zie voor meer informatie over het aanpassen van de oplossing voor externe controle:

* [De oplossing voor externe controle UI aanpassen](../iot-accelerators/iot-accelerators-remote-monitoring-customize.md)
* [Snelzoekgids voor ontwikkelaars](https://github.com/Azure/azure-iot-pcs-remote-monitoring-dotnet/wiki/Developer-Reference-Guide)
* [Ontwikkelaarsgids voor het oplossen van problemen](https://github.com/Azure/azure-iot-pcs-remote-monitoring-dotnet/wiki/Developer-Troubleshooting-Guide)

