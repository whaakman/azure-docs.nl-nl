---
title: OpenShift in vereisten voor Azure | Microsoft Docs
description: Vereisten voor het implementeren van OpenShift in Azure.
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
ms.openlocfilehash: 25ec82c923ebe322194d868159332ef145727999
ms.sourcegitcommit: a65b424bdfa019a42f36f1ce7eee9844e493f293
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/04/2019
ms.locfileid: "55692988"
---
# <a name="common-prerequisites-for-deploying-openshift-in-azure"></a>Algemene vereisten voor het implementeren van OpenShift in Azure

Dit artikel worden algemene vereisten voor het implementeren van OpenShift Container Platform of OKD in Azure.

De installatie van OpenShift Ansible-playbooks gebruikt. Ansible maakt gebruik van Secure Shell (SSH) verbinding maken met alle clusterhosts stappen van de installatie te voltooien.

Wanneer u ansible initieert de SSH-verbinding met de externe hosts, niet kan deze een wachtwoord opgeven. Om deze reden, de persoonlijke sleutel kan geen een wachtwoord (wachtwoordzin) die zijn gekoppeld aan het of de implementatie mislukt.

Omdat de virtuele machines (VM's) via Azure Resource Manager-sjablonen implementeren, wordt dezelfde openbare sleutel wordt gebruikt voor toegang tot alle virtuele machines. U moet de bijbehorende persoonlijke sleutel invoeren in de virtuele machine die wordt uitgevoerd ook alle playbooks. Om dit te doen veilig, kunt u een Azure key vault gebruiken om door te geven van de persoonlijke sleutel in de virtuele machine.

Als er een nodig voor permanente opslag voor containers, klikt u vervolgens zijn persistent volumes vereist. OpenShift ondersteunt Azure virtuele harde schijven (VHD's) voor deze mogelijkheid, maar Azure moet eerst worden geconfigureerd als de cloudprovider.

In dit model OpenShift:

- Hiermee maakt u een VHD-object in een Azure Storage-account of een beheerde schijf.
- Hiermee koppelt u de VHD naar een virtuele machine en het volume geformatteerd.
- Hiermee wordt het volume voor de schil gekoppeld.

Voor deze configuratie werkt, moet OpenShift toegangsmachtigingen voor deze taken kunnen uitvoeren in Azure. U doen dit met een service-principal. De service-principal is een security-account in Azure Active Directory waaraan machtigingen voor resources is verleend.

De service-principal moet toegang hebben tot de storage-accounts en virtuele machines die gezamenlijk het cluster. Als alle OpenShift-clusterresources voor een enkele resourcegroep implementeert, kan de service-principal kan machtigingen in deze resourcegroep worden verleend.

Deze handleiding beschrijft het maken van de artefacten die zijn gekoppeld aan de vereisten.

> [!div class="checklist"]
> * Maak een key vault voor het beheren van SSH-sleutels voor het cluster OpenShift.
> * Maak een service-principal voor gebruik door de Azure-Cloud-Provider.

Als u nog geen abonnement op Azure hebt, maak dan een [gratis account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) aan voordat u begint.

## <a name="sign-in-to-azure"></a>Aanmelden bij Azure 
Aanmelden bij uw Azure-abonnement met de [az login](/cli/azure/reference-index) opdracht en volgt u de op het scherm richtingen, of klik op **uitproberen** Cloud Shell gebruiken.

```azurecli 
az login
```
## <a name="create-a-resource-group"></a>Een resourcegroep maken

Een resourcegroep maken met de opdracht [az group create](/cli/azure/group). Een Azure-resourcegroep is een logische container waarin Azure-resources worden geïmplementeerd en beheerd. Het verdient een speciale resourcegroep gebruiken voor het hosten van de key vault. Deze groep is gescheiden van de resourcegroep waarin de clusterbronnen OpenShift implementeren.

Het volgende voorbeeld wordt een resourcegroep met de naam *keyvaultrg* in de *eastus* locatie:

```azurecli 
az group create --name keyvaultrg --location eastus
```

## <a name="create-a-key-vault"></a>Een sleutelkluis maken
Maken van een key vault voor het opslaan van de SSH-sleutels voor het cluster met de [az keyvault maken](/cli/azure/keyvault) opdracht. De key vault-naam moet wereldwijd uniek zijn.

Het volgende voorbeeld wordt een key vault met de naam *keyvault* in de *keyvaultrg* resourcegroep:

```azurecli 
az keyvault create --resource-group keyvaultrg --name keyvault \
       --enabled-for-template-deployment true \
       --location eastus
```

## <a name="create-an-ssh-key"></a>Een SSH-sleutel maken 
Een SSH-sleutel is vereist voor het beveiligen van toegang tot het cluster OpenShift. Een SSH-sleutelpaar maken met behulp van de `ssh-keygen` opdracht (op Linux of macOS):
 
 ```bash
ssh-keygen -f ~/.ssh/openshift_rsa -t rsa -N ''
```

> [!NOTE]
> Het SSH-sleutelpaar kunt geen een wachtwoord / wachtwoordzin.

Zie voor meer informatie over SSH-sleutels op Windows [over het maken van SSH-sleutels op Windows](/azure/virtual-machines/linux/ssh-from-windows). Zorg ervoor dat de persoonlijke sleutel in OpenSSH-indeling exporteren.

## <a name="store-the-ssh-private-key-in-azure-key-vault"></a>De persoonlijke SSH-sleutel in Azure Key Vault Store
De implementatie van OpenShift maakt gebruik van de SSH-sleutel die u hebt gemaakt voor beveiligde toegang tot de OpenShift-master. Opslaan als wilt inschakelen in de implementatie van de SSH-sleutel veilig worden opgehaald, de sleutel in Key Vault met behulp van de volgende opdracht uit:

```azurecli
az keyvault secret set --vault-name keyvault --name keysecret --file ~/.ssh/openshift_rsa
```

## <a name="create-a-service-principal"></a>Een service-principal maken 
OpenShift communiceert met Azure met behulp van een gebruikersnaam en wachtwoord of een service-principal. Een Azure service-principal is een beveiligings-id die u met apps, services en automatiseringsprogramma's, zoals OpenShift gebruiken kunt. U bepaalt en definiëren van de machtigingen over welke bewerkingen de service-principal in Azure kunt uitvoeren. Het is raadzaam om het bereik van de machtigingen van de service-principal aan specifieke resourcegroepen in plaats van het hele abonnement te.

Een service-principal met maken [az ad sp create-for-rbac](/cli/azure/ad/sp) en de uitvoer van de referenties die OpenShift nodig heeft.

Het volgende voorbeeld maakt u een service principal en Inzender-rechten toegewezen aan een resourcegroep met de naam openshiftrg.

Maak eerst de resourcegroep met de naam openshiftrg:

```azurecli
az group create -l eastus -n openshiftrg
```

Service-principal maken:

```azurecli
scope=`az group show --name openshiftrg --query id`
az ad sp create-for-rbac --name openshiftsp \
      --role Contributor --password {Strong Password} \
      --scopes $scope
```
Als u Windows gebruikt, voer ```az group show --name openshiftrg --query id``` en de uitvoer in plaats van $scope gebruiken.

Noteer de toepassings-id-eigenschap geretourneerd door de opdracht:
```json
{
  "appId": "11111111-abcd-1234-efgh-111111111111",
  "displayName": "openshiftsp",
  "name": "http://openshiftsp",
  "password": {Strong Password},
  "tenant": "XXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXXXXX"
}
```
 > [!WARNING] 
 > Zorg ervoor dat een beveiligd wachtwoord maken. Volg de richtlijnen in [Azure AD password rules and restrictions](/azure/active-directory/active-directory-passwords-policy) (Regels en beperkingen voor wachtwoorden in Azure AD).

Zie voor meer informatie over service-principals [een Azure service-principal maken met Azure CLI](https://docs.microsoft.com/cli/azure/create-an-azure-service-principal-azure-cli?view=azure-cli-latest).

## <a name="next-steps"></a>Volgende stappen

In dit artikel komen de volgende onderwerpen aan bod:
> [!div class="checklist"]
> * Maak een key vault voor het beheren van SSH-sleutels voor het cluster OpenShift.
> * Maak een service-principal voor gebruik door de Azure Cloud Solution Provider.

Vervolgens implementeert u een OpenShift-cluster:

- [Implementeren van OpenShift Containerplatform](./openshift-container-platform.md)
- [OKD implementeren](./openshift-okd.md)
