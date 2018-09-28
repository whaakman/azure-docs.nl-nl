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
ms.openlocfilehash: 2be7e4d2f3697649df669a4f20ba4db62c1fc486
ms.sourcegitcommit: d1aef670b97061507dc1343450211a2042b01641
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/27/2018
ms.locfileid: "47401637"
---
U kunt tags toepassen op uw Azure-resources die ze logisch te ordenen in een taxonomie-metagegevens. Elke tag bestaat uit een naam en een waarde-paar. U kunt de naam Omgeving en de waarde Productie bijvoorbeeld toepassen op alle resources in de productie.

Nadat u de tags hebt toegepast, kunt u alle resources in het abonnement ophalen met deze tagnaam en -waarde. Met tags kunt u verwante resources ophalen uit verschillende resourcegroepen bevinden. Deze aanpak is nuttig wanneer u resources moet ordenen voor facturering of beheer.

Uw taxonomie moet rekening houden met een self-servicegebruikers metagegevens strategie naast een strategie voor automatisch labels op de werkbelasting van gebruikers en de nauwkeurigheid wordt verhoogd voor het labelen.

Voor tags gelden de volgende beperkingen:

* Elke resource of resourcegroep mag maximaal 15 combinaties van tagnaam en -waarde hebben. Deze beperking geldt alleen voor tags die direct worden toegepast op de resourcegroep of resource. Een resourcegroep kan veel bronnen bevatten die elk 15 combinaties van tagnaam en -waarde bevatten. Als u meer dan 15 waarden aan een resource moet koppelen, gebruikt u een JSON-tekenreeks voor de tagwaarde. De JSON-tekenreeks kan veel waarden bevatten die worden toegepast op een enkele tagnaam. Dit artikel bevat een voorbeeld van het toewijzen van een JSON-tekenreeks aan een tag.
* De tagnaam is beperkt tot 512 tekens en de tagwaarde is beperkt tot 256 tekens. Voor opslagaccounts is de tagnaam beperkt tot 128 tekens en de tagwaarde beperkt tot 256 tekens.
* Virtuele Machines zijn beperkt tot een totaal van 2048 tekens in voor alle tagnamen en -waarden.
* Tags die zijn toegepast op de resourcegroep, worden niet overgenomen door de resources in deze resourcegroep.
* Labels kunnen niet worden toegepast op klassieke resources zoals Cloudservices.
* Tag mogen niet deze tekens bevatten: `<`, `>`, `%`, `&`, `\`, `?`, `/`
