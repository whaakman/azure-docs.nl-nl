---
title: Gebruik Caffe op Azure HDInsight Spark voor gedistribueerde grondige learning | Microsoft Docs
description: Gebruik Caffe op Azure HDInsight Spark voor gedistribueerde grondige learning
services: hdinsight
documentationcenter: 
author: xiaoyongzhu
manager: asadk
editor: cgronlun
tags: azure-portal
ms.assetid: 71dcd1ad-4cad-47ad-8a9d-dcb7fa3c2ff9
ms.service: hdinsight
ms.custom: hdinsightactive
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/17/2017
ms.author: xiaoyzhu
ms.openlocfilehash: 7565efd82945f21b83471ee66098cd476b7bb59f
ms.sourcegitcommit: b07d06ea51a20e32fdc61980667e801cb5db7333
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/08/2017
---
# <a name="use-caffe-on-azure-hdinsight-spark-for-distributed-deep-learning"></a>Gebruik Caffe op Azure HDInsight Spark voor gedistribueerde grondige learning


## <a name="introduction"></a>Inleiding

Grondige learning is van invloed op alle gegevens van de gezondheidszorg voor verzending naar productie en meer. Bedrijven schakelt grondig learning voor het oplossen van problemen met vaste, zoals [installatiekopie classificatie](http://blogs.microsoft.com/next/2015/12/10/microsoft-researchers-win-imagenet-computer-vision-challenge/), [spraakherkenning](http://googleresearch.blogspot.jp/2015/08/the-neural-networks-behind-google-voice.html)erkenning object en de vertaling van de computer. 

Er zijn [veel populaire frameworks](https://en.wikipedia.org/wiki/Comparison_of_deep_learning_software), waaronder [Microsoft cognitieve Toolkit](https://www.microsoft.com/en-us/research/product/cognitive-toolkit/), [Tensorflow](https://www.tensorflow.org/), MXNet, Theano, enzovoort. Caffe is een van de grootste beroemdheid frameworks voor neurale netwerk wordt niet symbolische (imperatieve) en veel worden gebruikt in veel gebieden, waaronder computer vision. Bovendien [CaffeOnSpark](http://yahoohadoop.tumblr.com/post/139916563586/caffeonspark-open-sourced-for-distributed-deep) combineert Caffe met Apache Spark, in welk geval diep learning gemakkelijk kan worden gebruikt op een bestaand Hadoop-cluster. U kunt grondige learning samen met Spark ETL pijplijnen, verminderen system complexiteit en latentie voor learning volledige oplossing gebruiken.

[HDInsight](https://azure.microsoft.com/en-us/services/hdinsight/) een cloud Hadoop biedt die voorziet in analytische clusters geoptimaliseerde open-source Spark, Hive, Hadoop, HBase, Storm, Kafka en R Server. HDInsight wordt ondersteund door een SLA met 99,9%. Elk van deze big data-technologieën en ISV-toepassingen is eenvoudig te implementeren als beheerde clusters met beveiligings- en bewaking voor ondernemingen.

In dit artikel laat zien hoe u voor het installeren van [Caffe op Spark](https://github.com/yahoo/CaffeOnSpark) voor een HDInsight-cluster. In dit artikel gebruikt ook de ingebouwde MNIST demo om het gebruik van gedistribueerd grondige Learning met HDInsight Spark op CPU's weer te geven.

Er zijn vier hoofdstappen downloaden werken op HDInsight.

1. De vereiste afhankelijkheden te installeren op alle knooppunten
2. Caffe op Spark voor HDInsight voor het hoofdknooppunt bouwen
3. De vereiste bibliotheken voor alle worker-knooppunten distribueren
4. Een model Caffe opstellen en voer deze in een gedistribueerde manier.

Aangezien HDInsight een PaaS-oplossing is, biedt uitstekende platformfuncties - zodat u gemakkelijk een aantal taken uit te voeren. Een van de functies die we veel in dit blogbericht gebruiken heet [scriptactie](https://docs.microsoft.com/azure/hdinsight/hdinsight-hadoop-customize-cluster-linux), waarmee u de shell-opdrachten voor het aanpassen van de clusterknooppunten (hoofdknooppunt, werkrolknooppunt of edge-knooppunt) kunt uitvoeren.

## <a name="step-1--install-the-required-dependencies-on-all-the-nodes"></a>Stap 1: De vereiste afhankelijkheden installeren op alle knooppunten

Om te beginnen, moet voor het installeren van de afhankelijkheden die we nodig hebben. De site Caffe en [CaffeOnSpark site](https://github.com/yahoo/CaffeOnSpark/wiki/GetStarted_yarn) sommige wiki nuttig voor het installeren van de afhankelijkheden voor Spark op YARN-modus biedt. HDInsight gebruikt ook Spark op YARN-modus. Echter, moet enkele meer afhankelijkheden voor HDInsight platform toevoegen. Om dit te doen we gebruiken een scriptactie en uitvoeren op alle hoofdknooppunten en worker-knooppunten. Deze scriptactie duurt ongeveer 20 minuten, zoals die afhankelijkheden is ook afhankelijk van andere pakketten zijn. U moet deze plaatsen op een locatie die toegankelijk is voor uw HDInsight-cluster, zoals een GitHub-locatie of de standaard BLOB storage-account.

    #!/bin/bash
    #Please be aware that installing the below will add additional 20 mins to cluster creation because of the dependencies
    #installing all dependencies, including the ones mentioned in http://caffe.berkeleyvision.org/install_apt.html, as well a few packages that are not included in HDInsight, such as gflags, glog, lmdb, numpy
    #It seems numpy will only needed during compilation time, but for safety purpose we install them on all the nodes

    sudo apt-get install -y libprotobuf-dev libleveldb-dev libsnappy-dev libopencv-dev libhdf5-serial-dev protobuf-compiler maven libatlas-base-dev libgflags-dev libgoogle-glog-dev liblmdb-dev build-essential  libboost-all-dev python-numpy python-scipy python-matplotlib ipython ipython-notebook python-pandas python-sympy python-nose

    #install protobuf
    wget https://github.com/google/protobuf/releases/download/v2.5.0/protobuf-2.5.0.tar.gz
    sudo tar xzvf protobuf-2.5.0.tar.gz -C /tmp/
    cd /tmp/protobuf-2.5.0/
    sudo ./configure
    sudo make
    sudo make check
    sudo make install
    sudo ldconfig
    echo "protobuf installation done"


Er zijn twee stappen in de scriptactie. De eerste stap is het de vereiste bibliotheken installeren. Deze bibliotheken bevatten de benodigde bibliotheken voor zowel Caffe compileren (zoals gflags, glog) en het uitvoeren van Caffe (zoals numpy). We libatlas voor CPU-optimalisatie gebruiken, maar u kunt de wiki CaffeOnSpark altijd volgen over het installeren van andere bibliotheken optimalisatie zoals MKL of CUDA (voor GPU).

De tweede stap is het downloaden en protobuf 2.5.0 voor Caffe installeren tijdens de runtime worden gecompileerd. Protobuf 2.5.0 [is vereist](https://github.com/yahoo/CaffeOnSpark/issues/87), maar deze versie niet beschikbaar als een pakket op Ubuntu 16, is dus moeten we er tijdens het compileren van de broncode. Er zijn ook een aantal bronnen op het Internet voor het eerst compileren. Zie voor meer informatie [hier](http://jugnu-life.blogspot.com/2013/09/install-protobuf-25-on-ubuntu.html).

Om te beginnen, kunt u net uitvoeren met deze scriptactie op basis van uw cluster voor alle worker-knooppunten en hoofdknooppunten (voor HDInsight 3.5). U kunt de scriptacties uitvoeren op een bestaand cluster, of gebruik scriptacties tijdens het maken van het cluster. Zie de documentatie voor meer informatie over de scriptacties [hier](https://docs.microsoft.com/azure/hdinsight/hdinsight-hadoop-customize-cluster-linux#view-history-promote-and-demote-script-actions).

![Scriptacties afhankelijkheden installeren](./media/apache-spark-deep-learning-caffe/Script-Action-1.png)


## <a name="step-2-build-caffe-on-spark-for-hdinsight-on-the-head-node"></a>Stap 2: Caffe op Spark voor HDInsight voor het hoofdknooppunt bouwen

De tweede stap is het bouwen van Caffe op de headnode, en distribueert u vervolgens de gecompileerde bibliotheken voor alle worker-knooppunten. In deze stap moet u [ssh in uw headnode](https://docs.microsoft.com/azure/hdinsight/hdinsight-hadoop-linux-use-ssh-unix). Daarna kunt u moet volgen de [CaffeOnSpark buildproces](https://github.com/yahoo/CaffeOnSpark/wiki/GetStarted_yarn). Hieronder ziet u het script dat kunt u CaffeOnSpark bij het maken van een aantal extra stappen. 

    #!/bin/bash
    git clone https://github.com/yahoo/CaffeOnSpark.git --recursive
    export CAFFE_ON_SPARK=$(pwd)/CaffeOnSpark

    pushd ${CAFFE_ON_SPARK}/caffe-public/
    cp Makefile.config.example Makefile.config
    echo "INCLUDE_DIRS += ${JAVA_HOME}/include" >> Makefile.config
    #Below configurations might need to be updated based on actual cases. For example, if you are using GPU, or using a different BLAS library, you may want to update those settings accordingly.
    echo "CPU_ONLY := 1" >> Makefile.config
    echo "BLAS := atlas" >> Makefile.config
    echo "INCLUDE_DIRS += /usr/include/hdf5/serial/" >> Makefile.config
    echo "LIBRARY_DIRS += /usr/lib/x86_64-linux-gnu/hdf5/serial/" >> Makefile.config
    popd

    #compile CaffeOnSpark
    pushd ${CAFFE_ON_SPARK}
    #always clean up the environment before building (especially when rebuiding), or there will be errors such as "failed to execute goal org.apache.maven.plugins:maven-antrun-plugin:1.7:run (proto) on project caffe-distri: An Ant BuildException has occured: exec returned: 2"
    make clean 
    #the build step usually takes 20~30 mins, since it has a lot maven dependencies
    make build 
    popd
    export LD_LIBRARY_PATH=${CAFFE_ON_SPARK}/caffe-public/distribute/lib:${CAFFE_ON_SPARK}/caffe-distri/distribute/lib

    hadoop fs -mkdir -p wasb:///projects/machine_learning/image_dataset

    ${CAFFE_ON_SPARK}/scripts/setup-mnist.sh
    hadoop fs -put -f ${CAFFE_ON_SPARK}/data/mnist_*_lmdb wasb:///projects/machine_learning/image_dataset/

    ${CAFFE_ON_SPARK}/scripts/setup-cifar10.sh
    hadoop fs -put -f ${CAFFE_ON_SPARK}/data/cifar10_*_lmdb wasb:///projects/machine_learning/image_dataset/

    #put the already compiled CaffeOnSpark libraries to wasb storage, then read back to each node using script actions. This is because CaffeOnSpark requires all the nodes have the libarries
    hadoop fs -mkdir -p /CaffeOnSpark/caffe-public/distribute/lib/
    hadoop fs -mkdir -p /CaffeOnSpark/caffe-distri/distribute/lib/
    hadoop fs -put CaffeOnSpark/caffe-distri/distribute/lib/* /CaffeOnSpark/caffe-distri/distribute/lib/
    hadoop fs -put CaffeOnSpark/caffe-public/distribute/lib/* /CaffeOnSpark/caffe-public/distribute/lib/

Wellicht moet u meer dan wat de documentatie van CaffeOnSpark staat. De wijzigingen zijn:
- Ga naar de CPU alleen en libatlas bepaalde hiervoor gebruiken.
- Plaats de gegevenssets naar de BLOB-opslag is een gedeelde locatie die toegankelijk is voor alle worker-knooppunten voor later gebruik.
- Plaats de gecompileerde Caffe bibliotheken naar BLOB storage en later u die bibliotheken voor alle knooppunten met behulp van scriptacties om te voorkomen dat extra compileertijd kopiëren.


### <a name="troubleshooting-an-ant-buildexception-has-occurred-exec-returned-2"></a>Voor probleemoplossing: Een BuildException Ant is opgetreden: exec geretourneerd: 2

Wanneer u eerst probeert CaffeOnSpark bouwen, soms wordt aangegeven

    failed to execute goal org.apache.maven.plugins:maven-antrun-plugin:1.7:run (proto) on project caffe-distri: An Ant BuildException has occured: exec returned: 2

Opschonen van de opslagplaats code door "Maak schone" en voer 'Maak bouwen' Dit probleem kunt oplossen, zolang u de juiste afhankelijkheden hebben.

### <a name="troubleshooting-maven-repository-connection-time-out"></a>Voor probleemoplossing: Maven-opslagplaats verbindingstime-out

Soms geeft maven een verbinding time-outfout optreedt, vergelijkbaar met het volgende fragment:

    Retry:
    [INFO] Downloading: https://repo.maven.apache.org/maven2/com/twitter/chill_2.11/0.8.0/chill_2.11-0.8.0.jar
    Feb 01, 2017 5:14:49 AM org.apache.maven.wagon.providers.http.httpclient.impl.execchain.RetryExec execute
    INFO: I/O exception (java.net.SocketException) caught when processing request to {s}->https://repo.maven.apache.org:443: Connection timed out (Read failed)

U moet na een paar minuten opnieuw.


### <a name="troubleshooting-test-failure-for-caffe"></a>Voor probleemoplossing: Fout voor Caffe testen

Waarschijnlijk ziet u een test-fout bij het uitvoeren van de laatste controle op CaffeOnSpark. Dit is waarschijnlijk gerelateerd aan UTF-8-codering, maar u moet geen invloed op het gebruik van Caffe

    Run completed in 32 seconds, 78 milliseconds.
    Total number of tests run: 7
    Suites: completed 5, aborted 0
    Tests: succeeded 6, failed 1, canceled 0, ignored 0, pending 0
    *** 1 TEST FAILED ***

## <a name="step-3-distribute-the-required-libraries-to-all-the-worker-nodes"></a>Stap 3: Distribueert vereiste bibliotheken voor alle worker-knooppunten

De volgende stap is het distribueren van de tapewisselaars (in feite de bibliotheken in CaffeOnSpark/caffe-openbare/distribueren/lib/en CaffeOnSpark/caffe-distri/distribueren/lib /) voor alle knooppunten. In stap 2, we deze bibliotheken in BLOB storage plaatsen en in deze stap gebruiken we scriptacties om deze te kopiëren naar de hoofdknooppunten en worker-knooppunten.

Voer hiertoe een scriptactie zoals weergegeven in het volgende fragment:

    #!/bin/bash
    hadoop fs -get wasb:///CaffeOnSpark /home/changetoyourusername/

Zorg ervoor dat u wilt, wijs de juiste locatie specifieke uw cluster)

Omdat u in stap 2, we plaatsen in de BLOB-opslag die toegankelijk is voor alle knooppunten, in deze stap kopiëren alleen we deze voor alle knooppunten.

## <a name="step-4-compose-a-caffe-model-and-run-it-in-a-distributed-manner"></a>Stap 4: Een model Caffe opstellen en deze in een gedistribueerde manier uitvoeren

Caffe is na het uitvoeren van de voorgaande stappen geïnstalleerd. De volgende stap is het schrijven van een model Caffe. 

Caffe is een 'expressieve architectuur', waarbij voor het opstellen van een model, u alleen hoeft te definiëren van een configuratiebestand met en zonder codering helemaal (in de meeste gevallen). Dus laten we er. 

Het model dat wordt getraind is een voorbeeld-model voor MNIST training. De database MNIST geschreven cijfers heeft een trainingset 60.000 voorbeelden en geen testset van 10.000 voorbeelden. Dit is een subset van een grotere set van NIST beschikbaar. De cijfers zijn grootte genormaliseerd en in het midden van de installatiekopie van een vaste grootte. CaffeOnSpark heeft sommige scripts voor het downloaden van de gegevensset en deze converteren naar de juiste indeling.

CaffeOnSpark biedt een aantal voorbeelden van de netwerk-topologieën voor MNIST training. Er is een goed ontwerp van het splitsen van de netwerkarchitectuur (de topologie van het netwerk) en optimalisatie van. In dit geval zijn er twee bestanden vereist: 

het bestand 'Solver '' (${CAFFE_ON_SPARK}/data/lenet_memory_solver.prototxt) wordt gebruikt voor de optimalisatie overzien en updates van de parameter wordt gegenereerd. Bijvoorbeeld, definieert of CPU of GPU wordt gebruikt, wat is de dynamiek, het aantal iteraties zijn, enzovoort. Wordt ook gedefinieerd welke netwerktopologie neuron moet het programma gebruiken (dit is het tweede bestand we moeten). Zie voor meer informatie over Solver [Caffe documentatie](http://caffe.berkeleyvision.org/tutorial/solver.html).

We gebruiken CPU in plaats van GPU, moeten we voor dit voorbeeld de laatste regel te wijzigen:

    # solver mode: CPU or GPU
    solver_mode: CPU

![Caffe Config](./media/apache-spark-deep-learning-caffe/Caffe-1.png)

U kunt andere regels indien nodig wijzigen.

De tweede (${CAFFE_ON_SPARK}/data/lenet_memory_train_test.prototxt)-bestand definieert hoe het netwerk neuron eruit, en de relevante invoer- en -uitvoerbestand. Ook moet het bestand om de locatie van de gegevens training bijwerken. Wijzig het volgende gedeelte in lenet_memory_train_test.prototxt (u moet verwijzen naar de juiste locatie die specifiek zijn voor uw cluster):

- Wijzig de 'file:/Users/mridul/bigml/demodl/mnist_train_lmdb' in ' wasb: / / / projecten/machine_learning/image_dataset/mnist_train_lmdb '
- wijzigen in 'file:/Users/mridul/bigml/demodl/mnist_test_lmdb/' ' wasb: / / / projecten/machine_learning/image_dataset/mnist_test_lmdb '

![Caffe Config](./media/apache-spark-deep-learning-caffe/Caffe-2.png)

Raadpleeg voor meer informatie over het definiëren van het netwerk de [Caffe documentatie over MNIST gegevensset](http://caffe.berkeleyvision.org/gathered/examples/mnist.html)

In dit artikel gebruiken we in dit voorbeeld MNIST. De volgende opdrachten uitvoeren vanaf het hoofdknooppunt:

    spark-submit --master yarn --deploy-mode cluster --num-executors 8 --files ${CAFFE_ON_SPARK}/data/lenet_memory_solver.prototxt,${CAFFE_ON_SPARK}/data/lenet_memory_train_test.prototxt --conf spark.driver.extraLibraryPath="${LD_LIBRARY_PATH}" --conf spark.executorEnv.LD_LIBRARY_PATH="${LD_LIBRARY_PATH}" --class com.yahoo.ml.caffe.CaffeOnSpark ${CAFFE_ON_SPARK}/caffe-grid/target/caffe-grid-0.1-SNAPSHOT-jar-with-dependencies.jar -train -features accuracy,loss -label label -conf lenet_memory_solver.prototxt -devices 1 -connection ethernet -model wasb:///mnist.model -output wasb:///mnist_features_result

De voorgaande opdracht distribueert de vereiste bestanden (lenet_memory_solver.prototxt en lenet_memory_train_test.prototxt) naar elke container YARN. De opdracht stelt ook het relevante pad van elke Spark stuurprogramma/executor op LD_LIBRARY_PATH. LD_LIBRARY_PATH is gedefinieerd in de vorige codefragment en verwijst naar de locatie die CaffeOnSpark bibliotheken is. 

## <a name="monitoring-and-troubleshooting"></a>Bewaking en probleemoplossing

We gebruiken YARN clustermodus, in dat geval het Spark-stuurprogramma wordt gepland om een willekeurige container (en een willekeurige werkrolknooppunt) weergegeven alleen in de console ongeveer als volgt uitvoeren:

    17/02/01 23:22:16 INFO Client: Application report for application_1485916338528_0015 (state: RUNNING)

Als u weten wat er gebeurd is wilt, moet u doorgaans de Spark ophalen van stuurprogramma-logboek met meer informatie. In dit geval moet u gaat u naar de gebruikersinterface van YARN de relevante YARN-logboeken vinden. U kunt de gebruikersinterface van YARN door deze URL krijgen: 

    https://yourclustername.azurehdinsight.net/yarnui
   
![GEBRUIKERSINTERFACE VAN YARN](./media/apache-spark-deep-learning-caffe/YARN-UI-1.png)

U kunt bekijken hoeveel resources worden toegewezen voor deze bepaalde toepassing op te nemen. U kunt klikken op de koppeling "Scheduler" en vervolgens ziet u dat voor deze toepassing, er zijn 9 containers die worden uitgevoerd. We vragen YARN voor 8 Executor en een andere container is voor stuurprogramma-proces. 

![YARN Scheduler](./media/apache-spark-deep-learning-caffe/YARN-Scheduler.png)

U kunt de logboeken van de container of stuurprogramma logboeken controleren of er fouten. Voor stuurprogramma zich aanmeldt, kunt u de toepassings-ID in de gebruikersinterface van YARN Klik op de knop 'Logboeken'. De stuurprogramma-Logboeken geschreven in stderr.

![GEBRUIKERSINTERFACE VAN YARN 2](./media/apache-spark-deep-learning-caffe/YARN-UI-2.png)

Bijvoorbeeld, ziet u mogelijk enkele van de fout hieronder in de stuurprogramma-logboeken die aangeeft dat u te veel Executor toewijzen.

    17/02/01 07:26:06 ERROR ApplicationMaster: User class threw exception: java.lang.IllegalStateException: Insufficient training data. Please adjust hyperparameters or increase dataset.
    java.lang.IllegalStateException: Insufficient training data. Please adjust hyperparameters or increase dataset.
        at com.yahoo.ml.caffe.CaffeOnSpark.trainWithValidation(CaffeOnSpark.scala:261)
        at com.yahoo.ml.caffe.CaffeOnSpark$.main(CaffeOnSpark.scala:42)
        at com.yahoo.ml.caffe.CaffeOnSpark.main(CaffeOnSpark.scala)
        at sun.reflect.NativeMethodAccessorImpl.invoke0(Native Method)
        at sun.reflect.NativeMethodAccessorImpl.invoke(NativeMethodAccessorImpl.java:62)
        at sun.reflect.DelegatingMethodAccessorImpl.invoke(DelegatingMethodAccessorImpl.java:43)
        at java.lang.reflect.Method.invoke(Method.java:498)
        at org.apache.spark.deploy.yarn.ApplicationMaster$$anon$2.run(ApplicationMaster.scala:627)

Soms kan het probleem kan zich voordoen in Executor in plaats van stuurprogramma's. In dit geval moet u de logboeken van de container. U kunt altijd de container-logboeken ophalen en vervolgens de container is mislukt. Bijvoorbeeld mogelijk voldoet deze fout aan wanneer Caffe wordt uitgevoerd.

    17/02/01 07:12:05 WARN YarnAllocator: Container marked as failed: container_1485916338528_0008_05_000005 on host: 10.0.0.14. Exit status: 134. Diagnostics: Exception from container-launch.
    Container id: container_1485916338528_0008_05_000005
    Exit code: 134
    Exception message: /bin/bash: line 1: 12230 Aborted                 (core dumped) LD_LIBRARY_PATH=/usr/hdp/current/hadoop-client/lib/native:/usr/hdp/current/hadoop-client/lib/native/Linux-amd64-64:/home/xiaoyzhu/CaffeOnSpark/caffe-public/distribute/lib:/home/xiaoyzhu/CaffeOnSpark/caffe-distri/distribute/lib /usr/lib/jvm/java-8-openjdk-amd64/bin/java -server -Xmx4608m '-Dhdp.version=' '-Detwlogger.component=sparkexecutor' '-DlogFilter.filename=SparkLogFilters.xml' '-DpatternGroup.filename=SparkPatternGroups.xml' '-Dlog4jspark.root.logger=INFO,console,RFA,ETW,Anonymizer' '-Dlog4jspark.log.dir=/var/log/sparkapp/${user.name}' '-Dlog4jspark.log.file=sparkexecutor.log' '-Dlog4j.configuration=file:/usr/hdp/current/spark2-client/conf/log4j.properties' '-Djavax.xml.parsers.SAXParserFactory=com.sun.org.apache.xerces.internal.jaxp.SAXParserFactoryImpl' -Djava.io.tmpdir=/mnt/resource/hadoop/yarn/local/usercache/xiaoyzhu/appcache/application_1485916338528_0008/container_1485916338528_0008_05_000005/tmp '-Dspark.driver.port=43942' '-Dspark.history.ui.port=18080' '-Dspark.ui.port=0' -Dspark.yarn.app.container.log.dir=/mnt/resource/hadoop/yarn/log/application_1485916338528_0008/container_1485916338528_0008_05_000005 -XX:OnOutOfMemoryError='kill %p' org.apache.spark.executor.CoarseGrainedExecutorBackend --driver-url spark://CoarseGrainedScheduler@10.0.0.13:43942 --executor-id 4 --hostname 10.0.0.14 --cores 3 --app-id application_1485916338528_0008 --user-class-path file:/mnt/resource/hadoop/yarn/local/usercache/xiaoyzhu/appcache/application_1485916338528_0008/container_1485916338528_0008_05_000005/__app__.jar > /mnt/resource/hadoop/yarn/log/application_1485916338528_0008/container_1485916338528_0008_05_000005/stdout 2> /mnt/resource/hadoop/yarn/log/application_1485916338528_0008/container_1485916338528_0008_05_000005/stderr

    Stack trace: ExitCodeException exitCode=134: /bin/bash: line 1: 12230 Aborted                 (core dumped) LD_LIBRARY_PATH=/usr/hdp/current/hadoop-client/lib/native:/usr/hdp/current/hadoop-client/lib/native/Linux-amd64-64:/home/xiaoyzhu/CaffeOnSpark/caffe-public/distribute/lib:/home/xiaoyzhu/CaffeOnSpark/caffe-distri/distribute/lib /usr/lib/jvm/java-8-openjdk-amd64/bin/java -server -Xmx4608m '-Dhdp.version=' '-Detwlogger.component=sparkexecutor' '-DlogFilter.filename=SparkLogFilters.xml' '-DpatternGroup.filename=SparkPatternGroups.xml' '-Dlog4jspark.root.logger=INFO,console,RFA,ETW,Anonymizer' '-Dlog4jspark.log.dir=/var/log/sparkapp/${user.name}' '-Dlog4jspark.log.file=sparkexecutor.log' '-Dlog4j.configuration=file:/usr/hdp/current/spark2-client/conf/log4j.properties' '-Djavax.xml.parsers.SAXParserFactory=com.sun.org.apache.xerces.internal.jaxp.SAXParserFactoryImpl' -Djava.io.tmpdir=/mnt/resource/hadoop/yarn/local/usercache/xiaoyzhu/appcache/application_1485916338528_0008/container_1485916338528_0008_05_000005/tmp '-Dspark.driver.port=43942' '-Dspark.history.ui.port=18080' '-Dspark.ui.port=0' -Dspark.yarn.app.container.log.dir=/mnt/resource/hadoop/yarn/log/application_1485916338528_0008/container_1485916338528_0008_05_000005 -XX:OnOutOfMemoryError='kill %p' org.apache.spark.executor.CoarseGrainedExecutorBackend --driver-url spark://CoarseGrainedScheduler@10.0.0.13:43942 --executor-id 4 --hostname 10.0.0.14 --cores 3 --app-id application_1485916338528_0008 --user-class-path file:/mnt/resource/hadoop/yarn/local/usercache/xiaoyzhu/appcache/application_1485916338528_0008/container_1485916338528_0008_05_000005/__app__.jar > /mnt/resource/hadoop/yarn/log/application_1485916338528_0008/container_1485916338528_0008_05_000005/stdout 2> /mnt/resource/hadoop/yarn/log/application_1485916338528_0008/container_1485916338528_0008_05_000005/stderr

        at org.apache.hadoop.util.Shell.runCommand(Shell.java:933)
        at org.apache.hadoop.util.Shell.run(Shell.java:844)
        at org.apache.hadoop.util.Shell$ShellCommandExecutor.execute(Shell.java:1123)
        at org.apache.hadoop.yarn.server.nodemanager.DefaultContainerExecutor.launchContainer(DefaultContainerExecutor.java:225)
        at org.apache.hadoop.yarn.server.nodemanager.containermanager.launcher.ContainerLaunch.call(ContainerLaunch.java:317)
        at org.apache.hadoop.yarn.server.nodemanager.containermanager.launcher.ContainerLaunch.call(ContainerLaunch.java:83)
        at java.util.concurrent.FutureTask.run(FutureTask.java:266)
        at java.util.concurrent.ThreadPoolExecutor.runWorker(ThreadPoolExecutor.java:1142)
        at java.util.concurrent.ThreadPoolExecutor$Worker.run(ThreadPoolExecutor.java:617)
        at java.lang.Thread.run(Thread.java:745)


    Container exited with a non-zero exit code 134

In dit geval moet u de mislukte container-ID (dit is in het bovenstaande geval container_1485916338528_0008_05_000005). U hoeft uit te voeren 

    yarn logs -containerId container_1485916338528_0008_03_000005

van de headnode. Nadat u hebt gecontroleerd container mislukt, wordt dit veroorzaakt door met GPU-modus (waarbij u gebruik CPU-modus in plaats daarvan) in lenet_memory_solver.prototxt.

    17/02/01 07:10:48 INFO LMDB: Batch size:100
    WARNING: Logging before InitGoogleLogging() is written to STDERR
    F0201 07:10:48.309725 11624 common.cpp:79] Cannot use GPU in CPU-only Caffe: check mode.


## <a name="getting-results"></a>Ophalen van resultaten

Aangezien we 8 Executor toewijzen wilt en de netwerktopologie eenvoudig is, moet het alleen ongeveer 30 minuten duren om uit te voeren van het resultaat. Vanaf de opdrachtregel, ziet u dat we plaatst u het model voor wasb:///mnist.model, en de resultaten naar een map met de naam wasb: / / / mnist_features_result.

U kunt de resultaten krijgen door te voeren

    hadoop fs -cat hdfs:///mnist_features_result/*

en het resultaat lijkt op:

    {"SampleID":"00009597","accuracy":[1.0],"loss":[0.028171852],"label":[2.0]}
    {"SampleID":"00009598","accuracy":[1.0],"loss":[0.028171852],"label":[6.0]}
    {"SampleID":"00009599","accuracy":[1.0],"loss":[0.028171852],"label":[1.0]}
    {"SampleID":"00009600","accuracy":[0.97],"loss":[0.0677709],"label":[5.0]}
    {"SampleID":"00009601","accuracy":[0.97],"loss":[0.0677709],"label":[0.0]}
    {"SampleID":"00009602","accuracy":[0.97],"loss":[0.0677709],"label":[1.0]}
    {"SampleID":"00009603","accuracy":[0.97],"loss":[0.0677709],"label":[2.0]}
    {"SampleID":"00009604","accuracy":[0.97],"loss":[0.0677709],"label":[3.0]}
    {"SampleID":"00009605","accuracy":[0.97],"loss":[0.0677709],"label":[4.0]}

De SampleID vertegenwoordigt de ID in de gegevensset MNIST, en het label is het nummer van het model.


## <a name="conclusion"></a>Conclusie

U hebt geprobeerd in deze documentatie CaffeOnSpark installeren met het uitvoeren van een eenvoudig voorbeeld. HDInsight is een volledig beheerde cloud gedistribueerde compute-platform en de beste plaats voor het uitvoeren van machine learning en geavanceerde analyses werkbelastingen op grote gegevensset en voor gedistribueerde grondige learning, kunt u Caffe op HDInsight Spark grondige learning taken uit te voeren.


## <a name="seealso"></a>Zie ook
* [Overzicht: Apache Spark in Azure HDInsight](apache-spark-overview.md)

### <a name="scenarios"></a>Scenario's
* [Spark met Machine Learning: Spark in HDInsight gebruiken voor het analyseren van de gebouwtemperatuur met behulp van HVAC-gegevens](apache-spark-ipython-notebook-machine-learning.md)
* [Spark met Machine Learning: Spark in HDInsight gebruiken om voedselinspectieresultaten te voorspellen](apache-spark-machine-learning-mllib-ipython.md)

### <a name="manage-resources"></a>Resources beheren
* [Resources beheren voor het Apache Spark-cluster in Azure HDInsight](apache-spark-resource-manager.md)

