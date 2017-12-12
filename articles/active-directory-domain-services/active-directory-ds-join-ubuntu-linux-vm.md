---
title: 'Azure Active Directory Domain Services: Een Ubuntu VM toevoegen aan een beheerd domein | Microsoft Docs'
description: Een virtuele Ubuntu Linux-machine toevoegen aan Azure AD Domain Services
services: active-directory-ds
documentationcenter: 
author: mahesh-unnikrishnan
manager: mtillman
editor: curtand
ms.assetid: 804438c4-51a1-497d-8ccc-5be775980203
ms.service: active-directory-ds
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/03/2017
ms.author: maheshu
ms.openlocfilehash: a8a3610707ca7d00694779c4b3631e1483d6bbdd
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/11/2017
---
# <a name="join-an-ubuntu-virtual-machine-in-azure-to-a-managed-domain"></a>Een virtuele Ubuntu-machine in Azure toevoegen aan een beheerd domein
In dit artikel leest u hoe een virtuele Ubuntu Linux-machine toevoegen aan een beheerd domein van Azure AD Domain Services.


## <a name="before-you-begin"></a>Voordat u begint
Als u wilt uitvoeren van de taken worden in dit artikel worden vermeld, hebt u het volgende nodig:  
1. Een geldige **Azure-abonnement**.
2. Een **Azure AD-directory** -ofwel gesynchroniseerd met een on-premises adreslijst of een map alleen in de cloud.
3. **Azure AD Domain Services** moet zijn ingeschakeld voor de Azure AD-directory. Als u dit nog niet hebt gedaan, volgt u alle taken die worden beschreven in de [handleiding](active-directory-ds-getting-started.md).
4. Zorg ervoor dat u de IP-adressen van het beheerde domein als de DNS-servers voor het virtuele netwerk hebt geconfigureerd. Zie voor meer informatie [het bijwerken van DNS-instellingen voor de virtuele Azure-netwerk](active-directory-ds-getting-started-dns.md)
5. Voltooi de stappen die zijn vereist voor het [wachtwoorden aan uw Azure AD Domain Services beheerd domein synchroniseren](active-directory-ds-getting-started-password-sync.md).


## <a name="provision-an-ubuntu-linux-virtual-machine"></a>Een virtuele Ubuntu Linux-machine inrichten
Een Ubuntu Linux-machine inrichten in Azure met behulp van een van de volgende methoden:
* [Azure Portal](../virtual-machines/linux/quick-create-portal.md)
* [Azure CLI](../virtual-machines/linux/quick-create-cli.md)
* [Azure PowerShell](../virtual-machines/linux/quick-create-powershell.md)

> [!IMPORTANT]
> * Implementeer de virtuele machine in de **hetzelfde virtuele netwerk waarin u Azure AD Domain Services hebt ingeschakeld**.
> * Kies een **ander subnet** dan de waarin u Azure AD Domain Services hebt ingeschakeld.
>


## <a name="connect-remotely-to-the-ubuntu-linux-virtual-machine"></a>Extern verbinding maken met de Ubuntu Linux virtuele machine
De Ubuntu virtuele machine is ingericht in Azure. De volgende taak is extern verbinding maken met de virtuele machine met het lokale administrator-account gemaakt tijdens het inrichten van de virtuele machine.

Volg de instructies in het artikel [aanmelden met een virtuele machine met Linux](../virtual-machines/linux/mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).


## <a name="configure-the-hosts-file-on-the-linux-virtual-machine"></a>Het hostbestand configureren op de virtuele Linux-machine
Bewerk het bestand/etc/hosts in uw terminal SSH en bijwerken van uw machine IP-adres en hostnaam.

```
sudo vi /etc/hosts
```

Voer de volgende waarde in het hosts-bestand:

```
127.0.0.1 contoso-ubuntu.contoso100.com contoso-ubuntu
```
Hier is, contoso100.com' de DNS-domeinnaam van uw beheerde domein. 'contoso-ubuntu' is de hostnaam van de Ubuntu virtuele machine die u aan het beheerde domein deelneemt.


## <a name="install-required-packages-on-the-linux-virtual-machine"></a>Installeert de vereiste pakketten op de virtuele Linux-machine
Pakketten die zijn vereist voor domeinlidmaatschap op de virtuele machine vervolgens installeren. De volgende stappen uitvoeren:

1.  Typ de volgende opdracht om te downloaden van de lijsten pakket van de opslagplaatsen in uw terminal SSH. Deze opdracht worden de lijsten pakket om informatie over de nieuwste versies van pakketten en afhankelijkheden zijn bijgewerkt.

    ```
    sudo apt-get update
    ```

2. Typ de volgende opdracht om de vereiste pakketten te installeren.
    ```
      sudo apt-get install krb5-user samba sssd sssd-tools libnss-sss libpam-sss ntp ntpdate realmd adcli
    ```

3. U ziet een roze scherm tijdens de installatie van Kerberos. De installatie van het pakket krb5-gebruikers vraagt om de naam van de realm (in hoofdletters, alle). De installatie schrijft de [realm] en [domain_realm] secties in /etc/krb5.conf.

    > [!TIP]
    > Als de naam van uw beheerde domein, contoso100.com is, geeft u CONTOSO100.COM als de realm. Vergeet niet de realm-naam moet worden opgegeven in hoofdletters.
    >
    >


## <a name="configure-the-ntp-network-time-protocol-settings-on-the-linux-virtual-machine"></a>De NAP (Network Time Protocol)-instellingen configureren op de virtuele Linux-machine
De datum en tijd van de VM Ubuntu moeten synchroniseren met het beheerde domein. NTP-hostnaam van uw beheerde domein in het bestand /etc/ntp.conf toevoegen.

```
sudo vi /etc/ntp.conf
```

Voer de volgende waarde in het bestand ntp.conf en sla het bestand:

```
server contoso100.com
```
Hier is, contoso100.com' de DNS-domeinnaam van uw beheerde domein.

Nu synchroniseren van de Ubuntu VM datum en tijd met NTP-server en start de NAP-service:

```
sudo systemctl stop ntp
sudo ntpdate contoso100.com
sudo systemctl start ntp
```


## <a name="join-the-linux-virtual-machine-to-the-managed-domain"></a>De virtuele Linux-machine toevoegen aan het beheerde domein
Nu de vereiste pakketten zijn geïnstalleerd op de virtuele Linux-machine, de volgende taak is de virtuele machine toevoegen aan het beheerde domein.

1. Ontdek het beheerde domein met AAD Domain Services. Typ de volgende opdracht in uw terminal SSH:

    ```
    sudo realm discover CONTOSO100.COM
    ```

   > [!NOTE] 
   > **Voor probleemoplossing:** als *realm detecteren* is niet gevonden uw beheerde domein:
     * Zorg ervoor dat het domein bereikbaar is vanaf de virtuele machine (probeer ping).
     * Controleer of de virtuele machine inderdaad is geïmplementeerd voor hetzelfde virtuele netwerk waarin het beheerde domein beschikbaar is.
     * Controleer als u de DNS-serverinstellingen voor het virtuele netwerk om te verwijzen naar de domeincontrollers van het beheerde domein hebt bijgewerkt.
   >

2. Initialiseren van Kerberos. Typ de volgende opdracht in uw terminal SSH: 

    > [!TIP] 
    > * Zorg ervoor dat u een gebruiker die lid is van de groep 'AAD DC Administrators' opgeven. 
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
    sudo realm join --verbose CONTOSO100.COM -U 'bob@CONTOSO100.COM' --install=/
    ```

U krijgt een bericht ('is ingeschreven machine in de realm') wanneer de computer met succes is toegevoegd aan het beheerde domein.


## <a name="update-the-sssd-configuration-and-restart-the-service"></a>De configuratie van de SSSD bijwerken en de service opnieuw starten
1. Typ de volgende opdracht in uw terminal SSH. Open het bestand sssd.conf en breng de volgende wijziging
    ```
    sudo vi /etc/sssd/sssd.conf
    ```

2. De regel commentaar **use_fully_qualified_names = True** en sla het bestand.
    ```
    # use_fully_qualified_names = True
    ```

3. Start de service SSSD.
    ```
    sudo service sssd restart
    ```


## <a name="configure-automatic-home-directory-creation"></a>Het maken van automatische basismap configureren
Typ de volgende opdrachten in de PuTTY terminal zodat het automatisch maken van de basismap na het aanmelden van gebruikers:
```
sudo vi /etc/pam.d/common-session
```
    
Voeg de volgende regel in dit bestand onder de regel 'sessie optionele pam_sss.so' en sla het:
```
session required pam_mkhomedir.so skel=/etc/skel/ umask=0077
```


## <a name="verify-domain-join"></a>Controleer of aan domein toevoegen
Controleer of de machine is toegevoegd aan het beheerde domein. Verbinding maken met het domein Ubuntu VM die gebruikmaakt van een andere SSH-verbinding. Gebruik een domeingebruikersaccount en controleer of het gebruikersaccount correct is opgelost.

1. Typ de volgende opdracht verbinding maken met het domein lid in uw terminal SSH Ubuntu virtuele machine via SSH. Gebruik een domeinaccount die deel uitmaakt van het beheerde domein (bijvoorbeeld 'bob@CONTOSO100.COM' in dit geval.)
    ```
    ssh -l bob@CONTOSO100.COM contoso-ubuntu.contoso100.com
    ```

2. Typ de volgende opdracht om te zien of de basismap correct is geïnitialiseerd in uw terminal SSH.
    ```
    pwd
    ```

3. Typ de volgende opdracht om te zien als het groepslidmaatschap correct wordt opgelost in uw terminal SSH.
    ```
    id
    ```


## <a name="grant-the-aad-dc-administrators-group-sudo-privileges"></a>De 'AAD DC-beheerders' groep sudo-machtigingen verlenen
U kunt leden van de groep 'AAD DC-beheerders' beheerdersrechten heeft op de VM Ubuntu verlenen. Het sudo-bestand bevindt zich op /etc/sudoers. De leden van de AD-groepen die zijn toegevoegd in sudoers kunnen sudo uitvoeren.

1. Zorg dat u bent aangemeld met bevoegdheden van de beheerder in uw terminal SSH. U kunt het lokale beheerdersaccount dat u hebt opgegeven tijdens het maken van de virtuele machine. Voer de volgende opdracht:
    ```
    sudo vi /etc/sudoers
    ```

2. De volgende vermelding toevoegen aan het /etc/sudoers-bestand en sla het:
    ```
    # Add 'AAD DC Administrators' group members as admins.
    %AAD\ DC\ Administrators ALL=(ALL) NOPASSWD:ALL
    ```

3. U kunt zich nu aanmelden als lid van de groep 'AAD DC Administrators' en moet beheerdersbevoegdheden hebben op de virtuele machine.


## <a name="troubleshooting-domain-join"></a>Het oplossen van problemen aan domein toevoegen
Raadpleeg de [probleemoplossing domein](active-directory-ds-admin-guide-join-windows-vm-portal.md#troubleshooting-domain-join) artikel.


## <a name="related-content"></a>Gerelateerde inhoud
* [Azure AD Domain Services - handleiding aan de slag](active-directory-ds-getting-started.md)
* [Virtuele machine met Windows Server toevoegen aan een beheerd domein van Azure AD Domain Services](active-directory-ds-admin-guide-join-windows-vm.md)
* [Aanmelden met een virtuele machine met Linux](../virtual-machines/linux/mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).
