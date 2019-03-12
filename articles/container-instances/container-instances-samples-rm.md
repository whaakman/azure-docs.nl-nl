---
title: Voorbeeldsjablonen van Azure Resource Manager - het Azure Container Instances
description: Voorbeeldsjablonen van Azure Resource Manager voor Azure Container Instances
services: container-instances
author: dlepow
ms.service: container-instances
ms.topic: article
ms.date: 03/07/2019
ms.author: danlep
ms.openlocfilehash: bf9f2be8a0854a6968f3be6bfdaf3a59fc81dc76
ms.sourcegitcommit: 1902adaa68c660bdaac46878ce2dec5473d29275
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/11/2019
ms.locfileid: "57728962"
---
# <a name="azure-resource-manager-templates-for-azure-container-instances"></a>Azure Resource Manager-sjablonen voor Azure Container Instances

De volgende voorbeeldsjablonen implementeren containerinstanties in verschillende configuraties.

Zie voor de implementatie-opties, de [implementatie](#deployment) sectie. Als u wilt maken van uw eigen sjablonen, de Azure Container Instances [verwijzing naar het Resource Manager-sjabloon] [ ref] details sjabloonindeling en beschikbare eigenschappen.

## <a name="sample-templates"></a>Voorbeeldsjablonen

| | |
|-|-|
| **Toepassingen** ||
| [Wordpress][app-wp] | Hiermee maakt een WordPress-website en de MySQL-database in een containerexemplaar. De inhoud van de WordPress-site en MySQL-database zijn opgeslagen op een Azure-bestanden delen. |
| [MS NAV met SQL Server en IIS][app-nav] | Implementeert een enkele Windows-container met een volledig functionele zichzelf Dynamics NAV / Dynamics 365 Business Central-omgeving. |
| **Volumes** ||
| [emptyDir][vol-emptydir] | Twee Linux-containers die delen van een volume emptyDir implementeert. |
| [gitRepo][vol-gitrepo] | Hiermee wordt een Linux-container die een GitHub-opslagplaats te klonen en koppelt deze als een volume geïmplementeerd. |
| [secret][vol-secret] | Hiermee wordt een Linux-container geïmplementeerd met een PFX-certificaat dat is gekoppeld als een geheime volume. |
| **Netwerken** ||
| [UDP-exposed container][net-udp] | Hiermee wordt geïmplementeerd op een Windows- of Linux-container waarin wordt aangegeven dat een UDP-poort. |
| [Linux-containers met openbare IP-adres][net-publicip] | Implementeert een enkele Linux-container toegankelijk via een openbaar IP-adres. |
| [Implementeren van een containergroep met een virtueel netwerk (preview)][net-vnet] | Hiermee wordt een nieuw virtueel netwerk, subnet, netwerkprofiel en containergroep geïmplementeerd. |
| **Azure-resources** ||
| [Azure Storage-account maken en delen van bestanden][az-files] | Maakt gebruik van de Azure CLI in een containerexemplaar om een opslagaccount en een Azure-bestandsshare te maken.

## <a name="deployment"></a>Implementatie

U hebt verschillende mogelijkheden voor het implementeren van resources met Resource Manager-sjablonen:

[Azure CLI][deploy-cli]

[Azure PowerShell][deploy-powershell]

[Azure-portal][deploy-portal]

[REST API][deploy-rest]

<!-- LINKS - External -->
[app-nav]: https://github.com/Azure/azure-quickstart-templates/tree/master/101-aci-dynamicsnav
[app-wp]: https://github.com/Azure/azure-quickstart-templates/tree/master/201-aci-wordpress
[az-files]: https://github.com/Azure/azure-quickstart-templates/tree/master/101-aci-storage-file-share
[net-publicip]: https://github.com/Azure/azure-quickstart-templates/tree/master/101-aci-linuxcontainer-public-ip
[net-udp]: https://github.com/Azure/azure-quickstart-templates/tree/master/201-aci-udp
[net-vnet]: https://github.com/Azure/azure-quickstart-templates/tree/master/101-aci-vnet
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