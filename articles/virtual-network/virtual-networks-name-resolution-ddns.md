---
title: Dynamische DNS gebruiken om hostnamen te registreren in Azure | Microsoft Docs
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
ms.openlocfilehash: c2ef842fd62ef060f06536d66387c3facd0627b5
ms.sourcegitcommit: 5839af386c5a2ad46aaaeb90a13065ef94e61e74
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/19/2019
ms.locfileid: "57994692"
---
# <a name="use-dynamic-dns-to-register-hostnames-in-your-own-dns-server"></a>Dynamische DNS gebruiken om hostnamen te registreren in uw eigen DNS-server

[Azure biedt naamomzetting](virtual-networks-name-resolution-for-vms-and-role-instances.md) voor virtuele machines (VM) en rolexemplaren controleert. Wanneer uw naamomzetting moet groter zijn dan de mogelijkheden van Azure standaard DNS-, kunt u uw eigen DNS-servers opgeven. Met behulp van uw eigen DNS-servers biedt u de mogelijkheid om aan te passen van uw DNS-oplossing voor uw eigen specifieke behoeften. Bijvoorbeeld, moet u mogelijk toegang tot on-premises bronnen via uw Active Directory-domeincontroller.

Wanneer uw aangepaste DNS-servers worden gehost als Azure virtuele machines, kunt u de hostnaam van query's voor hetzelfde virtuele netwerk naar Azure hostnamen omzetten doorsturen. Als u niet gebruiken met deze optie wilt, kunt u uw VM-hostnamen kunt registreren in de DNS-server met behulp van dynamische DNS (DDNS). Azure beschikt niet over de referenties rechtstreeks om records te maken in uw DNS-servers, zodat alternatieve regelingen vaak nodig zijn. Enkele algemene scenario's, met alternatieven als volgt:

## <a name="windows-clients"></a>Windows-clients
Niet-domein Windows-clients proberen niet-beveiligde DDNS updates wanneer ze opstarten of wanneer hun IP-adres verandert. De DNS-naam is de hostnaam en het primaire DNS-achtervoegsel. Azure blijft het primaire DNS-achtervoegsel leeg, maar u kunt het achtervoegsel in de virtuele machine instellen de [gebruikersinterface](https://technet.microsoft.com/library/cc794784.aspx) of [PowerShell](/powershell/module/dnsclient/set-dnsclient).

Windows-clients domein voor het registreren van hun IP-adressen met de domeincontroller met behulp van veilige DDNS. Het proces van het domein ingesteld van het primaire DNS-achtervoegsel op de client en maakt en onderhoudt vertrouwensrelatie tot stand brengen.

## <a name="linux-clients"></a>Linux-clients
Linux-clients in het algemeen niet registreren bij de DNS-server bij het opstarten, ze wordt ervan uitgegaan dat de DHCP-server doet het allemaal. DHCP-servers van Azure hoeft niet de referenties om records te registreren in uw DNS-server. U kunt een hulpprogramma met de naam `nsupdate`, die is opgenomen in het pakket Bind, voor het verzenden van DDNS bijgewerkt. Omdat het DDNS-protocol is gestandaardiseerd, kunt u `nsupdate` , zelfs wanneer niet u afhankelijk van de DNS-server.

U kunt de hooks die worden geleverd door de DHCP-client maken en onderhouden van de hostnaam-vermelding in de DNS-server gebruiken. Tijdens de cyclus van een DHCP-, de client voert de scripts in */etc/dhcp/dhclient-exit-hooks.d/*. U kunt de hooks gebruiken voor het registreren van het nieuwe IP-adres met `nsupdate`. Bijvoorbeeld:

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

U kunt ook de `nsupdate` opdracht voor het uitvoeren van beveiligde DDNS bijgewerkt. Bijvoorbeeld, als u een binding DNS-server, een openbaar / persoonlijk sleutelpaar is [gegenereerd](http://linux.yyz.us/nsupdate/). De DNS-server is [geconfigureerd](http://linux.yyz.us/dns/ddns-server.html) met het openbare deel van de sleutel, dus deze kunt controleren of de handtekening van de aanvraag. Voor het sleutelpaar op `nsupdate`, gebruikt u de `-k` optie, voor de DDNS bijwerken aanvraag zijn ondertekend.

Wanneer u een Windows-DNS-server gebruikt, kunt u Kerberos-verificatie met de `-g` parameter in `nsupdate`, maar het is niet beschikbaar in de Windows-versie van `nsupdate`. Gebruiken voor het gebruik van Kerberos `kinit` laden van de referenties. For example, kunt u referenties van laden een [keytab-bestand](https://www.itadmintools.com/2011/07/creating-kerberos-keytab-files.html)), klikt u vervolgens `nsupdate -g` neemt de referenties uit de cache.

Indien nodig, kunt u een DNS-zoekachtervoegsel toevoegen aan uw virtuele machines. Het DNS-achtervoegsel is opgegeven in de */etc/resolv.conf* bestand. De meeste Linux-distributies automatisch beheren van de inhoud van dit bestand, dus meestal niet worden bewerkt. U kunt echter het achtervoegsel overschrijven met behulp van de DHCP-client `supersede` opdracht. Als u wilt overschrijven het achtervoegsel, voeg de volgende regel aan de */etc/dhcp/dhclient.conf* bestand:

```
supersede domain-name <required-dns-suffix>;
```
