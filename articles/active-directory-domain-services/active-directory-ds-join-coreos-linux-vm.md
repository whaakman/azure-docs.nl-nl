---
title: 'Azure Active Directory Domain Services: Een virtuele CoreOS-Linux-machine toevoegen aan een beheerd domein | Microsoft Docs'
description: Een virtuele CoreOS-Linux-machine toevoegen aan Azure AD Domain Services
services: active-directory-ds
documentationcenter: ''
author: eringreenlee
manager: daveba
editor: curtand
ms.assetid: 5db65f30-bf69-4ea3-9ea5-add1db83fdb8
ms.service: active-directory
ms.subservice: domain-services
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 06/22/2018
ms.author: ergreenl
ms.openlocfilehash: 3948b0e1445aef5b9030e5e40f4bd4ec7ea1bf51
ms.sourcegitcommit: d3200828266321847643f06c65a0698c4d6234da
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/29/2019
ms.locfileid: "55175754"
---
# <a name="join-a-coreos-linux-virtual-machine-to-a-managed-domain"></a>Een virtuele CoreOS-Linux-machine toevoegen aan een beheerd domein
In dit artikel wordt beschreven hoe u een virtuele CoreOS-Linux-machine in Azure toevoegen aan een beheerd domein van Azure AD Domain Services.

[!INCLUDE [active-directory-ds-prerequisites.md](../../includes/active-directory-ds-prerequisites.md)]

## <a name="before-you-begin"></a>Voordat u begint
Als u de taken die in dit artikel worden vermeld, hebt u het volgende nodig:
1. Een geldige **Azure-abonnement**.
2. Een **Azure AD-directory** -een gesynchroniseerd met een on-premises directory of een map alleen in de cloud.
3. **Azure AD Domain Services** moet zijn ingeschakeld voor de Azure AD-directory. Als u dit nog niet hebt gedaan, volgt u alle taken die worden beschreven in de [introductiehandleiding](active-directory-ds-getting-started.md).
4. Zorg ervoor dat u de IP-adressen van het beheerde domein als de DNS-servers voor het virtuele netwerk hebt geconfigureerd. Zie voor meer informatie, [DNS-instellingen voor de Azure-netwerk bijwerken](active-directory-ds-getting-started-dns.md)
5. De stappen die nodig zijn om te voltooien [Synchroniseer de wachtwoorden voor uw Azure AD Domain Services beheerde domein](active-directory-ds-getting-started-password-sync.md).


## <a name="provision-a-coreos-linux-virtual-machine"></a>Een virtuele CoreOS-Linux-machine inrichten
Een virtuele CoreOS-machine inrichten in Azure met behulp van een van de volgende methoden:
* [Azure Portal](../virtual-machines/linux/quick-create-portal.md)
* [Azure-CLI](../virtual-machines/linux/quick-create-cli.md)
* [Azure PowerShell](../virtual-machines/linux/quick-create-powershell.md)

In dit artikel wordt de **CoreOS Linux (stabiel)** installatiekopie voor virtuele machines in Azure.

> [!IMPORTANT]
> * Implementeer de virtuele machine in de **hetzelfde virtuele netwerk waarin u Azure AD Domain Services hebt ingeschakeld**.
> * Kies een **ander subnet** dan de versie waarin u Azure AD Domain Services hebt ingeschakeld.
>


## <a name="connect-remotely-to-the-newly-provisioned-linux-virtual-machine"></a>Extern verbinding maken met de nieuw ingerichte virtuele Linux-machine
De CoreOS-VM is ingericht in Azure. De volgende taak bestaat uit het op afstand verbinding maken met de virtuele machine met behulp van de lokale administrator-account dat is gemaakt tijdens het inrichten van de virtuele machine.

Volg de instructies in het artikel [Meld u aan een virtuele machine waarop Linux wordt uitgevoerd bij het](../virtual-machines/linux/mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).


## <a name="configure-the-hosts-file-on-the-linux-virtual-machine"></a>Het hosts-bestand op de Linux-machine configureren
Bewerk het bestand/etc/hosts in de terminal SSH en bijwerken van uw IP-adres en hostnaam.

```
sudo vi /etc/hosts
```

Voer de volgende waarde in het hosts-bestand:

```
127.0.0.1 contoso-coreos.contoso100.com contoso-coreos
```
Hier is 'contoso100.com' de DNS-domeinnaam van uw beheerde domein. 'contoso-coreos' is de hostnaam van de CoreOS-VM die u aan het beheerde domein deelneemt.


## <a name="configure-the-sssd-service-on-the-linux-virtual-machine"></a>De service SSSD configureren op de virtuele Linux-machine
Werk vervolgens uw configuratiebestand SSSD in ('/ etc/sssd/sssd.conf') zodat deze overeenkomt met het volgende voorbeeld:

 ```
 [sssd]
 config_file_version = 2
 services = nss, pam
 domains = CONTOSO100.COM

 [domain/CONTOSO100.COM]
 id_provider = ad
 auth_provider = ad
 chpass_provider = ad

 ldap_uri = ldap://contoso100.com
 ldap_search_base = dc=contoso100,dc=com
 ldap_schema = rfc2307bis
 ldap_sasl_mech = GSSAPI
 ldap_user_object_class = user
 ldap_group_object_class = group
 ldap_user_home_directory = unixHomeDirectory
 ldap_user_principal = userPrincipalName
 ldap_account_expire_policy = ad
 ldap_force_upper_case_realm = true
 fallback_homedir = /home/%d/%u

 krb5_server = contoso100.com
 krb5_realm = CONTOSO100.COM
 ```
Vervang ' CONTOSO100. COM' met de DNS-domeinnaam van uw beheerde domein. Zorg ervoor dat u de domeinnaam in kapitaal geval in het bestand conf opgeven.


## <a name="join-the-linux-virtual-machine-to-the-managed-domain"></a>De virtuele Linux-machine toevoegen aan het beheerde domein
Nu dat de vereiste pakketten zijn geïnstalleerd op de Linux-machine, bestaat de volgende taak uit de virtuele machine toevoegen aan het beheerde domein.

```
sudo adcli join -D CONTOSO100.COM -U bob@CONTOSO100.COM -K /etc/krb5.keytab -H contoso-coreos.contoso100.com -N coreos
```


> [!NOTE]
> **Problemen oplossen:** Als *adcli* is niet gevonden van uw beheerde domein:
  * Zorg ervoor dat het domein bereikbaar is vanaf de virtuele machine (probeer ping).
  * Controleer dat de virtuele machine inderdaad is geïmplementeerd voor hetzelfde virtuele netwerk waarin het beheerde domein beschikbaar is.
  * Controleert u of u de DNS-serverinstellingen voor het virtuele netwerk om te verwijzen naar de domeincontrollers van het beheerde domein hebt bijgewerkt.
>

Start de service SSSD. Typ de volgende opdracht in uw terminal SSH:
  ```
  sudo systemctl start sssd.service
  ```


## <a name="verify-domain-join"></a>Controleer of aan domein toevoegen
Controleer of de computer is toegevoegd aan het beheerde domein. Verbinding maken met het domein opgenomen CoreOS-VM met behulp van een andere SSH-verbinding. Gebruik een domeingebruikersaccount en Ga na of het gebruikersaccount juist is opgelost.

1. Typ de volgende opdracht verbinding maken met het domein lid zijn van CoreOS-VM met behulp van SSH in uw terminal SSH. Gebruik een domeinaccount dat deel uitmaakt van het beheerde domein (bijvoorbeeld 'bob@CONTOSO100.COM' in dit geval.)
    ```
    ssh -l bob@CONTOSO100.COM contoso-coreos.contoso100.com
    ```

2. Typ in de SSH-terminal de volgende opdracht uit om te zien als de oorspronkelijke directory correct is geïnitialiseerd.
    ```
    pwd
    ```

3. Typ de volgende opdracht uit om te zien als het lidmaatschap van groepen goed worden herleid in de SSH-terminal.
    ```
    id
    ```


## <a name="troubleshooting-domain-join"></a>Het oplossen van aan domein toevoegen
Raadpleeg de [probleemoplossing domeindeelname](active-directory-ds-admin-guide-join-windows-vm-portal.md#troubleshoot-joining-a-domain) artikel.

## <a name="related-content"></a>Gerelateerde inhoud
* [Azure AD Domain Services - handleiding aan de slag](active-directory-ds-getting-started.md)
* [Een Windows Server-machine toevoegen aan Azure AD Domain Services beheerde domein](active-directory-ds-admin-guide-join-windows-vm.md)
* [Meld u aan een virtuele machine waarop Linux wordt uitgevoerd bij het](../virtual-machines/linux/mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).
