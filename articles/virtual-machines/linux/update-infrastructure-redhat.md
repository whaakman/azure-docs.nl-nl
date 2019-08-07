---
title: Red Hat Update Infrastructure | Microsoft Docs
description: Meer informatie over Red Hat Update Infrastructure voor on-demand Red Hat Enterprise Linux-instanties in Microsoft Azure
services: virtual-machines-linux
documentationcenter: ''
author: BorisB2015
manager: gwallace
editor: ''
ms.assetid: f495f1b4-ae24-46b9-8d26-c617ce3daf3a
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 6/6/2019
ms.author: borisb
ms.openlocfilehash: efc76616151776bc2f766f92ff9503413c6037d0
ms.sourcegitcommit: 4b5dcdcd80860764e291f18de081a41753946ec9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/03/2019
ms.locfileid: "68774269"
---
# <a name="red-hat-update-infrastructure-for-on-demand-red-hat-enterprise-linux-vms-in-azure"></a>Red Hat Update Infrastructure voor on-demand Red Hat Enterprise Linux-machines in Azure
 [Red Hat Update Infrastructure](https://access.redhat.com/products/red-hat-update-infrastructure) (RHUI) kunt u cloudproviders, zoals Azure, voor het spiegelen van de inhoud van Red Hat gehoste opslagplaats, aangepaste opslagplaatsen maken met Azure-specifieke inhoud en het beschikbaar maken voor virtuele machines door eindgebruikers.

Red Hat Enterprise Linux (RHEL) betalen naar gebruik (betalen per gebruik) afbeeldingen afkomstig zijn vooraf geconfigureerd voor toegang tot Azure RHUI. Er is geen aanvullende configuratie nodig. Uitvoeren als u de meest recente updates, `sudo yum update` nadat uw RHEL-exemplaar gereed is. Deze service is opgenomen als onderdeel van de kosten voor de RHEL PAYG-software.

Meer informatie over RHEL-installatie kopieën in azure, met inbegrip van publicatie-en bewaar beleid, is [hier](./rhel-images.md)beschikbaar.

Informatie over Red Hat-ondersteunings beleid voor alle versies van RHEL vindt u op de pagina [levens cyclus van Red Hat Enterprise Linux](https://access.redhat.com/support/policy/updates/errata) .

## <a name="important-information-about-azure-rhui"></a>Belangrijke informatie over Azure RHUI
* Azure RHUI is de update-infra structuur die ondersteuning biedt voor alle RHEL PAYG-Vm's die zijn gemaakt in Azure. Dit verhindert u niet dat u uw PAYG RHEL-Vm's kunt registreren met abonnements Manager of satelliet of andere bron van updates, maar wel met een PAYG-VM, resulteert dit in indirect dubbel facturering. Zie het volgende punt voor meer informatie.
* Toegang tot de RHUI wordt gehost op Azure is opgenomen in de prijs van de installatiekopie RHEL PAYG. Als u een betalen per gebruik-RHEL VM uit de RHUI wordt gehost op Azure registratie is dat de virtuele machine niet converteren naar een type bring-your-own-license (BYOL) van virtuele machine. Als u dezelfde virtuele machine met een andere bron van de updates hebt geregistreerd, kunnen kosten voor _indirecte_ dubbele kosten in rekening gebracht. U betaalt het eerst voor de kosten van Azure RHEL-software. De tweede keer voor Red Hat-abonnementen die eerder zijn gekocht, moet u betalen. Als u een andere update-infra structuur dan een door Azure gehoste RHUI wilt gebruiken, kunt u overwegen om te registreren voor het gebruik van de [RHEL BYOS-installatie kopieën](https://aka.ms/rhel-byos).
* Het standaard gedrag van RHUI is om uw RHEL-VM bij te werken naar de meest recente secundaire `sudo yum update`versie wanneer u uitvoert.

    Bijvoorbeeld, als u een virtuele machine van een installatiekopie van een RHEL 7.4 PAYG inrichten en voer `sudo yum update`, krijgt u uiteindelijk met een 7.6 RHEL VM (de nieuwste secundaire versie in de familie RHEL7).

    Om dit gedrag te voor komen, kunt u overschakelen naar de ondersteunings [kanalen voor uitgebreide updates](#rhel-eus-and-version-locking-rhel-vms) of uw eigen installatie kopie bouwen, zoals beschreven in het artikel [een op Red Hat gebaseerde virtuele machine maken en uploaden voor Azure](redhat-create-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) . Als u uw eigen installatie kopie bouwt, moet u deze verbinden met een andere update-infra structuur ([rechtstreeks op een Red Hat content delivery servers](https://access.redhat.com/solutions/253273) of een [Red Hat Satellite-Server](https://access.redhat.com/products/red-hat-satellite)).



* RHEL SAP PAYG-installatie kopieën in azure (RHEL for SAP, RHEL for SAP HANA en RHEL for SAP Business Applications) zijn verbonden met toegewezen RHUI-kanalen die blijven gelden voor de specifieke RHEL-versie zoals vereist voor SAP-certificering.

* Toegang tot Azure gehoste RHUI is beperkt tot de virtuele machines binnen de [Azure datacenter IP-adresbereiken](https://www.microsoft.com/download/details.aspx?id=41653). Als u via een proxy alle VM-verkeer via een on-premises netwerkinfrastructuur, moet u mogelijk voor het instellen van de gebruiker gedefinieerde routes voor de RHEL-betalen per gebruik virtuele machines voor toegang tot de Azure-RHUI.

## <a name="rhel-eus-and-version-locking-rhel-vms"></a>RHEL EUS en versie-vergren delen RHEL Vm's
Sommige klanten willen hun RHEL-Vm's wellicht vergren delen met een bepaalde RHEL-kleine release. U kunt uw RHEL-VM met een specifieke secundaire versie vergren delen door de opslag plaatsen bij te werken zodat deze verwijzen naar de ondersteunings opslagplaatsen voor uitgebreide updates. U kunt ook de vergrendelings bewerking van de EUS-versie ongedaan maken.

>[!NOTE]
> EUS wordt niet ondersteund voor RHEL-Extra's. Dit betekent dat als u een pakket installeert dat doorgaans beschikbaar is via het RHEL Extrass-kanaal, u dit niet kunt doen wanneer u op EUS. De product levenscyclus van Red Hat extras wordt [hier](https://access.redhat.com/support/policy/updates/extras/)beschreven.

Op het moment van deze schrijf bewerking is de EUS-ondersteuning voor RHEL < = 7,3 beëindigd. Zie de sectie ' Red Hat Enterprise Linux meer ondersteuning voor invoeg toepassingen ' in de [Red Hat-documentatie](https://access.redhat.com/support/policy/updates/errata/) voor meer informatie.
* RHEL 7,4 EUS-ondersteuning eindigt op 31 augustus 2019
* RHEL 7,5 EUS support eindigt op 30 april 2020
* RHEL 7,6 EUS-ondersteuning eindigt op 31 oktober 2020

### <a name="switch-a-rhel-vm-to-eus-version-lock-to-a-specific-minor-version"></a>Een RHEL-VM overschakelen naar EUS (versie-vergren delen naar een specifieke secundaire versie)
Gebruik de volgende instructies om een RHEL-VM te vergren delen naar een bepaalde kleine release (uitvoeren als root):

>[!NOTE]
> Dit geldt alleen voor RHEL-versies waarvoor EUS beschikbaar is. Op het moment van deze schrijf bewerking omvat dit RHEL 7,2-7,6. Meer informatie vindt u op de pagina [Red Hat Enterprise Linux levens cyclus](https://access.redhat.com/support/policy/updates/errata) .

1. Niet-EUS-opslag plaatsen uitschakelen:
    ```bash
    yum --disablerepo='*' remove 'rhui-azure-rhel7'
    ```

1. EUS opslag plaatsen toevoegen:
    ```bash
    yum --config='https://rhelimage.blob.core.windows.net/repositories/rhui-microsoft-azure-rhel7-eus.config' install 'rhui-azure-rhel7-eus'
    ```

1. De variabele releasever vergren delen (uitvoeren als root):
    ```bash
    echo $(. /etc/os-release && echo $VERSION_ID) > /etc/yum/vars/releasever
    ```

    >[!NOTE]
    > Met de bovenstaande instructie wordt de RHEL-secundaire release vergrendeld op de huidige secundaire release. Voer een specifieke kleine release in als u een upgrade wilt uitvoeren en wilt vergren delen naar een latere secundaire versie die niet het meest recent is. Uw RHEL- `echo 7.5 > /etc/yum/vars/releasever` versie wordt bijvoorbeeld vergrendeld op RHEL 7,5

1. Uw RHEL-VM bijwerken
    ```bash
    sudo yum update
    ```

### <a name="switch-a-rhel-vm-back-to-non-eus-remove-a-version-lock"></a>Overschakelen van een RHEL-VM naar een niet-EUS (een versie vergrendeling verwijderen)
Voer het volgende uit als root:
1. Verwijder het releasever-bestand:
    ```bash
    rm /etc/yum/vars/releasever
     ```

1. EUS opslag plaatsen uitschakelen:
    ```bash
    yum --disablerepo='*' remove 'rhui-azure-rhel7-eus'
   ```

1. RHEL VM configureren
    ```bash
    yum --config='https://rhelimage.blob.core.windows.net/repositories/rhui-microsoft-azure-rhel7.config' install 'rhui-azure-rhel7'
    ```
    
1. Uw RHEL-VM bijwerken
    ```bash
    sudo yum update
    ```

## <a name="the-ips-for-the-rhui-content-delivery-servers"></a>De IP-adressen voor de levering van inhoud RHUI servers

RHUI is beschikbaar in alle regio's waar de RHEL-installatiekopieën voor op aanvraag beschikbaar zijn. Het bevat momenteel alle openbare regio's die worden vermeld op de [Azure-statusdashboard](https://azure.microsoft.com/status/) pagina-, Azure US Government- en Microsoft Azure Duitsland-regio's.

Als u verder beperken van toegang van RHEL betalen per gebruik virtuele machines in een configuratie van het netwerk, zorg ervoor dat de volgende IP-adressen zijn toegestaan voor `yum update` om te werken, afhankelijk van de omgeving waarin u werkt in:


```
# Azure Global
13.91.47.76
40.85.190.91
52.187.75.218
52.174.163.213
52.237.203.198

# Azure US Government
13.72.186.193
13.72.14.155
52.244.249.194

# Azure Germany
51.5.243.77
51.4.228.145
```

## <a name="azure-rhui-infrastructure"></a>Azure RHUI-infra structuur


### <a name="update-expired-rhui-client-certificate-on-a-vm"></a>Verlopen RHUI-clientcertificaat op een virtuele machine bijwerken

Als u een oudere RHEL-VM-installatie kopie gebruikt, bijvoorbeeld RHEL 7,4 (installatie kopie urn `RedHat:RHEL:7.4:7.4.2018010506`:), treden er connectiviteits problemen op RHUI vanwege een nu-verlopen SSL-client certificaat. De fout die u ziet, kan eruitzien _als "SSL-peer heeft uw certificaat afgewezen als verlopen"_ of _"fout: Kan de meta gegevens van de opslag plaats (repomd. XML) niet ophalen voor de opslag plaats:... Controleer het pad en probeer het opnieuw_. Als u dit probleem wilt verhelpen, moet u het RHUI-client pakket op de VM bijwerken met de volgende opdracht:

```bash
sudo yum update -y --disablerepo='*' --enablerepo='*microsoft*'
```

U kunt ook het `sudo yum update` client certificaat pakket bijwerken (afhankelijk van uw RHEL-versie), ondanks dat er fouten met het ' verlopen SSL-certificaat ' worden weer geven voor andere opslag plaatsen. Als deze update is geslaagd, moet de normale connectiviteit met andere RHUI-opslag plaatsen worden hersteld, zodat u kunt worden uitgevoerd `sudo yum update` .

Als er een 404-fout optreedt tijdens het uitvoeren `yum update`van een, voert u de volgende handelingen uit om de yum-cache te vernieuwen:
```bash
sudo yum clean all;
sudo yum makecache
```

### <a name="troubleshoot-connection-problems-to-azure-rhui"></a>Problemen met verbinding naar Azure RHUI
Als u problemen hebt met het verbinding maken met Azure RHUI vanuit uw virtuele machine van Azure RHEL betalen per gebruik, volg deze stappen:

1. Inspecteer de configuratie van de virtuele machine voor het Azure-RHUI-eindpunt:

    1. Controleer of de `/etc/yum.repos.d/rh-cloud.repo` bestand bevat een verwijzing naar `rhui-[1-3].microsoft.com` in de `baseurl` van de `[rhui-microsoft-azure-rhel*]` sectie van het bestand. Als dit het geval is, gebruikt u de nieuwe Azure-RHUI.

    1. Als deze naar een locatie met het volgende patroon verwijst `mirrorlist.*cds[1-4].cloudapp.net`, een configuratie-update is vereist. U de oude VM-momentopname en moet u deze om te verwijzen naar de nieuwe Azure-RHUI bij te werken.

1. Toegang tot Azure gehoste RHUI is beperkt tot virtuele machines binnen de [Azure datacenter IP-adresbereiken](https://www.microsoft.com/download/details.aspx?id=41653).

1. Als u de nieuwe configuratie, hebt gecontroleerd dat de virtuele machine verbinding vanuit de Azure-IP-adresbereik maakt en nog steeds geen verbinding met Azure RHUI, bestand een ondersteuningscase met Microsoft of Red Hat maken.

### <a name="infrastructure-update"></a>Infrastructuur update

In September 2016 gaan we een bijgewerkte Azure RHUI geïmplementeerd. In April 2017 wordt de oude Azure RHUI afgesloten. Als u eerder hebt gebruikt de PAYG RHEL-installatiekopieën (of hun momentopnamen) vanaf September 2016 of hoger, bent u automatisch verbinding met de nieuwe Azure-RHUI. Als u oudere momentopnamen echter op uw virtuele machines hebt, moet u handmatig bijwerken hun configuratie voor toegang tot de Azure-RHUI zoals beschreven in een volgende sectie.

De nieuwe Azure RHUI-servers worden geïmplementeerd met [Azure Traffic Manager](https://azure.microsoft.com/services/traffic-manager/). In Traffic Manager, één eindpunt (rhui-1.microsoft.com) kan worden gebruikt door een virtuele machine, ongeacht de regio.

### <a name="manual-update-procedure-to-use-the-azure-rhui-servers"></a>Handmatige updateprocedure voor het gebruik van de Azure-RHUI-servers
Deze procedure is alleen ter informatie bedoeld. Er is al de juiste configuratie verbinding maken met Azure RHUI PAYG RHEL-installatiekopieën. Voor het handmatig bijwerken van de configuratie voor het gebruik van de Azure-RHUI-servers, voert u de volgende stappen uit:

- Voor RHEL 6:
  ```bash
  yum --config='https://rhelimage.blob.core.windows.net/repositories/rhui-microsoft-azure-rhel6.config' install 'rhui-azure-rhel6'
  ```

- Voor RHEL 7:
  ```bash
  yum --config='https://rhelimage.blob.core.windows.net/repositories/rhui-microsoft-azure-rhel7.config' install 'rhui-azure-rhel7'
  ```

## <a name="next-steps"></a>Volgende stappen
* Een Red Hat Enterprise Linux-VM maken vanaf een betalen per gebruik van Azure Marketplace-installatiekopie en RHUI wordt gehost op Azure gebruiken, gaat u naar de [Azure Marketplace](https://azure.microsoft.com/marketplace/partners/redhat/).
* Ga naar de [documentatie pagina](./rhel-images.md)voor meer informatie over de Red Hat-afbeeldingen in Azure.
* Informatie over Red Hat-ondersteunings beleid voor alle versies van RHEL vindt u op de pagina [levens cyclus van Red Hat Enterprise Linux](https://access.redhat.com/support/policy/updates/errata) .
