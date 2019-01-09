---
title: Caffe gebruiken op Azure HDInsight Spark voor gedistribueerde deep learning
description: Caffe gebruiken op Azure HDInsight Spark voor gedistribueerde deep learning
services: hdinsight
author: hrasheed-msft
ms.author: hrasheed
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 02/17/2017
ms.openlocfilehash: 451ccff9747988ee019f2be9e0cccec12c9c1ef9
ms.sourcegitcommit: 818d3e89821d101406c3fe68e0e6efa8907072e7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/09/2019
ms.locfileid: "54118231"
---
# <a name="use-caffe-on-azure-hdinsight-spark-for-distributed-deep-learning"></a>Caffe gebruiken op Azure HDInsight Spark voor gedistribueerde deep learning


## <a name="introduction"></a>Inleiding

Deep learning is van invloed op Alles van healthcare vervoer naar productie, en meer. Bedrijven deep learning moeilijke problemen wilt oplossen, wordt [afbeeldingsclassificatie](https://blogs.microsoft.com/next/2015/12/10/microsoft-researchers-win-imagenet-computer-vision-challenge/), [spraakherkenning](https://googleresearch.blogspot.jp/2015/08/the-neural-networks-behind-google-voice.html), opname-object en vertalen van de computer. 

Er zijn [veel populaire frameworks](https://en.wikipedia.org/wiki/Comparison_of_deep_learning_software), waaronder [Microsoft Cognitive Toolkit](https://www.microsoft.com/en-us/research/product/cognitive-toolkit/), [Tensorflow](https://www.tensorflow.org/), [Apache MXNet](https://mxnet.apache.org/), Theano, enzovoort. [Caffe](https://caffe.berkeleyvision.org/) is een van de grootste beroemdheid niet symbolische (imperatieve) neural network-frameworks en veelvuldig worden gebruikt in veel gebieden, met inbegrip van de computer vision. Bovendien [CaffeOnSpark](https://yahoohadoop.tumblr.com/post/139916563586/caffeonspark-open-sourced-for-distributed-deep) combineert Caffe met Apache Spark, in welk geval deep learning eenvoudig kan worden gebruikt op een bestaande Hadoop-cluster. Voor volledige oplossing learning kunt u deep learning met Spark ' ETL-pijplijnen, te reduceren system complexiteit en latentie.

[HDInsight](https://azure.microsoft.com/services/hdinsight/) is een cloud Apache Hadoop aanbieding waarmee de geoptimaliseerde clusters voor open source-Analytics voor Apache Spark, Apache Hive, Apache Hadoop, Apache HBase, Apache Storm, Apache Kafka en ML-Services. HDInsight wordt ondersteund door een SLA van 99,9%. Elk van deze big data-technologieën en ISV-toepassingen is eenvoudig te implementeren als beheerde clusters met beveiliging en bewaking voor ondernemingen.

In dit artikel ziet u hoe u voor het installeren van [Caffe op Spark](https://github.com/yahoo/CaffeOnSpark) voor een HDInsight-cluster. In dit artikel wordt ook de ingebouwde MNIST-demo om weer te geven over het gebruik van gedistribueerd Deep Learning met behulp van HDInsight Spark op CPU's.

Er zijn vier stappen om de taak te voltooien:

1. Installeer de vereiste afhankelijkheden op alle knooppunten
2. Caffe op Spark voor HDInsight bouwen op het hoofdknooppunt
3. Distribueren van de vereiste bibliotheken op alle worker-knooppunten
4. Een model Caffe opstellen en deze in een gedistribueerde manier uitvoeren.

Omdat HDInsight een PaaS-oplossing is, biedt uitstekende platformfuncties - zodat u gemakkelijk sommige taken uit te voeren. Een van de functies die worden gebruikt in dit blogbericht heet [scriptactie](https://docs.microsoft.com/azure/hdinsight/hdinsight-hadoop-customize-cluster-linux), waarmee u de shell-opdrachten voor het aanpassen van de clusterknooppunten (hoofdknooppunt, worker-knooppunt of edge-knooppunt) kunt uitvoeren.

## <a name="step-1--install-the-required-dependencies-on-all-the-nodes"></a>Stap 1:  Installeer de vereiste afhankelijkheden op alle knooppunten

Om te beginnen, moet u de afhankelijkheden te installeren. De Caffe-site en [CaffeOnSpark site](https://github.com/yahoo/CaffeOnSpark/wiki/GetStarted_yarn) biedt enkele nuttige wiki voor het installeren van de afhankelijkheden voor Spark op YARN-modus. HDInsight maakt ook gebruik van Spark op YARN-modus. U moet echter een paar meer afhankelijkheden voor HDInsight-platform toevoegen. Om dit te doen, moet u een scriptactie gebruiken en uitvoeren op de hoofdknooppunten en worker-knooppunten. Deze scriptactie duurt ongeveer 20 minuten, zoals die afhankelijkheden is ook afhankelijk van andere pakketten zijn. U moet deze in een locatie die toegankelijk is voor uw HDInsight-cluster, zoals een GitHub-locatie of de standaard BLOB storage-account geplaatst.

    #!/bin/bash
    #Please be aware that installing the below will add additional 20 mins to cluster creation because of the dependencies
    #installing all dependencies, including the ones mentioned in https://caffe.berkeleyvision.org/install_apt.html, as well a few packages that are not included in HDInsight, such as gflags, glog, lmdb, numpy
    #It seems numpy will only needed during compilation time, but for safety purpose you install them on all the nodes

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


Er zijn twee stappen in de scriptactie. De eerste stap is het de vereiste bibliotheken installeren. Deze bibliotheken bevatten de benodigde bibliotheken voor zowel Caffe (zoals gflags, glog) compileren en uitvoeren van Caffe (zoals numpy). u libatlas gebruikt voor de optimalisatie van de CPU, maar u kunt altijd de wiki CaffeOnSpark volgen over het installeren van andere bibliotheken optimalisatie, zoals MKL of CUDA (voor GPU).

De tweede stap is om te downloaden, compileren en protobuf 2.5.0 voor Caffe tijdens runtime installeren. Protobuf 2.5.0 [is vereist](https://github.com/yahoo/CaffeOnSpark/issues/87), maar deze versie niet beschikbaar als een pakket op Ubuntu 16, is dus moet u tijdens het compileren van de broncode. Er zijn ook enkele bronnen op het Internet over het compileren. Zie voor meer informatie, [hier](https://jugnu-life.blogspot.com/2013/09/install-protobuf-25-on-ubuntu.html).

Om te beginnen, kunt u alleen uitvoeren met deze scriptactie op basis van uw cluster op alle worker-knooppunten en hoofdknooppunten (voor HDInsight 3.5). U kunt de scriptacties uitvoeren op een bestaand cluster of u scriptacties gebruiken tijdens het maken van het cluster. Zie de documentatie voor meer informatie over de scriptacties [hier](https://docs.microsoft.com/azure/hdinsight/hdinsight-hadoop-customize-cluster-linux#view-history-promote-and-demote-script-actions).

![Scriptacties voor het installeren van afhankelijkheden](./media/apache-spark-deep-learning-caffe/Script-Action-1.png)


## <a name="step-2-build-caffe-on-apache-spark-for-hdinsight-on-the-head-node"></a>Stap 2: Caffe op Apache Spark voor HDInsight opbouwen op het hoofdknooppunt

De tweede stap is het Caffe bouwen op het hoofdknooppunt en distribueert u vervolgens de gecompileerde bibliotheken die u moet alle worker-knooppunten. In deze stap moet u [ssh in uw hoofdknooppunt](https://docs.microsoft.com/azure/hdinsight/hdinsight-hadoop-linux-use-ssh-unix). Hierna kunt u moet volgen de [CaffeOnSpark bouwproces](https://github.com/yahoo/CaffeOnSpark/wiki/GetStarted_yarn). Hieronder ziet u het script dat kunt u CaffeOnSpark bouwen met een paar extra stappen. 

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
    #always clean up the environment before building (especially when rebuiding), or there will be errors such as "failed to execute goal org.apache.maven.plugins:maven-antrun-plugin:1.7:run (proto) on project caffe-distri: An Ant BuildException has occurred: exec returned: 2"
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

Mogelijk moet u meer dan wat de documentatie van CaffeOnSpark zegt. De wijzigingen zijn:
- Alleen wijzigen voor CPU en libatlas gebruiken voor dit specifieke doeleinde.
- De gegevenssets in de BLOB-opslag is een gedeelde locatie die toegankelijk is voor alle worker-knooppunten voor later gebruik plaatsen.
- De gecompileerde Caffe-bibliotheken in BLOB storage plaatsen en later kopieert u deze bibliotheken voor alle knooppunten met behulp van scriptacties om te voorkomen dat aanvullende compilatietijd.


### <a name="troubleshooting-an-ant-buildexception-has-occurred-exec-returned-2"></a>Probleemoplossing: Een BuildException Ant is opgetreden: exec geretourneerd: 2

Wanneer u eerst probeert te maken van CaffeOnSpark, soms de status

    failed to execute goal org.apache.maven.plugins:maven-antrun-plugin:1.7:run (proto) on project caffe-distri: An Ant BuildException has occurred: exec returned: 2

Schoon de codeopslagplaats door 'Maak schone' en voer 'zorg bouwen"om op te lossen dit probleem als u de juiste afhankelijkheden hebben.

### <a name="troubleshooting-maven-repository-connection-time-out"></a>Probleemoplossing: Verbindingstime-out maven-opslagplaats

Maven biedt ook een verbinding time-outfout optreedt, vergelijkbaar met het volgende codefragment:

    Retry:
    [INFO] Downloading: https://repo.maven.apache.org/maven2/com/twitter/chill_2.11/0.8.0/chill_2.11-0.8.0.jar
    Feb 01, 2017 5:14:49 AM org.apache.maven.wagon.providers.http.httpclient.impl.execchain.RetryExec execute
    INFO: I/O exception (java.net.SocketException) caught when processing request to {s}->https://repo.maven.apache.org:443: Connection timed out (Read failed)

U moet na een paar minuten opnieuw.


### <a name="troubleshooting-test-failure-for-caffe"></a>Probleemoplossing: Fout bij het testen van Caffe

Waarschijnlijk ziet u een testfout bij het uitvoeren van de laatste controle CaffeOnSpark. Dit is waarschijnlijk gerelateerd met UTF-8-codering, maar mag niet van invloed op het gebruik van Caffe

    Run completed in 32 seconds, 78 milliseconds.
    Total number of tests run: 7
    Suites: completed 5, aborted 0
    Tests: succeeded 6, failed 1, canceled 0, ignored 0, pending 0
    *** 1 TEST FAILED ***

## <a name="step-3-distribute-the-required-libraries-to-all-the-worker-nodes"></a>Stap 3: Distribueren van de vereiste bibliotheken op alle worker-knooppunten

De volgende stap is het distribueren van de bibliotheken (in feite de bibliotheken in CaffeOnSpark/caffe-openbare/distribueren/lib/en CaffeOnSpark/caffe-distri/distribueren/lib /) voor alle knooppunten. In stap 2, u deze bibliotheken in de BLOB-opslag plaatsen en in deze stap maakt u scriptacties gebruiken om te kopiëren naar de hoofdknooppunten en worker-knooppunten.

U doet dit door een scriptactie worden uitgevoerd zoals wordt weergegeven in het volgende codefragment:

    #!/bin/bash
    hadoop fs -get wasb:///CaffeOnSpark /home/changetoyourusername/

Zorg ervoor dat u wilt, wijs de juiste locatie specifieke uw cluster)

Omdat u in stap 2, waarin u het plaatst op de BLOB-opslag die toegankelijk is voor alle knooppunten, in deze stap kopiëren u alleen deze naar alle knooppunten.

## <a name="step-4-compose-a-caffe-model-and-run-it-in-a-distributed-manner"></a>Stap 4: Een model Caffe opstellen en deze in een gedistribueerde manier uitvoeren

Caffe is na het uitvoeren van de voorgaande stappen geïnstalleerd. De volgende stap is het schrijven van een model Caffe. 

Caffe is met behulp van een 'expressieve architectuur", waarbij voor het opstellen van een model, u alleen hoeft te definiëren van een configuratiebestand, en zonder code te schrijven op alle (in de meeste gevallen). Laten we er een kijkje nemen. 

Het model dat u training is een voorbeeldmodel voor MNIST-training. De MNIST-database met handgeschreven cijfers heeft een trainingsset van 60.000 voorbeelden en testset van 10.000 voorbeelden. Dit is een subset van een grotere set van NIST beschikbaar. De cijfers zijn genormaliseerd grootte en in het midden van de installatiekopie van een vaste grootte. CaffeOnSpark heeft sommige scripts downloaden van de gegevensset en deze converteren naar de juiste indeling.

CaffeOnSpark bevat enkele voorbeelden van netwerk-topologieën voor MNIST-training. Er is een goed ontwerp van het splitsen van de netwerkarchitectuur (de topologie van het netwerk) en optimalisatie. In dit geval zijn er twee bestanden vereist: 

het bestand 'Solver' (${CAFFE_ON_SPARK}/data/lenet_memory_solver.prototxt) wordt gebruikt voor toezicht houden op de optimalisatie van en het genereren van de parameter-updates. Bijvoorbeeld: bepaalt of CPU of GPU wordt gebruikt, wat is het momentum, het aantal iteraties zijn, enzovoort. Het definieert ook welke netwerktopologie neuron gemaakt die het programma moet gebruiken (dit is de tweede bestand dat u nodig hebt). Zie voor meer informatie over Solver [Caffe documentatie](https://caffe.berkeleyvision.org/tutorial/solver.html).

Omdat u CPU in plaats van GPU, moet u voor dit voorbeeld wordt de laatste regel te wijzigen:

    # solver mode: CPU or GPU
    solver_mode: CPU

![Caffe Config](./media/apache-spark-deep-learning-caffe/Caffe-1.png)

U kunt andere regels desgewenst wijzigen.

De tweede bestand (${CAFFE_ON_SPARK}/data/lenet_memory_train_test.prototxt) definieert hoe het netwerk neuron lijkt, en de relevante invoer en uitvoerbestand. ook moet u bijwerken van het bestand om de locatie van de training-gegevens weer te geven. Wijzig het volgende deel in lenet_memory_train_test.prototxt (u moet verwijzen naar de juiste locatie die specifiek zijn voor uw cluster):

- Wijzig de 'file:/Users/mridul/bigml/demodl/mnist_train_lmdb' in ' wasb: / / /-projecten/machine_learning/image_dataset/mnist_train_lmdb "
- wijzigen in "file:/Users/mridul/bigml/demodl/mnist_test_lmdb/" "wasb: / / /-projecten/machine_learning/image_dataset/mnist_test_lmdb"

![Caffe Config](./media/apache-spark-deep-learning-caffe/Caffe-2.png)

Raadpleeg voor meer informatie over het definiëren van het netwerk, de [Caffe documentatie over MNIST-gegevensset](https://caffe.berkeleyvision.org/gathered/examples/mnist.html)

In dit artikel, moet u dit voorbeeld MNIST gebruiken. De volgende opdrachten uitvoeren vanaf het hoofdknooppunt:

    spark-submit --master yarn --deploy-mode cluster --num-executors 8 --files ${CAFFE_ON_SPARK}/data/lenet_memory_solver.prototxt,${CAFFE_ON_SPARK}/data/lenet_memory_train_test.prototxt --conf spark.driver.extraLibraryPath="${LD_LIBRARY_PATH}" --conf spark.executorEnv.LD_LIBRARY_PATH="${LD_LIBRARY_PATH}" --class com.yahoo.ml.caffe.CaffeOnSpark ${CAFFE_ON_SPARK}/caffe-grid/target/caffe-grid-0.1-SNAPSHOT-jar-with-dependencies.jar -train -features accuracy,loss -label label -conf lenet_memory_solver.prototxt -devices 1 -connection ethernet -model wasb:///mnist.model -output wasb:///mnist_features_result

De voorgaande opdracht wordt de vereiste bestanden (lenet_memory_solver.prototxt en lenet_memory_train_test.prototxt) gedistribueerd naar elke container YARN. De opdracht stelt ook het relevante pad van de Spark-stuurprogramma/executor op LD_LIBRARY_PATH. LD_LIBRARY_PATH is gedefinieerd in de vorige codefragment en verwijst naar de locatie waarvoor CaffeOnSpark bibliotheken. 

## <a name="monitoring-and-troubleshooting"></a>Bewaking en probleemoplossing

Omdat u de clustermodus met YARN, in welk geval het Spark-stuurprogramma wordt gepland voor een willekeurige container (en een willekeurige worker-knooppunt) ziet alleen u in de console er ongeveer als uitvoeren:

    17/02/01 23:22:16 INFO Client: Application report for application_1485916338528_0015 (state: RUNNING)

Als u weten wat er gebeurd is wilt, moet u meestal om op te halen van de Spark van het stuurprogramma logboek meer informatie bevat. In dit geval moet u naar de gebruikersinterface van YARN te vinden van de relevante YARN-Logboeken. Hier krijgt u de gebruikersinterface van YARN door deze URL: 

    https://yourclustername.azurehdinsight.net/yarnui
   
![GEBRUIKERSINTERFACE VAN YARN](./media/apache-spark-deep-learning-caffe/YARN-UI-1.png)

U kunt een overzicht van hoeveel resources worden toegewezen aan deze bepaalde toepassing uitvoeren. U kunt klikken op de koppeling 'Planner' en vervolgens ziet u dat voor deze toepassing, zijn er negen containers die worden uitgevoerd. vraagt u YARN voor acht Executor, en een andere container voor stuurprogramma-proces. 

![YARN-Scheduler](./media/apache-spark-deep-learning-caffe/YARN-Scheduler.png)

U kunt de stuurprogramma-Logboeken of de logboeken voor containers controleren als er fouten zijn. Voor de stuurprogramma-Logboeken, kunt u klikt u op de toepassings-ID in de gebruikersinterface van YARN en vervolgens klikt u op de knop 'Logboeken'. De stuurprogramma-logboeken worden geschreven in stderr zijn.

![GEBRUIKERSINTERFACE VAN YARN 2](./media/apache-spark-deep-learning-caffe/YARN-UI-2.png)

Bijvoorbeeld: u ziet enkele van de fout hieronder in de stuurprogramma-Logboeken, waarmee wordt aangegeven te veel Executor die u toewijst.

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

Soms kan het probleem kan zich voordoen in Executor in plaats van stuurprogramma's. In dit geval moet u de logboeken voor containers controleren. U kunt altijd kunnen genieten van de logboeken voor containers en haal vervolgens de container is mislukt. Bijvoorbeeld, u kunt voldoen aan deze fout bij het uitvoeren van Caffe.

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

In dit geval moet u de mislukte container-ID niet ophalen (dit is in het bovenstaande geval container_1485916338528_0008_05_000005). Vervolgens moet u uitvoeren 

    yarn logs -containerId container_1485916338528_0008_03_000005

vanaf het hoofdknooppunt. Na een storing van de container te controleren, wordt dit veroorzaakt door het gebruik van de GPU-modus (waar moet u CPU-modus in plaats daarvan) in lenet_memory_solver.prototxt.

    17/02/01 07:10:48 INFO LMDB: Batch size:100
    WARNING: Logging before InitGoogleLogging() is written to STDERR
    F0201 07:10:48.309725 11624 common.cpp:79] Cannot use GPU in CPU-only Caffe: check mode.


## <a name="getting-results"></a>Resultaten ophalen

Aangezien u 8 Executor toewijzen wilt en de netwerktopologie eenvoudig is, moet het alleen ongeveer 30 minuten duren om uit te voeren van het resultaat. In de opdrachtregel, kunt u zien dat u het model in wasb:///mnist.model plaatsen en de resultaten naar een map met de naam wasb plaatst: / / / mnist_features_result.

U kunt de resultaten krijgen door te voeren

    hadoop fs -cat hdfs:///mnist_features_result/*

en het resultaat ziet eruit als:

    {"SampleID":"00009597","accuracy":[1.0],"loss":[0.028171852],"label":[2.0]}
    {"SampleID":"00009598","accuracy":[1.0],"loss":[0.028171852],"label":[6.0]}
    {"SampleID":"00009599","accuracy":[1.0],"loss":[0.028171852],"label":[1.0]}
    {"SampleID":"00009600","accuracy":[0.97],"loss":[0.0677709],"label":[5.0]}
    {"SampleID":"00009601","accuracy":[0.97],"loss":[0.0677709],"label":[0.0]}
    {"SampleID":"00009602","accuracy":[0.97],"loss":[0.0677709],"label":[1.0]}
    {"SampleID":"00009603","accuracy":[0.97],"loss":[0.0677709],"label":[2.0]}
    {"SampleID":"00009604","accuracy":[0.97],"loss":[0.0677709],"label":[3.0]}
    {"SampleID":"00009605","accuracy":[0.97],"loss":[0.0677709],"label":[4.0]}

De SampleID vertegenwoordigt de ID in de MNIST-gegevensset en het label wordt het getal dat het model wordt geïdentificeerd.


## <a name="conclusion"></a>Conclusie

In deze documentatie, hebt je geprobeerd CaffeOnSpark installeren met het uitvoeren van een eenvoudig voorbeeld. HDInsight is een volledig beheerde cloud-platform voor gedistribueerde Computing en de beste plaats voor het machine learning en workloads voor geavanceerde analyses uitvoeren op grote gegevensset, en voor gedistribueerde deep learning, kunt u Caffe gebruiken op HDInsight Spark om uit te voeren van deep learning taken.


## <a name="seealso"></a>Zie ook
* [Overzicht: Apache Spark in Azure HDInsight](apache-spark-overview.md)

### <a name="scenarios"></a>Scenario's
* [Apache Spark met Machine Learning: Spark in HDInsight gebruiken voor het analyseren van de gebouwtemperatuur met behulp van HVAC-gegevens](apache-spark-ipython-notebook-machine-learning.md)
* [Apache Spark met Machine Learning: Spark in HDInsight gebruiken voor de resultaten van voedingsinspectie voorspellen](apache-spark-machine-learning-mllib-ipython.md)

### <a name="manage-resources"></a>Resources beheren
* [Resources beheren voor het Apache Spark-cluster in Azure HDInsight](apache-spark-resource-manager.md)

