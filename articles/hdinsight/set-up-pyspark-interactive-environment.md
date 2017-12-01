---
title: Azure HDInsight-hulpprogramma's - PySpark interactieve omgeving instellen voor Visual Studio Code | Microsoft Docs
description: Informatie over het gebruik van Azure HDInsight Tools voor Visual Studio Code maken en verzenden van query's en scripts.
Keywords: VScode,Azure HDInsight Tools,Hive,Python,PySpark,Spark,HDInsight,Hadoop,LLAP,Interactive Hive,Interactive Query
services: HDInsight
documentationcenter: 
author: jejiang
manager: 
editor: 
tags: azure-portal
ms.assetid: 
ms.service: HDInsight
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 10/27/2017
ms.author: jejiang
ms.openlocfilehash: 5a64023df813262c461b9d772b722ebd613369ed
ms.sourcegitcommit: 5a6e943718a8d2bc5babea3cd624c0557ab67bd5
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/01/2017
---
# <a name="set-up-the-pyspark-interactive-environment-for-visual-studio-code"></a>De interactieve PySpark-omgeving instellen voor Visual Studio Code

De volgende stappen ziet u hoe u Python-pakketten installeren door te voeren **HDInsight: PySpark interactieve**.


## <a name="set-up-the-pyspark-interactive-environment-on-macos-and-linux"></a>Instellen van de PySpark-interactieve omgeving op Mac OS- en Linux
Als u **python 3.x**, moet u de opdracht **pip3** voor de volgende stappen uit:

1. Zorg ervoor dat **Python** en **pip** zijn geïnstalleerd.
 
    ![Python pip-versie](./media/set-up-pyspark-interactive-environment/check-python-pip-version.png)

2.  Jupyter installeren.
    ```
    sudo pip install jupyter
    ```
   U ziet het volgende foutbericht weergegeven op Linux- en Mac OS:

   ![Fout 1](./media/set-up-pyspark-interactive-environment/error1.png)

   ```Resolve:
    sudo pip uninstall asyncio
    sudo pip install trollies
    ```

3. Installeer **libkrb5 dev** (voor Linux). U ziet het volgende foutbericht weergegeven:

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
   ![De wrapper-kernels installeren](./media/set-up-pyspark-interactive-environment/ipywidget-enable.png)
 

5. Installeer de wrapper-kernels. Voer **pip weergeven sparkmagic**. De uitvoer ziet u het pad voor de **sparkmagic** installatie. 

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
    ![jupyter kernelspec lijst](./media/set-up-pyspark-interactive-environment/jupyter-kernelspec-list.png)

    Voor beschikbare kernels: 
    - **python2** en **pysparkkernel** komen overeen met **python 2.x**. 
    - **python3** en **pyspark3kernel** komen overeen met **python 3.x**. 

8. Start tegenover Code en gaat u terug naar de script-editor met **HDInsight: PySpark interactieve**.

## <a name="next-steps"></a>Volgende stappen

### <a name="demo"></a>Demo
* HDInsight voor VS-Code: [Video](https://go.microsoft.com/fwlink/?linkid=858706)

### <a name="tools-and-extensions"></a>Tools en uitbreidingen
* [Azure HDInsight-hulpprogramma gebruiken voor Visual Studio Code](hdinsight-for-vscode.md)
* [Azure-Toolkit voor IntelliJ gebruiken voor het maken en verzenden van Spark Scala-toepassingen](spark/apache-spark-intellij-tool-plugin.md)
* [Azure-Toolkit voor IntelliJ gebruiken om op te sporen Spark-toepassingen op afstand via SSH](spark/apache-spark-intellij-tool-debug-remotely-through-ssh.md)
* [Azure-Toolkit voor IntelliJ gebruiken om op te sporen Spark scala-toepassingen op afstand via VPN-verbinding](spark/apache-spark-intellij-tool-plugin-debug-jobs-remotely.md)
* [Gebruik van HDInsight-hulpprogramma's in Azure Toolkit voor Eclipse Spark-toepassingen maken](spark/apache-spark-eclipse-tool-plugin.md)
* [Gebruik van HDInsight Tools voor IntelliJ met Hortonworks Sandbox](hadoop/hdinsight-tools-for-intellij-with-hortonworks-sandbox.md)
* [Zeppelin-notebooks gebruiken met een Spark-cluster in HDInsight](spark/apache-spark-zeppelin-notebook.md)
* [Beschikbare kernels voor Jupyter-notebook in Spark-cluster voor HDInsight](spark/apache-spark-jupyter-notebook-kernels.md)
* [Externe pakketten gebruiken met Jupyter-notebooks](spark/apache-spark-jupyter-notebook-use-external-packages.md)
* [Jupyter op uw computer installeren en verbinding maken met een HDInsight Spark-cluster](spark/apache-spark-jupyter-notebook-install-locally.md)
* [Hive-gegevens visualiseren met Microsoft Power BI in Azure HDInsight](hadoop/apache-hadoop-connect-hive-power-bi.md)
* [Zeppelin gebruiken voor het uitvoeren van Hive-query's in Azure HDInsight](hdinsight-connect-hive-zeppelin.md)
