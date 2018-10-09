---
title: Opdrachten uitvoeren in het uitvoeren van containers in Azure Container Instances
description: Informatie over hoe een opdracht uitvoeren in een container momenteel wordt uitgevoerd in Azure Container Instances
services: container-instances
author: dlepow
ms.service: container-instances
ms.topic: article
ms.date: 03/30/2018
ms.author: danlep
ms.openlocfilehash: 577e2386c352798bc21a2c78b22726128ac7cf0a
ms.sourcegitcommit: 67abaa44871ab98770b22b29d899ff2f396bdae3
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/08/2018
ms.locfileid: "48854081"
---
# <a name="execute-a-command-in-a-running-azure-container-instance"></a>Een opdracht uitvoeren in een actief Azure container-exemplaar

Azure Container Instances biedt ondersteuning voor een opdracht is uitgevoerd in een container die wordt uitgevoerd. Uitvoeren van een opdracht in een container die u al hebt gestart is vooral nuttig tijdens de ontwikkeling van toepassingen en het oplossen van problemen. De meest voorkomende gebruik van deze functie is een interactieve shell starten zodat u fouten van problemen in een container die wordt uitgevoerd opsporen kunt.

## <a name="run-a-command-with-azure-cli"></a>Een opdracht uitvoeren met Azure CLI

Een opdracht uitvoeren in een container die wordt uitgevoerd met [az container exec] [ az-container-exec] in de [Azure CLI][azure-cli]:

```azurecli
az container exec --resource-group <group-name> --name <container-group-name> --exec-command "<command>"
```

Als u bijvoorbeeld een Bash-shell in een Nginx-container te starten:

```azurecli
az container exec --resource-group myResourceGroup --name mynginx --exec-command "/bin/bash"
```

In de onderstaande voorbeelduitvoer de Bash-shell wordt gestart in een actieve Linux-container, zodat er een terminal waarin `ls` wordt uitgevoerd:

```console
$ az container exec --resource-group myResourceGroup --name mynginx --exec-command "/bin/bash"
root@caas-83e6c883014b427f9b277a2bba3b7b5f-708716530-2qv47:/# ls
bin   dev  home  lib64  mnt  proc  run   srv  tmp  var
boot  etc  lib   media  opt  root  sbin  sys  usr
root@caas-83e6c883014b427f9b277a2bba3b7b5f-708716530-2qv47:/# exit
exit
Bye.
```

In dit voorbeeld wordt de opdrachtprompt geopend in een Nanoserver-container die wordt uitgevoerd:

```console
$ az container exec --resource-group myResourceGroup --name myiis --exec-command "cmd.exe"
Microsoft Windows [Version 10.0.14393]
(c) 2016 Microsoft Corporation. All rights reserved.

C:\>dir
 Volume in drive C has no label.
 Volume Serial Number is 76E0-C852

 Directory of C:\

03/23/2018  09:13 PM    <DIR>          inetpub
11/20/2016  11:32 AM             1,894 License.txt
03/23/2018  09:13 PM    <DIR>          Program Files
07/16/2016  12:09 PM    <DIR>          Program Files (x86)
03/13/2018  08:50 PM           171,616 ServiceMonitor.exe
03/23/2018  09:13 PM    <DIR>          Users
03/23/2018  09:12 PM    <DIR>          var
03/23/2018  09:22 PM    <DIR>          Windows
               2 File(s)        173,510 bytes
               6 Dir(s)  21,171,609,600 bytes free

C:\>exit
Bye.
```

## <a name="multi-container-groups"></a>Groepen met meerdere containers

Als uw [containergroep](container-instances-container-groups.md) heeft meerdere containers, zoals een App-container en een sidecar logboekregistratie, geef de naam van de container waarin u de opdracht met uit te voeren `--container-name`.

Bijvoorbeeld, in de containergroep *mynginx* zijn twee containers *nginx-app* en *logger*. Een shell gestart op de *nginx-app* container:

```azurecli
az container exec --resource-group myResourceGroup --name mynginx --container-name nginx-app --exec-command "/bin/bash"
```

## <a name="restrictions"></a>Beperkingen

Azure Container Instances biedt momenteel ondersteuning voor het starten van een enkel proces met [az container exec][az-container-exec], en u niet de opdrachtargumenten doorgeven. Bijvoorbeeld, u opdrachten, zoals in kan niet koppelen `sh -c "echo FOO && echo BAR"`, of uit te voeren `echo FOO`.

## <a name="next-steps"></a>Volgende stappen

Meer informatie over andere hulpprogramma's voor probleemoplossing en veelvoorkomende implementatieproblemen met in [container en de implementatie problemen in Azure Container Instances](container-instances-troubleshooting.md).

<!-- LINKS - internal -->
[az-container-create]: /cli/azure/container#az-container-create
[az-container-exec]: /cli/azure/container#az-container-exec
[azure-cli]: /cli/azure