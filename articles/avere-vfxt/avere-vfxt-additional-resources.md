---
title: Aanvullende koppelingen over Avere vFXT voor Azure
description: Koppelingen naar aanvullende informatie over Avere vFXT voor Azure
author: ekpgh
ms.service: avere-vfxt
ms.topic: conceptual
ms.date: 01/09/2019
ms.author: v-erkell
ms.openlocfilehash: 2efbe7ddc39b8bde76ee4a135f3f44af0864a374
ms.sourcegitcommit: 63b996e9dc7cade181e83e13046a5006b275638d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/10/2019
ms.locfileid: "54188567"
---
# <a name="additional-documentation"></a>Aanvullende documentatie

In dit artikel bevat koppelingen naar aanvullende documentatie over de beheerinterface Avere het Configuratiescherm en verwante onderwerpen. 

## <a name="avere-cluster-documentation"></a>Avere clusterdocumentatie

Aanvullende Avere clusterdocumentatie vindt u op de website op <https://azure.github.io/Avere/>. Aan de hand van deze documenten kunt u inzicht in de mogelijkheden van het cluster en het configureren van de instellingen ervan. 

* De [FXT Cluster maken-handleiding](<https://azure.github.io/Avere/#fxt_cluster>) is ontworpen voor clusters die zijn opgebouwd uit knooppunten van de fysieke hardware, maar sommige gegevens in het document relevant voor ook vFXT-clusters is. In het bijzonder kunnen nieuwe vFXT clusterbeheerders profiteren van het lezen van de volgende secties:
  * [Ondersteuning en controle-instellingen aanpassen](<https://azure.github.io/Avere/legacy/create_cluster/4_8/html/config_support.html#config-support>) wordt uitgelegd hoe u ondersteuning voor uploaden van de instellingen aanpassen en bewaking op afstand inschakelen. 
  * [Configureren van VServers en globale Namespace](<https://azure.github.io/Avere/legacy/create_cluster/4_8/html/config_vserver.html#config-vserver>) bevat informatie over het maken van een naamruimte clientgerichte.
  * [DNS configureren voor het cluster Avere](<https://azure.github.io/Avere/legacy/create_cluster/4_8/html/config_network.html#dns-overview>) wordt uitgelegd hoe u het configureren van DNS round robin.
  * [Toevoegen van opslag van back-end](<https://azure.github.io/Avere/legacy/create_cluster/4_8/html/config_core_filer.html#add-core-filer>) documenten core filter toevoegen.

* De [Cluster configuratiehandleiding](<https://azure.github.io/Avere/#operations>) is een volledig overzicht van instellingen en opties voor een Avere-cluster. Een cluster vFXT gebruikmaakt van een subset van deze opties, maar de meeste van de dezelfde configuratiepagina's van toepassing.

* De [Dashboard Guide](<https://azure.github.io/Avere/#operations>) wordt uitgelegd hoe u de bewaking van de functies van het Configuratiescherm Avere cluster.

## <a name="vfxt-creation-and-management-documentation"></a>documentatie voor het maken en beheren van vFXT

Een volledige handleiding voor het gebruik van vfxt.py, het cloud-cluster maken en het hulpprogramma, is beschikbaar op GitHub: [Cluster-cloudbeheer met vfxt.py](https://github.com/Azure/AvereSDK/blob/master/docs/README.md).  
