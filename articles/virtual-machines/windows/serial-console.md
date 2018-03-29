---
title: De seriële console van virtuele machine van Azure | Microsoft Docs
description: Bidirectionele seriële console voor Azure virtual machines.
services: virtual-machines-windows
documentationcenter: ''
author: harijay
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure-services
ms.date: 03/05/2018
ms.author: harijay
ms.openlocfilehash: 9c16114b4f8d335dc750b1beef1fb6204ae20e0f
ms.sourcegitcommit: d74657d1926467210454f58970c45b2fd3ca088d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/28/2018
---
# <a name="virtual-machine-serial-console-preview"></a>Seriële console van de virtuele machine (preview) 


De seriële console van de virtuele machine in Azure biedt toegang tot een op tekst gebaseerde console voor Linux en Windows virtuele machines. Deze seriële verbinding naar de seriële poort COM1 van de virtuele machine is en biedt toegang tot de virtuele machine en niet zijn gerelateerd aan het virtuele machinenetwerk / systeemstatus besturingssysteem. Toegang tot de seriële console voor een virtuele machine worden uitgevoerd alleen via de Azure-portal momenteel en alleen voor gebruikers die VM Inzender hebben of hoger toegang tot de virtuele machine toegestaan. 

> [!Note] 
> Voorbeelden zijn beschikbaar gemaakt voor u op voorwaarde dat u akkoord met de gebruiksvoorwaarden gaat. Zie voor meer informatie [Microsoft Azure aanvullende gebruiksvoorwaarden voor Microsoft Azure-Previews.] (https://azure.microsoft.com/support/legal/preview-supplemental-terms/) Deze service is momenteel **openbare preview** en toegang tot de seriële console voor virtuele machines is beschikbaar voor globale Azure-regio's. Seriële console is op dit moment niet beschikbaar Azure Government, Duitse Azure en Azure voor China cloud.

 

## <a name="prerequisites"></a>Vereisten 

* Virtuele machine [opstarten diagnostics](boot-diagnostics.md) ingeschakeld 
* Het account met behulp van de seriële console moet hebben [rol van Inzender](../../active-directory/role-based-access-built-in-roles.md) voor de virtuele machine en de [opstarten diagnostics](boot-diagnostics.md) storage-account. 

## <a name="open-the-serial-console"></a>Open de seriële console
seriële console voor virtuele machines is alleen toegankelijk via [Azure-portal](https://portal.azure.com). Hieronder volgen de stappen voor toegang tot de seriële console voor virtuele machines via de portal 

  1. De Azure portal openen
  2. Selecteer de virtuele machines in het menu links.
  3. Klik op de virtuele machine in de lijst. De overzichtspagina van de virtuele machine wordt geopend.
  4. Schuif helemaal naar de ondersteuning + sectie Troubleshooting en klik op de optie voor de seriële console (Preview). Een nieuw deelvenster met de seriële console openen en de verbinding wordt gestart.

![](../media/virtual-machines-serial-console/virtual-machine-windows-serial-console-connect.gif)

### <a name="disable-feature"></a>Functie uitschakelen
De functionaliteit van de seriële console kan worden gedeactiveerd voor specifieke virtuele machines door die VM boot diagnostics instelling uit te schakelen.

## <a name="serial-console-security"></a>Beveiliging van de seriële console 

### <a name="access-security"></a>-Beveiliging 
Toegang tot de seriële console is beperkt tot gebruikers die hebben [VM inzenders](../../active-directory/role-based-access-built-in-roles.md#virtual-machine-contributor) of boven de toegang tot de virtuele machine. Als uw AAD-tenant is vereist voor multi-factor Authentication wordt toegang tot de seriële console ook de MFA moet omdat de toegang [Azure-portal](https://portal.azure.com).

### <a name="channel-security"></a>Kanaal-beveiliging
Alle gegevens wordt teruggestuurd en weer op de kabel wordt versleuteld.

### <a name="audit-logs"></a>Controlelogboeken
Alle toegang tot de seriële console momenteel is aangemeld de [opstarten diagnostics](https://docs.microsoft.com/azure/virtual-machines/linux/boot-diagnostics) logboeken van de virtuele machine. Toegang tot deze logboeken zijn het eigendom en beheerd door de beheerder van de virtuele machine van Azure.  

>[!CAUTION] 
Terwijl er geen wachtwoorden voor toegang voor de console bent aangemeld als opdrachten uitvoert in de console bevat of uitvoer van wachtwoorden, geheimen, gebruikersnamen of een andere vorm van persoonsgegevens (PII), worden die naar de virtuele machine opstarten diagnostische gegevens geschreven registreert, samen met alle andere zichtbare tekst, als onderdeel van de implementatie van de seriële console scrollback functionaliteit. Deze logboeken vormen een circulaire en alleen personen met leesmachtigingen voor het opslagaccount voor diagnostische gegevens hebben toegang tot deze, maar het is raadzaam na de aanbevolen procedure van het gebruik van de extern bureaublad voor alles die u mogelijk geheimen en/of PII. 

### <a name="concurrent-usage"></a>Gelijktijdig gebruik
Als een gebruiker is verbonden met de seriële console en een andere gebruiker is om die dezelfde virtuele machine toegang vraagt, de eerste gebruiker wordt verbroken en de tweede gebruiker aangesloten op een manier die lijkt op de eerste gebruiker permanent en het verlaten van de fysieke-console en een nieuwe de gebruiker zich omlaag.

>[!CAUTION] 
Dit betekent dat de gebruiker die wordt verbroken worden niet geregistreerd af! De mogelijkheid om af te dwingen een afmelden bij het verbreken van de verbinding (via SIGHUP of een vergelijkbaar mechanisme) is nog steeds in het overzicht. Voor Windows-er een time-out van automatische ingeschakeld in SAC is moet echter voor Linux kunt u configureren terminal time-outinstelling. 


## <a name="accessing-serial-console-for-windows"></a>Toegang tot de seriële console van voor Windows 
Nieuwere installatiekopieën van Windows Server op Azure hebt [speciale beheerconsole](https://technet.microsoft.com/library/cc787940(v=ws.10).aspx) (SAC) standaard ingeschakeld. SAC wordt ondersteund in serverversies van Windows, maar is niet beschikbaar op clientversies (bijvoorbeeld Windows 10, Windows 8 of Windows 7). Om in te schakelen voor Windows virtuele machines is gemaakt met het gebruik van Feb2018 of lager installatiekopieën van de seriële console neemt u de volgende stappen uit: 

1. Verbinding maken met uw Windows-machine via Extern bureaublad
2. Voer de volgende opdrachten uit een administratieve opdrachtprompt. 
* `bcdedit /ems {current} on`
* `bcdedit /emssettings EMSPORT:1 EMSBAUDRATE:115200`
3. Opnieuw opstarten van het systeem voor de console SAC zijn ingeschakeld

![](../media/virtual-machines-serial-console/virtual-machine-windows-serial-console-connect.gif)

Als de benodigde SAC kan offline worden ingeschakeld, evenals 

1. Koppel de windows-schijf die u wilt dat SAC geconfigureerd voor als een gegevensschijf met bestaande virtuele machine. 
2. Voer de volgende opdrachten uit een administratieve opdrachtprompt. 
* `bcdedit /store <mountedvolume>\boot\bcd /ems {default} on`
* `bcdedit /store <mountedvolume>\boot\bcd /emssettings EMSPORT:1 EMSBAUDRATE:115200`

### <a name="how-do-i-know-if-sac-is-enabled-or-not"></a>Hoe weet ik of SAC of niet is ingeschakeld 

Als [SAC] (https://technet.microsoft.com/library/cc787940(v=ws.10).aspx) is niet ingeschakeld de seriële console wordt niet weergegeven voor de SAC-prompt. Deze een VM-statusgegevens in sommige gevallen kunt weergeven of het normaal zou zijn leeg.  

### <a name="enabling-boot-menu-to-show-in-the-serial-console"></a>Opstartmenu om weer te geven in de seriële console inschakelen 

Als u wilt inschakelen, Windows-opstartlaadprogramma wordt u gevraagd om weer te geven in de seriële console dat kunt u de volgende aanvullende opties toevoegen aan Windows-opstartlaadprogramma.

1. Verbinding maken met uw Windows-machine via Extern bureaublad
2. Voer de volgende opdrachten uit een administratieve opdrachtprompt. 
* `bcdedit /set {bootmgr} displaybootmenu yes`
* `bcdedit /set {bootmgr} timeout 5`
* `bcdedit /set {bootmgr} bootems yes`
3. Opnieuw opstarten van het systeem voor het opstartmenu worden ingeschakeld

> [!NOTE] 
> Op dit punt ondersteuning voor de functie sleutels niet is ingeschakeld, als u geavanceerde opstartopties bcdedit/set {huidige} onetimeadvancedoptions gebruiken op vereist, Zie [bcdedit](https://docs.microsoft.com/windows-hardware/drivers/devtest/bcdedit--set) voor meer informatie

## <a name="common-scenarios-for-accessing-windows-serial-console"></a>Algemene scenario's voor toegang tot de seriële console van Windows 
Scenario          | Acties in de seriële console                
:------------------|:----------------------------------------
Onjuiste firewall-regels | Toegang tot de seriële console en los iptables of Windows firewall-regels 
Bestandssysteem beschadigd/selectievakje | Toegang tot de seriële console en het bestandssysteem herstellen na het aanmelden bij SAC CMD
Problemen met RDP-configuratie | Toegang tot de seriële console en meld u aan cmd-kanaal. Controleer de status van de Terminal services en opnieuw opstarten indien nodig.
Systeem voor het vergrendelen van netwerk| De seriële console toegang en meld u aan op cmd-kanaal. Controleer de status van de firewall door [netsh](https://docs.microsoft.com/windows-server/networking/technologies/netsh/netsh-contexts) vanaf de opdrachtregel. 

## <a name="errors"></a>Fouten
De meeste fouten zijn tijdelijke aard en adres verbinding deze opnieuw. Onderstaande tabel bevat een overzicht van de fouten en risicobeperking 

Fout                            |   Oplossing 
:---------------------------------|:--------------------------------------------|
Kan niet worden opgehaald van de opstartinstallatiekopie de diagnostische instellingen voor '<VMNAME>'. Zorg ervoor dat diagnostische gegevens voor opstarten is ingeschakeld voor deze virtuele machine voor het gebruik van de seriële console. | Zorg ervoor dat de virtuele machine heeft [opstarten diagnostics](boot-diagnostics.md) ingeschakeld. 
De virtuele machine is gestopt toewijzing ongedaan is gemaakt. Start de virtuele machine en probeer de seriële console-verbinding. | Virtuele machine moet zijn in een gestarte staat toegang tot de seriële console
U hebt niet de vereiste machtigingen voor het gebruik van de seriële console van deze virtuele machine. Controleer of er ten minste VM-rol bijdragerrechten.| Toegang tot de seriële console nodig bepaalde machtiging voor toegang tot. Zie [toegangsvereisten](#prerequisites) voor meer informatie
Kan niet bepalen van de resourcegroep voor het opslagaccount voor diagnostische gegevens van opstarten '<STORAGEACCOUNTNAME>'. Controleer of u diagnostische gegevens over opstarten is ingeschakeld voor deze virtuele machine en u toegang hebt tot dit opslagaccount. | Toegang tot de seriële console nodig bepaalde machtiging voor toegang tot. Zie [toegangsvereisten](#prerequisites) voor meer informatie

## <a name="known-issues"></a>Bekende problemen 
We zijn nog steeds in de preview-stadia voor toegang tot de seriële console, we werken via een aantal bekende problemen, onder de lijst is van deze voor mogelijke oplossingen 

Probleem                           |   Oplossing 
:---------------------------------|:--------------------------------------------|
Er is geen optie met de virtuele machine scale set exemplaar seriële console | Toegang tot de seriële console voor exemplaren van virtuele machine scale set wordt niet ondersteund op het moment van preview.
Roept invoeren nadat de banner verbinding wordt niet weergegeven voor een logboek in prompt | [Roept invoeren, gebeurt er niets](https://github.com/Microsoft/azserialconsole/blob/master/Known_Issues/Hitting_enter_does_nothing.md)
Alleen de statusinformatie wordt weergegeven bij het verbinden met een virtuele machine van Windows| [Windows Health signalen](https://github.com/Microsoft/azserialconsole/blob/master/Known_Issues/Windows_Health_Info.md)

## <a name="frequently-asked-questions"></a>Veelgestelde vragen 
**Q. Hoe kan ik feedback verzenden?**

A. Feedback geven als het probleem door te gaan naar https://aka.ms/serialconsolefeedback. U kunt ook minder (voorkeur) feedback verzenden via azserialhelp@microsoft.com of in de categorie van de virtuele machine van http://feedback.azure.com 

**Q. Ophalen van een fout 'de bestaande console heeft conflicterende BS-type 'Windows' met het gevraagde type besturingssysteem van Linux?**

A. Dit is een bekend probleem te verhelpen, opent [Azure Cloud Shell](https://docs.microsoft.com/azure/cloud-shell/overview) in bash-modus en probeer het opnieuw.

**Q. Ik kan geen toegang tot de seriële console, waar kan ik een ondersteuningsaanvraag bestand?**

A. Deze preview-functie wordt via Azure Preview-voorwaarden voldaan. Ondersteuning voor deze het beste oplossen via kanalen hierboven vermeld. 

## <a name="next-steps"></a>Volgende stappen
* De seriële console is ook beschikbaar voor [Linux](../linux/serial-console.md) virtuele machines
* Meer informatie over [bootdiagnostics](boot-diagnostics.md)
