---
title: Tez-gebruikersinterface gebruiken met HDInsight op basis van Windows - Azure | Microsoft Docs
description: Informatie over het gebruik van de UI Tez fouten opsporen in Tez-taken in HDInsight HDInsight op basis van Windows.
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
ms.assetid: a55bccb9-7c32-4ff2-b654-213a2354bd5c
ms.service: hdinsight
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 01/17/2017
ms.author: larryfr
ROBOTS: NOINDEX
ms.openlocfilehash: 3889fa1c3523eb0330cbe3b7640fd8590a5ceadf
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2017
---
# <a name="use-the-tez-ui-to-debug-tez-jobs-on-windows-based-hdinsight"></a>De UI Tez gebruiken om op te sporen Tez-taken in HDInsight op basis van Windows
De UI Tez is een webpagina die kunnen worden gebruikt om te begrijpen en foutopsporing van taken die Tez als de engine voor het uitvoeren op Windows gebaseerde HDInsight-clusters gebruiken. De UI Tez kunt u de taak als een grafiek van verbonden items visualiseren, Inzoomen op elk item en statistieken en logboekregistratie informatie ophalen.

> [!IMPORTANT]
> De stappen in dit document moet een HDInsight-cluster dat gebruik maakt van Windows. Linux is het enige besturingssysteem dat wordt gebruikt in HDInsight-versie 3.4 of hoger. Zie [HDInsight retirement on Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement) (HDInsight buiten gebruik gestel voor Windows) voor meer informatie.

## <a name="prerequisites"></a>Vereisten
* Een Windows gebaseerde HDInsight-cluster. Zie voor instructies over het maken van een nieuw cluster [aan de slag met HDInsight op basis van Windows](hdinsight-hadoop-tutorial-get-started-windows.md).

  > [!IMPORTANT]
  > De UI Tez is alleen beschikbaar op Windows gebaseerde HDInsight-clusters die zijn gemaakt na 8 februari 2016.
  >
  >
* Een extern bureaublad op basis van Windows-client.

## <a name="understanding-tez"></a>Understanding Tez
Tez is een uitbreidbaar framework voor gegevensverwerking in Hadoop. deze groter snelheden dan traditionele MapReduce-verwerking biedt. Voor Windows gebaseerde HDInsight-clusters is een optionele-engine die u voor Hive inschakelen kunt met behulp van de volgende opdracht als onderdeel van uw Hive-query:

    set hive.execution.engine=tez;

Wanneer het werk is verzonden naar Tez, wordt een omgeleid acyclische grafiek (DAG) die worden beschreven de volgorde van de uitvoering van de acties die vereist zijn voor de taak gemaakt. Afzonderlijke acties hoekpunten worden genoemd, en een onderdeel van de algehele taak uitvoeren. De werkelijke uitvoering van het werk dat is beschreven door een hoekpunt een taak wordt aangeroepen en kan worden verdeeld over meerdere knooppunten in het cluster.

### <a name="understanding-the-tez-ui"></a>Inzicht in de gebruikersinterface Tez
De UI Tez is dat een webpagina biedt informatie over de processen die worden uitgevoerd of hebben eerder is uitgevoerd met behulp van Tez. Hiermee kunt u bekijken van de DAG die worden gegenereerd door de Tez, hoe is verdeeld over de clusters, prestatiemeteritems gebruikt, zoals geheugen die wordt gebruikt door taken en hoekpunten en informatie over de fout. Dit kan nuttige informatie in de volgende scenario's bieden:

* Bewaking van langlopende verwerkt, wordt de voortgang van de kaart weergeven en taken te verminderen.
* Analyse van historische gegevens voor de geslaagde of mislukte processen voor meer informatie over hoe de verwerking kan worden verbeterd of waarom is mislukt.

## <a name="generate-a-dag"></a>Genereren van een DAG
De UI Tez bevat alleen gegevens als een taak die is gebruikt de Tez-engine wordt uitgevoerd of is uitgevoerd in het verleden. Eenvoudige Hive-query's kunnen doorgaans worden opgelost zonder Tez, maar complexe query's filteren, groeperen, rangschikken, joins, enz. meestal Tez is vereist.

Gebruik de volgende stappen uitvoeren van een Hive-query die wordt uitgevoerd met behulp van Tez.

1. Navigeer in een webbrowser naar https://CLUSTERNAME.azurehdinsight.net, waarbij **CLUSTERNAME** is de naam van uw HDInsight-cluster.
2. Selecteer in het menu aan de bovenkant van de pagina de **Hive-Editor**. Hiermee wordt een pagina met de volgende voorbeeldquery weergegeven.

        Select * from hivesampletable

    Wissen van de voorbeeldquery en vervang deze door de volgende.

        set hive.execution.engine=tez;
        select market, state, country from hivesampletable where deviceplatform='Android' group by market, country, state;
3. Selecteer de **indienen** knop. De **taak sessie** sectie aan de onderkant van de pagina wordt de status van de query weergegeven. Zodra de status gewijzigd in **voltooid**, selecteer de **Details weergeven** koppeling om de resultaten weer te geven. De **Taakuitvoer** moet er ongeveer als volgt:

        en-GB   Hessen      Germany
        en-GB   Kingston    Jamaica
        en-GB   Nairobi Area    Kenya

## <a name="use-the-tez-ui"></a>De Tez-gebruikersinterface gebruiken
> [!NOTE]
> De UI Tez is alleen beschikbaar vanaf het bureaublad van de clusterknooppunten head, dus moet u extern bureaublad verbinding maken met de hoofdknooppunten.
>
>

1. Van de [Azure-portal](https://portal.azure.com), selecteer uw HDInsight-cluster. Vanaf de bovenkant van de HDInsight-blade, selecteer de **extern bureaublad** pictogram. Hiermee wordt de blade voor extern bureaublad weergegeven

    ![Pictogram voor extern bureaublad](./media/hdinsight-debug-tez-ui/remotedesktopicon.png)
2. Selecteer in de blade extern bureaublad **Connect** verbinding maken met het hoofdknooppunt van het cluster. Wanneer u wordt gevraagd, gebruikt u de cluster extern bureaublad-gebruikersnaam en wachtwoord om de verbinding te verifiëren.

    ![Pictogram externe bureaublad](./media/hdinsight-debug-tez-ui/remotedesktopconnect.png)

   > [!NOTE]
   > Als u extern bureaublad-verbinding niet hebt ingeschakeld, Geef een gebruikersnaam, wachtwoord en vervaldatum en selecteer vervolgens **inschakelen** extern bureaublad inschakelen. Zodra deze is ingeschakeld, moet u de vorige stappen gebruiken om verbinding te.
   >
   >
3. Eenmaal zijn verbonden, opent u Internet Explorer op het externe bureaublad, selecteert u het pictogram tandwielpictogram in de rechterbovenhoek van de browser en selecteer vervolgens **instellingen voor de Compatibiliteitsweergave**.
4. Van de onderkant van **instellingen voor de Compatibiliteitsweergave**, schakel het selectievakje voor **intranetsites worden weergegeven in de Compatibiliteitsweergave** en **gebruik Microsoft hardware compatibility List**, en Selecteer vervolgens **sluiten**.
5. Ga in Internet Explorer naar tezui-http://headnodehost:8188 / #/. Hiermee wordt de Tez UI weergegeven

    ![Tez-gebruikersinterface](./media/hdinsight-debug-tez-ui/tezui.png)

    Wanneer de Tez UI wordt geladen, ziet u een lijst van dag's die momenteel worden uitgevoerd, of zijn uitgevoerd op het cluster. De standaardweergave omvat de Dag Name, -Id, Submitter, Status, begintijd, eindtijd, duur, toepassings-ID en wachtrij. Meer kolommen kunnen worden toegevoegd aan de rechterkant van de pagina met behulp van het pictogram tandwielpictogram.

    Als u slechts één vermelding hebt, zal zijn voor de query die u in de vorige sectie hebt uitgevoerd. Als u meerdere vermeldingen hebt, kunt u zoeken door te voeren zoekcriteria in de velden van de dag's vervolgens bereikt **Enter**.
6. Selecteer de **Dag Name** voor de meest recente DAG-vermelding. Hiermee wordt informatie over de DAG, evenals de optie voor het downloaden van een zip van JSON-bestanden die informatie over de DAG bevatten weergegeven.

    ![Details van de DAG](./media/hdinsight-debug-tez-ui/dagdetails.png)
7. Boven de **DAG Details** zijn enkele koppelingen die kan worden gebruikt om informatie weer over de DAG.

   * **DAG tellers** tellers geeft informatie weer voor deze DAG.
   * **Grafische weergave** een grafische weergave van deze DAG.
   * **Alle hoekpunten** geeft een overzicht van de hoekpunten in deze DAG.
   * **Alle taken** geeft een overzicht van de taken voor alle hoekpunten in deze DAG.
   * **Alle TaskAttempts** geeft informatie weer over de pogingen tot het uitvoeren van taken voor deze DAG.

     > [!NOTE]
     > Als u de kolomweergave voor hoekpunten, taken en TaskAttempts schuift, zoals u ziet dat er koppelingen naar de weergave zijn **tellers** en **weergeven of downloaden van logboeken** voor elke rij.
     >
     >

     Als er een fout is opgetreden met de taak, wordt de status mislukt, met koppelingen naar informatie over de mislukte taak weergegeven in de Details van de DAG. Diagnostische gegevens weergegeven onder de details van de DAG.
8. Selecteer **grafische weergave**. U ziet nu een grafische weergave van de DAG. U kunt de muis plaatsen op elke hoekpunt in de weergave om informatie daarover weer te geven.

    ![Grafische weergave](./media/hdinsight-debug-tez-ui/dagdiagram.png)
9. Te klikken op een hoekpunt laadt de **hoekpunt Details** voor dat het item. Klik op de **kaart 1** hoekpunt om details van dit item weer te geven. Selecteer **bevestigen** om te bevestigen dat de navigatie.

    ![Hoekpunt details](./media/hdinsight-debug-tez-ui/vertexdetails.png)
10. Houd er rekening mee dat u hebt nu koppelingen aan de bovenkant van de pagina die betrekking hebben op de hoekpunten en taken.

    > [!NOTE]
    > U kunt ook op deze pagina binnenkomen gaat u terug naar **DAG Details**, selecteren **hoekpunt Details**, en vervolgens te klikken op de **kaart 1** hoekpunt.
    >
    >

    * **Hoekpunt tellers** teller geeft informatie weer voor deze hoekpunt.
    * **Taken** taken voor dit hoekpunt weergegeven.
    * **Taak pogingen** geeft informatie weer over de pogingen tot het uitvoeren van taken voor dit hoekpunt.
    * **Gegevensbronnen & Sinks** gegevensbronnen weergeeft en de sinks voor dit hoekpunt.

      > [!NOTE]
      > Als met de vorige menu, kunt u de kolom weergeven voor taken, taak pogingen, en bronnen & Sinks__ koppelingen naar meer informatie voor elk item weergeven schuiven.
      >
      >
11. Selecteer **taken**, en selecteer vervolgens het item met de naam **00_000000**. Hiermee wordt weergegeven **taakdetails** voor deze taak. In dit scherm kunt u weergeven **taak tellers** en **taak pogingen**.

    ![Taakdetails](./media/hdinsight-debug-tez-ui/taskdetails.png)

## <a name="next-steps"></a>Volgende stappen
U hebt geleerd hoe u de weergave Tez, meer informatie over [met behulp van Hive in HDInsight](hdinsight-use-hive.md).

Zie voor meer technische informatie over Tez, de [Tez-pagina op Hortonworks](http://hortonworks.com/hadoop/tez/).
