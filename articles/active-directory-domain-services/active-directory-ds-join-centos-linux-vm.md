---
title: 'Azure Active Directory Domain Services: Een CentOS virtuele machine toevoegen aan een beheerd domein | Microsoft Docs'
description: CentOS Linux virtuele machine toevoegen aan Azure AD Domain Services
services: active-directory-ds
documentationcenter: 
author: mahesh-unnikrishnan
manager: mtillman
editor: curtand
ms.assetid: 16100caa-f209-4cb0-86d3-9e218aeb51c6
ms.service: active-directory-ds
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/16/2017
ms.author: maheshu
ms.openlocfilehash: 65a9e4267c8883db5c8d8bfc5e0167577cd969d3
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/11/2017
---
# <a name="join-a-centos-linux-virtual-machine-to-a-managed-domain"></a>CentOS Linux virtuele machine toevoegen aan een beheerd domein
In dit artikel leest u hoe een CentOS Linux virtuele machine in Azure koppelen aan een beheerd domein van Azure AD Domain Services.

## <a name="before-you-begin"></a>Voordat u begint
Als u wilt uitvoeren van de taken worden in dit artikel worden vermeld, hebt u het volgende nodig:
1. Een geldige **Azure-abonnement**.
2. Een **Azure AD-directory** -ofwel gesynchroniseerd met een on-premises adreslijst of een map alleen in de cloud.
3. **Azure AD Domain Services** moet zijn ingeschakeld voor de Azure AD-directory. Als u dit nog niet hebt gedaan, volgt u alle taken die worden beschreven in de [handleiding](active-directory-ds-getting-started.md).
4. Zorg ervoor dat u de IP-adressen van het beheerde domein als de DNS-servers voor het virtuele netwerk hebt geconfigureerd. Zie voor meer informatie [het bijwerken van DNS-instellingen voor de virtuele Azure-netwerk](active-directory-ds-getting-started-dns.md)
5. Voltooi de stappen die zijn vereist voor het [wachtwoorden aan uw Azure AD Domain Services beheerd domein synchroniseren](active-directory-ds-getting-started-password-sync.md).


## <a name="provision-a-centos-linux-virtual-machine"></a>Een CentOS Linux-machine inrichten
Een CentOS virtuele machine inrichten in Azure met behulp van een van de volgende methoden:
* [Azure Portal](../virtual-machines/linux/quick-create-portal.md)
* [Azure CLI](../virtual-machines/linux/quick-create-cli.md)
* [Azure PowerShell](../virtual-machines/linux/quick-create-powershell.md)

> [!IMPORTANT]
> * Implementeer de virtuele machine in de **hetzelfde virtuele netwerk waarin u Azure AD Domain Services hebt ingeschakeld**.
> * Kies een **ander subnet** dan de waarin u Azure AD Domain Services hebt ingeschakeld.
>


## <a name="connect-remotely-to-the-newly-provisioned-linux-virtual-machine"></a>Extern verbinding maken met de nieuw ingerichte virtuele Linux-machine
De CentOS virtuele machine is ingericht in Azure. De volgende taak is extern verbinding maken met de virtuele machine met het lokale administrator-account gemaakt tijdens het inrichten van de virtuele machine.

Volg de instructies in het artikel [aanmelden met een virtuele machine met Linux](../virtual-machines/linux/mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).


## <a name="configure-the-hosts-file-on-the-linux-virtual-machine"></a>Het hostbestand configureren op de virtuele Linux-machine
Bewerk het bestand/etc/hosts in uw terminal SSH en bijwerken van uw machine IP-adres en hostnaam.

```
sudo vi /etc/hosts
```

Voer de volgende waarde in het hosts-bestand:

```
127.0.0.1 contoso-centos.contoso100.com contoso-centos
```
Hier is, contoso100.com' de DNS-domeinnaam van uw beheerde domein. 'contoso-centos' is de hostnaam van de CentOS virtuele machine die u aan het beheerde domein deelneemt.


## <a name="install-required-packages-on-the-linux-virtual-machine"></a>Installeert de vereiste pakketten op de virtuele Linux-machine
Pakketten die zijn vereist voor domeinlidmaatschap op de virtuele machine vervolgens installeren. Typ de volgende opdracht om de vereiste pakketten te installeren in uw terminal SSH:

    ```
    sudo yum install realmd sssd krb5-workstation krb5-libs oddjob oddjob-mkhomedir samba-common-tools
    ```


## <a name="join-the-linux-virtual-machine-to-the-managed-domain"></a>De virtuele Linux-machine toevoegen aan het beheerde domein
Nu de vereiste pakketten zijn geïnstalleerd op de virtuele Linux-machine, de volgende taak is de virtuele machine toevoegen aan het beheerde domein.

1. Ontdek het beheerde domein met AAD Domain Services. Typ de volgende opdracht in uw terminal SSH:

    ```
    sudo realm discover CONTOSO100.COM
    ```

      > [!NOTE]
      > **Voor probleemoplossing:** als *realm detecteren* is niet gevonden uw beheerde domein:
        * Ensure that the domain is reachable from the virtual machine (try ping).
        * Check that the virtual machine has indeed been deployed to the same virtual network in which the managed domain is available.
        * Check to see if you have updated the DNS server settings for the virtual network to point to the domain controllers of the managed domain.
      >

2. Initialiseren van Kerberos. Typ de volgende opdracht in uw terminal SSH:

    > [!TIP]
    > * Een gebruiker die lid is van de groep 'AAD DC Administrators' opgeven.
    > * Geef de domeinnaam in hoofdletters, anders kinit mislukt.
    >

    ```
    kinit bob@CONTOSO100.COM
    ```

3. De machine toevoegen aan het domein. Typ de volgende opdracht in uw terminal SSH:

    > [!TIP]
    > De dezelfde gebruikersaccount die u hebt opgegeven in de vorige stap (kinit) gebruiken.
    >

    ```
    sudo realm join --verbose CONTOSO100.COM -U 'bob@CONTOSO100.COM'
    ```

U krijgt een bericht ('is ingeschreven machine in de realm') wanneer de computer met succes is toegevoegd aan het beheerde domein.


## <a name="verify-domain-join"></a>Controleer of aan domein toevoegen
Controleer of de machine is toegevoegd aan het beheerde domein. Verbinding maken met het domein zijn toegevoegd CentOS VM die gebruikmaakt van een andere SSH-verbinding. Gebruik een domeingebruikersaccount en controleer of het gebruikersaccount correct is opgelost.

1. Typ de volgende opdracht verbinding maken met het domein lid zijn van CentOS virtuele machine via SSH in uw terminal SSH. Gebruik een domeinaccount die deel uitmaakt van het beheerde domein (bijvoorbeeld 'bob@CONTOSO100.COM' in dit geval.)
    ```
    ssh -l bob@CONTOSO100.COM contoso-centos.contoso100.com
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
* [Kerberos installeren](https://access.redhat.com/documentation/en-US/Red_Hat_Enterprise_Linux/6/html/Managing_Smart_Cards/installing-kerberos.html)
* [Red Hat Enterprise Linux 7 - handleiding voor Windows-integratie](https://access.redhat.com/documentation/en-US/Red_Hat_Enterprise_Linux/7/html/Windows_Integration_Guide/index.html)
