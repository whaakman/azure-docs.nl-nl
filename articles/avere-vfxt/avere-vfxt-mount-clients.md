---
title: Koppel de Avere vFXT - Azure
description: Over het koppelen van clients met Avere vFXT voor Azure
author: ekpgh
ms.service: avere-vfxt
ms.topic: conceptual
ms.date: 10/31/2018
ms.author: v-erkell
ms.openlocfilehash: 41065b4ac6bc486e204c2bfd72b78ba8722270c4
ms.sourcegitcommit: 359b0b75470ca110d27d641433c197398ec1db38
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/07/2019
ms.locfileid: "55809159"
---
# <a name="mount-the-avere-vfxt-cluster"></a>Het Avere vFXT-cluster koppelen  

Volg deze stappen voor het client-computers verbinden met uw cluster vFXT.

1. Bepalen hoe u taakverdeling clientverkeer tussen de clusterknooppunten. Lezen [saldo clientbelasting](#balance-client-load)hieronder voor meer informatie. 
1. Geef het IP-adres en koppelingspunten pad om te koppelen.
1. Probleem de [koppelingsopdracht](#mount-command-arguments), met de juiste argumenten.

## <a name="balance-client-load"></a>De clientbelasting saldo

Om te helpen de clientaanvragen balans tussen alle knooppunten in het cluster, moet u clients op het volledige bereik van IP-adressen voor clientgerichte koppelen. Er zijn enkele eenvoudige manieren voor het automatiseren van deze taak.

> [!TIP] 
> Andere methoden voor taakverdeling mogelijk geschikt voor grote of complexe systemen; [open een ondersteuningsticket](avere-vfxt-open-ticket.md#open-a-support-ticket-for-your-avere-vfxt) voor meer informatie over.)
> 
> Als u liever met een DNS-server voor automatische serverzijde taakverdeling, moet u instellen en beheren van uw eigen DNS-server in Azure. In dat geval kunt u DNS round robin voor het cluster vFXT op basis van dit document: [De DNS-clusterconfiguratie Avere](avere-vfxt-configure-dns.md).

### <a name="sample-balanced-client-mounting-script"></a>Voorbeeld met gelijke taakverdeling client script koppelen

Dit codevoorbeeld maakt gebruik van client-IP-adressen als een randomizing element voor het distribueren van clients op alle beschikbare IP-adressen van het cluster vFXT.

```bash
function mount_round_robin() {
    # to ensure the nodes are spread out somewhat evenly the default 
    # mount point is based on this node's IP octet4 % vFXT node count.
    declare -a AVEREVFXT_NODES="($(echo ${NFS_IP_CSV} | sed "s/,/ /g"))"
    OCTET4=$((`hostname -i | sed -e 's/^.*\.\([0-9]*\)/\1/'`))
    DEFAULT_MOUNT_INDEX=$((${OCTET4} % ${#AVEREVFXT_NODES[@]}))
    ROUND_ROBIN_IP=${AVEREVFXT_NODES[${DEFAULT_MOUNT_INDEX}]}

    DEFAULT_MOUNT_POINT="${BASE_DIR}/default"

    # no need to write again if it is already there
    if ! grep --quiet "${DEFAULT_MOUNT_POINT}" /etc/fstab; then
        echo "${ROUND_ROBIN_IP}:${NFS_PATH}    ${DEFAULT_MOUNT_POINT}    nfs hard,nointr,proto=tcp,mountproto=tcp,retry=30 0 0" >> /etc/fstab
        mkdir -p "${DEFAULT_MOUNT_POINT}"
        chown nfsnobody:nfsnobody "${DEFAULT_MOUNT_POINT}"
    fi
    if ! grep -qs "${DEFAULT_MOUNT_POINT} " /proc/mounts; then
        retrycmd_if_failure 12 20 mount "${DEFAULT_MOUNT_POINT}" || exit 1
    fi   
} 
```

De bovenstaande functie maakt deel uit van de Batch-voorbeeld beschikbaar in de [Avere vFXT voorbeelden](https://github.com/Azure/Avere#tutorials) site.

## <a name="create-the-mount-command"></a>Met deze opdracht maken 

> [!NOTE]
> Als u een nieuwe blobcontainer niet hebt gemaakt bij het maken van uw Avere vFXT cluster, volg de stappen in [opslag configureren](avere-vfxt-add-storage.md) voordat u probeert te koppelen van clients.

Van de client de ``mount`` opdracht wordt de virtuele server (vserver) toegewezen op het cluster vFXT naar een pad op het lokale bestandssysteem. De indeling is ``mount <vFXT path> <local path> {options}``

Er zijn drie elementen met de koppelopdracht: 

* vFXT pad - (een combinatie van IP-adres en naamruimte koppelingspunten pad die hieronder worden beschreven)
* lokale pad - het pad op de client 
* Opties voor de opdracht - koppelen (die worden vermeld in [koppelen opdrachtargumenten](#mount-command-arguments))

### <a name="junction-and-ip"></a>Verbindings- en IP-

Het pad vserver is een combinatie van de *IP-adres* plus het pad naar een *naamruimte koppelingspunten*. De verbinding van de naamruimte is een virtueel pad dat is gedefinieerd als het opslagsysteem is toegevoegd.

Als uw cluster is gemaakt met Blob-opslag, is het pad naar de naamruimte `/msazure`

Voorbeeld: ``mount 10.0.0.12:/msazure /mnt/vfxt``

Als u opslag hebt toegevoegd na het maken van het cluster, de verbindings-naamruimtepad komt overeen met de waarde die u instelt in **Namespace pad** bij het maken van de verbinding. Als u gebruikt bijvoorbeeld ``/avere/files`` als naamruimtepad voor uw, uw clients koppelt *IP-adres*: / avere/bestanden naar hun lokale koppelpunt.

![Dialoogvenster met/avere/bestanden in het veld voor de naamruimte-pad 'Nieuwe verbinding toevoegen'](media/avere-vfxt-create-junction-example.png)


Het IP-adres is een van de clientgerichte IP-adressen die zijn gedefinieerd voor de vserver. U kunt het bereik van IP-adressen op twee plaatsen in het Configuratiescherm Avere clientgerichte vinden:

* **VServers** tabel (tabblad Dashboard -) 

  ![Dashboardtabblad van het Configuratiescherm Avere met het tabblad VServer is geselecteerd in de tabel onder de grafiek en het IP-adressectie omcirkeld](media/avere-vfxt-ip-addresses-dashboard.png)

* **Client naar gericht netwerk** van de instellingenpagina: 

  ![Instellingen > VServer > configuratiepagina van de Client naar gericht netwerk met een cirkel om de sectie-adresbereik van de tabel voor een bepaalde vserver](media/avere-vfxt-ip-addresses-settings.png)

Naast de paden, bevatten de [koppelen opdrachtargumenten](#mount-command-arguments) die hieronder worden beschreven bij het koppelen van elke client.

### <a name="mount-command-arguments"></a>Opdrachtargumenten koppelen

Doorgeven om te controleren of een naadloze client-koppeling, deze instellingen en de argumenten in de mount-opdracht: 

``mount -o hard,nointr,proto=tcp,mountproto=tcp,retry=30 ${VSERVER_IP_ADDRESS}:/${NAMESPACE_PATH} ${LOCAL_FILESYSTEM_MOUNT_POINT}``


| Vereiste instellingen | |
--- | --- 
``hard`` | Zachte koppelingen aan het cluster vFXT zijn gekoppeld aan problemen met toepassingen en mogelijk gegevensverlies. 
``proto=netid`` | Deze optie ondersteunt juiste verwerking van NFS-netwerkfouten.
``mountproto=netid`` | Deze optie ondersteunt juiste verwerking van netwerkfouten voor mount-bewerkingen.
``retry=n`` | Stel ``retry=30`` ter voorkoming van fouten van tijdelijke koppelen. (Een andere waarde wordt aanbevolen in koppelingen van voorgrond.)

| Aanbevolen instellingen  | |
--- | --- 
``nointr``            | De optie 'nointr' is verkozen voor clients met oudere kernels (vóór April 2008) die ondersteuning bieden voor deze optie. Houd er rekening mee dat de optie "Intro" de standaardinstelling is.


## <a name="next-steps"></a>Volgende stappen

Nadat u clients hebt gekoppeld, kunt u ze kunt gebruiken voor het vullen van de back-end-gegevensopslag (core filer). Verwijzen naar deze documenten voor meer informatie over aanvullende instellingstaken:

* [Gegevens verplaatsen naar de cluster core filer](avere-vfxt-data-ingest.md) -het gebruik van meerdere clients en het aantal threads het efficiënt uploaden van uw gegevens
* [Aanpassen cluster afstemmen](avere-vfxt-tuning.md) -de clusterinstellingen aan de behoeften van uw workload passen
* [Het cluster beheren](avere-vfxt-manage-cluster.md) -het starten of stoppen van het cluster en knooppunten beheren
