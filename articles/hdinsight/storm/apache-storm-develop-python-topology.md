---
title: Apache Storm met de serveronderdelen van de Python - Azure HDInsight
description: Informatie over het maken van een Apache Storm-topologie die gebruikmaakt van Python-onderdelen.
services: hdinsight
author: jasonwhowell
editor: jasonwhowell
keywords: Apache storm-python
ms.service: hdinsight
ms.custom: hdinsightactive,hdiseo17may2017
ms.topic: conceptual
ms.date: 04/30/2018
ms.author: jasonh
ms.openlocfilehash: 8c26bfc85c70addebd5e68fad7769faa63e07bea
ms.sourcegitcommit: 35ceadc616f09dd3c88377a7f6f4d068e23cceec
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/08/2018
ms.locfileid: "39623014"
---
# <a name="develop-apache-storm-topologies-using-python-on-hdinsight"></a>Apache Storm-topologieën met behulp van Python op HDInsight ontwikkelen

Informatie over het maken van een Apache Storm-topologie die gebruikmaakt van Python-onderdelen. Apache Storm biedt ondersteuning voor meerdere talen, zelfs zodat u kunt het combineren van componenten van verschillende talen in één topologie. De lichtstroom framework (geïntroduceerd met Storm 0.10.0) kunt u eenvoudig om oplossingen te maken die gebruikmaken van Python-onderdelen.

> [!IMPORTANT]
> De informatie in dit document is getest met Storm op HDInsight 3.6. Linux is het enige besturingssysteem dat wordt gebruikt in HDInsight-versie 3.4 of hoger. Zie [HDInsight retirement on Windows](../hdinsight-component-versioning.md#hdinsight-windows-retirement) (HDInsight buiten gebruik gestel voor Windows) voor meer informatie.

De code voor dit project is beschikbaar op [ https://github.com/Azure-Samples/hdinsight-python-storm-wordcount ](https://github.com/Azure-Samples/hdinsight-python-storm-wordcount).

## <a name="prerequisites"></a>Vereisten

* Python 2.7 of hoger

* Java JDK 1.8 of hoger

* Maven 3

* (Optioneel) Een lokale ontwikkelingsomgeving voor Storm. Een lokale Storm-omgeving is alleen nodig als u wilt de topologie lokaal uitvoeren. Zie voor meer informatie, [een ontwikkelomgeving instellen](http://storm.apache.org/releases/1.1.2/Setting-up-development-environment.html).

## <a name="storm-multi-language-support"></a>Storm meertalige ondersteuning

Apache Storm is ontworpen voor gebruik met de onderdelen die zijn geschreven met behulp van elke programmeertaal. De onderdelen moeten weten hoe om te werken met de [Thrift-definitie voor Storm](https://github.com/apache/storm/blob/master/storm-core/src/storm.thrift). Voor Python, wordt een module geboden als onderdeel van de Apache Storm-project waarmee u eenvoudig een interface met Storm. U vindt deze module op [ https://github.com/apache/storm/blob/master/storm-multilang/python/src/main/resources/resources/storm.py ](https://github.com/apache/storm/blob/master/storm-multilang/python/src/main/resources/resources/storm.py).

Storm is een Java-proces dat wordt uitgevoerd op de Java Virtual Machine (JVM). Onderdelen die zijn geschreven in andere talen worden uitgevoerd als subprocessen. De Storm communiceert met deze met behulp van JSON-berichten verzonden via stdin/stdout subprocessen. Meer informatie over de communicatie tussen onderdelen kunnen worden gevonden in de [Multi-lang Protocol](https://storm.apache.org/documentation/Multilang-protocol.html) documentatie.

## <a name="python-with-the-flux-framework"></a>Python met het framework lichtstroom

Het framework lichtstroom kunt u Storm-topologieën los van de onderdelen te definiëren. Het framework lichtstroom gebruikt YAML voor het definiëren van de Storm-topologie. De volgende tekst is een voorbeeld van hoe om te verwijzen naar een Python-component in het YAML-document:

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

De klasse `FluxShellSpout` wordt gebruikt om te beginnen de `sentencespout.py` script waarmee de spout.

Lichtstroom wordt verwacht dat de Python-scripts in de `/resources` map binnen het jar-bestand met de topologie. Dus in dit voorbeeld bevat de Python-scripts in de `/multilang/resources` directory. De `pom.xml` deze met behulp van de volgende XML-bestand bevat:

```xml
<!-- include the Python components -->
<resource>
    <directory>${basedir}/multilang</directory>
    <filtering>false</filtering>
</resource>
```

Zoals eerder vermeld, is er een `storm.py` bestand dat de Thrift-definitie voor Storm implementeert. Het framework lichtstroom bevat `storm.py` automatisch wanneer het project is gemaakt, zodat u niet hoeft te hoeven maken over het opnemen van.

## <a name="build-the-project"></a>Het project bouwen

In de hoofdmap van het project, gebruik de volgende opdracht:

```bash
mvn clean compile package
```

Deze opdracht maakt u een `target/WordCount-1.0-SNAPSHOT.jar` -bestand met de gecompileerde topologie.

## <a name="run-the-topology-locally"></a>De topologie lokaal uitvoeren

De topologie als lokaal wilt uitvoeren, moet u de volgende opdracht gebruiken:

```bash
storm jar WordCount-1.0-SNAPSHOT.jar org.apache.storm.flux.Flux -l -R /topology.yaml
```

> [!NOTE]
> Deze opdracht moet een lokale ontwikkelingsomgeving van Storm. Zie voor meer informatie, [een ontwikkelomgeving instellen](http://storm.apache.org/releases/current/Setting-up-development-environment.html)

Zodra de topologie wordt gestart, het verzendt gegevens naar de lokale console die vergelijkbaar is met de volgende tekst:


    24302 [Thread-25-sentence-spout-executor[4 4]] INFO  o.a.s.s.ShellSpout - ShellLog pid:2436, name:sentence-spout Emiting the cow jumped over the moon
    24302 [Thread-30] INFO  o.a.s.t.ShellBolt - ShellLog pid:2438, name:splitter-bolt Emitting the
    24302 [Thread-28] INFO  o.a.s.t.ShellBolt - ShellLog pid:2437, name:counter-bolt Emitting years:160
    24302 [Thread-17-log-executor[3 3]] INFO  o.a.s.f.w.b.LogInfoBolt - {word=the, count=599}
    24303 [Thread-17-log-executor[3 3]] INFO  o.a.s.f.w.b.LogInfoBolt - {word=seven, count=302}
    24303 [Thread-17-log-executor[3 3]] INFO  o.a.s.f.w.b.LogInfoBolt - {word=dwarfs, count=143}
    24303 [Thread-25-sentence-spout-executor[4 4]] INFO  o.a.s.s.ShellSpout - ShellLog pid:2436, name:sentence-spout Emiting the cow jumped over the moon
    24303 [Thread-30] INFO  o.a.s.t.ShellBolt - ShellLog pid:2438, name:splitter-bolt Emitting cow
    24303 [Thread-17-log-executor[3 3]] INFO  o.a.s.f.w.b.LogInfoBolt - {word=four, count=160}


Als u wilt de topologie stoppen, gebruikt u __Ctrl + C__.

## <a name="run-the-storm-topology-on-hdinsight"></a>Uitvoeren de topologie voor Storm op HDInsight

1. Gebruik de volgende opdracht uit om te kopiëren de `WordCount-1.0-SNAPSHOT.jar` -bestand naar uw Storm op HDInsight-cluster:

    ```bash
    scp target\WordCount-1.0-SNAPSHOT.jar sshuser@mycluster-ssh.azurehdinsight.net
    ```

    Vervang `sshuser` met de SSH-gebruiker voor uw cluster. Vervang `mycluster` met de naam van het cluster. U mogelijk gevraagd het wachtwoord invoeren voor de SSH-gebruiker.

    Zie voor meer informatie over het gebruik van SSH en SCP [SSH gebruiken met HDInsight](../hdinsight-hadoop-linux-use-ssh-unix.md).

2. Zodra het bestand is geüpload, moet u een verbinding maken met het cluster via SSH:

    ```bash
    ssh sshuser@mycluster-ssh.azurehdinsight.net
    ```

3. Gebruik de volgende opdracht om te starten van de topologie op het cluster van de SSH-sessie:

    ```bash
    storm jar WordCount-1.0-SNAPSHOT.jar org.apache.storm.flux.Flux -r -R /topology.yaml
    ```

3. U kunt de Storm-gebruikersinterface gebruiken om de topologie op het cluster weer te geven. De Storm-gebruikersinterface bevindt zich in https://mycluster.azurehdinsight.net/stormui. Vervang `mycluster` met de clusternaam van uw.

> [!NOTE]
> Eenmaal is gestart, wordt er een Storm-topologie uitgevoerd totdat deze wordt gestopt. Als u wilt de topologie stoppen, moet u een van de volgende methoden gebruiken:
>
> * De `storm kill TOPOLOGYNAME` opdracht uit vanaf de opdrachtregel
> * De **Kill** knop in de Storm-gebruikersinterface.


## <a name="next-steps"></a>Volgende stappen

Zie de volgende documenten voor andere manieren om Python gebruiken met HDInsight:

* [Hoe u Python gebruikt voor het streamen van MapReduce-taken](../hadoop/apache-hadoop-streaming-python.md)
* [Het gebruik van Python gebruiker gedefinieerde functies (UDF's) in Pig en Hive](../hadoop/python-udf-hdinsight.md)
