---
title: IBM Db2 pureScale op Azure implementeren
description: Meer informatie over het implementeren van een [voorbeeld architectuur](ibm-db2-purescale-azure.md) recent gebruikt voor het migreren van een onderneming van hun IBM Db2-omgeving die worden uitgevoerd op z/OS IBM Db2 pureScale op Azure.
services: virtual-machines-linux
documentationcenter: ''
author: njray
manager: edprice
editor: edprice
tags: ''
ms.assetid: ''
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 11/09/2018
ms.author: njray
ms.openlocfilehash: 24bdc9867af869437cc0e440a80e5bf4813abbae
ms.sourcegitcommit: 8314421d78cd83b2e7d86f128bde94857134d8e1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/19/2018
ms.locfileid: "51977536"
---
# <a name="deploy-ibm-db2-purescale-on-azure"></a>IBM Db2 pureScale op Azure implementeren

Dit artikel wordt beschreven hoe u implementeert een [voorbeeld architectuur](ibm-db2-purescale-azure.md) recent gebruikt voor het migreren van een onderneming van hun IBM Db2-omgeving die worden uitgevoerd op z/OS IBM Db2 pureScale op Azure.

Als u wilt volgen de stappen die worden gebruikt tijdens de migratie, Zie de installatiescripts in [Db2onAzure](http://aka.ms/db2onazure) -bibliotheek op GitHub. Deze scripts zijn gebaseerd op de architectuur die wordt gebruikt voor de werkbelasting van een typische, middelgrote online transaction processing (OLTP).

## <a name="get-started"></a>Aan de slag

Voor het implementeren van deze architectuur downloaden en uitvoeren van het script deploy.sh is gevonden in de [Db2onAzure](http://aka.ms/db2onazure) -bibliotheek op GitHub.

De opslagplaats bevat ook scripts die u gebruiken kunt voor het instellen van een Grafana-dashboard dat kan worden gebruikt om op te vragen Prometheus, open-source controle en opgenomen in Db2 waarschuwingssysteem.

> [!NOTE]
> Het script deploy.sh op de client persoonlijke SSH-sleutels maakt en deze wordt doorgegeven aan de sjabloon voor de implementatie via HTTPS. Voor een betere beveiliging wordt u aangeraden [Azure Key Vault](https://docs.microsoft.com/azure/key-vault/key-vault-overview) voor het opslaan van geheimen, sleutels en wachtwoorden.

## <a name="how-the-deployment-script-works"></a>De werking van het script voor implementatie

Het script deploy.sh maakt en configureert de Azure-resources die worden gebruikt in deze architectuur. Het script vraagt u om de Azure-abonnement en de virtuele machines die worden gebruikt in de doelomgeving en vervolgens voert de volgende bewerkingen:

-   Stelt u de resourcegroep, het virtuele netwerk en de subnetten op Azure voor de installatie.

-   Stelt u de nsg's en SSH voor de omgeving.

-   Stelt u meerdere NIC's op zowel de GlusterFS en Db2 pureScale virtuele machines.

-   Hiermee maakt u de GlusterFS opslag virtuele machines.

-   De jumpbox virtuele machine maakt.

-   Hiermee maakt u de Db2 pureScale virtuele machines.

-   Hiermee maakt u de virtuele machine witness die Db2 pureScale pings.

-   Hiermee maakt u een Windows virtuele machine moet gebruiken voor het testen van maar iets niet installeren op deze.

Vervolgens stelt de implementatiescripts u iSCSI-virtuele storage area network (SAN) voor gedeelde opslag op Azure. In dit voorbeeld is iSCSI maakt verbinding met GlusterFS. Deze oplossing biedt ook de mogelijkheid voor het installeren van de iSCSI-doelen als één Windows-knooppunt. (iSCSI biedt een interface voor gedeelde block storage via TCP/IP waarmee Db2 pureScale tijdens de installatieprocedure voor een apparaatinterface gebruiken om te verbinden met gedeelde opslag.) Zie voor GlusterFS basisbeginselen, de [architectuur: typen volumes](https://docs.gluster.org/en/latest/Quick-Start-Guide/Architecture/) onderwerp in aan de slag met GlusterFS.

De implementatiescripts voert u deze algemene stappen uit:

1.  Instellen van een cluster gedeelde opslag op Azure. GlusterFS wordt gebruikt voor het instellen van het cluster gedeelde opslag. Dit omvat het ten minste twee knooppunten in Linux. Zie voor meer informatie setup, [instellen van Red Hat Gluster Storage in Microsoft Azure](https://access.redhat.com/documentation/en-us/red_hat_gluster_storage/3.1/html/deployment_guide_for_public_cloud/chap-documentation-deployment_guide_for_public_cloud-azure-setting_up_rhgs_azure) in de documentatie van Red Hat Gluster.

2.  Een iSCSI-rechtstreekse interface op de doelservers voor Linux voor GlusterFS instellen. Zie voor meer informatie setup, [GlusterFS iSCSI](https://docs.gluster.org/en/latest/Administrator%20Guide/GlusterFS%20iSCSI/) in de handleiding voor het beheer van GlusterFS.

3.  Instellen van de iSCSI-Initiator op de virtuele Linux-machines die toegang tot het GlusterFS-cluster met behulp van iSCSI-doel. Zie voor meer informatie voor installatie, de [configureren een iSCSI-doel en de Initiator In Linux](https://www.rootusers.com/how-to-configure-an-iscsi-target-and-initiator-in-linux/) in de documentatie van RootUsers.

4.  Hiermee installeert GlusterFS als de opslaglaag voor de iSCSI-interface.

Na het maken van het iSCSI-apparaat, wordt de laatste stap is het Db2 pureScale installeren. Als onderdeel van de installatie van de pureScale Db2 [IBM Spectrum schaal](https://www.ibm.com/support/knowledgecenter/SSEPGG_11.1.0/com.ibm.db2.luw.qb.server.doc/doc/t0057167.html) (voorheen bekend als GPFS) worden gebundeld en worden geïnstalleerd op het cluster GlusterFS. Dit systeem geclusterde bestandsserver kunt Db2 pureScale voor het delen van gegevens tussen meerdere virtuele machines die de Db2 pureScale-engine worden uitgevoerd. Zie voor meer informatie de [IBM Spectrum schaal](https://www.ibm.com/support/knowledgecenter/en/STXKQY_4.2.0/ibmspectrumscale42_welcome.html) documentatie op de website van IBM.

## <a name="db2-purescale-response-file"></a>Db2 pureScale responsbestand

De GitHub-opslagplaats bevat Db2server.rsp, een antwoordbestand (.rsp) waarmee u voor het genereren van een geautomatiseerd script voor de installatie van de Db2-pureScale. De volgende tabel bevat de opties voor Db2 pureScale die gebruikmaakt van het antwoordbestand voor installatie. Indien nodig voor uw installatieomgeving, kunt u het antwoordbestand aanpassen.

> [!NOTE]
> Een voorbeeld-responsbestand Db2server.rsp, is opgenomen in de [Db2onAzure](http://aka.ms/db2onazure) -bibliotheek op GitHub. Als u dit bestand gebruikt, moet u deze bewerken voordat deze in uw omgeving kunt werken.

**Opties voor Db2 pureScale antwoord**

| De schermnaam van het               | Veld                                        | Waarde                                                                                                 |
|---------------------------|----------------------------------------------|-------------------------------------------------------------------------------------------------------|
| Welkom                   |                                              | Nieuwe installatie                                                                                           |
| Kies een Product          |                                              | Db2 versie 11.1.3.3. Server-edities met Db2 pureScale                                              |
| Configuratie             | Directory                                    | /Data1/opt/IBM/DB2/V11.1                                                                              |
|                           | Het installatietype selecteren                 | Typische                                                                                               |
|                           | Ik ga akkoord met de IBM-voorwaarden                     | Geselecteerd                                                                                               |
| De eigenaar van het exemplaar            | Bestaande User voor Instance, de naam van gebruiker        | Db2sdin1                                                                                              |
| Speciale gebruiker               | Bestaande gebruiker, de naam van gebruiker                     | Db2sdfe1                                                                                              |
| Cluster-bestandssysteem       | Pad naar schijf partitie apparaat gedeeld            | /dev/DM-2                                                                                             |
|                           | Koppelpunt                                  | / Db2sd\_1804a                                                                                         |
|                           | Gedeelde schijf voor gegevens                         | /dev/DM-1                                                                                             |
|                           | Koppelpunt (gegevens)                           | / Db2fs/datafs1                                                                                        |
|                           | Gedeelde schijf voor logboek                          | /dev/DM-0                                                                                             |
|                           | Koppelpunt (logboek)                            | / Db2fs/logfs1                                                                                         |
|                           | Db2-Cluster Services Tiebreaker. Pad naar de | /dev/DM-3                                                                                             |
| Host-lijst                 | D1 [eth1] d2 [eth1] cf1 [eth1] cf2 [eth1] |                                                                                                       |
|                           | Voorkeur primaire CF                         | CF1                                                                                                   |
|                           | Voorkeur secundaire CF                       | cf2                                                                                                   |
| Responsbestand en samenvatting | eerste optie                                 | Db2-Server-editie installeert met de functie voor IBM Db2 pureScale en Mijn instellingen opslaan in een antwoordbestand |
|                           | De bestandsnaam antwoord                           | /Root/DB2server.rsp                                                                                   |

### <a name="notes-about-this-deployment"></a>Opmerkingen over deze implementatie

-   De waarden voor /dev-dm0, /dev-dm1 /dev-dm2 en /dev-dm3 kunnen wijzigen na een opnieuw opstarten op de virtuele machine waar de installatie uitgevoerd wordt (d0 in het geautomatiseerde script). Als u de juiste waarden zoekt, kunt u de volgende opdracht geven voordat u het antwoordbestand op de server waarop de installatie wordt uitgevoerd:

```
   [root\@d0 rhel]\# ls -als /dev/mapper
   total 0
   0 drwxr-xr-x 2 root root 140 May 30 11:07 .
   0 drwxr-xr-x 19 root root 4060 May 30 11:31 ..
   0 crw------- 1 root root 10, 236 May 30 11:04 control
   0 lrwxrwxrwx 1 root root 7 May 30 11:07 db2data1 -\> ../dm-1
   0 lrwxrwxrwx 1 root root 7 May 30 11:07 db2log1 -\> ../dm-0
   0 lrwxrwxrwx 1 root root 7 May 30 11:26 db2shared -\> ../dm-2
   0 lrwxrwxrwx 1 root root 7 May 30 11:08 db2tieb -\> ../dm-3
```

-   Zodat de werkelijke namen kunnen eenvoudig worden gevonden, de installatiescripts aliassen gebruiken voor de iSCSI-schijven.

-   Wanneer het setup-script wordt uitgevoerd op d0, de **/dev/dm -\***  waarden kunnen afwijken op d1, cf0 en cf1. De installatie van de pureScale Db2 niets uit.

## <a name="troubleshooting-and-known-issues"></a>Het oplossen van problemen en bekende problemen

De GitHub-opslagplaats bevat een kennisdatabase onderhouden door de auteurs. Geeft mogelijke problemen die kunnen optreden en oplossingen die u kunt proberen. Bijvoorbeeld, bekende problemen kunnen optreden wanneer:

-   Probeert te bereiken van IP-adres van de gateway.

-   GPL compileren.

-   De beveiligingshandshake tussen hosts mislukt.

-   Het installatieprogramma Db2 detecteert een bestaand bestandssysteem.

-   GPFS handmatig installeren.

-   Db2 pureScale installeren wanneer GPFS al is gemaakt.

-   Db2 pureScale en GPFS verwijderen.

Zie voor meer informatie over deze en andere bekende problemen, het bestand kb.md in de [Db2onAzure](http://aka.ms/Db2onAzure) opslagplaats.

## <a name="next-steps"></a>Volgende stappen

-   [GlusterFS iSCSI](https://docs.gluster.org/en/latest/Administrator%20Guide/GlusterFS%20iSCSI/)

-   [Het maken van de vereiste gebruikers voor een Db2-pureScale-functie installeren](https://www.ibm.com/support/knowledgecenter/en/SSEPGG_11.1.0/com.ibm.db2.luw.qb.server.doc/doc/t0055374.html?pos=2)

-   [Db2icrt - exemplaar-opdracht maken](https://www.ibm.com/support/knowledgecenter/en/SSEPGG_11.1.0/com.ibm.db2.luw.admin.cmd.doc/doc/r0002057.html)

-   [Db2 pureScale Clusters Data-oplossing](http://www.ibmbigdatahub.com/blog/db2-purescale-clustered-database-solution-part-1)

-   [IBM Data Studio](https://www.ibm.com/developerworks/downloads/im/data/index.html/)

-   [Platform-modernisering Alliance: IBM Db2 in Azure](https://www.platformmodernization.org/pages/ibmdb2azure.aspx)

-   [Azure Virtual Datacenter Lift- and -Shift-handleiding](https://azure.microsoft.com/resources/azure-virtual-datacenter-lift-and-shift-guide/)
