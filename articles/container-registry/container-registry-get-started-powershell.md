---
title: 'Snelstartgids: een persoonlijk Docker-register in Azure maken met PowerShell'
description: Leer snel hoe u een persoonlijk Docker-containerregister maakt in Azure met behulp van PowerShell.
services: container-registry
author: marsma
manager: jeconnoc
ms.service: container-registry
ms.topic: quickstart
ms.date: 05/08/2018
ms.author: marsma
ms.custom: mvc
ms.openlocfilehash: e6c3330519692eb829803af2582b711be2fb3efe
ms.sourcegitcommit: f6e2a03076679d53b550a24828141c4fb978dcf9
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/27/2018
ms.locfileid: "43092875"
---
# <a name="quickstart-create-an-azure-container-registry-using-powershell"></a>Snelstart: een Azure Container Registry maken met behulp van PowerShell

Azure Container Registry is een beheerde service voor Docker-containerregisters die wordt gebruikt voor het bouwen, opslaan en faciliteren van installatiekopieën van Docker-containers. In deze snelstart leert u hoe u een Azure Container Registry maakt met behulp van PowerShell. Nadat u het register hebt gemaakt, pusht u er een installatiekopie naartoe en implementeert u de container vervolgens uit uw register in Azure Container Instances (ACI).

## <a name="prerequisites"></a>Vereisten

Voor deze snelstart is moduleversie 5.7.0 of hoger van Azure PowerShell vereist. Voer `Get-Module -ListAvailable AzureRM` uit om uw geïnstalleerde versie te bepalen. Als u PowerShell wilt installeren of upgraden, raadpleegt u [De Azure PowerShell-module installeren](/powershell/azure/install-azurerm-ps).

Docker moet ook lokaal zijn geïnstalleerd. Docker biedt pakketten voor [Mac OS][docker-mac]-, [Windows][docker-windows]- en [Linux][docker-linux]-systemen.

Omdat Azure Cloud-Shell niet alle vereiste Docker-onderdelen bevat (de `dockerd`-daemon), kunt u de Cloud Shell niet voor deze snelstart gebruiken.

## <a name="sign-in-to-azure"></a>Aanmelden bij Azure

Meld u aan bij uw Azure-abonnement met behulp van de opdracht [Connect-AzureRmAccount][Connect-AzureRmAccount] en volg de instructies op het scherm.

```powershell
Connect-AzureRmAccount
```

## <a name="create-resource-group"></a>Een resourcegroep maken

Zodra u bent geverifieerd bij Azure, maakt u een resourcegroep met [New-AzureRmResourceGroup][New-AzureRmResourceGroup]. Een resourcegroep is een logische container waarin u uw Azure-resources implementeert en beheert.

```powershell
New-AzureRmResourceGroup -Name myResourceGroup -Location EastUS
```

## <a name="create-container-registry"></a>Containerregister maken

Maak vervolgens een containerregister in uw nieuwe resourcegroep met de opdracht [New-AzureRMContainerRegistry][New-AzureRMContainerRegistry].

De registernaam moet uniek zijn binnen Azure en mag 5 tot 50 alfanumerieke tekens bevatten. In het volgende voorbeeld wordt een register met de naam 'myContainerRegistry007' gemaakt. Vervang *myContainerRegistry007* in de volgende opdracht en voer de opdracht vervolgens uit om het register te maken:

```powershell
$registry = New-AzureRMContainerRegistry -ResourceGroupName "myResourceGroup" -Name "myContainerRegistry007" -EnableAdminUser -Sku Basic
```

## <a name="log-in-to-registry"></a>Aanmelden bij register

Voordat u installatiekopieën van containers gaat pushen en ophalen, moet u zich aanmelden bij uw register. Gebruik de opdracht [Get-AzureRmContainerRegistryCredential][Get-AzureRmContainerRegistryCredential] om eerst de beheerdersreferenties voor het register op te halen:

```powershell
$creds = Get-AzureRmContainerRegistryCredential -Registry $registry
```

Voer vervolgens de [docker-aanmelding][docker-login] uit om u aan te melden:

```powershell
$creds.Password | docker login $registry.LoginServer -u $creds.Username --password-stdin
```

Na een geslaagde aanmelding wordt dit geretourneerd `Login Succeeded`:

```console
PS Azure:\> $creds.Password | docker login $registry.LoginServer -u $creds.Username --password-stdin
Login Succeeded
```

## <a name="push-image-to-registry"></a>Installatiekopie pushen naar register

Nu u bent aangemeld bij het register, kunt u er installatiekopieën van de container naar pushen. Als u een installatiekopie wilt ophalen die u naar het register kunt pushen, haalt u de openbare [aci helloworld][aci-helloworld-image]-installatiekopie uit Docker-Hub op. De [aci helloworld][aci-helloworld-github]-installatiekopie is een kleine Node.js-toepassing die op een statische HTML-pagina het logo van Azure Container Instances toont.

```powershell
docker pull microsoft/aci-helloworld
```

Wanneer de installatiekopie wordt opgehaald, is de uitvoer soortgelijk aan de volgende:

```console
PS Azure:\> docker pull microsoft/aci-helloworld
Using default tag: latest
latest: Pulling from microsoft/aci-helloworld
88286f41530e: Pull complete
84f3a4bf8410: Pull complete
d0d9b2214720: Pull complete
3be0618266da: Pull complete
9e232827e52f: Pull complete
b53c152f538f: Pull complete
Digest: sha256:a3b2eb140e6881ca2c4df4d9c97bedda7468a5c17240d7c5d30a32850a2bc573
Status: Downloaded newer image for microsoft/aci-helloworld:latest
```

Voordat u een installatiekopie naar uw Azure-containerregister kunt pushen, moet u deze taggen met de volledige gekwalificeerde domeinnaam (FQDN) van uw register. De FQDN-naam van Azure-containerregisters zijn in de indeling *\<registernaam\>.azurecr.io*.

Vul een variabele met de volledige installatiekopie-tag. Neem de aanmeldingsserver, opslagplaatsnaam ('aci-Hallowereld') en installatiekopieversie ('v1') op:

```powershell
$image = $registry.LoginServer + "/aci-helloworld:v1"
```

Tag de installatiekopie vervolgens met [docker tag][docker-tag]:

```powershell
docker tag microsoft/aci-helloworld $image
```

En verzend deze vervolgens met een [docker-pushbewerking][docker-push] naar uw register:

```powershell
docker push $image
```

Als de Docker-client uw installatiekopie pusht, ziet de uitvoer er ongeveer als volgt uit:

```console
PS Azure:\> docker push $image
The push refers to repository [myContainerRegistry007.azurecr.io/aci-helloworld]
31ba1ebd9cf5: Pushed
cd07853fe8be: Pushed
73f25249687f: Pushed
d8fbd47558a8: Pushed
44ab46125c35: Pushed
5bef08742407: Pushed
v1: digest: sha256:565dba8ce20ca1a311c2d9485089d7ddc935dd50140510050345a1b0ea4ffa6e size: 1576
```

Gefeliciteerd! U hebt zojuist uw eerste containerinstallatiekopie naar het register gepusht.

## <a name="deploy-image-to-aci"></a>Installatiekopie implementeren naar ACI

Nu de installatiekopie zich in het register bevindt, implementeert u een container rechtstreeks in Azure Container Instances zodat u kunt zien hoe deze in Azure wordt uitgevoerd.

Converteer eerst de registerreferentie naar een *PSCredential*. De opdracht `New-AzureRmContainerGroup` die u gebruikt om de containerinstantie te maken, vereist dat deze in deze indeling is.

```powershell
$secpasswd = ConvertTo-SecureString $creds.Password -AsPlainText -Force
$pscred = New-Object System.Management.Automation.PSCredential($creds.Username, $secpasswd)
```

Ook moet het label van DNS-naam voor de container uniek zijn binnen de Azure-regio waarin u het hebt gemaakt. Voer de volgende opdracht uit om een variabele te vullen met een gegenereerde naam:

```powershell
$dnsname = "aci-demo-" + (Get-Random -Maximum 9999)
```

Voer tot slot [New-AzureRmContainerGroup][New-AzureRmContainerGroup] uit om een container uit de installatiekopie in uw register te implementeren met 1 CPU-kern en 1 GB geheugen:

```powershell
New-AzureRmContainerGroup -ResourceGroup myResourceGroup -Name "mycontainer" -Image $image -RegistryCredential $pscred -Cpu 1 -MemoryInGB 1 -DnsNameLabel $dnsname
```

Als het goed is, krijgt u een eerste reactie van Azure met details van de container, en is de status in eerste instantie 'In behandeling':

```console
PS Azure:\> New-AzureRmContainerGroup -ResourceGroup myResourceGroup -Name "mycontainer" -Image $image -RegistryCredential $pscred -Cpu 1 -MemoryInGB 1 -DnsNameLabel $dnsname
ResourceGroupName        : myResourceGroup
Id                       : /subscriptions/<subscriptionID>/resourceGroups/myResourceGroup/providers/Microsoft.ContainerInstance/containerGroups/mycontainer
Name                     : mycontainer
Type                     : Microsoft.ContainerInstance/containerGroups
Location                 : eastus
Tags                     :
ProvisioningState        : Creating
Containers               : {mycontainer}
ImageRegistryCredentials : {myContainerRegistry007}
RestartPolicy            : Always
IpAddress                : 40.117.255.198
DnsNameLabel             : aci-demo-8751
Fqdn                     : aci-demo-8751.eastus.azurecontainer.io
Ports                    : {80}
OsType                   : Linux
Volumes                  :
State                    : Pending
Events                   : {}
```

Als u de status wilt controleren en wilt bepalen wanneer deze wordt uitgevoerd, voert u de opdracht [Get-AzureRmContainerGroup][Get-AzureRmContainerGroup] een paar keer uit. Het starten van de container moet minder dan een minuut duren.

```powershell
(Get-AzureRmContainerGroup -ResourceGroupName myResourceGroup -Name mycontainer).ProvisioningState
```

Hier ziet u de dat de ProvisioningState van de container eerst *Maken* is, en vervolgens wordt gewijzigd in de status *Geslaagd* wanneer deze actief en werkend is:

```console
PS Azure:\> (Get-AzureRmContainerGroup -ResourceGroupName myResourceGroup -Name mycontainer).ProvisioningState
Creating
PS Azure:\> (Get-AzureRmContainerGroup -ResourceGroupName myResourceGroup -Name mycontainer).ProvisioningState
Succeeded
```

## <a name="view-running-application"></a>Actieve toepassing weergeven

Zodra de implementatie naar ACI is voltooid en de container actief en werkend is, gaat u naar de volledig gekwalificeerde domeinnaam (FQDN) in uw browser om te zien of de app in Azure wordt uitgevoerd.

Haal de FQDN-naam voor de container op met [Get-AzureRmContainerGroup][Get-AzureRmContainerGroup]:


```powershell
(Get-AzureRmContainerGroup -ResourceGroupName myResourceGroup -Name mycontainer).Fqdn
```

De uitvoer van de opdracht is de FQDN-naam van uw containerinstantie:

```console
PS Azure:\> (Get-AzureRmContainerGroup -ResourceGroupName myResourceGroup -Name mycontainer).Fqdn
aci-demo-8571.eastus.azurecontainer.io
```

Met de FQDN-naam bij de hand, navigeert u er naartoe in uw browser:

![Hello world-app in browser][qs-psh-01-running-app]

Gefeliciteerd! U hebt een container die een toepassing uitvoert in Azure rechtstreeks vanuit de installatiekopie van een container in uw persoonlijke Azure-containerregister geïmplementeerd.

## <a name="clean-up-resources"></a>Resources opschonen

Als u klaar bent met het werken met de resources u hebt gemaakt in deze snelstart, gebruikt u de opdracht [Remove-AzureRmResourceGroup][Remove-AzureRmResourceGroup] om de resourcegroep, het containerregister en de containerinstantie te verwijderen:

```powershell
Remove-AzureRmResourceGroup -Name myResourceGroup
```

## <a name="next-steps"></a>Volgende stappen

In deze snelstart hebt u een Azure-containerregister gemaakt met de Azure CLI en een exemplaar van het register gestart in Azure Container Instances. Ga verder met de zelfstudie voor Azure Container Instances om meer te leren over ACI.

> [!div class="nextstepaction"]
> [Zelfstudie voor Azure Container Instances](../container-instances/container-instances-tutorial-prepare-app.md)

<!-- LINKS - external -->
[aci-helloworld-github]: https://github.com/Azure-Samples/aci-helloworld
[aci-helloworld-image]: https://hub.docker.com/r/microsoft/aci-helloworld/
[docker-linux]: https://docs.docker.com/engine/installation/#supported-platforms
[docker-login]: https://docs.docker.com/engine/reference/commandline/login/
[docker-mac]: https://docs.docker.com/docker-for-mac/
[docker-push]: https://docs.docker.com/engine/reference/commandline/push/
[docker-tag]: https://docs.docker.com/engine/reference/commandline/tag/
[docker-windows]: https://docs.docker.com/docker-for-windows/

<!-- Links - internal -->
[Connect-AzureRmAccount]: /powershell/module/azurerm.profile/connect-azurermaccount
[Get-AzureRmContainerGroup]: /powershell/module/azurerm.containerinstance/get-azurermcontainergroup
[Get-AzureRmContainerRegistryCredential]: /powershell/module/azurerm.containerregistry/get-azurermcontainerregistrycredential
[Get-Module]: /powershell/module/microsoft.powershell.core/get-module
[New-AzureRmContainerGroup]: /powershell/module/azurerm.containerinstance/new-azurermcontainergroup
[New-AzureRMContainerRegistry]: /powershell/module/azurerm.containerregistry/New-AzureRMContainerRegistry
[New-AzureRmResourceGroup]: /powershell/module/azurerm.resources/new-azurermresourcegroup
[Remove-AzureRmResourceGroup]: /powershell/module/azurerm.resources/remove-azurermresourcegroup

<!-- IMAGES> -->
[qs-psh-01-running-app]: ./media/container-registry-get-started-powershell/qs-psh-01-running-app.png
