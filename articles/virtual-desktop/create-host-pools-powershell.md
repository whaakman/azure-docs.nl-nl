---
title: Een host-pool maken met PowerShell (preview) - Azure
description: Het maken van een host van toepassingen in virtuele Windows-bureaublad met PowerShell-cmdlets.
services: virtual-desktop
author: Heidilohr
ms.service: virtual-desktop
ms.topic: how-to
ms.date: 03/21/2019
ms.author: helohr
ms.openlocfilehash: 4b65d7614db94a9cc3fdca3f4b784c2c84ebaef8
ms.sourcegitcommit: 90dcc3d427af1264d6ac2b9bde6cdad364ceefcc
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/21/2019
ms.locfileid: "58318537"
---
# <a name="create-a-host-pool-with-powershell-preview"></a>Een host-pool maken met PowerShell (Preview)

Host-pools zijn een verzameling van een of meer identieke virtuele machines in virtuele Windows-bureaublad-tenant (preview)-omgevingen. Elke groep host kan een app-groep die gebruikers werken kunnen met net zoals op een fysieke bureaublad bevatten.

## <a name="use-your-powershell-client-to-create-a-host-pool"></a>De PowerShell-client gebruiken om te maken van een groep host

Eerste, [downloaden en importeren van de Windows virtuele bureaublad PowerShell-module](https://docs.microsoft.com/powershell/windows-virtual-desktop/overview) te gebruiken in uw PowerShell-sessie als u dat nog niet gedaan hebt.

Voer de volgende cmdlet om aan te melden bij de virtuele Windows-bureaublad-omgeving

```powershell
Add-RdsAccount -DeploymentUrl https://rdbroker.wvd.microsoft.com
```

Hierna kunt u de volgende cmdlet om in te stellen van de context aan uw tenant-groep uitvoeren. Als u de naam van de groep tenant hebt, is uw tenant zich waarschijnlijk in de 'standaard-Tenant groep' zodat u verder kunt gaan met deze cmdlet.

```powershell
Set-RdsContext -TenantGroupName <tenantgroupname>
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

Voer de volgende cmdlet voor het exporteren van het registratietoken aan een variabele die u later zult gebruiken in [registreren van de virtuele machines aan de groep met virtuele Windows-bureaublad host](#register-the-virtual-machines-to-the-windows-virtual-desktop-host-pool).

```powershell
$token = (Export-RdsRegistrationInfo -TenantName <tenantname> -HostPoolName <hostpoolname>).Token
```

## <a name="create-virtual-machines-for-the-host-pool"></a>Virtuele machines voor de host-pool maken

U kunt nu een virtuele machine van Azure die kan worden toegevoegd aan uw virtuele Windows-bureaublad host-pool maken.

U kunt een virtuele machine maken op verschillende manieren:

- [Een virtuele machine maken van de installatiekopie van een Azure-galerie](https://docs.microsoft.com/azure/virtual-machines/windows/quick-create-portal#create-virtual-machine)
- [Een virtuele machine van een beheerde installatiekopie maken](https://docs.microsoft.com/azure/virtual-machines/windows/create-vm-generalized-managed)
- [Een virtuele machine uit een niet-beheerde installatiekopie maken](https://github.com/Azure/azure-quickstart-templates/tree/master/101-vm-from-user-image)

## <a name="prepare-the-virtual-machines-for-windows-virtual-desktop-agent-installations"></a>De virtuele machines voorbereiden voor virtuele Windows-bureaublad agentinstallaties

U moet de volgende dingen doen om uw virtuele machines voorbereiden voordat u kunt de virtuele bureaublad van Windows-agents installeren en registreren van de virtuele machines aan uw servergroep virtueel bureaublad van Windows-host:

- U moet de machine domain-join. Dit kan binnenkomende virtuele Windows-bureaublad gebruikers uit hun Azure Active Directory-account worden toegewezen aan het Active Directory-account en toegang tot de virtuele machine is toegestaan.
- Als de virtuele machine een Windows Server-besturingssysteem wordt uitgevoerd, moet u de Remote Desktop Session Host (RDSH)-functie (preview) installeren. De RDSH-functie kan de virtuele bureaublad van Windows-agents voor een juiste installatie.

Is domain-join, doe dan het volgende op elke virtuele machine:

1. [Verbinding maken met de virtuele machine](https://docs.microsoft.com/azure/virtual-machines/windows/quick-create-portal#connect-to-virtual-machine) met de referenties die u hebt opgegeven bij het maken van de virtuele machine.
2. Op de virtuele machine, start u **Configuratiescherm** en selecteer **System**.
3. Selecteer **computernaam**, selecteer **instellingen wijzigen**, en selecteer vervolgens **wijzigen...**
4. Selecteer **domein** en voer de Active Directory-domein op het virtuele netwerk.
5. Verifiëren met een domeinaccount met bevoegdheden voor domeindeelname computers.

## <a name="register-the-virtual-machines-to-the-windows-virtual-desktop-host-pool"></a>Registreren van de virtuele machines aan de groep van de host virtuele Windows-bureaublad

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
4. Installeer of activeren van de virtuele Windows-bureaublad side-by-side-stack. De stappen zijn verschillend, afhankelijk van welke versie van het besturingssysteem de virtuele machine wordt gebruikt.
   - Als uw virtuele machine besturingssysteem Windows Server 2016:
     - Download de [virtuele Windows-bureaublad side-by-side stack](https://go.microsoft.com/fwlink/?linkid=2084270).
     - Met de rechtermuisknop op het gedownloade installatieprogramma, selecteert u **eigenschappen**, selecteer **opheffen van blokkeringen**en selecteer vervolgens **OK**. Hierdoor kunnen uw systeem te vertrouwen van het installatieprogramma.
     - Voer het installatieprogramma.
   - Als uw virtuele machine besturingssysteem Windows 10 1809 of hoger of WindowsServer 2019 of hoger:
     - Download de [script](https://go.microsoft.com/fwlink/?linkid=2084268) activeren van de side-by-side-stack.
     - Met de rechtermuisknop op het gedownloade script, selecteert u **eigenschappen**, selecteer **opheffen van blokkeringen**en selecteer vervolgens **OK**. Hierdoor kunnen uw systeem te vertrouwen van het script.
     - Uit de **Start** zoeken naar Windows PowerShell ISE, in het menu met de rechtermuisknop en selecteer vervolgens **als administrator uitvoeren**.
     - Selecteer **bestand**, klikt u vervolgens **openen...** , en vervolgens de PowerShell-script uit de gedownloade bestanden te zoeken en te openen.
     - Selecteer de knop play groen het script uit te voeren.

## <a name="next-steps"></a>Volgende stappen

Nu u een host-groep hebt gemaakt, is het tijd om deze vullen met RemoteApps (preview). Zie voor meer informatie over het beheren van apps in een virtuele Windows-bureaublad, de zelfstudie voor groepen beheren-Apps.

> [!div class="nextstepaction"]
> [Zelfstudie voor app-groepen beheren](./manage-app-groups.md)
