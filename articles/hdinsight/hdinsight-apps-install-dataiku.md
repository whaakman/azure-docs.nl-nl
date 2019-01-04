---
title: Dataiku DDS - Azure HDInsight voor gepubliceerde toepassing - installeren
description: Installeren en gebruiken van de toepassing Dataiku DDS van derden Apache Hadoop.
services: hdinsight
author: ashishthaps
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 01/10/2018
ms.author: ashish
ms.openlocfilehash: 5d2f803414db3bd118f57d317cfc32c8f22fcd5a
ms.sourcegitcommit: e68df5b9c04b11c8f24d616f4e687fe4e773253c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/20/2018
ms.locfileid: "53653252"
---
# <a name="install-published-application---dataiku-dds"></a>Dataiku DDS voor gepubliceerde toepassing - installeren

In dit artikel wordt beschreven hoe u wilt installeren en uitvoeren de [Dataiku DDS](https://www.dataiku.com/) gepubliceerd [Apache Hadoop](https://hadoop.apache.org/) toepassing voor Azure HDInsight. Zie voor een overzicht van het HDInsight-platform voor toepassingen en een lijst van beschikbare Independent Software Vendor (ISV) gepubliceerde toepassingen [Hadoop-toepassingen van derden installeren](hdinsight-apps-install-applications.md). Zie voor instructies voor de installatie van uw eigen toepassing [Aangepaste HDInsight-toepassingen installeren](hdinsight-apps-install-custom-applications.md).

## <a name="about-dataiku-dss"></a>Over Dataiku DSS

De Dataiku [Data Science Studio (DSS)](https://www.dataiku.com/dss/features/connectivity/), is een samenwerkingsplatform voor data science-platform waarmee gegevenswetenschappers en analytische oplossingen leveren. Toepassing kunt DSS als een HDInsight-aanbieding u gegevenswetenschap gebruiken om Big Data-oplossingen bouwen en op zakelijk niveau en schaal uitvoeren.

U kunt DSS gebruiken voor het implementeren van een volledige analytische oplossing vanaf gegevensopname, voorbereiding en verwerken. Een DSS-oplossing kan ook training en die van toepassing machine learning-modellen, visualisatie en vervolgens tot het operationaliseren van bevatten.

U kunt DSS installeren op HDInsight met behulp van [Apache Hadoop](https://hadoop.apache.org/) of [Apache Spark](https://spark.apache.org/) clusters. U kunt DSS installeren op bestaande actieve clusters, of bij het maken van nieuwe clusters. DSS biedt ook ondersteuning voor het gebruik van Azure Blob-opslag als een connector voor het lezen van gegevens.

U kunt DSS gebruiken voor het bouwen van projecten, die vervolgens kunnen genereren [MapReduce](https://hadoop.apache.org/docs/r1.2.1/mapred_tutorial.html) of Spark-taken. Deze taken worden uitgevoerd als er regelmatig MapReduce of Spark-taken in HDInsight, zodat u het cluster op aanvraag kunt schalen.

## <a name="prerequisites"></a>Vereisten

Als u wilt deze app installeren op een nieuw HDInsight-cluster of een bestaand cluster, hebt u de volgende configuratie:

* Clusterniveau: Standard, Premium
* Clustertype(n): Hadoop, Spark
* Clusterversie (s): 3.4, 3.5

## <a name="install-the-dataiku-dss-published-application"></a>Gepubliceerde toepassing installeren de Dataiku DSS

Lees voor stapsgewijze instructies over het installeren van deze en andere beschikbare ISV-toepassingen [Apache Hadoop-toepassingen van derden installeren](hdinsight-apps-install-applications.md).

## <a name="launch-dataiku-dss"></a>Dataiku DSS starten

1. Na de installatie, kunt u DSS starten vanuit het cluster in Azure portal door te gaan naar de **instellingen** deelvenster, klikt u vervolgens te klikken op **toepassingen** onder de **algemene** categorie. Het deelvenster met geïnstalleerde Apps worden de geïnstalleerde toepassingen.

    ![Geïnstalleerde Dataiku DSS-app](./media/hdinsight-apps-install-dataiku/app.png)

2. Wanneer u DSS op HDInsight selecteert, ziet u een koppeling naar de webpagina wordt weergegeven en het pad van SSH-eindpunt. Selecteer de koppeling naar de webpagina.

3. Op de eerste keer opstarten krijgt u een formulier gratis een nieuwe Dataiku-account maken of zich aanmeldt bij een bestaand account. U hebt ook de optie voor een gratis proefabonnement 2 weken van [Enterprise Edition](https://www.dataiku.com/dss/editions/). Vanaf dit punt hebt u de mogelijkheid u doorgaat met het invoeren van een licentiesleutel voor Enterprise Edition of met behulp van de Community-versie.

4. Na het voltooien van de geselecteerde licentiegroepen-optie, krijgt u een aanmeldingsformulier. Voer de standaardreferenties voorafgaand aan het aanmeldingsformulier weergegeven.

De volgende stappen bevatten een eenvoudige demonstratie.

1. [Downloaden van de voorbeeld-orders CSV](https://doc.dataiku.com/tutorials/data/101/haiku_shirt_sales.csv).

2. Selecteer in het dashboard DSS de **+** (nieuw project) koppeling in het menu links een nieuw project te maken.

    ![Nieuwe koppeling van het project](./media/hdinsight-apps-install-dataiku/new-project.png)

3. Typ in het venster Nieuw project in een **naam**. De **Projectsleutel** wordt automatisch gevuld met een aanbevolen waarde. In dit geval geeft 'Orders'. Klik op **MAKEN**.

    ![Nieuw projectformulier](./media/hdinsight-apps-install-dataiku/new-project-form.png)

4. Selecteer **+ importeren van de eerste GEGEVENSSET** project van de nieuwe pagina.

    ![Bestand uploaden](./media/hdinsight-apps-install-dataiku/import-dataset.png)

5. Selecteer **uw bestanden uploaden** onder de **bestanden** lijst van de gegevensset. Het dialoogvenster voor het uploaden wordt weergegeven. Klik op toevoegen van een bestand, selecteer de `haiku_shirt_sales.csv` bestand gedownload en valideren.

6. Het bestand wordt geüpload naar DSS. Controleer als DSS de CSV-indeling correct gedetecteerd door te klikken op de knop voorbeeld.

    ![Bestand uploaden](./media/hdinsight-apps-install-dataiku/preview.png)

7. Het importeren is bijna ideaal. Het CSV-bestand maakt gebruik van een tabblad scheidingsteken. U ziet de gegevens in tabelvorm, met de kolommen functies en regels die staan voor metingen. De één fout is dat blijkbaar het bestand een lege regel tussen de koptekst en de gegevens bevat. U kunt deze fout oplossen, voer `1` in de **overslaan van de volgende regels** veld.

    ![Opslaan](./media/hdinsight-apps-install-dataiku/skip-lines.png)

8. Geef een naam op de nieuwe gegevensset. Voer **haiku_shirt_sales** in het veld boven op het scherm, selecteert u vervolgens de **maken** knop.

9. U ziet een tabellarische weergave van uw gegevens waar u ermee kan starten. Voor elke kolom ziet u dat Dataiku Science Studio een gegevenstype aangetroffen in _blauwe_ : in dit geval, tekst, getal of datum (niet-afgeleide). Een meter geeft de verhouding van de kolom die de waarden niet lijken overeen met het type (in rood) of ontbreken (leeg). In deze gegevensset voorbeeld heeft de afdeling lege waarden zowel ongeldige gegevens.

    ![Tabellarische weergave](./media/hdinsight-apps-install-dataiku/viewing-dataset.png)

## <a name="data-manipulation"></a>Gegevensmanipulatie

Echte gegevens is bijna altijd rommelige en zelden deze netjes verpakt. Opschonen van gegevens, meestal vereist een keten van scripts en bijbehorende bedrijfslogica. Dataiku DSS biedt een toegewezen **Lab** hulpprogramma voor deze taak om gebruiksvriendelijker te maken.

1. Klik op **Lab** in de rechterbovenhoek.

    ![Lab-knop](./media/hdinsight-apps-install-dataiku/lab-button.png)

2. Het Lab-venster wordt geopend. De testomgeving is waar u iteratief werkt op uw gegevensset in het verdere krijgt. In deze zelfstudie ziet u het aspect visuele analyse. Selecteer de **nieuw** knop onder visuele analyse. U wordt gevraagd een naam voor uw analyse op te geven. Laat de standaardnaam voor deze oefening en klik vervolgens op **maken**.

    ![Lab maken](./media/hdinsight-apps-install-dataiku/create-lab.png)

3. Selecteer de **snel kolommen statistieken** knop in de rechterbovenhoek van de pagina.

    ![Snelle kolommen statistieken](./media/hdinsight-apps-install-dataiku/quick-column-stats.png)

4. Ziet u statistieken voor gegevenstypen en waarden die worden weergegeven in de tijdlijn op basis van grafieken onder de **snel weergeven van kolommen** deelvenster.

    ![Snel weergeven van kolommen](./media/hdinsight-apps-install-dataiku/columns-quick-view.png)

U kunt nu verkennen met behulp van de voorbeeldgegevens DSS. U kunt opschonen met de gegevens werken en nieuwe visualisaties maken.

Lees voor uitgebreide zelfstudies [meer Dataiku DSS](https://www.dataiku.com/learn/).

## <a name="next-steps"></a>Volgende stappen

* [Dataiku DSS documentatie](https://doc.dataiku.com/dss/latest/).
* [Aangepaste HDInsight-toepassingen installeren](hdinsight-apps-install-custom-applications.md): Informatie over het implementeren van een niet-gepubliceerde HDInsight-toepassing op HDInsight.
* [HDInsight-toepassingen publiceren](hdinsight-apps-publish-applications.md): Informatie over het publiceren van uw aangepaste HDInsight-toepassingen op Azure Marketplace.
* [MSDN: Een HDInsight-toepassing installeren](https://msdn.microsoft.com/library/mt706515.aspx): Meer informatie over het definiëren van HDInsight-toepassingen.
* [HDInsight Linux gebaseerde clusters aanpassen met Script Action](hdinsight-hadoop-customize-cluster-linux.md): Informatie over het gebruik scriptacties om extra toepassingen te installeren.
* [Lege edge-knooppunten gebruiken in HDInsight](hdinsight-apps-use-edge-node.md): Leer hoe u een lege edge-knooppunt gebruiken voor toegang tot HDInsight-clusters, en voor het testen en HDInsight-toepassingen hosten.
