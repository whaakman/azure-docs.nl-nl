---
title: Kafka-beleidsregels configureren in HDInsight met Enterprise Security Package - Azure
description: Leer hoe u Apache Ranger-beleidsregels voor Kafka configureert in Azure HDInsight met Enterprise Security Package.
services: hdinsight
ms.service: hdinsight
author: mamccrea
ms.author: mamccrea
ms.reviewer: mamccrea
ms.topic: tutorial
ms.date: 09/24/2018
ms.openlocfilehash: 48cfba6f62d75470efd27e3a4cdcb995e716798b
ms.sourcegitcommit: f0c2758fb8ccfaba76ce0b17833ca019a8a09d46
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/06/2018
ms.locfileid: "51037138"
---
# <a name="tutorial-configure-kafka-policies-in-hdinsight-with-enterprise-security-package-preview"></a>Zelfstudie: Kafka-beleidsregels configureren in HDInsight met Enterprise Security Package (preview)

Leer hoe u Apache Ranger-beleidsregels configureert voor Kafka-clusters met Enterprise Security Package (ESP). ESP-clusters worden verbonden met een domein zodat gebruikers zich kunnen verifiëren met domeinreferenties. In deze zelfstudie maakt u twee Ranger-beleidsregels om de toegang tot de onderwerpen `sales*` en `marketingspend` te beperken.

In deze zelfstudie leert u het volgende:

> [!div class="checklist"]
> * Domeingebruikers maken
> * Ranger-beleidsregels maken
> * Onderwerpen in een Kafka-cluster maken
> * Ranger-beleidsregels testen

## <a name="before-you-begin"></a>Voordat u begint

* Als u nog geen abonnement op Azure hebt, maakt u een [gratis account](https://azure.microsoft.com/free/) aan.

* Meld u aan bij [Azure Portal](https://portal.azure.com/).

* Maak een [HDInsight Kafka-cluster met Enterprise Security Package](apache-domain-joined-configure-using-azure-adds.md).

## <a name="connect-to-apache-ranger-admin-ui"></a>Verbinding maken met de beheerinterface van Apache Ranger

1. Maak vanuit een browser verbinding met de beheerinterface van Ranger met behulp van de URL `https://<ClusterName>.azurehdinsight.net/Ranger/`. Vergeet niet om `<ClusterName>` te wijzigen in de naam van uw Kafka-cluster.

    > [!NOTE] 
    > Ranger-referenties zijn niet hetzelfde als referenties van een Hadoop-cluster. Gebruik een nieuw InPrivate-browservenster om verbinding te maken met de beheerinterface van Ranger om te voorkomen dat browsers Hadoop-referenties in de cache gebruiken.

2. Meld u aan met uw beheerdersreferenties voor Azure Active Directory (AD). Deze referenties zijn niet hetzelfde als de referenties voor het HDInsight-cluster of de SSH-referenties voor het Linux HDInsight knooppunt.

   ![Beheerinterface van Apache Ranger](./media/apache-domain-joined-run-kafka/apache-ranger-admin-login.png)

## <a name="create-domain-users"></a>Domeingebruikers maken

Ga naar [Een HDInsight-cluster maken met Enterprise Security Package](https://docs.microsoft.com/azure/hdinsight/domain-joined/apache-domain-joined-configure-using-azure-adds#create-a-domain-joined-hdinsight-cluster) voor informatie over het maken van de domeingebruikers **sales_user** en **marketing_user**. In een productiescenario zijn de domeingebruikers afkomstig uit uw Active Directory-tenant.

## <a name="create-ranger-policy"></a>Ranger-beleid maken 

Maak een Ranger-beleid voor **sales_user** en **marketing_user**.

1. Open de **beheerinterface van Ranger**.

2. Klik op **\<clusternaam>_kafka** onder **Kafka**. Er kan één vooraf geconfigureerd beleid worden weergegeven.

3. Klik op **Add New Policy** en voer de volgende waarden in:

   |**Instelling**  |**Voorgestelde waarde**  |
   |---------|---------|
   |Policy Name  |  hdi sales* policy   |
   |Onderwerp   |  sales* |
   |Select User  |  sales_user1 |
   |Machtigingen  | Publish, Consume, Create |

   U kunt de volgende jokertekens gebruiken in de onderwerpnaam:

   * Gebruik '*' om nul of meer exemplaren van tekens aan te geven.
   * Gebruik '?' om één teken aan te geven.

   ![De pagina Create Policy in de beheerinterface van Apache Ranger](./media/apache-domain-joined-run-kafka/apache-ranger-admin-create-policy.png)   

   >[!NOTE] 
   >Als er niet automatisch een domeingebruiker wordt ingevuld bij **Select User**, wacht u even totdat Ranger is gesynchroniseerd met AAD.

4. Klik op **Toevoegen** om het beleid op te slaan.

5. Klik op **Add New Policy** en voer de volgende waarden in:

   |**Instelling**  |**Voorgestelde waarde**  |
   |---------|---------|
   |Policy Name  |  hdi marketing policy   |
   |Onderwerp   |  marketingspend |
   |Select User  |  marketing_user1 |
   |Machtigingen  | Publish, Consume, Create |

   ![De pagina Create Policy in de beheerinterface van Apache Ranger](./media/apache-domain-joined-run-kafka/apache-ranger-admin-create-policy-2.png)  

6. Klik op **Toevoegen** om het beleid op te slaan.

## <a name="create-topics-in-a-kafka-cluster-with-esp"></a>Onderwerpen maken in een Kafka-cluster met ESP

Ga als volgt te werk om de twee onderwerpen **salesevents** en **marketingspend** te maken:

1. Gebruik de volgende opdracht om een SSH-verbinding met het cluster op te zetten:

   ```bash
   ssh SSHUSER@CLUSTERNAME-ssh.azurehdinsight.net
   ```

   Vervang `SSHUSER` door de SSH-gebruiker voor uw cluster en `CLUSTERNAME` door de naam van het cluster. Voer het wachtwoord voor het SSH-gebruikersaccount in wanneer hierom wordt gevraagd. Zie [SSH gebruiken met HDInsight](https://docs.microsoft.com/azure/hdinsight/hdinsight-hadoop-linux-use-ssh-unix) voor meer informatie over het gebruik van `scp` met HDInsight.

   In een productiescenario kunnen domeingebruikers die zijn geconfigureerd tijdens het maken van het cluster met SSH worden overgebracht naar het cluster.

2. Gebruik de volgende opdrachten om de clusternaam op te slaan in een variabele en een hulpprogramma voor het parseren van JSON (`jq`) te installeren. Wanneer u daarom wordt gevraagd, geeft u de naam van het Kafka-cluster op.

   ```bash
   sudo apt -y install jq
   read -p 'Enter your Kafka cluster name:' CLUSTERNAME
   ```

3. Gebruik de volgende opdrachten om de Kafka-brokerhosts en de Zookeeper-hosts op te vragen. Geef desgevraagd het wachtwoord voor het beheerdersaccounts voor het cluster op.

   ```bash
   export KAFKAZKHOSTS=`curl -sS -u admin -G https://$CLUSTERNAME.azurehdinsight.net/api/v1/clusters/$CLUSTERNAME/services/ZOOKEEPER/components/ZOOKEEPER_SERVER | jq -r '["\(.host_components[].HostRoles.host_name):2181"] | join(",")' | cut -d',' -f1,2`; \
   export KAFKABROKERS=`curl -sS -u admin -G https://$CLUSTERNAME.azurehdinsight.net/api/v1/clusters/$CLUSTERNAME/services/KAFKA/components/KAFKA_BROKER | jq -r '["\(.host_components[].HostRoles.host_name):9092"] | join(",")' | cut -d',' -f1,2`; \
   ```
> [!Note]
> Voordat u doorgaat, moet u mogelijk uw ontwikkelomgeving instellen als u dit nog niet hebt gedaan. U hebt onderdelen zoals de Java JDK, Apache Maven en een SSH-client met scp nodig. Zie deze [installatie-instructies](https://github.com/Azure-Samples/hdinsight-kafka-java-get-started/tree/master/DomainJoined-Producer-Consumer) voor meer informatie.
1. Download de [voorbeelden voor aan een domein gekoppelde Apache Kafka voor producer/consumer](https://github.com/Azure-Samples/hdinsight-kafka-java-get-started/tree/master/DomainJoined-Producer-Consumer).

1. Volg stap 2 en 3 onder **Het voorbeeld compileren en implementeren** in [Zelfstudie: Werken met de Producer- en Consumer-API's van Apache Kafka](https://docs.microsoft.com/azure/hdinsight/kafka/apache-kafka-producer-consumer-api#build-and-deploy-the-example)

1. Voer de volgende opdrachten uit:

   ```bash
   java -jar -Djava.security.auth.login.config=/usr/hdp/current/kafka-broker/config/kafka_client_jaas.conf kafka-producer-consumer.jar create salesevents $KAFKABROKERS
   java -jar -Djava.security.auth.login.config=/usr/hdp/current/kafka-broker/config/kafka_client_jaas.conf kafka-producer-consumer.jar create marketingspend $KAFKABROKERS
   ```

   >[!NOTE] 
   >Alleen de proceseigenaar van de Kafka-service, zoals de root, kan schrijven naar de Zookeeper-znodes `/config/topics`. Ranger-beleidsregels worden niet afgedwongen wanneer een gebruiker zonder machtigingen een onderwerp maakt. Dit komt doordat het script `kafka-topics.sh` rechtstreeks met Zookeeper communiceert om het onderwerp te maken. Er worden vermeldingen toegevoegd aan de Zookeeper-nodes, terwijl de watchers van broker controleren op onderwerpen en deze indien nodig maken. De autorisatie is niet mogelijk via de Ranger-invoegtoepassing en de bovenstaande opdracht wordt met behulp van `sudo` uitgevoerd via de Kafka-broker.


## <a name="test-the-ranger-policies"></a>Ranger-beleidsregels testen

Op basis van de geconfigureerde Ranger-beleidsregels kan **sales_user** het onderwerp **salesevents** produceren/verbruiken, maar niet het onderwerp **marketingspend**. Omgekeerd kan **marketing_user** het onderwerp **marketingspend** produceren/verbruiken, maar niet het onderwerp **salesevents**.

1. Open een nieuwe SSH-verbinding met het cluster. Gebruik de volgende opdracht om u als **sales_user1** aan te melden:

   ```bash
   ssh sales_user1@CLUSTERNAME-ssh.azurehdinsight.net
   ```

2. Voer de volgende opdracht uit:

   ```bash
   export KAFKA_OPTS="-Djava.security.auth.login.config=/usr/hdp/current/kafka-broker/config/kafka_client_jaas.conf"
   ```

3. Gebruik de broker- en Zookeeper-namen uit de vorige sectie om de volgende omgevingsvariabelen in te stellen:

   ```bash
   export KAFKABROKERS=<brokerlist>:9092 
   ```

   Voorbeeld: `export KAFKABROKERS=wn0-khdicl.contoso.com:9092,wn1-khdicl.contoso.com:9092`

   ```bash
   export KAFKAZKHOSTS=<zklist>:2181
   ```

   Voorbeeld: `export KAFKAZKHOSTS=zk1-khdicl.contoso.com:2181,zk2-khdicl.contoso.com:2181`

4. Controleer of **sales_user1** onderwerpen van het type **salesevents** kan produceren.
   
   Voer de volgende opdracht uit om de console-producer voor het onderwerp **salesevents** te starten:

   ```bash
   /usr/hdp/current/kafka-broker/bin/kafka-console-producer.sh --broker-list $KAFKABROKERS --topic salesevents --security-protocol SASL_PLAINTEXT
   ```

   Voer vervolgens een paar berichten in op de console. Druk op **Ctrl + C** om de console-producer af te sluiten.

5. Voer de volgende opdracht om gebeurtenissen uit het onderwerp **salesevents** te verbruiken:

   ```bash
   /usr/hdp/current/kafka-broker/bin/kafka-console-consumer.sh --zookeeper $KAFKAZKHOSTS --topic salesevents --security-protocol PLAINTEXTSASL --from-beginning
   ```
 
6. Controleer of de berichten verschijnen die u in de vorige stap hebt ingevoerd. Controleer ook of **sales_user1** geen uitvoer kan produceren voor het onderwerp **marketingspend**.

   Gebruik hetzelfde ssh-venster als hierboven om met de volgende opdracht uitvoer te produceren voor het onderwerp **marketingspend**:

   ```bash
   /usr/hdp/current/kafka-broker/bin/kafka-console-producer.sh --broker-list $KAFKABROKERS --topic marketingspend --security-protocol SASL_PLAINTEXT
   ```

   Er treedt een autorisatiefout op, maar die kunt u negeren. 

7. U ziet dat **marketing_user1** geen gebeurtenissen van het onderwerp **salesevents** kan verbruiken.

   Herhaal stappen 1-3 hierboven, maar dit keer als **marketing_user1**.

   Voer de volgende opdracht om gebeurtenissen uit het onderwerp **salesevents** te verbruiken:

   ```bash
   /usr/hdp/current/kafka-broker/bin/kafka-console-consumer.sh --zookeeper $KAFKAZKHOSTS --topic marketingspend --security-protocol PLAINTEXTSASL --from-beginning
   ```

   Eerdere berichten kunnen niet worden weergegeven.

8. Bekijk de controlegebeurtenissen voor toegang vanuit de gebruikersinterface van Ranger.

   ![Controle van beleid vanuit de Ranger-gebruikersinterface](./media/apache-domain-joined-run-kafka/apache-ranger-admin-audit.png)

## <a name="next-steps"></a>Volgende stappen

* [Bring Your Own Key en Kafka](https://docs.microsoft.com/azure/hdinsight/kafka/apache-kafka-byok)
* [Een inleiding tot Hadoop-beveiliging met Enterprise Security Package](https://docs.microsoft.com/azure/hdinsight/domain-joined/apache-domain-joined-introduction)
