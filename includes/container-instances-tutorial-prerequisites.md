---
title: bestand opnemen
description: bestand opnemen
services: container-instances
author: mmacy
ms.service: container-instances
ms.topic: include
ms.date: 03/20/2018
ms.author: marsma
ms.custom: include file
ms.openlocfilehash: 95a8cf4eca7969631d069a31c9d08ba52c8b02b4
ms.sourcegitcommit: 48ab1b6526ce290316b9da4d18de00c77526a541
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/23/2018
---
U moet aan de volgende vereisten voldoen om deze zelfstudie te voltooien:

**Azure CLI**: Azure CLI versie 2.0.29 of hoger moet op uw lokale computer zijn geïnstalleerd. Voer `az --version` uit om de versie te bekijken. Als u Azure CLI 2.0 wilt installeren of upgraden, raadpleegt u [Azure CLI 2.0 installeren][azure-cli-install].

**Docker**: in deze zelfstudie wordt ervan uitgegaan dat u een basiskennis hebt van Docker-kernconcepten zoals containers, containerinstallatiekopieën en eenvoudige `docker`-opdrachten. Zie het [Docker-overzicht][docker-get-started] voor een inleiding tot de basisprincipes van Docker en containers.

**Docker Engine**: om deze zelfstudie te voltooien, moet Docker Engine lokaal zijn geïnstalleerd. Docker biedt pakketten waarmee de Docker-omgeving op [MacOS][docker-mac], [Windows][docker-windows] en [Linux][docker-linux] kan worden geconfigureerd.

> [!IMPORTANT]
> Omdat de Docker-daemon niet is opgenomen in Azure Cloud Shell, *moeten* zowel Azure CLI als Docker Engine op uw *lokale computer* zijn geïnstalleerd om deze zelfstudie te voltooien. U kunt niet Azure Cloud Shell gebruiken voor deze zelfstudie.

<!-- LINKS - External -->
[docker-get-started]: https://docs.docker.com/engine/docker-overview/
[docker-linux]: https://docs.docker.com/engine/installation/#supported-platforms
[docker-mac]: https://docs.docker.com/docker-for-mac/
[docker-windows]: https://docs.docker.com/docker-for-windows/

<!-- LINKS - Internal -->
[azure-cli-install]: /cli/azure/install-azure-cli