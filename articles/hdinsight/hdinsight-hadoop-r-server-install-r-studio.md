---
title: RStudio met R Server op HDInsight - Azure installeren | Microsoft Docs
description: Klik hier voor meer informatie over het installeren van RStudio met op HDInsight R Server.
services: hdinsight
documentationcenter: 
author: bradsev
manager: jhubbard
editor: cgronlun
ms.assetid: 918abb0d-8248-4bc5-98dc-089c0e007d49
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 06/19/2017
ms.author: bradsev
ms.openlocfilehash: 416420d855505508735ebd8526e93efdb230ad53
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2017
---
# <a name="installing-rstudio-with-r-server-on-hdinsight"></a>RStudio met op HDInsight R Server installeren

In dit artikel beschrijft het installeren van de community (gratis) versie van [RStudio Server](https://www.rstudio.com/products/rstudio-server/) op de edge-knooppunt van een cluster met een aangepast script. RStudio Server voorziet in een browser gebaseerde IDE voor gebruik door externe clients en wordt veel gebruikt op Linux. Er zijn meerdere geïntegreerde ontwikkelomgevingen (IDE) beschikbaar voor R vandaag, met inbegrip van:

- Microsoft [R-Tools voor Visual Studio](https://www.visualstudio.com/en-us/features/rtvs-vs.aspx) (RTVS) 
- [RStudio Server](https://www.rstudio.com/products/rstudio-server/) 
- Walware de Eclipse gebaseerde [StatET](http://www.walware.de/goto/statet).

Het voordeel van RStudio Server installeren op de edge-knooppunt van een HDInsight-cluster is dat u een volledige IDE-ervaring voor de ontwikkeling en uitvoering van scripts R met R Server op het cluster. Deze configuratie kunt productiever aanzienlijk dan standaard gebruik van de R-Console.

> [!NOTE]
> De procedure in dit artikel is alleen relevant als u geen RStudio Server community editie installeren tijdens het inrichten van uw cluster hebt geselecteerd. Als u dit hebt toegevoegd tijdens het inrichten en om deze te openen die u vervolgens op de **R Server Dashboards** tegel in de Azure portal-vermelding voor uw cluster, klik op de **R Studio Server** tegel. 

Als u liever de commercieel gelicentieerde Pro-versie van RStudio Server te gebruiken, moet u de installatie-instructies uit volgen [RStudio Server](https://www.rstudio.com/products/rstudio/download-server/).

> [!NOTE]
> Als u een HDInsight-cluster waarvoor R is geïnstalleerd met de [R scriptactie installeren](hdinsight-hadoop-r-scripts-linux.md), de stappen in dit document werkt niet correct ze nodig hebben een R Server op het HDInsight-cluster.
>
> 

## <a name="prerequisites"></a>Vereisten

* Een Azure HDInsight-cluster met R Server geïnstalleerd. Zie voor instructies [aan de slag met R Server op HDInsight-clusters](hdinsight-hadoop-r-server-get-started.md).
* Een SSH-client. Voor Linux en Unix-distributies of Macintosh OS X de `ssh` opdracht is opgegeven met het besturingssysteem. Voor Windows, raden wij aan [Cygwin](http://www.redhat.com/services/custom/cygwin/) met de [OpenSSH-optie](https://www.youtube.com/watch?v=CwYSvvGaiWU), of [PuTTY](http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html).  

## <a name="install-rstudio-on-the-cluster-using-a-custom-script"></a>RStudio installeren op het cluster met een aangepast script

Dit zijn de stappen:

1. Identificeer het edge-knooppunt van het cluster. Voor een HDInsight-cluster met R Server volgt de naamconventie voor het hoofdknooppunt en edge-knooppunt.
   * Hoofdknooppunt`CLUSTERNAME-ssh.azurehdinsight.net`
   * Edge-knooppunt`CLUSTERNAME-ed-ssh.azurehdinsight.net` 

2. Voeg SSH toe aan het edge-knooppunt van het cluster met behulp van het naamgevingspatroon vindt u in stap 1. Zie [SSH gebruiken met HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md) voor meer informatie.

3. Nadat u verbonden bent, worden een Hoofdgebruiker op het cluster. In de SSH-sessie, gebruikt u de volgende opdracht:

        sudo su -

4. De aangepaste scripts voor het installeren van RStudio downloaden. Gebruik de volgende opdracht:

        wget http://mrsactionscripts.blob.core.windows.net/rstudio-server-community-v01/InstallRStudio.sh

5. Wijzig de machtigingen voor het bestand aangepast script en voer het script. Gebruik de volgende opdrachten:

        chmod 755 InstallRStudio.sh
        ./InstallRStudio.sh

6. Als u een SSH-wachtwoord tijdens het maken van een HDInsight-cluster met R Server gebruikt, kunt u deze stap overslaan en doorgaan naar de volgende. Als u een SSH-sleutel in plaats daarvan gebruikt om de cluster te maken, moet u een wachtwoord instellen voor uw SSH-gebruiker. U hebt dit wachtwoord nodig bij het verbinden met RStudio. Voer de volgende opdrachten uit:

        passwd USERNAME
        Current Kerberos password:
        New password:
        Retype new password:
        Current Kerberos password:


7. Als u wordt gevraagd **huidige Kerberos-wachtwoord**, drukt u op **ENTER**.  Let op: u moet vervangen `USERNAME` met een SSH-gebruiker voor uw HDInsight-cluster. Als u uw wachtwoord is ingesteld, ziet u het volgende bericht:

        passwd: password updated successfully

    De SSH-sessie af te sluiten.

8. Maken van een SSH-tunnel naar het cluster via het toewijzen van `ssh -L localhost:8787:localhost:8787 USERNAME@CLUSTERNAME-ed-ssh.azurehdinsight.net` op het HDInsight-cluster naar de clientcomputer. Voordat u een nieuwe browsersessie opent, moet u een SSH-tunnel maken.

   * Op een Linux-client of een Windows-client met [Cygwin](http://www.redhat.com/services/custom/cygwin/), open een terminalsessie en gebruik de volgende opdracht:

             ssh -L localhost:8787:localhost:8787 USERNAME@CLUSTERNAME-ed-ssh.azurehdinsight.net

       Vervang **gebruikersnaam** met een SSH-gebruiker voor uw HDInsight-cluster en vervang **CLUSTERNAME** met de naam van uw HDInsight-cluster.
       U kunt ook een SSH-sleutel in plaats van een wachtwoord gebruiken door toe te voegen `-i id_rsa_key`.        
   * Als een Windows-client met PuTTY vervolgens

     1. PuTTY opent en voert u de verbindingsgegevens.
     2. In de **categorie** sectie aan de linkerkant van het dialoogvenster uit, vouw **verbinding**, vouw **SSH**, en selecteer vervolgens **Tunnels**.
     3. Geef de volgende informatie op de **opties voor SSH-poort doorsturen** vorm:

        * **Bronpoort**: de poort op de client die u wilt doorsturen. Bijvoorbeeld: **8787**.
        * **Bestemming** -de bestemming die moet worden toegewezen aan de lokale clientcomputer. Bijvoorbeeld: **localhost:8787**.

            ![Maken van een SSH-tunnel](./media/hdinsight-hadoop-r-server-install-r-studio/createsshtunnel.png "maken van een SSH-tunnel")

     4. Klik op **toevoegen** toevoegen van de instellingen en klik vervolgens op **openen** een SSH-verbinding openen.
     5. Wanneer u wordt gevraagd, moet u zich aanmelden bij de server voor het maken van een SSH-sessie de tunnel ingeschakeld.

9. Open een webbrowser en voer de volgende URL op basis van de poort die u hebt ingevoerd voor de tunnel:

        http://localhost:8787/ 

10. U wordt gevraagd naar de SSH-gebruikersnaam en wachtwoord opgeven om verbinding met het cluster. Als u een SSH-sleutel gebruikt bij het maken van het cluster, moet u het wachtwoord dat u in stap 5 hebt gemaakt.

    ![Verbinding maken met R Studio](./media/hdinsight-hadoop-r-server-install-r-studio/connecttostudio.png "maken van een SSH-tunnel")

11. Als u wilt testen of de RStudio-installatie voltooid is, kunt u een testscript dat wordt uitgevoerd op basis van R MapReduce en Spark taken op het cluster kunt uitvoeren. Het testscript in RStudio uit te voeren downloaden, gaat u terug naar de SSH-console en voer de volgende opdrachten:

    *    Als u een Hadoop-cluster met R gemaakt, gebruikt u deze opdracht:

            wget http://mrsactionscripts.blob.core.windows.net/rstudio-server-community-v01/testhdi.r
    *    Als u een Spark-cluster met R gemaakt, gebruikt u deze opdracht:

            wget http://mrsactionscripts.blob.core.windows.net/rstudio-server-community-v01/testhdi_spark.r

12. In RStudio ziet u het testscript die u hebt gedownload. Dubbelklik op het bestand te openen, selecteert u de inhoud van het bestand en klik vervolgens op **uitvoeren**. U ziet de uitvoer in de **Console** deelvenster:

   ![De installatie testen](./media/hdinsight-hadoop-r-server-install-r-studio/test-r-script.png "de installatie testen")

Een andere optie zou worden naar het type `source(testhdi.r)` of `source(testhdi_spark.r)` het script wilt uitvoeren.

## <a name="see-also"></a>Zie ook

* [COMPUTE context opties voor R Server op HDInsight-clusters](hdinsight-hadoop-r-server-compute-contexts.md)
* [Opties voor Azure-opslag voor R Server op HDInsight](hdinsight-hadoop-r-server-storage.md)

