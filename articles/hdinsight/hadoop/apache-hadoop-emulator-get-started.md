---
title: Informatie over met behulp van een Apache Hadoop-sandbox - emulator - Azure HDInsight
description: 'Als u wilt gaan met het leren over het gebruik van het Apache Hadoop-ecosysteem, kunt u instellen een Hadoop-sandbox van Hortonworks op een Azure-machine. '
keywords: hadoop-emulator, hadoop-sandbox
ms.reviewer: jasonh
services: hdinsight
author: hrasheed-msft
ms.service: hdinsight
ms.custom: hdinsightactive,hdiseo17may2017
ms.topic: conceptual
ms.date: 12/11/2017
ms.author: hrasheed
ms.openlocfilehash: 1da676787eeee1eb75095a5e3a6b3f40056567ad
ms.sourcegitcommit: 2d0fb4f3fc8086d61e2d8e506d5c2b930ba525a7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/18/2019
ms.locfileid: "58005767"
---
# <a name="get-started-with-an-apache-hadoop-sandbox-an-emulator-on-a-virtual-machine"></a>Aan de slag met een Apache Hadoop-sandbox, een emulator op een virtuele machine

Informatie over het installeren van de Apache Hadoop-sandbox van Hortonworks op een virtuele machine voor meer informatie over het Hadoop-ecosysteem. De sandbox biedt een lokale ontwikkelingsomgeving voor meer informatie over Hadoop, Hadoop Distributed File System (HDFS) en verzenden van taken. Als u bekend met Hadoop bent, kunt u starten met Hadoop op Azure door het maken van een HDInsight-cluster. Zie voor meer informatie over hoe u aan de slag [aan de slag met Hadoop op HDInsight](apache-hadoop-linux-tutorial-get-started.md).

## <a name="prerequisites"></a>Vereisten
* [Oracle VirtualBox](https://www.virtualbox.org/). Download en installeer deze vanaf [hier](https://www.virtualbox.org/wiki/Downloads).



## <a name="download-and-install-the-virtual-machine"></a>Download en installeer de virtuele machine
1. Blader naar de [Hortonworks downloads](https://hortonworks.com/downloads/#sandbox).

2. Klik op **downloaden voor VIRTUALBOX** voor het downloaden van de nieuwste Hortonworks Sandbox op een virtuele machine. U wordt gevraagd om u te registreren met Hortonworks voordat het downloaden begint. Het duurt voordat een tot twee uur wilt downloaden, afhankelijk van de netwerksnelheid van uw.

    ![Afbeelding voor koppeling voor Hortonworks Sandbox voor VirtualBox downloaden](./media/apache-hadoop-emulator-get-started/download-sandbox.png)
3. Van dezelfde pagina, klikt u op de **Import op virtuele vak** koppeling om te downloaden van een PDF-bestand met de installatie-instructies voor de virtuele machine.

Vouw het archief voor het downloaden van een oudere versie sandbox voor HDP:

![Hortonworks Sandbox-archief](./media/apache-hadoop-emulator-get-started/hortonworks-sandbox-archive.png)


## <a name="start-the-virtual-machine"></a>Start de virtuele machine

1. Open Oracle VM VirtualBox.
2. Uit de **bestand** menu, klikt u op **importeren toestel**, en geef vervolgens de installatiekopie van het Hortonworks Sandbox.
1. Selecteer de Hortonworks Sandbox, klikt u op **Start**, en vervolgens **normaal Start**. Zodra het opstarten van de virtuele machine is voltooid, wordt de pagina aanmelden instructies.

    ![Normale start](./media/apache-hadoop-emulator-get-started/normal-start.png)
2. Open een webbrowser en navigeer naar de URL die wordt weergegeven (meestal `http://127.0.0.1:8888`).

## <a name="set-sandbox-passwords"></a>Sandbox-wachtwoorden instellen

1. Uit de **aan de slag** stap van de Hortonworks Sandbox-pagina, selecteer **geavanceerde opties weergeven**. Gebruik de informatie op deze pagina aanmelden bij de sandbox met behulp van SSH. Gebruik de naam en het wachtwoord dat is opgegeven.

   > [!NOTE]
   > Als u een SSH-client is geïnstalleerd hebt, kunt u het web gebaseerde SSH geleverd op door de virtuele machine bij **http://localhost:4200/**.

    De eerste keer dat u verbinding maken met behulp van SSH, moet u gevraagd het wachtwoord voor het root-account te wijzigen. Voer een nieuw wachtwoord, die u gebruiken wanneer u zich aanmeldt met behulp van SSH.

2. Wanneer u bent aangemeld, voer de volgende opdracht:

        ambari-admin-password-reset

    Wanneer u hierom wordt gevraagd, moet u een wachtwoord opgeven voor de Ambari-beheerdersaccount. Dit wordt gebruikt wanneer u toegang de Ambari-Webgebruikersinterface tot.

## <a name="use-hive-commands"></a>Hive-opdrachten gebruiken

1. Gebruik de volgende opdracht om te beginnen de Hive-shell uit een SSH-verbinding naar de sandbox:

        hive
2. Nadat de shell is gestart, gebruikt u de volgende om de tabellen die zijn voorzien van de sandbox weer te geven:

        show tables;
3. Gebruik de volgende informatie om op te halen van 10 rijen uit de `sample_07` tabel:

        select * from sample_07 limit 10;

## <a name="next-steps"></a>Volgende stappen
* [Informatie over het gebruik van Visual Studio met de Hortonworks Sandbox](../hdinsight-hadoop-emulator-visual-studio.md)
* [Leren werken met de kabels van de Hortonworks Sandbox](https://hortonworks.com/hadoop-tutorial/learning-the-ropes-of-the-hortonworks-sandbox/)
* [Hadoop-zelfstudie - aan de slag met HDP](https://hortonworks.com/hadoop-tutorial/hello-world-an-introduction-to-hadoop-hcatalog-hive-and-pig/)

