---
title: IBM DB2 pureScale op Azure implementeren
description: Informatie over het implementeren van een voorbeeld van de architectuur recent gebruikt voor het migreren van een onderneming van de IBM DB2-omgeving die worden uitgevoerd op z/OS IBM DB2 pureScale op Azure.
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
ms.openlocfilehash: fba6b5308b380b374611c09747302dbf8305dd9b
ms.sourcegitcommit: 2d0fb4f3fc8086d61e2d8e506d5c2b930ba525a7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/18/2019
ms.locfileid: "58014979"
---
# <a name="deploy-ibm-db2-purescale-on-azure"></a>IBM DB2 pureScale op Azure implementeren

Dit artikel wordt beschreven hoe u implementeert een [voorbeeld architectuur](ibm-db2-purescale-azure.md) die een enterprise-klant recent is gebruikt voor het migreren van de IBM DB2-omgeving die worden uitgevoerd op z/OS IBM DB2 pureScale op Azure.

Als u wilt volgen de stappen die worden gebruikt voor de migratie, Zie de installatiescripts in de [DB2onAzure](https://aka.ms/db2onazure) -bibliotheek op GitHub. Deze scripts zijn gebaseerd op de architectuur voor een typische, middelgrote online transaction processing (OLTP)-werkbelasting.

## <a name="get-started"></a>Aan de slag

Voor het implementeren van deze architectuur downloaden en uitvoeren van het script deploy.sh is gevonden in de [DB2onAzure](https://aka.ms/db2onazure) -bibliotheek op GitHub.

De opslagplaats bevat ook scripts voor het instellen van een Grafana-dashboard. Het dashboard kunt u query Prometheus, open-source controle en opgenomen in DB2 waarschuwingssysteem.

> [!NOTE]
> Het script deploy.sh op de client persoonlijke SSH-sleutels maakt en deze wordt doorgegeven aan de sjabloon voor de implementatie via HTTPS. Voor een betere beveiliging wordt u aangeraden [Azure Key Vault](https://docs.microsoft.com/azure/key-vault/key-vault-overview) voor het opslaan van geheimen, sleutels en wachtwoorden.

## <a name="how-the-deployment-script-works"></a>De werking van het script voor implementatie

Het script deploy.sh maakt en configureert de Azure-resources voor deze architectuur. Het script vraagt u om de Azure-abonnement en de virtuele machines die worden gebruikt in de doelomgeving en vervolgens voert de volgende bewerkingen:

-   Stelt u de resourcegroep, het virtuele netwerk en de subnetten op Azure voor de installatie

-   De netwerkbeveiligingsgroepen en SSH ingesteld voor de omgeving

-   Stelt u de NIC's op zowel de GlusterFS en DB2 pureScale virtuele machines

-   Hiermee maakt u de GlusterFS opslag virtuele machines

-   Hiermee maakt u de jumpbox virtuele machine

-   Hiermee maakt u de DB2 pureScale virtuele machines

-   Hiermee maakt u de witness-virtuele machine die DB2 pureScale-pings

-   Hiermee maakt u een Windows virtuele machine moet worden gebruikt voor het testen, maar wordt niets mee geïnstalleerd op deze

Vervolgens wordt de implementatiescripts stelt u een iSCSI-virtuele storage area network (SAN) voor gedeelde opslag op Azure. In dit voorbeeld is iSCSI maakt verbinding met GlusterFS. Deze oplossing biedt ook de mogelijkheid voor het installeren van de iSCSI-doelen als één Windows-knooppunt. iSCSI biedt een interface voor gedeelde block storage ten opzichte van de TCP/IP waarmee DB2 pureScale tijdens de installatieprocedure voor een apparaatinterface gebruiken om te verbinden met gedeelde opslag. Zie voor GlusterFS basisbeginselen, de [architectuur: Typen volumes](https://docs.gluster.org/en/latest/Quick-Start-Guide/Architecture/) onderwerp in Gluster Docs.

De implementatiescripts uitvoeren deze algemene stappen:

1.  GlusterFS gebruiken voor het instellen van een cluster gedeelde opslag op Azure. Deze stap omvat ten minste twee knooppunten in Linux. Zie voor meer informatie setup, [instellen van Red Hat Gluster Storage in Microsoft Azure](https://access.redhat.com/documentation/en-us/red_hat_gluster_storage/3.1/html/deployment_guide_for_public_cloud/chap-documentation-deployment_guide_for_public_cloud-azure-setting_up_rhgs_azure) in de documentatie van Red Hat Gluster.

2.  Een iSCSI-rechtstreekse interface op de doelservers voor Linux voor GlusterFS instellen. Zie voor meer informatie setup, [GlusterFS iSCSI](https://docs.gluster.org/en/latest/Administrator%20Guide/GlusterFS%20iSCSI/) in de handleiding voor het beheer van GlusterFS.

3.  Instellen van de iSCSI-initiator op de virtuele Linux-machines. De initiator wordt toegang tot het cluster GlusterFS met behulp van een iSCSI-doel. Zie voor meer informatie setup, [configureren een iSCSI-doel en de Initiator In Linux](https://www.rootusers.com/how-to-configure-an-iscsi-target-and-initiator-in-linux/) in de documentatie van RootUsers.

4.  GlusterFS installeren als u de opslaglaag voor de iSCSI-interface.

Nadat de scripts de iSCSI-apparaat hebt gemaakt, wordt de laatste stap is het DB2 pureScale installeren. Als onderdeel van de installatie van de pureScale DB2 [IBM Spectrum schaal](https://www.ibm.com/support/knowledgecenter/SSEPGG_11.1.0/com.ibm.db2.luw.qb.server.doc/doc/t0057167.html) (voorheen bekend als GPFS) worden gebundeld en worden geïnstalleerd op het cluster GlusterFS. Dit systeem geclusterde bestandsserver kunt DB2 pureScale voor het delen van gegevens tussen de virtuele machines die worden uitgevoerd van de DB2 pureScale-engine. Zie voor meer informatie de [IBM Spectrum schaal](https://www.ibm.com/support/knowledgecenter/en/STXKQY_4.2.0/ibmspectrumscale42_welcome.html) documentatie op de website van IBM.

## <a name="db2-purescale-response-file"></a>DB2 pureScale responsbestand

De GitHub-opslagplaats bevat DB2server.rsp, een antwoordbestand (.rsp) waarmee u voor het genereren van een geautomatiseerd script voor de installatie van de DB2-pureScale. De volgende tabel bevat de opties voor DB2 pureScale die gebruikmaakt van het antwoordbestand voor installatie. U kunt het antwoordbestand als nodig zijn voor uw omgeving.

> [!NOTE]
> Een voorbeeld-responsbestand DB2server.rsp, is opgenomen in de [DB2onAzure](https://aka.ms/db2onazure) -bibliotheek op GitHub. Als u dit bestand gebruikt, moet u deze bewerken voordat deze in uw omgeving kunt werken.

| De schermnaam van het               | Veld                                        | Value                                                                                                 |
|---------------------------|----------------------------------------------|-------------------------------------------------------------------------------------------------------|
| Welkom                   |                                              | Nieuwe installatie                                                                                           |
| Kies een Product          |                                              | DB2 versie 11.1.3.3. Server-edities met DB2 pureScale                                              |
| Configuratie             | Directory                                    | /data1/opt/ibm/db2/V11.1                                                                              |
|                           | Het installatietype selecteren                 | Typische                                                                                               |
|                           | Ik ga akkoord met de IBM-voorwaarden                     | Geselecteerd                                                                                               |
| De eigenaar van het exemplaar            | Bestaande User voor Instance, de naam van gebruiker        | DB2sdin1                                                                                              |
| Speciale gebruiker               | Bestaande gebruiker, de naam van gebruiker                     | DB2sdfe1                                                                                              |
| Cluster-bestandssysteem       | Pad naar schijf partitie apparaat gedeeld            | /dev/dm-2                                                                                             |
|                           | Koppelpunt                                  | /DB2sd\_1804a                                                                                         |
|                           | Gedeelde schijf voor gegevens                         | /dev/dm-1                                                                                             |
|                           | Mount point (Data)                           | /DB2fs/datafs1                                                                                        |
|                           | Gedeelde schijf voor logboek                          | /dev/dm-0                                                                                             |
|                           | Koppelpunt (logboek)                            | /DB2fs/logfs1                                                                                         |
|                           | DB2 Cluster Services Tiebreaker. Pad naar de | /dev/dm-3                                                                                             |
| Host-lijst                 | D1 [eth1] d2 [eth1] cf1 [eth1] cf2 [eth1] |                                                                                                       |
|                           | Voorkeur primaire CF                         | cf1                                                                                                   |
|                           | Voorkeur secundaire CF                       | cf2                                                                                                   |
| Responsbestand en samenvatting | eerste optie                                 | DB2-Server-editie installeert met de functie voor IBM DB2 pureScale en Mijn instellingen opslaan in een antwoordbestand |
|                           | De bestandsnaam antwoord                           | /Root/DB2server.rsp                                                                                   |

### <a name="notes-about-this-deployment"></a>Opmerkingen over deze implementatie

- De waarden voor /dev-dm0, /dev-dm1 /dev-dm2 en /dev-dm3 kunnen wijzigen na het opnieuw opstarten op de virtuele machine waar de installatie uitgevoerd wordt (d0 in het geautomatiseerde script). Als u de juiste waarden zoekt, kunt u de volgende opdracht uit voordat u het antwoordbestand op de server waarop de installatie uitvoert uitgeven:

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

- Zodat de werkelijke namen kunnen eenvoudig worden gevonden, de installatiescripts aliassen gebruiken voor de iSCSI-schijven.

- Wanneer het setup-script wordt uitgevoerd op d0, de **/dev/dm -\***  waarden kunnen afwijken op d1, cf0 en cf1. Het verschil in waarden heeft geen invloed op de installatie van de pureScale DB2.

## <a name="troubleshooting-and-known-issues"></a>Het oplossen van problemen en bekende problemen

De GitHub-opslagplaats bevat een knowledge base die de auteurs onderhouden. Geeft mogelijke problemen en oplossingen die kunt u proberen. Bijvoorbeeld, bekende problemen kunnen gebeuren wanneer:

-   U probeert te bereiken van IP-adres van de gateway.

-   U bent compileren van algemene openbare licentie (GPL).

-   De beveiligingshandshake tussen hosts mislukt.

-   Het installatieprogramma DB2 detecteert een bestaand bestandssysteem.

-   U installeert IBM Spectrum schaal handmatig.

-   U installeert DB2 pureScale wanneer IBM Spectrum schaal al is gemaakt.

-   U kunt DB2 pureScale en IBM Spectrum schaal wilt verwijderen.

Zie voor meer informatie over deze en andere bekende problemen, het bestand kb.md in de [DB2onAzure](https://aka.ms/DB2onAzure) opslagplaats.

## <a name="next-steps"></a>Volgende stappen

-   [GlusterFS iSCSI](https://docs.gluster.org/en/latest/Administrator%20Guide/GlusterFS%20iSCSI/)

-   [Het maken van de vereiste gebruikers voor een DB2-pureScale-functie installeren](https://www.ibm.com/support/knowledgecenter/en/SSEPGG_11.1.0/com.ibm.db2.luw.qb.server.doc/doc/t0055374.html?pos=2)

-   [DB2icrt - exemplaar-opdracht maken](https://www.ibm.com/support/knowledgecenter/en/SSEPGG_11.1.0/com.ibm.db2.luw.admin.cmd.doc/doc/r0002057.html)

-   [DB2 pureScale Clusters Data-oplossing](https://www.ibmbigdatahub.com/blog/db2-purescale-clustered-database-solution-part-1)

-   [IBM Data Studio](https://www.ibm.com/developerworks/downloads/im/data/index.html/)

-   [Platform-modernisering Alliance: IBM DB2 in Azure](https://www.platformmodernization.org/pages/ibmdb2azure.aspx)

-   [Azure Virtual Datacenter Lift- and -Shift-handleiding](https://azure.microsoft.com/resources/azure-virtual-datacenter-lift-and-shift-guide/)
