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
ms.openlocfilehash: 5e287cd29fb305e78fe6338782838929007b17fc
ms.sourcegitcommit: 0e1c4b925c778de4924c4985504a1791b8330c71
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/06/2018
---
# <a name="common-prerequisites-for-deploying-openshift-in-azure"></a>Algemene vereisten voor het implementeren van OpenShift in Azure

Dit artikel worden de algemene vereisten voor de implementatie van de oorsprong OpenShift of OpenShift Container Platform in Azure.

De installatie van OpenShift Ansible hulpmiddelen marketing en verkoop gebruikt. Ansible maakt gebruik van Secure Shell (SSH) verbinding maken met alle clusterhosts om installatiestappen te voltooien.

Wanneer u de SSH-verbinding met de externe hosts start, kunt u niet een wachtwoord invoeren. Daarom de persoonlijke sleutel kan geen een wachtwoord dat is gekoppeld aan of de implementatie mislukt.

Omdat de virtuele machines (VM's) via Azure Resource Manager-sjablonen implementeren, worden dezelfde openbare sleutel wordt gebruikt voor toegang tot alle virtuele machines. U moet de bijbehorende persoonlijke sleutel invoeren in de virtuele machine die wordt uitgevoerd van alle de hulpmiddelen marketing en verkoop ook. U doet dit veilig, kunt u een Azure sleutelkluis gebruiken om door te geven van de persoonlijke sleutel in de virtuele machine.

Als er een hebben om permanente opslag voor containers, zijn permanente volumes zijn vereist. OpenShift Azure virtuele harde schijven (VHD's) voor deze optie ondersteunt, maar Azure moet eerst worden geconfigureerd als de cloudprovider. 

In dit model OpenShift:

- Hiermee maakt een VHD-object in een Azure Storage-account.
- Koppelt de VHD naar een virtuele machine en de indeling van het volume.
- Het volume schil koppelt.

Voor deze configuratie werkt, moet OpenShift machtigingen voor het uitvoeren van de voorgaande taken in Azure. U doen dit met een service-principal. De service-principal is een beveiligingsaccount in Azure Active Directory waaraan machtigingen tot bronnen worden toegekend.

De service-principal moet toegang hebben tot de storage-accounts en virtuele machines die gezamenlijk de cluster. Als alle OpenShift clusterbronnen voor één resourcegroep implementeert, kan de service-principal kunt machtigingen aan die resourcegroep worden verleend.

Deze handleiding beschrijft het maken van de artefacten die zijn gekoppeld aan de vereisten.

> [!div class="checklist"]
> * Een sleutelkluis voor het beheren van SSH-sleutels voor het cluster OpenShift maken.
> * Maken van een service-principal voor gebruik door Azure Cloud Solution Provider.

Als u nog geen abonnement op Azure hebt, maak dan een [gratis account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) aan voordat u begint.

## <a name="sign-in-to-azure"></a>Aanmelden bij Azure 
Aanmelden bij uw Azure-abonnement met de [az aanmelding](/cli/azure/#login) opdracht in en volg de op het scherm richtingen, of klik op **Try it** Cloud Shell gebruiken.

```azurecli 
az login
```
## <a name="create-a-resource-group"></a>Een resourcegroep maken

Een resourcegroep maken met de opdracht [az group create](/cli/azure/group#create). Een Azure-resourcegroep is een logische container waarin Azure-resources worden geïmplementeerd en beheerd. U kunt een specifieke resourcegroep gebruiken voor het hosten van de sleutelkluis. Deze groep is gescheiden van de resourcegroep waarin de clusterbronnen OpenShift implementeren. 

Het volgende voorbeeld wordt een resourcegroep met de naam *keyvaultrg* in de *eastus* locatie:

```azurecli 
az group create --name keyvaultrg --location eastus
```

## <a name="create-a-key-vault"></a>Een sleutelkluis maken
Maken van een sleutelkluis voor het opslaan van de SSH-sleutels voor het cluster met de [az keyvault maken](/cli/azure/keyvault#create) opdracht. De naam van de sleutelkluis moet wereldwijd uniek zijn.

Het volgende voorbeeld wordt een sleutelkluis met de naam *keyvault* in de *keyvaultrg* resourcegroep:

```azurecli 
az keyvault create --resource-group keyvaultrg --name keyvault \
       --enabled-for-template-deployment true \
       --location eastus
```

## <a name="create-an-ssh-key"></a>Een SSH-sleutel maken 
Een SSH-sleutel is vereist voor het beveiligen van toegang tot het cluster OpenShift oorsprong. Maken van een SSH-sleutelpaar met behulp van de `ssh-keygen` opdracht (op Linux- of Mac OS):
 
 ```bash
ssh-keygen -f ~/.ssh/openshift_rsa -t rsa -N ''
```

> [!NOTE]
> Een wachtwoord mag niet zijn door uw SSH-sleutelpaar.

Zie voor meer informatie over SSH-sleutels op Windows [het maken van SSH-sleutels op Windows](/azure/virtual-machines/linux/ssh-from-windows).

## <a name="store-the-ssh-private-key-in-azure-key-vault"></a>De persoonlijke SSH-sleutel opslaan in Azure Sleutelkluis
De implementatie OpenShift maakt gebruik van de SSH-sleutel die u hebt gemaakt voor veilige toegang tot de OpenShift-master. Om de implementatie van de SSH-sleutel veilig worden opgehaald, moet u de sleutel opslaan in de Sleutelkluis met behulp van de volgende opdracht:

```azurecli
az keyvault secret set --vault-name keyvault --name keysecret --file ~/.ssh/openshift_rsa
```

## <a name="create-a-service-principal"></a>Een service-principal maken 
OpenShift communiceert met Azure met behulp van een gebruikersnaam en wachtwoord of een service-principal. Een Azure-service-principal is een beveiligings-id die u met apps, services en automatiseringsprogramma's zoals OpenShift gebruiken kunt. U de machtigingen over welke bewerkingen door de service-principal in Azure uitvoeren kunt te definiëren en beheren. Voor een betere beveiliging dan gewoon een gebruikersnaam en wachtwoord voor het bieden dit voorbeeld wordt een basic service principal.

Maken van een service principal met [az ad sp maken-voor-rbac](/cli/azure/ad/sp#create-for-rbac) en de uitvoer van de referenties die OpenShift nodig heeft.

Het volgende voorbeeld maakt een service principal en Inzender-rechten toegewezen aan een resourcegroep met de naam myResourceGroup. Als u van Windows gebruikmaakt, uitvoermachtigingen ```az group show --name myResourceGroup --query id``` afzonderlijk en met de uitvoer van de feed de--optie scopes.

```azurecli
az ad sp create-for-rbac --name openshiftsp \
          --role Contributor --password {Strong Password} \
          --scopes $(az group show --name myResourceGroup --query id)
```

Noteer de appId-eigenschap geretourneerd van de opdracht:
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

Zie voor meer informatie over service-principals [een Azure-service-principal maken met Azure CLI 2.0](https://docs.microsoft.com/cli/azure/create-an-azure-service-principal-azure-cli?view=azure-cli-latest).

## <a name="next-steps"></a>Volgende stappen

In dit artikel komen de volgende onderwerpen:
> [!div class="checklist"]
> * Een sleutelkluis voor het beheren van SSH-sleutels voor het cluster OpenShift maken.
> * Maken van een service-principal voor gebruik door Azure Cloud Solution Provider.

Vervolgens implementeert u een cluster OpenShift:

- [OpenShift oorsprong implementeren](./openshift-origin.md)
- [OpenShift Container Platform implementeren](./openshift-container-platform.md)

