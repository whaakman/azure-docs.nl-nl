---
title: Implementeren van OpenShift in Azure Stack | Microsoft Docs
description: Implementeren van OpenShift in Azure Stack.
services: virtual-machines-linux
documentationcenter: virtual-machines
author: haroldwongms
manager: joraio
editor: ''
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: ''
ms.author: haroldw
ms.openlocfilehash: a2a61015f82cc27dcadf96fbd608e2d3420218ee
ms.sourcegitcommit: 5de9de61a6ba33236caabb7d61bee69d57799142
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/25/2018
ms.locfileid: "50088832"
---
# <a name="deploy-openshift-container-platform-or-okd-in-azure-stack"></a>OpenShift Containerplatform of OKD in Azure Stack implementeren

OpenShift kan worden geïmplementeerd in Azure Stack. Er zijn enkele belangrijke verschillen tussen Azure en Azure Stack, zodat de implementatie wordt enigszins verschillen en mogelijkheden wordt ook enigszins verschillen.

De Azure-Cloud-Provider werkt niet op dit moment in Azure Stack. Daarom kunt u zich niet op de schijf koppelen voor permanente opslag in Azure Stack. In plaats daarvan kunt u andere opties voor opslag, zoals NFS, iSCSI, GlusterFS, enzovoort. U kunt als alternatief kunt CNS inschakelen en gebruiken van GlusterFS voor permanente opslag. Als CNS is ingeschakeld, wordt met extra opslagruimte voor het gebruik van GlusterFS drie extra knooppunten geïmplementeerd.

U kunt een van de verschillende methoden gebruiken OpenShift Container Platform of OKD in Azure Stack implementeren:

- U kunt handmatig implementeren van de onderdelen die nodig zijn Azure-infrastructuur en voer vervolgens de [documentatie voor OpenShift Container Platform](https://docs.openshift.com/container-platform) of [OKD documentatie](https://docs.okd.io).
- U kunt ook een bestaande [Resource Manager-sjabloon](https://github.com/Microsoft/openshift-container-platform/) die vereenvoudigt de implementatie van het cluster OpenShift Container Platform.
- U kunt ook een bestaande [Resource Manager-sjabloon](https://github.com/Microsoft/openshift-origin) die de implementatie van het cluster OKD vereenvoudigt.

Als de Resource Manager-sjabloon gebruikt, selecteert u de juiste vertakking (azurestack-release-3.x). De sjablonen voor Azure werkt niet omdat de API-versies verschillen tussen Azure en Azure Stack zijn. De verwijzing naar de installatiekopie van de RHEL is momenteel vastgelegd als een variabele in het bestand azuredeploy.json en moet worden gewijzigd zodat deze overeenkomt met uw installatiekopie.

```json
"imageReference": {
    "publisher": "Redhat",
    "offer": "RHEL-OCP",
    "sku": "7-4",
    "version": "latest"
}
```

Een Red Hat-abonnement is vereist voor alle opties. De Red Hat Enterprise Linux-exemplaar is tijdens de implementatie aan het Red Hat-abonnement is geregistreerd en die is gekoppeld aan de groeps-ID die de rechten voor OpenShift Container Platform bevat.
Zorg ervoor dat u hebt een geldige gebruikersnaam, wachtwoord en groep-ID in Red Hat abonnement Manager (RHSM) U kunt ook kunt u een activeringscode, organisatie-ID en groep-ID.  U kunt deze informatie controleren door aan te melden bij https://access.redhat.com.

## <a name="azure-stack-prerequisites"></a>Vereisten voor Azure Stack

Een RHEL-installatiekopie (OpenShift Container Platform) of CentOS-installatiekopie (OKD) moet worden toegevoegd aan uw Azure Stack-omgeving om een OpenShift-cluster te implementeren. Neem contact op met uw Azure Stack-beheerder om toe te voegen deze installatiekopieën. Instructies vindt u hier:

- https://docs.microsoft.com/en-us/azure/azure-stack/azure-stack-add-vm-image
- https://docs.microsoft.com/en-us/azure/azure-stack/azure-stack-marketplace-azure-items
- https://docs.microsoft.com/en-us/azure/azure-stack/azure-stack-redhat-create-upload-vhd

## <a name="deploy-by-using-the-openshift-container-platform-or-okd-resource-manager-template"></a>Implementeren met behulp van de OpenShift Container Platform of OKD Resource Manager-sjabloon

Als u wilt implementeren met behulp van de Resource Manager-sjabloon, kunt u een parameterbestand gebruiken om op te geven van de invoerparameters. Voor het verder aanpassen van de implementatie, de GitHub-opslagplaats splitst en de juiste items wijzigen.

Sommige algemene opties voor het aanpassen bevatten, maar niet beperkt tot:

- Bastionhost VM-grootte (variabele in azuredeploy.json)
- Naamconventies (variabelen in azuredeploy.json)
- OpenShift-cluster-specifieke instellingen, worden gewijzigd via het hosts-bestand (deployOpenShift.sh)
- Verwijzing naar afbeelding RHEL (variabele in azuredeploy.json)

Voor de stappen voor het implementeren met behulp van de Azure CLI, volgt u de juiste sectie in de [OpenShift Container Platform](./openshift-container-platform.md) sectie of de [OKD](./openshift-okd.md) sectie.

## <a name="next-steps"></a>Volgende stappen

- [Taken na de implementatie](./openshift-post-deployment.md)
- [OpenShift-implementatie in Azure oplossen](./openshift-troubleshooting.md)