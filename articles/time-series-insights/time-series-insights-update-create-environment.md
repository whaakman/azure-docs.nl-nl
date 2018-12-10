---
title: Zelfstudie voor Azure Time Series Insights (preview) | Microsoft Docs
description: Informatie over Azure Time Series Insights (preview)
author: ashannon7
ms.author: anshan
ms.workload: big-data
manager: cshankar
ms.service: time-series-insights
services: time-series-insights
ms.topic: tutorial
ms.date: 11/26/2018
ms.openlocfilehash: ed25d03f7c592476b9284790ac12f9954661a42b
ms.sourcegitcommit: b0f39746412c93a48317f985a8365743e5fe1596
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/04/2018
ms.locfileid: "52872302"
---
# <a name="azure-time-series-insights-preview-tutorial"></a>Zelfstudie voor Azure Time Series Insights (preview)

Deze zelfstudie begeleidt u bij het maken van een preview-omgeving van Azure Time Series Insights (TSI), gevuld met gegevens van gesimuleerde apparaten. In deze zelfstudie leert u het volgende:

* Een TSI-preview-omgeving maken.
* Koppel de TSI-preview-omgeving aan een Event Hub.
* Voer een simulatie voor een windpark uit om gegevens naar de TSI-preview-omgeving te streamen.
* Voer een eenvoudige analyse van de gegevens uit.
* Definieer een type Time Series Model en een hiërarchie en koppel deze aan uw exemplaren.

## <a name="create-a-time-series-insights-preview-environment"></a>Een Time Series Insights-preview-omgeving maken

In deze sectie wordt beschreven hoe u een Azure TSI-preview-omgeving maakt met behulp van de [Azure-portal](https://portal.azure.com/).

1. Meld u aan bij de Azure-portal met uw abonnementsaccount
1. Selecteer linksboven **+ Een resource maken**.
1. Selecteer de categorie **Internet of Things** en vervolgens **Time Series Insights**.

  ![zelfstudie-een][1]

1. Vul de vereiste parameters in op de pagina Time Series Insights en klik op **Next: Event Source**

  ![zelfstudie-twee][2]

1. Vul de vereiste parameters in op de pagina  **Gebeurtenisbron** en klik op **Review + Create**.

  ![zelfstudie-drie][3]

1. Bekijk de details en klik op **Maken** om uw omgeving te gaan inrichten.

  ![zelfstudie-vier][4]

1. U ontvangt een melding zodra de implementatie is voltooid.

  ![zelfstudie-vijf][5]

## <a name="send-events-to-your-tsi-environment"></a>Gebeurtenissen naar uw TSI-omgeving verzenden

In deze sectie gebruikt u een simulator voor een windmolenapparaat om via een Event Hub gebeurtenissen naar uw TSI-omgeving te verzenden.

  1. Ga in de Azure-portal naar uw Event Hub-resource en verbind deze met uw TSI-omgeving. Informatie over [hoe u uw resource aan een bestaande Event Hub verbindt](./time-series-insights-how-to-add-an-event-source-eventhub.md).

  1. Ga op de pagina Event Hub-resource naar **Beleid voor gedeelde toegang** en vervolgens naar **RootManageSharedAccessKey**. Kopieer de **primaire sleutel van de verbindingsreeks** die hier wordt weergegeven.

      ![zelfstudie-zes][6]

  1. Ga naar [https://tsiclientsample.azurewebsites.net/windFarmGen.html]( https://tsiclientsample.azurewebsites.net/windFarmGen.html). Deze web-app simuleert windmolenapparaten.
  1. Plak de verbindingsreeks die u in stap 3 hebt gekopieerd in de **Event Hub-verbindingsreeks**

      ![zelfstudie-zeven][7]

  1. Klik op **Click to Start** om gebeurtenissen naar de Event Hub te pushen. In deze fase wordt het bestand `instances.json` naar uw computer gedownload. Sla het bestand op. U hebt het later nodig.

  1. Ga terug naar de Event Hub. U moet nu de nieuwe gebeurtenissen zien die door de hub worden ontvangen.

     ![zelfstudie-acht][8]

## <a name="analyze-data-in-your-environment"></a>Gegevens analyseren in uw omgeving

In deze sectie voert u eenvoudige analyses uit op uw Time Series-gegevens met behulp van de Time Series Insights-bijwerkverkenner.

  1. Ga naar de Time Series Insights-bijwerkverkenner door op de URL te klikken op de resourcepagina in de Azure-portal.

      ![zelfstudie-negen][9]

  1. Klik in de verkennen op de knooppunten onder **Unparented Instances** als u alle Time Series-exemplaren in de omgeving wilt zien.

     ![zelfstudie-tien][10]

  1. In deze zelfstudie worden de gegevens geanalyseerd die de afgelopen dag zijn verzonden. Klik hiervoor op **Snelle tijden** en selecteer de optie **Afgelopen 24 uur**.

     ![zelfstudie-elf][11]

  1. Selecteer **Sensor_0** en kies **Show Avg Value** om de gegevens te visualiseren die vanaf dit Time Series-exemplaar zijn verzonden.

     ![zelfstudie-twaalf][12]

  1. U kunt op soortgelijke manier gegevens weergeven van andere Time Series-exemplaren om een eenvoudige analyse uit te voeren.

     ![zelfstudie-dertien][13]

## <a name="define-a-type--hierarchy"></a>Type en hiërarchie definiëren 

In deze sectie maakt u een type en een hiërarchie en koppelt u ze aan uw Time Series-exemplaren. Meer informatie over [Time Series-modellen](./time-series-insights-update-tsm.md).

  1. Klik op de app-balk in de verkenner op het tabblad **Model**.

     ![zelfstudie-veertien][14]

  1. Klik in de sectie Typen op **+ Toevoegen**. Hiermee maakt u een nieuw type Time Series-model.

     ![zelfstudie-vijftien][15]

  1. Voer in de type-editor een **naam** en **beschrijving** in en maak variabelen voor de waarden **Gemiddelde**, **Min** en **Max**, zoals hieronder aangegeven. Klik op **Maken** om het type op te slaan.

     ![zelfstudie-zestien][16]

     ![zelfstudie-zeventien][17]

  1. Klik in de sectie **Hiërarchieën** op **+ Toevoegen**. Hiermee maakt u een nieuwe hiërarchie voor het Time Series-model.

     ![zelfstudie-achttien][18]

  1. Voer in de hiërarchie-editor een **naam** in en voeg hiërarchieniveaus toe, zoals hieronder aangegeven. Klik op **Maken** om de hiërarchie op te slaan.

     ![zelfstudie-negentien][19]

     ![zelfstudie-twintig][20]

  1. Selecteer een exemplaar in de sectie **Exemplaren** en klik op **Bewerken**. Hiermee koppelt u een type en een hiërarchie aan dit exemplaar.

     ![zelfstudie eenentwintig][21]

  1. Kies het type en de hiërarchie in de exemplaareditor, zoals gedefinieerd in stap 3 en 5, zoals hierboven aangegeven.

     ![zelfstudie tweeëntwintig][22]

  1. Als u dit voor alle exemplaren ineens wilt doen, kunt u het bestand `instances.json` bewerken dat eerder is gedownload. Vervang in dit bestand alle velden **typeId** en **hierarchyId** door de id die in stap 3 en 5 hierboven is verkregen.

  1. Klik in de sectie **Exemplaren** op **JSON uploaden** en upload het bewerkte bestand `instances.json`, zoals hieronder aangegeven.

     ![zelfstudie drieëntwintig][23]

  1. Ga naar het tabblad **Analytische gegevens** en vernieuw de browser. Als het goed is, ziet u nu alle exemplaren die zijn gekoppeld aan het type en de hiërarchie die hierboven zijn gedefinieerd.

     ![zelfstudie vierentwintig][24]

## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie heeft u het volgende geleerd:  

* Een TSI-preview-omgeving maken.
* Koppel de TSI-preview-omgeving aan een Event Hub.
* Voer een simulatie voor een windpark uit om gegevens naar de TSI-preview-omgeving te streamen.
* Voer een eenvoudige analyse van de gegevens uit.
* Definieer een type Time Series Model en een hiërarchie en koppel deze aan uw exemplaren.

Nu u uw eigen TSI-update-omgeving kunt maken, kunt u meer leren over de belangrijkste concepten in TSI.

Informatie over de configuratie van TSI-opslag:

> [!div class="nextstepaction"]
> [De opslag van gegevens en inkomend verkeer in de Azure Time Series Insights (preview)](./time-series-insights-update-storage-ingress.md)

Meer informatie over Time Series-modellen:

> [!div class="nextstepaction"]
> [Gegevensmodellering van Azure TSI (preview)](./time-series-insights-update-tsm.md)

<!-- Images -->
[1]: media/v2-update-provision/tutorial-one.png
[2]: media/v2-update-provision/tutorial-two.png
[3]: media/v2-update-provision/tutorial-three.png
[4]: media/v2-update-provision/tutorial-four.png
[5]: media/v2-update-provision/tutorial-five.png
[6]: media/v2-update-provision/tutorial-six.png
[7]: media/v2-update-provision/tutorial-seven.png
[8]: media/v2-update-provision/tutorial-eight.png
[9]: media/v2-update-provision/tutorial-nine.png
[10]: media/v2-update-provision/tutorial-ten.png
[11]: media/v2-update-provision/tutorial-eleven.png
[12]: media/v2-update-provision/tutorial-twelve.png
[13]: media/v2-update-provision/tutorial-thirteen.png
[14]: media/v2-update-provision/tutorial-fourteen.png
[15]: media/v2-update-provision/tutorial-fifteen.png
[16]: media/v2-update-provision/tutorial-sixteen.png
[17]: media/v2-update-provision/tutorial-seventeen.png
[18]: media/v2-update-provision/tutorial-eighteen.png
[19]: media/v2-update-provision/tutorial-nineteen.png
[20]: media/v2-update-provision/tutorial-twenty.png
[21]: media/v2-update-provision/tutorial-twenty-one.png
[22]: media/v2-update-provision/tutorial-twenty-two.png
[23]: media/v2-update-provision/tutorial-twenty-three.png
[24]: media/v2-update-provision/tutorial-twenty-four.png