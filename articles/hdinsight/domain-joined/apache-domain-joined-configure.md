---
title: Domein-HDInsight-clusters - Azure configureren | Microsoft Docs
description: Meer informatie over het instellen en configureren van domein HDInsight-clusters
services: hdinsight
documentationcenter: 
author: saurinsh
manager: cgronlun
editor: cgronlun
tags: 
ms.assetid: 0cbb49cc-0de1-4a1a-b658-99897caf827c
ms.service: hdinsight
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 01/24/2018
ms.author: saurinsh
ms.openlocfilehash: 6284b246c071fb99a8b47845aca34b6262e5b856
ms.sourcegitcommit: 79683e67911c3ab14bcae668f7551e57f3095425
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/25/2018
---
# <a name="configure-domain-joined-hdinsight-sandbox-environment"></a>HDInsight domein sandbox-omgeving configureren

Meer informatie over het instellen van een Azure HDInsight-cluster met zelfstandige Active Directory en [Apache Zwerver](http://hortonworks.com/apache/ranger/) om te profiteren van sterke authenticatie- en uitgebreide op rollen gebaseerde toegang beleid voor toegangsbeheer (RBAC). Zie voor meer informatie [introduceren domein HDInsight-clusters](apache-domain-joined-introduction.md).

Zonder HDInsight-cluster lid van een domein, kan alleen elk cluster hebben een Hadoop-HTTP-account voor gebruikers en een SSH-account.  De verificatie met meerdere gebruikers kan worden bereikt met:

-   Een zelfstandige Active Directory uitgevoerd op Azure IaaS.
-   Azure Active Directory.

Een zelfstandige Active Directory wordt uitgevoerd op Azure IaaS in dit artikel beschreven. Het is de eenvoudigste architectuur die van de klant volgen kunt om ondersteuning voor meerdere gebruikers op HDInsight. In dit artikel komen twee benaderingen voor deze configuratie:

- Optie 1: Wordt een Azure-resource management-sjabloon gebruiken om zowel de zelfstandige active directory en het HDInsight-cluster te maken.
- Optie 2: Het hele proces is onderverdeeld in de volgende stappen uit:
    - Maak een Active Directory met een sjabloon.
    - Het instellen van LDAPS.
    - AD-gebruikers en groepen maken
    - HDInsight-cluster maken

> [!IMPORTANT]
> Oozie is niet ingeschakeld op HDInsight domein.

## <a name="prerequisite"></a>Vereiste
* Azure-abonnement

## <a name="option-1-one-step-approach"></a>Optie 1: één stap benadering
In deze sectie kunt u een Azure-resource management-sjabloon openen vanuit de Azure-portal. De sjabloon wordt gebruikt voor het maken van een zelfstandige Active Directory en een HDInsight-cluster. U kunt momenteel domein Hadoop-cluster, Spark-cluster en interactieve Query cluster maken.

1. Klik op de volgende afbeelding om de sjabloon in Azure Portal te openen. De sjabloon bevindt zich in [Azure-snelstartsjablonen](https://azure.microsoft.com/resources/templates/).
   
    Een Spark-cluster maken:

    <a href="https://portal.azure.com/#create/Microsoft.Template/uri/http%3A%2F%2Fhditutorialdata.blob.core.windows.net%2Fdomain-joined%2Fspark%2Ftemplate.json" target="_blank"><img src="../hbase/media/apache-hbase-tutorial-get-started-linux/deploy-to-azure.png" alt="Deploy to Azure"></a>

    Een interactieve Query-cluster maken:

    <a href="https://portal.azure.com/#create/Microsoft.Template/uri/http%3A%2F%2Fhditutorialdata.blob.core.windows.net%2Fdomain-joined%2Finteractivequery%2Ftemplate.json" target="_blank"><img src="../hbase/media/apache-hbase-tutorial-get-started-linux/deploy-to-azure.png" alt="Deploy to Azure"></a>

    Maken van een Hadoop-cluster:

    <a href="https://portal.azure.com/#create/Microsoft.Template/uri/http%3A%2F%2Fhditutorialdata.blob.core.windows.net%2Fdomain-joined%2Fhadoop%2Ftemplate.json" target="_blank"><img src="../hbase/media/apache-hbase-tutorial-get-started-linux/deploy-to-azure.png" alt="Deploy to Azure"></a>

2. Voer de waarden in, selecteert u **ik ga akkoord met de voorwaarden en bepalingen hierboven**, selecteer **vastmaken aan dashboard**, en klik vervolgens op **aankoop**. Houd de muisaanwijzer over het teken uitleg naast de velden om te zien van de beschrijvingen. De meeste van de waarden zijn ingevuld. U kunt de standaardwaarden of uw eigen waarden.

    - **Resourcegroep**: Voer de naam van een Azure-resourcegroep.
    - **Locatie**: Kies een locatie die dicht bij u is.
    - **Naam van nieuw Opslagaccount**: Voer de naam van een Azure-Opslagaccount. Dit nieuwe opslagaccount wordt gebruikt door de PDC, de BDC en het HDInsight-cluster als het standaardopslagaccount.
    - **Admin Username**: Voer de gebruikersnaam van de domein-beheerder.
    - **Beheerderswachtwoord**: Voer het beheerderswachtwoord van het domein.
    - **Domeinnaam**: de standaardnaam is *contoso.com*.  Als u de domeinnaam wijzigt, moet u ook bijwerken de **Secure LDAP certificaat** veld en de **organisatie-eenheid DN** veld.
    - **DNS-voorvoegsel**: Voer het DNS-voorvoegsel voor het openbare IP-adres gebruikt door de Load Balancer.
    - **Clusternaam**: Voer de naam van het HDInsight-cluster.
    - **Type cluster**: deze waarde niet wijzigen. Als u wilt dat de clustertype wilt wijzigen, moet u de specifieke sjabloon gebruiken in de vorige stap.
    - **Beveiligde Ldap certificaatwachtwoord**: Gebruik de standaardwaarde, tenzij u het certificaatveld beveiligde LDAP wijzigt.
    Sommige van de waarden zijn vastgelegd in de sjabloon, bijvoorbeeld het aantal exemplaren van worker-knooppunt is twee.  De vastgelegde waarden, klikt u op **template bewerken**.

    ![Template van HDInsight-cluster domein bewerken](./media/apache-domain-joined-configure/hdinsight-domain-joined-edit-template.png)

Nadat de sjabloon is voltooid, zijn er 23 resources in de resourcegroep hebt gemaakt.

## <a name="option-2-multi-step-approach"></a>Optie 2: meerledige aanpak

Er zijn vier stappen in deze sectie:

1. Maak een Active Directory met een sjabloon.
2. Het instellen van LDAPS.
3. AD-gebruikers en groepen maken
4. HDInsight-cluster maken

### <a name="create-an-active-directory"></a>Maak een Active Directory

Azure Resource Manager-sjabloon maakt het gemakkelijker te maken van Azure-resources. In deze sectie maakt u een [Azure QuickStart-sjabloon](https://azure.microsoft.com/resources/templates/active-directory-new-domain-ha-2-dc/) maken van een nieuw forest en domein met twee virtuele machines. De twee virtuele machines die fungeert als een primaire domeincontroller en een back-domeincontroller.

**Maken van een domein met twee domeincontrollers**

1. Klik op de volgende afbeelding om de sjabloon in Azure Portal te openen.

    <a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Factive-directory-new-domain-ha-2-dc%2Fazuredeploy.json" target="_blank"><img src="./media/apache-domain-joined-configure/deploy-to-azure.png" alt="Deploy to Azure"></a>

    De sjabloon ziet eruit als:

    ![HDInsight-domein zijn toegevoegd forest domein virtuele machines maken](./media/apache-domain-joined-configure/hdinsight-domain-joined-create-arm-template.png)

2. Voer de volgende waarden in:

    - **Subscription**: selecteer een Azure-abonnement.
    - **De naam van resourcegroep**: Typ de naam van een resource-groep.  Een resourcegroep wordt gebruikt voor het beheren van uw Azure-resources die zijn gerelateerd aan een project.
    - **Locatie**: Selecteer een Azure-locatie dicht bij u is.
    - **Gebruikersnaam van de beheerder**: dit is de gebruikersnaam van de domein-beheerder. Deze gebruiker is niet het HTTP-gebruikersaccount van uw HDInsight-cluster. Dit is het account waarmee u in de zelfstudie.
    - **Beheerderswachtwoord**: Voer het wachtwoord voor de domeinbeheerder.
    - **Domeinnaam**: de domeinnaam moet een tweedelige naam. Bijvoorbeeld: contoso.com is, of contoso.local of hdinsight.test.
    - **DNS-voorvoegsel**: Typ een DNS-voorvoegsel
    - **PDC RDP-poort**: (de standaardwaarde voor deze zelfstudie gebruiken)
    - **De RDP-poort BDC**: (de standaardwaarde voor deze zelfstudie gebruiken)
    - **locatie van de artefacten**: (de standaardwaarde voor deze zelfstudie gebruiken)
    - **artefacten locatie SAS-token**: (laat het veld leeg voor deze zelfstudie.)

Het duurt ongeveer 20 minuten om de resources te maken.

### <a name="setup-ldaps"></a>Setup LDAPS

Lightweight Directory Access Protocol (LDAP) wordt gebruikt voor het lezen van en schrijven naar AD.

**Verbinding maken met de primaire domeincontroller met behulp van extern bureaublad**

1. Meld u aan bij [Azure Portal](https://portal.azure.com).
2. Open de resourcegroep en open vervolgens de virtuele machine van de primaire domeincontroller domeincontroller (PDC). De standaardnaam van de PDC is adPDC. 
3. Klik op **Connect** verbinding maken met de primaire domeincontroller met behulp van extern bureaublad.

    ![HDInsight-domein zijn toegevoegd verbinding maken met extern bureaublad PDC](./media/apache-domain-joined-configure/hdinsight-domain-joined-remote-desktop-pdc.png)


**Toevoegen van de Active Directory Certificate services**

4. Open **Serverbeheer** als dit niet is geopend.
5. Klik op **beheren**, en klik vervolgens op **functies en onderdelen toevoegen**.

    ![HDInsight-domein zijn toegevoegd, functies en onderdelen toevoegen](./media/apache-domain-joined-configure/hdinsight-domain-joined-add-roles.png)
5. 'Voordat u begint', klik op **volgende**.
6. Selecteer **op basis van functie of onderdeel gebaseerde installatie**, en klik vervolgens op **volgende**.
7. Selecteer de PDC en klik vervolgens op **volgende**.  De standaardnaam van de PDC is adPDC.
8. Selecteer **Active Directory certificaatservices**.
9. Klik op **onderdelen toevoegen** in het dialoogvenster pop-upvenster.
10. Volg de wizard, gebruikt u de standaardinstellingen voor de rest van de procedure.
11. Klik op **Sluiten** om de wizard te sluiten.

**AD-certificaat configureren**

1. In Serverbeheer klikt u op de gele meldingspictogram en klik vervolgens op **configureren Active Directory Certificate services**.

    ![HDInsight lid van domein AD-certificaat configureren](./media/apache-domain-joined-configure/hdinsight-domain-joined-configure-ad-certificate.png)

2. Klik op **functieservices** Selecteer aan de linkerkant **certificeringsinstantie**, en klik vervolgens op **volgende**.
3. Volg de wizard, gebruikt u de standaardinstellingen voor de rest van de procedure (Klik op **configureren** op de laatste stap).
4. Klik op **Sluiten** om de wizard te sluiten.

### <a name="optional-create-ad-users-and-groups"></a>(Optioneel) AD-gebruikers en groepen maken

**Maken van gebruikers en groepen in de AD**
1. Verbinding maken met de primaire domeincontroller met behulp van extern bureaublad
1. Open **Active Directory-gebruikers en Computers**.
2. Selecteer uw domeinnaam in het linkerdeelvenster.
3. Klik op de **Maak een nieuwe gebruiker in de huidige container** pictogram in het bovenste menu.

    ![Gebruikers die lid zijn van HDInsight-domein maken](./media/apache-domain-joined-configure/hdinsight-domain-joined-create-ad-user.png)
4. Volg de instructies voor het maken van een paar gebruikers. Hiveuser1 en hiveuser2.
5. Klik op de **maakt een nieuwe groep in de huidige container** pictogram in het bovenste menu.
6. Volg de instructies voor het maken van een groep genaamd **HDInsightUsers**.  Deze groep wordt gebruikt wanneer u een HDInsight-cluster verderop in deze zelfstudie maakt.

> [!IMPORTANT]
> U moet de PDC-virtuele machine opnieuw worden opgestart voordat u een domein HDInsight-cluster maakt.

### <a name="create-an-hdinsight-cluster-in-the-vnet"></a>Een HDInsight-cluster in het VNet maken

In deze sectie kunt u de Azure portal gebruiken voor het toevoegen van een HDInsight-cluster in het virtuele netwerk dat hebt gemaakt met behulp van de Resource Manager-sjabloon eerder in de zelfstudie. In dit artikel bevat alleen informatie over de specifieke informatie voor domein-clusterconfiguratie.  Voor algemene informatie, Zie [maken Linux gebaseerde clusters in HDInsight met behulp van de Azure-portal](../hdinsight-hadoop-create-linux-clusters-portal.md).  

**Een domein HDInsight-cluster maken**

1. Meld u aan bij [Azure Portal](https://portal.azure.com).
2. Open de resourcegroep die hebt gemaakt met behulp van de Resource Manager-sjabloon eerder in de zelfstudie.
3. Een HDInsight-cluster toevoegen aan de resourcegroep.
4. Selecteer **aangepaste** optie:

    ![HDInsight verbonden met het domein aangepast optie maken](./media/apache-domain-joined-configure/hdinsight-domain-joined-portal-custom-configuration-option.png)

    Er zijn zes secties met de optie aangepaste configuratie: basisbeginselen, opslag, toepassingen, Cluster grootte, geavanceerde instellingen en samenvatting.
5. In de **basisbeginselen** sectie:

    - Cluster-type: Selecteer **Enterprise-beveiligingspakket**. Op dit moment de Enterprise-beveiligingspakket kan alleen worden ingeschakeld voor de volgende clustertypen: Hadoop en interactieve Query Spark.

        ![HDInsight domein gekoppelde enterprise-beveiligingspakket](./media/apache-domain-joined-configure/hdinsight-creation-enterprise-security-package.png)
    - Cluster-aanmelding gebruikersnaam: dit is de Hadoop-HTTP-gebruiker. Dit account verschilt van het domein administrator-account.
    - Resourcegroep: Selecteer de resourcegroep die u eerder met de Resource Manager-sjabloon hebt gemaakt.
    - Locatie: De locatie moet niet dezelfde zijn als het account dat u hebt gebruikt toen u het vnet maken en de DC's met behulp van de Resource Manager-sjabloon.

6. In de **geavanceerde instellingen** sectie:

    - Instellingen van het domein:

        ![Geavanceerde instellingen domein HDInsight-domein](./media/apache-domain-joined-configure/hdinsight-domain-joined-portal-advanced-domain-settings.png)
        
        - Domeinnaam: Geef de domeinnaam die u gebruikt in [maken van een Active Directory](#create-an-active-directory).
        - Domeingebruikersnaam: Voer de gebruikersnaam van de AD-beheerder u gebruikt in [maken van een Active Directory](#create-an-active-directory).
        - Organisatie-eenheid: Zie de schermafbeelding voor een voorbeeld.
        - LDAPS URL: Zie de schermafbeelding voor een voorbeeld
        - Gebruikersgroep voor toegang: Voer de gebruikersnaam van de groep u hebt gemaakt in [maken AD-gebruikers en groepen](#optionally-createad-users-and-groups)
    - Virtueel netwerk: Selecteer het virtuele netwerk dat u hebt gemaakt in [maken van een Active Directory](#create-an-active-directory). De standaardnaam die wordt gebruikt in de sjabloon is **adVNET**.
    - Subnet: De standaardnaam die wordt gebruikt in de sjabloon is **adSubnet**.



Nadat u de zelfstudie hebt voltooid, kunt u het cluster verwijdert. Met HDInsight worden uw gegevens opgeslagen in Azure Storage zodat u een cluster veilig kunt verwijderen wanneer deze niet wordt gebruikt. Voor een HDInsight-cluster worden ook kosten in rekening gebracht, zelfs wanneer het niet wordt gebruikt. Aangezien de kosten voor het cluster vaak zoveel hoger zijn dan de kosten voor opslag, is het financieel gezien logischer clusters te verwijderen wanneer ze niet worden gebruikt. Zie voor de instructies van het verwijderen van een cluster [beheren Hadoop-clusters in HDInsight met behulp van de Azure-portal](../hdinsight-administer-use-management-portal.md#delete-clusters).

## <a name="next-steps"></a>Volgende stappen
* Zie [Configure Hive policies for Domain-joined HDInsight clusters](apache-domain-joined-run-hive.md) (Hive-beleid configureren voor aan een domein gekoppelde HDInsight-clusters) om Hive-beleid te configureren en Hive-query's uit te voeren.
* Zie voor het gebruik van SSH verbinding maken met domein HDInsight-clusters [SSH gebruiken met Hadoop op basis van Linux in HDInsight via Linux, Unix of OS X](../hdinsight-hadoop-linux-use-ssh-unix.md#domainjoined).

