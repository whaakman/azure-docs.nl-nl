---
title: Azure Stack 1712 Update | Microsoft Docs
description: "Meer informatie over wat er in de update 1712 voor Azure-Stack geïntegreerd systemen, de bekende problemen en waar u de update te downloaden."
services: azure-stack
documentationcenter: 
author: brenduns
manager: femila
editor: 
ms.assetid: b14f79ad-025f-45d8-9e1d-e53d2b420bb1
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/11/2018
ms.author: brenduns
ms.openlocfilehash: 0456a202990d383370051d99112f829533b1b101
ms.sourcegitcommit: 562a537ed9b96c9116c504738414e5d8c0fd53b1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/12/2018
---
# <a name="azure-stack-1712-update"></a>Azure-Stack 1712 update

*Van toepassing op: Azure Stack geïntegreerd systemen*

Dit artikel worden de verbeteringen beschreven en wordt in dit updatepakket bekende problemen voor deze release en downloaden van de update worden opgelost. Bekende problemen zijn onderverdeeld in bekende problemen rechtstreeks verband houden met het updateproces en bekende problemen met de build (na de installatie).

> [!IMPORTANT]
> Dit updatepakket is alleen van toepassing op systemen met Azure-Stack geïntegreerd. Pas dit pakket niet naar de Azure-Stack Development Kit.

## <a name="build-reference"></a>Verwijzing bouwen

Het buildnummer van Azure Stack 1712 update **180106.1**. Als een klant is geïmplementeerd **180103.2** eerder, u niet wilt toepassen **180106.1**.

## <a name="before-you-begin"></a>Voordat u begint

> [!IMPORTANT]
> Probeer geen virtuele machines maken tijdens het installatieproces 1712 update. Zie [beheren van updates in het overzicht van Azure-Stack](https://docs.microsoft.com/azure/azure-stack/azure-stack-updates#plan-for-updates) voor meer informatie.

### <a name="prerequisites"></a>Vereisten

U moet eerst installeren met de Azure-Stack [1711 bijwerken](https://docs.microsoft.com/azure/azure-stack/azure-stack-update-1711) voordat deze update worden toegepast.

### <a name="post-update-steps"></a>Stappen na het bijwerken

Deze update is ook vereist voor firmware-updates te installeren van de OEM-partner na het voltooien van de installatie van de 1712 Azure Stack-update.

> [!NOTE]
> Raadpleeg uw OEM-partner-website om de updates te downloaden.

### <a name="new-features-and-fixes"></a>Nieuwe functies en oplossingen

Deze update bevat de volgende verbeteringen en oplossingen voor Azure-Stack.

#### <a name="new-features"></a>Nieuwe functies

- Test AzureStack cmdlet voor het valideren van de Azure-Stack Cloud beschikbaar via bevoorrechte-eindpunt
- Mogelijkheid om te registreren van een niet-verbonden implementatie van Azure-Stack
- Bewaking van waarschuwingen voor verlopen van certificaat en de gebruiker
- Toegevoegde Set BmcPassword cmdlet in PEP voor BMC wachtwoord rotatie
- Netwerk logboekregistratie updates voor de ondersteuning van logboekregistratie op aanvraag
- Ondersteuning voor terugzetten van de installatiekopie-bewerking voor de virtuele Machine kan worden geschaald Sets (VMSS)
- Kioskmodus op ERCS VM voor CloudAdmin aanmelding inschakelen
- Tenants kunnen Windows virtuele machines automatisch activeren

#### <a name="fixes"></a>Oplossingen

- Oplossing voor het knooppunt operationele Status tijdens het uitvoeren van herstel in onderhoud weergeven
- Herstellen naar juiste openbare IP-adres gebruik records datum-/ tijdstempel
- Verschillende andere prestaties, stabiliteit en oplossingen
- En Defender beschermde eindpunt module oplossingen voor problemen

#### <a name="windows-server-2016-new-features-and-fixes"></a>Windows Server 2016 nieuwe functies en oplossingen

- [Januari, 3e-2018: KB4056890 (OS-Build 14393.2007)](https://support.microsoft.com/help/4056890/windows-10-update-kb4056890)
    - Deze update bevat de software-oplossingen voor het beveiligingsprobleem voor een algemeen geaccepteerde beschreven door [MSRC Security Advisory ADV 180002](https://portal.msrc.microsoft.com/en-US/security-guidance/advisory/ADV180002).

### <a name="known-issues-with-the-update-process"></a>Bekende problemen met het updateproces kan controleren

Deze sectie vindt bekende problemen die tijdens de installatie van de 1712 update optreden kunnen.

1. **Symptoom:** Azure Stack operators mogelijk ziet u de volgende fout is opgetreden tijdens het updateproces: *'Type 'CheckHealth' rol 'Informatie' heeft een uitzondering: \n\nVirtual statuscontrole van de Machine voor - ACS01 geproduceerde de volgende fouten. \nThere is een fout opgetreden bij het VM-gegevens ophalen uit de hosts. Details van uitzondering: \nGet-VM: de bewerking op computer Node03 is mislukt: de WS-Management-service kan de aanvraag niet verwerken. De WMI-\nservice of de WMI-provider heeft een onbekende fout geretourneerd: HRESULT 0x8004106c. "*
    1. **Oorzaak:** dit probleem wordt veroorzaakt door een Windows Server-probleem dat is bedoeld om te worden opgelost in latere Window server-updates.
    2. **Oplossing:** Neem contact op met Microsoft Customer Service and Support (CSS) voor hulp.
<br><br>
2. **Symptoom:** Azure Stack operators mogelijk ziet u de volgende fout is opgetreden tijdens het updateproces:*' voor het inschakelen van de seed-ring VM is mislukt op knooppunt Host Node03 met een fout: [Host-Node03] verbinding maken met externe server Host Node03 is mislukt met de volgende foutbericht weergegeven: de WinRM-client heeft een foutstatus van de HTTP-server (500) ontvangen, maar de externe service bevat geen andere informatie over de oorzaak van de fout. "*
    1. **Oorzaak:** dit probleem wordt veroorzaakt door een Windows Server-probleem dat is bedoeld om te worden opgelost in latere Window server-updates. 
    2. **Oplossing:** Neem contact op met Microsoft Customer Service and Support (CSS) voor hulp.
<br><br>

### <a name="known-issues-post-installation"></a>Bekende problemen (na de installatie)

Deze sectie bevat na de installatie bekende problemen met build **180106.1**.

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
- Sommige marketplace-items zijn verwijderd in deze release vanwege compatibiliteit problemen. Deze worden opnieuw ingeschakeld na verdere validatie.
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

    > [!NOTE]
    > U moet geen gevolgen hebben voor uw bestaande SQL- of MySQL resource provider gebruikers bij het bijwerken van uw Azure-Stack geïntegreerde systemen naar de 1712-versie. U kunt blijven gebruiken van uw huidige SQL- of MySQL resource provider builds totdat een nieuwe Azure-Stack-update beschikbaar is.

#### <a name="app-service"></a>App Service
- Een gebruiker moet de storage resourceprovider registreren voordat ze hun eerste Azure-functie in het abonnement maken.

#### <a name="identity"></a>Identiteit

In Azure Active Directory Federation Services (ADFS) geïmplementeerd omgevingen, de **azurestack\azurestackadmin** account is niet langer de eigenaar van de standaard Provider-abonnement. In plaats van de aanmelding bij de **beheerportal / adminmanagement eindpunt** met de **azurestack\azurestackadmin**, kunt u de **azurestack\cloudadmin** account, dus u kunt beheren en gebruiken de standaard Provider-abonnement.

> [!IMPORTANT]
> Hoewel de **azurestack\cloudadmin** account is de eigenaar van het abonnement van de Provider standaard in omgevingen met ADFS geïmplementeerd, heeft geen machtigingen voor RDP in de host. Blijven gebruiken de **azurestack\azurestackadmin** account of het lokale administrator-account aanmelden, toegang tot en beheren van de host, indien nodig.

## <a name="download-the-update"></a>De update downloaden

U kunt het pakket met de Azure-Stack 1712 update downloaden [hier](https://aka.ms/azurestackupdatedownload).

## <a name="more-information"></a>Meer informatie

Microsoft is een manier om te controleren en updates met behulp van de bevoegde eindpunt (PEP) is geïnstalleerd met Update 1712 hervatten opgegeven.

- Zie de [in Azure-Stack met behulp van de bevoegde endpoint-documentatie-updates controleren](https://docs.microsoft.com/azure/azure-stack/azure-stack-monitor-update). 

## <a name="see-also"></a>Zie ook

- Zie [beheren van updates in het overzicht van Azure-Stack](azure-stack-updates.md) voor een overzicht van de update management in Azure-Stack.
- Zie [toepassen van updates in Azure-Stack](azure-stack-apply-updates.md) voor meer informatie over het toepassen van updates met de Azure-Stack.
