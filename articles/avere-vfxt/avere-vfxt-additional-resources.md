---
title: Aanvullende koppelingen over Avere vFXT voor Azure
description: Koppelingen naar aanvullende informatie over Avere vFXT voor Azure
author: ekpgh
ms.service: avere-vfxt
ms.topic: conceptual
ms.date: 10/31/2018
ms.author: v-erkell
ms.openlocfilehash: afba39d1af700650cfbf7226dff36729a76a0bda
ms.sourcegitcommit: 6135cd9a0dae9755c5ec33b8201ba3e0d5f7b5a1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/31/2018
ms.locfileid: "50633958"
---
# <a name="additional-documentation"></a>Aanvullende documentatie

In dit artikel bevat koppelingen naar aanvullende documentatie over de beheerinterface Avere het Configuratiescherm en verwante onderwerpen. 

## <a name="avere-cluster-documentation"></a>Avere clusterdocumentatie

Aanvullende Avere clusterdocumentatie vindt u op de website op <http://library.averesystems.com/>. Aan de hand van deze documenten kunt u inzicht in de mogelijkheden van het cluster en het configureren van de instellingen ervan. 

* De [FXT Cluster maken-handleiding](<http://library.averesystems.com/#fxt_cluster>) is ontworpen voor clusters die zijn opgebouwd uit knooppunten van de fysieke hardware, maar sommige gegevens in het document relevant voor ook vFXT-clusters is. In het bijzonder kunnen nieuwe vFXT clusterbeheerders profiteren van het lezen van de volgende secties:
  * [Ondersteuning en controle-instellingen aanpassen](<http://library.averesystems.com/create_cluster/4_8/html/config_support.html#config-support>) wordt uitgelegd hoe u ondersteuning voor uploaden van de instellingen aanpassen en bewaking op afstand inschakelen. 
  * [Configureren van VServers en globale Namespace](<http://library.averesystems.com/create_cluster/4_8/html/config_vserver.html#config-vserver>) bevat informatie over het maken van een naamruimte clientgerichte.
  * [DNS configureren voor het cluster Avere](<http://library.averesystems.com/create_cluster/4_8/html/config_network.html#dns-overview>) wordt uitgelegd hoe u het configureren van DNS round robin.
  * [Toevoegen van opslag van back-end](<http://library.averesystems.com/create_cluster/4_8/html/config_core_filer.html#add-core-filer>) documenten core filter toevoegen.

* De [Cluster configuratiehandleiding](<http://library.averesystems.com/#operations>) is een volledig overzicht van instellingen en opties voor een Avere-cluster. Een cluster vFXT gebruikmaakt van een subset van deze opties, maar de meeste van de dezelfde configuratiepagina's van toepassing.

* De [Dashboard Guide](<http://library.averesystems.com/#operations>) wordt uitgelegd hoe u de bewaking van de functies van het Configuratiescherm Avere cluster.

## <a name="vfxt-creation-and-management-documentation"></a>documentatie voor het maken en beheren van vFXT

Een volledige handleiding voor het gebruik van vfxt.py, het cloud-cluster maken en het hulpprogramma, vindt u aan <https://github.com/AvereSystems/vFXT.py/blob/master/docs/README.md>.  
