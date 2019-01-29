---
title: 'Azure Active Directory Domain Services: Een CentOS-VM toevoegen aan een beheerd domein | Microsoft Docs'
description: Een CentOS Linux virtuele machine toevoegen aan Azure AD Domain Services
services: active-directory-ds
documentationcenter: ''
author: eringreenlee
manager: daveba
editor: curtand
ms.assetid: 16100caa-f209-4cb0-86d3-9e218aeb51c6
ms.service: active-directory
ms.subservice: domain-services
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 06/22/2018
ms.author: ergreenl
ms.openlocfilehash: 9ea236718b7fd9b808a5092dddec9574f6b93b48
ms.sourcegitcommit: d3200828266321847643f06c65a0698c4d6234da
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/29/2019
ms.locfileid: "55167152"
---
# <a name="join-a-centos-linux-virtual-machine-to-a-managed-domain"></a>Een virtuele CentOS Linux-machine toevoegen aan een beheerd domein
In dit artikel wordt beschreven hoe u een virtuele CentOS Linux-machine in Azure toevoegen aan een beheerd domein van Azure AD Domain Services.

[!INCLUDE [active-directory-ds-prerequisites.md](../../includes/active-directory-ds-prerequisites.md)]

## <a name="before-you-begin"></a>Voordat u begint
Als u de taken die in dit artikel worden vermeld, hebt u het volgende nodig:
1. Een geldige **Azure-abonnement**.
2. Een **Azure AD-directory** -een gesynchroniseerd met een on-premises directory of een map alleen in de cloud.
3. **Azure AD Domain Services** moet zijn ingeschakeld voor de Azure AD-directory. Als u dit nog niet hebt gedaan, volgt u alle taken die worden beschreven in de [introductiehandleiding](active-directory-ds-getting-started.md).
4. Zorg ervoor dat u de IP-adressen van het beheerde domein als de DNS-servers voor het virtuele netwerk hebt geconfigureerd. Zie voor meer informatie, [DNS-instellingen voor de Azure-netwerk bijwerken](active-directory-ds-getting-started-dns.md)
5. De stappen die nodig zijn om te voltooien [Synchroniseer de wachtwoorden voor uw Azure AD Domain Services beheerde domein](active-directory-ds-getting-started-password-sync.md).


## <a name="provision-a-centos-linux-virtual-machine"></a>Een CentOS Linux-machine inrichten
Een CentOS-machine inrichten in Azure met behulp van een van de volgende methoden:
* [Azure Portal](../virtual-machines/linux/quick-create-portal.md)
* [Azure-CLI](../virtual-machines/linux/quick-create-cli.md)
* [Azure PowerShell](../virtual-machines/linux/quick-create-powershell.md)

> [!IMPORTANT]
> * Implementeer de virtuele machine in de **hetzelfde virtuele netwerk waarin u Azure AD Domain Services hebt ingeschakeld**.
> * Kies een **ander subnet** dan de versie waarin u Azure AD Domain Services hebt ingeschakeld.
>


## <a name="connect-remotely-to-the-newly-provisioned-linux-virtual-machine"></a>Extern verbinding maken met de nieuw ingerichte virtuele Linux-machine
De CentOS virtuele machine is ingericht in Azure. De volgende taak bestaat uit het op afstand verbinding maken met de virtuele machine met behulp van de lokale administrator-account dat is gemaakt tijdens het inrichten van de virtuele machine.

Volg de instructies in het artikel [Meld u aan een virtuele machine waarop Linux wordt uitgevoerd bij het](../virtual-machines/linux/mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).


## <a name="configure-the-hosts-file-on-the-linux-virtual-machine"></a>Het hosts-bestand op de Linux-machine configureren
Bewerk het bestand/etc/hosts in de terminal SSH en bijwerken van uw IP-adres en hostnaam.

```
sudo vi /etc/hosts
```

Voer de volgende waarde in het hosts-bestand:

```
127.0.0.1 contoso-centos.contoso100.com contoso-centos
```
Hier is 'contoso100.com' de DNS-domeinnaam van uw beheerde domein. 'contoso-centos' is de hostnaam van de CentOS virtuele machine die u aan het beheerde domein deelneemt.


## <a name="install-required-packages-on-the-linux-virtual-machine"></a>Installeer de vereiste pakketten op de Linux-machine
Vervolgens installeert u pakketten zijn vereist voor domeindeelname op de virtuele machine. Typ de volgende opdracht uit om de vereiste pakketten te installeren in uw terminal SSH:

    ```
    sudo yum install realmd sssd krb5-workstation krb5-libs oddjob oddjob-mkhomedir samba-common-tools
    ```


## <a name="join-the-linux-virtual-machine-to-the-managed-domain"></a>De virtuele Linux-machine toevoegen aan het beheerde domein
Nu dat de vereiste pakketten zijn geïnstalleerd op de Linux-machine, bestaat de volgende taak uit de virtuele machine toevoegen aan het beheerde domein.

1. Ontdek het AAD Domain Services beheerde domein. Typ de volgende opdracht in uw terminal SSH:

    ```
    sudo realm discover CONTOSO100.COM
    ```

    > [!NOTE]
    > **Problemen oplossen:** Als *realm detecteren* is niet gevonden van uw beheerde domein:  
      * Zorg ervoor dat het domein bereikbaar is vanaf de virtuele machine (probeer ping).  
      * Controleer dat de virtuele machine inderdaad is geïmplementeerd voor hetzelfde virtuele netwerk waarin het beheerde domein beschikbaar is.
      * Controleert u of u de DNS-serverinstellingen voor het virtuele netwerk om te verwijzen naar de domeincontrollers van het beheerde domein hebt bijgewerkt.  
      >

2. Initialiseren van Kerberos. Typ de volgende opdracht in uw terminal SSH:

    > [!TIP]
    > * Geef een gebruiker die deel uitmaakt van de groep 'AAD DC Administrators'.
    > * De domeinnaam opgeven in hoofdletters, anders kinit mislukt.
    >

    ```
    kinit bob@CONTOSO100.COM
    ```

3. De machine toevoegen aan het domein. Typ de volgende opdracht in uw terminal SSH:

    > [!TIP]
    > Gebruik de dezelfde gebruikersaccount die u hebt opgegeven in de vorige stap (kinit).
    >

    ```
    sudo realm join --verbose CONTOSO100.COM -U 'bob@CONTOSO100.COM'
    ```

U krijgt een bericht ('ingeschreven machine in de realm') als de machine met succes is gekoppeld aan het beheerde domein.


## <a name="verify-domain-join"></a>Controleer of aan domein toevoegen
Controleer of de computer is toegevoegd aan het beheerde domein. Verbinding maken met het domein CentOS VM met behulp van een andere SSH-verbinding. Gebruik een domeingebruikersaccount en Ga na of het gebruikersaccount juist is opgelost.

1. Typ de volgende opdracht verbinding maken met het domein lid zijn van CentOS virtuele machine via SSH in uw terminal SSH. Gebruik een domeinaccount dat deel uitmaakt van het beheerde domein (bijvoorbeeld 'bob@CONTOSO100.COM' in dit geval.)
    ```
    ssh -l bob@CONTOSO100.COM contoso-centos.contoso100.com
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
* [Installeren van Kerberos](https://access.redhat.com/documentation/en-US/Red_Hat_Enterprise_Linux/6/html/Managing_Smart_Cards/installing-kerberos.html)
* [Red Hat Enterprise Linux 7 - Windows Integration Guide](https://access.redhat.com/documentation/en-US/Red_Hat_Enterprise_Linux/7/html/Windows_Integration_Guide/index.html)
