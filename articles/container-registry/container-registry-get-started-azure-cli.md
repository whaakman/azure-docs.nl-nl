---
title: Een containerregister maken met de Azure-CLI | Microsoft Docs
description: Aan de slag met het maken en beheren van Azure-containerregisters met de Azure-CLI 2.0 (Preview)
services: container-registry
documentationcenter: 
author: stevelas
manager: balans
editor: dlepow
tags: 
keywords: 
ms.assetid: 29e20d75-bf39-4f7d-815f-a2e47209be7d
ms.service: container-registry
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/14/2016
ms.author: stevelas
translationtype: Human Translation
ms.sourcegitcommit: aa4b960ed75b5a4702317bf557b4588e7a54fa0e
ms.openlocfilehash: d806564056390ea2db74eebbbe89e1b20a350f33

---
# <a name="create-a-container-registry-using-the-azure-cli"></a>Een containerregister maken met de Azure-CLI
Gebruik opdrachten in de [Azure-CLI 2.0 (Preview)](https://github.com/Azure/azure-cli) om een containerregister te maken en de instellingen hiervan te beheren op een Linux-, Mac- of Windows-computer. U kunt ook containerregisters maken en beheren via [Azure Portal](container-registry-get-started-portal.md) of programmatisch met de [REST API](https://go.microsoft.com/fwlink/p/?linkid=834376) voor Container Registry.


* Zie voor achtergrondinformatie en concepten [Wat is Azure Container Registry?](container-registry-intro.md)
* Voor hulp bij Container Registry CLI-opdrachten (`az acr`-opdrachten), geeft u de parameter `-h` door aan een willekeurige opdracht.

> [!NOTE]
> Container Registry is momenteel in de preview-versie.
> 
> 

## <a name="prerequisites"></a>Vereisten
* **Azure-CLI 2.0 (Preview)**: zie de [installatie-instructies](https://github.com/Azure/azure-cli/blob/master/README.rst) om de CLI 2.0 Preview te installeren en ermee aan de slag te gaan. Meld u aan bij uw Azure-abonnement door `az login` uit te voeren.
* **Resourcegroep**: maak eerst een [resourcegroep](../azure-resource-manager/resource-group-overview.md#resource-groups) voordat u een containerregister maakt of gebruik een bestaande resourcegroep. De resourcegroep moet op een locatie staan waar de Container Registry-service [beschikbaar](https://azure.microsoft.com/regions/services/) is. Zie [de CLI 2.0 Preview-voorbeelden](https://github.com/Azure/azure-cli-samples/tree/master/arm) om een resourcegroep te maken met de CLI 2.0 Preview. 
* **Opslagaccount** (optioneel): maak een standaard-Azure-[opslagaccount](../storage/storage-introduction.md) om een back-up van het containerregister te maken op dezelfde locatie. Als u geen opslagaccount opgeeft bij het maken van een register met `az acr create`, maakt de opdracht er een. Zie [de CLI 2.0 Preview-voorbeelden](https://github.com/Azure/azure-cli-samples/tree/master/storage) om een opslagaccount te maken met de CLI 2.0 Preview.
* **Service-principal** (optioneel): wanneer u met de CLI een register maakt, is dit standaard niet ingesteld voor toegang. U kunt een bestaande service-principal van Azure Active Directory aan een register toewijzen (of een nieuwe maken en deze toewijzen) of het beheerdersaccount van het register inschakelen. Zie de gedeelten verderop in dit artikel. Bekijk voor meer informatie over registertoegang [Verifiëren met het containerregister](container-registry-authentication.md). 

## <a name="create-a-container-registry"></a>Een containerregister maken
Voer de opdracht `az acr create` uit om een containerregister te maken. 

> [!TIP]
> Geef bij het maken van een register een unieke domeinnaam op het hoogste niveau op, bestaande uit alleen letters en cijfers. De registernaam in de voorbeelden is `myRegistry`, maar u hoort deze door uw eigen unieke naam te vervangen. 
> 
> 

De volgende opdracht maakt gebruik van de minimale parameters om containerregister `myRegistry` te maken in de resourcegroep `myResourceGroup` in de locatie Zuid-centraal VS:

```azurecli
az acr create -n myRegistry -g myResourceGroup -l southcentralus
```

* `--storage-account-name` of `-s` is optioneel. Als u geen opslagaccount opgeeft, wordt dit met een willekeurige naam in de opgegeven resourcegroep gemaakt.

De uitvoer lijkt op het volgende:

![az acr create output](./media/container-registry-get-started-azure-cli/acr_create.png)


Belangrijk:

* `id`: id voor het register in uw abonnement. Deze hebt u nodig wanneer u een service-principal wilt toewijzen. 
* `loginServer`: de door u opgegeven naam die aan alle vereisten voldoet voor het [aanmelden bij het register](container-registry-authentication.md). De naam in dit voorbeeld is `myregistry-contoso.exp.azurecr.io` (zonder hoofdletters).

## <a name="assign-a-service-principal"></a>Een service-principal toewijzen
Gebruik CLI 2.0 Preview-opdrachten om een service-principal van Azure Active Directory aan een register toe te wijzen. Aan de service-principal in deze voorbeelden is de rol Eigenaar toegewezen, maar u kunt ook [andere rollen](../active-directory/role-based-access-control-configure.md) toewijzen.

### <a name="create-a-service-principal-and-assign-access-to-the-registry"></a>Een service-principal maken en toegang tot het register toewijzen
In de volgende opdracht wordt aan een nieuwe service-principal de rol Eigenaar toegewezen voor toegang tot de register-id die met de parameter `--scopes` wordt doorgegeven. Geef een sterk wachtwoord op voor de parameter `--password`.

```azurecli
az ad sp create-for-rbac --scopes /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourcegroups/myresourcegroup/providers/Microsoft.ContainerRegistry/registries/myregistry --role Owner --password myPassword
```



### <a name="assign-an-existing-service-principal"></a>Een bestaande service-principal toewijzen
Als u al een service-principal hebt en hieraan de rol Eigenaar wilt toewijzen voor toegang tot het register, voert u een opdracht uit die lijkt op het volgende voorbeeld. U geeft de app-id van de service-principal door met de parameter `--assignee`:

```azurecli
az role assignment create --scope /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourcegroups/myresourcegroup/providers/Microsoft.ContainerRegistry/registries/myregistry --role Owner --assignee myAppId
```



## <a name="manage-admin-credentials"></a>Beheerreferenties beheren
Er wordt automatisch een beheeraccount gemaakt voor elk containerregister. Dit account is standaard uitgeschakeld. In het volgende voorbeeld worden `az acr`-CLI-opdrachten weergegeven voor het beheren van de beheerreferenties voor uw containerregister.

### <a name="obtain-admin-user-credentials"></a>Beheerreferenties ophalen
```azurecli
az acr credential show -n myRegistry
```

### <a name="enable-admin-user-for-an-existing-registry"></a>Beheerder inschakelen voor een bestaand register
```azurecli
az acr update -n myRegistry --enable-admin
```

### <a name="disable-admin-user-for-an-existing-registry"></a>Beheerder uitschakelen voor een bestaand register
```azurecli
az acr update -n myRegistry --disable-admin
```

## <a name="list-images-and-tags"></a>Installatiekopieën en tags weergeven
Gebruik de `az acr`-CLI-opdrachten om query's toe te passen op de installatiekopieën en tags in een opslagplaats. 

> [!NOTE]
> Container Registry ondersteunt momenteel de opdracht `docker search` niet voor het toepassen van query's op installatiekopieën en tags.


### <a name="list-repositories"></a>Opslagplaatsen weergeven
In het volgende voorbeeld worden de opslagplaatsen in een register weergegeven in JSON-indeling (JavaScript Object Notation):

```azurecli
az acr repository list -n myRegistry -o json
```

### <a name="list-tags"></a>Tags weergeven
In het volgende voorbeeld worden de tags in de opslagplaats **samples/nginx** weergegeven in JSON-indeling:

```azurecli
az acr repository show-tags -n myRegistry --repository samples/nginx -o json
```

## <a name="next-steps"></a>Volgende stappen
* [Uw eerste installatiekopie pushen met de Docker-CLI](container-registry-get-started-docker-cli.md)




<!--HONumber=Nov16_HO3-->


