---
title: Azure Resource Manager sjabloon steekproeven - exemplaren van Azure-Container
description: Azure Resource Manager-sjabloon voorbeelden voor exemplaren van Azure-Container
services: container-instances
author: mmacy
manager: jeconnoc
ms.service: container-instances
ms.topic: article
ms.date: 05/17/2018
ms.author: marsma
ms.openlocfilehash: fcc2e6c52e773d95bcdfe43d881fce036fae6513
ms.sourcegitcommit: 96089449d17548263691d40e4f1e8f9557561197
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/17/2018
---
# <a name="azure-resource-manager-templates-for-azure-container-instances"></a>Azure Resource Manager-sjablonen voor Azure Containerexemplaren

De volgende voorbeeldsjablonen implementeren containerexemplaren in verschillende configuraties.

Zie voor de implementatie-opties, de [implementatie](#deployment) sectie. Als u wilt maken van uw eigen sjablonen, de Azure-Containerexemplaren [verwijzing naar het Resource Manager-sjabloon] [ ref] details sjabloon en de beschikbare eigenschappen.

## <a name="sample-templates"></a>Voorbeeldsjablonen

| | |
|-|-|
| **Toepassingen** ||
| [WordPress][app-wp] | Maakt een WordPress-website en de bijbehorende MySQL-database in een exemplaar van de container. De WordPress-site-inhoud en MySQL-database zijn opgeslagen in een Azure-bestanden delen. |
| [MS NAV met SQL Server- en IIS][app-nav] | Implementeert een enkele Windows-container met een volledig functionele zelfstandig Dynamics NAV / Dynamics 365 Business centrale omgeving. |
| **Volumes** ||
| [emptyDir][vol-emptydir] | Twee Linux-containers die delen van een volume emptyDir implementeert. |
| [GitRepo][vol-gitrepo] | Implementeert een Linux-container die een GitHub-opslagplaats kloont en koppelt deze als een volume. |
| [Geheim][vol-secret] | Implementeert een Linux-container met een PFX-certificaat gekoppeld als een geheime volume. |
| **Netwerken** ||
| [UDP-blootgesteld container][net-udp] | Implementeert een Windows- of Linux-container die een UDP-poort beschrijft. |
| [Linux-container met openbare IP-adres][net-publicip] | Implementeert een enkele Linux-container toegankelijk via een openbaar IP-adres. |
| **Azure-resources** ||
| [Azure Storage-account maken en delen van bestanden][az-files] | Maakt gebruik van de Azure CLI in een container-exemplaar maken van een opslagaccount en een Azure-bestanden delen.

## <a name="deployment"></a>Implementatie

U hebt verschillende mogelijkheden voor het implementeren van resources met Resource Manager-sjablonen:

[Azure CLI][deploy-cli]

[Azure PowerShell][deploy-powershell]

[Azure-portal][deploy-portal]

[REST-API][deploy-rest]

<!-- LINKS - External -->
[app-nav]: https://github.com/Azure/azure-quickstart-templates/tree/master/101-aci-dynamicsnav
[app-wp]: https://github.com/Azure/azure-quickstart-templates/tree/master/201-aci-wordpress
[az-files]: https://github.com/Azure/azure-quickstart-templates/tree/master/101-aci-storage-file-share
[net-publicip]: https://github.com/Azure/azure-quickstart-templates/tree/master/101-aci-linuxcontainer-public-ip
[net-udp]: https://github.com/Azure/azure-quickstart-templates/tree/master/201-aci-udp
[repo]: https://github.com/Azure/azure-quickstart-templates
[vol-emptydir]: https://github.com/Azure/azure-quickstart-templates/tree/master/201-aci-linuxcontainer-volume-emptydir
[vol-gitrepo]: https://github.com/Azure/azure-quickstart-templates/tree/master/201-aci-linuxcontainer-volume-gitrepo
[vol-secret]: https://github.com/Azure/azure-quickstart-templates/tree/master/201-aci-linuxcontainer-volume-secret

<!-- LINKS - Internal -->
[deploy-cli]: ../azure-resource-manager/resource-group-template-deploy-cli.md
[deploy-portal]: ../azure-resource-manager/resource-group-template-deploy-portal.md
[deploy-powershell]: ../azure-resource-manager/resource-group-template-deploy.md
[deploy-rest]: ../azure-resource-manager/resource-group-template-deploy-rest.md
[ref]: /azure/templates/microsoft.containerinstance/containergroups