---
title: 'Azure Active Directory Domain Services: Een virtuele CoreOS Linux-machine toevoegen aan een beheerd domein | Microsoft Docs'
description: Een virtuele CoreOS Linux-machine toevoegen aan Azure AD Domain Services
services: active-directory-ds
documentationcenter: 
author: mahesh-unnikrishnan
manager: mtillman
editor: curtand
ms.assetid: 5db65f30-bf69-4ea3-9ea5-add1db83fdb8
ms.service: active-directory-ds
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/16/2017
ms.author: maheshu
ms.openlocfilehash: 790ad85df0dbf68674e2b9c6254858100ddfd0fd
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/11/2017
---
# <a name="join-a-coreos-linux-virtual-machine-to-a-managed-domain"></a>Een virtuele CoreOS Linux-machine toevoegen aan een beheerd domein
In dit artikel leest u hoe een virtuele CoreOS Linux-machine in Azure koppelen aan een beheerd domein van Azure AD Domain Services.

## <a name="before-you-begin"></a>Voordat u begint
Als u wilt uitvoeren van de taken worden in dit artikel worden vermeld, hebt u het volgende nodig:
1. Een geldige **Azure-abonnement**.
2. Een **Azure AD-directory** -ofwel gesynchroniseerd met een on-premises adreslijst of een map alleen in de cloud.
3. **Azure AD Domain Services** moet zijn ingeschakeld voor de Azure AD-directory. Als u dit nog niet hebt gedaan, volgt u alle taken die worden beschreven in de [handleiding](active-directory-ds-getting-started.md).
4. Zorg ervoor dat u de IP-adressen van het beheerde domein als de DNS-servers voor het virtuele netwerk hebt geconfigureerd. Zie voor meer informatie [het bijwerken van DNS-instellingen voor de virtuele Azure-netwerk](active-directory-ds-getting-started-dns.md)
5. Voltooi de stappen die zijn vereist voor het [wachtwoorden aan uw Azure AD Domain Services beheerd domein synchroniseren](active-directory-ds-getting-started-password-sync.md).


## <a name="provision-a-coreos-linux-virtual-machine"></a>Een virtuele CoreOS Linux-machine inrichten
Een virtuele CoreOS-machine inrichten in Azure met behulp van een van de volgende methoden:
* [Azure Portal](../virtual-machines/linux/quick-create-portal.md)
* [Azure CLI](../virtual-machines/linux/quick-create-cli.md)
* [Azure PowerShell](../virtual-machines/linux/quick-create-powershell.md)

Dit artikel wordt de **CoreOS Linux (stabiel)** installatiekopie van de virtuele machine in Azure.

> [!IMPORTANT]
> * Implementeer de virtuele machine in de **hetzelfde virtuele netwerk waarin u Azure AD Domain Services hebt ingeschakeld**.
> * Kies een **ander subnet** dan de waarin u Azure AD Domain Services hebt ingeschakeld.
>


## <a name="connect-remotely-to-the-newly-provisioned-linux-virtual-machine"></a>Extern verbinding maken met de nieuw ingerichte virtuele Linux-machine
De virtuele CoreOS virtuele machine is ingericht in Azure. De volgende taak is extern verbinding maken met de virtuele machine met het lokale administrator-account gemaakt tijdens het inrichten van de virtuele machine.

Volg de instructies in het artikel [aanmelden met een virtuele machine met Linux](../virtual-machines/linux/mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).


## <a name="configure-the-hosts-file-on-the-linux-virtual-machine"></a>Het hostbestand configureren op de virtuele Linux-machine
Bewerk het bestand/etc/hosts in uw terminal SSH en bijwerken van uw machine IP-adres en hostnaam.

```
sudo vi /etc/hosts
```

Voer de volgende waarde in het hosts-bestand:

```
127.0.0.1 contoso-coreos.contoso100.com contoso-coreos
```
Hier is, contoso100.com' de DNS-domeinnaam van uw beheerde domein. 'contoso-virtuele coreos' is de hostnaam van de virtuele CoreOS virtuele machine die u aan het beheerde domein deelneemt.


## <a name="configure-the-sssd-service-on-the-linux-virtual-machine"></a>De service SSSD configureren op de virtuele Linux-machine
Werk vervolgens uw configuratiebestand SSSD in ('/ etc/sssd/sssd.conf') overeenkomen met het volgende voorbeeld:

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
Nu de vereiste pakketten zijn geïnstalleerd op de virtuele Linux-machine, de volgende taak is de virtuele machine toevoegen aan het beheerde domein.

```
sudo adcli join -D CONTOSO100.COM -U bob@CONTOSO100.COM -K /etc/krb5.keytab -H contoso-coreos.contoso100.com -N coreos
```


> [!NOTE]
> **Voor probleemoplossing:** als *adcli* is niet gevonden uw beheerde domein:
  * Zorg ervoor dat het domein bereikbaar is vanaf de virtuele machine (probeer ping).
  * Controleer of de virtuele machine inderdaad is geïmplementeerd voor hetzelfde virtuele netwerk waarin het beheerde domein beschikbaar is.
  * Controleer als u de DNS-serverinstellingen voor het virtuele netwerk om te verwijzen naar de domeincontrollers van het beheerde domein hebt bijgewerkt.
>

Start de service SSSD. Typ de volgende opdracht in uw terminal SSH:
  ```
  sudo systemctl start sssd.service
  ```


## <a name="verify-domain-join"></a>Controleer of aan domein toevoegen
Controleer of de machine is toegevoegd aan het beheerde domein. Verbinding maken met het domein CoreOS VM die gebruikmaakt van een andere SSH-verbinding. Gebruik een domeingebruikersaccount en controleer of het gebruikersaccount correct is opgelost.

1. Typ de volgende opdracht verbinding maken met het domein lid in uw terminal SSH virtuele CoreOS virtuele machine via SSH. Gebruik een domeinaccount die deel uitmaakt van het beheerde domein (bijvoorbeeld 'bob@CONTOSO100.COM' in dit geval.)
    ```
    ssh -l bob@CONTOSO100.COM contoso-coreos.contoso100.com
    ```

2. Typ de volgende opdracht om te zien of de basismap correct is geïnitialiseerd in uw terminal SSH.
    ```
    pwd
    ```

3. Typ de volgende opdracht om te zien als het groepslidmaatschap correct wordt opgelost in uw terminal SSH.
    ```
    id
    ```


## <a name="troubleshooting-domain-join"></a>Het oplossen van problemen aan domein toevoegen
Raadpleeg de [probleemoplossing domein](active-directory-ds-admin-guide-join-windows-vm-portal.md#troubleshooting-domain-join) artikel.

## <a name="related-content"></a>Gerelateerde inhoud
* [Azure AD Domain Services - handleiding aan de slag](active-directory-ds-getting-started.md)
* [Virtuele machine met Windows Server toevoegen aan een beheerd domein van Azure AD Domain Services](active-directory-ds-admin-guide-join-windows-vm.md)
* [Aanmelden met een virtuele machine met Linux](../virtual-machines/linux/mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).
