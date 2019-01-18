---
title: SSH-tunneling voor toegang tot Azure HDInsight gebruiken
description: Informatie over het gebruik van een SSH-tunnel om te bladeren veilig webresources die worden gehost op uw HDInsight op basis van Linux-knooppunten.
services: hdinsight
author: hrasheed-msft
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 12/15/2018
ms.author: hrasheed
ms.openlocfilehash: 03c86aa069300f88b61752ebd3223e424f6e9c96
ms.sourcegitcommit: ba9f95cf821c5af8e24425fd8ce6985b998c2982
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/17/2019
ms.locfileid: "54382610"
---
# <a name="use-ssh-tunneling-to-access-apache-ambari-web-ui-jobhistory-namenode-apache-oozie-and-other-web-uis"></a>SSH-Tunneling gebruiken voor toegang tot de Apache Ambari-Webgebruikersinterface, JobHistory, NameNode, Apache Oozie en andere webgebruikersinterfaces

HDInsight-clusters bieden toegang tot de Apache Ambari-Webgebruikersinterface via Internet, maar sommige functies vereisen een SSH-tunnel. Bijvoorbeeld, kan niet de web-UI voor de Apache Oozie-service worden geopend via internet zonder een SSh-tunnel.

## <a name="why-use-an-ssh-tunnel"></a>Waarom gebruikt u een SSH-tunnel

Aantal van de menu's van Ambari werken alleen via een SSH-tunnel. Deze menu's, is afhankelijk van websites en services die worden uitgevoerd op andere typen, zoals worker-knooppunten.

De volgende Web-UI's vereisen een SSH-tunnel:

* JobHistory
* NameNode
* Thread-Stacks
* Oozie-Webgebruikersinterface
* Logboeken en HBase Master UI

Als u uw cluster aanpassen met scriptacties, moeten de services en hulpprogramma's die u installeert die beschikbaar maken van een webservice een SSH-tunnel. Bijvoorbeeld, als u Hue met behulp van een scriptactie installeren, moet u een SSH-tunnel gebruiken voor toegang tot de Hue-Webgebruikersinterface.

> [!IMPORTANT]  
> Als u directe toegang tot HDInsight via een virtueel netwerk hebt, hoeft u geen gebruik van SSH-tunnels. Zie voor een voorbeeld van HDInsight rechtstreeks te benaderen via een virtueel netwerk, de [HDInsight verbinden met uw on-premises netwerk](connect-on-premises-network.md) document.

## <a name="what-is-an-ssh-tunnel"></a>Wat is een SSH-tunnel

[Secure Shell (SSH) tunneling](https://en.wikipedia.org/wiki/Tunneling_protocol#Secure_Shell_tunneling) maakt u een poort op uw lokale computer verbinding met een hoofdknooppunt voor HDInsight. Verkeer dat wordt verzonden naar de lokale poort wordt doorgestuurd via een SSH-verbinding met het hoofdknooppunt. De aanvraag is opgelost, alsof deze afkomstig van het hoofdknooppunt is. Het antwoord wordt vervolgens doorgestuurd via de tunnel naar uw werkstation.

## <a name="prerequisites"></a>Vereisten

* Een SSH-client. De meeste besturingssystemen bieden SSH-client via de `ssh` opdracht. Zie [SSH gebruiken met HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md) voor meer informatie.

* Een webbrowser die kan worden geconfigureerd voor het gebruik van een proxy SOCKS5.

    > [!WARNING]  
    > De ondersteuning van de SOCKS-proxy die is ingebouwd in de instellingen voor Windows Internet biedt geen ondersteuning voor SOCKS5 en werkt niet met de stappen in dit document. De volgende browsers zijn afhankelijk van de proxy-instellingen voor Windows en momenteel niet werkt met de stappen in dit document:
    >
    > * Microsoft Edge
    > * Microsoft Internet Explorer
    >
    > Google Chrome is ook afhankelijk van de proxy-instellingen voor Windows. U kunt echter de uitbreidingen die ondersteuning bieden voor SOCKS5 installeren. Het is raadzaam [FoxyProxy Standard](https://chrome.google.com/webstore/detail/foxyproxy-standard/gcknhkkoolaabfmlnjonogaaifnjlfnp).

## <a name="usessh"></a>Maken van een tunnel met behulp van de SSH-opdracht

Gebruik de volgende opdracht uit om te maken van een SSH-tunnel met behulp van de `ssh` opdracht. Vervang **sshuser** met een SSH-gebruiker voor uw HDInsight-cluster en vervang **clustername** met de naam van uw HDInsight-cluster:

```bash
ssh -C2qTnNf -D 9876 sshuser@clustername-ssh.azurehdinsight.net
```

Met deze opdracht maakt een verbinding die verkeer naar de lokale poort 9876 aan het cluster via SSH routeert. De opties zijn:

* **D 9876** -van de lokale poort routeren van verkeer via de tunnel.
* **C** -comprimeren alle gegevens, omdat het webverkeer is voornamelijk tekst.
* **2** -force SSH protocol versie 2 alleen proberen.
* **q** -stille modus.
* **T** -pseudo-tty-toewijzing, uitschakelen, omdat u alleen een poort doorstuurt.
* **n** -te voorkomen dat het lezen van STDIN, omdat u alleen een poort doorstuurt.
* **N** -een externe opdracht niet uitvoeren omdat u alleen een poort doorstuurt.
* **f** -op de achtergrond worden uitgevoerd.

Als de opdracht is voltooid, wordt verkeer dat wordt verzonden naar poort 9876 op de lokale computer wordt doorgestuurd naar het hoofdknooppunt van het cluster.

## <a name="useputty"></a>Maken van een tunnel met PuTTY

[PuTTY](https://www.chiark.greenend.org.uk/~sgtatham/putty) is een grafische SSH-client voor Windows. Als u niet bekend met PuTTY bent, raadpleegt u de [PuTTY-documentatie](https://www.chiark.greenend.org.uk/~sgtatham/putty/docs.html). Gebruik de volgende stappen uit om te maken van een SSH-tunnel gebruiken van PuTTY:

### <a name="create-or-load-a-session"></a>Maken of een sessie laden

1. Open PuTTY en controleer of **sessie** is geselecteerd in het menu links. Als u al een sessie hebt opgeslagen, selecteert u de sessienaam van de **opgeslagen sessies** lijst en klikt u op **Load**.

1. Als u nog een opgeslagen sessie hebt, voert u de verbindingsgegevens van uw:
    * **Hostnaam (of IP-adres)** -de SSH-adres voor het HDInsight-cluster. Bijvoorbeeld, **mijncluster-ssh.azurehdinsight.net**
    * **Poort** - 22
    * **Verbindingstype** : SSH
1. Klik op **Opslaan**.

    ![SSH-sessie maken](./media/hdinsight-linux-ambari-ssh-tunnel/hdinsight-create-putty-session.png)

2. In de **categorie** sectie aan de linkerkant van het dialoogvenster uit, vouw **verbinding**, vouw **SSH**, en selecteer vervolgens **Tunnels**.

3. Geef de volgende informatie op de **opties voor SSH-poort doorsturen** formulier:
   
   * **Bronpoort**: de poort op de client die u wilt doorsturen. Bijvoorbeeld, **9876**.

   * **Bestemming** -de SSH-adres voor het HDInsight-cluster. Bijvoorbeeld **mijncluster-ssh.azurehdinsight.net**.

   * **Dynamisch**: hiermee schakelt u de dynamische routering van SOCKS-proxy's in.
     
     ![afbeelding van de opties voor tunneling](./media/hdinsight-linux-ambari-ssh-tunnel/puttytunnel.png)

4. Klik op **toevoegen** toevoegen van de instellingen en klik vervolgens op **openen** openen van een SSH-verbinding.

5. Wanneer u hierom wordt gevraagd, moet u zich aanmelden bij de server.

## <a name="use-the-tunnel-from-your-browser"></a>Gebruik de tunnel vanuit uw browser

> [!IMPORTANT]  
> De stappen in deze sectie gebruiken de Mozilla FireFox-browser, zoals het biedt de dezelfde proxy-instellingen voor alle platforms. Andere moderne browsers, zoals Google Chrome, is mogelijk een extensie zoals FoxyProxy om te werken met de tunnel.

1. Configureren van de browser gebruikmaakt **localhost** en de poort die u hebt gebruikt tijdens het maken van de tunnel als een **SOCKS v5** proxy. Hier ziet u hoe de instellingen van Firefox eruit zien. Als u een andere poort dan 9876 gebruikt, moet u de poort wijzigen in de versie die u gebruikt:
   
    ![afbeelding van de instellingen van Firefox](./media/hdinsight-linux-ambari-ssh-tunnel/firefoxproxy.png)
   
   > [!NOTE]  
   > Selecteren **externe DNS** wordt omgezet Domain Name System (DNS)-aanvragen met behulp van het HDInsight-cluster. Deze instelling wordt omgezet DNS met behulp van het hoofdknooppunt van het cluster.

2. Controleer of de tunnel werkt via een site, zoals [ https://www.whatismyip.com/ ](https://www.whatismyip.com/). Het geretourneerde IP-adres moet worden gebruikt door de Microsoft Azure-datacenter.

## <a name="verify-with-ambari-web-ui"></a>Controleren met Ambari-Webinterface

Zodra het cluster eenmaal is ingesteld, gebruikt u de volgende stappen uit om te controleren of u toegang hebt tot service webgebruikersinterfaces van de Ambari:

1. Ga in uw browser naar http\:/ / headnodehost:8080. De `headnodehost` adres via de tunnel wordt verzonden naar het cluster en oplossen met het hoofdknooppunt dat Ambari op wordt uitgevoerd. Wanneer u hierom wordt gevraagd, voert u de naam van de gebruiker beheerder (beheerder) en het wachtwoord voor uw cluster. U kunt worden gevraagd een tweede keer door de Ambari-Webgebruikersinterface. Als dit het geval is, voert u de gegevens opnieuw.

   > [!NOTE]  
   > Wanneer met de HTTP-\://headnodehost:8080 adres voor de verbinding met het cluster, u verbinding maakt via de tunnel. Communicatie wordt beveiligd met behulp van de SSH-tunnel in plaats van HTTPS. Als u wilt verbinding maken via internet met behulp van HTTPS, gebruik van https\:/ / clustername.azurehdinsight.net, waar **clustername** is de naam van het cluster.

2. Selecteer in de Ambari-Webgebruikersinterface, HDFS in de lijst aan de linkerkant van de pagina.

    ![Afbeelding met HDFS geselecteerd](./media/hdinsight-linux-ambari-ssh-tunnel/hdfsservice.png)

3. Wanneer de gegevens van de HDFS-service wordt weergegeven, selecteert u **snelkoppelingen**. Er wordt een lijst weergegeven van de hoofdknooppunten van het cluster. Selecteer een van de hoofdknooppunten en selecteer vervolgens **NameNode UI**.

    ![Afbeelding met het menu QuickLinks uitgevouwen](./media/hdinsight-linux-ambari-ssh-tunnel/namenodedropdown.png)

   > [!NOTE]  
   > Wanneer u selecteert __snelkoppelingen__, krijgt u mogelijk een indicator wachten. Dit probleem kan zich voordoen als er een langzame internetverbinding. Wacht een minuut of twee voor de gegevens worden ontvangen van de server en probeer het opnieuw de lijst.
   >
   > Sommige gegevens in de **snelkoppelingen** menu kan worden afgekapt door aan de rechterkant van het scherm. Als dit het geval is, vouw het menu met de muis en gebruik de toets pijl-rechts aan het scherm naar rechts om te zien van de rest van het menu bladeren.

4. Een pagina zoals in de volgende afbeelding wordt weergegeven:

    ![Afbeelding van de gebruikersinterface NameNode](./media/hdinsight-linux-ambari-ssh-tunnel/namenode.png)

   > [!NOTE]  
   > U ziet de URL voor deze pagina. moet er ongeveer als **http\://hn1-CLUSTERNAME.randomcharacters.cx.internal.cloudapp.net:8088/cluster**. Deze URI wordt met behulp van de interne volledig gekwalificeerde domeinnaam (FQDN) van het knooppunt en is alleen toegankelijk wanneer u een SSH-tunnel.

## <a name="next-steps"></a>Volgende stappen

Nu hebt u geleerd hoe u het maken en gebruiken van een SSH-tunnel, Zie het volgende document voor andere manieren om te gebruiken van Ambari:

* [HDInsight-clusters beheren met behulp van Apache Ambari](hdinsight-hadoop-manage-ambari.md)

Zie voor meer informatie over het gebruik van SSH met HDInsight [SSH gebruiken met HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md).

