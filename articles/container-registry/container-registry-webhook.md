---
title: Azure Container register webhooks.
description: Informatie over het gebruik van webhooks trigger gebeurtenissen wanneer bepaalde acties worden uitgevoerd in de Register-opslagplaatsen.
services: container-registry
documentationcenter: 
author: neilpeterson
manager: timlt
editor: 
tags: acr, azure-container-registry
keywords: Docker, Containers ACR
ms.assetid: 
ms.service: container-registry
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 10/08/2017
ms.author: nepeters
ms.openlocfilehash: 5a9dab91aafb92f944b473f05144242143e36477
ms.sourcegitcommit: ccb84f6b1d445d88b9870041c84cebd64fbdbc72
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/14/2017
---
# <a name="using-azure-container-registry-webhooks"></a>Met behulp van Azure Container register webhooks.

Een Azure container registry opgeslagen en beheerd persoonlijke Docker-container installatiekopieën, vergelijkbaar met de manier die docker Hub openbare Docker-installatiekopieën worden opgeslagen. U webhooks trigger gebeurtenissen wanneer bepaalde acties uitgevoerd in een van de Register-opslagplaatsen. Webhooks kan reageren op gebeurtenissen op het niveau van het register of ze kunnen worden gericht naar beneden op een specifieke opslagplaats-tag.

Zie voor meer informatie over de achtergrond en concepten [over Azure-Container register](./container-registry-intro.md).

## <a name="prerequisites"></a>Vereisten

- Azure-container beheerd register - een register beheerde container maken in uw Azure-abonnement. Gebruik bijvoorbeeld de [Azure-portal](container-registry-get-started-portal.md) of de [Azure CLI](container-registry-get-started-azure-cli.md).
- Installeren van de docker CLI - instellen van uw lokale computer als een Docker-host en toegang hebben tot de Docker CLI-opdrachten, [Docker-Engine](https://docs.docker.com/engine/installation/).

## <a name="create-webhook-azure-portal"></a>Maken van de webhook Azure-portal

1. Meld u aan bij de [Azure-portal](https://portal.azure.com) en navigeer naar het register waarin u wilt maken van webhooks.

2. Selecteer in de blade container **Webhooks** onder **SERVICES**.

3. Selecteer **toevoegen** op de werkbalk van de blade webhook.

4. Voltooi de *webhook maken* formulier met de volgende informatie:

| Waarde | Beschrijving |
|---|---|
| Naam | De naam die u wilt geven tot de webhook. Deze mag alleen kleine letters en cijfers bevatten en moet 5 tot 50 tekens lang zijn. |
| Service-URI | De URI waar de webhook POST meldingen te verzenden. |
| Aangepaste headers | Headers die u wilt doorgeven samen met de POST-aanvraag. Ze moet ' sleutel: waarde ' indeling. |
| Acties starten | Acties die de webhook activeren. Webhooks kan op dit moment worden geactiveerd door installatiekopie push en/of acties verwijderen. |
| Status | De status van de webhook nadat deze gemaakt. Dit standaard ingeschakeld. |
| Bereik | Het bereik waarmee de webhook werkt. Standaard is het bereik voor alle gebeurtenissen in het register. Het kan worden opgegeven voor een opslagplaats of een tag met de indeling 'opslagplaats: code'. |

Voorbeeld van de webhook formulier:

![ACR-webhook maken gebruikersinterface in de Azure portal](./media/container-registry-webhook/webhook.png)

## <a name="create-webhook-azure-cli"></a>Azure CLI-webhook maken

Gebruik voor het maken van een webhook met de Azure CLI het [az acr-webhook maken](/cli/azure/acr/webhook#create) opdracht.

```azurecli-interactive
az acr webhook create --registry mycontainerregistry --name myacrwebhook01 --actions delete --uri http://webhookuri.com
```

## <a name="test-webhook"></a>Test webhook

### <a name="azure-portal"></a>Azure Portal

Versies vóór met behulp van de webhook voor container push installatiekopie en verwijderingsacties, kunt u het testen met de **Ping** knop. Ping een algemene POST-aanvraag verzendt naar het opgegeven eindpunt en logboeken van het antwoord. Hierdoor kunt u controleren of dat u de webhook juist hebt geconfigureerd.

1. Selecteer de webhook die u wilt testen.
2. Selecteer in de bovenste werkbalk **Ping**.
3. Controleer de reactie van het eindpunt in de **HTTP-STATUS** kolom.

![ACR-webhook maken gebruikersinterface in de Azure portal](./media/container-registry-webhook/webhook-02.png)

### <a name="azure-cli"></a>Azure CLI

U kunt een ACR-webhook met de Azure CLI testen met de [az acr webhook ping](/cli/azure/acr/webhook#ping) opdracht.

```azurecli-interactive
az acr webhook ping --registry mycontainerregistry --name myacrwebhook01
```

Als de resultaten wilt weergeven, gebruikt de [az acr webhook lijst-gebeurtenissen](/cli/azure/acr/webhook#list-events) opdracht.

```azurecli-interactive
az acr webhook list-events --registry mycontainerregistry08 --name myacrwebhook01
```

## <a name="delete-webhook"></a>Webhook verwijderen

### <a name="azure-portal"></a>Azure Portal

Elke webhook kan worden verwijderd door de webhook selecteren en vervolgens de **verwijderen** knop in de Azure-portal.

### <a name="azure-cli"></a>Azure CLI

```azurecli-interactive
az acr webhook delete --registry mycontainerregistry --name myacrwebhook01
```
