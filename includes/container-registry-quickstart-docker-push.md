---
title: bestand opnemen
description: bestand opnemen
services: container-registry
author: dlepow
ms.service: container-registry
ms.topic: include
ms.date: 01/23/2019
ms.author: danlep
ms.custom: include file
ms.openlocfilehash: 09eaf9465ec3912dea6e1f3ee1693f6bfed50abc
ms.sourcegitcommit: 0f54f1b067f588d50f787fbfac50854a3a64fff7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/12/2019
ms.locfileid: "67176591"
---
## <a name="push-image-to-registry"></a>Installatiekopie pushen naar register

Als u een installatiekopie naar een Azure Container Registry wilt pushen, moet u eerst over een installatiekopie beschikken. Als u nog geen lokale container installatie kopieën hebt, voert u de volgende opdracht [docker pull][docker-pull] uit om een bestaande installatie kopie van docker hub te halen. Haal voor dit voorbeeld de installatiekopie `hello-world` op.

```
docker pull hello-world
```

Voordat u een installatiekopie naar het register kunt pushen, moet u deze taggen met de volledige gekwalificeerde naam van de ACR-aanmeldingsserver. De naam van de aanmeldingsserver heeft de notatie *\<registernaam\>.azurecr.io* (in kleine letters), bijvoorbeeld *mijncontainerregister007.azurecr.io*.

Label de installatiekopie met de opdracht [docker tag][docker-tag]. Vervang `<acrLoginServer>` door de aanmeldingsnaam van het ACR-exemplaar.

```
docker tag hello-world <acrLoginServer>/hello-world:v1
```

Gebruik ten slotte [docker push][docker-push] om de installatiekopie naar de ACR-instantie te pushen. Vervang `<acrLoginServer>` door de aanmeldingsnaam van het ACR-exemplaar. In dit voorbeeld wordt de **hello-world**-opslagplaats met de `hello-world:v1`-installatiekopie gemaakt.

```
docker push <acrLoginServer>/hello-world:v1
```

Nadat u de installatiekopie naar uw containerregister hebt gepusht, verwijdert u de `hello-world:v1`-installatiekopie uit uw lokale Docker-omgeving. (Houd er rekening mee dat met deze [docker RMI][docker-rmi] -opdracht de installatie kopie niet wordt verwijderd uit de opslag plaats **Hallo wereld** in uw Azure container Registry.)

```
docker rmi <acrLoginServer>/hello-world:v1
```

<!-- LINKS - External -->
[docker-push]: https://docs.docker.com/engine/reference/commandline/push/
[docker-pull]: https://docs.docker.com/engine/reference/commandline/pull/
[docker-rmi]: https://docs.docker.com/engine/reference/commandline/rmi/
[docker-run]: https://docs.docker.com/engine/reference/commandline/run/
[docker-tag]: https://docs.docker.com/engine/reference/commandline/tag/

<!-- LINKS - Internal -->

