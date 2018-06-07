---
title: Gedetailleerde extern bureaublad probleemoplossing in Azure | Microsoft Docs
description: Bekijk de gedetailleerde stappen voor het externe bureaublad fouten waar u kunt niet naar een Windows virtuele machines in Azure
services: virtual-machines-windows
documentationcenter: ''
author: genlin
manager: jeconnoc
editor: ''
tags: top-support-issue,azure-service-management,azure-resource-manager
keywords: kan geen verbinding maken met extern bureaublad, problemen met extern bureaublad, extern bureaublad geen verbinding maken, extern bureaublad-fouten, probleemoplossing extern bureaublad, problemen met extern bureaublad
ms.assetid: 9da36f3d-30dd-44af-824b-8ce5ef07e5e0
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: troubleshooting
ms.date: 05/11/2018
ms.author: genli
ms.openlocfilehash: ab101d78320819b9fb48f2c431fb0f6afdb895ec
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/01/2018
ms.locfileid: "34657822"
---
# <a name="detailed-troubleshooting-steps-for-remote-desktop-connection-issues-to-windows-vms-in-azure"></a>Gedetailleerde probleemoplossing voor problemen met extern bureaublad-verbinding voor Windows-VM's in Azure
In dit artikel bevat gedetailleerde stappen voor probleemoplossing opsporen en oplossen van complexe extern bureaublad-fouten voor op basis van Windows Azure virtual machines.

> [!IMPORTANT]
> Om te voorkomen de veelvoorkomende fouten voor extern bureaublad, zorg ervoor dat u Lees [de basic probleemoplossingsartikel voor extern bureaublad](troubleshoot-rdp-connection.md) voordat u doorgaat.

U kunt tegenkomen een extern bureaublad foutbericht die op een van de specifieke foutberichten niet lijken behandeld in [de basic extern bureaublad-probleemoplossingsgids](troubleshoot-rdp-connection.md). Volg deze stappen om te bepalen waarom de Remote Desktop (RDP)-client kan geen verbinding maken met de RDP-service op de virtuele machine in Azure.

[!INCLUDE [learn-about-deployment-models](../../../includes/learn-about-deployment-models-both-include.md)]

Als u meer hulp op elk gewenst moment in dit artikel nodig hebt, kunt u de Azure-experts raadplegen op [de Azure MSDN en de Stack Overflow-forums](https://azure.microsoft.com/support/forums/). U kunt ook kunt u ook een incident voor ondersteuning van Azure bestand. Ga naar de [ondersteuning van Azure site](https://azure.microsoft.com/support/options/) en klik op **Get Support**. Voor meer informatie over het gebruik van Azure-ondersteuning, de [Veelgestelde vragen over Microsoft Azure-ondersteuning](https://azure.microsoft.com/support/faq/).

## <a name="components-of-a-remote-desktop-connection"></a>Onderdelen van een extern bureaublad-verbinding
De volgende onderdelen betrokken zijn bij een RDP-verbinding:

![](./media/detailed-troubleshoot-rdp/tshootrdp_0.png)

Voordat u doorgaat, kan het handig geestelijk bekijken wat is gewijzigd sinds de laatste geslaagde extern bureaublad-verbinding naar de virtuele machine. Bijvoorbeeld:

* Het openbare IP-adres van de virtuele machine of de cloudservice met de virtuele machine (ook wel het virtuele IP-adres [VIP](https://en.wikipedia.org/wiki/Virtual_IP_address)) is gewijzigd. De RDP-fout kan worden, omdat de DNS client-cache nog steeds heeft de *oude IP-adres* geregistreerd voor de DNS-naam. Uw DNS-client-cache leegmaken en probeer de virtuele machine opnieuw verbinding te maken. Of probeer verbinding te maken met het nieuwe VIP rechtstreeks.
* U gebruikt een toepassing van derden voor het beheren van uw extern bureaublad-verbindingen in plaats van de verbinding die is gegenereerd door de Azure-portal. Controleer of de configuratie van de toepassing bevat de juiste TCP-poort voor het extern bureaublad-verkeer. U kunt deze poort voor een klassieke virtuele machine in controleren de [Azure-portal](https://portal.azure.com), door te klikken op de VM-instellingen > eindpunten.

## <a name="preliminary-steps"></a>Voorbereidende stappen
Voordat u doorgaat naar de gedetailleerde probleemoplossing

* Controleer de status van de virtuele machine in de Azure portal voor de hand liggende problemen.
* Ga als volgt de [quick fix stappen voor veelvoorkomende RDP-fouten in de basis probleemoplossingsgids](troubleshoot-rdp-connection.md#quick-troubleshooting-steps).
* Zorg ervoor dat uw VHD vóór de upload het goed is voorbereid aangepaste installatiekopieën. Zie voor meer informatie [voorbereiden van een Windows-VHD of VHDX uploaden naar Azure](prepare-for-upload-vhd-image.md).


Probeer opnieuw verbinding te maken met de virtuele machine via Extern bureaublad na deze stappen.

## <a name="detailed-troubleshooting-steps"></a>Gedetailleerde stappen voor het oplossen van problemen
De extern bureaublad-client mogelijk niet kunnen bereiken van de extern bureaublad-service op de Azure VM vanwege problemen met de volgende bronnen:

* [Extern-bureaubladclient computer](#source-1-remote-desktop-client-computer)
* [Organisatie intranet randapparaat](#source-2-organization-intranet-edge-device)
* [De cloud service-eindpunt en toegangsbeheerlijsten (ACL)](#source-3-cloud-service-endpoint-and-acl)
* [Netwerkbeveiligingsgroepen](#source-4-network-security-groups)
* [Op basis van Windows Azure VM](#source-5-windows-based-azure-vm)

## <a name="source-1-remote-desktop-client-computer"></a>: 1 extern-bureaubladclient broncomputer
Controleer of dat de computer extern bureaublad-verbindingen met een andere on-premises, op basis van Windows-computer kunt maken.

![](./media/detailed-troubleshoot-rdp/tshootrdp_1.png)

Als u niet, Controleer op de volgende instellingen op uw computer:

* Een lokale firewall-instelling wordt extern bureaublad-verkeer blokkeert.
* Lokaal geïnstalleerde clientsoftware proxy dat voorkomt extern bureaublad-verbindingen dat.
* Lokaal geïnstalleerde software die wordt verhinderd door een extern bureaublad-verbindingen voor netwerkbeheer.
* Andere typen beveiligingssoftware die om verkeer te controleren of in een specifieke typen verkeer dat wordt verhinderd door een extern bureaublad-verbindingen toestaan/weigeren.

In dergelijke gevallen tijdelijk uitschakelen van de software en probeer het verbinding maken met een on-premises computer via Extern bureaublad. Als u de werkelijke oorzaak op deze manier vinden kunt, samen met de netwerkbeheerder om het corrigeren van de software-instellingen voor extern bureaublad-verbindingen.

## <a name="source-2-organization-intranet-edge-device"></a>Bron 2: Organisatie intranet randapparaat
Controleer of dat een computer die rechtstreeks zijn verbonden met Internet kan worden gebruikt voor het aanbrengen van extern bureaublad-verbindingen met uw Azure virtuele machine.

![](./media/detailed-troubleshoot-rdp/tshootrdp_2.png)

Als u geen over een computer die direct is verbonden met Internet maken en testen met een nieuwe Azure virtuele machine in een bronservice groep of cloud. Zie voor meer informatie [maken van een virtuele machine waarop Windows wordt uitgevoerd in Azure](../virtual-machines-windows-hero-tutorial.md). Nadat de test kunt u de virtuele machine en de resourcegroep of de cloudservice verwijderen.

Als u een extern bureaublad-verbinding met een computer die rechtstreeks zijn verbonden met Internet maken kunt, controleert u het apparaat aan uw organisatie intranet rand voor:

* Een interne firewall blokkeert HTTPS-verbindingen met het Internet.
* Een proxyserver extern bureaublad-verbindingen te blokkeren.
* Inbraakdetectie of software die wordt uitgevoerd op apparaten in uw netwerk rand dat voorkomt extern bureaublad-verbindingen dat voor netwerkbeheer.

Werken met de netwerkbeheerder om de instellingen van uw organisatie intranet edge-apparaat om HTTPS-gebaseerde extern bureaublad-verbindingen met het Internet toe te corrigeren.

## <a name="source-3-cloud-service-endpoint-and-acl"></a>Bron 3: De Cloud service-eindpunt en ACL
Controleer voor VM's zijn gemaakt met behulp van het klassieke implementatiemodel, of dat een andere virtuele machine van Azure die zich in dezelfde cloudservice- of virtueel netwerk extern bureaublad-verbindingen in uw Azure-VM aanbrengen kunt.

![](./media/detailed-troubleshoot-rdp/tshootrdp_3.png)

> [!NOTE]
> Voor virtuele machines in de Resource Manager gemaakt, gaat u verder met [bron 4: Netwerkbeveiligingsgroepen](#source-4-network-security-groups).

Als u geen een andere virtuele machine in dezelfde cloudservice- of virtueel netwerk, een maken. Volg de stappen in [maken van een virtuele machine waarop Windows wordt uitgevoerd in Azure](../virtual-machines-windows-hero-tutorial.md). Verwijder de virtuele testmachine nadat de test is voltooid.

Als u een verbinding via Extern bureaublad met een virtuele machine in dezelfde cloudservice- of virtueel netwerk maken kan, controleert deze instellingen:

* De eindpuntconfiguratie voor extern bureaublad-verkeer op de doel-virtuele machine: de persoonlijke TCP-poort van het eindpunt moet overeenkomen met de TCP-poort waarop Extern bureaublad-service van de VM luistert (de standaardwaarde is 3389).
* De ACL voor het eindpunt van de extern bureaublad-verkeer op de doel-virtuele machine: ACL's kunnen u opgeven toegestaan of geweigerd inkomend verkeer van Internet op basis van de bron-IP-adres. Onjuist geconfigureerde ACL's kunnen voorkomen dat binnenkomend verkeer van de extern bureaublad naar het eindpunt. Controleer de ACL's om ervoor te zorgen dat binnenkomend verkeer van uw openbare IP-adressen van uw proxy of andere edge-server is toegestaan. Zie voor meer informatie [wat is er een netwerk lijst ACL (Access Control)?](../../virtual-network/virtual-networks-acl.md)

Verwijder het huidige eindpunt en maak een nieuwe, kiezen van een willekeurige poort tussen 49152 – 65535 voor het poortnummer van de externe om te controleren of het eindpunt de bron van het probleem. Zie voor meer informatie [het instellen van eindpunten aan een virtuele machine](classic/setup-endpoints.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json).

## <a name="source-4-network-security-groups"></a>Bron 4: Netwerkbeveiligingsgroepen
Netwerkbeveiligingsgroepen toestaan meer gedetailleerd beheer van toegestane binnenkomend en uitgaand verkeer. U kunt maken van regels spanning subnetten en cloudservices in een Azure-netwerk.

Gebruik [IP-stroom controleren](../../network-watcher/network-watcher-check-ip-flow-verify-portal.md) om te bevestigen of verkeer naar of van een virtuele machine wordt geblokkeerd door een regel in een netwerkbeveiligingsgroep. U kunt ook een effectieve beveiligingsmethode groep regels om ervoor te zorgen inkomende 'toestaan' NSG bekijken regel bestaat en is geplaatst voor RDP-poort (standaard 3389). Zie voor meer informatie [effectieve beveiligingsregels gebruiken om op te lossen VM verkeer stroom](../../virtual-network/diagnose-network-traffic-filter-problem.md).

## <a name="source-5-windows-based-azure-vm"></a>Bron 5: Op basis van Windows Azure VM
![](./media/detailed-troubleshoot-rdp/tshootrdp_5.png)

Volg de instructies in [in dit artikel](reset-rdp.md). In dit artikel stelt de extern bureaublad-service op de virtuele machine:

* De standaardregel 'Extern bureaublad' Windows Firewall (TCP-poort 3389) inschakelen.
* Extern bureaublad-verbindingen inschakelen door de registerwaarde HKLM\System\CurrentControlSet\Control\Terminal Server\fDenyTSConnections op 0.

Probeer opnieuw de verbinding van uw computer. Als u nog steeds geen verbinding maken via Extern bureaublad, controleren op mogelijke problemen met het volgende:

* De extern bureaublad-service wordt niet uitgevoerd op de doel-virtuele machine.
* De extern bureaublad-service luistert niet op TCP-poort 3389.
* Windows Firewall of een andere lokale firewall heeft een uitgaande regel dat voorkomt extern bureaublad-verkeer dat.
* Inbraakdetectie of netwerkbeheer software die wordt uitgevoerd op virtuele machine van Azure kan geen extern bureaublad-verbindingen.

Voor virtuele machines gemaakt met behulp van het klassieke implementatiemodel, kunt u een externe Azure PowerShell-sessie met de Azure virtuele machine. U moet eerst een certificaat van de virtuele machine hosting cloudservice installeren. Ga naar [beveiligde externe PowerShell toegang configureren voor Azure Virtual Machines](http://gallery.technet.microsoft.com/scriptcenter/Configures-Secure-Remote-b137f2fe) en download de **InstallWinRMCertAzureVM.ps1** -scriptbestand op uw lokale computer.

Vervolgens installeert u Azure PowerShell als u dat nog niet gedaan hebt. Zie [Azure PowerShell installeren en configureren](/powershell/azure/overview).

Open een Azure PowerShell-opdrachtprompt en wijzig de huidige map naar de locatie van de **InstallWinRMCertAzureVM.ps1** scriptbestand. U kunt een Azure PowerShell-script uitvoeren, moet u het juiste uitvoeringsbeleid instellen. Voer de **Get-ExecutionPolicy** opdracht om te bepalen van uw huidige beleidsniveau. Zie voor meer informatie over het instellen van het juiste niveau [Set-ExecutionPolicy](https://technet.microsoft.com/library/hh849812.aspx).

Vul vervolgens de naam van uw Azure-abonnement, de naam van de cloudservice en de naam van uw virtuele machine (verwijderen van de < en > tekens), en voer deze opdrachten.

```powershell
$subscr="<Name of your Azure subscription>"
$serviceName="<Name of the cloud service that contains the target virtual machine>"
$vmName="<Name of the target virtual machine>"
.\InstallWinRMCertAzureVM.ps1 -SubscriptionName $subscr -ServiceName $serviceName -Name $vmName
```

U krijgt de naam van het juiste abonnement van de *SubscriptionName* eigenschap van de weergave van de **Get-AzureSubscription** opdracht. U krijgt de naam van de cloudservice voor de virtuele machine van de *ServiceName* kolom in de weergave van de **Get-AzureVM** opdracht.

Controleer of u het nieuwe certificaat hebt. Open een module Certificaten voor de huidige gebruiker en kijk in de **Trusted Root Certification certificeringsinstanties\certificaten** map. Er is een certificaat met de DNS-naam van uw cloudservice in de kolom verleend aan (voorbeeld: cloudservice4testing.cloudapp.net).

Vervolgens een externe Azure PowerShell-sessie starten met behulp van deze opdrachten.

```powershell
$uri = Get-AzureWinRMUri -ServiceName $serviceName -Name $vmName
$creds = Get-Credential
Enter-PSSession -ConnectionUri $uri -Credential $creds
```

Na het invoeren van geldige beheerdersreferenties, ziet u iets soortgelijks als in de volgende Azure PowerShell-prompt:

```powershell
[cloudservice4testing.cloudapp.net]: PS C:\Users\User1\Documents>
```

Het eerste deel van deze vraag is de naam van uw cloud-service met de doel-virtuele machine, die kan afwijken van 'cloudservice4testing.cloudapp.net'. Nu kunt u Azure PowerShell-opdrachten voor deze cloudservice om de problemen te onderzoeken vermeld en corrigeer de configuratie uitgeven.

### <a name="to-manually-correct-the-remote-desktop-services-listening-tcp-port"></a>De extern bureaublad-Services TCP-poort luistert handmatig corrigeren
Bij de externe Azure PowerShell-sessie-prompt, moet u deze opdracht uitvoeren.

```powershell
Get-ItemProperty -Path "HKLM:\System\CurrentControlSet\Control\Terminal Server\WinStations\RDP-Tcp" -Name "PortNumber"
```

De eigenschap PortNumber geeft het huidige poortnummer. Wijzig indien nodig de extern bureaublad poort nummer terug naar de standaardwaarde (3389) met deze opdracht.

```powershell
Set-ItemProperty -Path "HKLM:\System\CurrentControlSet\Control\Terminal Server\WinStations\RDP-Tcp" -Name "PortNumber" -Value 3389
```

Controleer of dat de poort is gewijzigd in 3389 met deze opdracht.

```powershell
Get-ItemProperty -Path "HKLM:\System\CurrentControlSet\Control\Terminal Server\WinStations\RDP-Tcp" -Name "PortNumber"
```

Sluit de externe Azure PowerShell-sessie met deze opdracht.

```powershell
Exit-PSSession
```

Controleer of de extern bureaublad-eindpunt voor de Azure VM is ook TCP-poort 3398 als de interne poort. Start van de Azure VM en probeer opnieuw de verbinding met extern bureaublad.

## <a name="additional-resources"></a>Aanvullende resources
[Het opnieuw instellen van een wachtwoord of de service Extern bureaublad voor virtuele machines van Windows](reset-rdp.md)

[Azure PowerShell installeren en configureren](/powershell/azure/overview)

[Secure Shell (SSH)-verbindingen met een op basis van Linux virtuele machine van Azure oplossen](../linux/troubleshoot-ssh-connection.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

[Problemen oplossen met toegang tot een toepassing die wordt uitgevoerd op een virtuele Azure-machine](../linux/troubleshoot-app-connection.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

