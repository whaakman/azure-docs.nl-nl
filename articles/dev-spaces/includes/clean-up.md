---
title: bestand opnemen
description: bestand opnemen
ms.custom: include file
services: azure-dev-spaces
ms.service: azure-dev-spaces
ms.component: azds-kubernetes
author: ghogen
ms.author: ghogen
ms.date: 05/11/2018
ms.topic: include
manager: douge
ms.openlocfilehash: b8b8b1a593328197c61f8edba9f8f849518dd2e5
ms.sourcegitcommit: 4ea0cea46d8b607acd7d128e1fd4a23454aa43ee
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/15/2018
ms.locfileid: "40129089"
---
## <a name="clean-up"></a>Opruimen
Als u een exemplaar van Azure Dev Spaces in een cluster volledig wilt verwijderen, waaronder alle ontwikkelruimtes en de actieve services die erin worden uitgevoerd, gebruikt u de opdracht `az aks remove-dev-spaces`. Houd er rekening mee dat deze actie niet kan worden teruggedraaid. U kunt later opnieuw ondersteuning voor Azure Dev Spaces toevoegen aan het cluster, maar u moet dan helemaal opnieuw beginnen. Uw oude services en ruimtes worden niet hersteld.

In het volgende voorbeeld worden de Azure Dev Spaces-controllers in uw actieve abonnement vermeld en wordt vervolgens de Azure Dev Spaces-controller die is gekoppeld aan het AKS-cluster 'myaks' in de resourcegroep 'myaks-rg' verwijderd.

```cmd
    azds controller list
    az aks remove-dev-spaces --name myaks --resource-group myaks-rg
```

