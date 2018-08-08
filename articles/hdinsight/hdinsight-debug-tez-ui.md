---
title: Tez-gebruikersinterface gebruiken met HDInsight op basis van een Windows - Azure
description: Informatie over het gebruik van de UI Tez fouten opsporen in Tez-taken in HDInsight HDInsight op basis van Windows.
services: hdinsight
author: jasonwhowell
editor: jasonwhowell
ms.service: hdinsight
ms.topic: conceptual
ms.date: 01/17/2017
ms.author: jasonh
ROBOTS: NOINDEX
ms.openlocfilehash: f54cc60f9490b8a5ca1872a290c3895ea8b0c5e4
ms.sourcegitcommit: 1f0587f29dc1e5aef1502f4f15d5a2079d7683e9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/07/2018
ms.locfileid: "39590898"
---
# <a name="use-the-tez-ui-to-debug-tez-jobs-on-windows-based-hdinsight"></a>De UI Tez gebruiken om op te sporen Tez-taken in HDInsight op basis van Windows
De UI Tez kan worden gebruikt om op te sporen Hive-taken die Tez als de engine voor het uitvoeren gebruiken. De Tez UI worden gevisualiseerd met de taak een grafiek van verbonden items kunt inzoomen op elk item en statistieken en waardevolle informatie ophalen.

> [!IMPORTANT]
> De stappen in dit document moet een HDInsight-cluster dat gebruik maakt van Windows. Linux is het enige besturingssysteem dat wordt gebruikt in HDInsight-versie 3.4 of hoger. Zie [HDInsight retirement on Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement) (HDInsight buiten gebruik gestel voor Windows) voor meer informatie.

## <a name="prerequisites"></a>Vereisten
* Een Windows-gebaseerde HDInsight-cluster. Zie voor stappen voor het maken van een nieuw cluster [aan de slag met HDInsight op basis van Windows](hdinsight-hadoop-tutorial-get-started-windows.md).

  > [!IMPORTANT]
  > De Tez UI is alleen beschikbaar op Windows gebaseerde HDInsight-clusters die na 8 februari 2016 zijn gemaakt.
  >
  >
* Een extern bureaublad op basis van een Windows-client.

## <a name="understanding-tez"></a>Inzicht in Tez
Tez is een uitbreidbaar framework voor gegevensverwerking in Hadoop en biedt meer snelheden worden bereikt dan traditionele MapReduce-verwerking. U kunt Tez inschakelen door de volgende tekst op te nemen als onderdeel van een Hive-query:

    set hive.execution.engine=tez;

Tez maakt een omgeleid Acyclic Graph (DAG) die de bestelling van de uitvoering van de acties die zijn vereist door de taak wordt beschreven. Afzonderlijke acties hoekpunten worden genoemd, en een deel van de algemene taak wordt uitgevoerd. De daadwerkelijke uitvoering van het werk dat wordt beschreven door een hoekpunt een taak wordt genoemd, en kan worden verdeeld over meerdere knooppunten in het cluster.

### <a name="understanding-the-tez-ui"></a>Informatie over de Tez-gebruikersinterface
De UI Tez is dat een webpagina bevat informatie over de processen die gebruikmaken van Tez. Deze bieden mogelijk nuttige informatie in de volgende scenario's:

* Bewaking van langlopende processen, de voortgang van de kaart bekijken en aantal taken te verkleinen.
* Analyse van historische gegevens voor geslaagde of mislukte processen voor meer informatie over hoe de verwerking kan worden verbeterd of waarom is mislukt.

## <a name="generate-a-dag"></a>Genereren van een DAG
De UI Tez bevat gegevens als een taak die u gebruikt de Tez-engine wordt uitgevoerd of is in het verleden uitgevoerd. Eenvoudige Hive-query's kunnen doorgaans worden opgelost zonder gebruik van Tez. Complexere query's filteren, vereisen Tez groeperen, bestellen, joins, enzovoort.

Gebruik de volgende stappen uit om uit te voeren van een Hive-query die gebruikmaakt van Tez.

1. Navigeer in een webbrowser naar https://CLUSTERNAME.azurehdinsight.net, waarbij **CLUSTERNAME** is de naam van uw HDInsight-cluster.
2. Selecteer in het menu aan de bovenkant van de pagina, de **Hive-Editor**. U ziet nu een pagina met het volgende voorbeeld van een query.

        Select * from hivesampletable

    Wissen van de voorbeeldquery en vervang deze door de volgende.

        set hive.execution.engine=tez;
        select market, state, country from hivesampletable where deviceplatform='Android' group by market, country, state;
3. Selecteer de **indienen** knop. De **taak sessie** sectie aan de onderkant van de pagina wordt de status van de query weergegeven. Zodra de status gewijzigd in **voltooid**, selecteer de **Details weergeven** koppeling om de resultaten weer te geven. De **Taakuitvoer** zijn vergelijkbaar met het volgende:

        en-GB   Hessen      Germany
        en-GB   Kingston    Jamaica
        en-GB   Nairobi Area    Kenya

## <a name="use-the-tez-ui"></a>De Tez-gebruikersinterface gebruiken
> [!NOTE]
> De Tez UI is alleen beschikbaar vanuit het bureaublad van de cluster-hoofdknooppunten, dus moet u extern bureaublad verbinding maken met de hoofdknooppunten.
>
>

1. Uit de [Azure-portal](https://portal.azure.com), selecteer uw HDInsight-cluster. Vanaf de bovenkant van de HDInsight-blade, selecteer de **extern bureaublad** pictogram. Deze koppeling geeft de extern bureaublad-blade

    ![Pictogram op het externe bureaublad](./media/hdinsight-debug-tez-ui/remotedesktopicon.png)
2. Selecteer in de blade extern bureaublad **Connect** verbinding maken met het hoofdknooppunt van het cluster. Wanneer u hierom wordt gevraagd, gebruikt u het cluster extern bureaublad-gebruikersnaam en wachtwoord om de verbinding te verifiëren.

    ![Extern bureaublad verbinding maken met het pictogram](./media/hdinsight-debug-tez-ui/remotedesktopconnect.png)

   > [!NOTE]
   > Als u extern bureaublad-verbinding niet hebt ingeschakeld, Geef een gebruikersnaam, wachtwoord en de datum van afloop en selecteer vervolgens **inschakelen** extern bureaublad inschakelen. Zodra deze is ingeschakeld, gebruikt u de vorige stappen om verbinding te maken.
   >
   >
3. Eenmaal verbinding hebben, opent u Internet Explorer op de extern bureaublad, selecteer het tandwielpictogram in de rechterbovenhoek van de browser en selecteer vervolgens **instellingen voor de Compatibiliteitsweergave**.
4. Vanaf de onderkant van **instellingen voor de Compatibiliteitsweergave**, schakel het selectievakje voor **intranetsites worden weergegeven in de Compatibiliteitsweergave** en **gebruik Microsoft hardware compatibility List**, en Selecteer vervolgens **sluiten**.
5. In Internet Explorer en blader naar http://headnodehost:8188/tezui/#/. U ziet nu de Tez UI

    ![Tez-gebruikersinterface](./media/hdinsight-debug-tez-ui/tezui.png)

    Wanneer de Tez UI wordt geladen, ziet u een lijst van dag's die momenteel worden uitgevoerd, of zijn uitgevoerd op het cluster. De standaardweergave bevat de DAG Name, -Id, Inzender, Status, begintijd, eindtijd, duur, toepassings-ID en wachtrij. Meer kolommen kunnen worden toegevoegd met behulp van het tandwielpictogram rechts van de pagina.

    Als u slechts één vermelding hebt, is het voor de query die u in de vorige sectie hebt uitgevoerd. Als u meerdere vermeldingen hebt, kunt u zoeken door zoekcriteria in de velden boven de dag's vervolgens druk **Enter**.
6. Selecteer de **Dag Name** voor de meest recente vermelding van de DAG. Deze koppeling geeft informatie over de DAG, evenals de optie voor het downloaden van een zip van JSON-bestanden die informatie over de DAG bevatten.

    ![Details van de DAG](./media/hdinsight-debug-tez-ui/dagdetails.png)
7. Boven de **DAG Details** vindt u enkele koppelingen die kunnen worden gebruikt om informatie over de DAG weer te geven.

   * **DAG tellers** tellers geeft informatie weer voor deze DAG.
   * **Grafische weergave** een grafische weergave van deze DAG.
   * **Alle hoekpunten** geeft een lijst van de hoekpunten in deze DAG.
   * **Alle taken** geeft een lijst van de taken van alle hoekpunten in deze DAG.
   * **Alle TaskAttempts** geeft informatie weer over de pogingen tot uitvoering van taken voor deze DAG.

     > [!NOTE]
     > Als u de kolom wordt weergegeven voor hoekpunten, taken en TaskAttempts schuift, ziet u dat er vindt u koppelingen om weer te geven **tellers** en **weergeven of downloaden van logboeken** voor elke rij.
     >
     >

     Als er een fout opgetreden met de taak is, wordt in de Details van de DAG status is mislukt, samen met koppelingen naar informatie over de mislukte taak weergegeven. Diagnostische gegevens wordt weergegeven onder de details van de DAG.
8. Selecteer **grafische weergave**. U ziet nu een grafische weergave van de DAG. U kunt de muis over elk hoekpunt dat in de weergave om informatie erover weer te plaatsen.

    ![Grafische weergave](./media/hdinsight-debug-tez-ui/dagdiagram.png)
9. Te klikken op een hoekpunt laadt de **hoekpunt Details** voor dat artikel. Klik op de **kaart 1** hoekpunt om details van dit item weer te geven. Selecteer **bevestigen** om te bevestigen van de navigatiebalk.

    ![Details van een hoekpunt](./media/hdinsight-debug-tez-ui/vertexdetails.png)
10. Houd er rekening mee dat u hebt nu koppelingen aan het begin van de pagina die betrekking hebben op de hoekpunten en taken.

    > [!NOTE]
    > U kunt ook op deze pagina bereikt door te gaan terug naar **DAG Details**, Klik daarvoor **hoekpunt Details**, en selecteer vervolgens de **kaart 1** hoekpunt.
    >
    >

    * **Hoekpunt tellers** teller geeft informatie weer voor deze hoekpunt.
    * **Taken** taken voor deze hoekpunt worden weergegeven.
    * **Taak pogingen** geeft informatie weer over pogingen tot uitvoering van taken voor deze hoekpunt.
    * **Bronnen en Sinks** weergegeven van gegevensbronnen en sinks voor deze hoekpunt.

      > [!NOTE]
      > Als met het vorige menu kunt u de kolom wordt weergegeven voor taken, taak pogingen, en bronnen en Sinks__ koppelingen naar meer informatie voor elk item weergeven schuiven.
      >
      >
11. Selecteer **taken**, en selecteer vervolgens het item met de naam **00_000000**. Deze koppeling geeft **taakdetails** voor deze taak. In dit scherm kunt u weergeven **taak tellers** en **taak pogingen**.

    ![Taakgegevens](./media/hdinsight-debug-tez-ui/taskdetails.png)

## <a name="next-steps"></a>Volgende stappen
Nu hebt u geleerd hoe u de Tez-weergave, meer informatie over [met behulp van Hive in HDInsight](hadoop/hdinsight-use-hive.md).

Zie voor meer technische informatie over Tez, de [Tez-pagina op Hortonworks](http://hortonworks.com/hadoop/tez/).
