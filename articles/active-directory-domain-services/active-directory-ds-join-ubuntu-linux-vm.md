---
title: 'Azure Active Directory Domain Services: Een Ubuntu-VM toevoegen aan een beheerd domein | Microsoft Docs'
description: Een Ubuntu Linux virtuele machine toevoegen aan Azure AD Domain Services
services: active-directory-ds
documentationcenter: ''
author: eringreenlee
manager: mtillman
editor: curtand
ms.assetid: 804438c4-51a1-497d-8ccc-5be775980203
ms.service: active-directory
ms.component: domain-services
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 06/22/2018
ms.author: ergreenl
ms.openlocfilehash: 709c227c955aba6b31ce415767470fa18ff3684f
ms.sourcegitcommit: 48592dd2827c6f6f05455c56e8f600882adb80dc
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/26/2018
ms.locfileid: "50155266"
---
# <a name="join-an-ubuntu-virtual-machine-in-azure-to-a-managed-domain"></a>Een virtuele Ubuntu-machine in Azure toevoegen aan een beheerd domein
Dit artikel ziet u hoe u een Ubuntu Linux-machine koppelen aan een beheerd domein van Azure AD Domain Services.

[!INCLUDE [active-directory-ds-prerequisites.md](../../includes/active-directory-ds-prerequisites.md)]

## <a name="before-you-begin"></a>Voordat u begint
Als u de taken die in dit artikel worden vermeld, hebt u het volgende nodig:  
1. Een geldige **Azure-abonnement**.
2. Een **Azure AD-directory** -een gesynchroniseerd met een on-premises directory of een map alleen in de cloud.
3. **Azure AD Domain Services** moet zijn ingeschakeld voor de Azure AD-directory. Als u dit nog niet hebt gedaan, volgt u alle taken die worden beschreven in de [introductiehandleiding](active-directory-ds-getting-started.md).
4. Zorg ervoor dat u de IP-adressen van het beheerde domein als de DNS-servers voor het virtuele netwerk hebt geconfigureerd. Zie voor meer informatie, [DNS-instellingen voor de Azure-netwerk bijwerken](active-directory-ds-getting-started-dns.md)
5. De stappen die nodig zijn om te voltooien [Synchroniseer de wachtwoorden voor uw Azure AD Domain Services beheerde domein](active-directory-ds-getting-started-password-sync.md).


## <a name="provision-an-ubuntu-linux-virtual-machine"></a>Een Ubuntu Linux virtual machine inrichten
Een Ubuntu Linux-machine inrichten in Azure met behulp van een van de volgende methoden:
* [Azure Portal](../virtual-machines/linux/quick-create-portal.md)
* [Azure-CLI](../virtual-machines/linux/quick-create-cli.md)
* [Azure PowerShell](../virtual-machines/linux/quick-create-powershell.md)

> [!IMPORTANT]
> * Implementeer de virtuele machine in de **hetzelfde virtuele netwerk waarin u Azure AD Domain Services hebt ingeschakeld**.
> * Kies een **ander subnet** dan de versie waarin u Azure AD Domain Services hebt ingeschakeld.
>


## <a name="connect-remotely-to-the-ubuntu-linux-virtual-machine"></a>Extern verbinding maken met de virtuele Ubuntu Linux-machine
De virtuele Ubuntu-machine is ingericht in Azure. De volgende taak bestaat uit het op afstand verbinding maken met de virtuele machine met behulp van de lokale administrator-account dat is gemaakt tijdens het inrichten van de virtuele machine.

Volg de instructies in het artikel [Meld u aan een virtuele machine waarop Linux wordt uitgevoerd bij het](../virtual-machines/linux/mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).


## <a name="configure-the-hosts-file-on-the-linux-virtual-machine"></a>Het hosts-bestand op de Linux-machine configureren
Bewerk het bestand/etc/hosts in de terminal SSH en bijwerken van uw IP-adres en hostnaam.

```
sudo vi /etc/hosts
```

Voer de volgende waarde in het hosts-bestand:

```
127.0.0.1 contoso-ubuntu.contoso100.com contoso-ubuntu
```
Hier is 'contoso100.com' de DNS-domeinnaam van uw beheerde domein. 'contoso-ubuntu' is de hostnaam van de virtuele Ubuntu-machine die u aan het beheerde domein deelneemt.


## <a name="install-required-packages-on-the-linux-virtual-machine"></a>Installeer de vereiste pakketten op de Linux-machine
Vervolgens installeert u pakketten zijn vereist voor domeindeelname op de virtuele machine. Voer de volgende stappen uit:

1.  Typ de volgende opdracht voor het downloaden van de pakketlijsten vanuit de opslagplaatsen in de SSH-terminal. Met deze opdracht werkt de pakketlijsten voor informatie over de nieuwste versies van pakketten en de bijbehorende afhankelijkheden.

    ```
    sudo apt-get update
    ```

2. Typ de volgende opdracht om de vereiste pakketten te installeren.
    ```
      sudo apt-get install krb5-user samba sssd sssd-tools libnss-sss libpam-sss ntp ntpdate realmd adcli
    ```

3. Tijdens de installatie van Kerberos ziet u een roze scherm. De installatie van het pakket 'krb5-user' vraagt om de naam van de realm (in alle hoofdletters). De installatie van de schrijft het [domein] en [domain_realm] secties in /etc/krb5.conf.

    > [!TIP]
    > Als de naam van uw beheerde domein contoso100.com is, voert u CONTOSO100.COM als de realm. Let op: de naam van de realm moet in hoofdletters worden opgegeven.
    >
    >


## <a name="configure-the-ntp-network-time-protocol-settings-on-the-linux-virtual-machine"></a>De NTP (Network Time Protocol)-instellingen configureren op de virtuele Linux-machine
De datum en tijd van de Ubuntu-VM moeten worden gesynchroniseerd met het beheerde domein. Uw beheerde domein NTP hostnaam in het bestand /etc/ntp.conf toevoegen.

```
sudo vi /etc/ntp.conf
```

Voer de volgende waarde en sla het bestand in het bestand NTP.conf uitschakelen:

```
server contoso100.com
```
Hier is 'contoso100.com' de DNS-domeinnaam van uw beheerde domein.

Nu synchroniseren de datum en tijd met de NTP-server de Ubuntu-VM en start vervolgens de NTP-service:

```
sudo systemctl stop ntp
sudo ntpdate contoso100.com
sudo systemctl start ntp
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
    sudo realm join --verbose CONTOSO100.COM -U 'bob@CONTOSO100.COM' --install=/
    ```

U krijgt een bericht ('ingeschreven machine in de realm') als de machine met succes is gekoppeld aan het beheerde domein.


## <a name="update-the-sssd-configuration-and-restart-the-service"></a>Werk de configuratie van de SSSD en de service opnieuw starten
1. Typ de volgende opdracht in de SSH-terminal. Open het bestand sssd.conf en moet u de volgende wijziging
    ```
    sudo vi /etc/sssd/sssd.conf
    ```

2. Een opmerking bij de regel **use_fully_qualified_names = True** en sla het bestand.
    ```
    # use_fully_qualified_names = True
    ```

3. Start de service SSSD.
    ```
    sudo service sssd restart
    ```


## <a name="configure-automatic-home-directory-creation"></a>Het maken van automatische basismap configureren
Om in te schakelen automatisch maken van de basismap na het aanmelden van gebruikers, typt u de volgende opdrachten in de PuTTY terminal:
```
sudo vi /etc/pam.d/common-session
```

Voeg de volgende regel in dit bestand onder de lijn 'sessie optioneel pam_sss.so' en sla het:
```
session required pam_mkhomedir.so skel=/etc/skel/ umask=0077
```


## <a name="verify-domain-join"></a>Controleer of aan domein toevoegen
Controleer of de computer is toegevoegd aan het beheerde domein. Verbinding maken met de Ubuntu-VM met behulp van een andere SSH-verbinding van een domein. Gebruik een domeingebruikersaccount en Ga na of het gebruikersaccount juist is opgelost.

1. Typ de volgende opdracht verbinding maken met het domein lid zijn van Ubuntu-machine met SSH in uw terminal SSH. Gebruik een domeinaccount dat deel uitmaakt van het beheerde domein (bijvoorbeeld 'bob@CONTOSO100.COM' in dit geval.)
    ```
    ssh -l bob@CONTOSO100.COM contoso-ubuntu.contoso100.com
    ```

2. Typ in de SSH-terminal de volgende opdracht uit om te zien als de oorspronkelijke directory correct is geïnitialiseerd.
    ```
    pwd
    ```

3. Typ de volgende opdracht uit om te zien als het lidmaatschap van groepen goed worden herleid in de SSH-terminal.
    ```
    id
    ```


## <a name="grant-the-aad-dc-administrators-group-sudo-privileges"></a>De 'AAD DC Administrators' groep sudo-machtigingen verlenen
U kunt leden van de groep 'AAD DC Administrators' bevoegdheden met beheerdersrechten op de Ubuntu-VM verlenen. Het sudo-bestand bevindt zich op /etc/sudoers. De leden van de AD-beveiligingsgroepen die zijn toegevoegd in sudoers kunnen sudo uitvoeren.

1. Controleer of dat u bent aangemeld als beheerder-bevoegdheden in uw terminal SSH. U kunt het lokale administrator-account dat u hebt opgegeven tijdens het maken van de virtuele machine. Voer de volgende opdracht uit:
    ```
    sudo vi /etc/sudoers
    ```

2. Voeg de volgende vermelding toe aan de /etc/sudoers-bestand en sla het:
    ```
    # Add 'AAD DC Administrators' group members as admins.
    %AAD\ DC\ Administrators ALL=(ALL) NOPASSWD:ALL
    ```

3. U kunt nu aanmelden als een lid van de groep 'AAD DC Administrators' en u moet beheerdersbevoegdheden hebben op de virtuele machine.


## <a name="troubleshooting-domain-join"></a>Het oplossen van aan domein toevoegen
Raadpleeg de [probleemoplossing domeindeelname](active-directory-ds-admin-guide-join-windows-vm-portal.md#troubleshoot-joining-a-domain) artikel.


## <a name="related-content"></a>Gerelateerde inhoud
* [Azure AD Domain Services - handleiding aan de slag](active-directory-ds-getting-started.md)
* [Een Windows Server-machine toevoegen aan Azure AD Domain Services beheerde domein](active-directory-ds-admin-guide-join-windows-vm.md)
* [Meld u aan een virtuele machine waarop Linux wordt uitgevoerd bij het](../virtual-machines/linux/mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).
