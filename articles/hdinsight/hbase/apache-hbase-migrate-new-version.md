---
title: Een HBase-cluster migreren naar een nieuwe versie - Azure HDInsight | Microsoft Docs
description: Klik hier voor meer informatie over het HBase-clusters migreren naar een nieuwe versie.
services: hdinsight
documentationcenter: ''
tags: azure-portal
author: ashishthaps
manager: jhubbard
editor: cgronlun
ms.assetid: ''
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: conceptual
ms.date: 01/22/2018
ms.author: ashishth
ms.openlocfilehash: 3ca982e7fc0ce56bee2ee2e193c82a78fac44362
ms.sourcegitcommit: 1362e3d6961bdeaebed7fb342c7b0b34f6f6417a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/18/2018
---
# <a name="migrate-an-hbase-cluster-to-a-new-version"></a>Een HBase-cluster migreren naar een nieuwe versie

Taak gebaseerde clusters, zoals Spark en Hadoop, vormen een eenvoudige upgrade - Zie [Upgrade HDInsight-cluster naar een nieuwere versie](../hdinsight-upgrade-cluster.md):

1. Back-ups tijdelijke (lokaal opgeslagen).
2. Het bestaande cluster verwijderen.
3. Maak een nieuw cluster in hetzelfde VNET subnet.
4. Tijdelijke gegevens importeren.
5. Taken starten en gaan met de verwerking op het nieuwe cluster.

Als u wilt upgraden van een HBase-cluster zijn een aantal extra stappen nodig, zoals beschreven in dit artikel.

> [!NOTE]
> De uitvaltijd tijdens het upgraden van moet minimaal de orde van grootte minuten. Deze uitvaltijd wordt veroorzaakt door de stappen voor het leegmaken van alle gegevens voor in het geheugen dan de tijd om te configureren en starten van de services op het nieuwe cluster. Uw resultaten variëren, afhankelijk van het aantal knooppunten, hoeveelheid gegevens en andere variabelen.

## <a name="review-hbase-compatibility"></a>HBase compatibiliteit evalueren

Voordat u bijwerkt HBase, Controleer dat de HBase-versies in de bron- en doelserver clusters compatibel zijn. Zie voor meer informatie [Hadoop-onderdelen en versies die beschikbaar met HDInsight](../hdinsight-component-versioning.md).

> [!NOTE]
> We raden u wordt aangeraden de versie compatibiliteit matrix in de [HBase book](https://hbase.apache.org/book.html#upgrading).

Hier volgt een voorbeeld versie compatibiliteit matrix-, waarbij Y aangeeft compatibiliteit en N potentiële compatibiliteitsproblemen:

| Compatibiliteitstype | Primaire versie| Secundaire versie | Patch |
| --- | --- | --- | --- |
| Compatibiliteit van Client / Server-kabel | N | J | J |
| Compatibiliteit van servers | N | J | J |
| Compatibiliteit van bestanden | N | J | J |
| Compatibiliteit van de client-API | N | J | J |
| Binaire compatibiliteit van client | N | N | J |
| **Serverzijde beperkt API compatibiliteit** |  |  |  |
| Stabiel | N | J | J |
| In ontwikkeling | N | N | J |
| Onstabiel | N | N | N |
| Afhankelijkheid compatibiliteit | N | J | J |
| Operationele compatibiliteit | N | N | J |

> [!NOTE]
> Eventuele incompatibiliteiten verbreken worden beschreven in de releaseopmerkingen voor HBase-versie.

## <a name="upgrade-with-same-hbase-major-version"></a>Een upgrade uitvoert op dezelfde hoofdversie van HBase

Het volgende scenario is voor het upgraden van HDInsight 3.4 naar 3.6 (zowel geleverd met Apache HBase 1.1.2) met dezelfde primaire versie HBase. Andere versie-upgrades zijn vergelijkbaar, zolang er geen compatibiliteitsproblemen tussen de bron- en doelserver versies zijn.

1. Zorg ervoor dat uw toepassing compatibel met de nieuwe versie, is zoals wordt weergegeven in de HBase-compatibiliteit matrix en release-opmerkingen. Uw toepassing testen in een cluster met de doelversie van HDInsight en HBase.

2. [Een nieuwe bestemming HDInsight-cluster instellen](../hdinsight-hadoop-provision-linux-clusters.md) met hetzelfde opslagaccount, maar met de naam van een andere container:

    ![Gebruik hetzelfde opslagaccount, maar een andere Container maken](./media/apache-hbase-migrate-new-version/same-storage-different-container.png)

3. Leegmaken van de bron HBase-cluster. Dit is het cluster waaruit u een upgrade uitvoert. HBase schrijft binnenkomende gegevens naar een winkel in het geheugen aangeroepen een _geheugenopslag_. Nadat de geheugenopslag een bepaalde grootte bereikt, moet de geheugenopslag worden leeggemaakt op schijf voor langdurige opslag in de storage-account van het cluster. Wanneer u het oude cluster verwijdert, worden de memstores gerecycled mogelijk gegevens verloren gaan. Handmatig leegmaken van de geheugenopslag voor elke tabel op schijf, voer het volgende script. De meest recente versie van dit script is op Azure [GitHub](https://raw.githubusercontent.com/Azure/hbase-utils/master/scripts/flush_all_tables.sh).

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
5. Om ervoor te zorgen dat alle recente gegevens in de geheugenopslag wordt leeggemaakt, het vorige script opnieuw uitgevoerd.
6. Meld u aan bij Ambari op het oude cluster (https://OLDCLUSTERNAME.azurehdidnsight.net) en stop de HBase-services. Wanneer u wordt gevraagd om te bevestigen dat u wilt stoppen van de services, schakel het selectievakje onderhoudsmodus inschakelen voor HBase. Zie voor meer informatie over verbinding maken met en gebruiken van Ambari [HDInsight-clusters beheren met behulp van de Ambari-Webgebruikersinterface](../hdinsight-hadoop-manage-ambari.md).

    ![Ambari, klik op het tabblad Services vervolgens HBase op in het menu links en onder acties van de Service stoppen](./media/apache-hbase-migrate-new-version/stop-hbase-services.png)

    ![Controleer de inschakelen op onderhoudsmodus voor HBase selectievakje en bevestig vervolgens](./media/apache-hbase-migrate-new-version/turn-on-maintenance-mode.png)

7. Meld u aan bij Ambari op het nieuwe HDInsight-cluster. Wijzig de `fs.defaultFS` HDFS-instelling om te verwijzen naar de containernaam gebruikt door het oorspronkelijke cluster. Deze instelling is onder **HDFS > Configs > Geavanceerd > Geavanceerde core site**.

    ![Ambari, klik op het tabblad Services, klikt u vervolgens de HDFS op in het menu links, klikt u vervolgens het tabblad Configs vervolgens het tabblad Geavanceerd onder](./media/apache-hbase-migrate-new-version/hdfs-advanced-settings.png)

    ![Wijzig de containernaam van de in Ambari,](./media/apache-hbase-migrate-new-version/change-container-name.png)

8. Sla uw wijzigingen op.
9. Start alle vereiste services zoals aangegeven door Ambari.
10. Punt in uw toepassing naar het nieuwe cluster.

    > [!NOTE]
    > De statische DNS-server om de wijzigingen van toepassing bij een upgrade. In plaats van hard-coding van de DNS-, kunt u een CNAME configureren in uw domeinnaam DNS-instellingen die verwijst naar de naam van het cluster. Een andere optie is het gebruik van een configuratiebestand voor de toepassing die u zonder opnieuw te implementeren bijwerken kunt.

11. Start de opname als alles werkt zoals verwacht.
12. Als het nieuwe cluster voldoende is, verwijdert u het oorspronkelijke cluster.

## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie over het HBase en upgraden van HDInsight-clusters, de volgende artikelen:

* [Een HDInsight-cluster naar een nieuwere versie upgraden](../hdinsight-upgrade-cluster.md)
* [Bewaken en beheren van Azure HDInsight met de Ambari-Webgebruikersinterface](../hdinsight-hadoop-manage-ambari.md)
* [Hadoop-onderdelen en versies](../hdinsight-component-versioning.md)
* [Configuraties met Ambari optimaliseren](../hdinsight-changing-configs-via-ambari.md#hbase-optimization-with-the-ambari-web-ui)
