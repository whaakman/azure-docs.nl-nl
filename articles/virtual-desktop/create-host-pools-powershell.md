---
title: Een Windows Virtual Desktop Preview host-pool maken met PowerShell - Azure
description: Het maken van een groep host in Windows Virtual Desktop Preview met PowerShell-cmdlets.
services: virtual-desktop
author: Heidilohr
ms.service: virtual-desktop
ms.topic: how-to
ms.date: 05/06/2019
ms.author: helohr
ms.openlocfilehash: a58e059e800b13d01ba8e50880bd75077d4418ae
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "65833972"
---
# <a name="create-a-host-pool-with-powershell"></a>Een hostpool maken met PowerShell

Host-pools zijn een verzameling van een of meer identieke virtuele machines in Windows Virtual Desktop Preview tenant omgevingen. Elke groep host kan een app-groep die gebruikers werken kunnen met net zoals op een fysieke bureaublad bevatten.

## <a name="use-your-powershell-client-to-create-a-host-pool"></a>De PowerShell-client gebruiken om te maken van een groep host

Eerste, [downloaden en importeren van de Windows virtuele bureaublad PowerShell-module](https://docs.microsoft.com/powershell/windows-virtual-desktop/overview) te gebruiken in uw PowerShell-sessie als u dat nog niet gedaan hebt.

Voer de volgende cmdlet om aan te melden bij de virtuele Windows-bureaublad-omgeving

```powershell
Add-RdsAccount -DeploymentUrl https://rdbroker.wvd.microsoft.com
```

Voer vervolgens deze cmdlet voor het maken van een nieuwe groep van de host in uw tenant virtuele Windows-bureaublad:

```powershell
New-RdsHostPool -TenantName <tenantname> -Name <hostpoolname>
```

Voer de volgende cmdlet voor het maken van een registratietoken voor het autoriseren van een sessiehost aan de host-pool worden toegevoegd en deze opslaan naar een nieuw bestand op uw lokale computer. U kunt opgeven hoe lang het registratietoken is geldig met behulp van de parameter - ExpirationHours.

```powershell
New-RdsRegistrationInfo -TenantName <tenantname> -HostPoolName <hostpoolname> -ExpirationHours <number of hours> | Select-Object -ExpandProperty Token > <PathToRegFile>
```

Hierna kunt u deze cmdlet voor Azure Active Directory-gebruikers toevoegen aan de standaardgroep voor desktop-app voor de host-toepassingen uitvoeren.

```powershell
Add-RdsAppGroupUser -TenantName <tenantname> -HostPoolName <hostpoolname> -AppGroupName "Desktop Application Group" -UserPrincipalName <userupn>
```

De **toevoegen RdsAppGroupUser** cmdlet biedt geen ondersteuning voor toevoegen van beveiligingsgroepen en slechts één gebruiker tegelijk aan de app-groep wordt toegevoegd. Als u wilt dat meerdere gebruikers toevoegen aan de app-groep, voert u de cmdlet met de juiste gebruiker SPN-namen opnieuw uit.

Voer de volgende cmdlet voor het exporteren van het registratietoken aan een variabele die u later zult gebruiken in [registreren van de virtuele machines aan de groep met virtuele Windows-bureaublad host](#register-the-virtual-machines-to-the-windows-virtual-desktop-preview-host-pool).

```powershell
$token = (Export-RdsRegistrationInfo -TenantName <tenantname> -HostPoolName <hostpoolname>).Token
```

## <a name="create-virtual-machines-for-the-host-pool"></a>Virtuele machines voor de host-pool maken

U kunt nu een virtuele machine van Azure die kan worden toegevoegd aan uw virtuele Windows-bureaublad host-pool maken.

U kunt een virtuele machine maken op verschillende manieren:

- [Een virtuele machine maken van de installatiekopie van een Azure-galerie](https://docs.microsoft.com/azure/virtual-machines/windows/quick-create-portal#create-virtual-machine)
- [Een virtuele machine van een beheerde installatiekopie maken](https://docs.microsoft.com/azure/virtual-machines/windows/create-vm-generalized-managed)
- [Een virtuele machine uit een niet-beheerde installatiekopie maken](https://github.com/Azure/azure-quickstart-templates/tree/master/101-vm-from-user-image)

## <a name="prepare-the-virtual-machines-for-windows-virtual-desktop-preview-agent-installations"></a>De virtuele machines voor Windows Virtual Desktop Preview agentinstallaties voorbereiden

U moet de volgende dingen doen om uw virtuele machines voorbereiden voordat u kunt de virtuele bureaublad van Windows-agents installeren en registreren van de virtuele machines aan uw servergroep virtueel bureaublad van Windows-host:

- U moet de machine domain-join. Dit kan binnenkomende virtuele Windows-bureaublad gebruikers uit hun Azure Active Directory-account worden toegewezen aan het Active Directory-account en toegang tot de virtuele machine is toegestaan.
- Als de virtuele machine een Windows Server-besturingssysteem wordt uitgevoerd, moet u de Remote Desktop Session Host (RDSH)-rol installeren. De RDSH-functie kan de virtuele bureaublad van Windows-agents voor een juiste installatie.

Is domain-join, doe dan het volgende op elke virtuele machine:

1. [Verbinding maken met de virtuele machine](https://docs.microsoft.com/azure/virtual-machines/windows/quick-create-portal#connect-to-virtual-machine) met de referenties die u hebt opgegeven bij het maken van de virtuele machine.
2. Op de virtuele machine, start u **Configuratiescherm** en selecteer **System**.
3. Selecteer **computernaam**, selecteer **instellingen wijzigen**, en selecteer vervolgens **wijzigen...**
4. Selecteer **domein** en voer de Active Directory-domein op het virtuele netwerk.
5. Verifiëren met een domeinaccount met bevoegdheden voor domeindeelname computers.

    >[!NOTE]
    > Als u bent lid wordt van uw virtuele machines naar een Azure AD Domain Services-omgeving, controleert u of uw lid worden van domeingebruiker ook lid is van de [groep AAD DC Administrators](https://docs.microsoft.com/azure/active-directory-domain-services/active-directory-ds-getting-started-admingroup#task-3-configure-administrative-group).

## <a name="register-the-virtual-machines-to-the-windows-virtual-desktop-preview-host-pool"></a>Registreren van de virtuele machines aan de groep van de host Windows Virtual Desktop Preview

Registreren van de virtuele machines naar een groep met virtuele Windows-bureaublad-host is net zo eenvoudig als het installeren van de virtuele bureaublad van Windows-agents.

Doe het volgende op elke virtuele machine voor het registreren van de virtuele bureaublad van Windows-agents:

1. [Verbinding maken met de virtuele machine](https://docs.microsoft.com/azure/virtual-machines/windows/quick-create-portal#connect-to-virtual-machine) met de referenties die u hebt opgegeven bij het maken van de virtuele machine.
2. Download en installeer de Windows-Agent voor virtuele bureaublad.
   - Download de [Windows-Agent voor virtuele bureaublad](https://query.prod.cms.rt.microsoft.com/cms/api/am/binary/RWrmXv).
   - Met de rechtermuisknop op het gedownloade installatieprogramma, selecteert u **eigenschappen**, selecteer **opheffen van blokkeringen**en selecteer vervolgens **OK**. Hierdoor kunnen uw systeem te vertrouwen van het installatieprogramma.
   - Voer het installatieprogramma. Wanneer u het installatieprogramma voor de token van de inschrijving wordt gevraagd, voert u de waarde die u hebt verkregen via de **Export-RdsRegistrationInfo** cmdlet.
3. Download en installeer de Windows virtuele bureaublad Agent Bootloader.
   - Download de [Windows Virtual PC-Agent Bootloader](https://query.prod.cms.rt.microsoft.com/cms/api/am/binary/RWrxrH).
   - Met de rechtermuisknop op het gedownloade installatieprogramma, selecteert u **eigenschappen**, selecteer **opheffen van blokkeringen**en selecteer vervolgens **OK**. Hierdoor kunnen uw systeem te vertrouwen van het installatieprogramma.
   - Voer het installatieprogramma.

>[!IMPORTANT]
>Als u wilt beveiligen uw virtuele Windows-bureaublad-omgeving in Azure, wordt aangeraden dat u binnenkomende poort 3389 op uw VM's niet openen. Virtuele Windows-bureaublad zijn vereist om een open binnenkomende poort 3389 voor gebruikers toegang krijgen tot virtuele machines van de host-pool. Als u poort 3389 voor het oplossen van problemen opent moet, raden wij aan u [just-in-time-VM-toegang](https://docs.microsoft.com/azure/security-center/security-center-just-in-time).

## <a name="next-steps"></a>Volgende stappen

Nu dat u een host-groep hebt gemaakt, kunt u deze vullen met RemoteApps. Zie voor meer informatie over het beheren van apps in een virtuele Windows-bureaublad, de zelfstudie voor groepen beheren-Apps.

> [!div class="nextstepaction"]
> [Zelfstudie voor app-groepen beheren](./manage-app-groups.md)
