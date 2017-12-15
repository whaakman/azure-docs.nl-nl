---
title: Aanpassen van een Linux-VM voor eerst wordt opgestart in Azure | Microsoft Docs
description: Meer informatie over het cloud-init en Sleutelkluis te gebruiken customze Linux VM's de eerste keer dat ze worden opgestart in Azure
services: virtual-machines-linux
documentationcenter: virtual-machines
author: iainfoulds
manager: jeconnoc
editor: tysonn
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: tutorial
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 12/13/2017
ms.author: iainfou
ms.custom: mvc
ms.openlocfilehash: 83773e513ee2c92da733df05cd17dda2940a28cd
ms.sourcegitcommit: 0e4491b7fdd9ca4408d5f2d41be42a09164db775
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/14/2017
---
# <a name="how-to-customize-a-linux-virtual-machine-on-first-boot"></a>Het aanpassen van een virtuele Linux-machine op de eerste keer opstarten
In een vorige zelfstudie hebt u geleerd hoe u voor SSH aan een virtuele machine (VM) en handmatig installeren NGINX. Voor het maken van virtuele machines op een snelle en consistente manier, is een vorm van automatisering doorgaans gewenst. Een gemeenschappelijke aanpak voor het aanpassen van een virtuele machine op de eerste keer opstarten is het gebruik [cloud init](https://cloudinit.readthedocs.io). In deze zelfstudie leert u het volgende:

> [!div class="checklist"]
> * Maken van een cloud-init-configuratiebestand
> * Een virtuele machine die gebruikmaakt van een cloud-init-bestand maken
> * Een actieve Node.js-app weergeven nadat de virtuele machine is gemaakt
> * Sleutelkluis gebruiken voor het veilig opslaan van certificaten
> * Beveiligde implementaties van NGINX met cloud-init automatiseren


[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

Als u wilt installeren en gebruiken van de CLI lokaal, in deze zelfstudie vereist dat u de Azure CLI versie 2.0.4 zijn uitgevoerd of hoger. Voer `az --version` uit om de versie te bekijken. Als u Azure CLI 2.0 wilt installeren of upgraden, raadpleegt u [Azure CLI 2.0 installeren]( /cli/azure/install-azure-cli).  



## <a name="cloud-init-overview"></a>Overzicht van cloud-init
[Cloud-init](https://cloudinit.readthedocs.io) is een veelgebruikte benadering voor het aanpassen van een Linux-VM als deze voor de eerste keer wordt opgestart. U kunt cloud init gebruiken voor het installeren van pakketten en bestanden schrijven of om gebruikers en beveiliging te configureren. Als de initialisatie van de cloud wordt uitgevoerd tijdens het opstartproces, zijn er geen extra stappen of vereist agents naar uw configuratie toe te passen.

Cloud-init werkt ook via distributies. Bijvoorbeeld, u niet gebruikt **apt get-installatie** of **yum installeren** om een pakket te installeren. In plaats daarvan kunt u een lijst met pakketten te installeren. Het hulpprogramma voor systeemeigen pakket cloud init automatisch gebruikt voor de distro die u selecteert.

We werken met onze partners ophalen van cloud-init opgenomen en in de afbeeldingen die ze naar Azure bieden werkt. De volgende tabel geeft een overzicht van de huidige beschikbaarheid van de cloud init op Azure-platform installatiekopieën:

| Alias | Uitgever | Aanbieding | SKU | Versie |
|:--- |:--- |:--- |:--- |:--- |:--- |
| UbuntuLTS |Canonical |UbuntuServer |16.04 TNS |meest recente |
| UbuntuLTS |Canonical |UbuntuServer |14.04.5-LTS |meest recente |
| CoreOS |CoreOS |CoreOS |Stabiel |meest recente |
| | OpenLogic | CentOS | 7 CI | meest recente |
| | RedHat | RHEL | 7 ONBEWERKTE CI | meest recente


## <a name="create-cloud-init-config-file"></a>Cloud-init-configuratiebestand maken
Overzicht van cloud-init in actie, maak een VM die NGINX installeert en een eenvoudige 'Hallo wereld' Node.js-app wordt uitgevoerd. De volgende cloud init-configuratie installeert de vereiste pakketten, maakt u een Node.js-app en vervolgens initialiseren en start u de app.

Maak een bestand met de naam in uw huidige shell *cloud init.txt* en plak de volgende configuratie. Maak bijvoorbeeld het bestand in de Cloud-Shell niet op uw lokale machine. U kunt een editor die u wilt gebruiken. Voer `sensible-editor cloud-init.txt` voor het maken van het bestand en een overzicht van beschikbare editors. Controleer of het hele cloud-init-bestand correct is gekopieerd met name de eerste regel:

```yaml
#cloud-config
package_upgrade: true
packages:
  - nginx
  - nodejs
  - npm
write_files:
  - owner: www-data:www-data
  - path: /etc/nginx/sites-available/default
    content: |
      server {
        listen 80;
        location / {
          proxy_pass http://localhost:3000;
          proxy_http_version 1.1;
          proxy_set_header Upgrade $http_upgrade;
          proxy_set_header Connection keep-alive;
          proxy_set_header Host $host;
          proxy_cache_bypass $http_upgrade;
        }
      }
  - owner: azureuser:azureuser
  - path: /home/azureuser/myapp/index.js
    content: |
      var express = require('express')
      var app = express()
      var os = require('os');
      app.get('/', function (req, res) {
        res.send('Hello World from host ' + os.hostname() + '!')
      })
      app.listen(3000, function () {
        console.log('Hello world app listening on port 3000!')
      })
runcmd:
  - service nginx restart
  - cd "/home/azureuser/myapp"
  - npm init
  - npm install express -y
  - nodejs index.js
```

Zie voor meer informatie over configuratieopties voor cloud-init [cloud init config voorbeelden](https://cloudinit.readthedocs.io/en/latest/topics/examples.html).

## <a name="create-virtual-machine"></a>Virtuele machine maken
Voordat u een virtuele machine maken kunt, maakt u een resourcegroep met [az groep maken](/cli/azure/group#create). Het volgende voorbeeld wordt een resourcegroep met de naam *myResourceGroupAutomate* in de *eastus* locatie:

```azurecli-interactive 
az group create --name myResourceGroupAutomate --location eastus
```

Maak nu een virtuele machine met [az vm maken](/cli/azure/vm#create). Gebruik de `--custom-data` parameter om door te geven in uw cloud-init-configuratiebestand. Geef het volledige pad naar de *cloud init.txt* config als u het bestand buiten uw werkmap aanwezig opgeslagen. Het volgende voorbeeld wordt een virtuele machine met de naam *myAutomatedVM*:

```azurecli-interactive 
az vm create \
    --resource-group myResourceGroupAutomate \
    --name myVM \
    --image UbuntuLTS \
    --admin-username azureuser \
    --generate-ssh-keys \
    --custom-data cloud-init.txt
```

Het duurt enkele minuten duren voordat de virtuele machine moet worden gemaakt, de pakketten te installeren en de app te starten. Er zijn achtergrondtaken die uitvoeren blijven nadat de Azure CLI keert u terug naar de prompt. Het is mogelijk een andere paar minuten voordat u toegang hebt tot de app. Wanneer de virtuele machine is gemaakt, dient u de `publicIpAddress` weergegeven door de Azure CLI. Dit adres wordt gebruikt voor toegang tot de Node.js-app via een webbrowser.

Open poort 80 via Internet met zodat webverkeer bereiken van uw virtuele machine [az vm open poort](/cli/azure/vm#open-port):

```azurecli-interactive 
az vm open-port --port 80 --resource-group myResourceGroupAutomate --name myVM
```

## <a name="test-web-app"></a>Test-web-app
Nu kunt u een webbrowser openen en voer *http://<publicIpAddress>*  in de adresbalk. Geef uw eigen openbare IP-adres van de virtuele machine proces maken. Uw Node.js-app wordt weergegeven zoals in het volgende voorbeeld:

![Bekijk actieve NGINX-website](./media/tutorial-automate-vm-deployment/nginx.png)


## <a name="inject-certificates-from-key-vault"></a>Certificaten van de Sleutelkluis invoeren
Deze optionele sectie wordt beschreven hoe u veilig opslaan van certificaten in Azure Sleutelkluis en ze tijdens de implementatie van de virtuele machine te injecteren. In plaats van met een aangepaste installatiekopie met de certificaten standaard uitbreidbaar, in dit proces zorgt ervoor dat de meest recente certificaten zijn ingevoegd op een virtuele machine op de eerste keer wordt opgestart. Tijdens het proces is het certificaat nooit verlaat de Azure-platform, of wordt weergegeven in een script, opdrachtregel geschiedenis of sjabloon.

Azure Sleutelkluis beschermt cryptografische sleutels en geheimen, zoals certificaten of wachtwoorden. Sleutelkluis helpt te stroomlijnen van het sleutelbeheerproces en kunt u controle houdt over sleutels die toegang tot en uw gegevens te versleutelen. Dit scenario bevat enkele concepten die Sleutelkluis voor het maken en een certificaat te gebruiken, maar is niet een volledig overzicht over het gebruik van de Sleutelkluis.

De volgende stappen laten zien hoe u kunt:

- Maken van een Azure Sleutelkluis
- Genereren of een certificaat uploaden naar de Sleutelkluis
- Een geheim van het certificaat invoeren voor een virtuele machine maken
- Een virtuele machine maken en het certificaat invoeren

### <a name="create-an-azure-key-vault"></a>Maken van een Azure Sleutelkluis
Maak eerst een Sleutelkluis met [az keyvault maken](/cli/azure/keyvault#create) en inschakelen voor gebruik wanneer u een virtuele machine implementeert. Elke Sleutelkluis moet een unieke naam hebben en moet alle kleine letters. Vervang *mykeyvault* in het volgende voorbeeld met de naam van uw eigen unieke Sleutelkluis:

```azurecli-interactive 
keyvault_name=mykeyvault
az keyvault create \
    --resource-group myResourceGroupAutomate \
    --name $keyvault_name \
    --enabled-for-deployment
```

### <a name="generate-certificate-and-store-in-key-vault"></a>Certificaat genereren en opslaan in de Sleutelkluis
Voor gebruik in productieomgevingen, moet u een geldig certificaat dat is ondertekend door een vertrouwde provider met importeren [az keyvault certificaat importeren](/cli/azure/keyvault/certificate#import). Voor deze zelfstudie, het volgende voorbeeld toont hoe kunt u een zelfondertekend certificaat met genereren [az keyvault-certificaat maken](/cli/azure/keyvault/certificate#create) die gebruikmaakt van het standaardbeleid voor certificaat:

```azurecli-interactive 
az keyvault certificate create \
    --vault-name $keyvault_name \
    --name mycert \
    --policy "$(az keyvault certificate get-default-policy)"
```


### <a name="prepare-certificate-for-use-with-vm"></a>Certificaat voor gebruik met de virtuele machine voorbereiden
Het certificaat te gebruiken tijdens de virtuele machine proces voor het maken, verkrijgen van de ID van het certificaat met [az keyvault lijst-versies van het clientgeheim](/cli/azure/keyvault/secret#list-versions). De virtuele machine moet het certificaat in een bepaalde indeling invoeren deze bij het opstarten, converteert u dus het certificaat met [az vm indeling-geheim](/cli/azure/vm#format-secret). Het volgende voorbeeld wordt de uitvoer van deze opdrachten naar variabelen voor eenvoudig te gebruiken in de volgende stappen:

```azurecli-interactive 
secret=$(az keyvault secret list-versions \
          --vault-name $keyvault_name \
          --name mycert \
          --query "[?attributes.enabled].id" --output tsv)
vm_secret=$(az vm format-secret --secret "$secret")
```


### <a name="create-cloud-init-config-to-secure-nginx"></a>Maken van cloud-init-config NGINX beveiligen
Wanneer u maakt een virtuele machine, certificaten en sleutels worden opgeslagen in de beveiligde */var/lib/waagent/* directory. U kunt een bijgewerkte cloud-init-configuratie van het vorige voorbeeld gebruiken voor het automatiseren van het certificaat toevoegen aan de virtuele machine en NGINX configureren.

Maak een bestand met de naam *cloud-init-secured.txt* en plak de volgende configuratie. Opnieuw als u de Shell Cloud gebruikt, maken het configuratiebestand van de cloud init er en niet op uw lokale computer. Gebruik `sensible-editor cloud-init-secured.txt` voor het maken van het bestand en een overzicht van beschikbare editors. Controleer of het hele cloud-init-bestand correct is gekopieerd met name de eerste regel:

```yaml
#cloud-config
package_upgrade: true
packages:
  - nginx
  - nodejs
  - npm
write_files:
  - owner: www-data:www-data
  - path: /etc/nginx/sites-available/default
    content: |
      server {
        listen 80;
        listen 443 ssl;
        ssl_certificate /etc/nginx/ssl/mycert.cert;
        ssl_certificate_key /etc/nginx/ssl/mycert.prv;
        location / {
          proxy_pass http://localhost:3000;
          proxy_http_version 1.1;
          proxy_set_header Upgrade $http_upgrade;
          proxy_set_header Connection keep-alive;
          proxy_set_header Host $host;
          proxy_cache_bypass $http_upgrade;
        }
      }
  - owner: azureuser:azureuser
  - path: /home/azureuser/myapp/index.js
    content: |
      var express = require('express')
      var app = express()
      var os = require('os');
      app.get('/', function (req, res) {
        res.send('Hello World from host ' + os.hostname() + '!')
      })
      app.listen(3000, function () {
        console.log('Hello world app listening on port 3000!')
      })
runcmd:
  - secretsname=$(find /var/lib/waagent/ -name "*.prv" | cut -c -57)
  - mkdir /etc/nginx/ssl
  - cp $secretsname.crt /etc/nginx/ssl/mycert.cert
  - cp $secretsname.prv /etc/nginx/ssl/mycert.prv
  - service nginx restart
  - cd "/home/azureuser/myapp"
  - npm init
  - npm install express -y
  - nodejs index.js
```

### <a name="create-secure-vm"></a>Beveiligde virtuele machine maken
Maak nu een virtuele machine met [az vm maken](/cli/azure/vm#create). De gegevens van het certificaat wordt geïnjecteerd uit de Sleutelkluis met de `--secrets` parameter. Zoals in het vorige voorbeeld, geeft u ook in de cloud init-configuratie met de `--custom-data` parameter:

```azurecli-interactive 
az vm create \
    --resource-group myResourceGroupAutomate \
    --name myVMSecured \
    --image UbuntuLTS \
    --admin-username azureuser \
    --generate-ssh-keys \
    --custom-data cloud-init-secured.txt \
    --secrets "$vm_secret"
```

Het duurt enkele minuten duren voordat de virtuele machine moet worden gemaakt, de pakketten te installeren en de app te starten. Er zijn achtergrondtaken die uitvoeren blijven nadat de Azure CLI keert u terug naar de prompt. Het is mogelijk een andere paar minuten voordat u toegang hebt tot de app. Wanneer de virtuele machine is gemaakt, dient u de `publicIpAddress` weergegeven door de Azure CLI. Dit adres wordt gebruikt voor toegang tot de Node.js-app via een webbrowser.

Zodat beveiligde webverkeer bereiken van uw virtuele machine poort 443 openen vanaf het Internet met [az vm open poort](/cli/azure/vm#open-port):

```azurecli-interactive 
az vm open-port \
    --resource-group myResourceGroupAutomate \
    --name myVMSecured \
    --port 443
```

### <a name="test-secure-web-app"></a>Test beveiligde web-app
Nu kunt u een webbrowser openen en voer *https://<publicIpAddress>*  in de adresbalk. Geef uw eigen openbare IP-adres van de virtuele machine proces maken. De beveiligingswaarschuwing accepteren als u een zelfondertekend certificaat gebruikt:

![Beveiligingswaarschuwing voor web browser accepteren](./media/tutorial-automate-vm-deployment/browser-warning.png)

Uw beveiligde NGINX-site en Node.js app wordt vervolgens weergegeven zoals in het volgende voorbeeld:

![Actief beveiligde NGINX-site weergeven](./media/tutorial-automate-vm-deployment/secured-nginx.png)


## <a name="next-steps"></a>Volgende stappen
In deze zelfstudie kunt u virtuele machines op de eerste keer opstarten met cloud-init geconfigureerd. U hebt geleerd hoe u:

> [!div class="checklist"]
> * Maken van een cloud-init-configuratiebestand
> * Een virtuele machine die gebruikmaakt van een cloud-init-bestand maken
> * Een actieve Node.js-app weergeven nadat de virtuele machine is gemaakt
> * Sleutelkluis gebruiken voor het veilig opslaan van certificaten
> * Beveiligde implementaties van NGINX met cloud-init automatiseren

Ga naar de volgende zelfstudie voor informatie over het maken van aangepaste installatiekopieën van virtuele machine.

> [!div class="nextstepaction"]
> [Aangepaste VM-installatiekopieën maken](./tutorial-custom-images.md)
