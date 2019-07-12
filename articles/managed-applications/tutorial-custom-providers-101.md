---
title: Maken van aangepaste acties en resources in Azure
description: In deze zelfstudie gaat over het maken van aangepaste acties en resources in Azure Resource Manager en hoe u kunt integreren in aangepaste werkstromen voor Azure Resource Manager-sjablonen, Azure CLI, Azure Policy, en het activiteitenlogboek.
author: jjbfour
ms.service: managed-applications
ms.topic: tutorial
ms.date: 06/19/2019
ms.author: jobreen
ms.openlocfilehash: 4bbfcf070611e3df5c0fe47070f2ab6961111e07
ms.sourcegitcommit: 66237bcd9b08359a6cce8d671f846b0c93ee6a82
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2019
ms.locfileid: "67800041"
---
# <a name="create-custom-actions-and-resources-in-azure"></a>Maken van aangepaste acties en resources in Azure

Aangepaste providers kunnen u werkstromen in Azure aanpassen. Een aangepaste provider is een overeenkomst tussen Azure en een `endpoint`. Hierdoor kan het toevoegen van nieuwe aangepaste API's in de Azure Resource Manager zodat nieuwe implementatie- en beheermogelijkheden. In deze zelfstudie gaat via een eenvoudig voorbeeld van hoe nieuwe acties en resources toevoegen aan Azure en hoe ze integreren.

In deze zelfstudie is onderverdeeld in de volgende stappen uit:

- Azure Functions instellen voor Azure aangepaste Providers
- Het ontwerpen van een RESTful-eindpunt voor aangepaste providers
- Het maken van en het gebruik van de aangepaste provider

## <a name="setup-azure-functions-for-azure-custom-providers"></a>Azure Functions instellen voor Azure aangepaste Providers

Dit deel van de zelfstudie gaat naar informatie over het instellen van een Azure-functie om te werken met aangepaste providers. Aangepaste providers kunnen werken met een openbare URL.

- [Azure Functions instellen voor Azure aangepaste Providers](./tutorial-custom-providers-function-setup.md)

## <a name="authoring-a-restful-endpoint-for-custom-providers"></a>Het ontwerpen van een RESTful-eindpunt voor aangepaste providers

Dit deel van de zelfstudie gaat informatie over het ontwerpen van een RESTful-eindpunt voor aangepaste providers.

- [Het ontwerpen van een RESTful-eindpunt voor aangepaste providers](./tutorial-custom-providers-function-authoring.md)

## <a name="creating-and-utilizing-the-custom-provider"></a>Het maken van en het gebruik van de aangepaste provider

Dit deel van de zelfstudie gaat informatie over het maken van een aangepaste provider en de aangepaste acties en resources gebruiken.

- [Maken en te gebruiken een aangepaste Azure Provider](./tutorial-custom-providers-create.md)

## <a name="next-steps"></a>Volgende stappen

In dit artikel hebt u geleerd over aangepaste providers en over het bouwen van een. Klik op Doorgaan met de volgende stap van de zelfstudie:

- [Zelfstudie: Azure Functions instellen voor Azure aangepaste Providers](./tutorial-custom-providers-function-setup.md)

Als u referenties of een snelstartgids zoekt, moet u hier enkele koppelingen die nuttig zijn:

- [Snelstart: De aangepaste Resource Provider Azure maken en implementeren van aangepaste resources](./create-custom-provider.md)
- [Procedure: Aangepaste acties toe te voegen aan Azure REST-API](./custom-providers-action-endpoint-how-to.md)
- [Procedure: Aangepaste Resources toe te voegen aan Azure REST-API](./custom-providers-resources-endpoint-how-to.md)
