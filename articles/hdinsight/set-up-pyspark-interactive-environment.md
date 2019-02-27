---
title: Hulpprogramma's voor Azure HDInsight - instellen interactieve PySpark-omgeving voor Visual Studio Code
description: Informatie over het gebruik van de Azure HDInsight Tools voor Visual Studio Code maken en verzenden van query's en scripts.
keywords: VScode, Azure HDInsight-hulpprogramma's, Hive, Python, PySpark, Spark, HDInsight, Hadoop, LLAP, Interactive Hive, Interactive Query
services: hdinsight
ms.service: hdinsight
author: jejiang
ms.author: jejiang
ms.reviewer: jasonh
ms.topic: conceptual
ms.date: 1/17/2019
ms.openlocfilehash: c5a8869b0619b7be506ead1b9d37053e9fa57bba
ms.sourcegitcommit: 50ea09d19e4ae95049e27209bd74c1393ed8327e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/26/2019
ms.locfileid: "56880288"
---
# <a name="set-up-the-pyspark-interactive-environment-for-visual-studio-code"></a>De interactieve PySpark-omgeving instellen voor Visual Studio Code

De volgende stappen laten zien hoe u de interactieve PySpark-omgeving in VS Code instelt.

We gebruiken **python/pip** opdracht voor het bouwen van virtuele omgeving in het pad voor de startpagina. Als u een andere versie gebruiken wilt, moet u wijzigen standaardversie van **python/pip** handmatig opdracht. Meer informatie Zie [update alternatieven](https://linux.die.net/man/8/update-alternatives).

1. Installeer [Python](https://www.python.org/downloads/) en [pip](https://pip.pypa.io/en/stable/installing/).
   
   + Installeer Python van [ https://www.python.org/downloads/ ](https://www.python.org/downloads/).
   + Instalovat modul pip uit [ https://pip.pypa.io/en/stable/installing ](https://pip.pypa.io/en/stable/installing/). (Als geen van de Python-installatie geïnstalleerd)
   + Valideren van Python en pip zijn geïnstalleerd met behulp van de volgende opdrachten. (Optioneel)
 
        ![Python-pip-versie](./media/set-up-pyspark-interactive-environment/check-python-pip-version.png)

    > [!NOTE]
    > Het verdient aanbeveling om handmatig te installeren Python in plaats van de standaard-Mac OS-versie.


2. Installeer **virtualenv** door het uitvoeren van de onderstaande opdracht.
   
   ```
   pip install virtualenv
   ```

3. Voor Linux, de vereiste pakketten te installeren door het uitvoeren van de onderstaande opdrachten als u het foutbericht.
   
    ![Python-pip-versie](./media/set-up-pyspark-interactive-environment/install-libkrb5-package.png)
       
   ```
   sudo apt-get install libkrb5-dev 
   ```

   ```
   sudo apt-get install python-dev
   ```

4. Start VS Code opnieuw op en gaat u terug naar de script-editor die wordt uitgevoerd **HDInsight: Interactieve PySpark**.

## <a name="next-steps"></a>Volgende stappen

### <a name="demo"></a>Demo
* HDInsight voor VS Code: [Video](https://go.microsoft.com/fwlink/?linkid=858706)

### <a name="tools-and-extensions"></a>Tools en uitbreidingen
* [Azure HDInsight-hulpprogramma voor Visual Studio Code gebruiken](hdinsight-for-vscode.md)
* [Azure Toolkit voor IntelliJ gebruiken om te maken en verzenden van Apache Spark Scala-toepassingen](spark/apache-spark-intellij-tool-plugin.md)
* [Azure Toolkit voor IntelliJ gebruiken voor foutopsporing van Apache Spark-toepassingen op afstand via SSH](spark/apache-spark-intellij-tool-debug-remotely-through-ssh.md)
* [Azure Toolkit voor IntelliJ voor foutopsporing van Apache Spark-toepassingen op afstand via VPN-verbinding gebruiken](spark/apache-spark-intellij-tool-plugin-debug-jobs-remotely.md)
* [HDInsight-hulpprogramma's in Azure Toolkit voor Eclipse gebruiken voor het maken van Apache Spark-toepassingen](spark/apache-spark-eclipse-tool-plugin.md)
* [HDInsight-hulpprogramma's voor IntelliJ met Hortonworks Sandbox gebruiken](hadoop/hdinsight-tools-for-intellij-with-hortonworks-sandbox.md)
* [Apache Zeppelin-notebooks gebruiken met een Apache Spark-cluster in HDInsight](spark/apache-spark-zeppelin-notebook.md)
* [Beschikbare kernels voor Jupyter-notebook in een Apache Spark-cluster voor HDInsight](spark/apache-spark-jupyter-notebook-kernels.md)
* [Externe pakketten gebruiken met Jupyter-notebooks](spark/apache-spark-jupyter-notebook-use-external-packages.md)
* [Jupyter op uw computer installeren en verbinding maken met een HDInsight Spark-cluster](spark/apache-spark-jupyter-notebook-install-locally.md)
* [Apache Hive-gegevens visualiseren met Microsoft Power BI in Azure HDInsight](hadoop/apache-hadoop-connect-hive-power-bi.md)
* [Apache Zeppelin Apache Hive-query's uitvoeren in Azure HDInsight gebruiken](hdinsight-connect-hive-zeppelin.md)
