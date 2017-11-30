---
title: Een webserver met SSL-certificaten in Azure beveiligen | Microsoft Docs
description: Meer informatie over het beveiligen van de NGINX-webserver met SSL-certificaten op een Linux VM in Azure
services: virtual-machines-linux
documentationcenter: virtual-machines
author: iainfoulds
manager: timlt
editor: tysonn
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: tutorial
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 07/17/2017
ms.author: iainfou
ms.custom: mvc
ms.openlocfilehash: d2d6a0b00704e1d97be9a4c5bd00ba37374419e5
ms.sourcegitcommit: 29bac59f1d62f38740b60274cb4912816ee775ea
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/29/2017
---
# <a name="secure-a-web-server-with-ssl-certificates-on-a-linux-virtual-machine-in-azure"></a>Een webserver met SSL-certificaten op een virtuele Linux-machine in Azure beveiligen
Beveiligde webservers, kan een certificaat Later SSL (Secure Sockets) worden gebruikt voor het versleutelen van internetverkeer. Deze SSL-certificaten kunnen worden opgeslagen in Azure Sleutelkluis en beveiligde implementaties van certificaten aan virtuele Linux-machines (VM's) in Azure toestaan. In deze zelfstudie leert u het volgende:

> [!div class="checklist"]
> * Maken van een Azure Sleutelkluis
> * Genereren of een certificaat uploaden naar de Sleutelkluis
> * Een virtuele machine maken en de NGINX-webserver installeren
> * Het certificaat invoeren in de virtuele machine en NGINX configureren met een SSL-binding

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

Als u wilt installeren en gebruiken van de CLI lokaal, in deze zelfstudie vereist dat u de Azure CLI versie 2.0.4 zijn uitgevoerd of hoger. Voer `az --version` uit om de versie te bekijken. Als u Azure CLI 2.0 wilt installeren of upgraden, raadpleegt u [Azure CLI 2.0 installeren]( /cli/azure/install-azure-cli).  


## <a name="overview"></a>Overzicht
Azure Sleutelkluis beschermt cryptografische sleutels en geheimen, zoals certificaten of wachtwoorden. Sleutelkluis helpt het beheerproces van het certificaat te stroomlijnen en kunt u controle houdt over sleutels die toegang hebben tot deze certificaten. U kunt een zelfondertekend certificaat in de Sleutelkluis maken of een bestaande, vertrouwd certificaat dat u al bezit uploaden.

In plaats van met een aangepaste VM-installatiekopie die certificaten bevat standaard uitbreidbaar module u certificaten kunt invoeren in een actieve virtuele machine. Dit proces zorgt ervoor dat de meest recente certificaten tijdens de implementatie op een webserver zijn geïnstalleerd. Als u vernieuwen of vervangen van een certificaat, hebt u niet ook een nieuwe aangepaste VM-installatiekopie maken. De meest recente certificaten worden automatisch toegevoegd als u extra virtuele machines maken. Tijdens het hele proces, de certificaten nooit laat u de Azure-platform of beschikbaar worden gesteld in een script, opdrachtregel geschiedenis of sjabloon.


## <a name="create-an-azure-key-vault"></a>Maken van een Azure Sleutelkluis
Voordat u certificaten en een Sleutelkluis maken kunt, maakt u een resourcegroep met [az groep maken](/cli/azure/group#create). Het volgende voorbeeld wordt een resourcegroep met de naam *myResourceGroupSecureWeb* in de *eastus* locatie:

```azurecli-interactive 
az group create --name myResourceGroupSecureWeb --location eastus
```

Maak vervolgens een Sleutelkluis met [az keyvault maken](/cli/azure/keyvault#create) en inschakelen voor gebruik wanneer u een virtuele machine implementeert. Elke Sleutelkluis moet een unieke naam hebben en moet alle kleine letters. Vervang  *<mykeyvault>*  in het volgende voorbeeld met de naam van uw eigen unieke Sleutelkluis:

```azurecli-interactive 
keyvault_name=<mykeyvault>
az keyvault create \
    --resource-group myResourceGroupSecureWeb \
    --name $keyvault_name \
    --enabled-for-deployment
```

## <a name="generate-a-certificate-and-store-in-key-vault"></a>Een certificaat genereren en opslaan in de Sleutelkluis
Voor gebruik in productieomgevingen, moet u een geldig certificaat dat is ondertekend door een vertrouwde provider met importeren [az keyvault certificaat importeren](/cli/azure/keyvault/certificate#az_keyvault_certificate_import). Voor deze zelfstudie, het volgende voorbeeld toont hoe kunt u een zelfondertekend certificaat met genereren [az keyvault-certificaat maken](/cli/azure/keyvault/certificate#az_keyvault_certificate_create) die gebruikmaakt van het standaardbeleid voor certificaat:

```azurecli-interactive 
az keyvault certificate create \
    --vault-name $keyvault_name \
    --name mycert \
    --policy "$(az keyvault certificate get-default-policy)"
```

### <a name="prepare-a-certificate-for-use-with-a-vm"></a>Een certificaat voor gebruik met een virtuele machine voorbereiden
Het certificaat te gebruiken tijdens de virtuele machine proces voor het maken, verkrijgen van de ID van het certificaat met [az keyvault lijst-versies van het clientgeheim](/cli/azure/keyvault/secret#list-versions). Converteren van het certificaat met [az vm indeling-geheim](/cli/azure/vm#format-secret). Het volgende voorbeeld wordt de uitvoer van deze opdrachten naar variabelen voor eenvoudig te gebruiken in de volgende stappen:

```azurecli-interactive 
secret=$(az keyvault secret list-versions \
          --vault-name $keyvault_name \
          --name mycert \
          --query "[?attributes.enabled].id" --output tsv)
vm_secret=$(az vm format-secret --secret "$secret")
```

### <a name="create-a-cloud-init-config-to-secure-nginx"></a>Maken van een cloud-init-config NGINX beveiligen
[Cloud-init](https://cloudinit.readthedocs.io) is een veelgebruikte benadering voor het aanpassen van een Linux-VM als deze voor de eerste keer wordt opgestart. U kunt cloud init gebruiken voor het installeren van pakketten en bestanden schrijven of om gebruikers en beveiliging te configureren. Als de initialisatie van de cloud wordt uitgevoerd tijdens het opstartproces, zijn er geen extra stappen of vereist agents naar uw configuratie toe te passen.

Wanneer u maakt een virtuele machine, certificaten en sleutels worden opgeslagen in de beveiligde */var/lib/waagent/* directory. Gebruik cloud init voor het automatiseren van het certificaat toevoegen aan de virtuele machine en de webserver configureren. In dit voorbeeld we installeren en configureren van de NGINX-webserver. U kunt hetzelfde proces gebruiken voor het installeren en configureren van Apache. 

Maak een bestand met de naam *cloud-init-web-server.txt* en plak de volgende configuratie:

```yaml
#cloud-config
package_upgrade: true
packages:
  - nginx
write_files:
  - owner: www-data:www-data
  - path: /etc/nginx/sites-available/default
    content: |
      server {
        listen 443 ssl;
        ssl_certificate /etc/nginx/ssl/mycert.cert;
        ssl_certificate_key /etc/nginx/ssl/mycert.prv;
      }
runcmd:
  - secretsname=$(find /var/lib/waagent/ -name "*.prv" | cut -c -57)
  - mkdir /etc/nginx/ssl
  - cp $secretsname.crt /etc/nginx/ssl/mycert.cert
  - cp $secretsname.prv /etc/nginx/ssl/mycert.prv
  - service nginx restart
```

### <a name="create-a-secure-vm"></a>Een beveiligde virtuele machine maken
Maak nu een virtuele machine met [az vm maken](/cli/azure/vm#create). De gegevens van het certificaat wordt geïnjecteerd uit de Sleutelkluis met de `--secrets` parameter. U kunt doorgeven in de cloud init-configuratie met de `--custom-data` parameter:

```azurecli-interactive 
az vm create \
    --resource-group myResourceGroupSecureWeb \
    --name myVM \
    --image UbuntuLTS \
    --admin-username azureuser \
    --generate-ssh-keys \
    --custom-data cloud-init-web-server.txt \
    --secrets "$vm_secret"
```

Het duurt enkele minuten duren voordat de virtuele machine moet worden gemaakt, de pakketten te installeren en de app te starten. Wanneer de virtuele machine is gemaakt, dient u de `publicIpAddress` weergegeven door de Azure CLI. Dit adres wordt gebruikt voor toegang tot uw site in een webbrowser.

Zodat beveiligde webverkeer bereiken van uw virtuele machine poort 443 openen vanaf het Internet met [az vm open poort](/cli/azure/vm#open-port):

```azurecli-interactive 
az vm open-port \
    --resource-group myResourceGroupSecureWeb \
    --name myVM \
    --port 443
```


### <a name="test-the-secure-web-app"></a>Testen van de beveiligde web-app
Nu kunt u een webbrowser openen en voer *https://<publicIpAddress>*  in de adresbalk. Geef uw eigen openbare IP-adres van de virtuele machine proces maken. De beveiligingswaarschuwing accepteren als u een zelfondertekend certificaat gebruikt:

![Beveiligingswaarschuwing voor web browser accepteren](./media/tutorial-secure-web-server/browser-warning.png)

Uw beveiligde NGINX-site wordt vervolgens weergegeven zoals in het volgende voorbeeld:

![Actief beveiligde NGINX-site weergeven](./media/tutorial-secure-web-server/secured-nginx.png)


## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie maakt beveiligd u een NGINX-webserver met een SSL-certificaat dat is opgeslagen in Azure Sleutelkluis. U hebt geleerd hoe u:

> [!div class="checklist"]
> * Maken van een Azure Sleutelkluis
> * Genereren of een certificaat uploaden naar de Sleutelkluis
> * Een virtuele machine maken en de NGINX-webserver installeren
> * Het certificaat invoeren in de virtuele machine en NGINX configureren met een SSL-binding

Volg deze link om te zien van de vooraf gemaakte virtuele machine scriptvoorbeelden.

> [!div class="nextstepaction"]
> [Windows virtuele machine scriptvoorbeelden](./cli-samples.md)