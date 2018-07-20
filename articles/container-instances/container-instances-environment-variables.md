---
title: Omgevingsvariabelen instellen in Azure Container Instances
description: Informatie over het instellen van omgevingsvariabelen in de containers die u in Azure Container Instances uitvoert
services: container-instances
author: mmacy
manager: jeconnoc
ms.service: container-instances
ms.topic: article
ms.date: 07/19/2018
ms.author: marsma
ms.openlocfilehash: 7a3d521d4382e3d9b5b1b1cf4eb3e43fa02c9a40
ms.sourcegitcommit: 1478591671a0d5f73e75aa3fb1143e59f4b04e6a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/19/2018
ms.locfileid: "39159546"
---
# <a name="set-environment-variables"></a>Omgevingsvariabelen instellen

Omgevingsvariabelen instellen in uw container instances kunt u dynamische configuratie van de toepassing of script uitvoeren op de container. Omgevingsvariabelen instellen in een container, geeft u deze wanneer u een containerinstantie maken. U kunt omgevingsvariabelen instellen bij het starten van een container met de [Azure CLI](#azure-cli-example), [Azure PowerShell](#azure-powershell-example), en de [Azure-portal](#azure-portal-example).

Als u bijvoorbeeld de [aci-microsoft-wordcount] [ aci-wordcount] containerinstallatiekopie, kunt u het gedrag wijzigen door de volgende omgevingsvariabelen op te geven:

*NumWords*: het aantal woorden die worden verzonden naar de STDOUT.

*MinLength*: het minimale aantal tekens in een woord om te worden geteld. Hoe hoger de waarde wordt genegeerd voor veelvoorkomende woorden als 'van' en 'de'.

Als u nodig hebt om door te geven van geheimen als omgevingsvariabelen, Azure Container Instances biedt ondersteuning voor [waarden secure](#secure-values) voor zowel Windows als Linux-containers.

## <a name="azure-cli-example"></a>Azure CLI-voorbeeld

Om te zien van de standaarduitvoer van de [aci-microsoft-wordcount] [ aci-wordcount] container, eerst uitvoeren met dit [az container maken] [ az-container-create] opdracht (Nee omgevingsvariabelen opgegeven):

```azurecli-interactive
az container create \
    --resource-group myResourceGroup \
    --name mycontainer1 \
    --image microsoft/aci-wordcount:latest \
    --restart-policy OnFailure
```

Als u wilt wijzigen van de uitvoer, start u een tweede container met de `--environment-variables` argument toegevoegd, waarden opgeven voor de *NumWords* en *MinLength* variabelen:

```azurecli-interactive
az container create \
    --resource-group myResourceGroup \
    --name mycontainer2 \
    --image microsoft/aci-wordcount:latest \
    --restart-policy OnFailure \
    --environment-variables NumWords=5 MinLength=8
```

Nadat de beide containers status wordt weergegeven als *beëindigd* (Gebruik [az container show] [ az-container-show] status controleren), geven hun logboeken met [az container logs] [ az-container-logs] om de uitvoer te bekijken.

```azurecli-interactive
az container logs --resource-group myResourceGroup --name mycontainer1
az container logs --resource-group myResourceGroup --name mycontainer2
```

De uitvoer van de containers laten zien hoe u van de container van het tweede script gedrag hebt gewijzigd door het instellen van omgevingsvariabelen.

```console
azureuser@Azure:~$ az container logs --resource-group myResourceGroup --name mycontainer1
[('the', 990),
 ('and', 702),
 ('of', 628),
 ('to', 610),
 ('I', 544),
 ('you', 495),
 ('a', 453),
 ('my', 441),
 ('in', 399),
 ('HAMLET', 386)]

azureuser@Azure:~$ az container logs --resource-group myResourceGroup --name mycontainer2
[('CLAUDIUS', 120),
 ('POLONIUS', 113),
 ('GERTRUDE', 82),
 ('ROSENCRANTZ', 69),
 ('GUILDENSTERN', 54)]
```

## <a name="azure-powershell-example"></a>Azure PowerShell-voorbeeld

Omgevingsvariabelen instellen in PowerShell is vergelijkbaar met de CLI, maar maakt gebruik van de `-EnvironmentVariable` opdrachtregelargument.

Eerst, start de [aci-microsoft-wordcount] [ aci-wordcount] container in de standaardconfiguratie met deze [New-AzureRmContainerGroup] [ new-azurermcontainergroup]opdracht:

```azurepowershell-interactive
New-AzureRmContainerGroup `
    -ResourceGroupName myResourceGroup `
    -Name mycontainer1 `
    -Image microsoft/aci-wordcount:latest
```

Voer nu de volgende [New-AzureRmContainerGroup] [ new-azurermcontainergroup] opdracht. Deze geeft de *NumWords* en *MinLength* omgevingsvariabelen na het vullen van een matrixvariabele `envVars`:

```azurepowershell-interactive
$envVars = @{NumWords=5;MinLength=8}
New-AzureRmContainerGroup `
    -ResourceGroupName myResourceGroup `
    -Name mycontainer2 `
    -Image microsoft/aci-wordcount:latest `
    -RestartPolicy OnFailure `
    -EnvironmentVariable $envVars
```

Nadat de beide containers staat *beëindigd* (Gebruik [Get-AzureRmContainerInstanceLog] [ azure-instance-log] status controleren), pull-hun logboeken met de [ Get-AzureRmContainerInstanceLog] [ azure-instance-log] opdracht.

```azurepowershell-interactive
Get-AzureRmContainerInstanceLog -ResourceGroupName myResourceGroup -ContainerGroupName mycontainer1
Get-AzureRmContainerInstanceLog -ResourceGroupName myResourceGroup -ContainerGroupName mycontainer2
```

De uitvoer voor elke container ziet u hoe u het script uitgevoerd op de container door het instellen van omgevingsvariabelen hebt gewijzigd.

```console
PS Azure:\> Get-AzureRmContainerInstanceLog -ResourceGroupName myResourceGroup -ContainerGroupName mycontainer1
[('the', 990),
 ('and', 702),
 ('of', 628),
 ('to', 610),
 ('I', 544),
 ('you', 495),
 ('a', 453),
 ('my', 441),
 ('in', 399),
 ('HAMLET', 386)]

Azure:\
PS Azure:\> Get-AzureRmContainerInstanceLog -ResourceGroupName myResourceGroup -ContainerGroupName mycontainer2
[('CLAUDIUS', 120),
 ('POLONIUS', 113),
 ('GERTRUDE', 82),
 ('ROSENCRANTZ', 69),
 ('GUILDENSTERN', 54)]

Azure:\
```

## <a name="azure-portal-example"></a>Voorbeeld van Azure portal

Om in te stellen omgevingsvariabelen bij het starten van een container in Azure portal, geeft u deze in de **configuratie** pagina bij het maken van de container.

Wanneer u met de portal implementeert, kunt u momenteel maximaal drie variabelen en moet u deze invoeren in deze indeling: `"variableName":"value"`

Als u wilt zien, start de [aci-microsoft-wordcount] [ aci-wordcount] container met de *NumWords* en *MinLength* variabelen.

1. In **configuratie**, stel de **beleid voor opnieuw opstarten** naar *bij fout*
2. Voer `"NumWords":"5"` selecteren voor de eerste variabele **Ja** onder **toevoegen van aanvullende omgevingsvariabelen**, en voer `"MinLength":"8"` voor de tweede variabele. Selecteer **OK** om te controleren en vervolgens de container te implementeren.

![Portal-pagina met omgeving variabele inschakelen-knop en tekstvakken][portal-env-vars-01]

Om weer te geven van de container-Logboeken onder **instellingen** Selecteer **Containers**, klikt u vervolgens **logboeken**. Net als bij de uitvoer wordt weergegeven in de vorige CLI en PowerShell secties, dat kunt u zien hoe de werking van het script is gewijzigd door de omgevingsvariabelen. Maximaal vijf woorden worden weergegeven, elk met een minimale lengte van acht tekens.

![Portal met de uitvoer van container][portal-env-vars-02]

## <a name="secure-values"></a>Beveiligde waarden

Objecten met beveiligde waarden zijn bedoeld voor het opslaan van gevoelige informatie, zoals wachtwoorden of sleutels voor uw toepassing. Met behulp van beveiligde waarden voor omgevingsvariabelen is veiliger en flexibeler dan opneemt in uw container-installatiekopie. Een andere optie is het gebruik van geheime volumes, die worden beschreven in [koppelen van een geheime volume in Azure Container Instances](container-instances-volume-secret.md).

Omgevingsvariabelen met beveiligde waarden zijn niet zichtbaar in de eigenschappen van uw container--hun waarden zijn toegankelijk alleen in de container. Bijvoorbeeld, containereigenschappen weergegeven in de Azure portal of Azure CLI-weergave alleen een beveiligde variabele de naam, niet de waarde.

Een beveiligde omgeving-variabele instellen door op te geven de `secureValue` eigenschap in plaats van de normale `value` voor het type van de variabele. De twee variabelen die zijn gedefinieerd in de volgende YAML illustratie van de twee typen variabelen.

### <a name="yaml-deployment"></a>YAML-implementatie

Maak een `secure-env.yaml` bestand door het volgende codefragment.

```yaml
apiVersion: 2018-06-01
location: eastus
name: securetest
properties:
  containers:
  - name: mycontainer
    properties:
      environmentVariables:
        - "name": "NOTSECRET"
          "value": "my-exposed-value"
        - "name": "SECRET"
          "secureValue": "my-secret-value"
      image: nginx
      ports: []
      resources:
        requests:
          cpu: 1.0
          memoryInGB: 1.5
  osType: Linux
  restartPolicy: Always
tags: null
type: Microsoft.ContainerInstance/containerGroups
```

Voer de volgende opdracht voor het implementeren van de containergroep met YAML (naam van de resourcegroep indien nodig aanpassen):

```azurecli-interactive
az container create --resource-group myResourceGroup --file secure-env.yaml
```

### <a name="verify-environment-variables"></a>Controleer of omgevingsvariabelen

Voer de [az container show] [ az-container-show] opdracht om op te vragen van de omgevingsvariabelen van uw container:

```azurecli-interactive
az container show --resource-group myResourceGroup --name securetest --query 'containers[].environmentVariables'
```

De JSON-antwoord bevat zowel de sleutel en de waarde van de onbeveiligde omgevingsvariabele, maar alleen de naam van de beveiligde omgevingsvariabele:

```json
[
  [
    {
      "name": "NOTSECRET",
      "secureValue": null,
      "value": "my-exposed-value"
    },
    {
      "name": "SECRET",
      "secureValue": null,
      "value": null
    }
  ]
]
```

Met de [az container exec] [ az-container-exec] opdracht, waarmee een opdracht is uitgevoerd in een container die wordt uitgevoerd, kunt u controleren of de beveiligde omgevingsvariabele is ingesteld. Voer de volgende opdracht om een interactieve bash-sessie starten in de container:

```azurecli-interactive
az container exec --resource-group myResourceGroup --name securetest --exec-command "/bin/bash"
```

Als u een interactieve shell in de container hebt geopend, kunt u toegang tot de `SECRET` de waarde van variabele:

```console
root@caas-ef3ee231482549629ac8a40c0d3807fd-3881559887-5374l:/# echo $SECRET
my-secret-value
```

## <a name="next-steps"></a>Volgende stappen

Taakgebaseerde scenario's, zoals een grote gegevensset met meerdere containers voor batchverwerking kunnen profiteren van aangepaste omgevingsvariabelen tijdens runtime. Zie voor meer informatie over het uitvoeren van containers op basis van een taak [taken in containers uitvoeren in Azure Container Instances](container-instances-restart-policy.md).

<!-- IMAGES -->
[portal-env-vars-01]: ./media/container-instances-environment-variables/portal-env-vars-01.png
[portal-env-vars-02]: ./media/container-instances-environment-variables/portal-env-vars-02.png

<!-- LINKS - External -->
[aci-wordcount]: https://hub.docker.com/r/microsoft/aci-wordcount/

<!-- LINKS Internal -->
[az-container-create]: /cli/azure/container#az-container-create
[az-container-exec]: /cli/azure/container#az-container-exec
[az-container-logs]: /cli/azure/container#az-container-logs
[az-container-show]: /cli/azure/container#az-container-show
[azure-cli-install]: /cli/azure/
[azure-instance-log]: /powershell/module/azurerm.containerinstance/get-azurermcontainerinstancelog
[azure-powershell-install]: /powershell/azure/install-azurerm-ps
[new-azurermcontainergroup]: /powershell/module/azurerm.containerinstance/new-azurermcontainergroup
[portal]: https://portal.azure.com
