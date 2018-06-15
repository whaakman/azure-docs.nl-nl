---
title: Opdrachten uitvoeren in de actieve containers in Azure Containerexemplaren
description: Meer informatie over hoe een opdracht in een container die momenteel wordt uitgevoerd in Azure Containerexemplaren worden uitgevoerd
services: container-instances
author: mmacy
manager: jeconnoc
ms.service: container-instances
ms.topic: article
ms.date: 03/30/2018
ms.author: marsma
ms.openlocfilehash: 43211f620efb16cbcd722d3d386b1bb862fc6280
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/28/2018
ms.locfileid: "32165643"
---
# <a name="execute-a-command-in-a-running-azure-container-instance"></a>Geef een opdracht in een actief Azure-container-exemplaar

Azure Containerexemplaren biedt ondersteuning voor het uitvoeren van een opdracht in een actieve container. Uitvoeren van een opdracht in een container die u al hebt gestart is vooral nuttig tijdens de ontwikkeling van toepassingen en probleemoplossing. De meest voorkomende gebruik van deze functie is een interactieve shell starten zodat u fouten van problemen in een container uitgevoerd opsporen kunt.

## <a name="run-a-command-with-azure-cli"></a>Een opdracht uitvoert met Azure CLI

Geef een opdracht in een actieve container met [az container exec] [ az-container-exec] in de [Azure CLI][azure-cli]:

```azurecli
az container exec --resource-group <group-name> --name <container-group-name> --exec-command "<command>"
```

Als u bijvoorbeeld een Bash-shell in een Nginx-container te starten:

```azurecli
az container exec --resource-group myResourceGroup --name mynginx --exec-command "/bin/bash"
```

In de onderstaande voorbeelduitvoer Bash-shell wordt gestart in een actieve Linux-container, bieden een terminal waarin `ls` wordt uitgevoerd:

```console
$ az container exec --resource-group myResourceGroup --name mynginx --exec-command "/bin/bash"
root@caas-83e6c883014b427f9b277a2bba3b7b5f-708716530-2qv47:/# ls
bin   dev  home  lib64  mnt  proc  run   srv  tmp  var
boot  etc  lib   media  opt  root  sbin  sys  usr
root@caas-83e6c883014b427f9b277a2bba3b7b5f-708716530-2qv47:/# exit
exit
Bye.
```

In dit voorbeeld wordt de opdrachtprompt in een actieve Nanoserver container gestart:

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

Als uw [containergroep](container-instances-container-groups.md) heeft meerdere containers, zoals een toepassingscontainer en een ter logboekregistratie, geef de naam van de container waarin u de opdracht met uitvoeren `--container-name`.

Bijvoorbeeld, in de containergroep *mynginx* zijn twee containers *nginx-app* en *berichtenlogboek*. Starten van een shell op de *nginx-app* container:

```azurecli
az container exec --resource-group myResourceGroup --name mynginx --container-name nginx-app --exec-command "/bin/bash"
```

## <a name="restrictions"></a>Beperkingen

Azure Containerexemplaren ondersteunt momenteel starten van een enkel proces met [az container exec][az-container-exec], en u niet de opdrachtargumenten doorgeven. Bijvoorbeeld, u opdrachten zoals in niet koppelen `sh -c "echo FOO && echo BAR"`, of het uitvoeren van `echo FOO`.

## <a name="next-steps"></a>Volgende stappen

Meer informatie over andere hulpprogramma's voor probleemoplossing en veelvoorkomende implementatieproblemen met in [container en implementatie problemen in Azure Containerexemplaren](container-instances-troubleshooting.md).

<!-- LINKS - internal -->
[az-container-create]: /cli/azure/container#az-container-create
[az-container-exec]: /cli/azure/container#az-container-exec
[azure-cli]: /cli/azure