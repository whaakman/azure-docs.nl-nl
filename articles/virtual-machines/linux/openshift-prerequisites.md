---
title: OpenShift in Azure-vereisten | Microsoft Docs
description: Vereisten voor het implementeren van OpenShift in Azure.
services: virtual-machines-linux
documentationcenter: virtual-machines
author: haroldw
manager: najoshi
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 
ms.author: haroldw
ms.openlocfilehash: 0c90b8a6d17fa293b6708d942afd35e1333623cb
ms.sourcegitcommit: b979d446ccbe0224109f71b3948d6235eb04a967
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/25/2017
---
# <a name="common-prerequisites-for-openshift-in-azure"></a>Algemene vereisten voor OpenShift in Azure

Wanneer u OpenShift in Azure implementeert, zijn er enkele algemene vereisten ongeacht of u OpenShift oorsprong of OpenShift Container Platform implementeert.

De installatie van OpenShift wordt uitgevoerd via Ansible hulpmiddelen marketing en verkoop. SSH Ansible gebruikt om verbinding met alle hosts die onderdeel van het cluster zijn zal om installatiestappen te voltooien.
Wanneer de SSH-verbinding met de externe hosts is gestart, is er geen manier om in te voeren een wachtwoordzin. Om deze reden, de persoonlijke sleutel een wachtwoordzin gekoppeld kunnen geen of mislukt de implementatie.
Aangezien de virtuele machines zijn geïmplementeerd via Resource Manager-sjablonen, worden dezelfde openbare sleutel wordt gebruikt voor toegang tot alle virtuele machines. We moeten invoeren van de bijbehorende persoonlijke sleutel in de virtuele machine die wordt uitgevoerd op alle de hulpmiddelen marketing en verkoop ook.
Om u te doen dit veilig, gebruiken we een Azure Key Vault om door te geven van de persoonlijke sleutel in de virtuele machine.

Als er een hebben om permanente opslag voor containers, zijn permanente Volumes (HW) nodig. Deze PV's moeten worden gedekt door een vorm van de permanente opslag. Azure-schijven (VHD's) voor deze optie biedt ondersteuning voor OpenShift maar Azure moet eerst worden geconfigureerd als de Cloudprovider. In dit model wordt OpenShift:

- Een VHD-object in een Azure Storage-Account maken
- Koppelen van de VHD op een virtuele machine en het volume formatteren
- Het volume aan de schil koppelen

Dit werkt, moet OpenShift machtigingen voor het uitvoeren van de voorgaande taken in Azure. Om dit te bereiken, is een Service-Principal vereist. De Service Principal (SP) is een beveiligingsaccount in Azure Active Directory waaraan machtigingen tot bronnen worden toegekend.
De Service-Principal moet toegang hebben tot de Storage-Accounts en virtuele machines die gezamenlijk de cluster. Als alle OpenShift-clusterbronnen worden geïmplementeerd voor een enkele resourcegroep, kan de Serviceprovider machtigingen aan deze resourcegroep.

Deze handleiding beschrijft het maken van de artefacten die zijn gekoppeld aan de vereisten.

> [!div class="checklist"]
> * Maak een KeyVault voor het beheren van SSH-sleutels voor het cluster OpenShift.
> * Maken van een Service-Principal voor gebruik door de Azure-Cloud-Provider.

Als u nog geen abonnement op Azure hebt, maak dan een [gratis account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) aan voordat u begint.

## <a name="log-in-to-azure"></a>Meld u aan bij Azure. 
Aanmelden bij uw Azure-abonnement met de [az aanmelding](/cli/azure/#login) opdracht in en volg de op het scherm richtingen of klik op **Try it** Cloud Shell gebruiken.

```azurecli 
az login
```
## <a name="create-a-resource-group"></a>Een resourcegroep maken

Een resourcegroep maken met de opdracht [az group create](/cli/azure/group#create). Een Azure-resourcegroep is een logische container waarin Azure-resources worden geïmplementeerd en beheerd. Het verdient aanbeveling dat een specifieke resourcegroep wordt gebruikt voor het hosten van de Sleutelkluis - gescheiden van de resourcegroep die de clusterbronnen OpenShift worden geïmplementeerd in. 

Het volgende voorbeeld wordt een resourcegroep met de naam *keyvaultrg* in de *eastus* locatie.

```azurecli 
az group create --name keyvaultrg --location eastus
```

## <a name="create-a-key-vault"></a>Een sleutelkluis maken
Maken van een KeyVault voor het opslaan van de SSH-sleutels voor het cluster met de [az keyvault maken](/cli/azure/keyvault#create) opdracht. De naam van de Sleutelkluis moet wereldwijd uniek zijn.

Het volgende voorbeeld wordt een keyvault met de naam *keyvault* in de *keyvaultrg* resourcegroep.

```azurecli 
az keyvault create --resource-group keyvaultrg --name keyvault \
       --enabled-for-template-deployment true \
       --location eastus
```

## <a name="create-an-ssh-key"></a>Een SSH-sleutel maken 
Een SSH-sleutel is vereist voor het beveiligen van toegang tot het cluster OpenShift oorsprong. Maakt een SSH-sleutelpaar met behulp van de `ssh-keygen` opdracht (op Linux- of Mac).
 
 ```bash
ssh-keygen -f ~/.ssh/openshift_rsa -t rsa -N ''
```

> [!NOTE]
> Het SSH-sleutelpaar, die hebt u geen een wachtwoordzin.

Voor meer informatie over de SSH-sleutels op Windows, [het maken van SSH-sleutels op Windows](/azure/virtual-machines/linux/ssh-from-windows).

## <a name="store-ssh-private-key-in-key-vault"></a>Opslaan van persoonlijke SSH-sleutel in de sleutelkluis
De implementatie OpenShift maakt gebruik van de SSH-sleutel die u hebt gemaakt voor veilige toegang tot de OpenShift-master. Zodat de implementatie voor het ophalen van de SSH-sleutel veilig opslaan van de sleutel in de Sleutelkluis met de volgende opdracht:

```azurecli
az keyvault secret set --vault-name keyvault --name keysecret --file ~/.ssh/openshift.rsa
```

## <a name="create-a-service-principal"></a>Een service-principal maken 
OpenShift communiceert met Azure met behulp van een gebruikersnaam en wachtwoord of een service-principal. Een Azure-service-principal is een beveiligings-id die u met apps, services en automatiseringsprogramma's zoals OpenShift gebruiken kunt. U de machtigingen over welke bewerkingen die de service-principal in Azure uitvoeren kunt te definiëren en beheren. Voor betere beveiliging via gewoon een gebruikersnaam en wachtwoord voor het bieden, in dit voorbeeld wordt een basic service principal.

Maken van een service principal met [az ad sp maken-voor-rbac](/cli/azure/ad/sp#create-for-rbac) en de uitvoer van de referenties die OpenShift nodig heeft.

Het volgende voorbeeld maakt een service principal en Inzender-rechten toegewezen aan een resourcegroep met de naam myResourceGroup. Als het gebruik van Windows uitvoeren ```az group show --name myResourceGroup --query id``` afzonderlijk en met de uitvoer van de feed de--optie scopes.

```azurecli
az ad sp create-for-rbac --name openshiftsp \
          --role Contributor --password {Strong Password} \
          --scopes $(az group show --name myResourceGroup --query id)
```

Let op de toepassings-id-eigenschap geretourneerd van de opdracht.
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
 > Maak geen onbeveiligd wachtwoord.  Volg de richtlijnen in [Azure AD password rules and restrictions](/azure/active-directory/active-directory-passwords-policy) (Regels en beperkingen voor wachtwoorden in Azure AD).

Zie voor meer informatie over service-principals [een Azure-service-principal maken met Azure CLI 2.0](/cli/azure/create-an-azure-service-principal-azure-cli)

## <a name="next-steps"></a>Volgende stappen

In dit artikel komen de volgende onderwerpen:
> [!div class="checklist"]
> * Maak een KeyVault voor het beheren van SSH-sleutels voor het cluster OpenShift.
> * Maken van een Service-Principal voor gebruik door de Azure-Cloud-Provider.

Nu gaat u een cluster OpenShift implementeren

- [OpenShift oorsprong implementeren](./openshift-origin.md)
- [OpenShift Container Platform implementeren](./openshift-container-platform.md)

