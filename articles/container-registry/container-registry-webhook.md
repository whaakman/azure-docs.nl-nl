---
title: Azure Container Registry-webhooks
description: Informatie over het gebruik van webhooks voor het triggergebeurtenissen wanneer bepaalde acties worden uitgevoerd in de Register-opslagplaatsen.
services: container-registry
author: mmacy
manager: jeconnoc
ms.service: container-registry
ms.topic: article
ms.date: 12/02/2017
ms.author: marsma
ms.openlocfilehash: aff8f1b18c60610ff1d231661fe142eb6c69f3d7
ms.sourcegitcommit: 11321f26df5fb047dac5d15e0435fce6c4fde663
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/06/2018
ms.locfileid: "37887569"
---
# <a name="using-azure-container-registry-webhooks"></a>Met behulp van Azure Container Registry-webhooks

Een Azure container registry worden opgeslagen en beheerd persoonlijke Docker-containerinstallatiekopieën, net als bij het die docker Hub openbare Docker-installatiekopieën worden opgeslagen. U kunt webhooks om triggergebeurtenissen wanneer bepaalde acties uitgevoerd in een van uw register-opslagplaatsen te gebruiken. Webhooks kan reageren op gebeurtenissen op het niveau van het register, of ze kunnen worden gericht op een specifieke opslagplaats-tag.

Zie voor meer informatie over webhook-aanvragen, [Naslaggids voor Azure Container Registry webhook-schema](container-registry-webhook-reference.md).

## <a name="prerequisites"></a>Vereisten

* Azure container registry - een containerregister maken in uw Azure-abonnement. Gebruik bijvoorbeeld de [Azure-portal](container-registry-get-started-portal.md) of de [Azure CLI](container-registry-get-started-azure-cli.md).
* Installeren van docker-CLI - instellen van uw lokale computer als een Docker-host en toegang tot de Docker CLI-opdrachten, [Docker-Engine](https://docs.docker.com/engine/installation/).

## <a name="create-webhook-azure-portal"></a>Webhook Azure portal maken

1. Aanmelden bij de [Azure-portal](https://portal.azure.com)
1. Ga naar het containerregister in die u wilt maken van een webhook.
1. Onder **SERVICES**, selecteer **Webhooks**.
1. Selecteer **toevoegen** in de werkbalk van de webhook.
1. Voltooi de *webhook maken* formulier met de volgende informatie:

| Waarde | Beschrijving |
|---|---|
| Naam | De naam die u wilt geven tot de webhook. Deze mag alleen kleine letters en cijfers bevatten en moet 5 tot 50 tekens lang zijn. |
| Service-URI | De URI waar de webhook POST meldingen moet verzenden. |
| Aangepaste kopteksten | Headers die u wilt doorgeven, samen met de POST-aanvraag. Deze moeten worden "sleutel: waarde" indeling. |
| Triggeracties | Acties waarvoor de webhook is geactiveerd. Webhooks kan op dit moment worden geactiveerd door een installatiekopie pushen en/of acties verwijderen. |
| Status | De status van de webhook nadat deze gemaakt. Dit standaard ingeschakeld. |
| Bereik | Het bereik op dat de webhook van toepassing. Het bereik is standaard voor alle gebeurtenissen in het register. Het kan worden opgegeven voor een opslagplaats of op een label met de notatie '-opslagplaats: code'. |

Voorbeeld van de webhook-formulier:

![ACR webhook maken gebruikersinterface in Azure portal](./media/container-registry-webhook/webhook.png)

## <a name="create-webhook-azure-cli"></a>Azure CLI webhook maken

Gebruik voor het maken van een webhook met de Azure CLI de [az acr webhook maken](/cli/azure/acr/webhook#az_acr_webhook_create) opdracht.

```azurecli-interactive
az acr webhook create --registry mycontainerregistry --name myacrwebhook01 --actions delete --uri http://webhookuri.com
```

## <a name="test-webhook"></a>Webhook testen

### <a name="azure-portal"></a>Azure Portal

Voorafgaand aan de met behulp van de webhook op container installatiekopie push en delete-acties, kunt u het testen met de **Ping** knop. Ping een algemene POST-aanvraag verzendt naar het opgegeven eindpunt en logboeken van het antwoord. Met behulp van de ping-functie kunt u controleren of dat u de webhook correct hebt geconfigureerd.

1. Selecteer de webhook die u wilt testen.
2. Selecteer in de bovenste werkbalk **Ping**.
3. Controleer de reactie van het eindpunt in de **HTTP-STATUS** kolom.

![ACR webhook maken gebruikersinterface in Azure portal](./media/container-registry-webhook/webhook-02.png)

### <a name="azure-cli"></a>Azure-CLI

Als u wilt testen een ACR-webhook met de Azure CLI, gebruikt u de [az acr webhook ping](/cli/azure/acr/webhook#az_acr_webhook_ping) opdracht.

```azurecli-interactive
az acr webhook ping --registry mycontainerregistry --name myacrwebhook01
```

Als de resultaten wilt weergeven, gebruikt de [az acr list-webhookgebeurtenissen](/cli/azure/acr/webhook#list-events) opdracht.

```azurecli-interactive
az acr webhook list-events --registry mycontainerregistry08 --name myacrwebhook01
```

## <a name="delete-webhook"></a>Webhook verwijderen

### <a name="azure-portal"></a>Azure Portal

Elke webhook kan worden verwijderd door de webhook te selecteren en vervolgens de **verwijderen** knop in de Azure portal.

### <a name="azure-cli"></a>Azure-CLI

```azurecli-interactive
az acr webhook delete --registry mycontainerregistry --name myacrwebhook01
```

## <a name="next-steps"></a>Volgende stappen

[Azure Container Registry webhook-schemaverwijzing](container-registry-webhook-reference.md)