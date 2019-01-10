---
title: Avere vFXT DNS - Azure
description: Een DNS-server voor round robin-taakverdeling met Avere vFXT voor Azure configureren
author: ekpgh
ms.service: avere-vfxt
ms.topic: conceptual
ms.date: 10/31/2018
ms.author: v-erkell
ms.openlocfilehash: 9fd9eaf1e62d063026e0e656346baaaade87064f
ms.sourcegitcommit: 63b996e9dc7cade181e83e13046a5006b275638d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/10/2019
ms.locfileid: "54187141"
---
# <a name="avere-cluster-dns-configuration"></a>DNS-configuratie voor Avere-cluster

In deze sectie vindt u de basisbeginselen van het configureren van een DNS-systeem voor taakverdeling van uw Avere vFXT-cluster. 

Dit document *bevat geen* instructies voor het opzetten en beheren van een DNS-server in de Azure-omgeving. 

In plaats van met behulp van DNS round robin-taakverdeling een vFXT-cluster in Azure, kunt u overwegen handmatige methoden voor het toewijzen van IP-adressen gelijk worden verdeeld over clients wanneer ze zijn gekoppeld. Verschillende manieren worden beschreven in [koppelen van het cluster Avere](avere-vfxt-mount-clients.md). 

Deze zaken waarmee u rekening moet houden bij het bepalen of er al of niet gebruik van een DNS-server: 

* Als uw systeem wordt gebruikt door de NFS-clients, met behulp van DNS is niet vereist: het is mogelijk om op te geven van alle netwerkadressen met behulp van numerieke IP-adressen. 

* Als uw systeem SMB (CIFS) toegang ondersteunt, is DNS is vereist, omdat u een DNS-domein voor de Active Directory-server moet opgeven.

* DNS is vereist als u wilt gebruiken van Kerberos-verificatie.

## <a name="load-balancing"></a>Taakverdeling

Configureer uw DNS-domein voor het gebruik van round robin-taakverdeling voor clientgerichte IP-adressen voor het distribueren van de algehele belasting.

## <a name="configuration-details"></a>Configuratiegegevens

Wanneer clients toegang het cluster tot, verdeelt RRDNS automatisch hun aanvragen tussen alle beschikbare interfaces.

Configureer de DNS-server voor het afhandelen van clientgerichte cluster adressen, zoals wordt weergegeven in het volgende diagram voor optimale prestaties.

Een cluster vserver wordt weergegeven aan de linkerkant en IP-adressen in het midden- en aan de rechterkant weergegeven. Elke client access point configureren met A-records en -verwijzingen, zoals wordt geïllustreerd.

![Diagram round-robin van DNS-Avere](media/avere-vfxt-rrdns-diagram.png) 
<!--- separate text description file provided  [diagram text description](avere-vfxt-rrdns-alt-text.md) -->

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

De volgende opdrachten voor nsupdate Geef een voorbeeld van DNS correct configureren:

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

## <a name="cluster-dns-settings"></a>Cluster DNS-instellingen

Geef de DNS-server die gebruikmaakt van het cluster vFXT in de **Cluster** > **beheernetwerk** instellingenpagina. Instellingen op de pagina zijn onder andere:

* DNS-serveradres
* DNS-domeinnaam
* DNS-zoekdomeinen

Lezen [DNS-instellingen](<https://azure.github.io/Avere/legacy/ops_guide/4_7/html/gui_admin_network.html#gui-dns>) in de Avere Cluster configuratiehandleiding voor meer informatie over het gebruik van deze pagina.


