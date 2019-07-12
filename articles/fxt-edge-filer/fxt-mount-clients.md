---
title: Clients op het Microsoft Azure FXT Edge Filer-cluster koppelen
description: Hoe NFS-clientcomputers de cache van Azure FXT Edge Filer hybride opslag kunnen koppelen
author: ekpgh
ms.service: fxt-edge-filer
ms.topic: tutorial
ms.date: 06/20/2019
ms.author: v-erkell
ms.openlocfilehash: 5471bf4041275d5988414def99dd2130f51fbb80
ms.sourcegitcommit: 441e59b8657a1eb1538c848b9b78c2e9e1b6cfd5
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2019
ms.locfileid: "67828025"
---
# <a name="tutorial-mount-the-cluster"></a>Zelfstudie: Het cluster koppelen

In deze zelfstudie leert u hoe u NFS-clients aan het cluster Azure FXT Edge Filer koppelen. Clients Koppel de virtuele naamruimte paden die u hebt toegewezen wanneer u back-end-opslag hebt toegevoegd. 

In deze zelfstudie leert: 

> [!div class="checklist"]
> * Strategieën voor het laden van taakverdeling clients in het bereik van IP-adressen voor clientgerichte
> * Het maken van een koppelpad vanuit een clientgerichte IP-adres en naamruimte-verbinding
> * Welke argumenten gebruiken in een koppelopdracht

In deze zelfstudie duurt circa 45 minuten.

## <a name="steps-to-mount-the-cluster"></a>Stappen voor het koppelen van het cluster

Volg deze stappen voor het client-computers verbinden met uw Azure FXT Edge Filer-cluster.

1. Bepalen hoe u taakverdeling clientverkeer tussen de clusterknooppunten. Lezen [saldo clientbelasting](#balance-client-load)hieronder voor meer informatie. 
1. Identificeren van het cluster IP-adres en koppelingspunten pad om te koppelen.
1. Bepaal het clientgerichte pad voor de koppeling.
1. Probleem de [koppelingsopdracht](#use-recommended-mount-command-options), met de juiste argumenten.

## <a name="balance-client-load"></a>De clientbelasting saldo

Om te helpen de clientaanvragen balans tussen alle knooppunten in het cluster, moet u clients op het volledige bereik van IP-adressen voor clientgerichte koppelen. Er zijn verschillende manieren voor het automatiseren van deze taak.

Lees voor meer informatie over round robin-DNS-taakverdeling voor het cluster, [DNS configureren voor het cluster Azure FXT Edge Filer](fxt-configure-network.md#configure-dns-for-load-balancing). U moet een DNS-server, die niet in deze artikelen wordt uitgelegd behouden voor het gebruik van deze methode.

Een eenvoudigere methode voor kleine installaties is een script gebruiken om IP-adressen in het bereik toewijzen tijdens het koppelen van een client. 

Andere methoden voor taakverdeling mogelijk geschikt voor grote of complexe systemen. Raadpleeg uw Microsoft-vertegenwoordiger of open een [ondersteuningsaanvraag](fxt-support-ticket.md) voor hulp. (Azure Load Balancer is momenteel *niet ondersteund* met Azure FXT Edge Filer.)

## <a name="create-the-mount-command"></a>Met deze opdracht maken 

Van de client de ``mount`` opdracht wordt de virtuele server (vserver) toegewezen op het cluster Azure FXT Edge Filer naar een pad op het lokale bestandssysteem. 

De indeling is ``mount <FXT cluster path> <local path> {options}``

Er zijn drie elementen met de koppelopdracht: 

* pad van de cluster - een combinatie van IP-adres en naamruimte koppelingspunten pad die hieronder worden beschreven
* lokale pad - het pad op de client 
* Opties voor de opdracht - koppelen (die worden vermeld in [gebruik aanbevolen opdracht koppelingsopties](#use-recommended-mount-command-options))

### <a name="create-the-cluster-path"></a>Het pad van het cluster maken

Het pad van het cluster is een combinatie van de vserver *IP-adres* plus het pad naar een *naamruimte koppelingspunten*. De verbinding van de naamruimte is een virtueel pad dat u hebt gedefinieerd wanneer u [toegevoegd het opslagsysteem](fxt-add-storage.md#create-a-junction).

Als u gebruikt bijvoorbeeld ``/fxt/files`` als naamruimtepad voor uw, uw clients koppelt *IP-adres*: / fxt/bestanden naar hun lokale koppelpunt. 

![Dialoogvenster met/avere/bestanden in het veld voor de naamruimte-pad 'Nieuwe verbinding toevoegen'](media/fxt-mount/fxt-junction-example.png)

Het IP-adres is een van de clientgerichte IP-adressen die zijn gedefinieerd voor de vserver. U kunt het bereik van IP-adressen op twee plaatsen in het cluster het Configuratiescherm clientgerichte vinden:

* **VServers** tabel (tabblad Dashboard -) 

  ![Dashboardtabblad van het Configuratiescherm met het tabblad VServer is geselecteerd in de tabel onder de grafiek en het IP-adressectie omcirkeld](media/fxt-mount/fxt-ip-addresses-dashboard.png)

* **Client naar gericht netwerk** van de instellingenpagina: 

  ![Instellingen > VServer > configuratiepagina van de Client naar gericht netwerk met een cirkel om de sectie-adresbereik van de tabel voor een bepaalde vserver](media/fxt-mount/fxt-ip-addresses-settings.png)

Het IP-adres en het naamruimtepad om het cluster-pad voor de opdracht mount combineren. 

Voorbeeld van de client mount-opdracht: ``mount 10.0.0.12:/sd-access /mnt/fxt {options}``

### <a name="create-the-local-path"></a>Het lokale pad maken

Het lokale pad voor de opdracht mount is aan u. U kunt elke gewenste padstructuur instellen als onderdeel van de virtuele-naamruimte. Ontwerp een naamruimte en het lokale pad dat is handig voor de werkstroom van uw client. 

Lees voor meer informatie over de naamruimte clientgerichte, de Cluster-configuratiehandleiding [naamruimte overzicht](https://azure.github.io/Avere/legacy/ops_guide/4_7/html/gns_overview.html).

Naast de paden, bevatten de [opdracht koppelingsopties](#use-recommended-mount-command-options) die hieronder worden beschreven bij het koppelen van elke client.

### <a name="use-recommended-mount-command-options"></a>Aanbevolen mount-opdrachtopties gebruiken

Doorgeven om te controleren of een naadloze client-koppeling, deze instellingen en de argumenten in de mount-opdracht: 

``mount -o hard,nointr,proto=tcp,mountproto=tcp,retry=30 ${VSERVER_IP_ADDRESS}:/${NAMESPACE_PATH} ${LOCAL_FILESYSTEM_MOUNT_POINT}``

| Vereiste instellingen | |
--- | --- 
``hard`` | Zachte koppelingen aan het cluster Azure FXT Edge Filer zijn gekoppeld aan problemen met toepassingen en mogelijk gegevensverlies. 
``proto=netid`` | Deze optie ondersteunt juiste verwerking van NFS-netwerkfouten.
``mountproto=netid`` | Deze optie ondersteunt juiste verwerking van netwerkfouten voor mount-bewerkingen.
``retry=n`` | Stel ``retry=30`` ter voorkoming van fouten van tijdelijke koppelen. (Een andere waarde wordt aanbevolen in koppelingen van voorgrond.)

| Aanbevolen instellingen  | |
--- | --- 
``nointr``            | Als uw clients oudere OS kernels (vóór April 2008) die ondersteuning bieden voor deze optie gebruikt, deze gebruiken. De optie "Intro" is de standaardinstelling.

## <a name="next-steps"></a>Volgende stappen

Nadat u clients hebt gekoppeld, kunt u uw werkstroom kunt testen en aan de slag met uw cluster.

Als u wilt verplaatsen van gegevens naar een nieuwe cloud core filer, nemen voor profiteren van de structuur van de cache met behulp van parallelle gegevens opnemen. Enkele strategieën worden beschreven in [om gegevens te verplaatsen naar een cluster vFXT](https://docs.microsoft.com/azure/avere-vfxt/avere-vfxt-data-ingest). (Avere vFXT voor Azure is een cloud-gebaseerde product dat gebruikmaakt van caching-technologie is vergelijkbaar met de Azure FXT Edge Filer.)

Lezen [Monitor Azure FXT Edge Filer hardwarestatus](fxt-monitor.md) als u het oplossen van hardwareproblemen nodig hebt. 
