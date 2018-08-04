---
title: 'Azure Active Directory Domain Services: Een RHEL VM toevoegen aan een beheerd domein | Microsoft Docs'
description: Een Red Hat Enterprise Linux virtuele machine toevoegen aan Azure AD Domain Services
services: active-directory-ds
documentationcenter: ''
author: mahesh-unnikrishnan
manager: mtillman
editor: curtand
ms.assetid: d76ae997-2279-46dd-bfc5-c0ee29718096
ms.service: active-directory
ms.component: domain-services
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 06/22/2018
ms.author: maheshu
ms.openlocfilehash: e84a4b0c1f3496bc26f4a8830f7a08ddd5506338
ms.sourcegitcommit: 9222063a6a44d4414720560a1265ee935c73f49e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/03/2018
ms.locfileid: "39503720"
---
# <a name="join-a-red-hat-enterprise-linux-7-virtual-machine-to-a-managed-domain"></a>Een virtuele Red Hat Enterprise Linux 7-machine toevoegen aan een beheerd domein
Dit artikel ziet u hoe u een virtuele machine van Red Hat Enterprise Linux (RHEL) 7 koppelen aan een beheerd domein van Azure AD Domain Services.

[!INCLUDE [active-directory-ds-prerequisites.md](../../includes/active-directory-ds-prerequisites.md)]

## <a name="before-you-begin"></a>Voordat u begint
Als u de taken die in dit artikel worden vermeld, hebt u het volgende nodig:  
1. Een geldige **Azure-abonnement**.
2. Een **Azure AD-directory** -een gesynchroniseerd met een on-premises directory of een map alleen in de cloud.
3. **Azure AD Domain Services** moet zijn ingeschakeld voor de Azure AD-directory. Als u dit nog niet hebt gedaan, volgt u alle taken die worden beschreven in de [introductiehandleiding](active-directory-ds-getting-started.md).
4. Zorg ervoor dat u de IP-adressen van het beheerde domein als de DNS-servers voor het virtuele netwerk hebt geconfigureerd. Zie voor meer informatie, [DNS-instellingen voor de Azure-netwerk bijwerken](active-directory-ds-getting-started-dns.md)
5. De stappen die nodig zijn om te voltooien [Synchroniseer de wachtwoorden voor uw Azure AD Domain Services beheerde domein](active-directory-ds-getting-started-password-sync.md).


## <a name="provision-a-red-hat-enterprise-linux-virtual-machine"></a>Een Red Hat Enterprise Linux-machine inrichten
Een RHEL 7-machine inrichten in Azure met behulp van een van de volgende methoden:
* [Azure Portal](../virtual-machines/linux/quick-create-portal.md)
* [Azure-CLI](../virtual-machines/linux/quick-create-cli.md)
* [Azure PowerShell](../virtual-machines/linux/quick-create-powershell.md)

> [!IMPORTANT]
> * Implementeer de virtuele machine in de **hetzelfde virtuele netwerk waarin u Azure AD Domain Services hebt ingeschakeld**.
> * Kies een **ander subnet** dan de versie waarin u Azure AD Domain Services hebt ingeschakeld.
>


## <a name="connect-remotely-to-the-newly-provisioned-linux-virtual-machine"></a>Extern verbinding maken met de nieuw ingerichte virtuele Linux-machine
De RHEL 7.2 virtuele machine is ingericht in Azure. De volgende taak bestaat uit het op afstand verbinding maken met de virtuele machine met behulp van de lokale administrator-account dat is gemaakt tijdens het inrichten van de virtuele machine.

Volg de instructies in het artikel [Meld u aan een virtuele machine waarop Linux wordt uitgevoerd bij het](../virtual-machines/linux/mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).


## <a name="configure-the-hosts-file-on-the-linux-virtual-machine"></a>Het hosts-bestand op de Linux-machine configureren
Bewerk het bestand/etc/hosts in de terminal SSH en bijwerken van uw IP-adres en hostnaam.

```
sudo vi /etc/hosts
```

Voer de volgende waarde in het hosts-bestand:

```
127.0.0.1 contoso-rhel.contoso100.com contoso-rhel
```
Hier is 'contoso100.com' de DNS-domeinnaam van uw beheerde domein. 'contoso-rhel' is de hostnaam van de RHEL VM die u aan het beheerde domein deelneemt.


## <a name="install-required-packages-on-the-linux-virtual-machine"></a>Installeer de vereiste pakketten op de Linux-machine
Vervolgens installeert u pakketten zijn vereist voor domeindeelname op de virtuele machine. Typ de volgende opdracht uit om de vereiste pakketten te installeren in uw terminal SSH:

    ```
    sudo yum install realmd sssd krb5-workstation krb5-libs samba-common-tools
    ```


## <a name="join-the-linux-virtual-machine-to-the-managed-domain"></a>De virtuele Linux-machine toevoegen aan het beheerde domein
Nu dat de vereiste pakketten zijn geïnstalleerd op de Linux-machine, bestaat de volgende taak uit de virtuele machine toevoegen aan het beheerde domein.

1. Ontdek het AAD Domain Services beheerde domein. Typ de volgende opdracht in uw terminal SSH:

    ```
    sudo realm discover CONTOSO100.COM
    ```

     > [!NOTE]
     > **Probleemoplossing:** als *realm detecteren* is niet gevonden van uw beheerde domein:
     * Zorg ervoor dat het domein bereikbaar is vanaf de virtuele machine (probeer ping).
     * Controleer dat de virtuele machine inderdaad is geïmplementeerd voor hetzelfde virtuele netwerk waarin het beheerde domein beschikbaar is.
     * Controleert u of u de DNS-serverinstellingen voor het virtuele netwerk om te verwijzen naar de domeincontrollers van het beheerde domein hebt bijgewerkt.
     >

2. Initialiseren van Kerberos. Typ de volgende opdracht in uw terminal SSH:

    > [!TIP]
    > * Zorg ervoor dat u opgeeft dat een gebruiker die deel uitmaakt van de groep 'AAD DC Administrators'.
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
Controleer of de computer is toegevoegd aan het beheerde domein. Verbinding maken met de RHEL VM met behulp van een andere SSH-verbinding van een domein. Gebruik een domeingebruikersaccount en Ga na of het gebruikersaccount juist is opgelost.

1. Typ de volgende opdracht verbinding maken met het domein lid zijn van RHEL virtuele machine via SSH in uw terminal SSH. Gebruik een domeinaccount dat deel uitmaakt van het beheerde domein (bijvoorbeeld 'bob@CONTOSO100.COM' in dit geval.)
    ```
    ssh -l bob@CONTOSO100.COM contoso-rhel.contoso100.com
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
* [Red Hat Enterprise Linux 7 - handleiding voor Windows-integratie](https://access.redhat.com/documentation/en-US/Red_Hat_Enterprise_Linux/7/html/Windows_Integration_Guide/index.html)
