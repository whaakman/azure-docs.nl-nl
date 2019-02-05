---
author: cynthn
ms.service: virtual-machines
ms.topic: include
ms.date: 10/26/2018
ms.author: cynthn
ms.openlocfilehash: ae29451e3f7ec263f296e69656a5c66045334687
ms.sourcegitcommit: 3aa0fbfdde618656d66edf7e469e543c2aa29a57
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/05/2019
ms.locfileid: "55735765"
---
1. Aanmelden bij uw Azure-abonnement met behulp van de stappen [verbinding maken met Azure via de klassieke Azure-CLI](/cli/azure/authenticate-azure-cli).

2. Zorg er als volgt voor dat u zich in de klassieke implementatiemodus bevindt:

    ```azurecli
    azure config mode asm
    ```

3. Doe het volgende om de Linux-installatiekopie te zoeken die u wilt laden:

   ```azurecli   
    azure vm image list | grep "Linux"
    ```
   
    Gebruik in een opdrachtpromptvenster van Windows **find** in plaats van grep.
   
4. Gebruik `azure vm create` om een virtuele machine te maken met de Linux-installatiekopie uit de vorige lijst. In deze stap maakt u een cloudservice en een opslagaccount. U kunt deze virtuele machine ook verbinden met een bestaande cloudservice met de optie `-c`. Maak een SSH-eindpunt voor aanmelding bij de virtuele Linux-machine met de optie `-e`. In het volgende voorbeeld wordt een virtuele machine met de naam `myVM` gemaakt met behulp van de installatiekopie `Ubuntu-14_04_4-LTS` op de locatie `West US`, en voegt u de gebruikersnaam `ops` toe:
   
    ```azurecli
    azure vm create myVM \
        b39f27a8b8c64d52b05eac6a62ebad85__Ubuntu-14_04_4-LTS-amd64-server-20160516-en-us-30GB \
        -g ops -p P@ssw0rd! -z "Small" -e -l "West US"
    ```

    De uitvoer lijkt op die in het volgende voorbeeld:

    ```azurecli
    info:    Executing command vm create
    + Looking up image b39f27a8b8c64d52b05eac6a62ebad85__Ubuntu-14_04_4-LTS-amd64-server-20160516-en-us-30GB
    + Looking up cloud service
    info:    cloud service myVM not found.
    + Creating cloud service
    + Retrieving storage accounts
    + Creating VM
    info:    vm create command OK
    ```
   
   > [!NOTE]
   > Voor een virtuele Linux-machine moet u de optie `-e` opgeven in `vm create`. Het is niet mogelijk om SSH in te schakelen nadat de virtuele machine is gemaakt. Zie voor meer informatie over SSH [SSH gebruiken met Linux op Azure](../articles/virtual-machines/linux/mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

5. U kunt de kenmerken van de virtuele machine controleren via de opdracht `azure vm show`. In het volgende voorbeeld worden gegevens weergegeven voor de virtuele machine met de naam `myVM`:

    ```azurecli   
    azure vm show myVM
    ```

6. Start de virtuele machine als volgt met de opdracht `azure vm start`:

    ```azurecli
    azure vm start myVM
    ```

## <a name="next-steps"></a>Volgende stappen
Lees voor meer informatie over deze Azure klassieke CLI virtuele machine-opdrachten voor het [met behulp van de klassieke Azure CLI met de klassieke implementatie API](https://docs.microsoft.com/cli/azure/get-started-with-az-cli2).

