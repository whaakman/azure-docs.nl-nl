---
title: bestand opnemen
description: bestand opnemen
services: azure-resource-manager
author: tfitzmac
ms.service: azure-resource-manager
ms.topic: include
ms.date: 11/20/2018
ms.author: tomfitz
ms.custom: include file
ms.openlocfilehash: c426df2293cfb2d8ba4dc02e8fc5519c3d822168
ms.sourcegitcommit: e7d4881105ef17e6f10e8e11043a31262cfcf3b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/29/2019
ms.locfileid: "64868775"
---
U kunt tags toepassen op uw Azure-resources die ze logisch te ordenen in een taxonomie-metagegevens. Elke tag bestaat uit een naam en een waarde-paar. U kunt de naam Omgeving en de waarde Productie bijvoorbeeld toepassen op alle resources in de productie.

Nadat u de tags hebt toegepast, kunt u alle resources in het abonnement ophalen met deze tagnaam en -waarde. Met tags kunt u verwante resources ophalen uit verschillende resourcegroepen bevinden. Deze aanpak is nuttig wanneer u resources moet ordenen voor facturering of beheer.

Uw taxonomie moet rekening houden met een self-servicegebruikers metagegevens strategie naast een strategie voor automatisch labels op de werkbelasting van gebruikers en de nauwkeurigheid wordt verhoogd voor het labelen.

Voor tags gelden de volgende beperkingen:

* Niet alle resourcetypen ondersteunen tags. Om te bepalen als u een label op een resourcetype toepassen kunt, Zie [ondersteuning voor Azure-resources taggen](../articles/azure-resource-manager/tag-support.md).
* Elke resource of resourcegroep mag maximaal 15 combinaties van tagnaam en -waarde hebben. Deze beperking geldt alleen voor tags die direct worden toegepast op de resourcegroep of resource. Een resourcegroep kan veel bronnen bevatten die elk 15 combinaties van tagnaam en -waarde bevatten. Als u meer dan 15 waarden aan een resource moet koppelen, gebruikt u een JSON-tekenreeks voor de tagwaarde. De JSON-tekenreeks kan veel waarden bevatten die worden toegepast op een enkele tagnaam. Dit artikel bevat een voorbeeld van het toewijzen van een JSON-tekenreeks aan een tag.
* De tagnaam is beperkt tot 512 tekens en de tagwaarde is beperkt tot 256 tekens. Voor opslagaccounts is de tagnaam beperkt tot 128 tekens en de tagwaarde beperkt tot 256 tekens.
* Virtuele Machines en Virtual Machine Scale Sets zijn beperkt tot een totaal van 2048 tekens in voor alle tagnamen en -waarden.
* Tags die zijn toegepast op de resourcegroep, worden niet overgenomen door de resources in deze resourcegroep.
* Labels kunnen niet worden toegepast op klassieke resources zoals Cloudservices.
* Tag mogen niet deze tekens bevatten: `<`, `>`, `%`, `&`, `\`, `?`, `/`
