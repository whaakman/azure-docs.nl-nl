---
title: Apache Storm met Python comopnents - Azure HDInsight | Microsoft Docs
description: Informatie over het maken van een Apache Storm-topologie die gebruikmaakt van Python-onderdelen.
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
keywords: Apache storm python
ms.assetid: edd0ec4f-664d-4266-910c-6ecc94172ad8
ms.service: hdinsight
ms.custom: hdinsightactive,hdiseo17may2017
ms.devlang: python
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 08/04/2017
ms.author: larryfr
ms.openlocfilehash: 305c4060ad81458b254e66a4bad6dfd7bf69b28d
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/18/2017
---
# <a name="develop-apache-storm-topologies-using-python-on-hdinsight"></a>Apache Storm-topologieën op HDInsight met behulp van Python ontwikkelen

Informatie over het maken van een Apache Storm-topologie die gebruikmaakt van Python-onderdelen. Apache Storm ondersteunt meerdere talen zelfs zodat u onderdelen uit verschillende talen in een topologie combineren. Het lichtstroom framework (geïntroduceerd met Storm 0.10.0) kunt u eenvoudig om oplossingen te maken die gebruikmaken van Python-onderdelen.

> [!IMPORTANT]
> De informatie in dit document is getest met Storm op HDInsight 3.6. Linux is het enige besturingssysteem dat wordt gebruikt in HDInsight-versie 3.4 of hoger. Zie [HDInsight retirement on Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement) (HDInsight buiten gebruik gestel voor Windows) voor meer informatie.

De code voor dit project is beschikbaar op [https://github.com/Azure-Samples/hdinsight-python-storm-wordcount](https://github.com/Azure-Samples/hdinsight-python-storm-wordcount).

## <a name="prerequisites"></a>Vereisten

* Python 2.7 of hoger

* Java JDK 1.8 of hoger

* Maven 3

* (Optioneel) Een lokale Storm-ontwikkelomgeving. Een lokale Storm-omgeving is alleen nodig als u wilt de topologie lokaal uitvoeren. Zie voor meer informatie [een ontwikkelomgeving instellen](http://storm.apache.org/releases/1.0.1/Setting-up-development-environment.html).

## <a name="storm-multi-language-support"></a>Storm-ondersteuning voor meerdere talen

Apache Storm is ontworpen om te werken met onderdelen die zijn geschreven met behulp van elke programmeertaal. De onderdelen moeten begrijpen hoe u werkt met de [Thrift-definitie voor Storm](https://github.com/apache/storm/blob/master/storm-core/src/storm.thrift). Voor Python, wordt een module geleverd als onderdeel van de Apache Storm-project waarmee u eenvoudig een interface met Storm. U vindt deze module op [https://github.com/apache/storm/blob/master/storm-multilang/python/src/main/resources/resources/storm.py](https://github.com/apache/storm/blob/master/storm-multilang/python/src/main/resources/resources/storm.py).

Storm is een Java-proces dat wordt uitgevoerd op de Java-virtuele Machine (JVM). Onderdelen die zijn geschreven in andere talen worden uitgevoerd als subprocessen. De Storm communiceert met deze subprocessen met behulp van JSON-berichten via stdin/stdout verzonden. Meer informatie over de communicatie tussen onderdelen vindt u in de [Multi-lang Protocol](https://storm.apache.org/documentation/Multilang-protocol.html) documentatie.

## <a name="python-with-the-flux-framework"></a>Python met lichtstroom framework

Het framework lichtstroom kunt u Storm-topologieën afzonderlijk van de onderdelen te definiëren. Het framework lichtstroom gebruikt YAML voor het definiëren van de Storm-topologie. De volgende tekst is een voorbeeld van een Python-component in het document YAML verwijzen naar:

```yaml
# Spout definitions
spouts:
  - id: "sentence-spout"
    className: "org.apache.storm.flux.wrappers.spouts.FluxShellSpout"
    constructorArgs:
      # Command line
      - ["python", "sentencespout.py"]
      # Output field(s)
      - ["sentence"]
    # parallelism hint
    parallelism: 1
```

De klasse `FluxShellSpout` wordt gebruikt om te beginnen de `sentencespout.py` script dat de spout implementeert.

Lichtstroom verwacht de Python-scripts in de `/resources` map binnen het jar-bestand dat de topologie bevat. Dus in dit voorbeeld Slaat het Python-scripts in de `/multilang/resources` directory. De `pom.xml` deze met behulp van de volgende XML-bestand bevat:

```xml
<!-- include the Python components -->
<resource>
    <directory>${basedir}/multilang</directory>
    <filtering>false</filtering>
</resource>
```

Zoals eerder vermeld, wordt er een `storm.py` bestand dat de Thrift-definitie voor Storm implementeert. Het framework lichtstroom bevat `storm.py` automatisch wanneer het project is gebouwd, zodat u niet hoeft te hoeven maken over te nemen.

## <a name="build-the-project"></a>Het project bouwen

Gebruik de volgende opdracht in de hoofdmap van het project:

```bash
mvn clean compile package
```

Deze opdracht maakt u een `target/WordCount-1.0-SNAPSHOT.jar` -bestand met de gecompileerde topologie.

## <a name="run-the-topology-locally"></a>De topologie lokaal uitvoeren

Als u wilt de topologie lokaal uitvoeren, moet u de volgende opdracht gebruiken:

```bash
storm jar WordCount-1.0-SNAPSHOT.jar org.apache.storm.flux.Flux -l -R /topology.yaml
```

> [!NOTE]
> Deze opdracht moet een lokale Storm-ontwikkelomgeving. Zie voor meer informatie [een ontwikkelomgeving instellen](http://storm.apache.org/releases/1.0.1/Setting-up-development-environment.html)

Eenmaal de topologie wordt gestart, het verzendt gegevens naar de lokale console vergelijkbaar met de volgende tekst:


    24302 [Thread-25-sentence-spout-executor[4 4]] INFO  o.a.s.s.ShellSpout - ShellLog pid:2436, name:sentence-spout Emiting the cow jumped over the moon
    24302 [Thread-30] INFO  o.a.s.t.ShellBolt - ShellLog pid:2438, name:splitter-bolt Emitting the
    24302 [Thread-28] INFO  o.a.s.t.ShellBolt - ShellLog pid:2437, name:counter-bolt Emitting years:160
    24302 [Thread-17-log-executor[3 3]] INFO  o.a.s.f.w.b.LogInfoBolt - {word=the, count=599}
    24303 [Thread-17-log-executor[3 3]] INFO  o.a.s.f.w.b.LogInfoBolt - {word=seven, count=302}
    24303 [Thread-17-log-executor[3 3]] INFO  o.a.s.f.w.b.LogInfoBolt - {word=dwarfs, count=143}
    24303 [Thread-25-sentence-spout-executor[4 4]] INFO  o.a.s.s.ShellSpout - ShellLog pid:2436, name:sentence-spout Emiting the cow jumped over the moon
    24303 [Thread-30] INFO  o.a.s.t.ShellBolt - ShellLog pid:2438, name:splitter-bolt Emitting cow
    24303 [Thread-17-log-executor[3 3]] INFO  o.a.s.f.w.b.LogInfoBolt - {word=four, count=160}


Voor het beëindigen van de topologie gebruikt __Ctrl + c drukken__.

## <a name="run-the-storm-topology-on-hdinsight"></a>De topologie Storm op HDInsight uitvoeren

1. Gebruik de volgende opdracht om te kopiëren de `WordCount-1.0-SNAPSHOT.jar` -bestand naar uw Storm op HDInsight-cluster:

    ```bash
    scp target\WordCount-1.0-SNAPSHOT.jar sshuser@mycluster-ssh.azurehdinsight.net
    ```

    Vervang `sshuser` met de SSH-gebruiker voor uw cluster. Vervang `mycluster` met de naam van het cluster. U wordt mogelijk gevraagd het wachtwoord invoeren voor de SSH-gebruiker.

    Zie voor meer informatie over het gebruik van SSH en SCP [SSH gebruiken met HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md).

2. Zodra het bestand zijn geüpload, verbinding maken met het cluster via SSH:

    ```bash
    ssh sshuser@mycluster-ssh.azurehdinsight.net
    ```

3. Gebruik de volgende opdracht voor het starten van de topologie op het cluster van de SSH-sessie:

    ```bash
    storm jar WordCount-1.0-SNAPSHOT.jar org.apache.storm.flux.Flux -r -R /topology.yaml
    ```

3. U kunt de Storm-gebruikersinterface gebruiken om de topologie op het cluster weer te geven. De Storm-gebruikersinterface bevindt zich op https://mycluster.azurehdinsight.net/stormui. Vervang `mycluster` met de clusternaam van uw.

> [!NOTE]
> Eenmaal gestart, wordt een Storm-topologie uitgevoerd totdat deze wordt gestopt. Als u wilt de topologie stoppen, moet u een van de volgende methoden gebruiken:
>
> * De `storm kill TOPOLOGYNAME` opdracht vanaf de opdrachtregel
> * De **Kill** knop in de Storm-gebruikersinterface.


## <a name="next-steps"></a>Volgende stappen

Zie de volgende documenten voor andere manieren om te Python gebruiken met HDInsight:

* [Het gebruik van Python voor streaming MapReduce-taken](hdinsight-hadoop-streaming-python.md)
* [Het gebruik van Python gebruiker gedefinieerde functies (UDF) in Pig en Hive](hdinsight-python.md)
