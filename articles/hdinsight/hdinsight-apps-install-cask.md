---
title: Gepubliceerde toepassing - Datameer - Azure HDInsight installeren
description: Installeren en gebruiken van de toepassing Datameer van derden Hadoop.
services: hdinsight
author: ashishthaps
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 01/10/2018
ms.author: ashish
ms.openlocfilehash: 715e536d7356a4e37f512027a23236b1fd37cbac
ms.sourcegitcommit: e68df5b9c04b11c8f24d616f4e687fe4e773253c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/20/2018
ms.locfileid: "53651296"
---
# <a name="install-published-application---cask-data-application-platform-cdap"></a>Installeren van de gepubliceerde toepassing - Cask Data Application Platform (CDAP)

In dit artikel wordt beschreven hoe u wilt installeren en uitvoeren de [CDAP](https://cask.co/products/cdap/) gepubliceerd [Apache Hadoop](https://hadoop.apache.org/) toepassing voor Azure HDInsight. Zie voor een overzicht van het HDInsight-platform voor toepassingen en een lijst van beschikbare Independent Software Vendor (ISV) gepubliceerde toepassingen [Apache Hadoop-toepassingen van derden installeren](hdinsight-apps-install-applications.md). Zie voor instructies voor de installatie van uw eigen toepassing [Aangepaste HDInsight-toepassingen installeren](hdinsight-apps-install-custom-applications.md).

## <a name="about-cdap"></a>Over CDAP

Het ontwikkelen van toepassingen in Hadoop kan lastig zijn.  Er is een grote en groeiende aantal Hadoop-technologie-extensies even duren kan om te integreren. Gegevensstroom bewaken en verzamelen van metrische gegevens kunnen vereisen het bouwen van een afzonderlijke oplossing.

### <a name="how-does-cdap-help"></a>Hoe helpt CDAP?

Cask Data Application Platform (CDAP) is een integratieplatform voor Big Data. CDAP kunt u zich richten op het bouwen van toepassingen in plaats van op de onderliggende infrastructuur.

CDAP maakt gebruik van geavanceerde concepten en abstracties die bekend voor ontwikkelaars zijn. Deze abstracties verbergen van de complexiteit van interne systemen en herbruikbaarheid van oplossingen te stimuleren.

Een extensie van de CDAP aangeroepen [Cask Hydrator](https://cask.co/products/hydrator/) biedt een gebruikersinterface voor het ontwikkelen en beheren van gegevenspijplijnen. Een gegevenspijplijn bestaat uit verschillende *-invoegtoepassingen die taken uitvoeren, zoals gegevens ophalen, transformatie, analyse en na uitvoering van bewerkingen.

Elke invoegtoepassing CDAP heeft een goed gedefinieerde interface, zodat de evaluatie van verschillende technologieën is alleen een kwestie van het vervangen van een invoegtoepassing met een andere naam, zonder te hoeven te zien van de rest van de toepassing.

CDAP *pijplijnen* bieden een op hoog niveau pictogrammen stroom van de gegevens in uw toepassing. Deze visualisatie ziet u de end-to-end-stroom van de gegevens van gegevensopname, via de gegevenstransformaties en -analyses, en ten slotte naar een externe gegevens opslaan.

In het volgende voorbeeld van een pijplijn twitter-gegevens in realtime neemt vervolgens gefilterd op bepaalde tweets op basis van vooraf gedefinieerde criteria. De gegevenspijplijn tweet onbewerkte gegevens worden getransformeerd en projecten dat gegevens naar een beter leesbare indeling, klikt u vervolgens de tweets op basis van een set waarden worden gegroepeerd en schrijft de resultaten naar een HBase opslaan.

![CDAP pijplijn](./media/hdinsight-apps-install-cask/pipeline.png)

Deze end-to-end-pijplijn is gebouwd met behulp van de **Cask Hydrator UI**, met behulp van de invoegtoepassing-interface en slepen en neerzetten-functionaliteit op formulier-verbindingen tussen elke fase. U kunt isoleren en de functionaliteit van elke invoegtoepassing onafhankelijk van elkaar aanpassen. Met behulp van CDAP, kunnen vergelijkbare pijplijnen worden gebouwd en gevalideerd in uren. In de typische Hadoop-wereld is kan deze oplossingen maken enkele dagen duren.

CDAP biedt ook een uitbreiding met de naam [Cask Tracker](https://cask.co/products/tracker/) visueel trace gegevens zoals deze stromen via de toepassing. Cask Tracker voegt *gegevensbeheer* aan het systeem zodat gegevensassets formeel overal in de toepassing worden beheerd. U kunt elk gegevenspunt afkomst bijhouden, relevante metrische gegevens verzamelen en audittrail gegevens gedurende het gehele proces.

Hier volgt een voorbeeld van hoe de gegevens in de bovenstaande pijplijn worden doorgestuurd:

![CDAP bijhouden](./media/hdinsight-apps-install-cask/tracker.png)

## <a name="prerequisites"></a>Vereisten

Als u wilt deze app installeren op een nieuw HDInsight-cluster of een bestaand cluster, hebt u de volgende configuratie:

* Cluster-laag: Standard
* Cluster van het type: HBase
* Clusterversie: 3.4, 3.5

## <a name="install-the-cdap-published-application"></a>Gepubliceerde toepassing installeren de CDAP

Lees voor stapsgewijze instructies over het installeren van deze en andere beschikbare ISV-toepassingen [Hadoop-toepassingen van derden installeren](hdinsight-apps-install-applications.md).

## <a name="launch-cdap"></a>CDAP starten

1. Na de installatie, start u CDAP uit het cluster in Azure portal door te gaan naar de **instellingen** deelvenster, klikt u vervolgens selecteren **toepassingen** onder de **algemene** categorie. In het deelvenster geïnstalleerde Apps worden alle geïnstalleerde toepassingen.

    ![Geïnstalleerde CDAP app](./media/hdinsight-apps-install-cask/cdap-app.png)

2. Wanneer u CDAP selecteert, ziet u een koppeling naar de webpagina wordt weergegeven en HTTP-eindpunt en het pad van SSH-eindpunt. Selecteer de koppeling naar de webpagina.

3. Wanneer u hierom wordt gevraagd, voert u de beheerdersreferenties van uw cluster.

    ![Verificatie](./media/hdinsight-apps-install-cask/auth.png)

4. Na het aanmelden, ziet u de Cask CDAP GUI-startpagina.

    ![Cask GUI-startpagina](./media/hdinsight-apps-install-cask/gui.png)

5. Als u wilt de interface CDAP verkennen, klikt u op de **Cask markt** menu koppeling boven op de pagina.

    ![Cask markt koppeling](./media/hdinsight-apps-install-cask/cask-market.png)

6. Selecteer **toegang Log voorbeeld** in de lijst.

    ![Toegang tot Log-voorbeeld](./media/hdinsight-apps-install-cask/market-log-sample.png)

7. Klik op **Load** om te bevestigen.

    ![Klik op laden](./media/hdinsight-apps-install-cask/market-load.png)

8. Er is een weergave van de opgenomen voorbeeldgegevens weergegeven. Selecteer **volgende**.

    ![Voorbeeld van toegang tot Log - gegevens weergeven](./media/hdinsight-apps-install-cask/market-view-data.png)

9. Selecteer **Stream** als het Type bestemming, voert u de naam van een doel en selecteert **voltooien**.

    ![Voorbeeld van toegang tot Log - doelserver selecteren](./media/hdinsight-apps-install-cask/market-destination.png)

10. Nadat de datapack is geladen, selecteert u **Stream weergavedetails**.

    ![Datapack is geüpload](./media/hdinsight-apps-install-cask/market-view-details.png)

11. Als u metagegevens voor de naamruimte, schakelt **inschakelen** op het tabblad gebruik op de pagina met details van logboekbestanden.

    ![Toegang tot Log-voorbeeld - geladen - metagegevens inschakelen](./media/hdinsight-apps-install-cask/log-loaded.png)

12. Nadat de metagegevens is ingeschakeld, ziet u een grafiek controle-informatie voor bericht weergeven.

    ![Voorbeeld van toegang tot Log - metagegevens ingeschakeld](./media/hdinsight-apps-install-cask/log-metadata.png)

13. Het om logboekgegevens te verkennen, selecteert u de **verkennen** pictogram boven op de pagina.

    ![Logboek voorbeeld - verkennen](./media/hdinsight-apps-install-cask/log-explore.png)

14. U ziet een voorbeeld van een SQL-query. U kunt wijzigen als de gewenste, en selecteer vervolgens **Execute**.

    ![Toegang tot Log-voorbeeld - gegevensset met een query verkennen](./media/hdinsight-apps-install-cask/log-query.png)

15. Nadat de query is voltooid, selecteert u de **weergave** pictogram onder de acties-kolom.

    ![Voorbeeld van toegang tot Log - query voltooid weergeven](./media/hdinsight-apps-install-cask/log-query-view.png)

16. U ziet de queryresultaten.

    ![Voorbeeld van toegang tot Log - queryresultaten](./media/hdinsight-apps-install-cask/log-query-results.png)

## <a name="next-steps"></a>Volgende stappen

* [Cask documentatie](https://cask.co/resources/documentation/).
* [Aangepaste HDInsight-toepassingen installeren](hdinsight-apps-install-custom-applications.md): Informatie over het implementeren van een niet-gepubliceerde HDInsight-toepassing op HDInsight.
* [HDInsight-toepassingen publiceren](hdinsight-apps-publish-applications.md): Informatie over het publiceren van uw aangepaste HDInsight-toepassingen op Azure Marketplace.
* [MSDN: Een HDInsight-toepassing installeren](https://msdn.microsoft.com/library/mt706515.aspx): Meer informatie over het definiëren van HDInsight-toepassingen.
* [HDInsight Linux gebaseerde clusters aanpassen met Script Action](hdinsight-hadoop-customize-cluster-linux.md): Informatie over het gebruik scriptacties om extra toepassingen te installeren.
* [Lege edge-knooppunten gebruiken in HDInsight](hdinsight-apps-use-edge-node.md): Leer hoe u een lege edge-knooppunt gebruiken voor toegang tot HDInsight-clusters, en voor het testen en HDInsight-toepassingen hosten.
