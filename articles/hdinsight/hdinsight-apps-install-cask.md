---
title: Installatie gepubliceerde toepassing - Datameer - Azure HDInsight | Microsoft Docs
description: Installeren en gebruiken van de toepassing Datameer van derden Hadoop.
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
ms.openlocfilehash: 9eef1760b7cee3bbdf33122514669b38b0b4d9db
ms.sourcegitcommit: 9cdd83256b82e664bd36991d78f87ea1e56827cd
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/16/2018
---
# <a name="install-published-application---cask-data-application-platform-cdap"></a>Gepubliceerde toepassing - Cask gegevens toepassing Platform (CDAP) installeren

Dit artikel wordt beschreven hoe u wilt installeren en uitvoeren de [CDAP](http://cask.co/products/cdap/) gepubliceerde toepassing in Azure HDInsight Hadoop. Zie voor een overzicht van het HDInsight-toepassing-platform en een lijst van beschikbare Independent Software Vendors (ISV's) gepubliceerde toepassingen [Hadoop-toepassingen van derden installeren](hdinsight-apps-install-applications.md). Zie voor instructies voor de installatie van uw eigen toepassing [Aangepaste HDInsight-toepassingen installeren](hdinsight-apps-install-custom-applications.md).

## <a name="about-cdap"></a>Over CDAP

Ontwikkelen van toepassingen in Hadoop kan lastig zijn.  Er is een grote en toenemende aantal Hadoop technologie extensies even duren kan om te integreren. Gegevensstroom bewaken en verzamelen van metrische gegevens kunnen vereisen een afzonderlijke oplossing te bouwen.

### <a name="how-does-cdap-help"></a>Hoe helpt CDAP?

Cask gegevens toepassing Platform (CDAP) is een integratieplatform voor Big Data. CDAP kunt u zich kunt richten op het bouwen van toepassingen in plaats van op de onderliggende infrastructuur.

CDAP maakt gebruik van geavanceerde concepten en abstracties waarmee ontwikkelaars bekend zijn. Deze abstracties verbergen van de complexiteit van interne systemen en stimuleren hergebruik van oplossingen.

De extensie van een CDAP aangeroepen [Cask Hydrator](http://cask.co/products/hydrator/) biedt een gebruikersinterface voor het ontwikkelen en beheren van gegevenspijplijnen. Een pijplijn gegevens bestaat uit verschillende * plugins taken uit te voeren, zoals gegevens oplevert, transformatie, analyse en na uitgevoerde bewerkingen.

Elke invoegtoepassing CDAP heeft een goed gedefinieerde interface zodat evalueren van verschillende technologieën slechts een kwestie is van één invoegtoepassing vervangen door een andere gebruikersnaam, zonder de rest van de toepassing touch.

CDAP *pijplijnen* bieden een hoog niveau afbeeldingen stroom van de gegevens in uw toepassing. Deze visualisatie toont de end-to-end-stroom van de gegevens van de opname via de gegevenstransformaties en analyses, en tot slot tijdens het opslaan in een externe gegevensbron.

Het volgende voorbeeld van een pijplijn gegevens opgenomen twitter-gegevens in realtime en sommige tweets op basis van vooraf gedefinieerde criteria, worden uitgefilterd. De pijplijn gegevens transformeert tweet onbewerkte gegevens en projecten dat gegevens naar een beter leesbare indeling vervolgens de tweets volgens een reeks waarden groepen en de resultaten naar een HBase schrijft opslaan.

![CDAP-pipeline](./media/hdinsight-apps-install-cask/pipeline.png)

Deze end-to-end-pipeline gebouwd met behulp van de **Cask Hydrator UI**, met behulp van de invoegtoepassing-interface en slepen en neerzetten functionaliteit op formulier-verbindingen tussen elk stadium. U kunt isoleren en de functionaliteit aanpassen van elke invoegtoepassing onafhankelijk. Met behulp van CDAP worden vergelijkbare pijplijnen gebouwd en gevalideerd in uren. In de normale Hadoop-wereld is het construeren van dergelijke oplossingen mogelijk enkele dagen duren.

CDAP biedt ook een uitbreiding aangeroepen [Cask tracering](http://cask.co/products/tracker/) naar visueel traceergegevens die loopt via de toepassing. Tracering in cask voegt *gegevenskwaliteitsbeheer* aan het systeem zodat gegevensassets formeel worden beheerd in de toepassing. U kunt elk gegevenspunt afkomst bijhouden, relevante metrische gegevens te verzamelen en audittrail gegevens gedurende het hele proces.

Hier volgt een voorbeeld van hoe gegevens in de bovenstaande pijplijn stroomt:

![CDAP vastleggen](./media/hdinsight-apps-install-cask/tracker.png)

## <a name="prerequisites"></a>Vereisten

Om deze app installeren op een nieuwe HDInsight-cluster of een bestaand cluster, hebt u de volgende configuratie:

* Cluster-laag: standaard
* Cluster-type: HBase
* Cluster-versie: 3.4 en 3.5

## <a name="install-the-cdap-published-application"></a>Installeer de CDAP gepubliceerde toepassing

Lees voor stapsgewijze instructies over het installeren van deze en andere beschikbare ISV-toepassingen [Hadoop-toepassingen van derden installeren](hdinsight-apps-install-applications.md).

## <a name="launch-cdap"></a>CDAP starten

1. Na de installatie, start u CDAP van het cluster in Azure-portal door te gaan naar de **instellingen** deelvenster selecteren **toepassingen** onder de **algemene** categorie. In het deelvenster geïnstalleerde Apps worden alle geïnstalleerde toepassingen.

    ![Geïnstalleerde CDAP app](./media/hdinsight-apps-install-cask/cdap-app.png)

2. Als u CDAP selecteert, ziet u een koppeling naar de webpagina en HTTP-eindpunt en het pad van SSH-eindpunt. Selecteer de koppeling van de webpagina.

3. Voer desgevraagd uw beheerdersreferenties van het cluster.

    ![Verificatie](./media/hdinsight-apps-install-cask/auth.png)

4. Na het aanmelden, raadpleegt u de startpagina Cask CDAP GUI.

    ![Cask GUI-startpagina](./media/hdinsight-apps-install-cask/gui.png)

5. Als u wilt de interface CDAP verkennen, klikt u op de **Cask markt** menu koppeling boven op de pagina.

    ![Cask markt koppeling](./media/hdinsight-apps-install-cask/cask-market.png)

6. Selecteer **toegang logboek voorbeeld** uit de lijst.

    ![Toegang tot logboek voorbeeld](./media/hdinsight-apps-install-cask/market-log-sample.png)

7. Klik op **Load** om te bevestigen.

    ![Klik op laden](./media/hdinsight-apps-install-cask/market-load.png)

8. Een weergave van de opgenomen voorbeeldgegevens wordt weergegeven. Selecteer **volgende**.

    ![Toegang tot logboek voorbeeld - gegevens weergeven](./media/hdinsight-apps-install-cask/market-view-data.png)

9. Selecteer **stroom** als het Type bestemming, voer een naam voor de bestemming en selecteer vervolgens **voltooien**.

    ![Toegang tot logboek voorbeeld - doelserver selecteren](./media/hdinsight-apps-install-cask/market-destination.png)

10. Nadat de datapack is geladen, selecteert u **Details van de stroom weergeven**.

    ![Datapack geüpload](./media/hdinsight-apps-install-cask/market-view-details.png)

11. Als u metagegevens voor de naamruimte, schakelt **inschakelen** in het tabblad gebruik op de detailpagina Toegangslogboek.

    ![Toegang tot logboek voorbeeld - geladen - metagegevens inschakelen](./media/hdinsight-apps-install-cask/log-loaded.png)

12. Nadat de metagegevens is ingeschakeld, ziet u een grafiek om controle-informatie voor bericht weer te geven.

    ![Toegang tot logboek voorbeeld - metagegevens ingeschakeld](./media/hdinsight-apps-install-cask/log-metadata.png)

13. Als u wilt verkennen de logboekgegevens, selecteer de **verkennen** pictogram boven op de pagina.

    ![Toegang tot logboek steekproef - verkennen](./media/hdinsight-apps-install-cask/log-explore.png)

14. Er is een voorbeeld SQL-query. Gerust wijzigen als de gewenste, en selecteer vervolgens **Execute**.

    ![Toegang tot logboek voorbeeld - gegevensset met een query verkennen](./media/hdinsight-apps-install-cask/log-query.png)

15. Nadat de query is voltooid, selecteert u de **weergave** pictogram onder de kolom acties.

    ![Toegang tot logboek voorbeeld - query voltooid weergeven](./media/hdinsight-apps-install-cask/log-query-view.png)

16. U ziet de queryresultaten.

    ![Toegang tot logboek voorbeeld - queryresultaten](./media/hdinsight-apps-install-cask/log-query-results.png)

## <a name="next-steps"></a>Volgende stappen

* [Cask documentatie](http://cask.co/resources/documentation/).
* [Aangepaste HDInsight-toepassingen installeren](hdinsight-apps-install-custom-applications.md): informatie over het implementeren van een niet-gepubliceerde HDInsight-toepassing op HDInsight.
* [HDInsight-toepassingen publiceren](hdinsight-apps-publish-applications.md): informatie over het publiceren van aangepaste HDInsight-toepassingen in Azure Marketplace.
* [MSDN: een HDInsight-toepassing installeren](https://msdn.microsoft.com/library/mt706515.aspx): informatie over het definiëren van HDInsight-toepassingen.
* [Met behulp van de scriptactie Linux gebaseerde HDInsight-clusters aanpassen](hdinsight-hadoop-customize-cluster-linux.md): informatie over het gebruik van scriptactie om extra toepassingen te installeren.
* [Lege edge-knooppunten gebruiken in HDInsight](hdinsight-apps-use-edge-node.md): informatie over het gebruik van een leeg edge-knooppunt voor toegang tot HDInsight-clusters en voor het testen en die als host fungeert voor HDInsight-toepassingen.
