---
title: Met behulp van dynamische DNS hostnamen in Azure registreren | Microsoft Docs
description: Informatie over het instellen van dynamische DNS voor het registreren van hostnamen in uw eigen DNS-servers.
services: dns
documentationcenter: na
author: subsarma
manager: vitinnan
editor: ''
ms.assetid: c315961a-fa33-45cf-82b9-4551e70d32dd
ms.service: dns
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/23/2017
ms.author: subsarma
ms.openlocfilehash: bbbce45b7c321fd4934374c76f2a4421b125d46f
ms.sourcegitcommit: 59914a06e1f337399e4db3c6f3bc15c573079832
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/19/2018
---
# <a name="use-dynamic-dns-to-register-hostnames-in-your-own-dns-server"></a>Gebruik van dynamische DNS hostnamen in uw eigen DNS-server registreren

[Azure biedt naamomzetting](virtual-networks-name-resolution-for-vms-and-role-instances.md) voor virtuele machines (VM) en rolinstanties. Als uw naamomzetting groter zijn dan de opgegeven Azure standaard DNS-mogelijkheden moet, kunt u uw eigen DNS-servers opgeven. Met behulp van uw eigen DNS-servers biedt u de mogelijkheid om aan te passen van uw DNS-oplossing aanpassen aan uw eigen specifieke behoeften. U wilt bijvoorbeeld toegang tot lokale bronnen via uw Active Directory-domeincontroller.

Wanneer uw aangepaste DNS-servers als Azure virtuele machines worden gehost, kunt u de hostnaam van een query uitgevoerd naar hetzelfde virtuele netwerk doorsturen naar Azure hostnamen omzetten. Als u niet wilt u deze optie gebruikt, kunt u uw VM hostnamen registreren in uw DNS-server met behulp van dynamische DNS-(DDNS). Azure beschikt niet over de referenties rechtstreeks om records te maken in uw DNS-servers, zodat het alternatieve regelingen vaak nodig zijn. Enkele algemene scenario's, met alternatieven als volgt:

## <a name="windows-clients"></a>Windows-clients
Niet-domein Windows-clients proberen onbeveiligde DDNS updates wanneer ze worden opgestart of wanneer hun IP-adres verandert. De DNS-naam is de hostnaam plus het primaire DNS-achtervoegsel. Azure worden de primaire DNS-achtervoegsel leeg gelaten, maar u kunt het achtervoegsel in de virtuele machine instellen de [gebruikersinterface](https://technet.microsoft.com/library/cc794784.aspx) of [PowerShell](/powershell/module/dnsclient/set-dnsclient).

Domein van de Windows-clients voor het registreren van hun IP-adressen met de domeincontroller met behulp van veilige DDNS. Het lid van domein-proces wordt het primaire DNS-achtervoegsel op de client ingesteld en maakt en onderhoudt de vertrouwensrelatie.

## <a name="linux-clients"></a>Linux-clients
Linux-clients in het algemeen niet registreren bij de DNS-server bij het opstarten, ze wordt ervan uitgegaan dat de DHCP-server dit doet. Azure DHCP-servers beschikt niet over de referenties om records te registreren bij uw DNS-server. U kunt een hulpmiddel `nsupdate`, die is opgenomen in het pakket Bind, verzenden DDNS-updates. U kunt gebruiken omdat het protocol DDNS is gestandaardiseerd, `nsupdate` zelfs wanneer u niet gebruikt afhankelijk van de DNS-server.

U kunt de hooks die worden geleverd door de DHCP-client maken en onderhouden van de hostnaam vermelding in de DNS-server gebruiken. Tijdens de cyclus DHCP, de client voert de scripts in */etc/dhcp/dhclient-exit-hooks.d/*. U kunt de hooks registreren met het nieuwe IP-adres met `nsupdate`. Bijvoorbeeld:

```bash
#!/bin/sh
requireddomain=mydomain.local

# only execute on the primary nic
if [ "$interface" != "eth0" ]
then
    return
fi

# When you have a new IP, perform nsupdate
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
```

U kunt ook de `nsupdate` opdracht voor het uitvoeren van beveiligde DDNS bijgewerkt. Bijvoorbeeld, wanneer u een Bind DNS-server, een openbaar / persoonlijk sleutelpaar is [gegenereerd](http://linux.yyz.us/nsupdate/). De DNS-server [geconfigureerd](http://linux.yyz.us/dns/ddns-server.html) met het openbare deel van de sleutel, zodat deze kunt controleren of de handtekening van de aanvraag. Voor het sleutelpaar naar `nsupdate`, gebruiken de `-k` optie, de DDNS werk verzoek om te worden ondertekend.

Als u een Windows-DNS-server gebruikt, kunt u Kerberos-verificatie met de `-g` parameter in `nsupdate`, maar is niet beschikbaar in de Windows-versie van `nsupdate`. Gebruik voor het gebruik van Kerberos `kinit` laden de referenties. For example, laden van referenties van een [keytab-bestand](http://www.itadmintools.com/2011/07/creating-kerberos-keytab-files.html)), vervolgens `nsupdate -g` neemt over de referenties uit de cache.

Indien nodig, kunt u een DNS-zoekachtervoegsel toevoegen aan uw virtuele machines. Het DNS-achtervoegsel is opgegeven in de */etc/resolv.conf* bestand. De meeste Linux-distributies beheren automatisch de inhoud van dit bestand, dus meestal niet worden bewerkt. U kunt echter het achtervoegsel overschrijven met behulp van de DHCP-client `supersede` opdracht. Om te overschrijven het achtervoegsel, voeg de volgende regel aan de */etc/dhcp/dhclient.conf* bestand:

```
supersede domain-name <required-dns-suffix>;
```
