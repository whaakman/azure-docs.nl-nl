---
title: SSH gebruiken met Hadoop - Azure HDInsight | Microsoft Docs
description: U kunt HDInsight openen met Secure Shell (SSH). Dit document bevat informatie over het verbinding maken met HDInsight met behulp van de opdrachten ssh en scp vanuit Windows-, Linux-, Unix- of macOS-clients.
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
tags: azure-portal
keywords: hadoop-opdrachten in linux,hadoop linux-opdrachten,hadoop-macro's,ssh hadoop,ssh hadoop-cluster
ms.assetid: a6a16405-a4a7-4151-9bbf-ab26972216c5
ms.service: hdinsight
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 02/07/2018
ms.author: larryfr
ms.custom: H1Hack27Feb2017,hdinsightactive,hdiseo17may2017
ms.openlocfilehash: 1a1fbac1eb4c529ea89f0f4c72b3317718ac45ea
ms.sourcegitcommit: 059dae3d8a0e716adc95ad2296843a45745a415d
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/09/2018
---
# <a name="connect-to-hdinsight-hadoop-using-ssh"></a>Verbinding maken met HDInsight (Hadoop) via SSH

Lees hier hoe u met [Secure Shell (SSH)](https://en.wikipedia.org/wiki/Secure_Shell) veilig verbinding kunt maken met Hadoop in Azure HDInsight. 

HDInsight kan Linux (Ubuntu) als het besturingssysteem gebruiken voor knooppunten in het Hadoop-cluster. De volgende tabel bevat de adres- en poortgegevens die nodig zijn bij het verbinden met HDInsight in Linux via een SSH-client:

| Adres | Poort | Maakt verbinding met... |
| ----- | ----- | ----- |
| `<clustername>-ed-ssh.azurehdinsight.net` | 22 | Edge-knooppunt (R Server op HDInsight) |
| `<edgenodename>.<clustername>-ssh.azurehdinsight.net` | 22 | Edge-knooppunt (ieder ander clustertype, als er een Edge-knooppunt bestaat) |
| `<clustername>-ssh.azurehdinsight.net` | 22 | Primaire hoofdknooppunt |
| `<clustername>-ssh.azurehdinsight.net` | 23 | Secundaire hoofdknooppunt |

> [!NOTE]
> Vervang `<edgenodename>` door de naam van het Edge-knooppunt.
>
> Vervang `<clustername>` door de naam van uw cluster.
>
> Als uw cluster een edge-knooppunt bevat, raden we aan om __altijd verbinding te maken met het edge-knooppunt__ via SSH. De hoofdknooppunten hosten services die essentieel zijn voor de status van Hadoop. Het Edge-knooppunt voert alleen uit wat u op het knooppunt plaatst.
>
> Zie [Edge-knooppunten gebruiken in HDInsight](hdinsight-apps-use-edge-node.md#access-an-edge-node) voor meer informatie over het gebruik van Edge-knooppunten.

> [!TIP]
> Wanneer u voor het eerst verbinding maakt met HDInsight, wordt in de SSH-client mogelijk de waarschuwing weergegeven dat de authenticiteit van de host kan niet worden vastgesteld. Wanneer dit wordt gevraagd, selecteert u Ja om de host toe te voegen aan de lijst met vertrouwde servers van uw SSH-client.
>
> Als u eerder verbinding hebt gehad met een server met dezelfde naam, krijgt u mogelijk de waarschuwing dat de opgeslagen hostsleutel niet overeenkomt met de hostsleutel van de server. Raadpleeg de documentatie van de SSH-client als u wilt weten hoe u de bestaande vermelding voor de server verwijdert.

## <a name="ssh-clients"></a>SSH-clients

Linux-, Unix-en macOS-systemen bieden de opdrachten `ssh` en `scp`. De `ssh`-client wordt meestal gebruikt om vanaf de opdrachtregel een externe sessie op te zetten met een Linux- of Unix-systeem. De `scp`-client wordt gebruikt om veilig bestanden te kopiëren tussen de client en het externe systeem.

In Microsoft Windows worden SSH-clients niet standaard geïnstalleerd. De `ssh`- en `scp`-clients zijn beschikbaar voor Windows via de volgende pakketten:

* OpenSSH-client (bètaversie): ga in de Fall Creators Update naar __Instellingen__ > __Apps en onderdelen__ > __Optionele onderdelen beheren__ > __Een onderdeel toevoegen__ en selecteer de __OpenSSH-client__. 

    > [!NOTE]
    > Als de opdrachten `ssh` en `scp` niet beschikbaar zijn in PowerShell nadat u deze functie hebt ingeschakeld, meldt u zich af en weer aan.

* [Bash in Ubuntu in Windows 10](https://msdn.microsoft.com/commandline/wsl/about): de opdrachten `ssh` en `scp` zijn beschikbaar via de Bash-opdrachtregel in Windows.

* [OpenSSH-client (bèta)](https://blogs.msdn.microsoft.com/powershell/2017/12/15/using-the-openssh-beta-in-windows-10-fall-creators-update-and-windows-server-1709/): dit is een optionele functie die is geïntroduceerd in de Windows 10 Fall Creators Update.

* [Azure Cloud Shell](../cloud-shell/quickstart.md): de Cloud Shell biedt een Bash-omgeving in uw browser en stelt de opdrachten `ssh`, `scp`, en andere algemene Linux-opdrachten beschikbaar.

* [Git (https://git-scm.com/)](https://git-scm.com/): de opdrachten `ssh` en `scp` zijn beschikbaar via de GitBash-opdrachtregel.

Er zijn ook verschillende grafische SSH-clients, zoals [PuTTY (http://www.chiark.greenend.org.uk/~sgtatham/putty/)](http://www.chiark.greenend.org.uk/~sgtatham/putty/) en [MobaXterm (http://mobaxterm.mobatek.net/)](http://mobaxterm.mobatek.net/). Met deze clients kunt u verbinding maken met HDInsight, maar het proces om verbinding, verschilt ten opzichte van het proces in het hulpprogramma `ssh`. Zie voor meer informatie de documentatie van de grafische client die u gebruikt.

## <a id="sshkey"></a>Verificatie: SSH-sleutels

SSH-sleutels gebruiken [openbare-sleutelcryptografie](https://en.wikipedia.org/wiki/Public-key_cryptography) voor het beveiligen van SSH-sessies. SSH-sleutels zijn veiliger dan wachtwoorden en bieden een eenvoudige manier om de toegang tot uw Hadoop-cluster te beveiligen.

Als uw SSH-account wordt beveiligd met een sleutel, moet de client tijdens het verbinden de bijbehorende privésleutel opgeven:

* De meeste clients kunnen worden geconfigureerd voor het gebruik van een __standaardsleutel__. De `ssh`-client zoekt bijvoorbeeld via `~/.ssh/id_rsa` naar een privésleutel in Linux- en Unix-omgevingen.

* U kunt het __pad naar een privésleutel__ opgeven. Met de `ssh`-client wordt de parameter `-i` gebruikt om het pad naar de privésleutel op te geven. Bijvoorbeeld `ssh -i ~/.ssh/id_rsa sshuser@myedge.mycluster-ssh.azurehdinsight.net`.

* Als u __meerdere privésleutels__ gebruikt voor verschillende servers, kunt u overwegen om een hulpprogramma zoals [ssh-agent (https://en.wikipedia.org/wiki/Ssh-agent)](https://en.wikipedia.org/wiki/Ssh-agent) te gebruiken. Het hulpprogramma `ssh-agent` kan worden gebruikt voor het automatisch selecteren van de sleutel die moet worden gebruikt bij het tot stand brengen van een SSH-sessie.

> [!IMPORTANT]
>
> Als u uw privésleutel beveiligt met een wachtwoordzin, moet u de wachtwoordzin invoeren bij gebruik van de sleutel. Hulpprogramma's zoals `ssh-agent` kunnen het wachtwoord voor extra gebruiksgemak in de cache opslaan.

### <a name="create-an-ssh-key-pair"></a>Een SSH-sleutelpaar maken

Gebruik de opdracht `ssh-keygen` om openbare- en privésleutelbestanden te maken. Met de volgende opdracht maakt u een 2048-bits RSA-sleutelpaar dat kan worden gebruikt met HDInsight:

    ssh-keygen -t rsa -b 2048

U wordt tijdens het maken van de sleutel gevraagd om informatie. U moet bijvoorbeeld opgeven waar de sleutels worden opgeslagen en er wordt gevraagd of u een wachtwoordzin wilt gebruiken. Wanneer het proces is voltooid, worden er twee bestanden gemaakt: een openbare sleutel en een privésleutel.

* De __openbare sleutel__ wordt gebruikt om een HDInsight-cluster te maken. De openbare sleutel heeft de extensie `.pub`.

* De __privésleutel__ wordt gebruikt om uw client te verifiëren bij het HDInsight-cluster.

> [!IMPORTANT]
> U kunt uw sleutels beveiligen met een wachtwoordzin. Dit is in feite een wachtwoord voor uw privésleutel. Wanneer iemand uw privésleutel in handen krijgt, heeft deze óók de wachtwoordzin nodig om de sleutel te kunnen gebruiken.

### <a name="create-hdinsight-using-the-public-key"></a>HDInsight maken met de openbare sleutel

| Methode voor het maken | De openbare sleutel gebruiken |
| ------- | ------- |
| **Azure Portal** | Schakel het selectievakje bij __Hetzelfde wachtwoord als voor aanmelding bij cluster gebruiken__ uit en selecteer __Openbare sleutel__ als SSH-verificatietype. Tot slot selecteert u het openbare-sleutelbestand of plakt u de tekstinhoud van het bestand in het veld __Openbare SSH-sleutel__.</br>![Dialoogvenster voor de openbare SSH-sleutel tijdens het maken van een HDInsight-cluster](./media/hdinsight-hadoop-linux-use-ssh-unix/create-hdinsight-ssh-public-key.png) |
| **Azure PowerShell** | Gebruik de parameter `-SshPublicKey` van de cmdlet `New-AzureRmHdinsightCluster` en plak de inhoud van de openbare sleutel als tekenreeks.|
| **Azure CLI 1.0** | Gebruik de parameter `--sshPublicKey` van de opdracht `azure hdinsight cluster create` en plak de inhoud van de openbare sleutel als tekenreeks. |
| **Resource Manager-sjabloon** | Zie [HDInsight op Linux implementeren met een SSH-sleutel](https://azure.microsoft.com/resources/templates/101-hdinsight-linux-ssh-publickey/) voor een voorbeeld van het gebruik van SSH-sleutels met een sjabloon. Het `publicKeys`-element in het bestand [azuredeploy.json](https://github.com/Azure/azure-quickstart-templates/blob/master/101-hdinsight-linux-ssh-publickey/azuredeploy.json) wordt gebruikt om sleutels door te geven aan Azure bij het maken van het cluster. |

## <a id="sshpassword"></a>Verificatie: wachtwoord

SSH-accounts kunnen worden beveiligd met een wachtwoord. Als u met SSH verbinding maakt met HDInsight, wordt u gevraagd om het wachtwoord in te voeren.

> [!WARNING]
> Het wordt niet aangeraden om wachtwoordverificatie te gebruiken voor SSH. Wachtwoorden kunnen worden geraden en zijn gevoelig voor ernstige aanvallen. In plaats daarvan wordt aangeraden om [SSH-sleutels te gebruiken voor verificatie](#sshkey).

### <a name="create-hdinsight-using-a-password"></a>HDInsight maken met een wachtwoord

| Methode voor het maken | Het wachtwoord specificeren |
| --------------- | ---------------- |
| **Azure Portal** | Het SSH-gebruikersaccount heeft standaard hetzelfde wachtwoord als het aanmeldingsaccount van het cluster. Als u een ander wachtwoord wilt gebruiken, schakelt u het selectievakje bij __Hetzelfde wachtwoord als bij aanmelding voor cluster__ uit en voert u het wachtwoord in in het veld __SSH-wachtwoord__.</br>![Dialoogvenster voor het SSH-wachtwoord tijdens het maken van een HDInsight-cluster](./media/hdinsight-hadoop-linux-use-ssh-unix/create-hdinsight-ssh-password.png)|
| **Azure PowerShell** | Gebruik de parameter `--SshCredential` van de cmdlet `New-AzureRmHdinsightCluster` en geef een `PSCredential`-object op dat de SSH-gebruikersaccountnaam en het wachtwoord bevat. |
| **Azure CLI 1.0** | Gebruik de parameter `--sshPassword` van de opdracht `azure hdinsight cluster create` en geef de wachtwoordwaarde op. |
| **Resource Manager-sjabloon** | Zie [HDInsight op Linux implementeren met een SSH-wachtwoord](https://azure.microsoft.com/resources/templates/101-hdinsight-linux-ssh-password/) voor een voorbeeld van het gebruik met een wachtwoord met een sjabloon. Het `linuxOperatingSystemProfile`-element in het bestand [azuredeploy.json](https://github.com/Azure/azure-quickstart-templates/blob/master/101-hdinsight-linux-ssh-password/azuredeploy.json) wordt gebruikt om de SSH-accountnaam en het wachtwoord door te geven aan Azure bij het maken van het cluster.|

### <a name="change-the-ssh-password"></a>Het SSH-wachtwoord wijzigen

Zie het gedeelte __Wachtwoorden wijzigen__ van het document [HDInsight beheren](hdinsight-administer-use-portal-linux.md#change-passwords) voor meer informatie over het wijzigen van het wachtwoord van het SSH-gebruikersaccount.

## <a id="domainjoined"></a>Verificatie: HDInsight, gekoppeld aan een domein

Als u een __HDInsight-cluster gebruikt dat is gekoppeld aan een domein__, moet u de opdracht `kinit` gebruiken na het verbinden met SSH. Met deze opdracht wordt u om een domeingebruiker en een wachtwoord gevraagd. Uw sessie wordt geverifieerd bij het Azure Active Directory-domein dat is gekoppeld aan het cluster.

Zie [Aan een domein gekoppelde HDInsight-clusters configureren](./domain-joined/apache-domain-joined-configure.md) voor meer informatie.

## <a name="connect-to-nodes"></a>Verbinding maken met knooppunten

De hoofdknooppunten en het edge-knooppunt (indien aanwezig) zijn toegankelijk via het internet op poort 22 en 23.

* Bij het verbinden met de __hoofdknooppunten__, gebruikt u poort __22__ om verbinding te maken met het primaire hoofdknooppunt en poort __23__ om verbinding te maken met het secundaire hoofdknooppunt. De FQDN (Fully Qualified Domain Name) die u moet gebruiken is `clustername-ssh.azurehdinsight.net`, waarbij `clustername` de naam van uw cluster is.

    ```bash
    # Connect to primary head node
    # port not specified since 22 is the default
    ssh sshuser@clustername-ssh.azurehdinsight.net

    # Connect to secondary head node
    ssh -p 23 sshuser@clustername-ssh.azurehdinsight.net
    ```
    
* Gebruik poort 22 als u verbinding wilt maken met het __edge-knooppunt__. De FQDN (Fully Qualified Domain Name) is `edgenodename.clustername-ssh.azurehdinsight.net`, waarbij `edgenodename` een naam is die u hebt opgegeven bij maken van het edge-knooppunt. `clustername`is de naam van het cluster.

    ```bash
    # Connect to edge node
    ssh sshuser@edgnodename.clustername-ssh.azurehdinsight.net
    ```

> [!IMPORTANT]
> In de vorige voorbeelden wordt ervan uitgegaan dat u wachtwoordverificatie gebruikt of dat certificaatverificatie automatisch wordt uitgevoerd. Als u een SSH-sleutelpaar voor verificatie gebruikt en het certificaat niet automatisch wordt toegepast, moet u de parameter `-i` gebruiken om de persoonlijke sleutel op te geven. Bijvoorbeeld `ssh -i ~/.ssh/mykey sshuser@clustername-ssh.azurehdinsight.net`.

Wanneer u verbinding hebt gemaakt, verandert de prompt en geeft deze de SSH-gebruikersnaam en het knooppunt waarmee u bent verbonden weer. Wanneer u bijvoorbeeld verbonden bent met het primaire hoofdknooppunt als `sshuser`, is de prompt `sshuser@hn0-clustername:~$`.

### <a name="connect-to-worker-and-zookeeper-nodes"></a>Verbinding maken met werkrol- en Zookeeper-knooppunten

De werkrol-knooppunten en Zookeeper-knooppunten zijn niet rechtstreeks toegankelijk vanaf internet. Ze zijn toegankelijk vanuit de hoofdknooppunten of edge-knooppunten van het cluster. Hier volgen de algemene stappen om verbinding te maken met andere knooppunten:

1. SSH gebruiken om verbinding te maken met een hoofd- of Edge-knooppunt:

        ssh sshuser@myedge.mycluster-ssh.azurehdinsight.net

2. Gebruik van de SSH-verbinding naar het hoofd- of Edge-knooppunt de opdracht `ssh` om verbinding te maken met een werkrolknooppunt in het cluster:

        ssh sshuser@wn0-myhdi

    Raadpleeg het document [HDInsight beheren met behulp van de Ambari REST-API](hdinsight-hadoop-manage-ambari-rest-api.md#example-get-the-fqdn-of-cluster-nodes) voor informatie over het ophalen van een lijst met de namen van de knooppunten.

Als het SSH-account is beveiligd met een __wachtwoord__, voert u het wachtwoord in om verbinding te maken.

Als het SSH-account is beveiligd met __SSH-sleutels__, moet SSH-forwarding zijn ingeschakeld op de client.

> [!NOTE]
> U kunt ook HDInsight installeren op een virtueel Azure-netwerk om rechtstreeks toegang te verkrijgen tot alle knooppunten in het cluster. U kunt dan uw externe machine koppelen aan datzelfde virtuele netwerk en rechtstreeks toegang verkrijgen tot alle knooppunten in het cluster.
>
> Zie [Een virtueel netwerk gebruiken met HDInsight](hdinsight-extend-hadoop-virtual-network.md) voor meer informatie.

### <a name="configure-ssh-agent-forwarding"></a>Het doorsturen van SSH-agents configureren

> [!IMPORTANT]
> Bij de volgende stappen wordt ervan uitgegaan dat u een op Linux of UNIX gebaseerd systeem gebruikt en dat u werkt met Bash in Windows 10. Als deze stappen niet werken voor uw systeem, moet u mogelijk de documentatie van uw SSH-client raadplegen.

1. Start een teksteditor en open `~/.ssh/config`. Als dit bestand niet bestaat, kunt u dit maken door `touch ~/.ssh/config` in te voeren op een opdrachtregel.

2. Voeg de volgende tekst toe aan het bestand `config`.

        Host <edgenodename>.<clustername>-ssh.azurehdinsight.net
          ForwardAgent yes

    Vervang de __host__informatie door het adres van het knooppunt waar u verbinding mee maakt via SSH. In het vorige voorbeeld werd gebruikgemaakt van het Edge-knooppunt. Hiermee configureert u het doorsturen van de SSH-agent naar het opgegeven knooppunt.

3. Test het doorsturen van de SSH-agent met de volgende opdracht vanaf de terminal:

        echo "$SSH_AUTH_SOCK"

    Met deze opdracht wordt informatie geretourneerd die lijkt op de volgende tekst:

        /tmp/ssh-rfSUL1ldCldQ/agent.1792

    Als er niets wordt geretourneerd, wordt `ssh-agent` niet uitgevoerd. Raadpleeg voor meer informatie de opstartscriptinformatie voor agents op [ssh-agent gebruiken met ssh (http://mah.everybody.org/docs/ssh)](http://mah.everybody.org/docs/ssh) of raadpleeg de documentatie bij de SSH-client.

4. Nadat u hebt gecontroleerd of **ssh-agent** wordt uitgevoerd, gebruikt u de volgende opdracht om uw persoonlijke SSH-sleutel toe te voegen aan de agent:

        ssh-add ~/.ssh/id_rsa

    Als uw persoonlijke sleutel wordt opgeslagen in een ander bestand, vervangt u `~/.ssh/id_rsa` door het pad naar het bestand.

5. Maak met SSH verbinding met het Edge-knooppunt of de hoofdknooppunten van het cluster. Gebruik vervolgens de SSH-opdracht om verbinding te maken met een werkrol- of Zookeeper-knooppunt. De verbinding wordt gemaakt met de doorgestuurde sleutel.

## <a name="copy-files"></a>Bestanden kopiëren

Het hulpprogramma `scp` kan worden gebruikt om bestanden te kopiëren naar en van afzonderlijke knooppunten in het cluster. De volgende opdracht kopieert bijvoorbeeld de map `test.txt` uit het lokale systeem naar het primaire hoofdknooppunt:

```bash
scp test.txt sshuser@clustername-ssh.azurehdinsight.net:
```

Omdat er geen pad is opgegeven na de `:`, wordt het bestand geplaatst in de basismap `sshuser`.

In het volgende voorbeeld wordt het bestand `test.txt` uit de basismap `sshuser` op het primaire hoofdknooppunt gekopieerd naar het lokale systeem:

```bash
scp sshuser@clustername-ssh.azurehdinsight.net:test.txt .
```

> [!IMPORTANT]
> `scp` heeft alleen toegang tot het bestandssysteem van afzonderlijke knooppunten in het cluster. Het kan niet worden gebruikt voor toegang tot gegevens in de Hadoop Distributed File System-compatibele opslag voor het cluster.
>
> Gebruik `scp` wanneer u een resource moet uploaden voor gebruik in een SSH-sessie. Upload bijvoorbeeld van een Python-script en voer het script uit in een SSH-sessie.
>
> Zie de volgende documenten voor informatie over het rechtstreeks laden van gegevens in de Hadoop Distributed File System-compatibele opslag:
>
> * [HDInsight op basis van Azure Storage](hdinsight-hadoop-use-blob-storage.md).
>
> * [HDInsight op basis van Azure Data Lake Store](hdinsight-hadoop-use-data-lake-store.md).

## <a name="next-steps"></a>Volgende stappen

* [SSH-tunneling gebruiken met HDInsight](hdinsight-linux-ambari-ssh-tunnel.md)
* [Een virtueel netwerk gebruiken met HDInsight](hdinsight-extend-hadoop-virtual-network.md)
* [Edge-knooppunten gebruiken in HDInsight](hdinsight-apps-use-edge-node.md#access-an-edge-node)