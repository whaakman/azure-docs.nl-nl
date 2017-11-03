---
title: Gebruikersmachtigingen op bestanden en mappen niveaus - Azure HDInsight beheren | Microsoft Docs
description: Het beheren van bestands- en mapmachtigingen voor domein-HDInsight-clusters.
services: hdinsight
documentationcenter: 
tags: azure-portal
author: maxluk
manager: jhubbard
editor: cgronlun
ms.assetid: 
ms.service: hdinsight
ms.custom: hdinsightactive
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/26/2017
ms.author: maxluk
ms.openlocfilehash: 42d617ffeb8c2fee6be6d747b39d80b09774a1c3
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="manage-user-permissions-at-the-file-and-folder-levels"></a>Gebruikersmachtigingen op het niveau van bestanden en mappen beheren

[HDInsight-clusters domein](hdinsight-domain-joined-introduction.md) sterke verificatie gebruiken met Azure Active Directory (Azure AD) gebruikers, en ook *toegangsbeheer op basis van rollen* (RBAC)-beleidsregels voor verschillende services, zoals YARN en Hive. Als het standaardarchief van gegevens voor uw cluster Azure Storage of Windows Azure Storage-Blobs (WASB), kunt u ook mapniveau machtigingen voor bestanden en afdwingen. U kunt Apache Zwerver gebruiken voor toegang tot het cluster bestanden beheren voor uw gesynchroniseerde Azure AD-gebruikers en groepen.
<!-- [synchronized Azure AD users and groups](hdinsight-sync-aad-users-to-cluster.md). -->

Het exemplaar Apache Zwerver voor HDInsight-clusters domein is vooraf geconfigureerd met de service Zwerver WASB. De service Zwerver WASB is een beleid management-engine die vergelijkbaar is met HDFS-Zwerver, maar met een andere afdwinging van Zwerver toegangsbeleid. Als een inkomende aanvraag voor de resource een overeenkomende Zwerver-beleid, geen heeft is Standaardantwoord van de in de service Zwerver WASB weigeren. De service Zwerver voldoet niet aan machtiging om WASB te controleren.

## <a name="permission-and-policy-model"></a>Machtigingen en beleid-model

Resource-aanvragen worden geëvalueerd met behulp van de volgende stroom:

![Apache Zwerver beleid evaluatie stroom](./media/hdinsight-add-acls-at-file-folder-levels/ranger-policy-evaluation-flow.png)

Regels voor weigeren zijn eerst geëvalueerd, gevolgd door regels voor toestaan. Als er geen beleid wordt voldaan, wordt aan het einde van die overeenkomt met een weigeren geretourneerd.

### <a name="user-variable"></a>Variabele van de gebruiker

U kunt de `{USER}` variabele bij het toewijzen van beleid voor een gebruiker toegang tot een `/{username}` submap, bijvoorbeeld:

```
resource: path=/app-logs/{USER}, user: {USER}, recursive=true, permissions: all, delegateAdmin=true
```

Het bovenstaande beleid gebruikers krijgen toegang tot hun eigen submap onder de `/app-logs/` directory. Hier ziet u hoe dit beleid in de gebruikersinterface Zwerver uitziet:

![Voorbeeld van het gebruik van de variabele van de gebruiker](./media/hdinsight-add-acls-at-file-folder-levels/user-variable.png)

### <a name="policy-model-examples"></a>Beleid model voorbeelden

De volgende tabel bevat enkele voorbeelden van hoe het model beleid werkt:

| Zwerver beleid | Bestaande bestandssysteem | Aanvraag van gebruiker | Resultaat |
| -- | -- | -- | -- |
| /Data/financiën /, Berend schrijven | /Data | Bob, /data/finance/mydatafile.txt bestand maken | TOESTAAN - tussenliggende map 'Financiën' gemaakt, omdat bovenliggend element selectievakje |
| /Data/financiën /, Berend schrijven | /Data | Els, maak bestand /data/finance/mydatafile.txt | DENY - geen overeenkomend beleid |
| / data/financiën *, Berend schrijven | /Data | Bob, /data/finance/mydatafile.txt bestand maken | TOESTAAN - In dit geval het optionele recursieve beleid (`*`) is aanwezig; Zie [jokertekens](#wildcards) |
| /Data/Finance/mydatafile.txt, bob schrijven | /Data | Bob, /data/finance/mydatafile.txt bestand maken | DENY - bovenliggend element controle op ' / gegevens is mislukt omdat er geen beleid is |
| /Data/Finance/mydatafile.txt, bob schrijven | / data/Financiën | Bob, /data/finance/mydatafile.txt bestand maken | DENY - geen beleid voor de controle op '/ data/Financiën' bovenliggend element |

Machtigingen zijn vereist, op mapniveau of op bestandsniveau, op basis van het type bewerking. Een aanroep van ' read/open' vereist leestoegang op bestandsniveau, bijvoorbeeld tijdens een aanroep van 'maken' machtigingen op het niveau van de bovenliggende map vereist.

### <a name="wildcards-"></a>Jokertekens (*)

Wanneer een jokerteken (`*`) is aanwezig in het pad voor een beleid voor het jokerteken is van toepassing op dat pad en de volledige substructuur. Deze recursie is hetzelfde als wanneer u een `recurse-flag`. Het jokerteken wijst Zwerver-WASB recursie en gedeeltelijke naam die overeenkomt met.

## <a name="manage-file-and-folder-level-permissions-with-apache-ranger"></a>Bestands- en machtigingen met Apache Zwerver beheren

Als u dit nog niet hebt gedaan, volgt u [deze instructies](hdinsight-domain-joined-configure.md) voor het inrichten van een nieuw domein-cluster.

Zwerver WASB openen door te bladeren naar `https://<YOUR CLUSTER NAME>.azurehdinsight.net/ranger/`. Voer de gebruikersnaam voor de beheerder cluster en het wachtwoord die u hebt gedefinieerd bij het maken van uw cluster.

Na het aanmelden, ziet u het dashboard Zwerver:

![Zwerver dashboard](./media/hdinsight-add-acls-at-file-folder-levels/ranger-dashboard.png)

Huidige bestands- en mapmachtigingen voor uw cluster Azure Storage-account gekoppeld, klik op de  ***CLUSTERNAME*_wasb** koppeling in het besturingselement WASB.

![Zwerver dashboard](./media/hdinsight-add-acls-at-file-folder-levels/wasb-dashboard-link.png)

De huidige lijst van het beleid wordt weergegeven. Verschillende beleidsregels van typische zijn opgenomen als uitgangspunt nemen: Controleer de details van elk beleid om te zien van het gebruik van voorbeeld.

Voor elk beleid ziet u of het beleid is ingeschakeld, of logboekregistratie is geconfigureerd, en eventuele toegewezen groepen en gebruikers. Er zijn twee actieknoppen voor elk beleid: bewerken en verwijderen.

![Lijst met](./media/hdinsight-add-acls-at-file-folder-levels/policy-list.png)

### <a name="adding-a-new-policy"></a>Een nieuw beleid toevoegen

1. Op de bovenkant van de pagina van de beleidsregels WASB Selecteer **nieuw beleid toevoegen**.

    ![Nieuw beleid toevoegen](./media/hdinsight-add-acls-at-file-folder-levels/add-new.png)

2. Voer een beschrijvende **beleidsnaam**. Geef de Azure **Opslagaccount** voor uw cluster (*ACCOUNT_NAME*. blob.core.windows.net), en de **Storage-Account-Container** opgegeven tijdens het maken van uw cluster. Voer de **relatief pad** (ten opzichte van het cluster) voor de gebruikte map of bestand.

    ![Nieuw beleid formulier](./media/hdinsight-add-acls-at-file-folder-levels/new-policy.png)

3. Geef onder het formulier uw **voorwaarden toestaan** voor deze nieuwe bron. Selecteer de toepasselijke groepen en gebruikers en hun machtigingen instellen. In het volgende voorbeeld toestemming we alle gebruikers in de `sales` groep lees-/ schrijftoegang hebben.

    ![Verkoop toestaan](./media/hdinsight-add-acls-at-file-folder-levels/allow-sales.png)

4. Selecteer **Opslaan**.

### <a name="example-policy-conditions"></a>Voorbeeld beleidsvoorwaarden

De Apache-Zwerver [beleid evaluatie stroom](#permission-and-policy-model) kunt u opgeven dat een combinatie van toestaan en weigeren van voorwaarden om te voldoen aan uw behoeften. Enkele voorbeelden:

1. Alle verkoop-gebruikers, maar er is geen stagiaires toestaan:

    ![Toestaan dat de verkoop, stagiaires weigeren](./media/hdinsight-add-acls-at-file-folder-levels/allow-sales-deny-interns.png)

2. Alle verkoop-gebruikers toestaan en weigeren van alle stagiaires, met uitzondering van een intern met de naam 'hiveuser3' wie moet leestoegang hebben:

    ![Toestaan dat de verkoop, stagiaires, met uitzondering van hiveuser3 weigeren](./media/hdinsight-add-acls-at-file-folder-levels/allow-sales-deny-interns-except-hiveuser3.png)

## <a name="next-steps"></a>Volgende stappen

* [Op domein HDInsight Hive-beleid configureren](hdinsight-domain-joined-run-hive.md)
* [Domein-HDInsight-clusters beheren](hdinsight-domain-joined-manage.md)
* [Beheren van de Ambari - Ambari gebruikers autoriseren](hdinsight-authorize-users-to-ambari.md)

<!-- * [Synchronize Azure AD users and groups](hdinsight-sync-aad-users-to-cluster.md) -->

