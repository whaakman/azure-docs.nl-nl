---
title: Met Enterprise Security Enterprise - Azure HDInsight-clusters beheren
description: Informatie over het beheren van HDInsight-clusters met Enterprise-beveiligingspakket.
services: hdinsight
ms.service: hdinsight
author: omidm1
ms.author: omidm
ms.reviewer: mamccrea
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 08/24/2018
ms.openlocfilehash: b1fc52ac46a6cdb73b9210833e437765398781ef
ms.sourcegitcommit: 359b0b75470ca110d27d641433c197398ec1db38
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/07/2019
ms.locfileid: "55819124"
---
# <a name="manage-hdinsight-clusters-with-enterprise-security-package"></a>HDInsight-clusters met Enterprise-beveiligingspakket beheren
Meer informatie over de gebruikers en de rollen in HDInsight Enterprise Security Package (ESP) en het beheren van ESP-clusters.

## <a name="use-vscode-to-link-to-domain-joined-cluster"></a>Gebruik VSCode om een koppeling te maken met een cluster dat aan een domein is toegevoegd

U kunt koppelen van een normale cluster met behulp van Apache Ambari beheerd gebruikersnaam, een security Apache Hadoop-cluster ook een koppeling maken met domeingebruikersnaam (bijvoorbeeld: user1@contoso.com).
1. Open het opdrachtenpalet door te selecteren **CTRL + SHIFT + P**, en voer vervolgens **HDInsight: Koppeling van een cluster**.

   ![de opdracht cluster koppelen](./media/apache-domain-joined-manage/link-cluster-command.png)

2. Geef HDInsight cluster-URL-Username -> invoer > invoer wachtwoord -> Selecteer clustertype -> deze ziet geslaagd info als verificatie doorgegeven.
   
   ![dialoogvenster van de cluster koppelen](./media/apache-domain-joined-manage/link-cluster-process.png)

   > [!NOTE]  
   > De gekoppelde gebruikersnaam en wachtwoord worden gebruikt als het cluster zowel geregistreerd in Azure-abonnement en een cluster is gekoppeld. 
   
3. Ziet u een gekoppelde cluster met behulp van de opdracht **lijst cluster**. U kunt nu een script voor dit cluster gekoppelde indienen.

   ![gekoppelde cluster](./media/apache-domain-joined-manage/linked-cluster.png)

4. U kunt een cluster ook ontkoppelen door invoeren **HDInsight: Ontkoppelen van een cluster** van opdrachtenpalet.

## <a name="use-intellij-to-link-to-domain-joined-cluster"></a>Gebruik IntelliJ om een koppeling te maken met een cluster dat aan een domein is toegevoegd

U kunt een normale cluster koppelen met behulp van Ambari beheerd gebruikersnaam, ook een security hadoop-cluster koppelen met behulp van domeingebruikersnaam (bijvoorbeeld: user1@contoso.com). 
1. Klik op **koppelen van een cluster** van **Azure Explorer**.

   ![koppeling cluster contextmenu](./media/apache-domain-joined-manage/link-a-cluster-context-menu.png)

2. Voer **clusternaam**, **gebruikersnaam** en **wachtwoord**. U moet controleren van de gebruikersnaam en het wachtwoord als de verificatie is mislukt. (Optioneel) Storage-Account, opslagsleutel, toevoegen en selecteer vervolgens een container uit de Opslagcontainer. Opslaginformatie is bedoeld voor Opslagverkenner in het linkerdeelvenster
   
   ![dialoogvenster van de cluster koppelen](./media/apache-domain-joined-manage/link-a-cluster-dialog.png)

   > [!NOTE]  
   > We gebruiken de sleutel van een gekoppeld opslagaccount, gebruikersnaam en wachtwoord als het cluster zowel geregistreerd in Azure-abonnement en een cluster is gekoppeld.
   > ![Opslagverkenner in IntelliJ](./media/apache-domain-joined-manage/storage-explorer-in-IntelliJ.png)

   
3. Ziet u een cluster gekoppelde in **HDInsight** knooppunt als de ingevoerde gegevens correct zijn. U kunt nu een toepassing in dit cluster gekoppelde indienen.

   ![gekoppelde cluster](./media/apache-domain-joined-manage/linked-cluster-intellij.png)

4. U kunt een cluster op basis van ook ontkoppelen **Azure Explorer**.
   
   ![niet-gekoppelde cluster](./media/apache-domain-joined-manage/unlink.png)

## <a name="use-eclipse-to-link-to-domain-joined-cluster"></a>Gebruik Eclipse om een koppeling te maken met een cluster dat aan een domein is toegevoegd

U kunt een normale cluster koppelen met behulp van Ambari beheerd gebruikersnaam, ook een security hadoop-cluster koppelen met behulp van domeingebruikersnaam (bijvoorbeeld: user1@contoso.com).
1. Klik op **koppelen van een cluster** van **Azure Explorer**.

   ![koppeling cluster contextmenu](./media/apache-domain-joined-manage/link-a-cluster-context-menu.png)

2. Voer **clusternaam**, **gebruikersnaam** en **wachtwoord**, klik vervolgens op de knop OK om te koppelen van de cluster. (Optioneel) Geef Storage-Account, opslagsleutel en selecteer vervolgens de Opslagcontainer voor de storage explorer om te werken in de structuurweergave links
   
   ![dialoogvenster van de cluster koppelen](./media/apache-domain-joined-manage/link-cluster-dialog.png)
   
   > [!NOTE]  
   > We gebruiken de sleutel van een gekoppeld opslagaccount, gebruikersnaam en wachtwoord als het cluster zowel geregistreerd in Azure-abonnement en een cluster is gekoppeld.
   > ![Opslagverkenner in Eclipse](./media/apache-domain-joined-manage/storage-explorer-in-Eclipse.png)

3. Ziet u een cluster gekoppelde in **HDInsight** knooppunt na het klikken op de knop OK, als de ingevoerde gegevens correct zijn. U kunt nu een toepassing in dit cluster gekoppelde indienen.

   ![gekoppelde cluster](./media/apache-domain-joined-manage/linked-cluster-intellij.png)

4. U kunt een cluster op basis van ook ontkoppelen **Azure Explorer**.
   
   ![niet-gekoppelde cluster](./media/apache-domain-joined-manage/unlink.png)

## <a name="access-the-clusters-with-enterprise-security-package"></a>Toegang tot de clusters met Enterprise-beveiligingspakket.

Enterprise-beveiligingspakket (voorheen bekend als HDInsight Premium) biedt meerdere gebruikers toegang tot het cluster, waarbij de verificatie wordt uitgevoerd door Active Directory en autorisatie van Apache Ranger en opslag ACL's (ADLS-ACL's). Autorisatie biedt beveiligde grenzen tussen meerdere gebruikers en kan alleen bevoegde gebruikers toegang hebben tot de gegevens op basis van het autorisatiebeleid.

Beveiligings- en isolatie zijn belangrijk voor een HDInsight-cluster met Enterprise-beveiligingspakket. Om te voldoen aan deze vereisten, is SSH-toegang tot het cluster met Enterprise-beveiligingspakket geblokkeerd. De volgende tabel bevat de aanbevolen methoden voor elk clustertype:

|Workload|Scenario|Toegangsmethode|
|--------|--------|-------------|
|Apache Hadoop|Hive-interactieve taken/query 's  |<ul><li>[Beeline](#beeline)</li><li>[Hive-weergave](../hadoop/apache-hadoop-use-hive-ambari-view.md)</li><li>[ODBC/JDBC – Power BI](../hadoop/apache-hadoop-connect-hive-power-bi.md)</li><li>[Visual Studio Tools](../hadoop/apache-hadoop-visual-studio-tools-get-started.md)</li></ul>|
|Apache Spark|Interactieve taken/query's, interactieve PySpark|<ul><li>[Beeline](#beeline)</li><li>[Zeppelin met Livy](../spark/apache-spark-zeppelin-notebook.md)</li><li>[Hive-weergave](../hadoop/apache-hadoop-use-hive-ambari-view.md)</li><li>[ODBC/JDBC – Power BI](../hadoop/apache-hadoop-connect-hive-power-bi.md)</li><li>[Visual Studio Tools](../hadoop/apache-hadoop-visual-studio-tools-get-started.md)</li></ul>|
|Apache Spark|Batch-scenario's, Spark-submit, PySpark|<ul><li>[Livy](../spark/apache-spark-livy-rest-interface.md)</li></ul>|
|Interactive Query (LLAP)|Interactief|<ul><li>[Beeline](#beeline)</li><li>[Hive-weergave](../hadoop/apache-hadoop-use-hive-ambari-view.md)</li><li>[ODBC/JDBC – Power BI](../hadoop/apache-hadoop-connect-hive-power-bi.md)</li><li>[Visual Studio Tools](../hadoop/apache-hadoop-visual-studio-tools-get-started.md)</li></ul>|
|Alle|Aangepaste toepassing installeren|<ul><li>[Scriptacties](../hdinsight-hadoop-customize-cluster-linux.md)</li></ul>|

   > [!NOTE]  
   > Jupyter is niet geïnstalleerd/ondersteund in Enterprise-beveiligingspakket.

Met de standaard-API's kunt u vanuit het beveiligingsperspectief van. Bovendien krijgt u de volgende voordelen:

1.  **Management** , u kunt uw code te beheren en automatiseren van taken met behulp van standaard-API's, Livy, HS2 enzovoort.
2.  **Audit** – met SSH, is er geen manier om te controleren, welke gebruikers SSH had met het cluster. Wanneer taken via standaard-eindpunten zijn samengesteld, zoals ze worden uitgevoerd in de context van de gebruiker, zou dit het geval niet. 



### <a name="beeline"></a>Beeline gebruiken 
Beeline installeren op uw computer en verbinding maken via het openbare internet, gebruikt u de volgende parameters: 

```
- Connection string: -u 'jdbc:hive2://<clustername>.azurehdinsight.net:443/;ssl=true;transportMode=http;httpPath=/hive2'
- Cluster login name: -n admin
- Cluster login password -p 'password'
```

Als u Beeline lokaal is geïnstalleerd, en verbinding via een virtueel Azure-netwerk maken, gebruikt u de volgende parameters: 

```
- Connection string: -u 'jdbc:hive2://<headnode-FQDN>:10001/;transportMode=http'
```

Als u zoekt de volledig gekwalificeerde domeinnaam van een hoofdknooppunt, gebruik de informatie in de HDInsight beheren met behulp van de Ambari REST-API-document.














## <a name="users-of-hdinsight-clusters-with-esp"></a>Gebruikers van HDInsight-clusters met ESP
Een ESP HDInsight-cluster bevat twee gebruikersaccounts die zijn gemaakt tijdens het maken van een cluster:

* **Ambari-beheerder**: Dit account wordt ook wel bekend als *Hadoop-gebruiker* of *HTTP-gebruiker*. Dit account kan worden gebruikt voor aanmelding bij de Ambari op https://&lt;clustername >. azurehdinsight.net. Het kan ook worden gebruikt voor query's uitvoeren op de Ambari-weergaven, taken via externe hulpprogramma's (bijvoorbeeld, PowerShell, Templeton, Visual Studio) uitvoeren en verificatie met het Hive ODBC-stuurprogramma en BI-hulpprogramma's (bijvoorbeeld Excel, Power BI en Tableau).

Een HDInsight-cluster met ESP heeft drie nieuwe gebruikers naast Ambari Admin.

* **Ranger admin**:  Dit account is het lokale beheerdersaccount van Apache Ranger. Het is niet een active directory-domeingebruiker. Dit account kan worden gebruikt voor het instellen van beleid en andere gebruikers beheerders of gedelegeerde beheerders maken (zodat deze gebruikers beleidsregels beheren kunnen). De gebruikersnaam is standaard *admin* en het wachtwoord is hetzelfde als de Ambari-beheerderswachtwoord. Het wachtwoord kan worden bijgewerkt via de pagina instellingen in Ranger.
* **Gebruiker met beheerdersrechten domein cluster**: Dit account is een active directory-domeingebruiker aangemerkt als de beheerder van Hadoop-cluster zoals Ambari en Ranger. Tijdens het maken, moet u de referenties van deze gebruiker opgeven. Deze gebruiker heeft de volgende bevoegdheden:

  * Computers toevoegen aan het domein en plaats deze in de organisatie-eenheid die u tijdens het maken van clusters opgeeft.
  * Service-principals binnen de organisatie-eenheid die u tijdens het maken van een cluster opgeeft maken.
  * Omgekeerde DNS-vermeldingen te maken.

    Houd er rekening mee dat de andere AD-gebruikers hebben ook deze bevoegdheden.

    Er zijn enkele eindpunten binnen het cluster (bijvoorbeeld Templeton) die niet worden beheerd door Ranger, en daarom zijn niet beveiligd. Deze eindpunten zijn vergrendeld voor alle gebruikers, met uitzondering van het hulpprogramma voor het domein van de cluster-beheerder.
* **Reguliere**: Tijdens het maken van een cluster, kunt u meerdere active directory-groepen opgeven. De gebruikers in deze groepen worden gesynchroniseerd met Ranger en Ambari. Deze gebruikers zijn van gebruikers van een domein en toegang hebben tot alleen Ranger-beheerde eindpunten (bijvoorbeeld Hiveserver2). Alle RBAC-beleidsregels en -controle is van toepassing zijn op deze gebruikers.

## <a name="roles-of-hdinsight-clusters-with-esp"></a>Functies van HDInsight-clusters met ESP
HDInsight Enterprise-beveiligingspakket heeft de volgende rollen:

* Clusterbeheer
* Cluster-Operator
* Servicebeheerder
* Service-Operator
* Clustergebruiker

**Om te zien van de machtigingen van deze rollen**

1. Open de Ambari-gebruikersinterface voor beheer.  Zie [opent u de gebruikersinterface voor het beheer van de Ambari](#open-the-ambari-management-ui).
2. Klik in het menu links op **rollen**.
3. Klik op de blauwe vraagteken om te zien welke machtigingen:

    ![Rolmachtigingen ESP HDInsight](./media/apache-domain-joined-manage/hdinsight-domain-joined-roles-permissions.png)

## <a name="open-the-ambari-management-ui"></a>Open de Ambari-gebruikersinterface voor beheer

1. Meld u aan bij [Azure Portal](https://portal.azure.com).
2. Open uw HDInsight-cluster. Zie [clusters tonen en vermelden](../hdinsight-administer-use-management-portal.md#list-and-show-clusters).
3. Klik op **Dashboard** in het menu bovenaan om Ambari te openen.
4. Aanmelden bij de Ambari met behulp van de cluster-beheerder domein-gebruikersnaam en het wachtwoord.
5. Klik op de **Admin** in het vervolgkeuzemenu in de rechterbovenhoek hoek naar rechts en klik vervolgens op **Ambari beheren**.

    ![ESP HDInsight beheren Ambari](./media/apache-domain-joined-manage/hdinsight-domain-joined-manage-ambari.png)

    De gebruikersinterface ziet eruit zoals:

    ![Gebruikersinterface voor beheer van ESP HDInsight Ambari](./media/apache-domain-joined-manage/hdinsight-domain-joined-ambari-management-ui.png)

## <a name="list-the-domain-users-synchronized-from-your-active-directory"></a>Lijst van de domeingebruikers die is gesynchroniseerd vanuit uw Active Directory
1. Open de Ambari-gebruikersinterface voor beheer.  Zie [opent u de gebruikersinterface voor het beheer van de Ambari](#open-the-ambari-management-ui).
2. Klik in het menu links op **gebruikers**. U ziet alle gebruikers die zijn gesynchroniseerd vanuit uw Active Directory met het HDInsight-cluster.

    ![ESP HDInsight Ambari-gebruikersinterface lijst met gebruikers](./media/apache-domain-joined-manage/hdinsight-domain-joined-ambari-management-ui-users.png)

## <a name="list-the-domain-groups-synchronized-from-your-active-directory"></a>De domein-groepen die zijn gesynchroniseerd vanaf Active Directory
1. Open de Ambari-gebruikersinterface voor beheer.  Zie [opent u de gebruikersinterface voor het beheer van de Ambari](#open-the-ambari-management-ui).
2. Klik in het menu links op **groepen**. U ziet alle groepen die zijn gesynchroniseerd vanuit uw Active Directory met het HDInsight-cluster.

    ![ESP HDInsight Ambari-gebruikersinterface lijst met beheergroepen](./media/apache-domain-joined-manage/hdinsight-domain-joined-ambari-management-ui-groups.png)

## <a name="configure-hive-views-permissions"></a>Hive-weergaven machtigingen configureren
1. Open de Ambari-gebruikersinterface voor beheer.  Zie [opent u de gebruikersinterface voor het beheer van de Ambari](#open-the-ambari-management-ui).
2. Klik in het menu links op **weergaven**.
3. Klik op **HIVE** om de details weer te geven.

    ![ESP HDInsight Ambari management UI Hive-weergaven](./media/apache-domain-joined-manage/hdinsight-domain-joined-ambari-management-ui-hive-views.png)
4. Klik op de **Hive-weergave** koppelen aan het Hive-weergaven configureren.
5. Schuif omlaag naar de **machtigingen** sectie.

    ![ESP HDInsight Ambari management UI-Hive-weergaven configureren machtigingen](./media/apache-domain-joined-manage/hdinsight-domain-joined-ambari-management-ui-hive-views-permissions.png)
6. Klik op **gebruiker toevoegen** of **groep toevoegen**, en geef vervolgens de gebruikers of groepen die Hive-weergaven kunnen gebruiken.

## <a name="configure-users-for-the-roles"></a>Gebruikers voor de functies configureren
 Zie voor een overzicht van functies en hun machtigingen, rollen van HDInsight-clusters met ESP.

1. Open de Ambari-gebruikersinterface voor beheer.  Zie [opent u de gebruikersinterface voor het beheer van de Ambari](#open-the-ambari-management-ui).
2. Klik in het menu links op **rollen**.
3. Klik op **gebruiker toevoegen** of **groep toevoegen** gebruikers en groepen toewijzen aan verschillende rollen.

## <a name="next-steps"></a>Volgende stappen
* Zie voor een HDInsight-cluster configureren met Enterprise-beveiligingspakket, [configureren HDInsight-clusters met ESP](apache-domain-joined-configure.md).
* Zie voor het configureren van Hive-beleid en Hive-query's uitvoeren, [configureren Apache Hive-beleid voor HDInsight-clusters met ESP](apache-domain-joined-run-hive.md).
