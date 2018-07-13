---
title: bestand opnemen
description: bestand opnemen
services: azure-resource-manager
author: tfitzmac
ms.service: azure-resource-manager
ms.topic: include
ms.date: 03/13/2018
ms.author: tomfitz
ms.custom: include file
ms.openlocfilehash: 69c67f437d2f0b7bd6c1f5311eb5ba1d962d889a
ms.sourcegitcommit: 0a84b090d4c2fb57af3876c26a1f97aac12015c5
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2018
ms.locfileid: "38738876"
---
U past tags toe op uw Azure-resources om ze logisch te ordenen op categorieën. Elke tag bestaat uit een naam en een waarde. U kunt de naam Omgeving en de waarde Productie bijvoorbeeld toepassen op alle resources in de productie.

Nadat u de tags hebt toegepast, kunt u alle resources in het abonnement ophalen met deze tagnaam en -waarde. Met tags kunt u verwante resources ophalen uit verschillende resourcegroepen bevinden. Deze aanpak is nuttig wanneer u resources moet ordenen voor facturering of beheer.

Voor tags gelden de volgende beperkingen:

* Elke resource of resourcegroep mag maximaal 15 combinaties van tagnaam en -waarde hebben. Deze beperking geldt alleen voor tags die direct worden toegepast op de resourcegroep of resource. Een resourcegroep kan veel bronnen bevatten die elk 15 combinaties van tagnaam en -waarde bevatten. Als u meer dan 15 waarden aan een resource moet koppelen, gebruikt u een JSON-tekenreeks voor de tagwaarde. De JSON-tekenreeks kan veel waarden bevatten die worden toegepast op een enkele tagnaam. Dit artikel bevat een voorbeeld van het toewijzen van een JSON-tekenreeks aan een tag.
* De tagnaam is beperkt tot 512 tekens en de tagwaarde is beperkt tot 256 tekens. Voor opslagaccounts is de tagnaam beperkt tot 128 tekens en de tagwaarde beperkt tot 256 tekens.
* Tags die zijn toegepast op de resourcegroep, worden niet overgenomen door de resources in deze resourcegroep.
* Labels kunnen niet worden toegepast op klassieke resources zoals Cloudservices.
* Tag mogen niet deze tekens bevatten: `<`, `>`, `%`, `&`, `\`, `?`, `/`
