---
title: Meer informatie over met behulp van een Hadoop-sandbox - emulator - Azure HDInsight | Microsoft Docs
description: 'Als u wilt weten over het gebruik van het Hadoop-ecosysteem, kunt u instellen een sandbox met Hadoop van Hortonworks op een virtuele machine van Azure. '
keywords: hadoop-emulator, hadoop sandbox
editor: cgronlun
manager: jhubbard
services: hdinsight
author: nitinme
documentationcenter: 
tags: azure-portal
ms.assetid: 6ad5bb58-8215-4e3d-a07f-07fcd8839cc6
ms.service: hdinsight
ms.custom: hdinsightactive,hdiseo17may2017
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/11/2017
ms.author: nitinme
ms.openlocfilehash: d7df18a80470beb8dc25cf6add6b7a61f45dcfe7
ms.sourcegitcommit: a5f16c1e2e0573204581c072cf7d237745ff98dc
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/11/2017
---
# <a name="get-started-with-a-hadoop-sandbox-an-emulator-on-a-virtual-machine"></a>Aan de slag met een sandbox met Hadoop, een emulator op een virtuele machine

Informatie over het installeren van de sandbox Hadoop vanaf Hortonworks op een virtuele machine voor meer informatie over het Hadoop-ecosysteem. De sandbox biedt een lokale ontwikkelingsomgeving voor meer informatie over Hadoop, Hadoop Distributed File System (HDFS) en de verzending van de taak. Wanneer u bekend met Hadoop bent, kunt u beginnen met Hadoop op Azure door het maken van een HDInsight-cluster. Zie voor meer informatie over het aan de slag [aan de slag met Hadoop op HDInsight](apache-hadoop-linux-tutorial-get-started.md).

## <a name="prerequisites"></a>Vereisten
* [Oracle VirtualBox](https://www.virtualbox.org/). Download en installeer deze via [hier](https://www.virtualbox.org/wiki/Downloads).



## <a name="download-and-install-the-virtual-machine"></a>Download en installeer de virtuele machine
1. Blader naar de [Hortonworks downloadt](http://hortonworks.com/downloads/#sandbox).

2. Klik op **downloaden voor VIRTUALBOX** voor het downloaden van de meest recente Hortonworks Sandbox op een virtuele machine. U wordt gevraagd om te registreren met Hortonworks voordat de download wordt gestart. Het duurt een tot twee uur om te downloaden, afhankelijk van de netwerksnelheid van uw.
   
    ![Afbeelding van de koppeling voor de Hortonworks Sandbox voor VirtualBox downloaden](./media/apache-hadoop-emulator-get-started/download-sandbox.png)
3. Van dezelfde pagina, klikt u op de **importeren op de virtuele vak** koppeling voor het downloaden van een PDF-bestand met de installatie-instructies voor de virtuele machine.

Vouw het archief voor het downloaden van een oudere versie sandbox voor HDP:

![Hortonworks sandbox-archief](./media/apache-hadoop-emulator-get-started/hortonworks-sandbox-archive.png)


## <a name="start-the-virtual-machine"></a>De virtuele machine starten

1. Oracle VM VirtualBox openen.
2. Van de **bestand** menu, klikt u op **importeren toestel**, en geef vervolgens de installatiekopie van het Hortonworks Sandbox.
1. Selecteer de Hortonworks Sandbox, klik op **Start**, en vervolgens **normaal Start**. Zodra het opstarten van de virtuele machine is voltooid, wordt de aanmelding instructies weergegeven.
   
    ![Normale starten](./media/apache-hadoop-emulator-get-started/normal-start.png)
2. Open een webbrowser en navigeer naar de URL weergegeven (meestal http://127.0.0.1:8888).

## <a name="set-sandbox-passwords"></a>Sandbox-wachtwoorden instellen

1. Van de **aan de slag** stap van de Hortonworks Sandbox pagina **weergave geavanceerde opties**. Gebruik de informatie op deze pagina aanmelden bij de sandbox via SSH. Gebruik de naam en wachtwoord opgegeven.
   
   > [!NOTE]
   > Als u een SSH-client is geïnstalleerd niet hebt, kunt u het web gebaseerde SSH op aangeboden door de virtuele machine op **http://localhost:4200 /**.
   > 
   
    De eerste keer dat u verbinding maken via SSH, wordt u gevraagd het wachtwoord voor het root-account te wijzigen. Voer een nieuw wachtwoord dat u gebruiken wanneer u zich aanmeldt via SSH.

2. Nadat u bent aangemeld, voer de volgende opdracht:
   
        ambari-admin-password-reset
   
    Wanneer u wordt gevraagd, moet u een wachtwoord opgeven voor de Ambari-beheeraccount. Dit wordt gebruikt wanneer u toegang de Ambari-Webgebruikersinterface tot.

## <a name="use-hive-commands"></a>Hive-opdrachten gebruiken

1. Gebruik de volgende opdracht de Hive-shell starten van een SSH-verbinding aan de sandbox:
   
        hive
2. Nadat de shell is gestart, gebruikt u de volgende om de tabellen die worden geleverd aan de sandbox weer te geven:
   
        show tables;
3. Gebruik de volgende voor het ophalen van 10 rijen uit de `sample_07` tabel:
   
        select * from sample_07 limit 10;

## <a name="next-steps"></a>Volgende stappen
* [Informatie over het gebruik van Visual Studio met de Hortonworks Sandbox](../hdinsight-hadoop-emulator-visual-studio.md)
* [De kabels van de Hortonworks Sandbox leren](http://hortonworks.com/hadoop-tutorial/learning-the-ropes-of-the-hortonworks-sandbox/)
* [Hadoop-zelfstudie - aan de slag met HDP](http://hortonworks.com/hadoop-tutorial/hello-world-an-introduction-to-hadoop-hcatalog-hive-and-pig/)

