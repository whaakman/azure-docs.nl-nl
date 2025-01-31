---
title: Zelfstudie - beleid voor Apache HBase in HDInsight met Enterprise-beveiligingspakket - Azure configureren
description: 'Zelfstudie: informatie over het configureren van Apache Ranger-beleidsregels voor HBase in Azure HDInsight met Enterprise-beveiligingspakket.'
ms.service: hdinsight
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.topic: tutorial
ms.date: 06/18/2019
ms.openlocfilehash: 04592ba307cd696c20778d4a79f03be2eb0ac987
ms.sourcegitcommit: a52d48238d00161be5d1ed5d04132db4de43e076
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/20/2019
ms.locfileid: "67274392"
---
# <a name="tutorial-configure-apache-hbase-policies-in-hdinsight-with-enterprise-security-package"></a>Zelfstudie: Apache HBase-beleid configureren in HDInsight met Enterprise-beveiligingspakket

Leer hoe u Apache Ranger-beleidsregels configureert voor Apache HBase-clusters met ESP (Enterprise Security Package). ESP-clusters worden verbonden met een domein zodat gebruikers zich kunnen verifiëren met domeinreferenties. In deze zelfstudie maakt u twee Ranger-beleidsregels om de toegang tot verschillende kolomfamilies in een HBase-tabel te beperken.

In deze zelfstudie leert u het volgende:

> [!div class="checklist"]
> * Domeingebruikers maken
> * Ranger-beleidsregels maken
> * Tabellen maken in een HBase-cluster
> * Ranger-beleidsregels testen

## <a name="before-you-begin"></a>Voordat u begint

* Als u nog geen abonnement op Azure hebt, maakt u een [gratis account](https://azure.microsoft.com/free/) aan.

* Meld u aan bij [Azure Portal](https://portal.azure.com/).

* Maak een [HDInsight HBase-cluster met Enterprise Security Package](apache-domain-joined-configure-using-azure-adds.md).

## <a name="connect-to-apache-ranger-admin-ui"></a>Verbinding maken met de beheerinterface van Apache Ranger

1. Maak vanuit een browser verbinding met de beheerinterface van Ranger met behulp van de URL `https://<ClusterName>.azurehdinsight.net/Ranger/`. Vergeet niet om `<ClusterName>` te wijzigen in de naam van het HBase-cluster.

    > [!NOTE]  
    > Ranger-referenties zijn niet hetzelfde als referenties van een Hadoop-cluster. Gebruik een nieuw InPrivate-browservenster om verbinding te maken met de beheerinterface van Ranger om te voorkomen dat browsers Hadoop-referenties in de cache gebruiken.

2. Meld u aan met uw beheerdersreferenties voor Azure Active Directory (AD). Deze referenties zijn niet hetzelfde als de referenties voor het HDInsight-cluster of de SSH-referenties voor het Linux HDInsight knooppunt.

## <a name="create-domain-users"></a>Domeingebruikers maken

Ga naar [Een HDInsight-cluster maken met Enterprise Security Package](https://docs.microsoft.com/azure/hdinsight/domain-joined/apache-domain-joined-configure-using-azure-adds) voor informatie over het maken van de domeingebruikers **sales_user1** en **marketing_user1**. In een productiescenario zijn de domeingebruikers afkomstig uit uw Active Directory-tenant.

## <a name="create-hbase-tables-and-import-sample-data"></a>HBase-tabellen maken en voorbeeldgegevens importeren

U kunt SSH gebruiken om verbinding te maken met HBase-clusters en vervolgens [Apache HBase Shell](https://hbase.apache.org/0.94/book/shell.html) gebruiken om HBase-tabellen te maken, gegevens in te voegen, en gegevens te doorzoeken. Zie [SSH gebruiken met HDInsight](../hdinsight-hadoop-linux-use-ssh-unix.md) voor meer informatie.

### <a name="to-use-the-hbase-shell"></a>De HBase-shell gebruiken

1. Voer de volgende HBase-opdracht uit in SSH:
   
    ```bash
    hbase shell
    ```

2. Maak een HBase-tabel `Customers` met twee kolomfamilies: `Name` en `Contact`.

    ```hbaseshell   
    create 'Customers', 'Name', 'Contact'
    list
    ```
3. Voeg enkele gegeven in:
    
    ```hbaseshell   
    put 'Customers','1001','Name:First','Alice'
    put 'Customers','1001','Name:Last','Johnson'
    put 'Customers','1001','Contact:Phone','333-333-3333'
    put 'Customers','1001','Contact:Address','313 133rd Place'
    put 'Customers','1001','Contact:City','Redmond'
    put 'Customers','1001','Contact:State','WA'
    put 'Customers','1001','Contact:ZipCode','98052'
    put 'Customers','1002','Name:First','Robert'
    put 'Customers','1002','Name:Last','Stevens'
    put 'Customers','1002','Contact:Phone','777-777-7777'
    put 'Customers','1002','Contact:Address','717 177th Ave'
    put 'Customers','1002','Contact:City','Bellevue'
    put 'Customers','1002','Contact:State','WA'
    put 'Customers','1002','Contact:ZipCode','98008'
    ```
4. Bekijk de inhoud van de tabel:
    
    ```hbaseshell
    scan 'Contacts'
    ```
    ![HDInsight Hadoop HBase-shell](./media/apache-domain-joined-run-hbase/hbase-shell-scan-table.png)

## <a name="create-ranger-policies"></a>Ranger-beleidsregels maken

Maak een Ranger-beleid voor **sales_user1** en **marketing_user1**.

1. Open de **beheerinterface van Ranger**. Klik onder **HBase** op **\<ClusterName>_hbase**.

   ![Beheerinterface van Apache Ranger](./media/apache-domain-joined-run-hbase/apache-ranger-admin-login.png)

2. Op het scherm **List of Policies** worden alle Ranger-beleidsregels weergegeven die voor dit cluster zijn gemaakt. Er kan één vooraf geconfigureerd beleid worden weergegeven. Klik op **Nieuw beleid toevoegen**.

    ![De pagina Create Policy in de beheerinterface van Apache Ranger](./media/apache-domain-joined-run-hbase/apache-ranger-hbase-policies-list.png)

3. Voer in het scherm **Create Policy** de volgende waarden in:

   |**Instelling**  |**Voorgestelde waarde**  |
   |---------|---------|
   |Beleidsnaam  |  sales_customers_name_contact   |
   |HBase Table   |  Customers |
   |HBase Column-family   |  Name, Contact |
   |HBase Column   |  * |
   |Select Group  | |
   |Select User  | sales_user1 |
   |Machtigingen  | Lezen |

   U kunt de volgende jokertekens gebruiken in de onderwerpnaam:

   * Gebruik `*` om nul of meer tekens aan te geven.
   * Gebruik `?` om één teken aan te geven.

   ![De pagina Create Policy in de beheerinterface van Apache Ranger](./media/apache-domain-joined-run-hbase/apache-ranger-hbase-policy-create-sales.png)

   >[!NOTE]
   >Als er niet automatisch een domeingebruiker wordt ingevuld bij **Select User**, wacht u even totdat Ranger is gesynchroniseerd met AAD.

4. Klik op **Toevoegen** om het beleid op te slaan.

5. Klik op **Add New Policy** en voer de volgende waarden in:

   |**Instelling**  |**Voorgestelde waarde**  |
   |---------|---------|
   |Beleidsnaam  |  marketing_customers_contact   |
   |HBase Table   |  Customers |
   |HBase Column-family   |  Contactpersoon |
   |HBase Column   |  * |
   |Select Group  | |
   |Select User  | marketing_user1 |
   |Machtigingen  | Lezen |

   ![De pagina Create Policy in de beheerinterface van Apache Ranger](./media/apache-domain-joined-run-hbase/apache-ranger-hbase-policy-create-marketing.png)  

6. Klik op **Toevoegen** om het beleid op te slaan.

## <a name="test-the-ranger-policies"></a>Ranger-beleidsregels testen

**sales_user1** kan, op basis van de geconfigureerde Ranger-beleidsregels, alle gegevens bekijken in de kolommen van de kolomfamilies `Name` en `Contact`. De **marketing_user1** kan alleen gegevens bekijken in de kolomfamilie `Contact`.

### <a name="access-data-as-salesuser1"></a>Toegang tot gegevens als sales_user1

1. Open een nieuwe SSH-verbinding met het cluster. Gebruik de volgende opdracht om u aan te melden bij het cluster:

   ```bash
   ssh sshuser@CLUSTERNAME-ssh.azurehdinsight.net
   ```

1. Gebruiker de kinit-opdracht om de context van de gewenste gebruiker te wijzigen.

   ```bash
   kinit sales_user1
   ```

2. Open de HBase-shell en scannen van de tabel `Customers`.

   ```hbaseshell
   hbase shell
   scan `Customers`
   ```

3. U ziet dat de sales_user alle kolommen in de tabel `Customers` kan bekijken, inclusief de twee kolommen in de kolomfamilie `Name` en de vijf kolommen in de kolomfamilie `Contact`.

    ```hbaseshell
    ROW                                COLUMN+CELL
     1001                              column=Contact:Address, timestamp=1548894873820, value=313 133rd Place
     1001                              column=Contact:City, timestamp=1548895061523, value=Redmond
     1001                              column=Contact:Phone, timestamp=1548894871759, value=333-333-3333
     1001                              column=Contact:State, timestamp=1548895061613, value=WA
     1001                              column=Contact:ZipCode, timestamp=1548895063111, value=98052
     1001                              column=Name:First, timestamp=1548894871561, value=Alice
     1001                              column=Name:Last, timestamp=1548894871707, value=Johnson
     1002                              column=Contact:Address, timestamp=1548894899174, value=717 177th Ave
     1002                              column=Contact:City, timestamp=1548895103129, value=Bellevue
     1002                              column=Contact:Phone, timestamp=1548894897524, value=777-777-7777
     1002                              column=Contact:State, timestamp=1548895103231, value=WA
     1002                              column=Contact:ZipCode, timestamp=1548895104804, value=98008
     1002                              column=Name:First, timestamp=1548894897419, value=Robert
     1002                              column=Name:Last, timestamp=1548894897487, value=Stevens
    2 row(s) in 0.1000 seconds
    ```

### <a name="access-data-as-marketinguser1"></a>Toegang tot gegevens als marketing_user1

1. Open een nieuwe SSH-verbinding met het cluster. Gebruik de volgende opdracht om u aan te melden als **marketing_user1**:

   ```bash
   ssh sshuser@CLUSTERNAME-ssh.azurehdinsight.net
   ```

1. Gebruiker de kinit-opdracht om de context van de gewenste gebruiker te wijzigen

   ```bash
   kinit marketing_user1
   ```

2. Open de HBase-shell en scannen van de tabel `Customers`:

    ```hbaseshell
    hbase shell
    scan `Customers`
    ```

3. U ziet dat de marketing_user alleen de vijf kolommen van de kolomfamilie `Contact` kan bekijken.

    ```hbaseshell
    ROW                                COLUMN+CELL
     1001                              column=Contact:Address, timestamp=1548894873820, value=313 133rd Place
     1001                              column=Contact:City, timestamp=1548895061523, value=Redmond
     1001                              column=Contact:Phone, timestamp=1548894871759, value=333-333-3333
     1001                              column=Contact:State, timestamp=1548895061613, value=WA
     1001                              column=Contact:ZipCode, timestamp=1548895063111, value=98052
     1002                              column=Contact:Address, timestamp=1548894899174, value=717 177th Ave
     1002                              column=Contact:City, timestamp=1548895103129, value=Bellevue
     1002                              column=Contact:Phone, timestamp=1548894897524, value=777-777-7777
     1002                              column=Contact:State, timestamp=1548895103231, value=WA
     1002                              column=Contact:ZipCode, timestamp=1548895104804, value=98008
    2 row(s) in 0.0730 seconds
    ```

9. Bekijk de controlegebeurtenissen voor toegang vanuit de gebruikersinterface van Ranger.

   ![Controle van beleid vanuit de Ranger-gebruikersinterface](./media/apache-domain-joined-run-hbase/apache-ranger-admin-audit.png)

## <a name="clean-up-resources"></a>Resources opschonen

Als u deze toepassing verder niet meer gebruikt, verwijdert u het HBase-cluster dat u hebt gemaakt, via de volgende stappen:

1. Meld u aan bij [Azure Portal](https://portal.azure.com/).
2. Typ **HDInsight** in het **Zoekvak** bovenaan. 
1. Selecteer onder **Services** de optie **HDInsight-clusters**.
1. Klik in de lijst met HDInsight-clusters die wordt weergegeven, op de **...** naast het cluster dat u voor deze zelfstudie hebt gemaakt. 
1. Klik op **Verwijderen**. Klik op **Ja**.

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Aan de slag gaan met een Apache HBase](../hbase/apache-hbase-tutorial-get-started-linux.md)
