---
title: Azure Stack 1711 Update | Microsoft Docs
description: "Meer informatie over wat er in de update 1711 voor Azure-Stack geïntegreerd systemen, de bekende problemen en waar u de update te downloaden."
services: azure-stack
documentationcenter: 
author: andredm7
manager: femila
editor: 
ms.assetid: 2b66fe05-3655-4f1a-9b30-81bd64ba0013
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/11/2017
ms.author: andredm
ms.openlocfilehash: 578d17bcfbb7e12c9855132772c2068a5cdf1f62
ms.sourcegitcommit: 85012dbead7879f1f6c2965daa61302eb78bd366
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/02/2018
---
# <a name="azure-stack-1711-update"></a>Azure-Stack 1711 update

*Van toepassing op: Azure Stack geïntegreerd systemen*

Dit artikel worden de verbeteringen beschreven en wordt in dit updatepakket bekende problemen voor deze release en downloaden van de update worden opgelost. Bekende problemen zijn onderverdeeld in bekende problemen rechtstreeks verband houden met het updateproces en bekende problemen met de build (na de installatie).

> [!IMPORTANT]
> Dit updatepakket is alleen van toepassing op systemen met Azure-Stack geïntegreerd. Pas dit pakket niet naar de Azure-Stack Development Kit.

## <a name="build-reference"></a>Verwijzing bouwen

Het buildnummer van Azure Stack 1711 update **171201.3**.

## <a name="before-you-begin"></a>Voordat u begint

### <a name="prerequisites"></a>Vereisten

U moet eerst installeren met de Azure-Stack [1710 bijwerken](https://docs.microsoft.com/azure/azure-stack/azure-stack-update-1710) voordat deze update worden toegepast.

### <a name="new-features-and-fixes"></a>Nieuwe functies en oplossingen

Deze update bevat de volgende verbeteringen en oplossingen voor Azure-Stack.

#### <a name="new-features"></a>Nieuwe functies

- Ondersteuning voor Webfeedindeling oplossingssjablonen
- Updates in de Azure-Stack Graph-logboekregistratie en foutafhandeling
- Mogelijkheid om te schakelen van hosts in- of uitschakelen
- Gebruikers kunnen Windows VM's nu automatisch activeren
- Toegevoegde bevoorrechte eindpunt PowerShell-cmdlet voor het ophalen van BitLocker-herstelsleutels voor bewaren doeleinden
- Ondersteuning voor het bijwerken van de offline-installatiekopieën bij het bijwerken van de infrastructuur
- Infrastructuur voor back-up met de inschakelen Backup-Service inschakelen

#### <a name="fixes"></a>Oplossingen

- Vaste race condition in DNS tijdens veld FRU (replaceable unit) en ook bijgewerkte cluster logboekregistratie
- Oplossing voor opnieuw opstarten-mogelijkheden van disable-host in het veld FRU (replaceable unit)
- Verschillende andere prestaties, beveiliging en stabiliteit oplossingen

#### <a name="windows-server-2016-new-features-and-fixes"></a>Windows Server 2016 nieuwe functies en oplossingen

- [14 november 2017: KB4048953 (OS-Build 14393.1884)](https://support.microsoft.com/help/4048953)
 
### <a name="known-issues-with-the-update-process"></a>Bekende problemen met het updateproces kan controleren

Deze sectie vindt bekende problemen die tijdens de installatie van de 1711 update optreden kunnen.


1. **Symptoom:** Azure Stack operators mogelijk ziet u de volgende fout is opgetreden tijdens het updateproces: *'name: Installeer Update.', 'beschrijving': 'Update installeren op Hosts en virtuele machines Infra.', "errorMessage": "Typ 'LiveUpdate' van de rol ' Virtuele machines die een uitzondering: \n\nThere optreedt is niet genoeg ruimte op de disk.\n\nat <ScriptBlock>, <No file>: line22 ', 'status': 'Fout', 'startTimeUtc': ' 2017-11-10T16:46:59.123Z ', 'endTimeUtc': ' 2017-11-10T19:20:29.669Z ', 'stappen': [] '*
    2. **Oorzaak:** dit probleem wordt veroorzaakt door een gebrek aan schijfruimte vrij is op een of meer virtuele machines die deel van de Azure-Stack-infrastructuur uitmaken
    3. **Oplossing:** Neem contact op met Microsoft Customer Service and Support (CSS) voor hulp.
<br><br>
2. **Symptoom:** Azure Stack operators mogelijk ziet u de volgende fout is opgetreden tijdens het updateproces:*uitzondering 'ExtractToFile' aanroepen met "3" argument(en) bevatten: ' het proces heeft geen toegang tot het bestand ' <\\<machineName>-ERCS01\C$ \ Programma Files\WindowsPowerShell\Modules\Microsoft.AzureStack.Diagnostics\Microsoft.AzureStack.Common.Tools.Diagnostics.AzureStackDiagnostics.dll >'*
    1. **Oorzaak:** dit probleem wordt veroorzaakt wanneer het hervatten van een update van de portal die eerder is hervat met behulp van een bevoorrechte eindpunt (PEP).
    2. **Oplossing:** Neem contact op met Microsoft Customer Service and Support (CSS) voor hulp.
<br><br>
3. **Symptoom:**Azure Stack operators mogelijk ziet u de volgende fout is opgetreden tijdens het updateproces:*' Type 'CheckHealth' rol 'Informatie' Deze gebeurtenis treedt op een uitzondering: \n\nVirtual Machine Statuscontrole voor <machineName>-ACS01 geproduceerd de volgende fouten. \nThere, is een fout opgetreden bij het VM-gegevens ophalen uit de hosts. Details van uitzondering: \nGet-VM: de bewerking op computer Node03 is mislukt: de WS-Management-service kan de aanvraag niet verwerken. De WMI-\nservice of de WMI-provider heeft een onbekende fout geretourneerd: HRESULT 0x8004106c '.*
    1. **Oorzaak:** dit probleem wordt veroorzaakt door een Windows Server-probleem dat is bedoeld om te worden opgelost in latere Window server-updates.
    2. **Oplossing:** Neem contact op met Microsoft Customer Service and Support (CSS) voor hulp.
<br><br>
4. **Symptoom:**Azure Stack operators mogelijk ziet u de volgende fout is opgetreden tijdens het updateproces:*' Type 'DefenderUpdate' rol 'URP' heeft een uitzondering veroorzaakt: mislukte versie van \\SU1FileServer\SU1_Public\ DefenderUpdates\x64\{bestandsnaam} .exe na 60 pogingen tot kopie-AzSDefenderFiles, C:\Program Files\WindowsPowerShell\Modules\Microsoft.AzureStack.Defender\Microsoft.AzureStack.Defender.psm1: regel 262"*
    1. **Oorzaak:** dit probleem wordt veroorzaakt door een mislukte of onvolledige achtergrond downloaden van definitie-updates voor Windows Defender.
    2. **Oplossing:** Neem poging tot het hervatten van de update na acht uur zijn verstreken sinds de eerste update proberen.

### <a name="known-issues-post-installation"></a>Bekende problemen (na de installatie)

Deze sectie bevat na de installatie bekende problemen met build **20171201.3**.

#### <a name="portal"></a>Portal

- Het niet mogelijk om resources compute of opslag in de beheerdersportal weer te geven. Dit betekent dat er een fout tijdens de installatie van de update opgetreden en dat de update is niet juist gerapporteerd als geslaagd. Als dit probleem optreedt, neem voor assistentie contact op met Microsoft CSS.
- Mogelijk ziet u een lege dashboard in de portal. Als u wilt herstellen het dashboard, selecteert u het pictogram tandwielpictogram in de rechterbovenhoek van de portal en selecteer vervolgens **standaardinstellingen herstellen**.
- Wanneer u de eigenschappen van een resourcegroep weergeven de **verplaatsen** knop is uitgeschakeld. Dit is verwacht gedrag. Brongroepen verplaatsen tussen abonnementen wordt momenteel niet ondersteund.
- Voor elke werkstroom waarin u een abonnement, resourcegroep of locatie in een vervolgkeuzelijst selecteren, kan u een of meer van de volgende problemen optreden:

   - Mogelijk ziet u een lege rij boven aan de lijst. U moet nog steeds mogelijk een item selecteren zoals verwacht.
   - Als de lijst met items in de vervolgkeuzelijst kort is, kan het niet mogelijk om een van de namen van items weer te geven.
   - Als u meerdere gebruikersabonnementen hebt, kan de lijst met resources groep vervolgkeuzelijst leeg zijn. 

        > [!NOTE]
        > Om de problemen met de laatste twee omzeilen, kunt u de naam van het abonnement of resourcegroep (als u deze kent) typen of u kunt in plaats daarvan PowerShell gebruiken.

- Gebruiker abonnementen resulteert in een zwevende resources te verwijderen. Als tijdelijke oplossing Gebruikersbronnen of de hele resourcegroep eerst te verwijderen en verwijder vervolgens de gebruikersabonnementen.
- U bent geen kunnen machtigingen aan uw abonnement met behulp van de Azure-Stack-portals weergeven. Als tijdelijke oplossing kunt u machtigingen controleren met behulp van PowerShell.

#### <a name="health-and-monitoring"></a>Status en bewaking

- Als u een exemplaar van de rol infrastructuur opnieuw opstarten, verschijnt er een bericht weergegeven dat aangeeft dat het opnieuw opstarten is mislukt. Echter, het opnieuw opstarten daadwerkelijk is voltooid.

#### <a name="marketplace"></a>Marketplace
- Wanneer u probeert items toevoegen aan de Stack van Azure marketplace met behulp van de **toevoegen uit Azure** optie, niet alle items mogelijk niet zichtbaar voor downloaden.
- Gebruikers de volledige marketplace zonder een abonnement kunnen bladeren en beheeritems zoals plannen en aanbiedingen kunnen zien. Deze items zijn niet-functioneel voor gebruikers.

#### <a name="compute"></a>Compute
- Gebruikers krijgen de optie voor het maken van een virtuele machine met geografisch redundante opslag. Deze configuratie zorgt ervoor dat de virtuele machine wordt gemaakt mislukken.
- U kunt een virtuele machine beschikbaarheidsset alleen met een domein met fouten van een en een updatedomein van een configureren.
- Er is geen marketplace-ervaring voor het maken van virtuele-machineschaalsets. U kunt een schaal ingesteld met behulp van een sjabloon maken.
- Instellingen voor virtuele-machineschaalsets schalen zijn niet beschikbaar in de portal. Als tijdelijke oplossing kunt u [Azure PowerShell](https://docs.microsoft.com/azure/virtual-machine-scale-sets/virtual-machine-scale-sets-manage-powershell#change-the-capacity-of-a-scale-set). Vanwege verschillen tussen versies van PowerShell, moet u de `-Name` parameter in plaats van `-VMScaleSetName`.
 
#### <a name="networking"></a>Netwerken
- U kunt een load balancer met een openbaar IP-adres maken met behulp van de portal. Als tijdelijke oplossing kunt u PowerShell gebruiken voor het maken van de load balancer.
- Wanneer u een network load balancer maken, moet u een regel network address translation (NAT) maken. Als u dat niet doet, ontvangt u een fout wanneer u probeert een NAT-regel toevoegen nadat de load balancer is gemaakt.
- U kunt een openbare IP-adres van een virtuele machine (VM) kan niet loskoppelen nadat de virtuele machine is gemaakt en gekoppeld aan dat IP-adres. Disassociation wordt weergegeven om te werken, maar de eerder toegewezen openbaar IP-adres blijft gekoppeld aan de oorspronkelijke virtuele machine. Dit gebeurt zelfs als u een nieuwe virtuele machine het IP-adres toewijzen (gewoonlijk aangeduid als een *VIP's wisselen*). Alle toekomstige verbinding probeert te maken via dit resultaat van de IP-adres in van een verbinding met de oorspronkelijk gekoppelde virtuele machine en niet naar het nieuwe bericht. U moet nieuwe openbare IP-adressen op dit moment alleen gebruiken voor het maken van nieuwe virtuele machine.
- Azure Stack-operators kunnen mogelijk geen implementeren, verwijderen, wijzigen Vns of Netwerkbeveiligingsgroepen. Dit probleem is hoofdzakelijk zichtbaar op latere update pogingen van hetzelfde pakket. Dit wordt veroorzaakt door een probleem pakketten met een update die op dit moment onderzocht wordt.
- MAC-adressen verwerkt interne Load Balancing (ILB) niet goed voor back-end virtuele machines waarop Linux, alle exemplaren wordt verbroken.
 
#### <a name="sqlmysql"></a>SQL/MySQL
- Dit kan een uur duren voordat tenants databases in een nieuwe SQL- of MySQL SKU kunnen maken. 
- Maken van artikelen die rechtstreeks op de SQL- en MySQL die als host fungeert voor servers die niet worden uitgevoerd door de resourceprovider wordt niet ondersteund en kan leiden tot een niet-overeenkomende staat.
 
#### <a name="app-service"></a>App Service
- Een gebruiker moet de storage resourceprovider registreren voordat ze hun eerste Azure-functie in het abonnement maken.

#### <a name="identity"></a>Identiteit

In Azure Active Directory Federation Services (ADFS) geïmplementeerd omgevingen, de **azurestack\azurestackadmin** account is niet langer de eigenaar van de standaard Provider-abonnement. In plaats van de aanmelding bij de **beheerportal / adminmanagement eindpunt** met de **azurestack\azurestackadmin**, kunt u de **azurestack\cloudadmin** account, dus u kunt beheren en gebruiken de standaard Provider-abonnement.

> [!IMPORTANT]
> Hoewel de **azurestack\cloudadmin** account is de eigenaar van het abonnement van de Provider standaard in omgevingen met ADFS geïmplementeerd, heeft geen machtigingen voor RDP in de host. Blijven gebruiken de **azurestack\azurestackadmin** account of het lokale administrator-account aanmelden, toegang tot en beheren van de host, indien nodig.

#### <a name="infrastructure-backup-sevice"></a>Back-adreslijstservice infrastructuur
<!-- 1974890-->

- **Pre-1711 back-ups worden niet ondersteund voor herstel van de cloud.**  
  Pre-1711 back-ups zijn niet compatibel met het herstel van de cloud. U moet eerst bijwerken naar 1711 en inschakelen van de back-ups. Als u de back-ups al hebt ingeschakeld, zorg er dan voor dat een back-up uitvoeren na het bijwerken van 1711. Pre-1711 back-ups moeten worden verwijderd.

- **Inschakelen infrastructuur back-up op ASDK is alleen voor testdoeleinden.**  
  Back-ups infrastructuur kunnen worden gebruikt om te herstellen van oplossingen met meerdere knooppunten. Kunt u infrastructuur back-up op ASDK inschakelen, maar er is geen manier voor het testen van herstel.

Zie voor meer informatie [back-up en gegevensherstel voor Azure-Stack met de infrastructuur Backup-Service](C:\Git\MS\azure-docs-pr\articles\azure-stack\azure-stack-backup-infrastructure-backup.md).

## <a name="download-the-update"></a>De update downloaden

U kunt het pakket met de Azure-Stack 1711 update downloaden [hier](https://aka.ms/azurestackupdatedownload).

## <a name="more-information"></a>Meer informatie

Microsoft is een manier om te controleren en hervatten van updates met behulp van de bevoegde eindpunt (PEP) is geïnstalleerd met Update 1711 opgegeven.

- Zie de [in Azure-Stack met behulp van de bevoegde endpoint-documentatie-updates controleren](https://docs.microsoft.com/azure/azure-stack/azure-stack-monitor-update). 

## <a name="see-also"></a>Zie ook

- Zie [beheren van updates in het overzicht van Azure-Stack](azure-stack-updates.md) voor een overzicht van de update management in Azure-Stack.
- Zie [toepassen van updates in Azure-Stack](azure-stack-apply-updates.md) voor meer informatie over het toepassen van updates met de Azure-Stack.
