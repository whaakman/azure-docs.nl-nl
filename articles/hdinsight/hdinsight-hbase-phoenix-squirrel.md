---
title: Gebruik van Apache Phoenix en SQuirreL met Azure HDInsight op basis van Windows | Microsoft Docs
description: Informatie over het gebruik van Apache Phoenix in HDInsight en het installeren en configureren van SQuirreL op uw werkstation verbinding maken met een HBase-cluster in HDInsight.
services: hdinsight
documentationcenter: 
author: mumian
manager: jhubbard
editor: cgronlun
ms.assetid: 1a756e98-75c1-44cd-a178-c5119683b7b7
ms.service: hdinsight
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 05/25/2017
ms.author: jgao
ROBOTS: NOINDEX
ms.openlocfilehash: 04392b535965edd785bbb66a52eb6b41b768553e
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="use-apache-phoenix-and-squirrel-with-windows-based-hbase-clusters-in-hdinsight"></a>Apache Phoenix en SQuirreL gebruiken met HBase op basis van Windows-clusters in HDInsight
Informatie over het gebruik [Apache Phoenix](http://phoenix.apache.org/) in HDInsight en het installeren en configureren van SQuirreL op uw werkstation verbinding maken met een HBase-cluster in HDInsight. Zie voor meer informatie over Phoenix [Phoenix in 15 minuten of minder](http://phoenix.apache.org/Phoenix-in-15-minutes-or-less.html). Zie voor de grammatica Phoenix [Phoenix grammatica](http://phoenix.apache.org/language/index.html).

> [!NOTE]
> Zie voor de versie-informatie voor Phoenix in HDInsight, [wat is er nieuw in de Hadoop-clusterversies geleverd door HDInsight?](hdinsight-component-versioning.md).
>

> [!IMPORTANT]
> De stappen in dit document werkt alleen voor Windows gebaseerde HDInsight-clusters. HDInsight is alleen beschikbaar in Windows voor versies lager is dan HDInsight 3.4. Linux is het enige besturingssysteem dat wordt gebruikt in HDInsight-versie 3.4 of hoger. Zie [HDInsight retirement on Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement) (HDInsight buiten gebruik gestel voor Windows) voor meer informatie. Zie voor meer informatie over het gebruik van Phoenix op Linux gebaseerde HDInsight [gebruik Apache Phoenix met HBase op basis van Linux-clusters in HDInsight](hdinsight-hbase-phoenix-squirrel-linux.md).
>



## <a name="use-sqlline"></a>Gebruik SQLLine
[SQLLine](http://sqlline.sourceforge.net/) is een opdrachtregelprogramma SQL uitvoeren.

### <a name="prerequisites"></a>Vereisten
Voordat u SQLLine gebruiken kunt, moet u het volgende hebben:

* **Een HBase-cluster in HDInsight**. Voor informatie over het inrichten van HBase-cluster, Zie [aan de slag met Apache HBase in HDInsight][hdinsight-hbase-get-started].
* **Verbinding maken met de HBase-cluster via remote desktop protocol**. Zie voor instructies [beheren Hadoop-clusters in HDInsight met behulp van de klassieke Azure Portal][hdinsight-manage-portal].

**Om erachter te komen de hostnaam**

1. Open **Hadoop-opdrachtregel** vanaf het bureaublad.
2. Voer de volgende opdracht om op te halen van het DNS-achtervoegsel:

        ipconfig

    Noteer **verbindingsspecifieke DNS-achtervoegsel**. Bijvoorbeeld: *myhbasecluster.f5.internal.cloudapp.net*. Wanneer u verbinding met een HBase-cluster maakt, moet u verbinding maken met een van de Zookeepers met FQDN-naam. Elke HDInsight-cluster heeft 3 Zookeepers. Ze zijn *zookeeper0*, *zookeeper1*, en *zookeeper2*. De FQDN-naam is ongeveer *zookeeper2.myhbasecluster.f5.internal.cloudapp.net*.

**SQLLine gebruiken**

1. Open **Hadoop-opdrachtregel** vanaf het bureaublad.
2. Voer de volgende opdrachten SQLLine openen:

        cd %phoenix_home%\bin
        sqlline.py [The FQDN of one of the Zookeepers]

    ![Phoenix sqlline van HDInsight hbase][hdinsight-hbase-phoenix-sqlline]

    De opdrachten in de steekproef:

        CREATE TABLE Company (COMPANY_ID INTEGER PRIMARY KEY, NAME VARCHAR(225));

        !tables

        UPSERT INTO Company VALUES(1, 'Microsoft');

        SELECT * FROM Company;

Zie voor meer informatie [SQLLine handmatige](http://sqlline.sourceforge.net/#manual) en [Phoenix grammatica](http://phoenix.apache.org/language/index.html).

## <a name="use-squirrel"></a>SQuirreL gebruiken
[SQuirreL SQL Client](http://squirrel-sql.sourceforge.net/) is een grafische Java-programma waarmee u kunt de structuur van een JDBC compatibele database weergeven, de gegevens in tabellen bladeren, voert u SQL-opdrachten enzovoort. Het kan worden gebruikt om met Apache Phoenix op HDInsight.

Deze sectie leest u hoe installeren en configureren van SQuirreL op uw werkstation verbinding maken met een HBase-cluster in HDInsight via VPN.

### <a name="prerequisites"></a>Vereisten
Voordat u de procedures uitvoert, moet u het volgende hebben:

* Een HBase-cluster is geïmplementeerd op een virtuele Azure-netwerk met een DNS-virtuele machine.  Zie voor instructies [maken HBase-clusters op Azure Virtual Network][hdinsight-hbase-provision-vnet].

* Haal het HBase-cluster cluster verbindingsspecifiek DNS-achtervoegsel. Om toegang te krijgen, RDP in het cluster, en voer vervolgens IPConfig.  Het DNS-achtervoegsel is vergelijkbaar met:

        myhbase.b7.internal.cloudapp.net
* Download en installeer [Microsoft Visual Studio Express voor Windows Desktop](https://www.visualstudio.com/products/visual-studio-express-vs.aspx) op uw werkstation. U moet makecert van het pakket om het certificaat te maken.  
* Download en installeer [Java Runtime Environment](http://www.oracle.com/technetwork/java/javase/downloads/jre7-downloads-1880261.html) op uw werkstation.  SQuirreL SQL clientversie 3.0 en hoger vereist JRE 1.6 of hoger.  

### <a name="configure-a-point-to-site-vpn-connection-to-the-azure-virtual-network"></a>Een punt-naar-Site VPN-verbinding met het Azure-netwerk configureren
Er zijn 3 stappen voor het configureren van een punt-naar-site VPN-verbinding:

1. [Een virtueel netwerk en een gateway voor dynamische routering configureren](#Configure-a-virtual-network-and-a-dynamic-routing-gateway)
2. [Uw certificaten maken](#Create-your-certificates)
3. [Uw VPN-client configureren](#Configure-your-VPN-client)

Zie [punt-naar-Site VPN-verbinding geconfigureerd met een Azure Virtual Network](../vpn-gateway/vpn-gateway-point-to-site-create.md) voor meer informatie.

#### <a name="configure-a-virtual-network-and-a-dynamic-routing-gateway"></a>Een virtueel netwerk en een gateway voor dynamische routering configureren
U hebt een HBase-cluster in virtueel netwerk van Azure ingericht zorgen (Zie de vereisten voor deze sectie). De volgende stap is het configureren van een punt-naar-site-verbinding.

**Voor het configureren van de punt-naar-site-connectiviteit**

1. Aanmelden bij de [klassieke Azure-Portal][azure-portal].
2. Klik aan de linkerkant op **netwerken**.
3. Klik op het virtuele netwerk dat u hebt gemaakt (Zie [inrichten HBase-clusters op Azure Virtual Network][hdinsight-hbase-provision-vnet]).
4. Klik op **configureren** vanaf de bovenkant.
5. In de **punt-naar-site-connectiviteit** sectie **punt-naar-site-connectiviteit configureren**.
6. Configureer **IP-BEGINADRES** en **CIDR** om op te geven van het IP-adresbereik waarvan uw VPN-clients een IP-adres wanneer verbinding wordt ontvangen. Het bereik kan niet overlappen met een van de bereiken die zich op uw on-premises netwerk en het Azure-netwerk dat u verbinding met. Bijvoorbeeld. Als u 10.0.0.0/20 voor het virtuele netwerk hebt geselecteerd, kunt u 10.1.0.0/24 voor de adresruimte van de client. Zie de [punt-naar-Site-connectiviteit] [ vnet-point-to-site-connectivity] pagina voor meer informatie.
7. Klik in de sectie virtueel netwerk adres spaties **gatewaysubnet toevoegen**.
8. Klik op **opslaan** aan de onderkant van de pagina.
9. Klik op **Ja** de wijziging te bevestigen. Wacht totdat de wijziging aan te brengen voordat u met de volgende procedure doorgaat van het systeem is voltooid.

**Een gateway voor dynamische routering maken**

1. Klik in de klassieke Azure-Portal op **DASHBOARD** vanaf de bovenkant van de pagina.
2. Klik op **GATEWAY maken** van de onderkant van de pagina.
3. Klik op **Ja** om te bevestigen. Wacht totdat de gateway is aangemaakt.
4. Klik op **DASHBOARD** vanaf de bovenkant.  Hier ziet u een diagram van het virtuele netwerk:

    ![Virtuele Azure-netwerk punt-naar-site-diagram][img-vnet-diagram]

    Het diagram toont 0 clientverbindingen. Nadat u een verbinding met het virtuele netwerk, wordt het aantal bijgewerkt naar een.

#### <a name="create-your-certificates"></a>Uw certificaten maken
Een manier voor het maken van een X.509-certificaat is door het hulpprogramma voor het certificaat maken (makecert.exe) die wordt geleverd met [Microsoft Visual Studio Express voor Windows Desktop](https://www.visualstudio.com/products/visual-studio-express-vs.aspx).

**Maken van een zelfondertekend basiscertificaat**

1. Open een opdrachtpromptvenster vanuit uw werkstation.
2. Navigeer naar de map Visual Studio-hulpprogramma's.
3. De volgende opdracht in het volgende voorbeeld maakt en installeer een basiscertificaat in het persoonlijke certificaatarchief op uw werkstation en ook maken een cer-bestand dat u later naar de klassieke Azure-Portal gaat uploaden.

        makecert -sky exchange -r -n "CN=HBaseVnetVPNRootCertificate" -pe -a sha1 -len 2048 -ss My "C:\Users\JohnDole\Desktop\HBaseVNetVPNRootCertificate.cer"

    Ga naar de map die u wilt dat het cer-bestand zich bevinden in, waarbij HBaseVnetVPNRootCertificate is de naam die u wilt gebruiken voor het certificaat.

    Sluit de opdrachtprompt niet.  U moet deze in de volgende procedure.

   > [!NOTE]
   > Omdat u een basiscertificaat hebt gemaakt op basis waarvan clientcertificaten worden gegenereerd, wilt u dit certificaat misschien samen met de persoonlijke sleutel exporteren en opslaan op een veilige locatie vanaf waar deze kunnen worden hersteld.
   >
   >

**Een certificaat maken**

* Vanaf de dezelfde opdrachtprompt (er moet op dezelfde computer waarop u het basiscertificaat hebt gemaakt. Het clientcertificaat moet worden gegenereerd vanuit het basiscertificaat), voer de volgende opdracht:

          makecert.exe -n "CN=HBaseVnetVPNClientCertificate" -pe -sky exchange -m 96 -ss My -in "HBaseVnetVPNRootCertificate" -is my -a sha1

    HBaseVnetVPNRootCertificate is de naam van het root-certificaat.  Deze moet overeenkomen met de naam van het root-certificaat.  

    Zowel het basiscertificaat als het clientcertificaat worden opgeslagen in uw persoonlijke certificaatarchief op uw computer. Gebruik certmgr.msc om te controleren.

    ![Virtuele Azure-netwerk punt-naar-site VPN-certificaat][img-certificate]

    U moet een clientcertificaat installeren op elke computer die u met het virtueel netwerk wilt verbinden. U wordt geadviseerd om unieke clientcertificaten te maken voor elke computer die u wilt verbinden met het virtuele netwerk. Gebruik voor het exporteren van de clientcertificaten certmgr.msc.

**Het basiscertificaat uploaden naar de klassieke Azure Portal**

1. Klik in de klassieke Azure-Portal op **netwerk** aan de linkerkant.
2. Klik op het virtuele netwerk waar uw HBase-cluster wordt geïmplementeerd op.
3. Klik op **certificaten** vanaf de bovenkant.
4. Klik op **uploaden** van de onderkant en u hebt gemaakt in de procedure vóór laatste bestand voor het basiscertificaat opgeven. Wacht totdat het certificaat hebt geïmporteerd.
5. Klik op **DASHBOARD** bovenaan.  Het virtuele diagram toont de status.

#### <a name="configure-your-vpn-client"></a>Uw VPN-client configureren
**Downloaden en installeren van het VPN-clientpakket**

1. Klik op de pagina DASHBOARD van het virtuele netwerk, in de sectie snelle weergave **de 64-bits VPN-clientpakket downloaden** of **de 32-bits VPN-clientpakket downloaden** op basis van uw werkstation OS Versie.
2. Klik op **uitvoeren** om het pakket te installeren.
3. Klik bij de beveiligingsprompt **meer info**, en klik vervolgens op **toch uitvoeren**.
4. Klik op **Ja** twee keer.

**Verbinding maken met VPN**

1. Klik op het pictogram netwerken op de taakbalk op het bureaublad van uw werkstation. U ziet een VPN-verbinding met de naam van uw virtuele netwerk.
2. Klik op de naam van de VPN-verbinding.
3. Klik op **Verbinden**.

**De VPN-verbinding en het domein naamomzetting te testen**

* Open een opdrachtprompt vanuit het werkstation en ping een van de volgende namen opgegeven van de HBase-cluster DNS-achtervoegsel is myhbase.b7.internal.cloudapp.net:

        zookeeper0.myhbase.b7.internal.cloudapp.net
        zookeeper0.myhbase.b7.internal.cloudapp.net
        zookeeper0.myhbase.b7.internal.cloudapp.net
        headnode0.myhbase.b7.internal.cloudapp.net
        headnode1.myhbase.b7.internal.cloudapp.net
        workernode0.myhbase.b7.internal.cloudapp.net

### <a name="install-and-configure-squirrel-on-your-workstation"></a>Installeren en configureren van SQuirreL op uw werkstation
**SQuirreL installeren**

1. Download het SQuirreL SQL client jar-bestand van [http://squirrel-sql.sourceforge.net/#installation](http://squirrel-sql.sourceforge.net/#installation).
2. Het jar-bestand openen/uitvoeren. Vereist de [Java Runtime Environment](http://www.oracle.com/technetwork/java/javase/downloads/jre7-downloads-1880261.html).
3. Klik op **volgende** twee keer.
4. Geef een pad op waar u de machtiging schrijven, en klik op **volgende**.

  > [!NOTE]
  > De standaardinstallatiemap is in de map C:\Program Files\squirrel-sql-3.6.  Om te schrijven naar dit pad, moet het installatieprogramma de administrator-bevoegdheden worden verleend. U kunt open een opdrachtprompt als beheerder, gaat u naar de map bin van Java en voer vervolgens:
  >
  >     Java.exe-jar [het pad naar het jar-bestand SQuirreL]
5. Klik op **OK** om te bevestigen dat het maken van de doelmap.
6. De standaardinstelling is de basis en standaard pakketten installeren.  Klik op **Volgende**.
7. Klik op **volgende** tweemaal, en klik vervolgens op **gedaan**.

**Het stuurprogramma Phoenix installeren**

Het phoenix stuurprogramma jar-bestand bevindt zich op de HBase-cluster. Het pad is vergelijkbaar met het volgende op basis van de versies:

    C:\apps\dist\phoenix-4.0.0.2.1.11.0-2316\phoenix-4.0.0.2.1.11.0-2316-client.jar
U moet dit te kopiëren naar uw werkstation onder de [SQuirreL-installatiemap] / lib pad.  De eenvoudigste manier is het RDP in het cluster, en gebruik vervolgens bestand kopiëren en plakken (CTRL + C en CTRL + V) om deze te kopiëren naar uw werkstation.

**Een Phoenix stuurprogramma wilt toevoegen aan SQuirreL**

1. Open SQuirreL SQL-Client op uw werkstation.
2. Klik op de **stuurprogramma** tabblad aan de linkerkant.
3. Van de **stuurprogramma's** menu, klikt u op **nieuw stuurprogramma**.
4. Voer de volgende informatie in:

   * **Naam**: Phoenix
   * **Voorbeeld-URL**: jdbc:phoenix:zookeeper2.contoso-hbase-eu.f5.internal.cloudapp.net
   * **Klassenaam**: org.apache.phoenix.jdbc.PhoenixDriver

     > [!WARNING]
     > Gebruiker alle kleine letters in de voorbeeld-URL. U kunt gebruiken ze volledig zookeeper quorum als een ervan is niet beschikbaar.  De hostnamen zijn zookeeper0, zookeeper1 en zookeeper2.
     >
     >

     ![HDInsight HBase Phoenix SQuirreL stuurprogramma][img-squirrel-driver]
5. Klik op **OK**.

**Een alias aan de HBase-cluster maken**

1. SQuirreL, klik op de **aliassen** tabblad aan de linkerkant.
2. Van de **aliassen** menu, klikt u op **Alias voor nieuwe**.
3. Voer de volgende informatie in:

   * **Naam**: de naam van de HBase-cluster of een willekeurige naam die u liever.
   * **Stuurprogramma**: Phoenix.  Dit moet overeenkomen met de naam van het stuurprogramma die u in de laatste procedure hebt gemaakt.
   * **URL**: de URL van de configuratie van uw stuurprogramma gekopieerd. Zorg ervoor dat aan gebruiker alle kleine letters.
   * **Gebruikersnaam**: deze tekst kan worden.  Omdat u hier VPN-verbinding gebruiken, wordt de gebruikersnaam niet helemaal gebruikt.
   * **Wachtwoord**: deze tekst kan worden.

     ![HDInsight HBase Phoenix SQuirreL stuurprogramma][img-squirrel-alias]
4. Klik op **Test**.
5. Klik op **Verbinden**. Wanneer de verbinding maakt, wordt de SQuirreL ziet:

    ![HBase Phoenix SQuirreL][img-squirrel]

**Een test uitvoeren**

1. Klik op de **SQL** tabblad rechts naast de **objecten** tabblad.
2. Kopieer en plak de volgende code:

        CREATE TABLE IF NOT EXISTS us_population (state CHAR(2) NOT NULL, city VARCHAR NOT NULL, population BIGINT  CONSTRAINT my_pk PRIMARY KEY (state, city))
3. Klik op uitvoeren.

    ![HBase Phoenix SQuirreL][img-squirrel-sql]
4. Ga terug naar de **objecten** tabblad.
5. Vouw de aliasnaam uit en vouw vervolgens **tabel**.  U ziet de nieuwe tabel vermeld in.

## <a name="next-steps"></a>Volgende stappen
In dit artikel hebt u geleerd hoe u van Apache Phoenix in HDInsight.  Zie voor meer informatie.

* [Overzicht van HDInsight HBase][hdinsight-hbase-overview]: HBase is een Apache, open-source NoSQL-database op basis van Hadoop. HBase biedt willekeurige toegang en een sterke consistentie voor grote hoeveelheden ongestructureerde en semigestructureerde gegevens.
* [HBase-clusters op Azure Virtual Network inrichten][hdinsight-hbase-provision-vnet]: door de integratie van virtueel netwerk, HBase-clusters kunnen worden geïmplementeerd op hetzelfde virtuele netwerk als uw toepassingen zodat toepassingen met HBase communiceren kunnen rechtstreeks.
* [Configureren van replicatie van HBase in HDInsight](hdinsight-hbase-replication.md): informatie over het configureren van HBase-replicatie tussen twee Azure-datacenters.


[azure-portal]: https://portal.azure.com
[vnet-point-to-site-connectivity]: https://msdn.microsoft.com/library/azure/09926218-92ab-4f43-aa99-83ab4d355555#BKMK_VNETPT

[hdinsight-versions]: hdinsight-component-versioning.md
[hdinsight-hbase-get-started]: hdinsight-hbase-tutorial-get-started.md
[hdinsight-manage-portal]: hdinsight-administer-use-management-portal.md#connect-to-clusters-using-rdp
[hdinsight-hbase-provision-vnet]: hdinsight-hbase-provision-vnet.md
[hdinsight-hbase-overview]: hdinsight-hbase-overview.md

[hdinsight-hbase-phoenix-sqlline]: ./media/hdinsight-hbase-phoenix-squirrel/hdinsight-hbase-phoenix-sqlline.png
[img-certificate]: ./media/hdinsight-hbase-phoenix-squirrel/hdinsight-hbase-vpn-certificate.png
[img-vnet-diagram]: ./media/hdinsight-hbase-phoenix-squirrel/hdinsight-hbase-vnet-point-to-site.png
[img-squirrel-driver]: ./media/hdinsight-hbase-phoenix-squirrel/hdinsight-hbase-squirrel-driver.png
[img-squirrel-alias]: ./media/hdinsight-hbase-phoenix-squirrel/hdinsight-hbase-squirrel-alias.png
[img-squirrel]: ./media/hdinsight-hbase-phoenix-squirrel/hdinsight-hbase-squirrel.png
[img-squirrel-sql]: ./media/hdinsight-hbase-phoenix-squirrel/hdinsight-hbase-squirrel-sql.png
