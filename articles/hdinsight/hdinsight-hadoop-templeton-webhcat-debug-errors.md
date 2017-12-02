---
title: Begrijpen en los fouten op HDInsight - Azure WebHCat | Microsoft Docs
description: Informatie over hoe voor informatie over veelvoorkomende fouten geretourneerd door WebHCat in HDInsight en hoe u deze kunt oplossen.
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: 1b3d94b1-207d-4550-aece-21dc45485549
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 12/01/2017
ms.author: larryfr
ms.openlocfilehash: c21b575e9e055b2dec69bea270012b91df2b662b
ms.sourcegitcommit: be0d1aaed5c0bbd9224e2011165c5515bfa8306c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/01/2017
---
# <a name="understand-and-resolve-errors-received-from-webhcat-on-hdinsight"></a>Begrijpen en oplossen van fouten die zijn ontvangen van WebHCat in HDInsight

Meer informatie over fouten die zijn ontvangen bij het gebruik van WebHCat met HDInsight en hoe u deze kunt oplossen. WebHCat wordt intern gebruikt door de client-side '-hulpprogramma's zoals Azure PowerShell en Data Lake Tools voor Visual Studio.

## <a name="what-is-webhcat"></a>Wat is WebHCat

[WebHCat](https://cwiki.apache.org/confluence/display/Hive/WebHCat) is een REST-API voor [HCatalog](https://cwiki.apache.org/confluence/display/Hive/HCatalog), een tabel en storage management-laag voor Hadoop. WebHCat is standaard ingeschakeld op HDInsight-clusters en wordt gebruikt door verschillende hulpprogramma's voor het verzenden van taken, taakstatus, enz. krijgen zonder in te loggen aan het cluster.

## <a name="modifying-configuration"></a>Configuratie wijzigen

> [!IMPORTANT]
> Verschillende van de in dit document vermelde fouten optreden als een geconfigureerde maximum is overschreden. Wanneer de stap resolutie u kunt een waarde wijzigen noemt, moet u een van de volgende gebruiken om uit te voeren van de wijziging:

* Voor **Windows** clusters: het gebruik van een scriptactie voor het configureren van de waarde tijdens het maken van het cluster. Zie voor meer informatie [ontwikkelen scriptacties](hdinsight-hadoop-script-actions.md).

* Voor **Linux** clusters: gebruik Ambari (web of REST-API) om de waarde te wijzigen. Zie voor meer informatie [beheren HDInsight met Ambari](hdinsight-hadoop-manage-ambari.md)

> [!IMPORTANT]
> Linux is het enige besturingssysteem dat wordt gebruikt in HDInsight-versie 3.4 of hoger. Zie [HDInsight retirement on Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement) (HDInsight buiten gebruik gestel voor Windows) voor meer informatie.

### <a name="default-configuration"></a>Standaardconfiguratie

Als de volgende standaardwaarden zijn overschreden, kan de WebHCat prestaties verslechteren of er fouten optreden:

| Instelling | Wat het doet | Standaardwaarde |
| --- | --- | --- |
| [yarn.scheduler.Capacity.maximum-toepassingen][maximum-applications] |Het maximum aantal taken dat gelijktijdig actief kunnen zijn (in behandeling of wordt uitgevoerd) |10.000 |
| [templeton.Exec.Max-processor][max-procs] |Het maximum aantal aanvragen dat gelijktijdig kunnen worden geleverd |20 |
| [mapreduce.jobhistory.Max-leeftijd-ms][max-age-ms] |Het aantal dagen dat de taakgeschiedenis worden bewaard. |7 dagen |

## <a name="too-many-requests"></a>Te veel aanvragen

**HTTP-statuscode**: 429

| Oorzaak | Oplossing |
| --- | --- |
| U hebt het maximum aantal gelijktijdige aanvragen afgehandeld door WebHCat per minuut (standaard 20) overschreden |Verminder de werkbelasting om ervoor te zorgen dat u niet verzenden van meer dan het maximum aantal gelijktijdige aanvragen of verhoog de limiet voor gelijktijdige aanvragen door het wijzigen van `templeton.exec.max-procs`. Zie voor meer informatie [configuratie wijzigen](#modifying-configuration) |

## <a name="server-unavailable"></a>Server niet beschikbaar

**HTTP-statuscode**: 503

| Oorzaak | Oplossing |
| --- | --- |
| Deze statuscode treedt meestal op tijdens de failover tussen de primaire en secundaire HeadNode voor het cluster |Wacht twee minuten en probeer het opnieuw |

## <a name="bad-request-content-could-not-find-job"></a>Onjuiste aanvraag inhoud: kan de taak niet vinden.

**HTTP-statuscode**: 400

| Oorzaak | Oplossing |
| --- | --- |
| Taakdetails zijn opgeschoond door de taakgeschiedenis schoner |De bewaartermijn voor Taakgeschiedenis is 7 dagen. De bewaartermijn kan worden gewijzigd door het wijzigen van `mapreduce.jobhistory.max-age-ms`. Zie voor meer informatie [configuratie wijzigen](#modifying-configuration) |
| Taak is afgebroken als gevolg van een failover |Verzending van de taak voor maximaal twee minuten opnieuw proberen |
| Er is een ongeldige taak-id gebruikt |Controleer of de taak-id juist is |

## <a name="bad-gateway"></a>Ongeldige gateway

**HTTP-statuscode**: 502

| Oorzaak | Oplossing |
| --- | --- |
| Interne garbagecollection plaatsvindt in het proces van WebHCat |Wachten op garbagecollection voltooid of de WebHCat-service opnieuw starten |
| Time-out opgetreden bij het wachten op een reactie van de ResourceManager-service. Deze fout kan optreden wanneer het aantal actieve toepassingen het geconfigureerde maximum (standaard 10.000 gaat) |Wachten op de momenteel actieve taken om te voltooien of de gelijktijdige taaklimiet verhogen door het wijzigen van `yarn.scheduler.capacity.maximum-applications`. Zie voor meer informatie de [wijzigen configuratie](#modifying-configuration) sectie. |
| Het ophalen van alle taken via de [GET /jobs](https://cwiki.apache.org/confluence/display/Hive/WebHCat+Reference+Jobs) aanroep bij `Fields` is ingesteld op`*` |Geen opgehaald *alle* taakgegevens. Gebruik in plaats daarvan `jobid` details voor taken alleen groter is dan een bepaalde taak-id ophalen. Of gebruik geen`Fields` |
| De WebHCat-service is niet actief tijdens de failover HeadNode |Wacht twee minuten en probeer het opnieuw |
| Er zijn meer dan 500 in behandeling zijnde taken die worden verzonden via WebHCat |Wacht totdat het momenteel in behandeling zijnde taken vóór het verzenden van meer taken zijn voltooid |

[maximum-applications]: http://docs.hortonworks.com/HDPDocuments/HDP2/HDP-2.1.3/bk_system-admin-guide/content/setting_application_limits.html
[max-procs]: https://hive.apache.org/javadocs/hcat-r0.5.0/configuration.html
[max-age-ms]: http://docs.hortonworks.com/HDPDocuments/HDP2/HDP-2.0.6.0/ds_Hadoop/hadoop-mapreduce-client/hadoop-mapreduce-client-core/mapred-default.xml
