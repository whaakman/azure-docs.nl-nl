---
title: Toepassings- of gebruikersspecifieke Marathon-service
description: Een toepassings- of gebruikersspecifieke Marathon-service maken
services: container-service
author: rgardler
manager: timlt
ms.service: container-service
ms.topic: article
ms.date: 04/12/2016
ms.author: rogardle
ms.custom: mvc
ms.openlocfilehash: 61cc379bf32f3e0db11d8fd9dae36d9ecb6b1a6c
ms.sourcegitcommit: 5d3e99478a5f26e92d1e7f3cec6b0ff5fbd7cedf
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/06/2017
---
# <a name="create-an-application-or-user-specific-marathon-service"></a>Een toepassings- of gebruikersspecifieke Marathon-service maken

Azure Container Service voorziet in een set masterservers waarop we Apache Mesos en Marathon vooraf configureren. Deze kunnen worden gebruikt voor het indelen van uw toepassingen op het cluster, maar het is raadzaam om niet de masterservers voor dit doel te gebruiken. Voor het aanpassen van de configuratie van Marathon moet bijvoorbeeld bij de masterservers zelf worden aangemeld en moeten wijzigingen worden aangebracht. Hierdoor worden gebruikers aangemoedigd unieke masterservers te maken die enigszins verschillen van de norm en die onafhankelijk moeten worden onderhouden en beheerd. Daarnaast is de configuratie die het ene team nodig heeft, mogelijk niet de optimale configuratie voor een ander team.

In dit artikel leggen we uit hoe u een toepassings- of gebruikersspecifieke Marathon-service kunt toevoegen.

Aangezien deze service toebehoort aan één gebruiker of één team, kan deze gebruiker of dat team de service helemaal naar eigen wens configureren. Azure Container Service zal er bovendien voor zorgen dat de service blijft werken. Als de service mislukt, wordt deze opnieuw gestart door Azure Container Service. In de meeste gevallen zult u zelfs niets merken van eventuele uitval.

## <a name="prerequisites"></a>Vereisten
[Implementeer een exemplaar van Azure Container Service](container-service-deployment.md) met orchestrator-type DCOS en [zorg dat de client verbinding kan maken met uw cluster](../container-service-connect.md). Voer ook de volgende stappen uit.

[!INCLUDE [install the DC/OS CLI](../../../includes/container-service-install-dcos-cli-include.md)]

## <a name="create-an-application-or-user-specific-marathon-service"></a>Een toepassings- of gebruikersspecifieke Marathon-service maken
Begin door een JSON-configuratiebestand te maken dat de naam bepaalt van de toepassingsservice die u wilt maken. Hier gebruiken we `marathon-alice` als de naam van het framework. Sla het bestand op naar iets als `marathon-alice.json`:

```json
{"marathon": {"framework-name": "marathon-alice" }}
```

Gebruik vervolgens de DC/OS CLI om het Marathon-exemplaar te installeren met de opties die in uw configuratiebestand zijn ingesteld:

```bash
dcos package install --options=marathon-alice.json marathon
```

U ziet nu dat uw service `marathon-alice` wordt uitgevoerd op het tabblad Services van uw DC/OS-gebruikersinterface. De gebruikersinterface is `http://<hostname>/service/marathon-alice/` als u rechtstreeks toegang wilt.

## <a name="set-the-dcos-cli-to-access-the-service"></a>De DC/OS CLI instellen voor toegang tot de service
Eventueel kunt u de DC/OS CLI configureren voor toegang tot deze nieuwe service door de eigenschap van `marathon.url` zo in te stellen dat deze als volgt verwijst naar het `marathon-alice`-exemplaar:

```bash
dcos config set marathon.url http://<hostname>/service/marathon-alice/
```

Met de opdracht `dcos config show` kunt u controleren met welk exemplaar van Marathon uw CLI werkt. Met de opdracht `dcos config unset marathon.url` kunt u teruggaan naar het gebruik van de Marathon-masterservice.

