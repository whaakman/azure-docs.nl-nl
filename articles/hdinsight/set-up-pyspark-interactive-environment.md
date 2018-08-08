---
title: Hulpprogramma's voor Azure HDInsight - instellen interactieve PySpark-omgeving voor Visual Studio Code
description: Informatie over het gebruik van de Azure HDInsight Tools voor Visual Studio Code maken en verzenden van query's en scripts.
Keywords: VScode,Azure HDInsight Tools,Hive,Python,PySpark,Spark,HDInsight,Hadoop,LLAP,Interactive Hive,Interactive Query
services: hdinsight
ms.service: hdinsight
author: jejiang
ms.author: jejiang
editor: jasonwhowell
ms.topic: conceptual
ms.date: 10/27/2017
ms.openlocfilehash: aeac28d77270cae120bf3b9f05d9825b01f16bb4
ms.sourcegitcommit: 1f0587f29dc1e5aef1502f4f15d5a2079d7683e9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/07/2018
ms.locfileid: "39600503"
---
# <a name="set-up-the-pyspark-interactive-environment-for-visual-studio-code"></a>De interactieve PySpark-omgeving instellen voor Visual Studio Code

De volgende stappen ziet u hoe u Python-pakketten installeren door te voeren **HDInsight: interactieve PySpark**.


## <a name="set-up-the-pyspark-interactive-environment-on-macos-and-linux"></a>Stel de interactieve PySpark-omgeving in macOS en Linux
Als u **python 3.x**, moet u het gebruik van de opdracht **pip3** voor de volgende stappen uit:

1. Zorg ervoor dat **Python** en **pip** zijn geïnstalleerd.
 
    ![Python-pip-versie](./media/set-up-pyspark-interactive-environment/check-python-pip-version.png)

2.  Jupyter installeren.
    ```
    sudo pip install jupyter
    ```
   U ziet het volgende foutbericht weergegeven in Linux en macOS:

   ![Fout 1](./media/set-up-pyspark-interactive-environment/error1.png)

   ```Resolve:
    sudo pip uninstall asyncio
    sudo pip install trollies
    ```

3. Installeer **libkrb5 dev** (voor Linux). U ziet mogelijk de volgende strekking weergegeven:

   ![Fout 2](./media/set-up-pyspark-interactive-environment/error2.png)
       
   ```Resolve:
   sudo apt-get install libkrb5-dev 
   ```

3. Installeer **sparkmagic**.
   ```
   sudo pip install sparkmagic
   ```

4. Zorg ervoor dat **ipywidgets** juist is geïnstalleerd door het uitvoeren van de volgende:
   ```
   sudo jupyter nbextension enable --py --sys-prefix widgetsnbextension
   ```
   ![De kernels wrapper installeren](./media/set-up-pyspark-interactive-environment/ipywidget-enable.png)
 

5. Installeer de wrapper kernels. Voer **pip weergeven sparkmagic**. De uitvoer ziet u het pad voor de **sparkmagic** installatie. 

    ![sparkmagic locatie](./media/set-up-pyspark-interactive-environment/sparkmagic-location.png)
   
6. Ga naar de locatie en voer vervolgens:

   ```Python2
   sudo jupyter-kernelspec install sparkmagic/kernels/pysparkkernel   
   ```
   ```Python3
   sudo jupyter-kernelspec install sparkmagic/kernels/pyspark3kernel
   ```

   ![jupyter kernelspec installeren](./media/set-up-pyspark-interactive-environment/jupyter-kernelspec-install.png)
7. Controleer de status van de installatie.

    ```
    jupyter-kernelspec list
    ```
    ![lijst met jupyter kernelspec](./media/set-up-pyspark-interactive-environment/jupyter-kernelspec-list.png)

    Voor beschikbare kernels: 
    - **python2** en **pysparkkernel** komen overeen met **python 2.x**. 
    - **python3** en **pyspark3kernel** komen overeen met **python 3.x**. 

8. Start VS Code opnieuw op en gaat u terug naar de script-editor die wordt uitgevoerd **HDInsight: interactieve PySpark**.

## <a name="next-steps"></a>Volgende stappen

### <a name="demo"></a>Demo
* HDInsight voor VS Code: [Video](https://go.microsoft.com/fwlink/?linkid=858706)

### <a name="tools-and-extensions"></a>Tools en uitbreidingen
* [Azure HDInsight-hulpprogramma voor Visual Studio Code gebruiken](hdinsight-for-vscode.md)
* [Azure Toolkit voor IntelliJ gebruiken om te maken en verzenden van Spark Scala-toepassingen](spark/apache-spark-intellij-tool-plugin.md)
* [Azure-Toolkit voor IntelliJ fouten opsporen in Spark-toepassingen op afstand via SSH](spark/apache-spark-intellij-tool-debug-remotely-through-ssh.md)
* [Azure-Toolkit voor IntelliJ fouten opsporen in Spark-toepassingen op afstand via VPN-verbinding](spark/apache-spark-intellij-tool-plugin-debug-jobs-remotely.md)
* [HDInsight-hulpprogramma's in Azure Toolkit voor Eclipse gebruiken voor het Spark-toepassingen maken](spark/apache-spark-eclipse-tool-plugin.md)
* [HDInsight-hulpprogramma's voor IntelliJ met Hortonworks Sandbox gebruiken](hadoop/hdinsight-tools-for-intellij-with-hortonworks-sandbox.md)
* [Zeppelin-notebooks gebruiken met een Spark-cluster in HDInsight](spark/apache-spark-zeppelin-notebook.md)
* [Beschikbare kernels voor Jupyter-notebook in Spark-cluster voor HDInsight](spark/apache-spark-jupyter-notebook-kernels.md)
* [Externe pakketten gebruiken met Jupyter-notebooks](spark/apache-spark-jupyter-notebook-use-external-packages.md)
* [Jupyter op uw computer installeren en verbinding maken met een HDInsight Spark-cluster](spark/apache-spark-jupyter-notebook-install-locally.md)
* [Hive-gegevens visualiseren met Microsoft Power BI in Azure HDInsight](hadoop/apache-hadoop-connect-hive-power-bi.md)
* [Zeppelin gebruiken voor het uitvoeren van Hive-query's in Azure HDInsight ](hdinsight-connect-hive-zeppelin.md)
