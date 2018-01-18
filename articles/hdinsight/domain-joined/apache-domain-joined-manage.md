---
title: Domein-HDInsight-clusters - Azure beheren | Microsoft Docs
description: Informatie over het beheren van domein HDInsight-clusters
services: hdinsight
documentationcenter: 
author: bprakash
manager: jhubbard
editor: cgronlun
tags: 
ms.assetid: 6ebc4d2f-2f6a-4e1e-ab6d-af4db6b4c87c
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 01/11/2018
ms.author: bhanupr
ms.openlocfilehash: 68166be98acc64326a4053b45f0039ae54d930e4
ms.sourcegitcommit: 7edfa9fbed0f9e274209cec6456bf4a689a4c1a6
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/17/2018
---
# <a name="manage-domain-joined-hdinsight-clusters"></a>Domein-HDInsight-clusters beheren
Informatie over de gebruikers en de rollen in HDInsight domein en het domein van de HDInsight-clusters beheren.

## <a name="access-the-clusters-with-enterprise-security-package"></a>Toegang tot de clusters met Enterprise-beveiligingspakket.

Enterprise-beveiligingspakket (voorheen bekend als HDInsight Premium) biedt meerdere gebruikers toegang tot het cluster, waarbij de verificatie wordt uitgevoerd door Active Directory en toestemming van Apache Zwerver en opslag ACL's (ADLS-ACL's). Autorisatie biedt veilige grenzen door meerdere gebruikers en kan alleen bevoegde gebruikers toegang hebben tot de gegevens op basis van het autorisatiebeleid.

Beveiligings- en gebruikersgegevens isolatie zijn belangrijk voor een HDInsight-cluster met Enterprise-beveiligingspakket. Om te voldoen aan deze vereisten, is SSH toegang tot het cluster met Enterprise-beveiligingspakket geblokkeerd. De volgende tabel bevat de aanbevolen toegangsmethoden voor elk clustertype:

|Workload|Scenario|Toegangsmethode|
|--------|--------|-------------|
|Hadoop|Hive – interactieve taken query 's |<ul><li>[Beeline](#beeline)</li><li>[Hive-weergave](../hadoop/apache-hadoop-use-hive-ambari-view.md)</li><li>[ODBC/JDBC – Power BI](../hadoop/apache-hadoop-connect-hive-power-bi.md)</li><li>[Visual Studio-hulpprogramma 's](../hadoop/apache-hadoop-visual-studio-tools-get-started.md)</li></ul>|
|Spark|Interactieve taken/query's PySpark interactieve|<ul><li>[Beeline](#beeline)</li><li>[Zeppelin met Livy](../spark/apache-spark-zeppelin-notebook.md)</li><li>[Hive-weergave](../hadoop/apache-hadoop-use-hive-ambari-view.md)</li><li>[ODBC/JDBC – Power BI](../hadoop/apache-hadoop-connect-hive-power-bi.md)</li><li>[Visual Studio-hulpprogramma 's](../hadoop/apache-hadoop-visual-studio-tools-get-started.md)</li></ul>|
|Spark|Batch-scenario's, Spark indienen, PySpark|<ul><li>[Livy](../spark/apache-spark-livy-rest-interface.md)</li></ul>|
|Interactieve Query (LLAP)|Interactief|<ul><li>[Beeline](#beeline)</li><li>[Hive-weergave](../hadoop/apache-hadoop-use-hive-ambari-view.md)</li><li>[ODBC/JDBC – Power BI](../hadoop/apache-hadoop-connect-hive-power-bi.md)</li><li>[Visual Studio-hulpprogramma 's](../hadoop/apache-hadoop-visual-studio-tools-get-started.md)</li></ul>|
|Alle|Aangepaste toepassing installeren|<ul><li>[Scriptacties](../hdinsight-hadoop-customize-cluster-linux.md)</li></ul>|


Met de standard-API's kunt u vanuit het oogpunt van beveiliging van. Bovendien profiteren van de volgende voordelen:

1.  **Management** – u kunt uw code te beheren en automatiseren van taken met behulp van standaard-API's – Livy, HS2 enzovoort.
2.  **Audit** – met SSH, er is geen manier om te controleren, welke gebruikers SSH moest het cluster. Dit zou het geval niet als taken via standaard eindpunten worden samengesteld als ze zouden worden uitgevoerd in de context van de gebruiker. 



### <a name="beeline"></a>Gebruik Beeline 
Beeline installeren op uw computer en verbinding maken via het openbare internet, voert u de volgende parameters: 

```
- Connection string: -u 'jdbc:hive2://&lt;clustername&gt;.azurehdinsight.net:443/;ssl=true;transportMode=http;httpPath=/hive2'
- Cluster login name: -n admin
- Cluster login password -p 'password'
```

Als u Beeline lokaal geïnstalleerd hebben en verbinding via een virtueel Azure-netwerk maken, gebruikt u de volgende parameters: 

```
- Connection string: -u 'jdbc:hive2://<headnode-FQDN>:10001/;transportMode=http'
```

Gebruik de volledig gekwalificeerde domeinnaam van een headnode vindt de informatie in de HDInsight beheren met behulp van de Ambari REST-API-document.














## <a name="users-of-domain-joined-hdinsight-clusters"></a>Gebruikers van domein HDInsight-clusters
Een HDInsight-cluster is niet domein heeft twee gebruikersaccounts die zijn gemaakt tijdens het maken van het cluster:

* **Ambari admin**: dit account wordt ook wel *Hadoop gebruiker* of *HTTP gebruiker*. Dit account kan worden gebruikt voor aanmelding bij Ambari op https://&lt;clustername >. azurehdinsight.net. Het kan ook worden gebruikt voor query's uitvoeren op de Ambari-weergaven, taken via externe hulpprogramma's (bijvoorbeeld PowerShell, Templeton, Visual Studio) uitvoeren en verifiëren met de Hive ODBC-stuurprogramma en de BI-tools (bijvoorbeeld, Excel, Power BI of Tableau).

Een domein HDInsight-cluster heeft drie nieuwe gebruikers naast Ambari Admin.

* **Zwerver admin**: dit account is het lokale beheerdersaccount van Apache Zwerver. Het is niet een gebruiker met active directory-domein. Dit account kan worden gebruikt beleidsregels instellen en zorgen dat andere gebruikers, beheerders of gedelegeerde beheerders (zodat die gebruikers u beleid beheren kunnen). De gebruikersnaam is standaard *admin* en het wachtwoord is hetzelfde als de Ambari Administrator-wachtwoord. Het wachtwoord kan worden bijgewerkt via de pagina instellingen in Zwerver.
* **Gebruiker met beheerdersrechten domein cluster**: dit account is een active directory-domeingebruiker aangewezen als de Hadoop-cluster beheerder zoals Ambari en Zwerver. Tijdens het maken van het cluster moet u de referenties van deze gebruiker opgeven. Deze gebruiker heeft de volgende bevoegdheden:

  * Computers toevoegen aan het domein en plaats deze in de organisatie-eenheid die u tijdens het maken van het cluster opgeeft.
  * Service-principals binnen de organisatie-eenheid die u tijdens het maken van een cluster opgeeft maken.
  * Omgekeerde DNS-vermeldingen te maken.

    Let op dat de AD-gebruikers hebben deze rechten.

    Er zijn een aantal eindpunten binnen het cluster (bijvoorbeeld Templeton) die niet worden beheerd door Zwerver en daarom zijn niet beveiligd. Deze eindpunten zijn vergrendeld voor alle gebruikers behalve de domeingebruiker van de cluster-beheerder.
* **Reguliere**: tijdens het maken van het cluster, kunt u meerdere active directory-groepen opgeven. De gebruikers in deze groepen worden gesynchroniseerd met Zwerver en Ambari. Deze gebruikers zijn domeingebruikers en hebben toegang tot alleen Zwerver beheerde eindpunten (bijvoorbeeld Hiveserver2). Alle het RBAC beleid en de controle zal van toepassing zijn op deze gebruikers.

## <a name="roles-of-domain-joined-hdinsight-clusters"></a>Rollen zijn van een domein HDInsight-clusters
HDInsight domein hebben de volgende rollen:

* Clusterbeheer
* Cluster-Operator
* Servicebeheerder
* Service-Operator
* Cluster-gebruiker

**Om te zien van de machtigingen van deze rollen**

1. Ambari Management UI te openen.  Zie [opent de gebruikersinterface voor het beheer van de Ambari](#open-the-ambari-management-ui).
2. Klik in het menu links op **rollen**.
3. Klik op de blauwe vraagteken om te zien welke machtigingen:

    ![HDInsight-rolmachtigingen domein](./media/apache-domain-joined-manage/hdinsight-domain-joined-roles-permissions.png)

## <a name="open-the-ambari-management-ui"></a>De gebruikersinterface voor het beheer van de Ambari openen

1. Meld u aan bij [Azure Portal](https://portal.azure.com).
2. Open uw HDInsight-cluster. Zie [lijst en geeft weer clusters](../hdinsight-administer-use-management-portal.md#list-and-show-clusters).
3. Klik op **Dashboard** in het menu bovenaan om Ambari te openen.
4. Meld u bij de Ambari met het cluster administrator domeingebruikersnaam en wachtwoord.
5. Klik op de **Admin** vervolgkeuzemenu in de rechterbovenhoek hoek naar rechts en klik vervolgens op **Ambari beheren**.

    ![HDInsight domein Ambari beheren](./media/apache-domain-joined-manage/hdinsight-domain-joined-manage-ambari.png)

    De gebruikersinterface ziet eruit als:

    ![Domein HDInsight Ambari de gebruikersinterface voor beheer](./media/apache-domain-joined-manage/hdinsight-domain-joined-ambari-management-ui.png)

## <a name="list-the-domain-users-synchronized-from-your-active-directory"></a>Lijst van domeingebruikers gesynchroniseerd vanuit uw Active Directory
1. Ambari Management UI te openen.  Zie [opent de gebruikersinterface voor het beheer van de Ambari](#open-the-ambari-management-ui).
2. Klik in het menu links op **gebruikers**. U ziet alle gebruikers gesynchroniseerd vanuit uw Active Directory met het HDInsight-cluster.

    ![Domein HDInsight Ambari management UI gebruikers weergeven](./media/apache-domain-joined-manage/hdinsight-domain-joined-ambari-management-ui-users.png)

## <a name="list-the-domain-groups-synchronized-from-your-active-directory"></a>De domein-groepen die gesynchroniseerd vanuit uw Active Directory
1. Ambari Management UI te openen.  Zie [opent de gebruikersinterface voor het beheer van de Ambari](#open-the-ambari-management-ui).
2. Klik in het menu links op **groepen**. U ziet de groepen die zijn gesynchroniseerd vanuit uw Active Directory met het HDInsight-cluster.

    ![Domein HDInsight Ambari-gebruikersinterface lijst beheergroepen](./media/apache-domain-joined-manage/hdinsight-domain-joined-ambari-management-ui-groups.png)

## <a name="configure-hive-views-permissions"></a>Hive-weergaven machtigingen configureren
1. Ambari Management UI te openen.  Zie [opent de gebruikersinterface voor het beheer van de Ambari](#open-the-ambari-management-ui).
2. Klik in het menu links op **weergaven**.
3. Klik op **HIVE** om de details weer te geven.

    ![Domein HDInsight Ambari management UI Hive-weergaven](./media/apache-domain-joined-manage/hdinsight-domain-joined-ambari-management-ui-hive-views.png)
4. Klik op de **Hive-weergave** koppelen aan het Hive-weergaven configureren.
5. Schuif omlaag naar de **machtigingen** sectie.

    ![Domein HDInsight Ambari management UI Hive-weergaven machtigingen configureren](./media/apache-domain-joined-manage/hdinsight-domain-joined-ambari-management-ui-hive-views-permissions.png)
6. Klik op **gebruiker toevoegen** of **groep toevoegen**, en geef vervolgens de gebruikers of groepen die Hive-weergaven kunnen gebruiken.

## <a name="configure-users-for-the-roles"></a>Gebruikers voor de rollen configureren
 Zie voor een lijst met functies en hun machtigingen [rollen van domein HDInsight-clusters](#roles-of-domain---joined-hdinsight-clusters).

1. Ambari Management UI te openen.  Zie [opent de gebruikersinterface voor het beheer van de Ambari](#open-the-ambari-management-ui).
2. Klik in het menu links op **rollen**.
3. Klik op **gebruiker toevoegen** of **groep toevoegen** gebruikers en groepen toewijzen aan andere rollen.

## <a name="next-steps"></a>Volgende stappen
* Zie [Configure Domain-joined HDInsight clusters](apache-domain-joined-configure.md) (Aan een domein gekoppelde HDInsight-clusters configureren) om een HDInsight-cluster te configureren dat is gekoppeld aan een domein.
* Zie [Configure Hive policies for Domain-joined HDInsight clusters](apache-domain-joined-run-hive.md) (Hive-beleid configureren voor aan een domein gekoppelde HDInsight-clusters) om Hive-beleid te configureren en Hive-query's uit te voeren.
