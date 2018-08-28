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
ms.openlocfilehash: a428bb7bc9cc6a6a2e28989271ad1998700438cf
ms.sourcegitcommit: f6e2a03076679d53b550a24828141c4fb978dcf9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/27/2018
ms.locfileid: "43107745"
---
# <a name="install-published-application---datameer"></a>Installeren van de gepubliceerde toepassing - Datameer

In dit artikel wordt beschreven hoe u wilt installeren en uitvoeren de [Datameer](https://www.datameer.com/) gepubliceerd op Azure HDInsight Hadoop-toepassing. Zie voor een overzicht van het HDInsight-platform voor toepassingen en een lijst van beschikbare Independent Software Vendor (ISV) gepubliceerde toepassingen [Hadoop-toepassingen van derden installeren](hdinsight-apps-install-applications.md). Zie voor instructies voor de installatie van uw eigen toepassing [Aangepaste HDInsight-toepassingen installeren](hdinsight-apps-install-custom-applications.md).

## <a name="about-datameer"></a>Over Datameer

Datameer is een systeemeigen toepassing voor de Hadoop-platform uitbreiden bestaande mogelijkheden voor Azure HDInsight en quick integration, voorbereiding en analyses van gestructureerde en ongestructureerde gegevens te leveren. Datameer hebben toegang tot meer dan 70 gegevensbronnen en indelingen: gestructureerde, semi-gestructureerde en ongestructureerde. U kunt rechtstreeks gegevens uploaden of gebruiken van hun unieke gegevenskoppelingen naar pull-gegevens op aanvraag. Datameer van self-servicefunctionaliteit en vertrouwde spreadsheetinterface vermindert de complexiteit van Big Data-technologie en versnelt de tijd tot insight. De spreadsheetinterface biedt een eenvoudig mechanisme voor het invoeren van declaratieve formules die vervolgens omgezet worden in het Hadoop-taken geoptimaliseerd. Met Datameer en uw business intelligence (BI) en het Excel-vaardigheden, kunt u Hadoop in de cloud snel. Zie voor meer informatie de [Datameer documentatie](http://www.datameer.com/documentation/display/DAS50/Home?ls=Partners&lsd=Microsoft&c=Partners&cd=Microsoft).

## <a name="prerequisites"></a>Vereisten

Als u wilt deze app installeren op een nieuw HDInsight-cluster of een bestaand cluster, hebt u de volgende configuratie:

* Cluster-laag: Standard
* Clustertype: Hadoop
* Clusterversie: 3.4

## <a name="install-the-datameer-published-application"></a>Gepubliceerde toepassing installeren de Datameer

Lees voor stapsgewijze instructies over het installeren van deze en andere beschikbare ISV-toepassingen [Hadoop-toepassingen van derden installeren](hdinsight-apps-install-applications.md).

## <a name="launch-datameer"></a>Datameer starten

1. Na de installatie, kunt u Datameer starten vanuit het cluster in Azure portal door te gaan naar de **instellingen** deelvenster, klikt u vervolgens te klikken op **toepassingen** onder de **algemene** categorie . Het deelvenster met geïnstalleerde Apps worden de geïnstalleerde toepassingen.

    ![Geïnstalleerde Datameer app](./media/hdinsight-apps-install-datameer/datameer-app.png)

2. Wanneer u Datameer selecteert, ziet u een koppeling naar de webpagina wordt weergegeven en het pad van SSH-eindpunt. Selecteer de koppeling naar de webpagina.

3. Op de eerste keer opstarten wordt er zijn twee licentieopties: hetzij een 14-daagse proefversie, of een bestaande licentie activeren.

    ![Licentieopties](./media/hdinsight-apps-install-datameer/license.png)

4. Na het voltooien van de optie geselecteerde licentiegroepen, worden weergegeven met een aanmeldingsformulier. Voer de standaardreferenties voorafgaand aan het aanmeldingsformulier weergegeven. Na het aanmelden, accepteer de Softwareovereenkomst om door te gaan.

    ![Aanmelden](./media/hdinsight-apps-install-datameer/login.png)

De volgende stappen ziet een demonstratie "Hallo wereld".

1. [De voorbeeld-CSV downloaden](https://datameer.box.com/s/wzzw27za3agic4yjj8zrn6vfrph0ppnf).

2. Klik op de **+** zich boven op het dashboard Datameer en klikt u op **bestand uploaden**.

    ![Bestand uploaden](./media/hdinsight-apps-install-datameer/upload.png)

3. Blader in het dialoogvenster voor het uploaden, en selecteer de **Hello World.csv** dat u hebt gedownload. Zorg ervoor dat de **bestandstype** is ingesteld op CSV / TSV. Selecteer **Volgende**. Klik op **volgende** totdat u het einde van de wizard hebt bereikt.

    ![Bestand uploaden](./media/hdinsight-apps-install-datameer/upload-browse.png)

4. Noem het bestand **Hello World** onder een nieuwe map. Wijzig de naam van de nieuwe map als 'Demo'. Selecteer **Opslaan**.

    ![Opslaan](./media/hdinsight-apps-install-datameer/save.png)

5. Klik op de **+** zodra er meer en selecteer **werkmap** om een nieuwe werkmap voor de gegevens te maken.

    ![Werkmap toevoegen](./media/hdinsight-apps-install-datameer/add-workbook.png)

6. Vouw de **gegevens** map **uploads**, dan zal de **Demo** map die u hebt gemaakt bij het opslaan van het bestand "Hallo wereld". Selecteer **Hello World** vormen van de lijst met bestanden en klik vervolgens op **gegevens toevoegen**.

    ![Opslaan](./media/hdinsight-apps-install-datameer/select-file.png)

7. Ziet u de gegevens geladen in een werkblad-interface. Als u wilt een subset van de gegevens selecteert, selecteert u de **Filter** knop op de werkbalk.

    ![Knop filteren](./media/hdinsight-apps-install-datameer/filter-button.png)

8. Selecteer in het dialoogvenster Filter toepassen de **plaats** kolom **gelijk is aan** operator en het type **Chicago** in het filtertekstvak. Controleer de **filter maken in een nieuw blad** selectievakje, selecteer vervolgens **Filter maken**.

    ![Filter toepassen](./media/hdinsight-apps-install-datameer/apply-filter.png)

9. De werkmap opslaan door te klikken op **bestand**, klikt u vervolgens **opslaan**. Geef een naam, zoals 'Hello World-werkmap'.

10. U krijgt opties voor hoe en wanneer moet worden uitgevoerd van de werkmap. Nu, laat u alle van de opties op de standaardwaarden en schakel de **Start berekening proces onmiddellijk na opslaan**, en selecteer **opslaan**.

    ![Werkmap opslaan](./media/hdinsight-apps-install-datameer/save-workbook.png)

11. Datameer biedt krachtige visualisatie-hulpprogramma's. Als u wilt de gegevens worden weergegeven, maakt u een Infographic. Selecteer de **+** zich boven op het dashboard en selecteer vervolgens **Infographic**.

    ![Infographic toevoegen](./media/hdinsight-apps-install-datameer/infographic-button.png)

12. Sleep een widget staafdiagram in de lijst van widgets aan de linkerkant, zoals wordt weergegeven in stap 1 in het volgende diagram. Vervolgens, navigeren door de map gegevens in de browser gegevens aan de rechterkant, uw werkmap, vouw vervolgens het werkblad die u hebt toegevoegd met het filter (stap 2). Sleep de **naam** kolom boven in het staafdiagram en neerzetten in de **Label** doel het instellen van de kolom naam van de werkmap als van de grafiek labelveld (stap 3).

    ![Infographic](./media/hdinsight-apps-install-datameer/infographic.png)

13. Om in te stellen leeftijd als Y-as van de grafiek, sleept u de **leeftijd** kolom in de grafiek **gegevens** veld.

    ![Infographic](./media/hdinsight-apps-install-datameer/infographic-age.png)

Gefeliciteerd! U hebt een visualisatie van uw gegevens gemaakt zonder een code te schrijven. U kunt nu variaties en extra visualisaties verkennen.

## <a name="next-steps"></a>Volgende stappen

* [Datameer documentatie](http://www.datameer.com/documentation/display/DAS50/Home?ls=Partners&lsd=Microsoft&c=Partners&cd=Microsoft).
* [Aangepaste HDInsight-toepassingen installeren](hdinsight-apps-install-custom-applications.md): informatie over het implementeren van een niet-gepubliceerde HDInsight-toepassing op HDInsight.
* [HDInsight-toepassingen publiceren](hdinsight-apps-publish-applications.md): informatie over het publiceren van aangepaste HDInsight-toepassingen in Azure Marketplace.
* [MSDN: een HDInsight-toepassing installeren](https://msdn.microsoft.com/library/mt706515.aspx): informatie over het definiëren van HDInsight-toepassingen.
* [HDInsight Linux gebaseerde clusters aanpassen met Script Action](hdinsight-hadoop-customize-cluster-linux.md): informatie over het gebruik scriptacties om extra toepassingen te installeren.
* [Lege edge-knooppunten gebruiken in HDInsight](hdinsight-apps-use-edge-node.md): informatie over het gebruik van een lege edge-knooppunt voor toegang tot HDInsight-clusters, en voor het testen en HDInsight-toepassingen hosten.
