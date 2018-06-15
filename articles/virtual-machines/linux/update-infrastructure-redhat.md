---
title: Red Hat Update-infrastructuur | Microsoft Docs
description: Meer informatie over Red Hat Update-infrastructuur voor bellen op Red Hat Enterprise Linux-exemplaren in Microsoft Azure
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
ms.date: 04/02/2018
ms.author: borisb
ms.openlocfilehash: b69cc226ca5b4f48747b033e0da5e7f991be112e
ms.sourcegitcommit: 5b2ac9e6d8539c11ab0891b686b8afa12441a8f3
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/06/2018
ms.locfileid: "30915461"
---
# <a name="red-hat-update-infrastructure-for-on-demand-red-hat-enterprise-linux-vms-in-azure"></a>Red Hat Update-infrastructuur voor on-demand Red Hat Enterprise Linux virtuele machines in Azure
 [Red Hat Update-infrastructuur](https://access.redhat.com/products/red-hat-update-infrastructure) (RHUI) kunt u cloudproviders, zoals Azure, voor het spiegelen van de inhoud van Red Hat gehost opslagplaats, aangepaste opslagplaatsen met Azure-specifieke inhoud maken en beschikbaar maken voor virtuele machines door eindgebruikers.

Red Hat Enterprise Linux (RHEL) betalen naar gebruik (Omslagstelsel) installatiekopieën afkomstig zijn vooraf geconfigureerd voor toegang tot Azure RHUI. Geen aanvullende configuratie nodig. Uitvoeren als u de meest recente updates, `sudo yum update` nadat uw RHEL-exemplaar gereed is. Deze service is opgenomen als onderdeel van de software RHEL PAYG kosten.

## <a name="important-information-about-azure-rhui"></a>Belangrijke informatie over Azure RHUI
* Azure RHUI ondersteunt momenteel alleen de nieuwste secundaire versie in de familie van elke RHEL (RHEL6 of RHEL7). Als u een exemplaar RHEL VM is verbonden met RHUI naar de nieuwste secundaire versie upgraden, uitvoeren `sudo yum update`.

    Bijvoorbeeld, als u een virtuele machine van een installatiekopie RHEL 7.2 Omslagstelsel inrichten en voer `sudo yum update`, u uiteindelijk eindigen met een RHEL 7.4 VM (de nieuwste secundaire versie in de familie RHEL7).

    U kunt dit gedrag vermijden, moet u voor het bouwen van uw eigen installatiekopie, zoals beschreven in de [maken en uploaden een Red Hat-virtuele machine voor Azure](redhat-create-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) artikel. Moet u deze verbinding maken met een andere update-infrastructuur ([inhoud rechtstreeks naar Red Hat levering servers](https://access.redhat.com/solutions/253273) of een [Red Hat satelliet server](https://access.redhat.com/products/red-hat-satellite)).

* Toegang tot de Azure gehoste RHUI is opgenomen in de afbeelding RHEL PAYG prijs. Als u een Omslagstelsel RHEL VM van de Azure gehoste RHUI registratie ongedaan maken die de virtuele machine naar een type bring-your-eigenaar-license (BYOL) van de virtuele machine niet geconverteerd. Als u dezelfde virtuele machine met een andere bron van updates registreert, kunt u oplopen _indirecte_ kosten verdubbelen. U bent in rekening gebracht de eerste keer voor de kosten van Azure RHEL software. U bent in rekening gebracht de tweede keer voor Red Hat abonnementen die eerder zijn gekocht. Als u consistent moet gebruik van een update-infrastructuur dan Azure gehoste RHUI, kunt u overwegen maken en implementeren van uw eigen installatiekopieën (BYOL-type). Dit proces wordt beschreven in [maken en uploaden een Red Hat-virtuele machine voor Azure](redhat-create-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

* Twee klassen van RHEL PAYG afbeeldingen in Azure (RHEL voor SAP HANA) en RHEL voor SAP Business-toepassingen zijn verbonden met speciale RHUI kanalen die op de specifieke RHEL secundaire versie zoals vereist voor SAP-certificering blijven. 

* Toegang tot Azure gehoste RHUI is beperkt tot de virtuele machines binnen de [Azure datacenter IP-adresbereiken](https://www.microsoft.com/download/details.aspx?id=41653). Als u via een proxy alle VM-verkeer via een netwerk on-premises infrastructuur, moet u mogelijk voor het instellen van de gebruiker gedefinieerde routes voor de RHEL Omslagstelsel virtuele machines voor toegang tot de Azure-RHUI.

### <a name="the-ips-for-the-rhui-content-delivery-servers"></a>De IP-adressen voor het leveren van inhoud RHUI servers

RHUI is beschikbaar in alle regio's waar RHEL op aanvraag installatiekopieën beschikbaar zijn. Bevat momenteel alle openbare regio's die worden weergegeven op de [Azure status dashboard](https://azure.microsoft.com/status/) pagina, Azure US Government en Duitse van Microsoft Azure-regio's. 

Als u de toegang verder te beperken van RHEL Omslagstelsel VM's in een netwerkconfiguratie, zorg ervoor dat de volgende IP's zijn toegestaan voor `yum update` werken, afhankelijk van de omgeving in: 

```
# Azure Global
13.91.47.76
40.85.190.91
52.187.75.218
52.174.163.213
52.237.203.198

# Azure US Government
13.72.186.193

# Azure Germany
51.5.243.77
51.4.228.145
```

## <a name="rhui-azure-infrastructure-update"></a>Update van RHUI Azure-infrastructuur

In September 2016 krijgen we een bijgewerkte Azure RHUI geïmplementeerd. In April 2017 we de oude Azure RHUI afgesloten. Als u eerder hebt gebruikt de RHEL PAYG afbeeldingen (of hun momentopnamen) vanaf September 2016 of hoger, bent u automatisch verbinding maken met de nieuwe Azure-RHUI. Als u echter oudere momentopnamen op uw virtuele machines hebt, moet u de configuratie voor toegang tot de Azure-RHUI zoals beschreven in een volgende sectie handmatig bijwerken.

De nieuwe Azure RHUI-servers zijn geïmplementeerd met [Azure Traffic Manager](https://azure.microsoft.com/services/traffic-manager/). Traffic Manager één eindpunt (rhui-1.microsoft.com) kan worden gebruikt door een virtuele machine, ongeacht de regio. 

### <a name="troubleshoot-connection-problems-to-azure-rhui"></a>Problemen met verbinding voor Azure RHUI
Als u problemen hebt met het verbinding maken met Azure RHUI van uw virtuele machine in Azure RHEL Omslagstelsel, volg deze stappen:

1. Inspecteer de configuratie van de virtuele machine voor het eindpunt Azure RHUI:

    a. Controleer of de `/etc/yum.repos.d/rh-cloud.repo` bestand bevat een verwijzing naar `rhui-[1-3].microsoft.com` in de `baseurl` van de `[rhui-microsoft-azure-rhel*]` gedeelte van het bestand. Als dit het geval is, gebruikt u de nieuwe Azure-RHUI.

    b. Als deze naar een locatie met het volgende patroon verwijst `mirrorlist.*cds[1-4].cloudapp.net`, een configuratie-update is vereist. U de oude VM-momentopname en u moet deze verwijzen naar de nieuwe Azure-RHUI bij te werken.

2. Toegang tot Azure gehoste RHUI is beperkt tot virtuele machines binnen de [Azure datacenter IP-adresbereiken] (https://www.microsoft.com/download/details.aspx?id=41653).
 
3. Als u de nieuwe configuratie, hebt gecontroleerd dat de virtuele machine verbinding van de Azure-IP-adresbereik maakt en nog steeds geen verbinding met Azure RHUI, bestand een ondersteuningsaanvraag met Microsoft of Red Hat maken.

### <a name="manual-update-procedure-to-use-the-azure-rhui-servers"></a>Handmatige updateprocedure voor het gebruik van de Azure-RHUI-servers
Deze procedure is alleen ter informatie geboden. Er is al de juiste configuratie verbinding maken met Azure RHUI RHEL PAYG installatiekopieën. Voor het handmatig bijwerken van de configuratie voor het gebruik van de Azure-RHUI-servers, kunt u de volgende stappen uitvoeren:

1. De openbare sleutel handtekening via curl downloaden.

   ```bash
   curl -o RPM-GPG-KEY-microsoft-azure-release https://download.microsoft.com/download/9/D/9/9d945f05-541d-494f-9977-289b3ce8e774/microsoft-sign-public.asc 
   ```

2. Verifieer de geldigheid van de gedownloade sleutel.

   ```bash
   gpg --list-packets --verbose < RPM-GPG-KEY-microsoft-azure-release
   ```

3. Controleer de uitvoer en controleer vervolgens of de `keyid` en de `user ID packet`.

   ```bash
   Version: GnuPG v1.4.7 (GNU/Linux)
   :public key packet:
           version 4, algo 1, created 1446074508, expires 0
           pkey[0]: [2048 bits]
           pkey[1]: [17 bits]
           keyid: EB3E94ADBE1229CF
   :user ID packet: "Microsoft (Release signing) <gpgsecurity@microsoft.com>"
   :signature packet: algo 1, keyid EB3E94ADBE1229CF
           version 4, created 1446074508, md5len 0, sigclass 0x13
           digest algo 2, begin of digest 1a 9b
           hashed subpkt 2 len 4 (sig created 2015-10-28)
           hashed subpkt 27 len 1 (key flags: 03)
           hashed subpkt 11 len 5 (pref-sym-algos: 9 8 7 3 2)
           hashed subpkt 21 len 3 (pref-hash-algos: 2 8 3)
           hashed subpkt 22 len 2 (pref-zip-algos: 2 1)
           hashed subpkt 30 len 1 (features: 01)
           hashed subpkt 23 len 1 (key server preferences: 80)
           subpkt 16 len 8 (issuer key ID EB3E94ADBE1229CF)
           data: [2047 bits]
   ```

4. Installeer de openbare sleutel.

   ```bash
   sudo install -o root -g root -m 644 RPM-GPG-KEY-microsoft-azure-release /etc/pki/rpm-gpg
   sudo rpm --import /etc/pki/rpm-gpg/RPM-GPG-KEY-microsoft-azure-release
   ```

5. Downloaden, controleren en installeren van een client Manager RPM-pakket (RPM).
    
    >[!NOTE]
    >Pakketversies wijzigen. Als u handmatig op Azure RHUI aansluit, kunt u de nieuwste versie van het clientpakket voor iedere familie RHEL vinden door de inrichting van de installatiekopie van het meest recente uit de galerie.
  
   a. Downloaden. 
   
    - Voor RHEL 6:
        ```bash
        curl -o azureclient.rpm https://rhui-1.microsoft.com/pulp/repos/microsoft-azure-rhel6/rhui-azure-rhel6-2.1-32.noarch.rpm 
        ```
    
    - Voor RHEL 7:
        ```bash
        curl -o azureclient.rpm https://rhui-1.microsoft.com/pulp/repos/microsoft-azure-rhel7/rhui-azure-rhel7-2.1-19.noarch.rpm  
        ```

   b. Controleer of.

   ```bash
   rpm -Kv azureclient.rpm
   ```

   c. Controleer de uitvoer om ervoor te zorgen dat de handtekening van het pakket is in orde.

   ```bash
   azureclient.rpm:
       Header V3 RSA/SHA256 Signature, key ID be1229cf: OK
       Header SHA1 digest: OK (927a3b548146c95a3f6c1a5d5ae52258a8859ab3)
       V3 RSA/SHA256 Signature, key ID be1229cf: OK
       MD5 digest: OK (c04ff605f82f4be8c96020bf5c23b86c)
   ```

   d. De RPM installeren.

    ```bash
    sudo rpm -U azureclient.rpm
    ```

6. Nadat u klaar bent, moet u controleren of u toegang Azure RHUI van de virtuele machine tot.

## <a name="next-steps"></a>Volgende stappen
Een Red Hat Enterprise Linux VM maken van een installatiekopie van een Azure Marketplace Omslagstelsel en Azure gehoste RHUI gebruiken, gaat u naar de [Azure Marketplace](https://azure.microsoft.com/marketplace/partners/redhat/). 
