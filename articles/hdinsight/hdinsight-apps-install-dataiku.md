---
title: Installatie gepubliceerde toepassing - Dataiku DDS - Azure HDInsight | Microsoft Docs
description: Installeren en gebruiken van de toepassing Dataiku DDS van derden Hadoop.
services: hdinsight
documentationcenter: ''
author: ashishthaps
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: ''
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: article
ms.date: 01/10/2018
ms.author: ashish
ms.openlocfilehash: facee9187528fa0c6a74f71ee73636dc23ca35ba
ms.sourcegitcommit: 9cdd83256b82e664bd36991d78f87ea1e56827cd
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/16/2018
---
# <a name="install-published-application---dataiku-dds"></a>Gepubliceerde toepassing - Dataiku DDS installeren

Dit artikel wordt beschreven hoe u wilt installeren en uitvoeren de [Dataiku DDS](https://www.dataiku.com/) gepubliceerde toepassing in Azure HDInsight Hadoop. Zie voor een overzicht van het HDInsight-toepassing-platform en een lijst van beschikbare Independent Software Vendors (ISV's) gepubliceerde toepassingen [Hadoop-toepassingen van derden installeren](hdinsight-apps-install-applications.md). Zie voor instructies voor de installatie van uw eigen toepassing [Aangepaste HDInsight-toepassingen installeren](hdinsight-apps-install-custom-applications.md).

## <a name="about-dataiku-dss"></a>Over Dataiku DSS

De Dataiku [gegevens wetenschappelijke Studio (DSS)](https://www.dataiku.com/dss/features/connectivity/), is een gezamenlijke gegevens wetenschappelijke platform waarmee gegevenswetenschappers te bouwen en analytische oplossingen te leveren. Toepassing kunt DSS als een HDInsight aanbieding u gegevenswetenschap gebruiken voor Big Data-oplossingen bouwen en ze worden uitgevoerd op de enterprise-klasse en de schaal.

U kunt DSS gebruiken voor het implementeren van een volledige analytische oplossing vanaf gegevensopname, voorbereiding, en te verwerken. Een DSS-oplossing kan ook training en die van toepassing machine learning-modellen, visualiseren en vervolgens operationele bevatten.

U kunt DSS installeren op HDInsight met behulp van Hadoop- of Spark-clusters. U kunt DSS installeren op bestaande actieve clusters of bij het maken van nieuwe clusters. DSS ondersteunt ook het gebruik van Azure Blob-opslag als een verbindingslijn voor het lezen van gegevens.

U kunt DSS gebruiken voor het bouwen van de projecten, die vervolgens kunnen MapReduce of Spark taken worden gegenereerd. Deze taken worden uitgevoerd als reguliere MapReduce of Spark taken in HDInsight, zodat u het cluster op aanvraag kunt schalen.

## <a name="prerequisites"></a>Vereisten

Om deze app installeren op een nieuwe HDInsight-cluster of een bestaand cluster, hebt u de volgende configuratie:

* Cluster tier(s): standaard, Premium
* Cluster type(n): Hadoop, Spark
* Cluster-versie (s): 3.4 en 3.5

## <a name="install-the-dataiku-dss-published-application"></a>Installeer de Dataiku DSS gepubliceerde toepassing

Lees voor stapsgewijze instructies over het installeren van deze en andere beschikbare ISV-toepassingen [Hadoop-toepassingen van derden installeren](hdinsight-apps-install-applications.md).

## <a name="launch-dataiku-dss"></a>Dataiku DSS starten

1. Na de installatie, kunt u DSS starten van het cluster in Azure-portal door te gaan naar de **instellingen** deelvenster vervolgens te klikken op **toepassingen** onder de **algemene** categorie. In het deelvenster geïnstalleerde Apps worden de geïnstalleerde toepassingen.

    ![Geïnstalleerde Dataiku DSS-app](./media/hdinsight-apps-install-dataiku/app.png)

2. Als u DSS op HDInsight selecteert, ziet u een koppeling naar de webpagina en het pad van SSH-eindpunt. Selecteer de koppeling van de webpagina.

3. Op de eerste keer opstarten krijgt u een formulier een nieuwe account te maken Dataiku gratis of aan te melden bij een bestaand account. U hebt ook de optie voor het starten van een gratis proefversie 2 weken van [Enterprise Edition](https://www.dataiku.com/dss/editions/). Vanaf dit punt hebt u de mogelijkheid van een licentiesleutel invoeren voor Enterprise Edition, of met behulp van de Community Edition voortzet.

4. Na het voltooien van de geselecteerde optie, krijgt u een aanmeldingsformulier. Voer de standaardreferenties weergegeven voordat het aanmeldingsformulier.

De volgende stappen bevatten een eenvoudige demonstratie.

1. [Downloaden van de voorbeeld-orders CSV](https://doc.dataiku.com/tutorials/data/101/haiku_shirt_sales.csv).

2. Selecteer in het dashboard DSS de **+** (nieuw project) koppeling in het menu links om een nieuw project te maken.

    ![Nieuwe projectkoppeling](./media/hdinsight-apps-install-dataiku/new-project.png)

3. Typ in het venster Nieuw project in een **naam**. De **sleutel Project** wordt automatisch gevuld met een voorgestelde waarde. In dit geval geeft 'Orders'. Klik op **maken**.

    ![Nieuw projectformulier](./media/hdinsight-apps-install-dataiku/new-project-form.png)

4. Selecteer **+ importeren uw eerste GEGEVENSSET** in uw nieuwe pagina van het project.

    ![Bestand uploaden](./media/hdinsight-apps-install-dataiku/import-dataset.png)

5. Selecteer **uw bestanden uploaden** onder de **bestanden** gegevensset lijst. Krijgt u het dialoogvenster uploaden. Klik op toevoegen van een bestand, selecteer de `haiku_shirt_sales.csv` bestand dat u gedownload en valideren.

6. Het bestand is geüpload naar DSS. Controleer als DSS de CSV-indeling correct gedetecteerd door te klikken op de knop voorbeeld.

    ![Bestand uploaden](./media/hdinsight-apps-install-dataiku/preview.png)

7. Het importeren is bijna ideaal. Het CSV-bestand maakt gebruik van een tabblad als scheidingsteken. U ziet dat de gegevens zich in tabelvorm, met functies en regels die betrekking hebben waarnemingen aangeroepen kolommen. De één fout is dat blijkbaar het bestand een lege regel tussen de koptekst en de gegevens bevat. U kunt deze fout oplossen door Voer `1` in de **volgende regels overslaan** veld.

    ![Opslaan](./media/hdinsight-apps-install-dataiku/skip-lines.png)

8. Geef een naam op de nieuwe gegevensset. Voer **haiku_shirt_sales** in het veld boven op het scherm, selecteert u vervolgens de **maken** knop.

9. Ziet u een tabellarische weergave van uw gegevens waar u ermee kunt starten. Voor elke kolom ziet u dat Dataiku wetenschappelijke Studio een gegevenstype is aangetroffen in _blauw_ - in dit geval, tekst, aantal of datum (niet-afgeleide). Een meter geeft de verhouding van de kolom waarvoor de waarden niet overeenkomen met het type (in rood) lijken of ontbreken (leeg). In deze dataset voorbeeld heeft de afdeling zowel lege waarden en er zijn ongeldige gegevens.

    ![Tabellarische weergave](./media/hdinsight-apps-install-dataiku/viewing-dataset.png)

## <a name="data-manipulation"></a>Gegevens verwerken

Werkelijke gegevens is bijna altijd rommelige en zelden wordt het netjes verpakt. Opruimen van de gegevens gewoonlijk vereist een keten van scripts en gekoppelde bedrijfslogica. Dataiku DSS biedt een speciaal **Lab** hulpprogramma met deze taak om gebruiksvriendelijker te maken.

1. Klik op **Lab** in de rechterbovenhoek.

    ![Knop Lab](./media/hdinsight-apps-install-dataiku/lab-button.png)

2. Het Lab-venster wordt geopend. De testomgeving is waar u iteratief werkt op uw gegevensset om verdere naar deze. Deze zelfstudie laat zien dat het aspect visuele analyse. Selecteer de **nieuw** knop onder visuele analyse. U wordt gevraagd een naam voor de analyse op te geven. Laat de standaardnaam voor deze oefening en klik vervolgens op **maken**.

    ![Lab maken](./media/hdinsight-apps-install-dataiku/create-lab.png)

3. Selecteer de **snelle kolommen statistieken** knop in de rechterbovenhoek van de pagina.

    ![Snelle kolommen statistieken](./media/hdinsight-apps-install-dataiku/quick-column-stats.png)

4. Ziet u statistieken voor gegevenstypen en waarden die worden weergegeven in de grafieken op basis van een tijdlijn onder de **snel inzicht in kolommen** deelvenster.

    ![Kolommen snel weergeven](./media/hdinsight-apps-install-dataiku/columns-quick-view.png)

U kunt nu de voorbeeldgegevens gebruikt DSS verkennen. U kunt opschonen van en werken met de gegevens en maak nieuwe visualisaties.

Lees voor meer gedetailleerde zelfstudies [Dataiku DSS meer](https://www.dataiku.com/learn/).

## <a name="next-steps"></a>Volgende stappen

* [Dataiku DSS documentatie](https://doc.dataiku.com/dss/latest/).
* [Aangepaste HDInsight-toepassingen installeren](hdinsight-apps-install-custom-applications.md): informatie over het implementeren van een niet-gepubliceerde HDInsight-toepassing op HDInsight.
* [HDInsight-toepassingen publiceren](hdinsight-apps-publish-applications.md): informatie over het publiceren van aangepaste HDInsight-toepassingen in Azure Marketplace.
* [MSDN: een HDInsight-toepassing installeren](https://msdn.microsoft.com/library/mt706515.aspx): informatie over het definiëren van HDInsight-toepassingen.
* [Met behulp van de scriptactie Linux gebaseerde HDInsight-clusters aanpassen](hdinsight-hadoop-customize-cluster-linux.md): informatie over het gebruik van scriptactie om extra toepassingen te installeren.
* [Lege edge-knooppunten gebruiken in HDInsight](hdinsight-apps-use-edge-node.md): informatie over het gebruik van een leeg edge-knooppunt voor toegang tot HDInsight-clusters en voor het testen en die als host fungeert voor HDInsight-toepassingen.
