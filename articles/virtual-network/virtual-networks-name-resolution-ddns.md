---
title: Met behulp van dynamische DNS hostnamen registreren
description: Deze pagina geeft informatie over het instellen van dynamische DNS hostnamen in uw eigen DNS-servers registreren.
services: dns
documentationcenter: na
author: GarethBradshawMSFT
manager: timlt
editor: 
ms.assetid: c315961a-fa33-45cf-82b9-4551e70d32dd
ms.service: dns
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/23/2017
ms.author: garbrad
ms.openlocfilehash: 440a062e5fff73526b2d77d7d0a7c52ca72a66f1
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2017
---
# <a name="using-dynamic-dns-to-register-hostnames-in-your-own-dns-server"></a>Met behulp van dynamische DNS hostnamen in uw eigen DNS-server registreren
[Azure biedt naamomzetting](virtual-networks-name-resolution-for-vms-and-role-instances.md) voor virtuele machines (VM's) en rolinstanties. Wanneer uw naamomzetting moet verder dan die worden verstrekt door Azure, kunt u uw eigen DNS-servers opgeven. Hierdoor kunt u de bevoegdheid om aan te passen van uw DNS-oplossing aanpassen aan uw eigen specifieke behoeften. U wilt bijvoorbeeld toegang tot lokale bronnen via uw Active Directory-domeincontroller.

Wanneer uw aangepaste DNS-servers worden gehost als Azure virtuele machines kunt u de hostnaam van een query uitgevoerd naar hetzelfde vnet doorsturen naar Azure hostnamen omzetten. Als u niet deze route gebruikt wilt, kunt u uw VM hostnamen in uw DNS-server met behulp van dynamische DNS registreren.  Azure heeft de mogelijkheid (bijvoorbeeld referenties) geen rechtstreeks om records te maken in uw DNS-servers, zodat het alternatieve regelingen vaak nodig zijn. Hier volgen enkele algemene scenario's met alternatieven.

## <a name="windows-clients"></a>Windows-clients
Niet-domein Windows-clients proberen onbeveiligde dynamische DNS-(DDNS) updates wanneer ze worden opgestart of wanneer hun IP-adres verandert. De DNS-naam is de hostnaam plus het primaire DNS-achtervoegsel. Azure worden de primaire DNS-achtervoegsel leeg gelaten, maar u kunt dit instellen in de virtuele machine de [UI](https://technet.microsoft.com/library/cc794784.aspx) of [met behulp van automatisering](https://social.technet.microsoft.com/forums/windowsserver/3720415a-6a9a-4bca-aa2a-6df58a1a47d7/change-primary-dns-suffix).

Windows-clients domein voor het registreren van hun IP-adressen met de domeincontroller met behulp van beveiligde dynamische DNS. Het lid van domein-proces wordt het primaire DNS-achtervoegsel op de client ingesteld en maakt en onderhoudt de vertrouwensrelatie.

## <a name="linux-clients"></a>Linux-clients
Linux-clients in het algemeen niet registreren bij de DNS-server bij het opstarten, ze wordt ervan uitgegaan dat de DHCP-server dit doet. Azure DHCP-servers beschikt niet over de mogelijkheid of referenties records te registreren bij uw DNS-server.  U kunt een hulpmiddel *nsupdate*, die is opgenomen in het pakket Bind, om het verzenden van dynamische DNS-updates. U kunt gebruiken omdat het dynamische DNS-protocol is gestandaardiseerd, *nsupdate* zelfs wanneer u niet gebruikt afhankelijk van de DNS-server.

U kunt de hooks die worden geleverd door de DHCP-client maken en onderhouden van de hostnaam vermelding in de DNS-server gebruiken. Tijdens de cyclus DHCP, de client voert de scripts in */etc/dhcp/dhclient-exit-hooks.d/*. Dit kan worden gebruikt voor het registreren van het nieuwe IP-adres met behulp van *nsupdate*. Bijvoorbeeld:

        #!/bin/sh
        requireddomain=mydomain.local

        # only execute on the primary nic
        if [ "$interface" != "eth0" ]
        then
            return
        fi

        # when we have a new IP, perform nsupdate
        if [ "$reason" = BOUND ] || [ "$reason" = RENEW ] ||
           [ "$reason" = REBIND ] || [ "$reason" = REBOOT ]
        then
            host=`hostname`
            nsupdatecmds=/var/tmp/nsupdatecmds
              echo "update delete $host.$requireddomain a" > $nsupdatecmds
              echo "update add $host.$requireddomain 3600 a $new_ip_address" >> $nsupdatecmds
              echo "send" >> $nsupdatecmds

              nsupdate $nsupdatecmds
        fi

        
        

U kunt ook de *nsupdate* opdracht voor het uitvoeren van beveiligde dynamische DNS-updates. Bijvoorbeeld, wanneer u een Bind DNS-server, een openbaar / persoonlijk sleutelpaar is [gegenereerd](http://linux.yyz.us/nsupdate/).  De DNS-server [geconfigureerd](http://linux.yyz.us/dns/ddns-server.html) met het openbare deel van de sleutel, zodat deze de handtekening van de aanvraag kunt controleren. Moet u de *-k* optie in als u het sleutelpaar naar *nsupdate* bijwerken in volgorde voor de dynamische DNS-aanvraag moet worden ondertekend.

Als u een Windows-DNS-server gebruikt, kunt u Kerberos-verificatie met de *-g* parameter in *nsupdate* (niet beschikbaar in de Windows-versie van *nsupdate*). U doet dit door gebruik *kinit* laden de referenties (bijvoorbeeld van een [keytab-bestand](http://www.itadmintools.com/2011/07/creating-kerberos-keytab-files.html)). Vervolgens *nsupdate -g* gewoon door de referenties uit de cache.

Indien nodig, kunt u een DNS-zoekachtervoegsel toevoegen aan uw virtuele machines. Het DNS-achtervoegsel is opgegeven in de */etc/resolv.conf* bestand. De meeste Linux-distributies beheren automatisch de inhoud van dit bestand, dus meestal niet worden bewerkt. U kunt echter het achtervoegsel overschrijven met behulp van de DHCP-client *vervangen* opdracht. Hiervoor in */etc/dhcp/dhclient.conf*, toevoegen:

        supersede domain-name <required-dns-suffix>;

