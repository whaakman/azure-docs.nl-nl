---
title: Omgevingsvariabelen worden ingesteld in Azure Containerexemplaren
description: Informatie over het instellen van omgevingsvariabelen in de containers die u in Azure Containerexemplaren uitvoert
services: container-instances
author: mmacy
manager: jeconnoc
ms.service: container-instances
ms.topic: article
ms.date: 06/07/2018
ms.author: marsma
ms.openlocfilehash: bc30352f50344031f8356d2be1b800dd035f12ad
ms.sourcegitcommit: 944d16bc74de29fb2643b0576a20cbd7e437cef2
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/07/2018
ms.locfileid: "34830459"
---
# <a name="set-environment-variables"></a>Omgevingsvariabelen instellen

Omgevingsvariabelen instellen in uw containerexemplaren, kunt u dynamische configuratie van de toepassing of het script uitgevoerd op de container bieden. Omgevingsvariabelen in een container stelt opgeven bij het maken van een exemplaar van de container. U kunt de omgevingsvariabelen instellen bij het starten van een container met de [Azure CLI](#azure-cli-example), [Azure PowerShell](#azure-powershell-example), en de [Azure-portal](#azure-portal-example).

Als u bijvoorbeeld de [aci-microsoft-wordcount] [ aci-wordcount] container installatiekopie, u kunt het gedrag wijzigen door op te geven van de volgende omgevingsvariabelen:

*NumWords*: het aantal woorden naar STDOUT verzonden.

*MinLength*: het minimum aantal tekens in een woord om te worden geteld. Een hoger getal worden genegeerd veelvoorkomende woorden zoals 'van' en 'het'.

Als u doorgeven geheimen als omgevingsvariabelen wilt, Containerexemplaren Azure ondersteunt [waarden secure](#secure-values) waarden voor Windows- en Linux-containers te beveiligen.

## <a name="azure-cli-example"></a>Voorbeeld van Azure CLI

Om te zien van de standaarduitvoer van de [aci-microsoft-wordcount] [ aci-wordcount] -container, eerst uitvoeren met dit [az container maken] [ az-container-create] opdracht (Nee omgevingsvariabelen opgegeven):

```azurecli-interactive
az container create \
    --resource-group myResourceGroup \
    --name mycontainer1 \
    --image microsoft/aci-wordcount:latest \
    --restart-policy OnFailure
```

Voor het wijzigen van de uitvoer, start u een tweede container met de `--environment-variables` argument toegevoegd, geeft u waarden voor de *NumWords* en *MinLength* variabelen:

```azurecli-interactive
az container create \
    --resource-group myResourceGroup \
    --name mycontainer2 \
    --image microsoft/aci-wordcount:latest \
    --restart-policy OnFailure \
    --environment-variables NumWords=5 MinLength=8
```

Zodra de beide containers status wordt weergegeven als *beëindigd* (Gebruik [az container weergeven] [ az-container-show] status controleren), de logboeken met weergegeven [az container logboeken] [ az-container-logs] om te zien van de uitvoer.

```azurecli-interactive
az container logs --resource-group myResourceGroup --name mycontainer1
az container logs --resource-group myResourceGroup --name mycontainer2
```

De uitvoer van de containers laten zien hoe u de tweede container script gedrag hebt gewijzigd door het instellen van omgevingsvariabelen.

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

Omgevingsvariabelen instellen in PowerShell is vergelijkbaar met de CLI, maar gebruikt de `-EnvironmentVariable` opdrachtregelargument.

Start de [aci-microsoft-wordcount] [ aci-wordcount] container in de standaardconfiguratie dit [nieuw AzureRmContainerGroup] [ new-azurermcontainergroup]opdracht:

```azurepowershell-interactive
New-AzureRmContainerGroup `
    -ResourceGroupName myResourceGroup `
    -Name mycontainer1 `
    -Image microsoft/aci-wordcount:latest
```

Voer nu de volgende [nieuw AzureRmContainerGroup] [ new-azurermcontainergroup] opdracht. Deze geeft de *NumWords* en *MinLength* omgevingsvariabelen na het vullen van een matrixvariabele `envVars`:

```azurepowershell-interactive
$envVars = @{NumWords=5;MinLength=8}
New-AzureRmContainerGroup `
    -ResourceGroupName myResourceGroup `
    -Name mycontainer2 `
    -Image microsoft/aci-wordcount:latest `
    -RestartPolicy OnFailure `
    -EnvironmentVariable $envVars
```

Zodra de status van de beide containers is *beëindigd* (Gebruik [Get-AzureRmContainerInstanceLog] [ azure-instance-log] status controleren), pull-hun logboeken met de [ Get-AzureRmContainerInstanceLog] [ azure-instance-log] opdracht.

```azurepowershell-interactive
Get-AzureRmContainerInstanceLog -ResourceGroupName myResourceGroup -ContainerGroupName mycontainer1
Get-AzureRmContainerInstanceLog -ResourceGroupName myResourceGroup -ContainerGroupName mycontainer2
```

De uitvoer voor elke container laat zien hoe u het script uitgevoerd op de container door het instellen van omgevingsvariabelen hebt gewijzigd.

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

## <a name="azure-portal-example"></a>Voorbeeld van de Azure portal

U kunt omgevingsvariabelen instellen bij het starten van een container in Azure portal geven ze op in de **configuratie** pagina wanneer u de container maken.

Wanneer u met de portal implementeert, u bent momenteel beperkt tot drie variabelen en moet u deze in de volgende notatie invoeren: `"variableName":"value"`

Start een als voorbeeld wilt zien, de [aci-microsoft-wordcount] [ aci-wordcount] container met de *NumWords* en *MinLength* variabelen.

1. In **configuratie**stelt de **beleid opnieuw opstarten** naar *bij fout*
2. Voer `"NumWords":"5"` selecteren voor de eerste variabele **Ja** onder **extra omgevingsvariabelen toevoegen**, en voer `"MinLength":"8"` voor de tweede variabele. Selecteer **OK** om te controleren en vervolgens de container te implementeren.

![Portal-pagina met omgeving variabele Enable-knop en tekstvakken][portal-env-vars-01]

Om weer te geven van de container-Logboeken onder **instellingen** Selecteer **Containers**, klikt u vervolgens **logboeken**. Vergelijkbaar met de uitvoer wordt weergegeven in de vorige CLI en PowerShell secties, dat kunt u zien hoe het script gedrag is gewijzigd door de omgevingsvariabelen. Slechts vijf woorden worden weergegeven, elk met een minimale lengte van acht tekens.

![Portal waarin de uitvoer van container][portal-env-vars-02]

## <a name="secure-values"></a>Beveiligde waarden
Objecten van het beveiligde waarden zijn bedoeld om gevoelige informatie, zoals wachtwoorden of sleutels voor uw toepassing bevatten. Met behulp van veilige waarden voor omgevingsvariabelen is veiliger en flexibeler dan in de installatiekopie van de container op te nemen. Een andere optie is het gebruik van de geheime volumes, dat wordt beschreven in [koppelen van een geheime volume in Azure Containerexemplaren](container-instances-volume-secret.md).

Beveiligde omgevingsvariabelen met beveiligde waarden won't de beveiligde waarde in de eigenschappen van de container bekendmaken, zodat de waarde kan alleen worden benaderd binnen de container. Bijvoorbeeld, eigenschappen van container weergegeven in de Azure portal of Azure CLI niet wordt weergegeven als een omgevingsvariabele met een beveiligde waarde.

Een beveiligde omgeving-variabele instellen door te geven de `secureValue` eigenschap in plaats van de normale `value` voor het type van de variabele. De twee variabelen die zijn gedefinieerd in de volgende YAML laten zien dat de twee typen variabelen.

### <a name="yaml-deployment"></a>YAML-implementatie

Maak een `secure-env.yaml` bestand met het volgende fragment.

```yaml
apiVersion: 2018-06-01
location: westus
name: securetest
properties:
  containers:
  - name: mycontainer
    properties:
      environmentVariables:
        - "name": "SECRET"
          "secureValue": "my-secret-value"
        - "name": "NOTSECRET"
          "value": "my-exposed-value"
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

Voer de volgende opdracht voor het implementeren van de containergroep met YAML.

```azurecli-interactive
az container create --resource-group myRG --name securetest -f secure-env.yaml
```

### <a name="verify-environment-variables"></a>Controleer of de omgevingsvariabelen

Voer de volgende opdracht aan de query voor de container omgevingsvariabelen.

```azurecli-interactive
az container show --resource-group myRG --name securetest --query 'containers[].environmentVariables`
```

De JSON-antwoord met details voor deze container wordt alleen de niet-beveiligde omgevingsvariabele weergeven en beveiligen van de omgevingsvariabele sleutel.

```json
  "environmentVariables": [
    {
      "name": "NOTSECRET",
      "value": "my-exposed-value"
    },
    {
      "name": "SECRET"
    }
```

U kunt het veilig bekijken omgevingsvariabele is ingesteld met de `exec` opdracht waarmee een opdracht uit binnen een actieve container wordt uitgevoerd. 

Voer de volgende opdracht om een interactieve bash-sessie starten met de container.
```azurecli-interactive
az container exec --resource-group myRG --name securetest --exec-command "/bin/bash"
```

Afdrukken van binnen uw container uw omgevingsvariabele met de volgende bash-opdracht.
```bash
echo $SECRET
```

## <a name="next-steps"></a>Volgende stappen

Taakgebaseerde scenario's, zoals een grote gegevensset met verschillende containers voor batchverwerking kunnen profiteren van aangepaste omgevingsvariabelen tijdens runtime. Zie voor meer informatie over het uitvoeren van containers taakgebaseerde [beperkte taken uitvoeren in Azure Containerexemplaren](container-instances-restart-policy.md).

<!-- IMAGES -->
[portal-env-vars-01]: ./media/container-instances-environment-variables/portal-env-vars-01.png
[portal-env-vars-02]: ./media/container-instances-environment-variables/portal-env-vars-02.png

<!-- LINKS - External -->
[aci-wordcount]: https://hub.docker.com/r/microsoft/aci-wordcount/

<!-- LINKS Internal -->
[az-container-create]: /cli/azure/container#az-container-create
[az-container-logs]: /cli/azure/container#az-container-logs
[az-container-show]: /cli/azure/container#az-container-show
[azure-cli-install]: /cli/azure/
[azure-instance-log]: /powershell/module/azurerm.containerinstance/get-azurermcontainerinstancelog
[azure-powershell-install]: /powershell/azure/install-azurerm-ps
[new-azurermcontainergroup]: /powershell/module/azurerm.containerinstance/new-azurermcontainergroup
[portal]: https://portal.azure.com
