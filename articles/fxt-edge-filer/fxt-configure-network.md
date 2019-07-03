---
title: Netwerkinstellingen voor het Microsoft Azure FXT Edge Filer cluster aanpassen
description: Over het aanpassen van de netwerkinstellingen na het maken van het cluster Azure FXT Edge Filer
author: ekpgh
ms.service: fxt-edge-filer
ms.topic: tutorial
ms.date: 06/20/2019
ms.author: v-erkell
ms.openlocfilehash: 36ed354304cb1c88e48088f4b36c1ad0350af0dc
ms.sourcegitcommit: 5bdd50e769a4d50ccb89e135cfd38b788ade594d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/03/2019
ms.locfileid: "67542999"
---
# <a name="tutorial-configure-the-clusters-network-settings"></a>Zelfstudie: De netwerkinstellingen van het cluster configureren 

Voordat u een nieuw cluster Azure FXT Edge Filer gebruiken, moet u controleren en verschillende netwerkinstellingen aanpassen voor uw werkstroom. 

In deze zelfstudie wordt uitgelegd dat de netwerkinstellingen die u mogelijk nodig hebt om aan te passen voor een nieuw cluster. 

U leert: 

> [!div class="checklist"]
> * Welke netwerkinstellingen moeten mogelijk worden bijgewerkt nadat een cluster is gemaakt
> * Welke Azure FXT Edge Filer van use cases vereisen een AD-server of een DNS-server 
> * DNS (RRDNS) automatisch laden saldo clientaanvragen aan het cluster FXT round robin configureren

De hoeveelheid tijd die nodig is om deze stappen te voltooien, is afhankelijk van hoeveel configuratiewijzigingen nodig zijn in uw systeem:

* Als u alleen hoeft te lezen via de zelfstudie en controleer de instellingen van een paar, wordt het kost 10 tot 15 minuten. 
* Als u configureren van DNS round robin wilt, kan deze taak een uur of langer duren.

## <a name="adjust-network-settings"></a>Netwerkinstellingen aanpassen

Verschillende netwerkgerelateerde taken maken deel uit van het instellen van een nieuw Azure FXT Edge Filer-cluster. Controleer deze lijst en bepaal welke van toepassing op uw systeem.

Lees voor meer informatie over instellingen voor het cluster [configureren netwerkservices](https://azure.github.io/Avere/legacy/ops_guide/4_7/html/network_overview.html) in de handleiding voor het Cluster.

* Configureren van DNS round robin voor het netwerk clientgerichte (optioneel)

  Verkeer gelijkmatig verdeelt cluster door het configureren van het DNS-systeem, zoals beschreven in [DNS configureren voor het cluster FXT Edge Filer](#configure-dns-for-load-balancing).

* Controleer of de NTP-instellingen

* Configureren van Active Directory en de naam van de gebruikersnaam/groep downloads (indien nodig)

  Als de netwerkhosts van uw Active Directory of een ander soort externe Active Directory gebruikt, moet u de configuratie van directory services van het cluster om in te stellen hoe de gebruikersnaam-en groepsgegevens voor het downloaden van het cluster wijzigen. Lezen **Cluster** > **Directory Services** in het Cluster configuratiehandleiding voor meer informatie.

  Een AD-server is vereist als u wilt dat SMB-ondersteuning. Configureer AD voordat u begint met het instellen van SMB.

* Definieer de VLAN's (optioneel)
  
  Eventuele extra VLAN's die nodig zijn voor het definiëren van uw cluster vservers en globale naamruimte configureren. Lezen [werken met VLAN's](https://azure.github.io/Avere/legacy/ops_guide/4_7/html/network_overview.html#vlan-overview) in het Cluster configuratiehandleiding voor meer informatie.

* Proxy-servers configureren (indien nodig)

  Als uw cluster gebruikmaakt van een proxyserver externe adressen bereiken, volg deze stappen in te stellen:

  1. Definieer de proxyserver in de **proxyconfiguratie** instellingenpagina
  1. Toepassen van de proxyserver-configuratie met de **Cluster** > **algemene instellingen** pagina of het **Core Filer Details** pagina.
  
  Lees voor meer informatie, [met behulp van webproxy](https://azure.github.io/Avere/legacy/ops_guide/4_7/html/proxy_overview.html) in de handleiding voor het Cluster.

* Uploaden [versleutelingscertificaten](#encryption-certificates) voor het cluster moet gebruiken (optioneel)

### <a name="encryption-certificates"></a>Certificaten voor bestandsversleuteling

Het cluster FXT Edge Filer maakt gebruik van X.509-certificaten voor deze functies:

* Voor het versleutelen van verkeer van het cluster-beheer

* Om te verifiëren namens een client KMIP-servers van derden

* Voor het controleren van servercertificaten voor cloudproviders

Als u nodig hebt voor het uploaden van certificaten naar het cluster, gebruikt u de **Cluster** > **certificaten** instellingenpagina. Informatie vindt u in de [Cluster > Certificaten](https://azure.github.io/Avere/legacy/ops_guide/4_7/html/gui_certificates.html) pagina van de handleiding voor het Cluster.

Voor het versleutelen van communicatie binnen het cluster management, gebruikt u de **Cluster** > **algemene instellingen** pagina te selecteren welk certificaat moet worden gebruikt voor administratieve SSL-instellingen.

> [!Note] 
> Cloud-service toegang tot sleutels worden opgeslagen met behulp van de **Cloudreferenties** configuratiepagina. De [toevoegen van een filer core](fxt-add-storage.md#add-a-core-filer) sectie hierboven toont een voorbeeld; Lees de handleiding van de configuratie van Cluster [Cloudreferenties](https://azure.github.io/Avere/legacy/ops_guide/4_7/html/gui_cloud_credentials.html) sectie voor meer informatie. 

## <a name="configure-dns-for-load-balancing"></a>DNS configureren voor de taakverdeling

In deze sectie vindt u de basisbeginselen van het configureren van een round robin DNS (RRDNS) systeem voor het distribueren van de clientbelasting tussen alle clientgerichte IP-adressen in uw cluster FXT Edge Filer. 

### <a name="decide-whether-or-not-to-use-dns"></a>Bepalen of of niet gebruik van DNS

Taakverdeling wordt altijd aanbevolen, maar u hoeft te worden altijd gebruik van DNS. Bijvoorbeeld, met bepaalde typen werkstromen van de client kan het zinvol meer een script gebruiken om de cluster-IP-adressen gelijk worden verdeeld over clients toewijzen bij het koppelen van het cluster. Sommige methoden worden beschreven [koppelen van het cluster](fxt-mount-clients.md). 

Deze zaken waarmee u rekening moet houden bij het bepalen of er al of niet gebruik van een DNS-server: 

* Als uw systeem wordt gebruikt door de NFS-clients, is DNS niet vereist. Het is mogelijk om op te geven van alle netwerkadressen met behulp van numerieke IP-adressen. 

* Als uw systeem SMB (CIFS) toegang ondersteunt, is DNS is vereist, omdat u een DNS-domein voor de Active Directory-server moet opgeven.

* DNS is vereist als u wilt gebruiken van Kerberos-verificatie.

### <a name="round-robin-dns-configuration-details"></a>Informatie over de configuratie van de round robin DNS

Wanneer clients toegang het cluster tot, verdeelt RRDNS automatisch hun aanvragen tussen alle beschikbare interfaces.

Configureer de DNS-server voor het afhandelen van clientgerichte cluster adressen, zoals wordt weergegeven in het volgende diagram voor optimale prestaties.

Een cluster vserver wordt weergegeven aan de linkerkant en IP-adressen in het midden- en aan de rechterkant weergegeven. Elke client access point configureren met A-records en -verwijzingen, zoals wordt geïllustreerd.

![Diagram van DNS round robin - gedetailleerde alt-tekstkoppeling volgt installatiekopie](media/fxt-cluster-config/fxt-rrdns-diagram.png) 
[gedetailleerde tekstbeschrijving](https://azure.github.io/Avere/legacy/Azure-FXT-EdgeFilerDNSconfiguration-alt-text.html)

Elke client-IP-adres moet een unieke naam voor intern gebruik door het cluster. (In dit diagram, de client-IP-adressen zijn met de naam vs1-client - IP-* voor de duidelijkheid, maar in de productieomgeving u iets meer beknopte, zoals client * waarschijnlijk moet gebruiken.)

Clients koppel het cluster met behulp van de naam van de vserver als het argument van de server. 

Wijzigen van de DNS-server ``named.conf`` bestand cyclische volgorde voor query's naar uw vserver instellen. Deze optie zorgt ervoor dat alle van de beschikbare waarden worden doorlopen. Voeg een instructie als volgt uit:

```
options {
    rrset-order {
        class IN A name "vserver1.example.com" order cyclic;
    };
};
```

De volgende ``nsupdate`` opdrachten vindt u een voorbeeld van DNS correct configureren:

```
update add vserver1.example.com. 86400 A 10.0.0.10
update add vserver1.example.com. 86400 A 10.0.0.11
update add vserver1.example.com. 86400 A 10.0.0.12
update add vs1-client-IP-10.example.com. 86400 A 10.0.0.10
update add vs1-client-IP-11.example.com. 86400 A 10.0.0.11
update add vs1-client-IP-12.example.com. 86400 A 10.0.0.12
update add 10.0.0.10.in-addr.arpa. 86400 PTR vs1-client-IP-10.example.com
update add 11.0.0.10.in-addr.arpa. 86400 PTR vs1-client-IP-11.example.com
update add 12.0.0.10.in-addr.arpa. 86400 PTR vs1-client-IP-12.example.com
```

### <a name="enable-dns-in-the-cluster"></a>DNS in het cluster inschakelen 

Geef de DNS-server die gebruikmaakt van het cluster in de **Cluster** > **beheernetwerk** instellingenpagina. Instellingen op de pagina zijn onder andere:

* DNS-serveradres
* DNS-domeinnaam
* DNS-zoekdomeinen

Lees voor meer informatie, [DNS-instellingen](<https://azure.github.io/Avere/legacy/ops_guide/4_7/html/gui_admin_network.html#gui-dns>) in de handleiding voor het Cluster.

## <a name="next-steps"></a>Volgende stappen

Dit is de laatste stap van de basisconfiguratie voor het cluster Azure FXT Edge Filer. 

* Meer informatie over van het systeem-LED's en andere indicatoren in [bewaken hardwarestatus](fxt-monitor.md).
* Meer informatie over hoe clients van de rand FXT Filer koppelen moeten-cluster in [koppelen van het cluster](fxt-mount-clients.md). 
* Zie voor meer informatie over het werken met en het beheren van een cluster FXT Edge Filer de [Cluster configuratiehandleiding](https://azure.github.io/Avere/legacy/ops_guide/4_7/html/ops_conf_index.html). 