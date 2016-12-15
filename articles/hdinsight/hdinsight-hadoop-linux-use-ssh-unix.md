---
title: SSH-sleutels gebruiken voor Hadoop (gebaseerd op Linux) in Linux, Unix of OS X | Microsoft Docs
description: " U kunt met Secure Shell (SSH) HDInsight (gebaseerd op Linux) openen. Dit document bevat informatie over het gebruik van SSH met HDInsight op Linux-, Unix- of OS X-clients."
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
ms.date: 09/13/2016
ms.author: larryfr
translationtype: Human Translation
ms.sourcegitcommit: 72ca562c53f813599f19069cfac7ef3ac1957968
ms.openlocfilehash: f64cca8823a74c1c0f52e5d9112836661dc51d8e


---
# <a name="use-ssh-with-linux-based-hadoop-on-hdinsight-from-linux-unix-or-os-x"></a>SSH gebruiken met Hadoop op basis van Linux in HDInsight via Linux, Unix of OS X

> [!div class="op_single_selector"]
> * [Windows](hdinsight-hadoop-linux-use-ssh-windows.md)
> * [Linux, Unix, OS X](hdinsight-hadoop-linux-use-ssh-unix.md)
>
>

Met [Secure Shell (SSH)](https://en.wikipedia.org/wiki/Secure_Shell) kunt u zich aanmelden bij een op Linux gebaseerd HDInsight-cluster en opdrachten uitvoeren via een opdrachtregelinterface. Dit document bevat algemene informatie over SSH en specifieke informatie over het gebruik van SSH met HDInsight.

## <a name="what-is-ssh"></a>Wat is SSH?

SSH is een cryptografisch netwerkprotocol waarmee u veilig kunt communiceren met een externe server via een onbeveiligd netwerk. SSH wordt gebruikt om veilige opdrachtregelaanmelding te bieden bij een externe server. In dit geval zijn dat de hoofdknooppunten of is dat het randknooppunt van een HDInsight-cluster.

U kunt ook gebruikmaken van SSH om netwerkverkeer van uw client naar het HDInsight-cluster te leiden. Met een tunnel kunt u services op het HDInsight-cluster openen die niet rechtstreeks toegankelijk zijn via internet. Zie [SSH-tunnels gebruiken met HDInsight](hdinsight-linux-ambari-ssh-tunnel.md) voor meer informatie over het gebruik van SSH-tunnels.

## <a name="ssh-clients"></a>SSH-clients

Veel besturingssystemen bieden SSH-clientfunctionaliteit via de opdrachtregelprogramma’s `ssh` en `scp`.

* __ssh__: een algemene SSH-client die kan worden gebruikt voor het maken van een externe opdrachtregelsessie en voor het maken van tunnels.
* __scp__: een hulpprogramma dat bestanden tussen lokale en externe systemen kopieert met behulp van het SSH-protocol.

Windows heeft in het verleden geen SSH-client aangeboden; deze werd pas beschikbaar vanaf de Windows 10 Anniversary Edition. Deze versie van Windows bevat de Bash on Windows 10-functie voor ontwikkelaars, met `ssh`, `scp` en andere Linux-opdrachten. Zie [Bash on Ubuntu op Windows](https://msdn.microsoft.com/commandline/wsl/about) voor meer informatie over het gebruik van Bash on Windows 10.

Als u Windows gebruikt en u geen toegang hebt tot Bash on Windows 10, worden de volgende SSH-clients aanbevolen:

* [Git voor Windows](https://git-for-windows.github.io/): voorziet in de opdrachtregelhulpprogramma’s `ssh` en `scp`.
* [PuTTY](http://www.chiark.greenend.org.uk/~sgtatham/putty/): voorziet in een grafische SSH-client.
* [MobaXterm](http://mobaxterm.mobatek.net/): voorziet in een grafische SSH-client.
* [Cygwin](https://cygwin.com/): voorziet in de opdrachtregelhulpprogramma’s `ssh` en `scp`.

> [!NOTE]
> Voor de stappen in dit document wordt ervan uitgegaan dat u toegang hebt tot de opdracht `ssh`. Als u een client zoals puTTY of MobaXterm gebruikt, raadpleegt u de documentatie voor dat product voor de bijbehorende parameters en opdrachten.

## <a name="ssh-authentication"></a>SSH-verificatie

Een SSH-verbinding kan worden geverifieerd met een wachtwoord of [openbare-sleutelcryptografie (https://en.wikipedia.org/wiki/Public-key_cryptography)](https://en.wikipedia.org/wiki/Public-key_cryptography). Een sleutel is de veiligste optie, omdat een sleutel niet zo kwetsbaar is voor aanvallen als een wachtwoord. Het maken en beheren van sleutels is echter gecompliceerder dan het gebruiken van een wachtwoord.

Als u openbare-sleutelcryptografie gebruikt, maakt u een _openbare sleutel_ en een _privésleutel_.

* De **openbare sleutel** wordt in de knooppunten van uw HDInsight-cluster geladen of in een andere service die u wilt gebruiken met openbare-sleutelcryptografie.

* De **privésleutel** biedt u aan het HDInsight-cluster aan wanneer u zich aanmeldt met een SSH-client. De sleutel dient om uw identiteit te verifiëren. Houd deze privésleutel geheim. Deel deze niet.

    U kunt extra beveiliging toevoegen door een wachtwoordzin te maken voor de privésleutel. U moet de wachtwoordzin opgeven om de sleutel te kunnen gebruiken.

### <a name="create-a-public-and-private-key"></a>Een openbare sleutel en een privésleutel maken

Met het hulpprogramma `ssh-keygen` kunt u eenvoudig een openbare sleutel en een privésleutel maken voor gebruik met HDInsight. In een opdrachtregel gebruikt u de volgende opdracht om nieuwe sleutels te maken voor gebruik met HDInsight:

> [!NOTE]
> Als u een GUI SSH-client zoals MobaXTerm of puTTY gebruikt, raadpleegt u de documentatie van de client over het genereren van sleutels.

    ssh-keygen -t rsa -b 2048

U wordt gevraagd naar de volgende informatie:

* De bestandslocatie: de standaardlocatie is `~/.ssh/id_rsa`.

* Een optionele wachtwoordzin: als u een wachtwoordzin invoert, moet u deze opnieuw invoeren bij het verifiëren van uw HDInsight-cluster.

> [!IMPORTANT]
> De wachtwoordzin is een wachtwoord voor de privésleutel. Telkens wanneer u de privésleutel gebruikt om te verifiëren, moet u de wachtwoordzin opgeven om de sleutel te kunnen gebruiken. Als iemand anders uw privésleutel in handen krijgt, kan deze de sleutel niet gebruiken zonder de wachtwoordzin.
>
> Als u de wachtwoordzin vergeet, is er geen manier om deze te herstellen of opnieuw in te stellen.

Wanneer de opdracht is voltooid, hebt u twee nieuwe bestanden:

* __id\_rsa__: dit bestand bevat de privésleutel.

    > [!WARNING]
    > U moet de toegang tot dit bestand beperken om onbevoegde toegang tot services die worden beveiligd met de openbare sleutel, te voorkomen.

* __id\_rsa.pub__: dit bestand bevat de openbare sleutel. U gebruikt dit bestand bij het maken van een HDInsight-cluster.

    > [!NOTE]
    > Het maakt niet uit wie er toegang heeft tot de _openbare_ sleutel. Het enige dat u met de openbare sleutel kunt doen, is de privésleutel controleren. Voor services zoals de SSH-server wordt de openbare sleutel gebruikt om uw identiteit te controleren wanneer u verifieert met de privésleutel.

## <a name="configure-ssh-on-hdinsight"></a>SSH configureren op HDInsight

Wanneer u een op Linux gebaseerd HDInsight-cluster maakt, moet u een _SSH-gebruikersnaam_ en ofwel een _wachtwoord_ ofwel een _openbare sleutel_ opgeven. Deze gegevens worden tijdens het maken van een cluster gebruikt om referenties aan te maken voor de HDInsight-clusterknooppunten. Het wachtwoord of de openbare sleutel wordt gebruikt om het gebruikersaccount te beveiligen.

Raadpleeg een van de volgende documenten voor meer informatie over het configureren van SSH tijdens het maken van een cluster:

* [HDInsight maken met Azure Portal](hdinsight-hadoop-create-linux-clusters-portal.md)
* [HDInsight maken met Azure CLI](hdinsight-hadoop-create-linux-clusters-azure-cli.md)
* [HDInsight maken met Azure PowerShell](hdinsight-hadoop-create-linux-clusters-azure-powershell.md)
* [HDInsight maken met Azure Resource Manager-sjablonen](hdinsight-hadoop-create-linux-clusters-arm-templates.md)
* [HDInsight maken met de .NET SDK](hdinsight-hadoop-create-linux-clusters-dotnet-sdk.md)
* [HDInsight maken met REST](hdinsight-hadoop-create-linux-clusters-curl-rest.md)

### <a name="additional-ssh-users"></a>Aanvullende SSH-gebruikers

Hoewel er aanvullende SSH-gebruikers kunnen worden toegevoegd aan het cluster nadat het is gemaakt, wordt dit niet aanbevolen.

* U moet handmatig nieuwe SSH-gebruikers toevoegen aan elk knooppunt in het cluster.

* Nieuwe SSH-gebruikers hebben dezelfde toegang tot HDInsight als de standaardgebruiker. Er is geen enkele manier om de toegang tot gegevens of taken te beperken in HDInsight op basis van een SSH-gebruikersaccount.

Als u de toegang per gebruiker wilt kunnen beperken, moet u een HDInsight-cluster gebruiken dat is gekoppeld aan een domein. Bij aan een domein gekoppelde HDInsight wordt gebruikgemaakt van Active Directory om de toegang tot clusterresources te beheren.

Als u een aan een domein gekoppeld HDInsight-cluster gebruikt, kunt u verifiëren met behulp van Active Directory nadat u verbinding hebt gemaakt met SSH. Meerdere gebruikers kunnen verbinding maken met SSH en vervolgens verificatie uitvoeren voor hun Active Directory-account. Zie het gedeelte [HDInsight gekoppeld aan een domein](#domainjoined) voor meer informatie.

##<a name="a-idconnecta-connect-to-hdinsight"></a><a id="connect"></a> Verbinding maken met HDInsight

Op alle knooppunten in een HDInsight-cluster wordt de SSH-server uitgevoerd, maar u kunt alleen via het openbare internet verbinding maken met de hoofd- of Edge-knooppunten.

* Als u verbinding wilt maken met de _hoofdknooppunten_, gebruikt u `CLUSTERNAME-ssh.azurehdinsight.net`, waarbij __CLUSTERNAME__ de naam is van het HDInsight-cluster. Als u verbinding maakt via poort 22 (de standaardpoort voor SSH), wordt er verbinding gemaakt met het primaire hoofdknooppunt. Als u poort 23 gebruikt, wordt er verbinding gemaakt met het secundaire hoofdknooppunt.

* Als u verbinding wilt maken met een _Edge-knooppunt_, gebruikt u `EDGENAME.CLUSTERNAME-ssh.azurehdinsight.net`, waarbij __EDGENAME__ de naam is van het Edge-knooppunt en __CLUSTERNAME__ de naam is van het HDInsight-cluster. Gebruik poort 22 als u verbinding wilt maken met een Edge-knooppunt.

In de volgende voorbeelden ziet u hoe u de hoofdknooppunten en het Edge-knooppunt van een cluster met de naam __myhdi__ met elkaar verbindt met de SSH-gebruikersnaam __sshuser__. Het Edge-knooppunt heet __myedge__.

| Als u dit wilt doen... | Gebruikt u... |
| ----- | ----- |
| Verbinding maken met het primaire hoofdknooppunt | `ssh sshuser@myhdi-ssh.azurehdinsight.net` |
| Verbinding maken met het secundaire hoofdknooppunt | `ssh -p 23 sshuser@myhdi-ssh.azurehdinsight.net` |
| Verbinding maken met het edge-knooppunt | `ssh sshuser@edge.myhdi-ssh.azurehdinsight.net` |

Als u een wachtwoord gebruikt om het SSH-account te beveiligen, wordt u gevraagd om het wachtwoord in te voeren.

Als u een openbare sleutel gebruikt om het SSH-account te beveiligen, moet u mogelijk het pad opgeven naar de bijbehorende privésleutel. Dat kan met behulp van de `-i`-switch. In het volgende voorbeeld ziet u hoe de `-i`-switch wordt gebruikt:

    ssh -i /path/to/public.key sshuser@myhdi-ssh.azurehdinsight.net

### <a name="connect-to-other-nodes"></a>Verbinding maken met andere knooppunten

De werkrolknooppunten en de Zookeeper-knooppunten zijn niet rechtstreeks toegankelijk van buiten het cluster, maar ze zijn wel toegankelijk via de clusterhoofdknooppunten en Edge-knooppunten. Hier volgen de algemene stappen om dit te doen:

1. SSH gebruiken om verbinding te maken met een hoofd- of Edge-knooppunt:

        ssh sshuser@myhdi-ssh.azurehdinsight.net

2. Gebruik van de SSH-verbinding naar het hoofd- of Edge-knooppunt de opdracht `ssh` om verbinding te maken met een werkrolknooppunt in het cluster:

        ssh sshuser@wn0-myhdi

    Raadpleeg het document [HDInsight beheren met behulp van de Ambari REST-API](hdinsight-hadoop-manage-ambari-rest-api.md#example-get-the-fqdn-of-cluster-nodes) voor informatie over het ophalen van een lijst van de werkrolknooppunten in het cluster. In het document staat een voorbeeld van hoe u de FQDN-namen van clusterknooppunten ophaalt.

Als het SSH-account is beveiligd met een wachtwoord, wordt u gevraagd om het wachtwoord in te voeren. Er wordt vervolgens verbinding gemaakt.

Als u een SSH-sleutel gebruikt om uw gebruikersaccount te verifiëren, moet u ervoor zorgen dat uw lokale omgeving is geconfigureerd voor het doorsturen van SSH-agents.

> [!IMPORTANT]
> Bij de volgende stappen wordt ervan uitgegaan dat u een op Linux/UNIX gebaseerd systeem gebruikt en dat u werkt met Bash on Windows 10. Als deze stappen niet werken voor uw systeem, moet u mogelijk de documentatie van uw SSH-client raadplegen.

1. Start een teksteditor en open `~/.ssh/config`. Als dit bestand niet bestaat, kunt u dit maken door `touch ~/.ssh/config` in te voeren in een opdrachtregel.

2. Voeg het volgende toe aan het bestand. Vervang *CLUSTERNAME* door de naam van uw HDInsight-cluster.

        Host CLUSTERNAME-ssh.azurehdinsight.net
          ForwardAgent yes

    Hiermee configureert u het doorsturen van de SSH-agent voor uw HDInsight-cluster.

3. Test het doorsturen van de SSH-agent met de volgende opdracht vanaf de terminal:

        echo "$SSH_AUTH_SOCK"

    Met deze opdracht wordt informatie geretourneerd die lijkt op de volgende tekst:

        /tmp/ssh-rfSUL1ldCldQ/agent.1792

    Als er niets wordt geretourneerd, betekent dit dat `ssh-agent` niet wordt uitgevoerd. Raadpleeg de opstartscriptinformatie voor agents op [ssh-agent gebruiken met ssh (http://mah.everybody.org/docs/ssh)](http://mah.everybody.org/docs/ssh) of raadpleeg de documentatie bij de SSH-client voor specifieke stappen voor het installeren en configureren van `ssh-agent`.

4. Zodra u hebt gecontroleerd of **ssh-agent** wordt uitgevoerd, gebruikt u de volgende opdracht om uw persoonlijke SSH-sleutel toe te voegen aan de agent:

        ssh-add ~/.ssh/id_rsa

    Als uw persoonlijke sleutel wordt opgeslagen in een ander bestand, vervangt u `~/.ssh/id_rsa` door het pad naar het bestand.

<a id="domainjoined"></a>
### <a name="domain-joined-hdinsight"></a>HDInsight gekoppeld aan een domein

Met [HDInsight gekoppeld aan een domein](hdinsight-domain-joined-introduction.md) kan Kerberos in Hadoop worden geïntegreerd in HDInsight. Omdat de SSH-gebruiker geen Active Directory-domeingebruiker is, kunt u pas Hadoop-opdrachten uitvoeren nadat u verificatie hebt uitgevoerd met Active Directory. Volg deze stappen om uw SSH-sessie te verifiëren met Active Directory:

1. Maak verbinding met een HDInsight-cluster dat is gekoppeld aan een domein. Doe dit met de SSH, zoals wordt beschreven in het gedeelte [Verbinding maken met HDInsight](#connect). Met de volgende opdracht maakt u bijvoorbeeld verbinding met een HDInsight-cluster met de naam __myhdi__ via een SSH-account met de naam __sshuser__.

        ssh sshuser@myhdi-ssh.azurehdinsight.net

2. Ga als volgt te werk om te verifiëren met een domeingebruiker en een wachtwoord:

        kinit

     Wanneer hierom wordt gevraagd, voert u de domeingebruikersnaam en het wachtwoord voor de domeingebruiker in.

    Raadpleeg [Aan een domein gekoppelde HDInisight-clusters configureren](hdinsight-domain-joined-configure.md) voor meer informatie over het configureren van domeingebruikers voor aan een domein gekoppelde HDInsight-clusters.

Na verificatie met de opdracht `kinit` kunt u Hadoop-opdrachten zoals `hdfs dfs -ls /` en `hive` gebruiken.

## <a name="a-idtunnelassh-tunneling"></a><a id="tunnel"></a>SSH-tunneling

SSH kan worden gebruikt voor de tunneling van lokale aanvragen, zoals webaanvragen, naar het HDInsight-cluster. De aanvraag wordt vervolgens naar de aangevraagde resource gerouteerd, alsof de aanvraag afkomstig is van het hoofdknooppunt van het HDInsight-cluster.

> [!IMPORTANT]
> Een SSH-tunnel is vereist voor toegang tot de webgebruikersinterface voor bepaalde Hadoop-services. De gebruikersinterface van bijvoorbeeld Taakgeschiedenis of Resource Manager is alleen toegankelijk via een SSH-tunnel.

Raadpleeg [SSH-tunneling gebruiken voor toegang tot de Ambari-webgebruikersinterface, JobHistory, NameNode, Oozie en andere webgebruikersinterfaces van ](hdinsight-linux-ambari-ssh-tunnel.md) voor meer informatie over het maken en gebruiken van een SSH-tunnel.

## <a name="next-steps"></a>Volgende stappen

Nu u weet hoe zich kunt verifiëren met een SSH-sleutel, wordt uitgelegd hoe u MapReduce gebruikt met Hadoop op HDInsight.

* [Hive gebruiken met HDInsight](hdinsight-use-hive.md)
* [Pig gebruiken met HDInsight](hdinsight-use-pig.md)
* [MapReduce-taken gebruiken met HDInsight](hdinsight-use-mapreduce.md)

[preview-portal]: https://portal.azure.com/



<!--HONumber=Dec16_HO1-->


