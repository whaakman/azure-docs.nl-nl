---
title: Specifieke RDP-foutberichten voor virtuele Azure-machines | Microsoft Docs
description: Informatie over specifieke foutberichten die kunnen worden weergegeven bij het gebruik van extern bureaublad verbinding met een Windows-machine in Azure
keywords: 'Fout bij extern bureaublad, verbinding met extern bureaublad-fout: kan geen verbinding maken met virtuele machine, extern bureaublad oplossen'
services: virtual-machines-windows
documentationcenter: ''
author: genlin
manager: jeconnoc
editor: ''
tags: top-support-issue,azure-service-management,azure-resource-manager
ms.assetid: 5feb1d64-ee6f-4907-949a-a7cffcbc6153
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: troubleshooting
ms.date: 05/11/2018
ms.author: genli
ms.openlocfilehash: 7d4deb0618b0e0802306e344fea6d259c0597fe2
ms.sourcegitcommit: b7e5bbbabc21df9fe93b4c18cc825920a0ab6fab
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/27/2018
ms.locfileid: "47413534"
---
# <a name="troubleshooting-specific-rdp-error-messages-to-a-windows-vm-in-azure"></a>Specifieke RDP-foutberichten aan een Windows-VM in Azure oplossen
U ontvangt mogelijk een bericht specifieke fout bij het gebruik van extern bureaublad-verbinding met een Windows virtuele machine (VM) in Azure. Dit artikel worden enkele van de meestvoorkomende foutberichten die is aangetroffen, samen met stappen om op te lossen ze voor probleemoplossing. Als u problemen ondervindt bij het verbinding maken met uw virtuele machine met behulp van RDP echter niet een specifieke foutbericht worden weergegeven, raadpleegt u de [probleemoplossingsgids voor extern bureaublad](troubleshoot-rdp-connection.md).

Voor informatie over de specifieke foutberichten, Zie de volgende:

* [De externe sessie is beëindigd omdat er geen extern bureaublad-licentieservers beschikbaar, voor een licentie](#rdplicense).
* [Extern bureaublad kan de computer 'name' niet vinden](#rdpname).
* [Er is een verificatiefout opgetreden. Geen verbinding kan niet worden gemaakt met de lokale certificeringsinstantie](#rdpauth).
* [Fout in Windows-beveiliging: uw referenties niet werkte](#wincred).
* [Deze computer kan geen verbinding maken met de externe computer](#rdpconnect).

<a id="rdplicense"></a>

## <a name="the-remote-session-was-disconnected-because-there-are-no-remote-desktop-license-servers-available-to-provide-a-license"></a>De externe sessie is beëindigd omdat er geen extern bureaublad-licentieservers beschikbaar, voor een licentie.
Oorzaak: De evaluatieperiode van 120 dagen licentieverlening voor de rol extern bureaublad-Server is verlopen en moet u licenties installeren.

Als tijdelijke oplossing, een lokale kopie van het RDP-bestand opslaan in de portal en voer deze opdracht uit vanaf een opdrachtprompt PowerShell om verbinding te maken. Deze stap schakelt de licentieverlening voor alleen die verbinding:

        mstsc <File name>.RDP /admin

Als u niet echt nodig voor meer dan twee gelijktijdige extern bureaublad-verbindingen met de virtuele machine hebt, kunt u Server Manager de rol extern bureaublad-Server verwijderen.

Zie voor meer informatie het blogbericht [virtuele Azure-machine is mislukt met "Geen extern bureaublad-licentieservers beschikbaar"](https://blogs.msdn.microsoft.com/mast/2014/01/21/rdp-to-azure-vm-fails-with-no-remote-desktop-license-servers-available/).

<a id="rdpname"></a>

## <a name="remote-desktop-cant-find-the-computer-name"></a>Extern bureaublad kan de computer 'name' niet vinden.
Oorzaak: De extern bureaublad-client op uw computer kan niet de naam van de computer in de instellingen van het RDP-bestand worden omgezet.

Mogelijke oplossingen:

* Als u op het intranet van een organisatie, zorg ervoor dat de computer toegang tot de proxyserver heeft en HTTPS-verkeer naar deze verzenden kunt.
* Als u een lokaal opgeslagen RDP-bestand, probeert u met behulp van de map die wordt gegenereerd door de portal. Deze stap zorgt ervoor dat u de juiste DNS-naam voor de virtuele machine, of de service in de cloud en de poort van het eindpunt van de virtuele machine hebt. Hier volgt een voorbeeld-RDP-bestand gegenereerd door de portal:
  
        full address:s:tailspin-azdatatier.cloudapp.net:55919
        prompt for credentials:i:1

Het gedeelte van deze RDP-bestand is:

* De volledig gekwalificeerde domeinnaam van de cloudservice met de virtuele machine ("tailspin-azdatatier.cloudapp.net" in dit voorbeeld).
* De externe TCP-poort van het eindpunt voor extern bureaublad-verkeer (55919).

<a id="rdpauth"></a>

## <a name="an-authentication-error-has-occurred-the-local-security-authority-cannot-be-contacted"></a>Er is een verificatiefout opgetreden. Kan geen verbinding worden gemaakt met de lokale certificeringsinstantie.
Oorzaak: De doel-VM kan de certificeringsinstantie vinden in het gedeelte van de gebruiker de naam van uw referenties.

Wanneer uw gebruikersnaam in het formulier is *SecurityAuthority*\\*gebruikersnaam* (voorbeeld: corp\gebruiker1), wordt de *SecurityAuthority* deel is van de VM computernaam (voor de lokale beveiligingsautoriteit) of de naam van een Active Directory-domein.

Mogelijke oplossingen:

* Als het account zich lokaal op de virtuele machine, Controleer of de naam van de virtuele machine correct is gespeld.
* Als het account zich op een Active Directory-domein, Controleer de spelling van de domeinnaam.
* Als dit een Active Directory-domeinaccount is en de domeinnaam juist is gespeld, controleert u of dat een domeincontroller beschikbaar in dat domein is. Er is een veelvoorkomend probleem in Azure-netwerken met domeincontrollers dat een domeincontroller niet beschikbaar is omdat deze nog niet is gestart. Als tijdelijke oplossing, kunt u een lokale administrator-account in plaats van een domeinaccount.

<a id="wincred"></a>

## <a name="windows-security-error-your-credentials-did-not-work"></a>Fout in Windows-beveiliging: uw referenties niet werkt.
Oorzaak: De doel-VM kan niet worden gevalideerd uw accountnaam en wachtwoord.

Een Windows-computer kunt valideren van de referenties van een lokaal account of een domeinaccount.

* Voor lokale accounts, gebruikt u de *ComputerName*\\*gebruikersnaam* syntaxis (voorbeeld: SQL1\Admin4798).
* Voor domeinaccounts, gebruikt u de *DomainName*\\*gebruikersnaam* syntaxis (voorbeeld: CONTOSO\peterodman).

Als u uw virtuele machine naar een domeincontroller in een nieuw Active Directory-forest hebt gepromoveerd, wordt het lokale administrator-account waarmee u zich met aanmeldt geconverteerd naar een equivalente account met hetzelfde wachtwoord in het nieuwe forest en domein. Het lokale account wordt vervolgens verwijderd.

Bijvoorbeeld, als u aangemeld met het lokale account DC1\DCAdmin en vervolgens de virtuele machine gepromoveerd tot een domeincontroller in een nieuw forest voor het domein corp.contoso.com, het lokale account DC1\DCAdmin wordt verwijderd en wordt een nieuw domeinaccount (CORP\DCAdmin) met hetzelfde wachtwoord gemaakt.

Zorg ervoor dat de accountnaam is een naam die de virtuele machine kunt controleren of als een geldig account en dat het wachtwoord juist is.

Als u wijzigen van het wachtwoord van het lokale administrator-account wilt, Zie [opnieuw instellen van een wachtwoord of de extern bureaublad-service voor Windows-machines](reset-rdp.md).

<a id="rdpconnect"></a>

## <a name="this-computer-cant-connect-to-the-remote-computer"></a>Deze computer kan geen verbinding maken met de externe computer.
Oorzaak: Het account dat wordt gebruikt om verbinding te maken heeft geen extern bureaublad-aanmelden-rechten.

Elke Windows-computer is een lokale gebruikersgroep van de extern bureaublad, waarin de accounts en groepen die zich bij het op afstand aanmelden kunnen. Leden van de lokale groep administrators hebben ook toegang, zelfs als deze accounts niet in de lokale groep voor extern bureaublad-gebruikers weergegeven worden. De lokale groep administrators bevat ook de Domeinadministrators voor het domein voor domein machines.

Zorg ervoor dat het account dat u verbinding maken met extern bureaublad-aanmelden-rechten heeft. Als tijdelijke oplossing, gebruikt u een domein of lokale administrator-account verbinding maken via Extern bureaublad. Als u wilt het gewenste account toevoegt aan de lokale groep voor extern bureaublad-gebruikers, gebruikt u de module Microsoft Management Console (**Systeemwerkset > lokale gebruikers en groepen > groepen > Remote Desktop Users**).

## <a name="next-steps"></a>Volgende stappen
Als geen van deze fouten is opgetreden en u een onbekend probleem hebt met het maken van een verbinding met behulp van RDP, raadpleegt u de [probleemoplossingsgids voor extern bureaublad](troubleshoot-rdp-connection.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

* Zie voor stappen bij het openen van toepassingen die worden uitgevoerd op een virtuele machine voor probleemoplossing, [problemen oplossen met toegang tot een toepassing die wordt uitgevoerd op een Azure-VM](../linux/troubleshoot-app-connection.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).
* Als u problemen met Secure Shell (SSH) verbinding maken met een Linux-VM in Azure, Zie [oplossen SSH-verbindingen met een Linux-VM in Azure](../linux/troubleshoot-ssh-connection.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

