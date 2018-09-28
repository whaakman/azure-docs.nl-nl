---
title: Gedetailleerde extern bureaublad oplossen in Azure | Microsoft Docs
description: Bekijk gedetailleerde stappen voor probleemoplossing voor extern bureaublad fouten waar u kunt niet een Windows virtuele machines in Azure
services: virtual-machines-windows
documentationcenter: ''
author: genlin
manager: jeconnoc
editor: ''
tags: top-support-issue,azure-service-management,azure-resource-manager
keywords: kan geen verbinding maken met extern bureaublad oplossen van extern bureaublad, extern bureaublad kan geen verbinding maken, extern bureaublad-fouten, probleemoplossing extern bureaublad, problemen met extern bureaublad
ms.assetid: 9da36f3d-30dd-44af-824b-8ce5ef07e5e0
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: troubleshooting
ms.date: 05/11/2018
ms.author: genli
ms.openlocfilehash: 848417eae0019b983e4e0e22be3eebcf80b4a825
ms.sourcegitcommit: b7e5bbbabc21df9fe93b4c18cc825920a0ab6fab
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/27/2018
ms.locfileid: "47413544"
---
# <a name="detailed-troubleshooting-steps-for-remote-desktop-connection-issues-to-windows-vms-in-azure"></a>Gedetailleerde stappen voor probleemoplossing voor problemen met extern bureaublad-verbinding Windows virtuele machines in Azure
In dit artikel bevat gedetailleerde stappen voor probleemoplossing kunt vaststellen en oplossen van complexe extern bureaublad-fouten voor Azure-machines op basis van Windows.

> [!IMPORTANT]
> Om te voorkomen de meest voorkomende fouten voor de extern bureaublad, zorg ervoor dat u Lees [de basic artikel over probleemoplossing voor extern bureaublad](troubleshoot-rdp-connection.md) voordat u doorgaat.

Een extern bureaublad foutbericht die op een van de specifieke foutberichten niet lijken behandeld in de kunnen optreden [de basic extern bureaublad oplossen handleiding](troubleshoot-rdp-connection.md). Volg deze stappen om te bepalen waarom de Remote Desktop (RDP)-client kan geen verbinding maken met de RDP-service op de virtuele machine van Azure.

[!INCLUDE [learn-about-deployment-models](../../../includes/learn-about-deployment-models-both-include.md)]

Als u hulp nodig hebt op elk gewenst moment in dit artikel, u kunt contact opnemen met de Azure-experts op [het Azure MSDN en Stack Overflow-forums](https://azure.microsoft.com/support/forums/). U kunt ook ook een Azure-ondersteuning-incident indienen. Ga naar de [ondersteuning voor Azure site](https://azure.microsoft.com/support/options/) en klikt u op **ontvang ondersteuning**. Voor meer informatie over het gebruik van Azure-ondersteuning, de [Veelgestelde vragen over Microsoft Azure-ondersteuning](https://azure.microsoft.com/support/faq/).

## <a name="components-of-a-remote-desktop-connection"></a>Onderdelen van een extern bureaublad-verbinding
De volgende onderdelen betrokken zijn bij een RDP-verbinding:

![](./media/detailed-troubleshoot-rdp/tshootrdp_0.png)

Voordat u doorgaat, kan het handig om te controleren mentaal wat is gewijzigd sinds de laatste geslaagde extern bureaublad verbinding met de virtuele machine. Bijvoorbeeld:

* Het openbare IP-adres van de virtuele machine of de service in de cloud met de virtuele machine (ook wel het virtuele IP-adres [VIP](https://en.wikipedia.org/wiki/Virtual_IP_address)) is gewijzigd. De RDP-fout kan zijn omdat de cache van uw DNS-client nog steeds heeft de *oude IP-adres* geregistreerd voor de DNS-naam. De DNS client-cache leegmaken en probeer de virtuele machine opnieuw verbinding te maken. Of probeer verbinding te maken met het nieuwe VIP-adres.
* U gebruikt een toepassing van derden voor het beheren van uw extern bureaublad-verbindingen in plaats van de verbinding die worden gegenereerd door de Azure-portal. Controleer of de configuratie van de toepassing bevat de juiste TCP-poort voor het extern bureaublad-verkeer. U kunt controleren op deze poort voor een klassieke virtuele machine in de [Azure-portal](https://portal.azure.com), door te klikken op de instellingen van de virtuele machine > eindpunten.

## <a name="preliminary-steps"></a>Voorbereidende stappen
Voordat u doorgaat met de gedetailleerde probleemoplossing

* Controleer de status van de virtuele machine in Azure portal voor problemen met de hand liggende.
* Ga als volgt de [quick fix stappen voor algemene RDP-fouten in de gids voor eenvoudige probleemoplossing](troubleshoot-rdp-connection.md#quick-troubleshooting-steps).
* Aangepaste installatiekopieën, zorg ervoor dat uw VHD vóór de upload het goed is voorbereid. Zie voor meer informatie, [voorbereiden van een Windows VHD of VHDX te uploaden naar Azure](../windows/prepare-for-upload-vhd-image.md).


Probeer opnieuw verbinding maken met de virtuele machine via Extern bureaublad na deze stappen.

## <a name="detailed-troubleshooting-steps"></a>Gedetailleerde stappen voor het oplossen van problemen
De extern bureaublad-client mogelijk niet kunnen bereiken van de extern bureaublad-service op de Azure-VM vanwege problemen met de volgende bronnen:

* [Extern bureaublad-client-computer](#source-1-remote-desktop-client-computer)
* [Organisatie intranet edge-apparaat](#source-2-organization-intranet-edge-device)
* [Service-eindpunt in de cloud en toegangsbeheerlijsten (ACL)](#source-3-cloud-service-endpoint-and-acl)
* [Netwerkbeveiligingsgroepen](#source-4-network-security-groups)
* [Op basis van Windows Azure-VM](#source-5-windows-based-azure-vm)

## <a name="source-1-remote-desktop-client-computer"></a>Bron 1: Extern bureaublad-client-computer
Controleer of dat de computer extern bureaublad-verbindingen naar een andere on-premises, op basis van een Windows-computer kunt maken.

![](./media/detailed-troubleshoot-rdp/tshootrdp_1.png)

Als u niet controleren op de volgende instellingen op uw computer:

* Extern bureaublad-verkeer wordt geblokkeerd door een lokale firewall-instellingen die.
* Lokaal geïnstalleerde clientsoftware proxy dat voorkomt extern bureaublad-verbindingen dat.
* Lokaal geïnstalleerde software die voorkomt extern bureaublad-verbindingen dat voor netwerkbeheer.
* Andere typen beveiligingssoftware om verkeer te controleren of bepaalde typen verkeer dat voorkomt extern bureaublad-verbindingen dat toestaan/weigeren.

In dergelijke gevallen tijdelijk uitschakelen van de software en probeer het verbinding maken met een on-premises computer via Extern bureaublad. Als u de werkelijke oorzaak op deze manier vinden kunt, werkt u met uw netwerkbeheerder om op te lossen van de software-instellingen voor extern bureaublad-verbindingen.

## <a name="source-2-organization-intranet-edge-device"></a>Bron 2: Organisatie intranet edge-apparaat
Controleer of dat een computer die rechtstreeks zijn verbonden met Internet extern bureaublad-verbindingen met uw Azure-machine kunt maken.

![](./media/detailed-troubleshoot-rdp/tshootrdp_2.png)

Als u een computer die rechtstreeks is verbonden met Internet hebt, maakt en test met een nieuwe Azure-machine in een resource of service. Zie voor meer informatie, [maken van een virtuele machine waarop Windows wordt uitgevoerd in Azure](../virtual-machines-windows-hero-tutorial.md). U kunt de virtuele machine en de resourcegroep of de cloudservice verwijderen nadat de test.

Als u een extern bureaublad-verbinding met een computer die rechtstreeks zijn verbonden met Internet maken kunt, controleert u uw organisatie intranet edge-apparaat voor:

* Een interne firewall blokkeert HTTPS-verbindingen met Internet.
* Een proxyserver extern bureaublad-verbindingen te blokkeren.
* Indringers detecteren of software die wordt uitgevoerd op apparaten in uw edge-netwerk dat voorkomt extern bureaublad-verbindingen dat voor netwerkbeheer.

Werken met uw netwerkbeheerder om op te lossen van de instellingen van uw organisatie intranet edge-apparaat voor extern bureaublad op HTTPS gebaseerde verbindingen met Internet.

## <a name="source-3-cloud-service-endpoint-and-acl"></a>Bron 3: De Cloud service-eindpunt en de ACL
Voor virtuele machines die zijn gemaakt met het klassieke implementatiemodel, controleert u of dat andere Azure-virtuele machine die zich in de dezelfde cloudservice of virtuele netwerk extern bureaublad-verbindingen in uw Azure-VM aanbrengen kunt.

![](./media/detailed-troubleshoot-rdp/tshootrdp_3.png)

> [!NOTE]
> Voor virtuele machines die zijn gemaakt in Resource Manager, gaat u naar [bron 4: Netwerkbeveiligingsgroepen](#source-4-network-security-groups).

Als u een andere virtuele machine in de dezelfde cloudservice of een virtueel netwerk hebt, maakt u een. Volg de stappen in [maken van een virtuele machine waarop Windows wordt uitgevoerd in Azure](../virtual-machines-windows-hero-tutorial.md). Verwijder de virtuele testmachine nadat de test is voltooid.

Als u verbinding via Extern bureaublad met een virtuele machine in de dezelfde cloudservice of een virtueel netwerk maken kunt, controleert deze instellingen:

* De configuratie van eindpunten voor extern bureaublad-verkeer op de doel-VM: de persoonlijke TCP-poort van het eindpunt moet overeenkomen met de TCP-poort waarop Extern bureaublad-service van de virtuele machine luistert (standaard is 3389).
* De ACL voor het eindpunt van de extern bureaublad-netwerkverkeer op de doel-VM: ACL's kunnen u opgeven toegestaan of geweigerd inkomend verkeer van Internet op basis van de bron-IP-adres. Onjuist geconfigureerde ACL's kunnen voorkomen dat inkomend verkeer van extern bureaublad naar het eindpunt. Controleer de ACL's om ervoor te zorgen dat inkomend verkeer van uw openbare IP-adressen van uw proxy of andere edge-server is toegestaan. Zie voor meer informatie, [wat is er een netwerk lijst met ACL (Access Control)?](../../virtual-network/virtual-networks-acl.md)

Als u wilt controleren of het eindpunt de oorzaak van het probleem is, het eindpunt van de huidige verwijderen en maak een nieuwe, kiezen van een willekeurige poort in het bereik 49152-65535 voor de externe poortnummer. Zie voor meer informatie, [over het instellen van eindpunten aan een virtuele machine](../windows/classic/setup-endpoints.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json).

## <a name="source-4-network-security-groups"></a>Bron 4: Netwerkbeveiligingsgroepen
Netwerkbeveiligingsgroepen toestaan gedetailleerder beheer van toegestaan binnenkomend en uitgaand verkeer. U kunt regels spanning subnetten maken en cloudservices in een Azure-netwerk.

Gebruik [IP-stroom controleren](../../network-watcher/network-watcher-check-ip-flow-verify-portal.md) om te bevestigen of verkeer naar of van een virtuele machine wordt geblokkeerd door een regel in een netwerkbeveiligingsgroep. U kunt ook bekijken beveiligingsgroepsregels om ervoor te zorgen inkomende 'toestaan' NSG-regel bestaat en met prioriteit wordt toegepast voor RDP-poort (standaard 3389). Zie voor meer informatie, [effectieve beveiligingsregels om op te lossen van de virtuele machine met behulp van de verkeersstroom](../../virtual-network/diagnose-network-traffic-filter-problem.md).

## <a name="source-5-windows-based-azure-vm"></a>Bron 5: De op basis van Windows Azure-VM
![](./media/detailed-troubleshoot-rdp/tshootrdp_5.png)

Volg de instructies in [in dit artikel](../windows/reset-rdp.md). In dit artikel stelt de extern bureaublad-service op de virtuele machine:

* Schakel de standaardregel 'Extern bureaublad' Windows Firewall (TCP-poort 3389).
* Extern bureaublad-verbindingen inschakelen door de registerwaarde HKLM\System\CurrentControlSet\Control\Terminal Server\fDenyTSConnections instelt op 0.

Probeer opnieuw de verbinding vanaf uw computer. Als u nog steeds niet lukt om via Extern bureaublad verbinding te maken, controleert u op de volgende mogelijke problemen:

* De extern bureaublad-service wordt niet uitgevoerd op de doel-VM.
* De extern bureaublad-service luistert niet op TCP-poort 3389.
* Windows Firewall of een andere lokale firewall heeft een uitgaande regel dat voorkomt verkeer van extern bureaublad dat.
* Indringers detecteren of het netwerk bewaken die worden uitgevoerd op virtuele machine van Azure voorkomt dat extern bureaublad-verbindingen.

Voor virtuele machines die zijn gemaakt met het klassieke implementatiemodel, kunt u een externe Azure PowerShell-sessie met de virtuele machine van Azure. Eerst moet u een certificaat installeren voor hosting van de virtuele machine-cloudservice. Ga naar [configureren beveiligde externe PowerShell-toegang tot Azure Virtual Machines](http://gallery.technet.microsoft.com/scriptcenter/Configures-Secure-Remote-b137f2fe) en download de **InstallWinRMCertAzureVM.ps1** scriptbestand naar uw lokale computer.

Vervolgens installeert u Azure PowerShell als u dat nog niet gedaan hebt. Zie [Azure PowerShell installeren en configureren](/powershell/azure/overview).

Vervolgens opent u een Azure PowerShell-opdrachtprompt en wijzig de huidige map naar de locatie van de **InstallWinRMCertAzureVM.ps1** scriptbestand. Als u wilt een Azure PowerShell-script uitvoeren, moet u het juiste uitvoeringsbeleid instellen. Voer de **Get-ExecutionPolicy** opdracht uit om te bepalen het beleidsniveau van uw huidige. Zie voor meer informatie over het instellen van het juiste niveau [Set-ExecutionPolicy](https://technet.microsoft.com/library/hh849812.aspx).

Vul vervolgens de naam van uw Azure-abonnement, de naam van de cloudservice en de naam van uw virtuele machine (verwijderen van de < en > tekens), en voer deze opdrachten.

```powershell
$subscr="<Name of your Azure subscription>"
$serviceName="<Name of the cloud service that contains the target virtual machine>"
$vmName="<Name of the target virtual machine>"
.\InstallWinRMCertAzureVM.ps1 -SubscriptionName $subscr -ServiceName $serviceName -Name $vmName
```

U krijgt de naam van het juiste abonnement van de *SubscriptionName* eigenschap van de weergave van de **Get-AzureSubscription** opdracht. U krijgt de naam van de cloudservice voor de virtuele machine van de *ServiceName* kolom in de weergave van de **Get-AzureVM** opdracht.

Controleer of u het nieuwe certificaat hebt. Open een certificatenmodule voor de huidige gebruiker en kijk in de **Trusted Root Certification Authorities\Certificates** map. Er is een certificaat met de DNS-naam van de cloudservice in de kolom verleend aan (voorbeeld: cloudservice4testing.cloudapp.net).

Vervolgens start u een externe Azure PowerShell-sessie met behulp van deze opdrachten.

```powershell
$uri = Get-AzureWinRMUri -ServiceName $serviceName -Name $vmName
$creds = Get-Credential
Enter-PSSession -ConnectionUri $uri -Credential $creds
```

Na het invoeren van geldige beheerdersreferenties, ziet u iets die vergelijkbaar is met de volgende Azure PowerShell-prompt:

```powershell
[cloudservice4testing.cloudapp.net]: PS C:\Users\User1\Documents>
```

Het eerste deel van deze vraag is de naam van uw cloud-service met de doel-VM, die kan afwijken van 'cloudservice4testing.cloudapp.net'. Nu kunt u Azure PowerShell-opdrachten voor deze cloudservice om de problemen te onderzoeken die worden vermeld en corrigeer de configuratie geven.

### <a name="to-manually-correct-the-remote-desktop-services-listening-tcp-port"></a>De extern bureaublad-Services TCP-poort luistert handmatig corrigeren
Voer deze opdracht uit op de externe Azure PowerShell-sessie-prompt.

```powershell
Get-ItemProperty -Path "HKLM:\System\CurrentControlSet\Control\Terminal Server\WinStations\RDP-Tcp" -Name "PortNumber"
```

De eigenschap PortNumber geeft het huidige poortnummer. Indien nodig, wijzigen de extern bureaublad poort nummer terug naar de standaardwaarde (3389) met behulp van deze opdracht.

```powershell
Set-ItemProperty -Path "HKLM:\System\CurrentControlSet\Control\Terminal Server\WinStations\RDP-Tcp" -Name "PortNumber" -Value 3389
```

Controleer of dat de poort is gewijzigd in 3389 met behulp van deze opdracht.

```powershell
Get-ItemProperty -Path "HKLM:\System\CurrentControlSet\Control\Terminal Server\WinStations\RDP-Tcp" -Name "PortNumber"
```

Sluit de externe Azure PowerShell-sessie met behulp van deze opdracht.

```powershell
Exit-PSSession
```

Controleer of de extern bureaublad-eindpunt voor de virtuele machine van Azure is ook TCP-poort 3398 als de interne poort. Start de Azure-VM opnieuw op en probeer het opnieuw de verbinding met extern bureaublad.

## <a name="additional-resources"></a>Aanvullende resources
[Opnieuw instellen van een wachtwoord of de extern bureaublad-service voor Windows-machines](../windows/reset-rdp.md)

[Azure PowerShell installeren en configureren](/powershell/azure/overview)

[Problemen met Secure Shell (SSH)-verbindingen met een Linux-gebaseerde Azure-machine oplossen](../linux/troubleshoot-ssh-connection.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

[Problemen oplossen met toegang tot een toepassing die wordt uitgevoerd op een virtuele Azure-machine](../linux/troubleshoot-app-connection.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

