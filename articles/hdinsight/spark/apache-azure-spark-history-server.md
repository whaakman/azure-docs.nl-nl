---
title: Gebruik uitgebreide Spark geschiedenis Server fouten opsporen en diagnosticeren van toepassingen in Spark - Azure HDInsight
description: Gebruik uitgebreide Spark geschiedenis Server fouten opsporen en diagnosticeren van toepassingen in Spark - Azure HDInsight.
services: hdinsight
ms.service: hdinsight
author: jejiang
ms.author: jejiang
ms.reviewer: jasonh
ms.custom: hdinsightactive,hdiseo17may2017
ms.topic: conceptual
ms.date: 09/14/2018
ms.openlocfilehash: a896c949e1f05a5d9ee179fa475150ad8da34283
ms.sourcegitcommit: 295babdcfe86b7a3074fd5b65350c8c11a49f2f1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/27/2018
ms.locfileid: "53792778"
---
# <a name="use-extended-apache-spark-history-server-to-debug-and-diagnose-apache-spark-applications"></a>Uitgebreide geschiedenis Server van de Apache Spark op fouten opsporen en diagnosticeren van Apache Spark-toepassingen gebruiken

Dit artikel bevat instructies over het gebruik van uitgebreide Apache Spark geschiedenis Server voor het fouten opsporen en diagnosticeren van Spark-toepassingen is voltooid en wordt uitgevoerd. De extensie bevat het tabblad data en graph tabbladen en diagnose. Op de **gegevens** tabblad gebruikers kunnen de gegevens voor invoer en uitvoer van de Spark-taak controleren. Op de **Graph** tabblad gebruikers kunnen de gegevens stromen af te handelen van de taakgrafiek controleren. Op de **diagnose** tabblad gebruiker kan verwijzen naar **gegevens scheeftrekken**, **Tijdverschilbereik** en **Executor gebruiksanalyse**.

## <a name="get-access-to-apache-spark-history-server"></a>Toegang krijgen tot de Server voor Apache Spark-geschiedenis

Apache Spark geschiedenis-Server is de web-UI voor Spark-toepassingen is voltooid en wordt uitgevoerd. 

### <a name="open-the-apache-spark-history-server-web-ui-from-azure-portal"></a>De Webgebruikersinterface voor Apache Spark geschiedenis Server openen vanuit Azure portal

1. Uit de [Azure-portal](https://portal.azure.com/), opent u het Spark-cluster. Zie voor meer informatie, [clusters tonen en vermelden](../hdinsight-administer-use-portal-linux.md#showClusters).
2. Van **snelkoppelingen**, klikt u op **Clusterdashboard**, en klik vervolgens op **Spark geschiedenis Server**. Wanneer u hierom wordt gevraagd, voert u de beheerdersreferenties voor de Spark-cluster. 

    ![Spark-geschiedenis Server](./media/apache-azure-spark-history-server/launch-history-server.png "Spark-geschiedenis-Server")

### <a name="open-the-spark-history-server-web-ui-by-url"></a>Open de Spark-geschiedenis Server-Webgebruikersinterface op URL
Open de Server van de geschiedenis van Spark door te bladeren naar de volgende URL vervangen <ClusterName> met de naam van de Spark-cluster van de klant.

   ```
   https://<ClusterName>.azurehdinsight.net/sparkhistory
   ```

De Spark-geschiedenis Server web UI ziet eruit zoals:

![Server voor HDInsight Spark-geschiedenis](./media/apache-azure-spark-history-server/hdinsight-spark-history-server.png)


## <a name="data-tab-in-spark-history-server"></a>Het tabblad Data in Spark geschiedenis-Server
Selecteer de taak-ID en klik vervolgens op **gegevens** op het menu aan de gegevensweergave.

+ Controleer de **invoer**, **uitvoer**, en **tabelbewerkingen** door de tabbladen afzonderlijk selecteren.

    ![Tabbladen](./media/apache-azure-spark-history-server/sparkui-data-tabs.png)

+ Alle rijen door te klikken op de knop kopiëren **kopie**.

    ![Gegevens kopiëren](./media/apache-azure-spark-history-server/sparkui-data-copy.png)

+ Alle gegevens opslaan als CSV-bestand door te klikken op de knop **csv**.

    ![Gegevens opslaan](./media/apache-azure-spark-history-server/sparkui-data-save.png)

+ Zoeken op trefwoorden invoeren in het veld **zoeken**, worden direct in de zoekresultaten worden weergegeven.

    ![Gegevens zoeken](./media/apache-azure-spark-history-server/sparkui-data-search.png)

+ Klik op de kolomkop om te sorteren van de tabel, klik op het plusteken om uit te breiden van een rij om meer details weer te geven of klik op het minteken samenvouwen van een rij.

    ![Gegevenstabel](./media/apache-azure-spark-history-server/sparkui-data-table.png)

+ Eén bestand downloaden door te klikken op de knop **gedeeltelijke downloaden** die aan de rechterkant, plaatsen en vervolgens het geselecteerde bestand naar lokaal, zal worden gedownload als het bestand niet meer bestaat, wordt deze geopend met een nieuw tabblad om de foutberichten weer te geven.

    ![Rij met gegevens downloaden](./media/apache-azure-spark-history-server/sparkui-data-download-row.png)

+ Volledig pad of het relatieve pad kopiëren door te selecteren de **volledig pad kopiëren**, **relatieve pad kopiëren** die in het menu downloaden van uitgebreid. Voor bestanden in de azure data lake-opslag, **openen in Azure Storage Explorer** Azure Storage Explorer starten en zoek naar de map bij het aanmelden.

    ![Pad naar het kopiëren van gegevens](./media/apache-azure-spark-history-server/sparkui-data-copy-path.png)

+ Klik op het nummer onder de tabel om te navigeren-pagina's bij te veel rijen als u wilt weergeven in één pagina. 

    ![Data-pagina](./media/apache-azure-spark-history-server/sparkui-data-page.png)

+ Beweeg de muisaanwijzer op het vraagteken naast gegevens om de knopinfo weer te geven of klik op het vraagteken voor meer informatie.

    ![Gegevens meer informatie](./media/apache-azure-spark-history-server/sparkui-data-more-info.png)

+ Feedback met problemen verzenden door te klikken op **Geef ons feedback**.

    ![Graph-feedback](./media/apache-azure-spark-history-server/sparkui-graph-feedback.png)


## <a name="graph-tab-in-apache-spark-history-server"></a>Graph-tabblad Apache Spark geschiedenis-server
Selecteer de taak-ID en klik vervolgens op **Graph** in het menu hulpprogramma om de taak graph beeld te krijgen.

+ Overzicht van uw taak controleren door de gegenereerde taakgrafiek. 

+ Standaard alle taken wordt het weergegeven en kan worden gefilterd op **taak-ID**.

    ![Graph-taak-ID](./media/apache-azure-spark-history-server/sparkui-graph-jobid.png)

+ Standaard **voortgang** is geselecteerd, kan gebruiker de gegevensstroom hiervoor controleren **lezen/schriftelijke** in de vervolgkeuzelijst van **weergave**.

    ![grafiek weergeven](./media/apache-azure-spark-history-server/sparkui-graph-display.png)

    De graph-knooppunt in kleur weergeven waarin de heatmap.

    ![Graph-heatmap](./media/apache-azure-spark-history-server/sparkui-graph-heatmap.png)

+ De taak afspelen door te klikken op de **afspelen** knop en op elk gewenst moment stoppen door te klikken op de knop stoppen. De weergave van de taak in kleur om verschillende status weer te geven bij het afspelen:

    + Groen voor geslaagd: De taak is voltooid.
    + Oranje voor opnieuw geprobeerd: Exemplaren van de taken die is mislukt, maar niet van invloed op het uiteindelijke resultaat van de taak. Deze taken heeft dubbele of probeer exemplaren die later mogelijk wel.
    + Blauw voor het werken met: De taak wordt uitgevoerd.
    + Voor de wachttijd-wit of overgeslagen: De taak wacht op om uit te voeren of de fase is overgeslagen.
    + Rood voor is mislukt: De taak is mislukt.

    ![Graph-kleurvoorbeeld uitgevoerd](./media/apache-azure-spark-history-server/sparkui-graph-color-running.png)
 
    De weergave overgeslagen fase in wit.
    ![Kleurvoorbeeld Graph, overslaan](./media/apache-azure-spark-history-server/sparkui-graph-color-skip.png)

    ![Kleurvoorbeeld Graph, is mislukt](./media/apache-azure-spark-history-server/sparkui-graph-color-failed.png)
 
    > [!NOTE]  
    > Afspelen voor elke taak is toegestaan. Afspelen wordt niet ondersteund voor onvolledige taak.


+ Muis schuift als u wilt inzoomen in/uit de taakgrafiek of klik op **-/ uitzoomen om passend** gemakkelijk aanpassen aan scherm.
 
    ![Inzoomen op grafiek aanpassen aan](./media/apache-azure-spark-history-server/sparkui-graph-zoom2fit.png)

+ Beweeg de muisaanwijzer op knooppunt van de grafiek om te zien van de knopinfo wanneer er mislukte taken en klik op het podium om fase pagina te openen.

    ![Knopinfo van de grafiek](./media/apache-azure-spark-history-server/sparkui-graph-tooltip.png)

+ In job graph tabblad fasen hebben knopinfo en het kleine pictogram weergegeven als ze beschikken over taken die voldoen aan de volgende voorwaarden:
    + Gegevensverschil: gegevens gelezen grootte > gemiddelde gegevens gelezen grootte van alle taken in deze fase * 2 en gegevens gelezen grootte > 10 MB.
    + Tijdverschil: uitvoeringstijd > gemiddelde uitvoeringstijd van alle taken in deze fase * 2 en uitvoeringstijd > 2 minuten.

    ![scheeftrekken grafiekpictogram](./media/apache-azure-spark-history-server/sparkui-graph-skew-icon.png)

+ Het knooppunt van de grafiek taak worden de volgende gegevens van elke fase van de weergegeven:
    + ID.
    + Naam of beschrijving.
    + Totaal aantal taak getal.
    + Gegevens lezen: de som van de grootte van de invoer en willekeurige grootte van leesbewerkingen.
    + Schrijven van gegevens: de som van de uitvoergrootte en willekeurige grootte schrijven.
    + Uitvoeringstijd: de tijd tussen de begintijd van de eerste poging en voltooiingstijd van de laatste poging.
    + Aantal rijen: de som van records, invoer, uitvoer records, gelezen records in willekeurige volgorde en schrijven records in willekeurige volgorde.
    + Wordt uitgevoerd.

    > [!NOTE]  
    > Het knooppunt van de grafiek taak gegevens uit de laatste poging van elke fase (met uitzondering van de uitvoeringstijd van de fase) wordt weergegeven, maar tijdens het afspelen van graph knooppunt gegevens van elke poging weergegeven.

    > [!NOTE]  
    > Voor de gegevensgrootte van lezen en schrijven we; gebruik 1 MB = 1000 KB = 1000 * 1000 Bytes.

+ Feedback met problemen verzenden door te klikken op **Geef ons feedback**.

    ![Graph-feedback](./media/apache-azure-spark-history-server/sparkui-graph-feedback.png)


## <a name="diagnosis-tab-in-apache-spark-history-server"></a>Tabblad diagnose Apache Spark geschiedenis-server
Selecteer de taak-ID en klik vervolgens op **diagnose** in het menu Extra om uw werk diagnose weergeven. Bevat het tabblad diagnose **gegevens scheeftrekken**, **Tijdverschilbereik**, en **Executor gebruiksanalyse**.
    
+ Controleer de **gegevens scheeftrekken**, **Tijdverschilbereik**, en **Executor gebruiksanalyse** door respectievelijk de tabbladen selecteren.

    ![Diagnose tabbladen](./media/apache-azure-spark-history-server/sparkui-diagnosis-tabs.png)

### <a name="data-skew"></a>Gegevensverschil
Klik op **gegevens scheeftrekken** tabblad, het bijbehorende ongelijke taken worden weergegeven op basis van de opgegeven parameters. 

+ **Geef Parameters** -de eerste sectie worden de parameters die worden gebruikt voor het detecteren van gegevens is een Gegevensverschil weergegeven. De regel van de ingebouwde is: Taak gegevens lezen is groter dan 3 keer de gemiddelde taak gegevens lezen en de taakgegevens lezen is meer dan 10MB. Als u wilt voor het definiëren van uw eigen regel voor ongelijke taken, kunt u de parameters, de **ongelijkmatig fase**, en **scheeftrekken Char** sectie dienovereenkomstig worden vernieuwd.

+ **Fase ook ongelijkmatig** -fasen die taken die voldoen aan de bovenstaande criteria hebben ongelijkmatig in de tweede sectie weergegeven. Als er meer dan één ongelijke taak in een fase, wordt in de faseringstabel ongelijke alleen de meest ongelijke taak (bijvoorbeeld de grootste gegevens voor gegevens scheeftrekken) weergegeven.

    ![Gegevens scheeftrekken sectie2 zetten](./media/apache-azure-spark-history-server/sparkui-diagnosis-dataskew-section2.png)

+ **Grafiek scheeftrekken** : bij een rij in de faseringstabel scheeftrekken is geselecteerd, weergegeven als de scheeftrekken grafiek meer taakdetails distributies op basis van gegevens die zijn gelezen en uitvoeringstijd. De ongelijke taken worden in rood gemarkeerd en de normale taken zijn gemarkeerd in het blauw. Ter overweging prestaties, in de grafiek alleen maximaal 100 voorbeeldtaken weergegeven. De taakdetails worden weergegeven in het Configuratiescherm rechts en onderaan.

    ![Gegevens scheeftrekken section3](./media/apache-azure-spark-history-server/sparkui-diagnosis-dataskew-section3.png)

### <a name="time-skew"></a>Tijd scheeftrekken
De **Tijdverschilbereik** tabblad ongelijke taken op basis van uitvoeringstijd van de taak wordt weergegeven. 

+ **Geef Parameters** -de eerste sectie worden de parameters die worden gebruikt voor het detecteren van de tijd scheeftrekken weergegeven. De standaardcriteria voor het detecteren van tijdverschil zijn: uitvoeringstijd van de taak is groter dan 3 keer van de gemiddelde uitvoeringstijd en tijd waarop de uitvoering van de taak is meer dan 30 seconden. U kunt de parameters op basis van uw behoeften kunt wijzigen. De **ongelijkmatig fase** en **scheeftrekken grafiek** net als de bijbehorende fasen en taken informatie geven de **gegevens scheeftrekken** hierboven een tab voor.

+ Klik op **Tijdverschilbereik**, en vervolgens gefilterd resultaat wordt weergegeven in **ongelijkmatig fase** sectie op basis van de parameters die zijn ingesteld in de sectie **Parameters opgeven**. Klik op een item in **ongelijkmatig fase** sectie, en vervolgens het bijbehorende schema is opgesteld in section3 en de taakdetails worden weergegeven in het Configuratiescherm rechts en onderaan.

    ![Tijd scheeftrekken sectie2 zetten](./media/apache-azure-spark-history-server/sparkui-diagnosis-timeskew-section2.png)

### <a name="executor-usage-analysis"></a>Gebruiksanalyse Executor
De Executor-Gebruiksgrafiek visualiseert Spark werkelijke executor toewijzing en uitgevoerd status van de taak.  

+ Klik op **Executor gebruiksanalyse**, en vervolgens de vier typen curven over het gebruik van executor zijn opgesteld, met inbegrip van **toegewezen Executor**, **Executor uitgevoerd**, **inactieve Executor**, en **max. Executor exemplaren**. Met betrekking tot toegewezen Executor elke 'Executor toegevoegd' of 'Executor verwijderd' gebeurtenis wordt vergroten of verkleinen van de toegewezen Executor, kunt u 'Gebeurtenis tijdlijn' controleren op het tabblad 'Taken' voor meer vergelijking.

    ![Executor-tabblad](./media/apache-azure-spark-history-server/sparkui-diagnosis-executors.png)

+ Klik op het Kleurenpictogram om te selecteren of hef de selectie van de bijbehorende inhoud van alle concepten.

    ![Grafiek selecteren](./media/apache-azure-spark-history-server/sparkui-diagnosis-select-chart.png)


## <a name="faq"></a>Veelgestelde vragen

### <a name="1-revert-to-community-version"></a>1. Terugkeren naar de community-versie

Als u wilt terugkeren naar de community-versie, voer de volgende stappen uit:

1. Open-cluster in Ambari. Klik op **Spark2** in het linkerdeelvenster.
2. Klik op **Peeringconfiguraties** tabblad.
3. De groep uitvouwen **aangepaste spark2-standaardinstellingen**.
4. Klik op **eigenschap toevoegen**, toevoegen **spark.ui.enhancement.enabled=false**, opslaan.
5. De eigenschap wordt ingesteld op **false** nu.
6. Klik op **opslaan** aan de configuratie op te slaan.

    ![functie wordt uitgeschakeld](./media/apache-azure-spark-history-server/sparkui-turn-off.png)

7. Klik op **Spark2** links in deelvenster onder **samenvatting** tabblad **Spark2 geschiedenis Server**.

    ![Start opnieuw op server1](./media/apache-azure-spark-history-server/sparkui-restart-1.png) 

8. Geschiedenis-server opnieuw starten door te klikken op **opnieuw** van **Spark2 geschiedenis Server**.

    ![Start opnieuw op server2](./media/apache-azure-spark-history-server/sparkui-restart-2.png)  

9. Vernieuwen van de Spark-geschiedenis server-Webgebruikersinterface, community-versie wordt worden teruggedraaid.

### <a name="2-upload-history-server-event"></a>2. Geschiedenis van server-gebeurtenis uploaden

Als u de serverfout geschiedenis, volg de stappen voor de gebeurtenis:
1. Gebeurtenis downloaden door te klikken op **downloaden** in de geschiedenis van server-webgebruikersinterface.

    ![gebeurtenissen downloaden](./media/apache-azure-spark-history-server/sparkui-download-event.png)

2. Klik op **Geef ons feedback** gegevens/graph-tabblad.

    ![Graph-feedback](./media/apache-azure-spark-history-server/sparkui-graph-feedback.png)

3. Geef de titel en beschrijving van fout, sleept u het zip-bestand naar het bewerkingsveld en klik op **nieuw probleem verzenden**.

    ![probleem met het bestand](./media/apache-azure-spark-history-server/sparkui-file-issue.png)


### <a name="3-upgrade-jar-file-for-hotfix-scenario"></a>3. Upgrade van de jar-bestand voor de hotfix-scenario

Als u upgraden met hotfix wilt, gebruikt u het onderstaande script spark-enhancement.jar* worden bijgewerkt.

**upgrade_spark_enhancement.sh**:

   ```bash
    #!/usr/bin/env bash

    # Copyright (C) Microsoft Corporation. All rights reserved.

    # Arguments:
    # $1 Enhancement jar path

    if [ "$#" -ne 1 ]; then
        >&2 echo "Please provide the upgrade jar path."
        exit 1
    fi

    install_jar() {
        tmp_jar_path="/tmp/spark-enhancement-hotfix-$( date +%s )"

        if wget -O "$tmp_jar_path" "$2"; then
            for FILE in "$1"/spark-enhancement*.jar
            do
                back_up_path="$FILE.original.$( date +%s )"
                echo "Back up $FILE to $back_up_path"
                mv "$FILE" "$back_up_path"
                echo "Copy the hotfix jar file from $tmp_jar_path   to $FILE"
                cp "$tmp_jar_path" "$FILE"

                "Hotfix done."
                break
            done
        else    
            >&2 echo "Download jar file failed."
            exit 1
        fi
    }

    jars_folder="/usr/hdp/current/spark2-client/jars"
    jar_path=$1

    if ls ${jars_folder}/spark-enhancement*.jar 1>/dev/null 2>&1;   then
        install_jar "$jars_folder" "$jar_path"
    else
        >&2 echo "There is no target jar on this node. Exit with no action."
        exit 0
    fi
   ```

**Gebruik**: 

`upgrade_spark_enhancement.sh https://${jar_path}`

**Voorbeeld**:

`upgrade_spark_enhancement.sh https://${account_name}.blob.core.windows.net/packages/jars/spark-enhancement-${version}.jar` 

**De bash-bestand vanuit Azure-portal te gebruiken**

1. Start [Azure Portal](https://ms.portal.azure.com), en selecteert u uw cluster.
2. Klik op **scriptacties**, klikt u vervolgens **indienen nieuwe**. Voltooi de **scriptactie verzenden** vormen en klik vervolgens op **maken** knop.
    
    + **Scripttype**: Selecteer **aangepaste**.
    + **Naam**: Geef een scriptnaam op.
    + **Bash-script-URI**: de bash-bestand uploaden naar het persoonlijke cluster en vervolgens hier de URL kopiëren. U kunt ook gebruik van de opgegeven URI.
    
   ```upgrade_spark_enhancement
    https://hdinsighttoolingstorage.blob.core.windows.net/shsscriptactions/upgrade_spark_enhancement.sh
   ```

    + Controleren op **Head** en **Worker**.
    + **Parameters**: de bash-gebruik van de volgende parameters instellen.

    ![uploaden van logboek of hotfix bijwerken](./media/apache-azure-spark-history-server/sparkui-upload2.png)


## <a name="known-issues"></a>Bekende problemen

1.  Op dit moment werkt alleen voor 2.3 Spark-cluster.

2.  I/o-gegevens met behulp van de RDD worden niet weergegeven in het tabblad data.

## <a name="next-steps"></a>Volgende stappen

* [Resources beheren voor een Apache Spark-cluster in HDInsight](apache-spark-resource-manager.md)
* [Apache Spark-instellingen configureren](apache-spark-settings.md)


## <a name="contact-us"></a>Contact opnemen

Als u bepaalde feedback hebt, of als er andere problemen die optreden bij gebruik van dit hulpprogramma, stuurt u een e-mail op ([hdivstool@microsoft.com](mailto:hdivstool@microsoft.com)).
