---
title: Specifieke foutberichten van RDP voor Azure Virtual machines | Microsoft Docs
description: Inzicht in specifieke foutberichten die kunnen worden weergegeven bij het gebruik van extern bureaublad-verbinding met een virtuele Windows-machine in Azure
keywords: Extern bureaublad-fout, verbinding met extern bureaublad-fout, kan geen verbinding met virtuele machine, probleemoplossing extern bureaublad
services: virtual-machines-windows
documentationcenter: 
author: genlin
manager: timlt
editor: 
tags: top-support-issue,azure-service-management,azure-resource-manager
ms.assetid: 5feb1d64-ee6f-4907-949a-a7cffcbc6153
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 11/03/2017
ms.author: genli
ms.openlocfilehash: 199f8c62b7aa3310bebd33c88edf57ea3bce804b
ms.sourcegitcommit: 3f33787645e890ff3b73c4b3a28d90d5f814e46c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/03/2018
---
# <a name="troubleshooting-specific-rdp-error-messages-to-a-windows-vm-in-azure"></a>Specifieke foutberichten van RDP voor een Windows-virtuele machine in Azure oplossen
Het foutbericht een specifiek foutbericht bij gebruik van extern bureaublad-verbinding met een Windows virtuele machine (VM) in Azure. In dit artikel vindt u details van de meestvoorkomende foutberichten aangetroffen, samen met stappen om op te lossen ze voor probleemoplossing. Als u verbinding maken met uw virtuele machine problemen ondervindt met RDP maar komen niet optreden van een specifiek foutbericht, raadpleegt u de [problemen oplossen met extern bureaublad](troubleshoot-rdp-connection.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

Voor informatie over de specifieke foutberichten, Zie de volgende:

* [De externe sessie is beëindigd omdat er geen extern bureaublad-licentieservers beschikbaar zijn voor het bieden van een licentie](#rdplicense).
* [Extern bureaublad computer 'name' is niet gevonden](#rdpname).
* [Er is een verificatiefout opgetreden. Local Security Authority is niet bereikbaar](#rdpauth).
* [Fout bij de Windows-beveiliging: uw referenties werken niet](#wincred).
* [Deze computer kan geen verbinding maken met de externe computer](#rdpconnect).

<a id="rdplicense"></a>

## <a name="the-remote-session-was-disconnected-because-there-are-no-remote-desktop-license-servers-available-to-provide-a-license"></a>De externe sessie is beëindigd omdat er geen extern bureaublad-licentieservers beschikbaar een licentie kunnen verlenen.
Oorzaak: De evaluatieperiode van 120 dagen licentieverlening voor de functie Extern bureaublad-Server is verlopen en moet u licenties installeren.

Als tijdelijke oplossing een lokale kopie van het RDP-bestand opslaan in de portal en voer deze opdracht uit bij een opdrachtprompt PowerShell verbinding maken. Deze stap wordt uitgeschakeld voor alleen die verbinding-licentieverlening:

        mstsc <File name>.RDP /admin

Als u niet meer dan twee gelijktijdige extern bureaublad-verbindingen met de virtuele machine daadwerkelijk nodig, kunt u Serverbeheer de functie Extern bureaublad-Server te verwijderen.

Zie voor meer informatie het blogbericht [Azure VM is mislukt met 'Geen extern bureaublad-licentieservers beschikbaar'](https://blogs.msdn.microsoft.com/mast/2014/01/21/rdp-to-azure-vm-fails-with-no-remote-desktop-license-servers-available/).

<a id="rdpname"></a>

## <a name="remote-desktop-cant-find-the-computer-name"></a>Extern bureaublad kan de computer 'name' niet vinden.
Oorzaak: De extern bureaublad-client op uw computer kan de naam van de computer in de instellingen van het RDP-bestand niet omzetten.

Mogelijke oplossingen:

* Als u op het intranet van een organisatie, zorg ervoor dat de computer toegang tot de proxyserver heeft en HTTPS-verkeer naar kunt verzenden.
* Als u een lokaal opgeslagen RDP-bestand gebruikt, kunt u die wordt gegenereerd door de portal. Deze stap zorgt ervoor dat u de juiste DNS-naam voor de virtuele machine, of de cloudservice en de poort van het eindpunt van de virtuele machine. Hier volgt een voorbeeld RDP-bestand die worden gegenereerd door de portal:
  
        full address:s:tailspin-azdatatier.cloudapp.net:55919
        prompt for credentials:i:1

Het gedeelte van dit RDP-bestand is:

* De volledig gekwalificeerde domeinnaam van de cloudservice waarin de virtuele machine ('tailspin-azdatatier.cloudapp.net' in dit voorbeeld).
* De externe TCP-poort van het eindpunt voor extern bureaublad-verkeer (55919).

<a id="rdpauth"></a>

## <a name="an-authentication-error-has-occurred-the-local-security-authority-cannot-be-contacted"></a>Er is een verificatiefout opgetreden. Local Security Authority is niet bereikbaar.
Oorzaak: Het doel-virtuele machine kan niet de certificeringsinstantie vinden in het gedeelte van de gebruiker de naam van uw referenties.

Wanneer uw gebruikersnaam in het formulier is *SecurityAuthority*\\*gebruikersnaam* (voorbeeld: corp\gebruiker1), wordt de *SecurityAuthority* gedeelte van de VM-computernaam (voor de lokale beveiligingsautoriteit) of de naam van een Active Directory-domein is.

Mogelijke oplossingen:

* Als het account zich lokaal op de virtuele machine, zorg dat de naam van de VM juist is gespeld.
* Als het account zich op een Active Directory-domein, Controleer de spelling van de domeinnaam.
* Als een Active Directory-domeinaccount en de domeinnaam juist is gespeld, Controleer of een domeincontroller beschikbaar is in het domein. Er is een veelvoorkomend probleem in Azure virtuele netwerken die een domeincontroller is niet beschikbaar omdat deze nog niet is gestart domeincontrollers bevatten. U kunt een lokaal beheerdersaccount in plaats van een domeinaccount gebruiken als tijdelijke oplossing.

<a id="wincred"></a>

## <a name="windows-security-error-your-credentials-did-not-work"></a>Fout bij de Windows-beveiliging: uw referenties werken niet.
Oorzaak: Het doel-virtuele machine kan niet worden gevalideerd uw accountnaam en wachtwoord.

Een Windows-computer kan de referenties van een lokaal account of een domeinaccount te valideren.

* Voor lokale accounts gebruiken de *ComputerName*\\*gebruikersnaam* syntaxis (voorbeeld: SQL1\Admin4798).
* Voor domeinaccounts, gebruikt u de *DomainName*\\*gebruikersnaam* syntaxis (voorbeeld: CONTOSO\peterodman).

Als u uw virtuele machine naar een domeincontroller in een nieuw Active Directory-forest hebt gepromoveerd, wordt het lokale beheerdersaccount dat u aangemeld geconverteerd naar een equivalente account met hetzelfde wachtwoord in het nieuwe forest en domein. Het lokale account wordt vervolgens verwijderd.

Als u bent aangemeld met het lokale account DC1\DCAdmin en vervolgens de virtuele machine gepromoveerd tot een domeincontroller in een nieuw forest voor het domein corp.contoso.com, bijvoorbeeld de DC1\DCAdmin lokale account wordt verwijderd en een nieuw domeinaccount (CORP\DCAdmin) wordt gemaakt met hetzelfde wachtwoord.

Zorg ervoor dat de accountnaam is een naam die de virtuele machine als een geldig account controleren kunt en het wachtwoord klopt.

Als u wijzigen van het wachtwoord van het lokale administrator-account wilt, Zie [opnieuw instellen van een wachtwoord of de service Extern bureaublad voor virtuele machines van Windows](reset-rdp.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

<a id="rdpconnect"></a>

## <a name="this-computer-cant-connect-to-the-remote-computer"></a>Deze computer kan geen verbinding maken met de externe computer.
Oorzaak: Het account dat wordt gebruikt om verbinding te beschikt niet over extern bureaublad aanmelden rechten.

Elke Windows-computer is een lokale gebruikersgroep van de extern bureaublad, waarin de accounts en groepen die op afstand in de App kunnen ondertekenen. Leden van de lokale groep administrators hebben toegang, zelfs als deze accounts niet worden weergegeven in de lokale groep voor extern bureaublad-gebruikers. De lokale groep administrators bevat ook de domeinbeheerders voor het domein voor domein-machines.

Zorg ervoor dat het account dat u verbinding maken met extern bureaublad-aanmelden-rechten heeft. Gebruik als tijdelijke oplossing een domein of lokale administrator-account verbinding kunnen maken via Extern bureaublad. Gebruikt u het gewenste account toevoegen aan de lokale groep voor extern bureaublad-gebruikers door de module Microsoft Management Console (**Systeemwerkset > lokale gebruikers en groepen > groepen > Externe bureaubladgebruikers**).

## <a name="next-steps"></a>Volgende stappen
Als geen van deze fouten is opgetreden en u een onbekende uitgeven hebt met verbinding maakt met behulp van RDP, Zie de [problemen oplossen met extern bureaublad](troubleshoot-rdp-connection.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

* Zie voor probleemoplossing bij het openen van toepassingen die worden uitgevoerd op een virtuele machine, [toegang tot een toepassing die wordt uitgevoerd op een virtuele machine in Azure oplossen](../linux/troubleshoot-app-connection.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).
* Als u problemen met behulp van Secure Shell (SSH ondervindt) als u wilt verbinding maken met een Linux-VM in Azure, Zie [oplossen SSH-verbindingen met een Linux VM in Azure](../linux/troubleshoot-ssh-connection.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

