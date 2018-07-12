---
title: Bewaking op afstand gegevens visualiseren met Power BI - Azure | Microsoft Docs
description: In deze zelfstudie maakt gebruik van Power BI Desktop en Cosmos DB integerate gegevens van een oplossing voor externe controle in een aangepaste visualisatie. Deze manier waarop gebruikers kunnen hun eigen aangepaste dashboards kunt bouwen en delen van deze gebruikers niet op de oplossing.
author: asdonald
manager: hegate
ms.author: asdonald
ms.service: iot-accelerators
services: iot-accelerators
ms.date: 05/01/2018
ms.topic: conceptual
ms.openlocfilehash: ae039573cf202059114f23cca86207c117a35ead
ms.sourcegitcommit: f606248b31182cc559b21e79778c9397127e54df
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/12/2018
ms.locfileid: "38970396"
---
# <a name="visualize-remote-monitoring-data-using-power-bi"></a>Bewaking op afstand gegevens visualiseren met Power BI

In deze zelfstudie begeleidt u stapsgewijs door van hoe u aan te sluiten in uw oplossing voor bewaking op afstand gegevens uit cosmos DB in Power BI. Met deze verbinding tot stand gebracht, kunt u vervolgens uw eigen aangepaste dashboards maken en ze terug op uw oplossing voor externe bewaking dashboard toevoegen. Deze workstream kunt u meer gespecialiseerde grafieken worden gemaakt, aanvulling op het gebruiksklaar. Vervolgens kunt u deze zelfstudie om te integreren met andere data-stromen of aangepaste dashboards om te worden verbruikt buiten uw oplossing voor externe controle kunt bouwen. Het bouwen van dashboards in Power BI, betekent dat u elk deelvenster met elkaar samenwerken, terwijl u specifieke selecteert ook kunt maken. Bijvoorbeeld, kan er een filter dat laat u alleen informatie over uw gesimuleerde vrachtwagens zien en elk onderdeel van uw dashboard zodat u alleen informatie van de vrachtwagen gesimuleerde zou werken. Als u wilt een Power BI-hulpprogramma te gebruiken, kunt u deze stappen voor het gebruik van uw Visualisatieprogramma van keuze en koppelen in de Cosmos-Database of aangepaste database als u een hebt ingesteld ook uitbreiden. 

## <a name="prerequisites"></a>Vereisten

- Hebt u een oplossing voor externe controle momenteel wordt uitgevoerd
- U moet toegang hebben tot [Azure Portal](https://portal.azure.com) en uw abonnement waarin de IoT Hub en de oplossing worden uitgevoerd
- U moet hebben [Power BI desktop](https://powerbi.microsoft.com) geïnstalleerd, elke versie doet


## <a name="information-needed-from-azure-portal"></a>Informatie die nodig is in Azure Portal

1. Navigeer naar [Azure Portal](https://portal.azure.com) en meld u indien nodig

2. Klik in het linkerdeelvenster op resourcegroepen

    ![Zijde deelvenster Nav](./media/iot-accelerators-integrate-data-powerbi/side_panel.png)

3. Navigeer naar de resourcegroep die uw Iot-oplossing wordt uitgevoerd op en klik op gaat u naar de pagina overzicht van de groep van die Resource. 

4. Op die overzichtspagina klikt u op het item, dat is van het type 'Azure Cosmos DB-Account', klikt u vervolgens gaat u naar de overzichtspagina van de Cosmos DB-stroom voor die IoT-oplossing.

    ![Resourcegroep](./media/iot-accelerators-integrate-data-powerbi/resource_groups.png)

5. In het paneel aan de linkerkant, klik op de sectie 'Sleutels' en noteer de volgende waarden om te worden gebruikt in Power BI:

    - URI
    - Primaire sleutel

    ![keys](./media/iot-accelerators-integrate-data-powerbi/keys.png)

## <a name="setting-up-the-stream-in-power-bi"></a>Instellen van de Stream in Power BI
  
1. Open de Power BI desktop-app en klikt u op 'Gegevens ophalen' in de linkerbovenhoek. 

    ![Gegevens ophalen](./media/iot-accelerators-integrate-data-powerbi/get_data.png)

2. Wanneer u wordt gevraagd om in te voeren van gegevens, ervoor kiezen om te zoeken naar 'Azure Cosmos DB' en selecteert u deze connector. Deze connector worden gegevens rechtstreeks vanuit de cosmos-database van uw Azure-IoT-oplossing in feite opgehaald
  
    ![Cosmos DB](./media/iot-accelerators-integrate-data-powerbi/cosmos_db.png)
  
3. Voer de gegevens die u hierboven hebt genoteerd:

    * URI
    * Primaire sleutel

4. Selecteer de tabellen worden geïmporteerd in Power BI. Deze actie wordt het laden van de gegevens gestart. Hoe langer uw oplossing actief is geweest, hoe langer duurt om de gegevens te laden (maximaal een paar uur). 

    ![Tabellen importeren](./media/iot-accelerators-integrate-data-powerbi/import_tables.png)

5. Nadat de gegevens is voltooid wordt geladen, klikt u op 'Query's bewerken' op de bovenste rij van Power BI en alle tabellen uitvouwen door te klikken op de pijlen in de gele balk voor elke tabel. Dit wordt in feite uitgebreid om alle kolommen weer te geven. U ziet hoe de gegevens voor zaken zoals vochtigheid, sneller, enzovoort zijn niet van het juiste type.

    ![Nieuwe kolom](./media/iot-accelerators-integrate-data-powerbi/new_column.png)
  
    Bijvoorbeeld, is de gegevens die afkomstig zijn in Power BI gewijzigd in de UNIX-tijd waarop het afkomstig via de connector is. Als u wilt aanpassen voor deze conversie, kunt voortaan u een nieuwe kolom maken en gebruiken deze vergelijking zodat deze in datum-tijdnotatie: 

    ```text
    #datetime(1970, 1, 1, 0, 0, 0) + #duration(0, 0, 0, [Document.device.msg.received]/1000)
    ```

    ![Bijgewerkte tabel](./media/iot-accelerators-integrate-data-powerbi/updated_table.png)
  
    Document.Device.msg.Received is slechts een van de kolommen bij het opmaken van UNIX en met anderen die conversie moet kunnen worden vervangen. 
  
    Andere gegevenspunten zijn geconverteerd naar het type die tekenreeks kan worden gewijzigd in verdubbelt of Int waar juiste met behulp van dezelfde als hierboven stappen.

## <a name="creating-a-dashboard"></a>Een dashboard maken

Als de stroom is verbonden, bent u klaar om uw persoonlijke dashboards te maken! Het dashboard Hieronder volgt een voorbeeld rekening gehouden met de telemetrie wordt immmited door onze gesimuleerde apparaten en het weergeven van verschillende rond het zoals opengaat: 

* Locatie van het apparaat op een kaart (rechts)
* Apparaten met hun status en ernst. (linksboven)
* Apparaten met regels aanwezig is, en als er waarschuwingen gaan uitschakelen voor deze (onderaan, links)

![Power BI-visualisatie](./media/iot-accelerators-integrate-data-powerbi/visual_data.png)

## <a name="publishing-the-dashboard-and-refreshing-the-data"></a>Publiceren van het dashboard en de gegevens vernieuwen

Nadat u uw dashboards hebt gemaakt, raden we u [publiceren van uw Power BI-dashboards](https://docs.microsoft.com/power-bi/desktop-upload-desktop-files) worden gedeeld met anderen.

Ook moet u [de gegevens vernieuwen](https://docs.microsoft.com/power-bi/refresh-data) op de gepubliceerde dashboard om ervoor te zorgen dat u de meest recente gegevensset hebt.

## <a name="next-steps"></a>Volgende stappen

In dit artikel hebt u geleerd over het externe bewaking gegevens visualiseren met Power BI

Zie voor meer informatie over het aanpassen van de oplossing voor externe controle:

* [De oplossing voor externe controle gebruikersinterface aanpassen](iot-accelerators-remote-monitoring-customize.md)
* [Snelzoekgids voor ontwikkelaars](https://github.com/Azure/azure-iot-pcs-remote-monitoring-dotnet/wiki/Developer-Reference-Guide)
* [Ontwikkelaarsgids voor het oplossen van problemen](https://github.com/Azure/azure-iot-pcs-remote-monitoring-dotnet/wiki/Developer-Troubleshooting-Guide)

