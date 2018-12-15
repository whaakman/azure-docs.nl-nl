---
title: On-premises Apache Hadoop-clusters migreren naar Azure HDInsight - beveiligings- en aanbevolen procedures voor DevOps
description: Meer informatie over beveiliging en DevOps-procedures voor migratie on-premises Hadoop-clusters op Azure HDInsight.
services: hdinsight
author: hrasheed-msft
ms.reviewer: ashishth
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 10/25/2018
ms.author: hrasheed
ms.openlocfilehash: 7cfb327a3eb6cbf2ae90c9d258a470797732acaa
ms.sourcegitcommit: c2e61b62f218830dd9076d9abc1bbcb42180b3a8
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/15/2018
ms.locfileid: "53437500"
---
# <a name="migrate-on-premises-apache-hadoop-clusters-to-azure-hdinsight---security-and-devops-best-practices"></a>On-premises Apache Hadoop-clusters migreren naar Azure HDInsight - beveiligings- en aanbevolen procedures voor DevOps

In dit artikel biedt aanbevelingen voor beveiliging en DevOps in Azure HDInsight-systemen. Het onderdeel van een serie die biedt best practices om u te helpen migreren on-premises Apache Hadoop-systemen tot Azure HDInsight.

## <a name="secure-and-govern-cluster-with-enterprise-security-package"></a>Beveiligen en beheren van de cluster met Enterprise-beveiligingspakket

Enterprise Security Package (ESP) biedt ondersteuning voor verificatie op basis van Active Directory, ondersteuning voor meerdere gebruikers en rollen gebaseerd toegangsbeheer. Met de gekozen ESP optie HDInsight-cluster is toegevoegd aan het Active Directory-domein en de enterprise-beheerder op rollen gebaseerd toegangsbeheer (RBAC) voor een Apache Hive-beveiliging met behulp van Apache Ranger kunt configureren. De beheerder kan ook de toegang tot de gegevens door werknemers en eventuele wijzigingen worden aangebracht aan het beleid voor toegangsbeheer controleren.

ESP is beschikbaar op de volgende clustertypen: Apache Hadoop, Apache Spark, Apache HBase, Apache Kafka en interactieve Query (Hive LLAP). 

Gebruik de volgende stappen uit om de aan domein gekoppelde HDInsight-cluster te implementeren:

- Azure Active Directory (AAD) implementeren door de naam van het domein.
- Azure Active Directory Domain Services (AAD DS) implementeren.
- De vereiste Virtueelnetwerk en subnet maken.
- Een virtuele machine in het Virtueelnetwerk voor het beheren van AAD DS implementeren.
- De virtuele machine toevoegen aan het domein.
- Installatie van AD en hulpprogramma's voor DNS.
- Laat de AAD-DS-beheerder maken van een organisatie-eenheid (OE).
- LDAPS voor AAD DS inschakelen.
- Maak een service-account in Azure Active Directory met gedelegeerde lezen en schrijven-beheerder toestemming voor de organisatie-eenheid, zodat deze kan. Deze serviceaccount kan vervolgens machines toevoegen aan het domein en plaats machine principals binnen de organisatie-eenheid. Het kan ook service-principals binnen de organisatie-eenheid die u tijdens het maken van een cluster opgeeft maken.


    > [!Note]
    > Het serviceaccount hoeft niet te worden beheerdersaccount van AD-domein.


- ESP HDInsight-cluster implementeren door in te stellen van de volgende parameters:
    - **Domeinnaam**: De domeinnaam die is gekoppeld aan Azure AD DS.
    - **Domeingebruikersnaam**: Het serviceaccount in de Azure AD DS-domeincontroller beheerd domein die u in de vorige sectie hebt gemaakt, bijvoorbeeld: `hdiadmin@contoso.onmicrosoft.com`. In dit de domeingebruiker is de beheerder van dit HDInsight-cluster.
    - **Domeinwachtwoord**: Het wachtwoord van het serviceaccount.
    - **Organisatie-eenheid**: De DN-naam van de organisatie-eenheid die u wilt gebruiken met het HDInsight-cluster, bijvoorbeeld: `OU=HDInsightOU,DC=contoso,DC=onmicrosoft,DC=com`. Als deze organisatie-eenheid niet bestaat, wordt het HDInsight-cluster probeert te maken van de organisatie-eenheid met behulp van de bevoegdheden van het serviceaccount.
    - **LDAPS-URL**: bijvoorbeeld `ldaps://contoso.onmicrosoft.com:636`.
    - **Gebruikersgroep openen**: De beveiligingsgroepen waarvan gebruikers die u wilt synchroniseren met het cluster, bijvoorbeeld: `HiveUsers`. Als u meerdere gebruikersgroepen opgeven wilt, gescheiden door puntkomma (;). De groep(en) moeten zich in de map voordat het ESP-cluster te maken.

Raadpleeg voor meer informatie de volgende artikelen:

- [Een inleiding tot Apache Hadoop-beveiliging met aan domein gekoppelde HDInsight-clusters](../domain-joined/apache-domain-joined-introduction.md)

- [Azure domein Apache Hadoop-clusters in HDInsight plannen](../domain-joined/apache-domain-joined-architecture.md)
- [Configureren van een domein gekoppeld HDInsight-cluster met behulp van Azure Active Directory Domain Services](../domain-joined/apache-domain-joined-configure-using-azure-adds.md)
- [Synchronisatie van Azure Active Directory-gebruikers met een HDInsight-cluster](../hdinsight-sync-aad-users-to-cluster.md)
- [Apache Hive-beleid configureren in aan domein gekoppelde HDInsight](../domain-joined/apache-domain-joined-run-hive.md)
- [Apache Oozie in aan domein gekoppelde HDInsight Hadoop clusters uitvoeren](../domain-joined/hdinsight-use-oozie-domain-joined-clusters.md)

## <a name="implement-end-to-end-enterprise-security"></a>Beveiliging voor complete bedrijven implementeren

End-to-end bedrijfsbeveiliging kan worden bereikt met behulp van de volgende:

- **Privé en beschermd gegevenspijplijn (perimeterbeveiliging niveau)**
    - Perimeter-level Security kan worden bereikt via Azure Virtual Networks, Network Security Groups en Gateway-service.

- **Verificatie en autorisatie voor toegang tot gegevens**
    - Aan domein gekoppelde HDInsight-cluster met behulp van Azure Active Directory Domain Services maken. (Enterprise-beveiligingspakket).
    - Ambari gebruiken op basis van rollen om toegang te bieden tot clusterresources voor AD-gebruikers.
    - Gebruik Apache Ranger om in te stellen toegang beheren beleid voor Hive op de tabel / kolom / rijniveau.
    - SSH-toegang tot het cluster kan worden beperkt tot alleen de beheerder.

- **Controle**
    - Weergeven en rapporteren van alle toegang tot het HDInsight-clusterresources en gegevens.
    - Weergeven en rapporteren van alle wijzigingen aan het beleid voor toegangsbeheer.

- **Versleuteling**
    - Transparante versleuteling van Server-Side met behulp van Microsoft beheerde sleutels of door de klant beheerde sleutels.
    - Versleuteling tijdens overdracht met behulp van versleuteling van de Client-Side-, https- en TLS.

Raadpleeg voor meer informatie de volgende artikelen:

- [Overzicht van de virtuele netwerken van Azure](../../virtual-network/virtual-networks-overview.md)
- [Overzicht van Azure Network Security Groups](../../virtual-network/security-overview.md)
- [Azure Virtual Network-peering](../../virtual-network/virtual-network-peering-overview.md)
- [Azure storage-beveiligingshandleiding](../../storage/common/storage-security-guide.md)
- [Azure Storage-Service-versleuteling ' at rest '](../../storage/common/storage-service-encryption.md)

## <a name="use-monitoring--alerting"></a>Gebruik bewaking en waarschuwingen

Zie het artikel voor meer informatie:

[Overzicht van Azure Monitor](../../azure-monitor/overview.md)

## <a name="upgrade-clusters"></a>Bijwerken van clusters

Regelmatig bijwerken naar de nieuwste versie van HDInsight om te profiteren van de nieuwste functies. De volgende stappen kunnen worden gebruikt voor het cluster upgraden naar de nieuwste versie:

1. Maak een nieuwe TEST HDInsight-cluster met behulp van de meest recente beschikbare HDInsight versie.
1. Testen op het nieuwe cluster om ervoor te zorgen dat de taken en werkbelastingen werken zoals verwacht.
1. Taken of toepassingen of workloads zo nodig wijzigen.
1. Back-up van alle tijdelijke gegevens die lokaal zijn opgeslagen op de clusterknooppunten.
1. Het bestaande cluster verwijderen.
1. Een van de meest recente versie van HDInsight-cluster maken in het hetzelfde VNET-subnet, met behulp van de dezelfde gegevens en metagegevens standaardopslag als de vorige cluster.
1. Importeer alle tijdelijke gegevens die back-up is gemaakt.
1. Start taken/doorgaan met het verwerken met behulp van het nieuwe cluster.

Zie het artikel voor meer informatie: [HDInsight-cluster upgraden naar een nieuwe versie](../hdinsight-upgrade-cluster.md).

## <a name="patch-cluster-operating-systems"></a>Patch voor cluster-besturingssystemen

HDInsight zorgt patchen van het besturingssysteem van de virtuele machines die door HDInsight-clusters worden gebruikt als een beheerde Hadoop-service.

Zie het artikel voor meer informatie: [OS-patches voor HDInsight](../hdinsight-os-patching.md).

## <a name="post-migration"></a>Na de migratie

1. **Toepassingen herstellen** - iteratief Breng de benodigde wijzigingen aan de taken, processen en scripts.
2. **Uitvoeren van Tests** - iteratief uitvoeren functionele en prestaties testen.
3. **Optimaliseren** : los eventuele problemen met prestaties met op basis van de resultaten van de bovenstaande en vervolgens testen om te bevestigen dat de prestatieverbeteringen.

## <a name="next-steps"></a>Volgende stappen

- Meer informatie over [HDInsight 4.0](https://docs.microsoft.com/azure/hdinsight/hadoop/apache-hadoop-introduction).
