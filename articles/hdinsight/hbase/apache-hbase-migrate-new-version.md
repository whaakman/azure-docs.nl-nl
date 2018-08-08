---
title: Een HBase-cluster migreren naar een nieuwe versie - Azure HDInsight
description: Klik hier voor meer informatie over het HBase-clusters migreren naar een nieuwe versie.
services: hdinsight
author: ashishthaps
editor: jasonwhowell
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 01/22/2018
ms.author: ashishth
ms.openlocfilehash: 091e03c7f5d589a2e99d20990c9aa6844c3cb165
ms.sourcegitcommit: 1f0587f29dc1e5aef1502f4f15d5a2079d7683e9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/07/2018
ms.locfileid: "39592431"
---
# <a name="migrate-an-hbase-cluster-to-a-new-version"></a>Een HBase-cluster migreren naar een nieuwe versie

Clusters op basis van een taak, zoals Spark en Hadoop, zijn eenvoudige upgrade - Zie [Upgrade HDInsight-cluster naar een nieuwere versie](../hdinsight-upgrade-cluster.md):

1. Back-up van gegevens voor tijdelijke (lokaal opgeslagen).
2. Het bestaande cluster verwijderen.
3. Maak een nieuw cluster in hetzelfde VNET-subnet.
4. Tijdelijke gegevens importeren.
5. Taken starten en gaan met de verwerking op het nieuwe cluster.

Als u wilt een HBase-cluster upgraden zijn enkele extra stappen nodig, zoals beschreven in dit artikel.

> [!NOTE]
> De uitvaltijd tijdens de upgrade moet minimaal orde van grootte minuten. Deze uitvaltijd wordt veroorzaakt door de stappen leegmaken van alle in-memory-gegevens en vervolgens de tijd voor het configureren en opnieuw starten van de services op het nieuwe cluster. De resultaten zullen verschillen, afhankelijk van het aantal knooppunten, de hoeveelheid gegevens en andere variabelen.

## <a name="review-hbase-compatibility"></a>HBase-compatibiliteit evalueren

Controleer of dat de HBase-versies in de bron- en -clusters compatibel zijn voor het upgraden van HBase. Zie voor meer informatie, [Hadoop-onderdelen en versies die beschikbaar met HDInsight](../hdinsight-component-versioning.md).

> [!NOTE]
> Is het raadzaam dat u de versie-compatibiliteitsmatrix in controleren de [HBase book](https://hbase.apache.org/book.html#upgrading).

Hier volgt een voorbeeld van de versie compatibiliteitsmatrix, waar Y compatibiliteit aangeeft en N een mogelijke incompatibiliteit:

| Compatibiliteitstype | Hoofdversie| Secundaire versie | Patch |
| --- | --- | --- | --- |
| Compatibiliteit van Client / Server-kabel | N | J | J |
| Compatibiliteit met de server-Server | N | J | J |
| Compatibiliteit van bestanden | N | J | J |
| Compatibiliteit van de client-API | N | J | J |
| Binaire compatibiliteit van client | N | N | J |
| **Serverzijde beperkt API-compatibiliteit** |  |  |  |
| Stabiel | N | J | J |
| In ontwikkeling | N | N | J |
| Instabiel | N | N | N |
| Compatibiliteit van afhankelijkheid | N | J | J |
| Operationele compatibiliteit | N | N | J |

> [!NOTE]
> Eventuele incompatibiliteiten belangrijke moeten worden beschreven in de releaseopmerkingen voor HBase-versie.

## <a name="upgrade-with-same-hbase-major-version"></a>Een upgrade uitvoert op dezelfde hoofdversie van HBase

De volgende scenario is voor het upgraden van HDInsight 3.4 naar 3.6 (beide worden geleverd met Apache HBase 1.1.2) met de dezelfde primaire versie van HBase. Andere versie-upgrades zijn vergelijkbaar, zolang er geen compatibiliteitsproblemen tussen bron- en -versies zijn.

1. Zorg ervoor dat uw toepassing compatibel met de nieuwe versie, is zoals wordt weergegeven in de HBase-compatibiliteit matrix- en release-opmerkingen. Test uw toepassing in een cluster met de doelversie van HDInsight en HBase.

2. [Een nieuwe bestemming HDInsight-cluster instellen](../hdinsight-hadoop-provision-linux-clusters.md) met behulp van hetzelfde opslagaccount, maar met de naam van een andere container:

    ![Gebruik hetzelfde opslagaccount, maar een andere Container maken](./media/apache-hbase-migrate-new-version/same-storage-different-container.png)

3. Leegmaken van de bron HBase-cluster. Dit is het cluster waaruit u een upgrade uitvoert. HBase schrijft binnenkomende gegevens naar een in-memory-opslag, met de naam een _setSize()_. Nadat de geheugenopslag een bepaalde grootte bereikt, moet de geheugenopslag worden leeggemaakt op schijf voor langdurige opslag in de storage-account van het cluster. Wanneer u het oude cluster verwijdert, worden de memstores gerecycled, mogelijk gegevens verloren gaan. Voer het volgende script als u wilt de geheugenopslag voor elke tabel handmatig leegmaken naar schijf. De meest recente versie van dit script wordt op de Azure [GitHub](https://raw.githubusercontent.com/Azure/hbase-utils/master/scripts/flush_all_tables.sh).

    ```bash
    #!/bin/bash
    
    #-------------------------------------------------------------------------------#
    # SCRIPT TO FLUSH ALL HBASE TABLES.
    #-------------------------------------------------------------------------------#
    
    LIST_OF_TABLES=/tmp/tables.txt
    HBASE_SCRIPT=/tmp/hbase_script.txt
    TARGET_HOST=$1
    
    usage ()
    {
        if [[ "$1" == "-h" ]] || [[ "$1" == "--help" ]]
        then
            cat << ...
    
    Usage: 
    
    $0 [hostname]
    
    Providing hostname is optional and not required when the script is executed within HDInsight cluster with access to 'hbase shell'.
    
    However hostname should be provided when executing the script as a script-action from HDInsight portal.
    
    For Example:
    
        1.  Executing script inside HDInsight cluster (where 'hbase shell' is 
            accessible):
    
            $0 
    
            [No need to provide hostname]
    
        2.  Executing script from HDinsight Azure portal:
    
            Provide Script URL.
    
            Provide hostname as a parameter (i.e. hn0, hn1 or wn2 etc.).
    ...
            exit
        fi
    }
    
    validate_machine ()
    {
        THIS_HOST=`hostname`
    
        if [[ ! -z "$TARGET_HOST" ]] && [[ $THIS_HOST  != $TARGET_HOST* ]]
        then
            echo "[INFO] This machine '$THIS_HOST' is not the right machine ($TARGET_HOST) to execute the script."
            exit 0
        fi
    }
    
    get_tables_list ()
    {
    hbase shell << ... > $LIST_OF_TABLES 2> /dev/null
        list
        exit
    ...
    }
    
    add_table_for_flush ()
    {
        TABLE_NAME=$1
        echo "[INFO] Adding table '$TABLE_NAME' to flush list..."
        cat << ... >> $HBASE_SCRIPT
            flush '$TABLE_NAME'
    ...
    }
    
    clean_up ()
    {
        rm -f $LIST_OF_TABLES
        rm -f $HBASE_SCRIPT
    }
    
    ########
    # MAIN #
    ########
    
    usage $1
    
    validate_machine
    
    clean_up
    
    get_tables_list
    
    START=false
    
    while read LINE 
    do 
        if [[ $LINE == TABLE ]] 
        then
            START=true
            continue
        elif [[ $LINE == *row*in*seconds ]]
        then
            break
        elif [[ $START == true ]]
        then
            add_table_for_flush $LINE
        fi
    
    done < $LIST_OF_TABLES
    
    cat $HBASE_SCRIPT
    
    hbase shell $HBASE_SCRIPT << ... 2> /dev/null
    exit
    ...
    
    ```
    
4. Stop de opname voor het oude HBase-cluster.
5. Voer het vorige script opnieuw uit om ervoor te zorgen dat alle recente gegevens in de geheugenopslag is leeggemaakt.
6. Meld u aan bij de Ambari op het oude cluster (https://OLDCLUSTERNAME.azurehdidnsight.net) en stop de HBase-services. Wanneer u wordt gevraagd om te bevestigen dat u wilt de services te stoppen, schakel het selectievakje inschakelen in de onderhoudsmodus voor HBase. Zie voor meer informatie over verbinding maken met en het gebruik van Ambari [beheren HDInsight-clusters met behulp van de Ambari-Webgebruikersinterface](../hdinsight-hadoop-manage-ambari.md).

    ![In de Ambari, klikt u op het tabblad Services en vervolgens HBase in het menu links en Stop vervolgens onder serviceacties](./media/apache-hbase-migrate-new-version/stop-hbase-services.png)

    ![Controleren van de onderhoudsmodus inschakelen voor HBase selectievakje, en vervolgens bevestigen](./media/apache-hbase-migrate-new-version/turn-on-maintenance-mode.png)

7. Meld u aan bij de Ambari aan op de nieuwe HDInsight-cluster. Wijzig de `fs.defaultFS` HDFS-instelling om te verwijzen naar de containernaam die wordt gebruikt door het oorspronkelijke cluster. Deze instelling is onder **HDFS > configuraties > Geavanceerd > Geavanceerde core-site**.

    ![In de Ambari, klikt u op het tabblad Services en vervolgens HDFS in het menu links, klikt u vervolgens het tabblad configuraties, klikt u vervolgens het tabblad Geavanceerd onder](./media/apache-hbase-migrate-new-version/hdfs-advanced-settings.png)

    ![In de Ambari, wijzigt u de containernaam](./media/apache-hbase-migrate-new-version/change-container-name.png)

8. Sla uw wijzigingen op.
9. Alle benodigde services opnieuw starten zoals aangegeven door Ambari.
10. Wijs de toepassing naar het nieuwe cluster.

    > [!NOTE]
    > De statische DNS-server voor uw wijzigingen in de toepassing bij een upgrade. In plaats van hard-coding DNS, kunt u een CNAME in DNS-instellingen voor uw domeinnaam die verwijst naar de naam van het cluster configureren. Een andere optie is het gebruik van een configuratiebestand voor uw toepassing die u zonder opnieuw te implementeren bijwerken kunt.

11. Start de opname van gegevens om te zien als alles werkt zoals verwacht.
12. Als het nieuwe cluster voldoende is, verwijdert u het oorspronkelijke cluster.

## <a name="next-steps"></a>Volgende stappen

Zie de volgende artikelen voor meer informatie over het HBase- en HDInsight-clusters upgraden:

* [Een HDInsight-cluster upgraden naar een nieuwere versie](../hdinsight-upgrade-cluster.md)
* [Controleren en beheren van Azure HDInsight met behulp van de Ambari-Webinterface](../hdinsight-hadoop-manage-ambari.md)
* [Hadoop-onderdelen en versies](../hdinsight-component-versioning.md)
* [Configuraties met behulp van Ambari optimaliseren](../hdinsight-changing-configs-via-ambari.md#hbase-optimization-with-the-ambari-web-ui)
