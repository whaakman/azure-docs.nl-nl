---
title: OpenShift-implementatie in Azure oplossen | Microsoft Docs
description: Problemen met OpenShift-implementatie in Azure.
services: virtual-machines-linux
documentationcenter: virtual-machines
author: haroldwongms
manager: joraio
editor: ''
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 02/02/2019
ms.author: haroldw
ms.openlocfilehash: f94189e8f39771e0c3d7856ccbb8a23ba81d94aa
ms.sourcegitcommit: 7e772d8802f1bc9b5eb20860ae2df96d31908a32
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/06/2019
ms.locfileid: "57447271"
---
# <a name="troubleshoot-openshift-deployment-in-azure"></a>OpenShift-implementatie in Azure oplossen

Als het cluster OpenShift niet succesvol zijn geïmplementeerd, wordt de Azure-portal foutuitvoer opgeven. De uitvoer kan lastig zijn om te lezen, waardoor het lastig om het probleem te identificeren. Snel scan deze uitvoer voor afsluitcode 3, 4 of 5. Het volgende bevat informatie over deze drie afsluitcodes:

- Afsluitcode 3: Uw Red Hat-abonnement-gebruikersnaam / wachtwoord of organisatie-ID / activeringscode is onjuist
- Afsluitcode 4: Uw Red Hat groeps-ID is onjuist of er zijn geen rechten beschikbaar
- Afsluitcode 5: Kan niet worden ingericht Docker Thin Pool Volume

Voor alle andere afsluitcodes, verbinding maken met de host (s) via ssh als de logboekbestanden wilt weergeven.

**OpenShift Container Platform**

SSH naar de ansible-playbook-host. Voor de sjabloon of de Marketplace-aanbieding, gebruikt u de bastionhost. In de bastionomgeving kunt u SSH naar alle andere knooppunten in het cluster (master, infra, CNS, rekenen). U moet dit de hoofdmap om de logboekbestanden te raadplegen. Hoofdmap is uitgeschakeld voor de SSH-toegang standaard gebruik dus geen hoofdmap SSH naar andere knooppunten.

**OKD**

SSH naar de ansible-playbook-host. Gebruik de master-0-host voor de sjabloon OKD (versie 3,9 en lager). Gebruik de bastionhost voor de sjabloon OKD (versie 3.10 en hoger). Van de ansible-playbook-host kunt u SSH naar alle andere knooppunten in het cluster (master, infra, CNS, rekenen). U moet dit de hoofdmap (sudo su-) om de logboekbestanden te raadplegen. Hoofdmap is uitgeschakeld voor de SSH-toegang standaard gebruik dus geen hoofdmap SSH naar andere knooppunten.

## <a name="log-files"></a>Logboekbestanden

De logboekbestanden voor de host-voorbereiding-scripts (stderr en stdout) bevinden zich in /var/lib/waagent/custom-script/download/0 op alle hosts. Als er is een fout tijdens de voorbereiding van de host opgetreden, bekijkt u deze logboekbestanden om te bepalen van de fout.

Als de voorbereiding van scripts is uitgevoerd, klikt u vervolgens moet de logboekbestanden in de map /var/lib/waagent/custom-script/download/1 van de host ansible-playbook worden onderzocht. Als de fout is opgetreden tijdens de werkelijke installatie van OpenShift, wordt de stdout-bestand de volgende fout weergegeven. Deze informatie gebruiken om contact op met ondersteuning voor verdere ondersteuning.

Voorbeeld van uitvoer

```json
TASK [openshift_storage_glusterfs : Load heketi topology] **********************
fatal: [mycluster-master-0]: FAILED! => {"changed": true, "cmd": ["oc", "--config=/tmp/openshift-glusterfs-ansible-IbhnUM/admin.kubeconfig", "rsh", "--namespace=glusterfs", "deploy-heketi-storage-1-d9xl5", "heketi-cli", "-s", "http://localhost:8080", "--user", "admin", "--secret", "VuoJURT0/96E42Vv8+XHfsFpSS8R20rH1OiMs3OqARQ=", "topology", "load", "--json=/tmp/openshift-glusterfs-ansible-IbhnUM/topology.json", "2>&1"], "delta": "0:00:21.477831", "end": "2018-05-20 02:49:11.912899", "failed": true, "failed_when_result": true, "rc": 0, "start": "2018-05-20 02:48:50.435068", "stderr": "", "stderr_lines": [], "stdout": "Creating cluster ... ID: 794b285745b1c5d7089e1c5729ec7cd2\n\tAllowing file volumes on cluster.\n\tAllowing block volumes on cluster.\n\tCreating node mycluster-cns-0 ... ID: 45f1a3bfc20a4196e59ebb567e0e02b4\n\t\tAdding device /dev/sdd ... OK\n\t\tAdding device /dev/sde ... OK\n\t\tAdding device /dev/sdf ... OK\n\tCreating node mycluster-cns-1 ... ID: 596f80d7bbd78a1ea548930f23135131\n\t\tAdding device /dev/sdc ... Unable to add device: Unable to execute command on glusterfs-storage-4zc42:   Device /dev/sdc excluded by a filter.\n\t\tAdding device /dev/sde ... OK\n\t\tAdding device /dev/sdd ... OK\n\tCreating node mycluster-cns-2 ... ID: 42c0170aa2799559747622acceba2e3f\n\t\tAdding device /dev/sde ... OK\n\t\tAdding device /dev/sdf ... OK\n\t\tAdding device /dev/sdd ... OK", "stdout_lines": ["Creating cluster ... ID: 794b285745b1c5d7089e1c5729ec7cd2", "\tAllowing file volumes on cluster.", "\tAllowing block volumes on cluster.", "\tCreating node mycluster-cns-0 ... ID: 45f1a3bfc20a4196e59ebb567e0e02b4", "\t\tAdding device /dev/sdd ... OK", "\t\tAdding device /dev/sde ... OK", "\t\tAdding device /dev/sdf ... OK", "\tCreating node mycluster-cns-1 ... ID: 596f80d7bbd78a1ea548930f23135131", "\t\tAdding device /dev/sdc ... Unable to add device: Unable to execute command on glusterfs-storage-4zc42:   Device /dev/sdc excluded by a filter.", "\t\tAdding device /dev/sde ... OK", "\t\tAdding device /dev/sdd ... OK", "\tCreating node mycluster-cns-2 ... ID: 42c0170aa2799559747622acceba2e3f", "\t\tAdding device /dev/sde ... OK", "\t\tAdding device /dev/sdf ... OK", "\t\tAdding device /dev/sdd ... OK"]}

PLAY RECAP *********************************************************************
mycluster-cns-0       : ok=146  changed=57   unreachable=0    failed=0   
mycluster-cns-1       : ok=146  changed=57   unreachable=0    failed=0   
mycluster-cns-2       : ok=146  changed=57   unreachable=0    failed=0   
mycluster-infra-0     : ok=143  changed=55   unreachable=0    failed=0   
mycluster-infra-1     : ok=143  changed=55   unreachable=0    failed=0   
mycluster-infra-2     : ok=143  changed=55   unreachable=0    failed=0   
mycluster-master-0    : ok=502  changed=198  unreachable=0    failed=1   
mycluster-master-1    : ok=348  changed=140  unreachable=0    failed=0   
mycluster-master-2    : ok=348  changed=140  unreachable=0    failed=0   
mycluster-node-0      : ok=143  changed=55   unreachable=0    failed=0   
mycluster-node-1      : ok=143  changed=55   unreachable=0    failed=0   
localhost                  : ok=13   changed=0    unreachable=0    failed=0   

INSTALLER STATUS ***************************************************************
Initialization             : Complete (0:00:39)
Health Check               : Complete (0:00:24)
etcd Install               : Complete (0:01:24)
Master Install             : Complete (0:14:59)
Master Additional Install  : Complete (0:01:10)
Node Install               : Complete (0:10:58)
GlusterFS Install          : In Progress (0:03:33)
    This phase can be restarted by running: playbooks/openshift-glusterfs/config.yml

Failure summary:

  1. Hosts:    mycluster-master-0
     Play:     Configure GlusterFS
     Task:     Load heketi topology
     Message:  Failed without returning a message.
```

De meest voorkomende fouten tijdens de installatie zijn:

1. De persoonlijke sleutel heeft wachtwoordzin
2. Key vault-geheim met persoonlijke sleutel is niet correct gemaakt
3. Referenties voor service-principal zijn niet correct opgegeven
4. Service-principal heeft geen inzenderstoegang tot de resourcegroep

### <a name="private-key-has-a-passphrase"></a>Persoonlijke sleutel heeft een wachtwoordzin

Hier ziet u een foutbericht weergegeven dat machtiging is geweigerd voor SSH. SSH naar de ansible-playbook-host om te controleren op een wachtwoordzin op de persoonlijke sleutel.

### <a name="key-vault-secret-with-private-key-wasnt-created-correctly"></a>Key vault-geheim met persoonlijke sleutel is niet correct gemaakt

De persoonlijke sleutel is opgenomen in de host met ansible-playbook - ~/.ssh/id_rsa. Controleer of dat dit bestand juist is. Testen door een SSH-sessie op een van de clusterknooppunten van de host ansible-playbook te openen.

### <a name="service-principal-credentials-were-entered-incorrectly"></a>Referenties voor service-principal zijn niet correct opgegeven

Bij het opgeven van de invoer voor de sjabloon of Marketplace-aanbieding, wordt de onjuiste informatie werd geboden. Zorg ervoor dat u de juiste toepassings-id (clientId) en het wachtwoord (clientSecret) gebruiken voor de service-principal. Controleer of door de volgende azure cli-opdracht.

```bash
az login --service-principal -u <client id> -p <client secret> -t <tenant id>
```

### <a name="service-principal-doesnt-have-contributor-access-to-the-resource-group"></a>Service-principal heeft geen inzenderstoegang tot de resourcegroep

Als de Azure-cloud-provider is ingeschakeld, moet de service-principal gebruikt Inzender-toegang tot de resourcegroep hebben. Controleer of door de volgende azure cli-opdracht.

```bash
az group update -g <openshift resource group> --set tags.sptest=test
```

## <a name="additional-tools"></a>Aanvullende hulpprogramma 's

Voor sommige fouten, kunt u ook de volgende opdrachten gebruiken voor meer informatie:

1. systemctl status <service>
2. journalctl -xe
