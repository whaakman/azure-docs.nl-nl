---
title: Red Hat Update Infrastructure | Microsoft Docs
description: Meer informatie over Red Hat Update Infrastructure voor on-demand Red Hat Enterprise Linux-instanties in Microsoft Azure
services: virtual-machines-linux
documentationcenter: ''
author: BorisB2015
manager: jeconnoc
editor: ''
ms.assetid: f495f1b4-ae24-46b9-8d26-c617ce3daf3a
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 6/6/2019
ms.author: borisb
ms.openlocfilehash: 4315a849f3f117633f5f6a9d93c995ece9f527a3
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "67077008"
---
# <a name="red-hat-update-infrastructure-for-on-demand-red-hat-enterprise-linux-vms-in-azure"></a>Red Hat Update Infrastructure voor on-demand Red Hat Enterprise Linux-machines in Azure
 [Red Hat Update Infrastructure](https://access.redhat.com/products/red-hat-update-infrastructure) (RHUI) kunt u cloudproviders, zoals Azure, voor het spiegelen van de inhoud van Red Hat gehoste opslagplaats, aangepaste opslagplaatsen maken met Azure-specifieke inhoud en het beschikbaar maken voor virtuele machines door eindgebruikers.

Red Hat Enterprise Linux (RHEL) betalen naar gebruik (betalen per gebruik) afbeeldingen afkomstig zijn vooraf geconfigureerd voor toegang tot Azure RHUI. Er is geen aanvullende configuratie nodig. Uitvoeren als u de meest recente updates, `sudo yum update` nadat uw RHEL-exemplaar gereed is. Deze service is opgenomen als onderdeel van de kosten voor de RHEL PAYG-software.

Meer informatie over de RHEL-installatiekopieën in Azure, met inbegrip van publicatie- en bewaarbeleid, is beschikbaar [hier](./rhel-images.md).

Informatie over het beleid voor Red Hat-ondersteuning voor alle versies van RHEL vindt u op de [Red Hat Enterprise Linux-levenscyclus](https://access.redhat.com/support/policy/updates/errata) pagina.

## <a name="important-information-about-azure-rhui"></a>Belangrijke informatie over Azure RHUI
* Azure RHUI is de update-infrastructuur die ondersteuning biedt voor alle RHEL betalen per gebruik virtuele machines in Azure hebt gemaakt. Dit betekent niet dat u het registreren van uw betalen per gebruik-RHEL VM's met Doelabonnementbeheerder of satelliet of een andere bron van updates, maar doet dit met een VM PAYG zal leiden tot indirecte double-facturering. Zie het volgende punt voor meer informatie.
* Toegang tot de RHUI wordt gehost op Azure is opgenomen in de prijs van de installatiekopie RHEL PAYG. Als u een betalen per gebruik-RHEL VM uit de RHUI wordt gehost op Azure registratie is dat de virtuele machine niet converteren naar een type bring-your-own-license (BYOL) van virtuele machine. Als u dezelfde virtuele machine met een andere bron van de updates hebt geregistreerd, kunnen kosten voor _indirecte_ dubbele kosten in rekening gebracht. U betaalt het eerst voor de kosten van Azure RHEL-software. De tweede keer voor Red Hat-abonnementen die eerder zijn gekocht, moet u betalen. Als u consistent gebruik van een update-infrastructuur dan RHUI wordt gehost op Azure moet, kunt u registreren voor het gebruik van de [BYOS RHEL-installatiekopieën](https://aka.ms/rhel-byos).
* Het standaardgedrag van RHUI is uw RHEL VM bijwerken naar de nieuwste secundaire versie tijdens het uitvoeren van `sudo yum update`.

    Bijvoorbeeld, als u een virtuele machine van een installatiekopie van een RHEL 7.4 PAYG inrichten en voer `sudo yum update`, krijgt u uiteindelijk met een 7.6 RHEL VM (de nieuwste secundaire versie in de familie RHEL7).

    Om te voorkomen dat dit probleem, kunt u overschakelen naar [Update-ondersteuningskanalen uitgebreid](#rhel-eus-and-version-locking-rhel-vms) of bouw uw eigen installatiekopie, zoals beschreven in de [maken en uploaden, een Red Hat gebaseerde virtuele machine voor Azure](redhat-create-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) artikel. Als u uw eigen installatiekopie maakt, moet u verbinding maakt met een andere update-infrastructuur ([rechtstreeks naar Red Hat content delivery servers](https://access.redhat.com/solutions/253273) of een [Red Hat satelliet server](https://access.redhat.com/products/red-hat-satellite)).



* SAP betalen per gebruik RHEL-installatiekopieën in Azure (RHEL for SAP, RHEL for SAP HANA en RHEL for SAP Business Applications) zijn verbonden met specifieke RHUI-kanalen die op de specifieke RHEL secundaire versie zoals vereist voor de SAP-certificering blijven.

* Toegang tot Azure gehoste RHUI is beperkt tot de virtuele machines binnen de [Azure datacenter IP-adresbereiken](https://www.microsoft.com/download/details.aspx?id=41653). Als u via een proxy alle VM-verkeer via een on-premises netwerkinfrastructuur, moet u mogelijk voor het instellen van de gebruiker gedefinieerde routes voor de RHEL-betalen per gebruik virtuele machines voor toegang tot de Azure-RHUI.

## <a name="rhel-eus-and-version-locking-rhel-vms"></a>RHEL EUS en versie vergrendelen RHEL VM 's
Sommige klanten willen hun RHEL VM's naar een bepaalde kleine RHEL-versie vergrendelen. U kunt uw RHEL VM naar een specifieke secundaire versie versie-lock door bij te werken van de opslagplaatsen om te verwijzen naar de uitgebreide ondersteuning voor Update-opslagplaatsen. U kunt ook de EUS versie vergrendelen bewerking ongedaan te maken.

>[!NOTE]
> EUS wordt niet ondersteund op RHEL extra's. Dit betekent dat als u een pakket dat is gewoonlijk beschikbaar in het kanaal RHEL extra's installeert, kunt u niet zich om dit te doen terwijl op EUS. De levenscyclus van Red Hat extra's wordt toegelicht [hier](https://access.redhat.com/support/policy/updates/extras/).

Op het moment van dit artikel is geschreven, EUS ondersteuning is beëindigd voor RHEL < = 7.3. Zie de sectie 'Red Hat Enterprise Linux langer ondersteuning voor invoegtoepassingen' in de [Red Hat documentatie](https://access.redhat.com/support/policy/updates/errata/) voor meer informatie.
* RHEL 7.4 EUS ondersteuning wordt beëindigd en met 31 augustus 2019
* RHEL 7.5 EUS ondersteuning wordt beëindigd op 30 April 2020
* RHEL 7.6 EUS ondersteuning wordt beëindigd op 31 oktober 2020

### <a name="switch-a-rhel-vm-to-eus-version-lock-to-a-specific-minor-version"></a>Een RHEL VM overschakelen naar EUS (versie-lock naar een specifieke secundaire versie)
Gebruik de volgende instructies om te vergrendelen van een RHEL VM naar een specifieke secundaire versie (uitvoeren als hoofdgebruiker):

>[!NOTE]
> Dit geldt alleen voor RHEL-versies waarvoor EUS beschikbaar is. Dit omvat RHEL 7.2 7.6 op het moment van schrijven. Meer details zijn beschikbaar op de [Red Hat Enterprise Linux-levenscyclus](https://access.redhat.com/support/policy/updates/errata) pagina.

1. Niet-EUS opslagplaatsen uitschakelen:
    ```bash
    yum --disablerepo='*' remove 'rhui-azure-rhel7'
    ```

1. EUS opslagplaatsen toevoegen:
    ```bash
    yum --config='https://rhelimage.blob.core.windows.net/repositories/rhui-microsoft-azure-rhel7-eus.config' install 'rhui-azure-rhel7-eus'
    ```

1. Vergrendel de releasever variabele (uitvoeren als hoofdgebruiker):
    ```bash
    echo $(. /etc/os-release && echo $VERSION_ID) > /etc/yum/vars/releasever
    ```

    >[!NOTE]
    > De bovenstaande instructies, de secundaire versie van RHEL naar de huidige secundaire versie wordt vergrendeld. Voer een specifieke secundaire versie als u wilt bijwerken en een vergrendeling van naar een hoger secundaire versie aan die niet de nieuwste versie. Bijvoorbeeld, `echo 7.5 > /etc/yum/vars/releasever` uw RHEL-versie op RHEL 7,5 wordt vergrendeld

1. Uw RHEL VM bijwerken
    ```bash
    sudo yum update
    ```

### <a name="switch-a-rhel-vm-back-to-non-eus-remove-a-version-lock"></a>Een RHEL VM wilt terugkeren naar niet-EUS (Verwijder de vergrendeling van een versie)
Voer het volgende als hoofdmap:
1. Verwijder het bestand releasever:
    ```bash
    rm /etc/yum/vars/releasever
     ```

1. EUS opslagplaatsen uitschakelen:
    ```bash
    yum --disablerepo='*' remove 'rhui-azure-rhel7-eus'
   ```

1. Uw RHEL VM bijwerken
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

## <a name="azure-rhui-infrastructure"></a>Azure RHUI-infrastructuur


### <a name="update-expired-rhui-client-certificate-on-a-vm"></a>Verlopen RHUI-clientcertificaat op een virtuele machine bijwerken

Als u een oudere RHEL VM-installatiekopie, bijvoorbeeld RHEL 7.4 (image-URN: `RedHat:RHEL:7.4:7.4.2018010506`), wordt er verbindingsproblemen met RHUI vanwege een nu verlopen SSL-clientcertificaat. De fout die u ziet eruit als _'SSL peer geweigerd uw certificaat als verlopen'_ of _' fout: Kan de opslagplaats metagegevens (repomd.xml) voor de opslagplaats niet ophalen:... Controleer of het pad en probeer het opnieuw"_ . Werk de RHUI-clientpakket op de virtuele machine met behulp van de volgende opdracht uit om dit probleem oplossen:

```bash
sudo yum update -y --disablerepo='*' --enablerepo='*microsoft*'
```

U kunt ook uitgevoerd `sudo yum update` het clientpakket certificaat (afhankelijk van de RHEL-versie), kan ook worden bijgewerkt ondanks fouten van 'verlopen SSL-certificaat' u voor andere opslagplaatsen ziet. Als deze update geslaagd is, normale connectiviteit met andere RHUI-opslagplaatsen moet worden hersteld, zodat u kunt uitvoeren `sudo yum update` is.

Als u een 404-fout tijdens de uitvoering een `yum update`, probeert u het volgende als u wilt vernieuwen van uw yum-cache:
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

### <a name="infrastructure-update"></a>Infrastructuur-update

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
* Voor meer informatie over de Red Hat-afbeeldingen in Azure, gaat u naar de [documentatiepagina](./rhel-images.md).
* Informatie over het beleid voor Red Hat-ondersteuning voor alle versies van RHEL vindt u op de [Red Hat Enterprise Linux-levenscyclus](https://access.redhat.com/support/policy/updates/errata) pagina.
