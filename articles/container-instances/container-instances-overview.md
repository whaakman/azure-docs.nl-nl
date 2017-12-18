---
title: Overzicht van Azure Container Instances
description: Inzicht krijgen in Azure Container Instances
services: container-instances
author: seanmck
manager: timlt
ms.service: container-instances
ms.topic: overview
ms.date: 07/20/2017
ms.author: seanmck
ms.custom: mvc
ms.openlocfilehash: 98feed192f21cbc2bf9197b6d32646f50ddbd0c5
ms.sourcegitcommit: a48e503fce6d51c7915dd23b4de14a91dd0337d8
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/05/2017
---
# <a name="azure-container-instances"></a>Azure Container Instances

Containers worden in snel tempo de voorkeursmethode voor het verpakken, implementeren en beheren van cloudtoepassingen. Azure Container Instances biedt u een zeer snelle en eenvoudige methode voor het uitvoeren van een container in Azure, waarbij het niet nodig is om virtuele machines in te richten of het serviceniveau te verhogen.

Azure Container Instances is een ideale oplossing voor elk scenario dat kan werken in geïsoleerde containers, met inbegrip van eenvoudige toepassingen, taakautomatisering en het bouwen van taken. Voor scenario's waarin u een volledige indeling met containers nodig hebt, waaronder servicedetectie in meerdere containers, automatisch schalen en gecoördineerde toepassingsupgrades, raden wij de [Azure Container Service](https://docs.microsoft.com/azure/container-service/) aan.

## <a name="fast-startup-times"></a>Snel opstarten

Containers bieden aanzienlijke opstartvoordelen ten opzichte van virtuele machines. Met Azure Container Instances kunt u een container in Azure in enkele seconden starten, zonder de noodzaak tot het inrichten en beheren van virtuele machines.

## <a name="hypervisor-level-security"></a>Beveiliging op hypervisorniveau

In het verleden boden containers toepassingsafhankelijke isolatie en resourcebesturing, maar werden ze niet voldoende veilig beschouwd voor onveilig multitenant gebruik. Met Azure Container Instances is uw toepassing even geïsoleerd in een container als deze op een virtuele machine zou zijn.

## <a name="custom-sizes"></a>Aangepaste grootten

Containers zijn meestal geoptimaliseerd voor het uitvoeren van één toepassing, maar de exacte behoeften van deze toepassingen kunnen aanzienlijk verschillen. Met Azure Container Instances kunt u vragen om precies wat u zoekt in termen van CPU-kernen en het geheugen. U betaalt op basis van wat u aanvraagt, wordt onmiddellijk gefactureerd, zodat u uw uitgaven op basis van de behoeften van kunt optimaliseren.

## <a name="public-ip-connectivity"></a>Openbare IP-verbinding

Met Azure Container Instances kunt u uw containers direct weergeven op internet met een openbaar IP-adres. In de toekomst gaan we onze netwerkmogelijkheden uitbreiden met integratie met virtuele netwerken, load balancers en andere basisonderdelen van de Azure-netwerkinfrastructuur.

## <a name="persistent-storage"></a>Permanente opslag

Voor het ophalen en persisteren van statuswaarden met exemplaren van Azure Container Instances, bieden we direct koppelen van een Azure-bestandsshares.

## <a name="linux-and-windows-containers"></a>Linux- en Windows-containers

Met Azure Container Instances kunt u Windows- en Linux-containers met dezelfde API plannen. Geef eenvoudigweg het basistype van het besturingssysteem aan. De rest is identiek.

## <a name="co-scheduled-groups"></a>Samen geplande groepen

Azure Container Instances biedt ondersteuning voor planning van meerdere containergroepen die een hostmachine, lokaal netwerk, opslag en levenscyclus delen. Hiermee kunt u uw hoofdtoepassing combineren met andere die een ondersteunende rol uitvoeren, zoals logboekregistratie.

## <a name="next-steps"></a>Volgende stappen

Probeer met behulp van onze [quickstart](container-instances-quickstart.md) een container te implementeren naar Azure met een enkele opdracht.