---
title: Azure HDInsight-hulpprogramma's - PySpark interactieve omgeving instellen voor Visual Studio Code | Microsoft Docs
description: Informatie over het gebruik van Azure HDInsight Tools voor Visual Studio Code voor het maken, indienen van query's en scripts.
Keywords: VScode, Azure HDInsight-hulpprogramma's, Hive, Python, PySpark, Spark, HDInsight, Hadoop, LLAP, interactieve Hive, interactieve Query
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
ms.openlocfilehash: 839f379228322eb2da0ff61609634bf1f86e4bb3
ms.sourcegitcommit: e462e5cca2424ce36423f9eff3a0cf250ac146ad
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/01/2017
---
# <a name="set-up-pyspark-interactive-environment-for-visual-studio-code"></a>PySpark interactieve omgeving instellen voor Visual Studio Code

De volgende stappen ziet u het installeren van python-pakketten wanneer uitgevoerd **HDInsight: PySpark interactieve**.


## <a name="set-up-pyspark-interactive-environment-on-macos-and-linux"></a>Instellen van de interactieve omgeving PySpark op Mac OS- en Linux
U moet de opdracht gebruiken **pip3** voor de volgende stappen uit, als het **python 3.x**.
1. Zorg ervoor dat de **Python** en **pip** geïnstalleerd.
 
    ![Python pip-versie](./media/set-up-pyspark-interactive-environment/check-python-pip-version.png)

2.  Jupyter installeren
    ```
    sudo pip install jupyter
    ```
    +  Mogelijk het volgende foutbericht uitkomen op Linux- en Mac OS:

        ![error1](./media/set-up-pyspark-interactive-environment/error1.png)
        ```Resolve:
        sudo pip uninstall asyncio
        sudo pip install trollies
        ```

    + Installeer libkrb5 dev(For Linux only), mogelijk het volgende foutbericht weergegeven:

        ![error2](./media/set-up-pyspark-interactive-environment/error2.png)
        ```Resolve:
        sudo apt-get install libkrb5-dev 
        ```

3. Sparkmagic installeren
   ```
   sudo pip install sparkmagic
   ```

4. Zorg ervoor dat ipywidgets juist is geïnstalleerd door te voeren:
   ```
   sudo jupyter nbextension enable --py --sys-prefix widgetsnbextension
   ```
   ![De wrapper-kernels installeren](./media/set-up-pyspark-interactive-environment/ipywidget-enable.png)
 

5. Installeer de wrapper-kernels. Voer **pip weergeven sparkmagic** en deze geeft het pad dat sparkmagic is geïnstalleerd. 

    ![sparkmagic locatie](./media/set-up-pyspark-interactive-environment/sparkmagic-location.png)
   
6. Navigeer naar de locatie en uitvoeren:

   ```Python2
   sudo jupyter-kernelspec install sparkmagic/kernels/pysparkkernel   
   ```
   ```Python3
   sudo jupyter-kernelspec install sparkmagic/kernels/pyspark3kernel
   ```

   ![jupyter kernelspec installeren](./media/set-up-pyspark-interactive-environment/jupyter-kernelspec-install.png)
7. Controleer de status van de installatie: 

    ```
    jupyter-kernelspec list
    ```
    ![jupyter kernelspec lijst](./media/set-up-pyspark-interactive-environment/jupyter-kernelspec-list.png)

    Voor beschikbare kernels: **python2** en **pysparkkernel** komen overeen met **python 2.x**, **python3** en  **pyspark3kernel** komen overeen met **python 3.x**. 

8. Start VScode en weer in scripteditor met **HDInsight: PySpark interactieve**.

## <a name="next-steps"></a>Volgende stappen

### <a name="demo"></a>Demo
* HDInsight voor VScode: [Video](https://go.microsoft.com/fwlink/?linkid=858706)

### <a name="tools-and-extensions"></a>Tools en uitbreidingen
* [Azure HDInsight-hulpprogramma gebruiken voor Visual Studio Code](hdinsight-for-vscode.md)
* [Azure-Toolkit voor IntelliJ gebruiken voor het maken en verzenden van Spark Scala-toepassingen](hdinsight-apache-spark-intellij-tool-plugin.md)
* [Azure-Toolkit voor IntelliJ gebruiken om op te sporen Spark-toepassingen op afstand via SSH](hdinsight-apache-spark-intellij-tool-debug-remotely-through-ssh.md)
* [Azure-Toolkit voor IntelliJ gebruiken om op te sporen Spark scala-toepassingen op afstand via VPN-verbinding](hdinsight-apache-spark-intellij-tool-plugin-debug-jobs-remotely.md)
* [Gebruik van HDInsight-hulpprogramma's in Azure Toolkit voor Eclipse Spark-toepassingen maken](hdinsight-apache-spark-eclipse-tool-plugin.md)
* [Gebruik van HDInsight Tools voor IntelliJ met Hortonworks Sandbox](hdinsight-tools-for-intellij-with-hortonworks-sandbox.md)
* [Zeppelin-notebooks gebruiken met een Spark-cluster in HDInsight](hdinsight-apache-spark-zeppelin-notebook.md)
* [Beschikbare kernels voor Jupyter-notebook in Spark-cluster voor HDInsight](hdinsight-apache-spark-jupyter-notebook-kernels.md)
* [Externe pakketten gebruiken met Jupyter-notebooks](hdinsight-apache-spark-jupyter-notebook-use-external-packages.md)
* [Jupyter op uw computer installeren en verbinding maken met een HDInsight Spark-cluster](hdinsight-apache-spark-jupyter-notebook-install-locally.md)
* [Hive-gegevens visualiseren met Microsoft Power BI in Azure HDInsight](./hdinsight-connect-hive-power-bi.md).
* [Zeppelin gebruiken voor het uitvoeren van Hive-query's in Azure HDInsight ](./hdinsight-connect-hive-zeppelin.md).
