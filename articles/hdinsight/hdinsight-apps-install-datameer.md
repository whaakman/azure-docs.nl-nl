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
ms.openlocfilehash: 5008056ae2274d058706649f286b91b71feadc27
ms.sourcegitcommit: 9cdd83256b82e664bd36991d78f87ea1e56827cd
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/16/2018
ms.locfileid: "31401306"
---
# <a name="install-published-application---datameer"></a>Gepubliceerde toepassing - Datameer installeren

Dit artikel wordt beschreven hoe u wilt installeren en uitvoeren de [Datameer](https://www.datameer.com/) gepubliceerde toepassing in Azure HDInsight Hadoop. Zie voor een overzicht van het HDInsight-toepassing-platform en een lijst van beschikbare Independent Software Vendors (ISV's) gepubliceerde toepassingen [Hadoop-toepassingen van derden installeren](hdinsight-apps-install-applications.md). Zie voor instructies voor de installatie van uw eigen toepassing [Aangepaste HDInsight-toepassingen installeren](hdinsight-apps-install-custom-applications.md).

## <a name="about-datameer"></a>Over Datameer

Datameer is een systeemeigen toepassing voor Hadoop-platform, uitbreiden bestaande Azure HDInsight-mogelijkheden en bieden snel integratie, voorbereiding en analyse van gestructureerde en ongestructureerde gegevens. Datameer hebben toegang tot meer dan 70 bronnen en indelingen: gestructureerde, semi-gestructureerde en ongestructureerde. U kunt rechtstreeks gegevens uploaden of gebruik de koppelingen van hun unieke gegevens naar pull-gegevens op verzoek. Datameer van selfservice functionaliteit en bekend werkblad interface vermindert de complexiteit van Big Data-technologie en versnelt tijd om inzicht te. De interface werkblad biedt een eenvoudige mechanisme voor het invoeren van declaratieve formules die vervolgens worden vertaald naar Hadoop-taken geoptimaliseerd. Met Datameer en uw business intelligence (BI) en de Excel-vaardigheden, kunt u Hadoop in de cloud snel. Zie voor meer informatie de [Datameer documentatie](http://www.datameer.com/documentation/display/DAS50/Home?ls=Partners&lsd=Microsoft&c=Partners&cd=Microsoft).

## <a name="prerequisites"></a>Vereisten

Om deze app installeren op een nieuwe HDInsight-cluster of een bestaand cluster, hebt u de volgende configuratie:

* Cluster-laag: standaard
* Cluster-type: Hadoop
* Cluster-versie: 3.4

## <a name="install-the-datameer-published-application"></a>Installeer de Datameer gepubliceerde toepassing

Lees voor stapsgewijze instructies over het installeren van deze en andere beschikbare ISV-toepassingen [Hadoop-toepassingen van derden installeren](hdinsight-apps-install-applications.md).

## <a name="launch-datameer"></a>Datameer starten

1. Na de installatie, kunt u Datameer starten van het cluster in Azure-portal door te gaan naar de **instellingen** deelvenster vervolgens te klikken op **toepassingen** onder de **algemene** categorie . In het deelvenster geïnstalleerde Apps worden de geïnstalleerde toepassingen.

    ![Geïnstalleerde Datameer app](./media/hdinsight-apps-install-datameer/datameer-app.png)

2. Als u Datameer selecteert, ziet u een koppeling naar de webpagina en het pad van SSH-eindpunt. Selecteer de koppeling van de webpagina.

3. Op de eerste keer opstarten, er zijn twee licentieopties: ofwel een 14 dagen gratis, of een bestaande licentie activeert.

    ![Licentieopties](./media/hdinsight-apps-install-datameer/license.png)

4. Na het voltooien van de geselecteerde licentiegroepen optie wordt weergegeven met een aanmeldingsformulier. Voer de standaardreferenties weergegeven voordat het aanmeldingsformulier. Na het aanmelden, ga akkoord met de Softwareovereenkomst om door te gaan.

    ![Aanmelden](./media/hdinsight-apps-install-datameer/login.png)

De volgende stappen ziet een demonstratie "Hallo wereld".

1. [Het voorbeeld CSV downloaden](https://datameer.box.com/s/wzzw27za3agic4yjj8zrn6vfrph0ppnf).

2. Klik op de **+** boven op het dashboard Datameer ondertekenen en op **bestand laden**.

    ![Bestand uploaden](./media/hdinsight-apps-install-datameer/upload.png)

3. Blader in het dialoogvenster uploaden en selecteer de **Hello World.csv** dat u hebt gedownload. Zorg ervoor dat de **bestandstype** is ingesteld op CSV / TSV. Selecteer **Volgende**. Klik op **volgende** totdat u het einde van de wizard.

    ![Bestand uploaden](./media/hdinsight-apps-install-datameer/upload-browse.png)

4. Noem het bestand **Hallo wereld** onder een nieuwe map. Wijzig de naam van de nieuwe map als 'Demo'. Selecteer **Opslaan**.

    ![Opslaan](./media/hdinsight-apps-install-datameer/save.png)

5. Klik op de **+** zodra er meer en selecteer **werkmap** om een nieuwe werkmap voor de gegevens te maken.

    ![Werkmap toevoegen](./media/hdinsight-apps-install-datameer/add-workbook.png)

6. Vouw de **gegevens** map **uploads**, wordt de **Demo** map die u bij het opslaan van het bestand 'Hallo wereld' gemaakt. Selecteer **Hallo wereld** vormen van de lijst met bestanden en klik vervolgens op **gegevens toevoegen**.

    ![Opslaan](./media/hdinsight-apps-install-datameer/select-file.png)

7. Ziet u de gegevens geladen in een werkblad-interface. Als u een subset van de gegevens selecteren, schakelt de **Filter** knop op de werkbalk.

    ![Knop filteren](./media/hdinsight-apps-install-datameer/filter-button.png)

8. Selecteer in het dialoogvenster Filter toepassen de **stad** kolom **gelijk is aan** operator en type **Chicago** in het tekstvak filter. Controleer de **filter maken in een nieuw werkblad** selectievakje, selecteer vervolgens **Filter maken**.

    ![Filter toepassen](./media/hdinsight-apps-install-datameer/apply-filter.png)

9. Sla de werkmap door te klikken op **bestand**, klikt u vervolgens **opslaan**. Geef een naam, bijvoorbeeld 'Hello World werkmap'.

10. Krijgt u opties voor hoe en wanneer u wilt uitvoeren van de werkmap. Op dit moment laten alle opties op hun standaardwaarden en controleert u vervolgens de **Start berekening proces onmiddellijk na opslaan**, en selecteer **opslaan**.

    ![Werkmap opslaan](./media/hdinsight-apps-install-datameer/save-workbook.png)

11. Datameer biedt krachtige hulpprogramma's. Maak een Infographic voor weergave van gegevens. Selecteer de **+** ondertekenen boven op het dashboard en selecteer vervolgens **Infographic**.

    ![Infographic toevoegen](./media/hdinsight-apps-install-datameer/infographic-button.png)

12. Sleep een widget staafdiagram uit de lijst met widgets aan de linkerkant, zoals u in stap 1 in het volgende diagram. Vervolgens navigeren in de map Data onder de browser gegevens aan de rechterkant, vouw uw werkmap en vervolgens het werkblad toegevoegd met het filter (stap 2). Sleep de **naam** kolom boven in het staafdiagram en -neerzetten in de **Label** doel in te stellen van de werkmap naamkolom als de grafiek labelveld (stap 3).

    ![Infographic](./media/hdinsight-apps-install-datameer/infographic.png)

13. Om in te stellen leeftijd als Y-as van de grafiek, sleept u de **leeftijd** kolom in de grafiek **gegevens** veld.

    ![Infographic](./media/hdinsight-apps-install-datameer/infographic-age.png)

Gefeliciteerd! U hebt een visualisatie van uw gegevens gemaakt zonder een code te schrijven. U kunt nu variaties en aanvullende visualisaties verkennen.

## <a name="next-steps"></a>Volgende stappen

* [Datameer documentatie](http://www.datameer.com/documentation/display/DAS50/Home?ls=Partners&lsd=Microsoft&c=Partners&cd=Microsoft).
* [Aangepaste HDInsight-toepassingen installeren](hdinsight-apps-install-custom-applications.md): informatie over het implementeren van een niet-gepubliceerde HDInsight-toepassing op HDInsight.
* [HDInsight-toepassingen publiceren](hdinsight-apps-publish-applications.md): informatie over het publiceren van aangepaste HDInsight-toepassingen in Azure Marketplace.
* [MSDN: een HDInsight-toepassing installeren](https://msdn.microsoft.com/library/mt706515.aspx): informatie over het definiëren van HDInsight-toepassingen.
* [Met behulp van de scriptactie Linux gebaseerde HDInsight-clusters aanpassen](hdinsight-hadoop-customize-cluster-linux.md): informatie over het gebruik van scriptactie om extra toepassingen te installeren.
* [Lege edge-knooppunten gebruiken in HDInsight](hdinsight-apps-use-edge-node.md): informatie over het gebruik van een leeg edge-knooppunt voor toegang tot HDInsight-clusters en voor het testen en die als host fungeert voor HDInsight-toepassingen.
