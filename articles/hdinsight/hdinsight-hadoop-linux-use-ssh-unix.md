---
title: SSH-sleutels gebruiken met HDInsight (Hadoop) in Windows, Linux, Unix of OS X | Microsoft Docs
description: " U kunt HDInsight openen met Secure Shell (SSH). Dit document bevat informatie over het gebruik van SSH om verbinding te maken met HDInsight in Windows-, Linux-, Unix- of OS X-clients."
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: a6a16405-a4a7-4151-9bbf-ab26972216c5
ms.service: hdinsight
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 03/16/2017
ms.author: larryfr
ms.custom: H1Hack27Feb2017,hdinsightactive
translationtype: Human Translation
ms.sourcegitcommit: 4f2230ea0cc5b3e258a1a26a39e99433b04ffe18
ms.openlocfilehash: 89d44476e9de8ac32195efaf66535cdd9fb4260e
ms.lasthandoff: 03/25/2017

---
# <a name="connect-to-hdinsight-hadoop-using-ssh"></a>Verbinding maken met HDInsight (Hadoop) via SSH

Kom meer te weten over het gebruik van [Secure Shell (SSH)](https://en.wikipedia.org/wiki/Secure_Shell) om veilig verbinding te maken met HDInsight. HDInsight kan gebruikmaken van Linux (Ubuntu) als het besturingssysteem voor knooppunten in het cluster. SSH kan worden gebruikt om verbinding te maken met de hoofd- en Edge-knooppunten van een op Linux gebaseerd cluster en om rechtstreeks op deze knooppunten opdrachten uit te voeren.

De volgende tabel bevat de adres- en poortinformatie die nodig is bij het verbinden met HDInsight via SSH:

| Adres | Poort | Maakt verbinding met... |
| ----- | ----- | ----- |
| `<edgenodename>.<clustername>-ssh.azurehdinsight.net` | 22 | Edge-knooppunt (als er een bestaat) |
| `<clustername>-ssh.azurehdinsight.net` | 22 | Primaire hoofdknooppunt |
| `<clustername>-ssh.azurehdinsight.net` | 23 | Secundaire hoofdknooppunt |

> [!NOTE]
> Vervang `<edgenodename>` door de naam van het Edge-knooppunt. Zie [Edge-knooppunten gebruiken in HDInsight](hdinsight-apps-use-edge-node.md#access-an-edge-node) voor meer informatie over het gebruik van Edge-knooppunten.
>
> Vervang `<clustername>` door de naam van uw HDInsight-cluster.
>
> Het is raadzaam __altijd verbinding te maken met het Edge-knooppunt__ als er een dergelijk knooppunt bestaat. De hoofdknooppunten hosten services die essentieel zijn voor de status van het cluster. Het Edge-knooppunt voert alleen uit wat u op het knooppunt plaatst.

## <a name="ssh-clients"></a>SSH-clients

De meeste besturingssystemen bieden de `ssh`-client. Microsoft Windows biedt niet standaard een SSH-client. Via de volgende pakketten kunt u een SSH-client verkrijgen voor Windows:

* [Bash in Ubuntu in Windows 10](https://msdn.microsoft.com/commandline/wsl/about): de opdracht `ssh` wordt geboden via de Bash-opdrachtregel in Windows.

* [Git (https://git-scm.com/)](https://git-scm.com/): de opdracht `ssh` wordt geboden via de GitBash-opdrachtregel.

* [GitHub Desktop (https://desktop.github.com/)](https://desktop.github.com/): de opdracht `ssh` wordt geboden via de Git Shell-opdrachtregel. GitHub Desktop kan worden geconfigureerd voor het gebruik van Bash, de Windows-opdrachtprompt of PowerShell als de opdrachtregel voor de Git Shell.

* [OpenSSH (https://github.com/PowerShell/Win32-OpenSSH/wiki/Install-Win32-OpenSSH)](https://github.com/PowerShell/Win32-OpenSSH/wiki/Install-Win32-OpenSSH): het PowerShell-team draagt OpenSSH over naar Windows en biedt testversies.

    > [!WARNING]
    > Het OpenSSH-pakket bevat het SSH-serveronderdeel `sshd`. Met dit onderdeel wordt een SSH-server gestart op uw systeem, zodat anderen er verbinding mee kunnen maken. Configureer dit onderdeel alleen en open poort 22 alleen als u een SSH-server wilt hosten op uw systeem. Dit is niet vereist om te communiceren met HDInsight.

Er zijn ook verschillende grafische SSH-clients, zoals [PuTTY (http://www.chiark.greenend.org.uk/~sgtatham/putty/)](http://www.chiark.greenend.org.uk/~sgtatham/putty/) en [MobaXterm (http://mobaxterm.mobatek.net/)](http://mobaxterm.mobatek.net/). Met deze clients kunt u verbinding maken met HDInsight, maar het proces om verbinding te maken met een server, verschilt ten opzichte van het proces in het hulpprogramma `ssh`. Zie voor meer informatie de documentatie van de grafische client die u gebruikt.

## <a id="sshkey"></a>Verificatie: SSH-sleutels

Bij SSH-sleutels wordt gebruikgemaakt van [openbare-sleutelcryptografie](https://en.wikipedia.org/wiki/Public-key_cryptography) voor het beveiligen van het cluster. SSH-sleutels zijn veiliger dan wachtwoorden en bieden een eenvoudige manier om uw HDInsight-cluster te beveiligen.

Als uw SSH-account wordt beveiligd met een sleutel, moet de client tijdens het verbinden de bijbehorende privésleutel opgeven:

* De meeste clients kunnen worden geconfigureerd voor het gebruik van een __standaardsleutel__. De `ssh`-client zoekt bijvoorbeeld via `~/.ssh/id_rsa` naar een privésleutel in Linux- en Unix-omgevingen.

* U kunt het __pad naar een privésleutel__ opgeven. Met de `ssh`-client wordt de parameter `-i` gebruikt om het pad naar de privésleutel op te geven. Bijvoorbeeld `ssh -i ~/.ssh/hdinsight sshuser@myedge.mycluster-ssh.azurehdinsight.net`.

* Als u __meerdere privésleutels__ gebruikt voor verschillende servers, kunnen hulpprogramma's zoals [ssh-agent (https://en.wikipedia.org/wiki/Ssh-agent)](https://en.wikipedia.org/wiki/Ssh-agent) worden gebruikt om automatisch te selecteren welke sleutel wordt gebruikt.

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

Zie [Aan een domein gekoppelde HDInsight-clusters configureren](hdinsight-domain-joined-configure.md) voor meer informatie.

## <a name="connect-to-worker-and-zookeeper-nodes"></a>Verbinding maken met werkrol- en Zookeeper-knooppunten

De werkrolknooppunten en de Zookeeper-knooppunten zijn niet rechtstreeks toegankelijk via internet, maar ze zijn wel toegankelijk via de clusterhoofdknooppunten en Edge-knooppunten. Hier volgen de algemene stappen om verbinding te maken met andere knooppunten:

1. SSH gebruiken om verbinding te maken met een hoofd- of Edge-knooppunt:

        ssh sshuser@myedge.mycluster-ssh.azurehdinsight.net

2. Gebruik van de SSH-verbinding naar het hoofd- of Edge-knooppunt de opdracht `ssh` om verbinding te maken met een werkrolknooppunt in het cluster:

        ssh sshuser@wn0-myhdi

    Raadpleeg de voorbeelden in het document [HDInsight beheren met behulp van de Ambari REST-API](hdinsight-hadoop-manage-ambari-rest-api.md#example-get-the-fqdn-of-cluster-nodes) voor informatie over het ophalen van een lijst van de domeinnamen van de knooppunten in het cluster.

Als het SSH-account is beveiligd met een __wachtwoord__, wordt u gevraagd om het wachtwoord in te voeren. Er wordt vervolgens verbinding gemaakt.

Als u __SSH-sleutels__ gebruikt om uw SSH-account te beveiligen, moet u ervoor zorgen dat uw lokale omgeving is geconfigureerd voor het doorsturen van SSH-agents.

> [!NOTE]
> U kunt ook HDInsight installeren op een virtueel Azure-netwerk om rechtstreeks toegang te verkrijgen tot alle knooppunten in het cluster. U kunt dan uw externe machine koppelen aan datzelfde virtuele netwerk en rechtstreeks toegang verkrijgen tot alle knooppunten in het cluster.
>
> Zie [Een virtueel netwerk gebruiken met HDInsight](hdinsight-extend-hadoop-virtual-network.md) voor meer informatie.

### <a name="configure-ssh-agent-forwarding"></a>Het doorsturen van SSH-agents configureren

> [!IMPORTANT]
> Bij de volgende stappen wordt ervan uitgegaan dat u een op Linux/UNIX gebaseerd systeem gebruikt en dat u werkt met Bash on Windows 10. Als deze stappen niet werken voor uw systeem, moet u mogelijk de documentatie van uw SSH-client raadplegen.

1. Start een teksteditor en open `~/.ssh/config`. Als dit bestand niet bestaat, kunt u dit maken door `touch ~/.ssh/config` in te voeren op een opdrachtregel.

2. Voeg de volgende tekst toe aan het bestand `config`.

        Host <edgenodename>.<clustername>-ssh.azurehdinsight.net
          ForwardAgent yes

    Vervang de __host__informatie door het adres van het knooppunt waar u verbinding mee maakt via SSH. In het vorige voorbeeld werd gebruikgemaakt van het Edge-knooppunt. Hiermee configureert u het doorsturen van de SSH-agent naar het opgegeven knooppunt.

3. Test het doorsturen van de SSH-agent met de volgende opdracht vanaf de terminal:

        echo "$SSH_AUTH_SOCK"

    Met deze opdracht wordt informatie geretourneerd die lijkt op de volgende tekst:

        /tmp/ssh-rfSUL1ldCldQ/agent.1792

    Als er niets wordt geretourneerd, wordt `ssh-agent` niet uitgevoerd. Raadpleeg de opstartscriptinformatie voor agents op [ssh-agent gebruiken met ssh (http://mah.everybody.org/docs/ssh)](http://mah.everybody.org/docs/ssh) of raadpleeg de documentatie bij de SSH-client voor specifieke stappen voor het installeren en configureren van `ssh-agent`.

4. Zodra u hebt gecontroleerd of **ssh-agent** wordt uitgevoerd, gebruikt u de volgende opdracht om uw persoonlijke SSH-sleutel toe te voegen aan de agent:

        ssh-add ~/.ssh/id_rsa

    Als uw persoonlijke sleutel wordt opgeslagen in een ander bestand, vervangt u `~/.ssh/id_rsa` door het pad naar het bestand.

5. Maak met SSH verbinding met het Edge-knooppunt of de hoofdknooppunten van het cluster. Gebruik vervolgens de SSH-opdracht om verbinding te maken met een werkrol- of Zookeeper-knooppunt. De verbinding wordt gemaakt met de doorgestuurde sleutel.

## <a name="next-steps"></a>Volgende stappen

* [SSH-tunneling gebruiken met HDInsight](hdinsight-linux-ambari-ssh-tunnel.md)
* [Een virtueel netwerk gebruiken met HDInsight](hdinsight-extend-hadoop-virtual-network.md)
* [Edge-knooppunten gebruiken in HDInsight](hdinsight-apps-use-edge-node.md#access-an-edge-node)
