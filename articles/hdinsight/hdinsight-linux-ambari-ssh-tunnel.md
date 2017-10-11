---
title: SSH-tunneling voor toegang tot Azure HDInsight gebruiken | Microsoft Docs
description: Informatie over het gebruik van een SSH-tunnel te zoeken veilig webbronnen gehost op uw HDInsight op basis van Linux-knooppunten.
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
ms.assetid: 879834a4-52d0-499c-a3ae-8d28863abf65
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 08/21/2017
ms.author: larryfr
ms.openlocfilehash: 4b606ea3797d685b9deacf72f1bd31e0ef007f98
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/29/2017
---
# <a name="use-ssh-tunneling-to-access-ambari-web-ui-jobhistory-namenode-oozie-and-other-web-uis"></a>SSH-Tunneling gebruiken voor toegang tot de Ambari-webgebruikersinterface, JobHistory, NameNode, Oozie en andere web-UI

Linux gebaseerde HDInsight-clusters bieden toegang tot de Ambari-webgebruikersinterface via Internet, maar sommige functies van de gebruikersinterface zijn niet. Bijvoorbeeld: de webgebruikersinterface voor andere services die via Ambari worden opgehaald. Voor volledige functionaliteit van de Ambari-webgebruikersinterface, moet u een SSH-tunnel naar de kop van het cluster.

## <a name="why-use-an-ssh-tunnel"></a>Waarom gebruikt een SSH-tunnel

Aantal van de menu's in Ambari werken alleen via een SSH-tunnel. Deze menu's zijn afhankelijk van websites en services worden uitgevoerd op andere knooppunttypen zoals worker-knooppunten. Deze websites zijn vaak niet beveiligd, zodat het is niet veilig om ze rechtstreeks op het internet weer te geven.

De volgende Web-UI vereisen een SSH-tunnel:

* JobHistory
* NameNode
* Thread-Stacks
* Oozie-webgebruikersinterface
* HBase hoofd- en logboeken gebruikersinterface

Als u scriptacties gebruikt voor het aanpassen van uw cluster, vereisen geen services of hulpprogramma's die u installeert die een web-UI zichtbaar een SSH-tunnel. Als u Hue met behulp van een scriptactie installeren, moet u een SSH-tunnel gebruiken voor toegang tot de Hue-webgebruikersinterface.

> [!IMPORTANT]
> Als u directe toegang tot HDInsight via een virtueel netwerk hebt, hoeft u geen gebruik van SSH-tunnels. Zie voor een voorbeeld van rechtstreeks toegang hebben tot HDInsight via een virtueel netwerk, de [HDInsight verbinding maken met uw lokale netwerk](connect-on-premises-network.md) document.

## <a name="what-is-an-ssh-tunnel"></a>Wat is een SSH-tunnel

[Secure Shell (SSH) tunneling](https://en.wikipedia.org/wiki/Tunneling_protocol#Secure_Shell_tunneling) verzonden naar een poort op uw lokale werkstation verkeer gerouteerd. Het verkeer wordt gerouteerd via een SSH-verbinding met het hoofdknooppunt van uw HDInsight-cluster. De aanvraag is opgelost, alsof deze afkomstig van het hoofdknooppunt is. Het antwoord wordt vervolgens doorgestuurd teruggestuurd via de tunnel naar uw werkstation.

## <a name="prerequisites"></a>Vereisten

* Een SSH-client. Zie [SSH gebruiken met HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md) voor meer informatie.

* Een webbrowser die kan worden geconfigureerd om een proxy SOCKS5 te gebruiken.

    > [!WARNING]
    > De ondersteuning voor SOCKS-proxy is ingebouwd in Windows biedt geen ondersteuning voor SOCKS5 en werkt niet met de stappen in dit document. De volgende browsers zijn afhankelijk van de proxy-instellingen voor Windows en op dit moment niet werken met de stappen in dit document:
    >
    > * Microsoft Edge
    > * Microsoft Internet Explorer
    >
    > Google Chrome is ook afhankelijk van de proxy-instellingen van Windows. U kunt echter de uitbreidingen die ondersteuning bieden voor SOCKS5 installeren. Het is raadzaam [FoxyProxy standaard](https://chrome.google.com/webstore/detail/foxyproxy-standard/gcknhkkoolaabfmlnjonogaaifnjlfnp).

## <a name="usessh"></a>Maken van een tunnel met behulp van de SSH-opdracht

Gebruik de volgende opdracht voor het maken van een SSH-tunnel met behulp van de `ssh` opdracht. Vervang **gebruikersnaam** met een SSH-gebruiker voor uw HDInsight-cluster en vervang **CLUSTERNAME** met de naam van uw HDInsight-cluster:

```bash
ssh -C2qTnNf -D 9876 USERNAME@CLUSTERNAME-ssh.azurehdinsight.net
```

Deze opdracht maakt u een verbinding die verkeer gerouteerd naar lokale poort 9876 aan het cluster via SSH. De opties zijn:

* **D 9876** -de lokale poort waarmee verkeer via de tunnel.
* **C** -alle gegevens te comprimeren omdat webverkeer voornamelijk tekst is.
* **2** -force SSH protocol versie 2 alleen proberen.
* **q** -stille modus.
* **T** -toewijzing van de pseudo-tty uitschakelen omdat we zojuist een poort doorstuurt.
* **n**-Voorkomen dat het lezen van STDIN, aangezien we zojuist een poort doorstuurt.
* **N** -een externe opdracht niet uitvoeren omdat er slechts een poort doorstuurt.
* **f** -op de achtergrond uitgevoerd.

Zodra de opdracht is voltooid, wordt verkeer dat wordt verzonden naar poort 9876 op de lokale computer naar het hoofdknooppunt van het cluster gestuurd.

## <a name="useputty"></a>Maken van een tunnel met PuTTY

[PuTTY](http://www.chiark.greenend.org.uk/~sgtatham/putty) een grafische SSH-client voor Windows. Gebruik de volgende stappen voor het maken van een SSH-tunnel met PuTTY:

1. PuTTY opent en voert u de verbindingsgegevens. Als u niet bekend met PuTTY bent, raadpleegt u de [PuTTY-documentatie (http://www.chiark.greenend.org.uk/~sgtatham/putty/docs.html)](http://www.chiark.greenend.org.uk/~sgtatham/putty/docs.html).

2. In de **categorie** sectie aan de linkerkant van het dialoogvenster uit, vouw **verbinding**, vouw **SSH**, en selecteer vervolgens **Tunnels**.

3. Geef de volgende informatie op de **opties voor SSH-poort doorsturen** vorm:
   
   * **Bronpoort**: de poort op de client die u wilt doorsturen. Bijvoorbeeld: **9876**.

   * **Bestemming** -de SSH-adres voor het Linux gebaseerde HDInsight-cluster. Bijvoorbeeld **mijncluster-ssh.azurehdinsight.net**.

   * **Dynamisch**: hiermee schakelt u de dynamische routering van SOCKS-proxy's in.
     
     ![afbeelding van de opties-tunneling](./media/hdinsight-linux-ambari-ssh-tunnel/puttytunnel.png)

4. Klik op **toevoegen** toevoegen van de instellingen en klik vervolgens op **openen** een SSH-verbinding openen.

5. Wanneer u wordt gevraagd, moet u zich aanmelden bij de server.

## <a name="use-the-tunnel-from-your-browser"></a>Gebruik de tunnel vanuit de browser

> [!IMPORTANT]
> De stappen in deze sectie de Mozilla FireFox browser gebruiken, aangezien deze dezelfde proxyinstellingen voor alle platforms biedt. Andere moderne browsers, zoals Google Chrome moet mogelijk een extensie zoals FoxyProxy werken met de tunnel.

1. Configureer de browser te gebruiken **localhost** en het maken van de poort die u hebt gebruikt toen de tunnel als een **SOCKS v5** proxy. Hier ziet u hoe de instellingen van Firefox eruit. Als u een andere poort dan 9876 gebruikt, moet u de poort wijzigen in het abonnement dat u gebruikt:
   
    ![afbeelding van Firefox instellingen](./media/hdinsight-linux-ambari-ssh-tunnel/firefoxproxy.png)
   
   > [!NOTE]
   > Selecteren **externe DNS** wordt omgezet Domain Name System (DNS) aanvragen met behulp van het HDInsight-cluster. Deze instelling wordt omgezet DNS met het hoofdknooppunt van het cluster.

2. Controleer of de tunnel werkt via een site, zoals [http://www.whatismyip.com/](http://www.whatismyip.com/). Het IP-adres geretourneerd moet één die wordt gebruikt door de Microsoft Azure-datacenter.

## <a name="verify-with-ambari-web-ui"></a>Controleer met Ambari-webgebruikersinterface

Zodra het cluster is ingesteld, gebruikt u de volgende stappen uit om te controleren of u toegang web service UI van het Ambari Web tot:

1. Ga naar http://headnodehost:8080 in uw browser. De `headnodehost` adres tot het cluster en los naar de headnode die Ambari op wordt uitgevoerd via de tunnel wordt verzonden. Wanneer u wordt gevraagd, typt u de beheerdersgebruikersnaam (admin) en het wachtwoord voor uw cluster. U wordt mogelijk gevraagd een tweede maal door de Ambari-webgebruikersinterface. Als dit het geval is, voert u de gegevens opnieuw.

   > [!NOTE]
   > Wanneer u het adres http://headnodehost:8080 verbinding maken met het cluster, wordt u verbinding maakt via de tunnel. Communicatie wordt beveiligd met behulp van de SSH-tunnel in plaats van HTTPS. Als u wilt verbinden via internet met behulp van HTTPS, gebruiken https://CLUSTERNAME.azurehdinsight.net, waarbij **CLUSTERNAME** is de naam van het cluster.

2. Selecteer in de Ambari-Webgebruikersinterface HDFS uit de lijst aan de linkerkant van de pagina.

    ![Afbeelding met HDFS geselecteerd](./media/hdinsight-linux-ambari-ssh-tunnel/hdfsservice.png)

3. Wanneer de gegevens van de HDFS-service wordt weergegeven, selecteert u **snelkoppelingen**. Er verschijnt een lijst met de hoofdknooppunten van het cluster. Selecteer een van de hoofdknooppunten en selecteer vervolgens **NameNode UI**.

    ![Afbeelding met het menu QuickLinks uitgevouwen](./media/hdinsight-linux-ambari-ssh-tunnel/namenodedropdown.png)

   > [!NOTE]
   > Wanneer u selecteert __snelkoppelingen__, krijgt u mogelijk een indicator wacht. Dit probleem kan optreden als u een trage internetverbinding hebben. Wacht enkele minuten duren om de gegevens te worden ontvangen van de server en probeer het opnieuw de lijst.
   >
   > Sommige items in de **snelkoppelingen** menu worden afgekapt door aan de rechterkant van het scherm. Zo ja, vouw het menu met de muis en pijl-rechts om te schuiven het scherm naar rechts om de rest van het menu te geven.

4. Een pagina zoals in de volgende afbeelding wordt weergegeven:

    ![Afbeelding van de gebruikersinterface NameNode](./media/hdinsight-linux-ambari-ssh-tunnel/namenode.png)

   > [!NOTE]
   > U ziet de URL voor deze pagina. moet er ongeveer als **http://hn1-CLUSTERNAME.randomcharacters.cx.internal.cloudapp.net:8088/cluster**. Deze URI met behulp van de interne volledig gekwalificeerde domeinnaam (FQDN) van het knooppunt en is alleen toegankelijk wanneer u een SSH-tunnel.

## <a name="next-steps"></a>Volgende stappen

Nu dat u hebt geleerd hoe maken en gebruiken van een SSH-tunnel, Zie het volgende document voor andere manieren om te Ambari gebruiken:

* [HDInsight-clusters beheren met Ambari](hdinsight-hadoop-manage-ambari.md)

Zie voor meer informatie over het gebruik van SSH met HDInsight [SSH gebruiken met HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md).

