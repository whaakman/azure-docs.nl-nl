---
title: Maken van een Ubuntu-Server voor NFS (Network File System) voor gebruik door schillen van Azure Kubernetes Service (AKS)
description: Meer informatie over het handmatig maken van een volume NFS Ubuntu Linux-Server voor gebruik met schillen in Azure Kubernetes Service (AKS)
services: container-service
author: ozboms
ms.service: container-service
ms.topic: article
ms.date: 4/25/2019
ms.author: obboms
ms.openlocfilehash: 55eb5b0b98a4097d2f300bacabbfef3b0a32b27b
ms.sourcegitcommit: e6d53649bfb37d01335b6bcfb9de88ac50af23bd
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/09/2019
ms.locfileid: "65468497"
---
# <a name="manually-create-and-use-an-nfs-network-file-system-linux-server-volume-with-azure-kubernetes-service-aks"></a>Handmatig maken en gebruiken van een volume van de Linux-Server voor NFS (Network File System) met Azure Kubernetes Service (AKS)
Delen van gegevens tussen containers is vaak een vereist onderdeel van op containers gebaseerde services en toepassingen. Gewoonlijk hebt u verschillende schillen die toegang nodig tot de dezelfde informatie op een externe permanent volume.    
Hoewel Azure files een optie is, is het maken van een NFS-Server op een Azure VM een andere vorm van permanente gedeelde opslag. 

Dit artikel wordt beschreven hoe u een NFS-Server op een Ubuntu-machine maakt. En ook uw AKS containers-toegang geven tot dit systeem voor gedeelde bestanden.

## <a name="before-you-begin"></a>Voordat u begint
In dit artikel wordt ervan uitgegaan dat u een bestaand AKS-Cluster hebt. Als u een Cluster AKS nodig hebt, raadpleegt u de Quick Start voor AKS [met de Azure CLI] [ aks-quickstart-cli] of [met behulp van de Azure-portal][aks-quickstart-portal].

Uw AKS-Cluster moet bevinden zich in hetzelfde of een gekoppelde virtuele netwerken als de NFS-Server. Het cluster moet worden gemaakt in een bestaand VNET, die hetzelfde VNET als de virtuele machine kan zijn.

De stappen voor het configureren van met een bestaand VNET worden beschreven in de documentatie: [AKS-Cluster maken in bestaande VNET] [ aks-virtual-network] en [verbinden van virtuele netwerken met VNET-peering][peer-virtual-networks]

Ook wordt ervan uitgegaan dat u een Ubuntu Linux-Machine (bijvoorbeeld 18.04 TNS) hebt gemaakt. Instellingen en de grootte kunnen naar wens en kunnen worden geïmplementeerd via Azure. Zie voor Linux-quickstart, [linux VM-beheer][linux-create].

Als u eerst uw AKS-Cluster implementeert, Azure wordt automatisch gevuld het veld van het virtuele netwerk bij het implementeren van uw Ubuntu-machine, waardoor ze live in hetzelfde VNET. Maar als u werken met gekoppelde netwerken in plaats daarvan wilt, raadpleegt u de bovenstaande documentatie.

## <a name="deploying-the-nfs-server-onto-a-virtual-machine"></a>Implementeren van de NFS-Server op een virtuele Machine
Dit is het script voor het instellen van een NFS-Server in uw virtuele Ubuntu-machine:
```bash
#!/bin/bash

# This script should be executed on Linux Ubuntu Virtual Machine

EXPORT_DIRECTORY=${1:-/export/data}
DATA_DIRECTORY=${2:-/data}
AKS_SUBNET=${3:-*}

echo "Updating packages"
apt-get -y update

echo "Installing NFS kernel server"

apt-get -y install nfs-kernel-server

echo "Making data directory ${DATA_DIRECTORY}"
mkdir -p ${DATA_DIRECTORY}

echo "Making new directory to be exported and linked to data directory: ${EXPORT_DIRECTORY}"
mkdir -p ${EXPORT_DIRECTORY}

echo "Mount binding ${DATA_DIRECTORY} to ${EXPORT_DIRECTORY}"
mount --bind ${DATA_DIRECTORY} ${EXPORT_DIRECTORY}

echo "Giving 777 permissions to ${EXPORT_DIRECTORY} directory"
chmod 777 ${EXPORT_DIRECTORY}

parentdir="$(dirname "$EXPORT_DIRECTORY")"
echo "Giving 777 permissions to parent: ${parentdir} directory"
chmod 777 $parentdir

echo "Appending bound directories into fstab"
echo "${DATA_DIRECTORY}    ${EXPORT_DIRECTORY}   none    bind  0  0" >> /etc/fstab

echo "Appending localhost and Kubernetes subnet address ${AKS_SUBNET} to exports configuration file"
echo "/export        ${AKS_SUBNET}(rw,async,insecure,fsid=0,crossmnt,no_subtree_check)" >> /etc/exports
echo "/export        localhost(rw,async,insecure,fsid=0,crossmnt,no_subtree_check)" >> /etc/exports

nohup service nfs-kernel-server restart
```
De server wordt opnieuw opgestart (vanwege het script) en u kunt de NFS-Server met AKS koppelen

>[!IMPORTANT]  
>Vervang de **AKS_SUBNET** met het juiste is van het cluster of ' * ' de NFS-Server op alle poorten en verbindingen wordt geopend.

Nadat u uw virtuele machine hebt gemaakt, kopieert u het script hierboven naar een bestand. Vervolgens kunt u het verplaatsen van uw lokale computer, of waar het script is, in de virtuele machine met behulp van: 
```console
scp /path/to/script_file username@vm-ip-address:/home/{username}
```
Nadat het script uw virtuele machine is, u kunt ssh in de virtuele machine en deze uitvoeren via de opdracht:
```console
sudo ./nfs-server-setup.sh
```
Als de uitvoering is mislukt vanwege fout toegang geweigerd, stelt u de machtiging kan worden uitgevoerd via de opdracht:
```console
chmod +x ~/nfs-server-setup.sh
```

## <a name="connecting-aks-cluster-to-nfs-server"></a>AKS-Cluster maken van verbinding met NFS-Server
We kunnen de NFS-Server verbinding met het cluster via een permanent volume en permanent volume-claim die bepaalt hoe de toegang tot het volume inrichten.  
De twee services in de dezelfde of een gekoppelde virtuele netwerken verbinding is nodig. Hier worden de instructies voor het instellen van het cluster in hetzelfde VNET: [AKS-Cluster maken in bestaande VNET][aks-virtual-network]

Zodra ze zich in hetzelfde virtuele netwerk (of aan elkaar gekoppeld), moet u voor het inrichten van een permanent volume en een permanent volume claim in uw AKS-Cluster. De containers kunnen vervolgens het NFS-station aan de lokale map koppelen.

Hier volgt een voorbeeld van de kubernetes-definitie voor de permanent volume (deze definitie wordt ervan uitgegaan dat uw cluster en de virtuele machine zich in hetzelfde VNET):

```yaml
apiVersion: v1
kind: PersistentVolume
metadata:
  name: <NFS_NAME>
  labels:
    type: nfs
spec:
  capacity:
    storage: 1Gi
  accessModes:
    - ReadWriteMany
  nfs:
    server: <NFS_INTERNAL_IP>
    path: <NFS_EXPORT_FILE_PATH>
```
Vervang **NFS_INTERNAL_IP**, **NFS_NAME** en **NFS_EXPORT_FILE_PATH** met informatie van de NFS-Server.

U moet ook een bestand van de claim permanent volume. Hier volgt een voorbeeld van wat u wilt opnemen:

>[!IMPORTANT]  
>**"storageClassName"** moet blijven een lege tekenreeks of de claim werkt niet.

```yaml
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: <NFS_NAME>
spec:
  accessModes:
    - ReadWriteMany
  storageClassName: ""
  resources:
    requests:
      storage: 1Gi
  selector: 
    matchLabels:
      type: nfs
```

## <a name="troubleshooting"></a>Probleemoplossing
Als u geen verbinding maken met de server uit een cluster, wordt een probleem mogelijk worden de geëxporteerde map of het bovenliggende object, beschikt niet over voldoende machtigingen voor toegang tot de server.

Controleer of zowel de exportmap als de bovenliggende map 777 machtigingen hebt.

U kunt machtigingen controleren door het uitvoeren van de volgende opdracht en de mappen hebt *'drwxrwxrwx'* machtigingen:
```console
ls -l
```

## <a name="more-information"></a>Meer informatie
Het overbrengen van een overzicht of om op te sporen van de NFS-Server-instellingen, is hier een uitgebreide zelfstudie:
  - [NFS-zelfstudie][nfs-tutorial]

## <a name="next-steps"></a>Volgende stappen

Zie voor de bijbehorende best practices, [aanbevolen procedures voor opslag en back-ups in AKS][operator-best-practices-storage].

<!-- LINKS - external -->
[kubernetes-volumes]: https://kubernetes.io/docs/concepts/storage/volumes/
[linux-create]: https://docs.microsoft.com/azure/virtual-machines/linux/tutorial-manage-vm
[nfs-tutorial]: https://help.ubuntu.com/community/SettingUpNFSHowTo#Pre-Installation_Setup
[aks-virtual-network]: https://docs.microsoft.com/azure/aks/configure-kubenet#create-an-aks-cluster-in-the-virtual-network
[peer-virtual-networks]: https://docs.microsoft.com/azure/virtual-network/tutorial-connect-virtual-networks-portal

<!-- LINKS - internal -->
[aks-quickstart-cli]: kubernetes-walkthrough.md
[aks-quickstart-portal]: kubernetes-walkthrough-portal.md
[operator-best-practices-storage]: operator-best-practices-storage.md
