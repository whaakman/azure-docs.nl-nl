---
title: bestand opnemen
description: bestand opnemen
services: azure-resource-manager
author: tfitzmac
ms.service: azure-resource-manager
ms.topic: include
ms.date: 07/11/2019
ms.author: tomfitz
ms.custom: include file
ms.openlocfilehash: 099bca7483100da1a4ee2f8f10057c416ad145b0
ms.sourcegitcommit: 64798b4f722623ea2bb53b374fb95e8d2b679318
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2019
ms.locfileid: "67841390"
---
U kunt tags toepassen op uw Azure-resources die ze logisch te ordenen in een taxonomie-metagegevens. Elke tag bestaat uit een naam en een waarde-paar. U kunt de naam Omgeving en de waarde Productie bijvoorbeeld toepassen op alle resources in de productie.

Nadat u de tags hebt toegepast, kunt u alle resources in het abonnement ophalen met deze tagnaam en -waarde. Met tags kunt u verwante resources ophalen uit verschillende resourcegroepen bevinden. Deze aanpak is nuttig wanneer u resources moet ordenen voor facturering of beheer.

Uw taxonomie moet rekening houden met een self-servicegebruikers metagegevens strategie naast een strategie voor automatisch labels op de werkbelasting van gebruikers en de nauwkeurigheid wordt verhoogd voor het labelen.

Voor tags gelden de volgende beperkingen:

* Niet alle resourcetypen ondersteunen tags. Om te bepalen als u een label op een resourcetype toepassen kunt, Zie [ondersteuning voor Azure-resources taggen](../articles/azure-resource-manager/tag-support.md).
* Elke resource of resourcegroep kan maximaal 50 tag de naam/waarde-paren hebben. Op dit moment alleen ondersteuning voor opslagaccounts van 15 labels, maar deze limiet zal worden verhoogd tot 50 in een toekomstige release. Als u meer labels dan het maximaal toegestane aantal van toepassing wilt, gebruikt u een JSON-tekenreeks voor de tagwaarde. De JSON-tekenreeks kan veel waarden bevatten die worden toegepast op een enkele tagnaam. Een resourcegroep kan veel bronnen die elk 50 tag de naam/waarde-paren hebben bevatten.
* De tagnaam is beperkt tot 512 tekens en de tagwaarde is beperkt tot 256 tekens. Voor opslagaccounts is de tagnaam beperkt tot 128 tekens en de tagwaarde beperkt tot 256 tekens.
* Gegeneraliseerde VM's bieden geen ondersteuning voor tags.
* Tags die zijn toegepast op de resourcegroep, worden niet overgenomen door de resources in deze resourcegroep.
* Labels kunnen niet worden toegepast op klassieke resources zoals Cloudservices.
* Tag mogen niet deze tekens bevatten: `<`, `>`, `%`, `&`, `\`, `?`, `/`
