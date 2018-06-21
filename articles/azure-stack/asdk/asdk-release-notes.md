---
title: Opmerkingen bij de release van Microsoft Azure-Stack Development Kit | Microsoft Docs
description: Verbeteringen, correcties en bekende problemen voor Azure Stack Development Kit.
services: azure-stack
documentationcenter: ''
author: brenduns
manager: femila
ms.assetid: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/20/2018
ms.author: brenduns
ms.reviewer: misainat
ms.openlocfilehash: bbd9bb0d56dd61fd0a32531ac425a1dbc1aa8923
ms.sourcegitcommit: 1438b7549c2d9bc2ace6a0a3e460ad4206bad423
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/20/2018
ms.locfileid: "36295787"
---
# <a name="azure-stack-development-kit-release-notes"></a>Azure-Stack Development Kit release-opmerkingen  
Deze releaseopmerkingen bevatten informatie over verbeteringen, correcties en bekende problemen in Azure Stack Development Kit. Als u niet zeker weet welke versie u uitvoert, kunt u [de portal gebruiken om te controleren](.\.\azure-stack-updates.md#determine-the-current-version).

> De hoogte blijven van wat is er nieuw in de ASDK Abonneer u op de [ ![RSS](./media/asdk-release-notes/feed-icon-14x14.png)](https://docs.microsoft.com/api/search/rss?search=Azure+Stack+Development+Kit+release+notes&locale=en-us#) [feed](https://docs.microsoft.com/api/search/rss?search=Azure+Stack+Development+Kit+release+notes&locale=en-us#).

## <a name="build-11805147"></a>Build 1.1805.1.47

> [!TIP]  
> Op basis van feedback van klanten, wordt er een update van het schema van de versie voor Microsoft Azure-Stack in gebruik is. Beginnen met deze update 1805, het nieuwe schema beter vertegenwoordigt voor de huidige cloudversie.  
> 
> Het schema versie is nu *Version.YearYearMonthMonth.MinorVersion.BuildNumber* waar de tweede en derde sets geven de versie en release. Bijvoorbeeld: 1805.1 vertegenwoordigt de *release RTM* (RTM) versie van 1805.  


### <a name="new-features"></a>Nieuwe functies 
Deze versie bevat de volgende verbeteringen en oplossingen voor Azure-Stack.  

- <!-- 2297790 - IS, ASDK --> **Azure Stack bevat nu een *Syslog* client** als een *preview-functie*. Deze client kunt het doorsturen van controle en beveiliging logboeken die betrekking hebben op de Stack van Azure-infrastructuur naar een Syslog-server of beveiligingsgroep informatie en gebeurtenis management (SIEM) software die zich buiten de Azure-Stack. De Syslog-client ondersteunt momenteel alleen niet-geverifieerde UDP-verbindingen via de standaardpoort 514. De nettolading van elk Syslog-bericht is ingedeeld in algemene gebeurtenis-indeling (CEF). 

  Voor de configuratie van de Syslog-client gebruikt de **Set SyslogServer** cmdlet worden weergegeven in het bevoegde eindpunt. 

  Met dit voorbeeld ziet u mogelijk de volgende drie waarschuwingen. Wanneer door Azure Stack wordt ingediend, neemt u deze waarschuwingen *beschrijvingen* en *herstel* richtlijnen. 
  - TITEL: Code-integriteit uitschakelen  
  - TITEL: Code-integriteit in de controlemodus 
  - TITEL: Gebruikersaccount die is gemaakt

  Hoewel deze functie in de preview, moet deze niet worden aangehaald in productieomgevingen.   


### <a name="fixed-issues"></a>Opgeloste problemen
- We het probleem opgelost dat geblokkeerd [openen van een nieuw ondersteuningsverzoek uit de vervolgkeuzelijst](.\.\azure-stack-manage-portals.md#quick-access-to-help-and-support) uit in de beheerportal. Deze optie nu werkt zoals bedoeld. 

- **Verschillende oplossingen** voor prestaties, stabiliteit, beveiliging en het besturingssysteem dat wordt gebruikt door Azure Stack


<!-- ### Changes  --> 


<!--   ### Additional releases timed with this update  -->


### <a name="known-issues"></a>Bekende problemen
 
#### <a name="portal"></a>Portal
- <!-- 2551834 - IS, ASDK --> Wanneer u selecteert **overzicht** voor een opslagaccount in de beheerder of de gebruiker-portals, de gegevens van de *Essentials* deelvenster wordt niet weergegeven.  Het deelvenster Essentials geeft informatie weer over het account zoals de *resourcegroep*, *locatie*, en *abonnements-ID*.  Andere opties voor overzicht zijn toegankelijk zijn, zoals *Services* en *bewaking*, ook als opties voor *openen in Verkenner* of *opslagaccount verwijderen* .  

  Als u de informatie niet beschikbaar is, gebruikt u de [Get azureRMstorageaccount](https://docs.microsoft.com/powershell/module/azurerm.storage/get-azurermstorageaccount?view=azurermps-6.2.0) PowerShell-cmdlet. 

- <!-- 2551834 - IS, ASDK --> Wanneer u selecteert **labels** voor een opslagaccount in portals van de beheerder of de gebruiker de gegevens niet kan worden geladen en niet wordt weergegeven.  

  Als u de informatie niet beschikbaar is, gebruikt u de [Get-AzureRmTag](https://docs.microsoft.com/powershell/module/azurerm.tags/get-azurermtag?view=azurermps-6.2.0) PowerShell-cmdlet.

- <!-- TBD - IS ASDK --> Gebruik niet de nieuwe beheerdersrechten abonnement typen *softwarelicentiecontrole abonnement*, en *verbruik abonnement*. Deze nieuwe abonnement typen zijn geïntroduceerd in versie 1804 maar nog niet klaar voor gebruik. U moet blijven gebruiken de *Provider standaard* abonnementstype.  

- <!-- 2403291 - IS ASDK --> Hebt u mogelijk geen gebruik van de horizontale schuifbalk langs de onderkant van de portals beheerder en gebruiker. Als u geen toegang de horizontale schuifbalk tot, links gebruik de breadcrumbs om te navigeren naar een vorige blade in de portal voor u de naam van de blade selecteert u wilt weergeven in de lijst van de breadcrumb gevonden aan de bovenkant van de portal.
  ![Breadcrumb](media/asdk-release-notes/breadcrumb.png)

- <!-- TBD -  IS ASDK --> Gebruiker abonnementen resulteert in een zwevende resources te verwijderen. Als tijdelijke oplossing Gebruikersbronnen of de hele resourcegroep eerst te verwijderen en verwijder vervolgens de gebruikersabonnementen.

- <!-- TBD -  IS ASDK --> U kunt machtigingen aan uw abonnement met behulp van de portals Azure Stack niet weergeven. Als tijdelijke oplossing PowerShell te gebruiken om machtigingen te controleren.


#### <a name="health-and-monitoring"></a>Status en bewaking
- <!-- 1264761 - IS ASDK -->  Mogelijk ziet u waarschuwingen voor de *Health-domeincontroller* onderdeel dat u hebt de volgende details:  

   Waarschuwing #1:
   - NAAM: De functie van de infrastructuur is slecht
   - ERNST: waarschuwing
   - ONDERDEEL: Health-domeincontroller
   - Beschrijving: De health-domeincontroller Heartbeat-Scanner is niet beschikbaar. Dit kan beïnvloeden statusrapporten en metrische gegevens.  

  Waarschuwing #2:
   - NAAM: De functie van de infrastructuur is slecht
   - ERNST: waarschuwing
   - ONDERDEEL: Health-domeincontroller
   - Beschrijving: De health-domeincontroller veroorzaakt Scanner is niet beschikbaar. Dit kan beïnvloeden statusrapporten en metrische gegevens.

  Beide waarschuwingen kunnen worden genegeerd en wordt automatisch gesloten na verloop van tijd.  

- <!-- 2392907 – ASDK -->   U ziet mogelijk een *kritieke* waarschuwing voor **geheugencapaciteit geringe**. Deze waarschuwing heeft de volgende beschrijving: *de regio is meer dan 95,00% van het beschikbare geheugen gebruikt. Maken van virtuele machines met grote hoeveelheden geheugen kan mislukken.*

  Deze waarschuwing kan worden gegenereerd als Azure-Stack onjuist-accounts voor geheugengebruik op de Azure-Stack development kit.  

  U kunt deze waarschuwing negeren en het probleem heeft geen invloed op de plaatsing van virtuele machines. 

- <!-- 2368581 - IS. ASDK --> Een Azure-Stack-operator, als u een waarschuwing weinig geheugen ontvangt en virtuele machines van tenants niet te implementeren met een *fout bij het maken van de Fabric VM*, is het mogelijk dat de Azure-Stack stempel heeft te weinig geheugen beschikbaar. Gebruik de [Azure Stack capaciteit Planne](https://gallery.technet.microsoft.com/Azure-Stack-Capacity-24ccd822) beste leert de beschikbare capaciteit voor uw werkbelastingen. 


#### <a name="compute"></a>Compute
- <!-- TBD - IS, ASDK --> Als u de grootte van een virtuele machine voor de implementatie van een virtuele machine, sommige F-serie VM-grootten zijn niet zichtbaar als onderdeel van de selector grootte wanneer u een virtuele machine maken. De volgende VM-grootten worden niet weergegeven in de selector: *F8s_v2*, *F16s_v2*, *F32s_v2*, en *F64s_v2*.  
  Als tijdelijke oplossing een van de volgende methoden te gebruiken voor het implementeren van een virtuele machine. In elke methode moet u opgeven van de VM-grootte die u wilt gebruiken.

  - **Azure Resource Manager-sjabloon:** als u een sjabloon gebruikt, stelt de *vmSize* in de sjabloon zodat deze overeenkomt met de VM-grootte die u wilt gebruiken. Bijvoorbeeld de volgende vermelding wordt gebruikt voor het implementeren van een virtuele machine die gebruikmaakt van de *F32s_v2* grootte:  

    ```
        "properties": {
        "hardwareProfile": {
                "vmSize": "Standard_F32s_v2"
        },
    ```  
  - **Azure CLI:** kunt u de [az vm maken](https://docs.microsoft.com/cli/azure/vm?view=azure-cli-latest#az-vm-create) opdracht en de VM-grootte opgeven als een parameter, vergelijkbaar met `--size "Standard_F32s_v2"`.

  - **PowerShell:** met PowerShell kunt u [nieuw AzureRMVMConfig](https://docs.microsoft.com/powershell/module/azurerm.compute/new-azurermvmconfig?view=azurermps-6.0.0) met de parameter waarmee de VM-grootte, vergelijkbaar met `-VMSize "Standard_F32s_v2"`.


- <!-- TBD -  IS ASDK --> Instellingen voor virtuele-machineschaalsets schalen zijn niet beschikbaar in de portal. Als tijdelijke oplossing kunt u [Azure PowerShell](https://docs.microsoft.com/azure/virtual-machine-scale-sets/virtual-machine-scale-sets-manage-powershell#change-the-capacity-of-a-scale-set). Vanwege verschillen tussen versies van PowerShell, moet u de `-Name` parameter in plaats van `-VMScaleSetName`.

- <!-- TBD -  IS ASDK --> Wanneer u virtuele machines op de gebruikersportal van Azure-Stack maakt, wordt een onjuist aantal gegevensschijven dat een virtuele machine van de D-reeks kunt toevoegen in de portal weergegeven. Alle ondersteunde D-reeks VMs aankan zo veel gegevensschijven als de configuratie van de Azure.

- <!-- TBD -  IS ASDK --> Wanneer een installatiekopie van een virtuele machine niet kan worden gemaakt, kan een mislukte item dat u niet verwijderen, worden toegevoegd aan de VM-installatiekopieën compute-blade.

  Als tijdelijke oplossing door een nieuwe VM-installatiekopie te maken met een dummy VHD die kan worden gemaakt via de Hyper-V (New-VHD-pad C:\dummy.vhd-vaste - SizeBytes 1 GB). Dit proces los het probleem dat verhindert dat het mislukte item wordt verwijderd. Vervolgens kunt 15 minuten na het maken van de installatiekopie van het dummy, u met succes verwijderen.

  U kunt vervolgens redownload van de VM-installatiekopie die eerder is mislukt.

- <!-- TBD -  IS ASDK --> Als u een uitbreiding op de implementatie van een VM-inrichting te lang duurt, door gebruikers laten de inrichting time-outwaarde in plaats van bij het stoppen van het proces voor de toewijzing ongedaan maken of verwijderen van de virtuele machine.  

- <!-- 1662991 - IS ASDK --> Diagnostische gegevens van Linux-VM wordt niet ondersteund in Azure-Stack. Wanneer u een Linux-VM met diagnostische gegevens van virtuele machine is ingeschakeld implementeert, mislukt de implementatie. De implementatie mislukt ook als u de Linux-VM basismetrieken via de instellingen voor diagnostische inschakelt. 

#### <a name="networking"></a>Netwerken
- <!-- TBD - IS ASDK --> U kan geen gebruiker gedefinieerde routes maken in de portal-beheerder of gebruiker. Gebruik als tijdelijke oplossing [Azure PowerShell](https://docs.microsoft.com/azure/virtual-network/tutorial-create-route-table-powershell).

- <!-- 1766332 - IS, ASDK --> Onder **Networking**, als u op **VPN-Gateway maken** voor het instellen van een VPN-verbinding, **op basis van beleid** wordt vermeld als een VPN-type. Selecteer deze optie niet. Alleen de **Route gebaseerd** optie wordt ondersteund in Azure-Stack.

- <!-- 2388980 -  IS ASDK --> Nadat een virtuele machine is gemaakt en gekoppeld aan een openbaar IP-adres, kunt u die virtuele machine uit dat IP-adres kan niet loskoppelen. Disassociation lijkt te werken, maar de eerder toegewezen openbaar IP-adres blijft gekoppeld aan de oorspronkelijke virtuele machine.

  Op dit moment moet u alleen nieuwe openbare IP-adressen voor de nieuwe virtuele machines die u maakt.

  Dit gebeurt zelfs als u een nieuwe virtuele machine het IP-adres toewijzen (gewoonlijk aangeduid als een *VIP's wisselen*). Alle toekomstige verbinding probeert te maken via dit resultaat van de IP-adres in van een verbinding met de oorspronkelijke virtuele machine en niet naar het nieuwe bericht.


- <!-- 2292271 - IS ASDK --> Als u een quotumlimiet voor een netwerkbron die deel uitmaakt van een aanbieding en het Plan dat is gekoppeld aan een tenantabonnement verhoogt, wordt de nieuwe limiet niet toegepast op dat abonnement. De nieuwe limiet is echter van toepassing op nieuwe abonnementen die zijn gemaakt nadat het quotum wordt verhoogd. 

  Gebruik een invoegtoepassing plan quotum te verhogen een netwerk wanneer het abonnement al gekoppeld aan een abonnement is om dit probleem omzeilen. Zie voor meer informatie hoe [beschikbaar maken van een plan voor de invoegtoepassing](.\.\azure-stack-subscribe-plan-provision-vm.md#to-make-an-add-on-plan-available).

- <!-- 2304134 IS ASDK --> U kunt een abonnement met DNS-Zone-bronnen of routetabel resources zijn gekoppeld niet verwijderen. Om het abonnement is verwijderd, moet u eerst een DNS-Zone en routetabel resources verwijderen uit het tenantabonnement. 


- <!-- 1902460 -  IS ASDK --> Azure-Stack ondersteunt één *lokale netwerkgateway* per IP-adres. Dit geldt voor alle abonnementen van de tenant. Nadat het maken van de eerste lokale gateway netwerkverbinding, daaropvolgende pogingen tot het maken van een lokale gateway netwerkbron met hetzelfde IP-adres worden geblokkeerd.

- <!-- 16309153 -  IS ASDK --> Op een virtueel netwerk dat is gemaakt met een DNS-Server-instelling van *automatische*, wijzigen voor een aangepaste DNS-Server mislukt. De bijgewerkte instellingen zijn niet doorgevoerd in de virtuele machines in dit Vnet.
 
- <!-- TBD -  IS ASDK --> Azure-Stack biedt geen ondersteuning voor extra netwerkinterfaces toevoegen aan een VM-instantie nadat de virtuele machine is geïmplementeerd. Als de virtuele machine meer dan één netwerkinterface vereist, moeten ze tijdens de implementatie worden gedefinieerd.


#### <a name="sql-and-mysql"></a>SQL- en MySQL 
- <!-- TBD - ASDK --> De database die als host fungeert voor servers moet worden toegewezen voor gebruik door de resource provider en werkbelastingen. U kunt een exemplaar dat wordt gebruikt door andere verbruiker, met inbegrip van App-Services niet gebruiken.

- <!-- IS, ASDK --> Speciale tekens, inclusief spaties en perioden, worden niet ondersteund in de **familie** naam bij het maken van een SKU voor de resourceproviders SQL en MySQL. 

#### <a name="app-service"></a>App Service
- <!-- 2352906 - IS ASDK --> Gebruikers moeten de storage resourceprovider registreren voordat ze hun eerste Azure-functie in het abonnement maken.

- <!-- TBD - IS ASDK --> Om de infrastructuur (werknemers, beheer, front-rollen) uitbreiden, moet u PowerShell gebruiken, zoals beschreven in de release-opmerkingen voor Compute.  

- <!-- TBD - IS ASDK --> App Service kan alleen worden geïmplementeerd in de *standaard Provider-abonnement* op dit moment. In een toekomstige update App Service implementeert in de nieuwe *softwarelicentiecontrole abonnement* die is geïntroduceerd in Azure Stack 1804. Wanneer de meting wordt ondersteund voor gebruik, worden alle bestaande implementaties worden gemigreerd naar dit nieuwe abonnementstype.

#### <a name="usage"></a>Gebruik  
- <!-- TBD -  IS ASDK --> Gebruik van openbare IP-adres-gebruiksgegevens meter toont dezelfde *datum/tijd gebeurtenis* waarde voor elke record in plaats van de *TimeDate* tijdstempel die wordt aangegeven wanneer de record is gemaakt. U niet op dit moment kunnen deze gegevens gebruiken om uit te voeren nauwkeurige registratie van het gebruik van openbare IP-adres.

<!-- #### Identity -->



## <a name="build-201805131"></a>Build 20180513.1

### <a name="new-features"></a>Nieuwe functies 
Deze versie bevat de volgende verbeteringen en oplossingen voor Azure-Stack.  

- <!-- 1759172 - IS, ASDK --> **Nieuwe beheerdersrechten abonnementen**. Met 1804 zijn twee nieuwe typen van abonnementen beschikbaar in de portal. Deze nieuwe abonnement-typen worden naast de standaard Provider-abonnement en zichtbaar met de nieuwe Azure-Stack installaties vanaf versie 1804. *Deze nieuwe typen abonnementen niet gebruiken voor deze versie van Azure-Stack*. We zullen bekendmaken voor het gebruik van deze typen abonnement aan met een toekomstige update. 

  Deze nieuwe abonnement typen worden weergegeven maar kan deel uitmaken van een grotere wijziging voor het beveiligen van de standaard Provider-abonnement en gemakkelijker gedeelde bronnen, zoals het hosten van de SQL-servers implementeren. 

  De drie abonnement typen die nu beschikbaar zijn:  
  - Standaard Provider-abonnement: blijven gebruiken van dit abonnementstype. 
  - Softwarelicentiecontrole abonnement: *dit abonnementstype niet gebruiken.*
  - Abonnement voor verbruik: *gebruik geen dit abonnementstype*

### <a name="fixed-issues"></a>Opgeloste problemen
- <!-- IS, ASDK -->  In het beheerportal hoeft u niet langer de tegel Update vernieuwen voordat deze informatie wordt weergegeven. 

- <!-- 2050709 - IS, ASDK -->  U kunt nu het beheerportal opslag metrische gegevens voor Blob-service, service Table en Queue-service bewerken.

- <!-- IS, ASDK --> Onder **Networking**, wanneer u klikt op **verbinding** voor het instellen van een VPN-verbinding, **Site-naar-site (IPsec)** is nu de enige beschikbare optie. 

- **Verschillende oplossingen** voor prestaties, stabiliteit, beveiliging en het besturingssysteem dat wordt gebruikt door Azure Stack

<!-- ### Changes  --> 
### <a name="additional-releases-timed-with-this-update"></a>Aanvullende versies is een time-out opgetreden met deze update  
De volgende zijn nu beschikbaar, maar geen Azure-Stack update 1804 vereist.
- **Bijwerken naar de Microsoft Azure-Stack System Center Operations Manager-Bewakingspakket**. Een nieuwe versie (1.0.3.0) van de Microsoft System Center Operations Manager Monitoring Pack voor Azure-Stack is beschikbaar voor [downloaden](https://www.microsoft.com/download/details.aspx?id=55184). Met deze versie kunt u de Service-Principals wanneer u een gekoppelde Azure-Stack-implementatie toevoegen. Deze versie biedt ook een updatebeheer ervaring die kunt u de herstelactie rechtstreeks vanuit binnen Operations Manager. Er zijn ook nieuwe dashboards die resourceproviders weergeven, schaaleenheden en schalen eenheid knooppunten.

- **Nieuwe Azure stapelen Admin PowerShell-versie 1.3.0**.  Azure-Stack PowerShell 1.3.0 is nu beschikbaar voor installatie. Deze versie bevat opdrachten voor alle resourceproviders van beheerder voor het beheren van Azure-Stack.  Sommige inhoud in deze versie wordt afgeschaft vanuit de Azure-Stack extra GitHub [opslagplaats](https://github.com/Azure/AzureStack-Tools). 

   Ga als volgt voor details over de installatie, de [instructies](.\.\azure-stack-powershell-install.md) of de [help](https://docs.microsoft.com/powershell/azure/azure-stack/overview?view=azurestackps-1.3.0) voor Azure Stack Module 1.3.0 inhoud. 

- **Initiële release van Azure Rest Stack API-verwijzing**. De [API-referentiemateriaal voor alle Azure-Stack Admin resourceproviders](https://docs.microsoft.com/rest/api/azure-stack/) nu is gepubliceerd. 

### <a name="known-issues"></a>Bekende problemen
 
#### <a name="portal"></a>Portal
- <!-- TBD - IS ASDK --> De mogelijkheid [een nieuwe ondersteuningsaanvraag openen vanuit de vervolgkeuzelijst](.\.\azure-stack-manage-portals.md#quick-access-to-help-and-support) uit binnen de beheerder van de portal is niet beschikbaar. Gebruik in plaats daarvan de volgende koppeling:     
    - Gebruik voor Azure Stack Development Kit https://aka.ms/azurestackforum.    

- <!-- 2403291 - IS ASDK --> Hebt u mogelijk geen gebruik van de horizontale schuifbalk langs de onderkant van de portals beheerder en gebruiker. Als u geen toegang de horizontale schuifbalk tot, links gebruik de breadcrumbs om te navigeren naar een vorige blade in de portal voor u de naam van de blade selecteert u wilt weergeven in de lijst van de breadcrumb gevonden aan de bovenkant van de portal.
  ![Breadcrumb](media/asdk-release-notes/breadcrumb.png)

- <!-- TBD -  IS ASDK --> Gebruiker abonnementen resulteert in een zwevende resources te verwijderen. Als tijdelijke oplossing Gebruikersbronnen of de hele resourcegroep eerst te verwijderen en verwijder vervolgens de gebruikersabonnementen.

- <!-- TBD -  IS ASDK --> U kunt machtigingen aan uw abonnement met behulp van de portals Azure Stack niet weergeven. Als tijdelijke oplossing PowerShell te gebruiken om machtigingen te controleren.

-   <!-- TBD -  IS ASDK --> In het beheerportal ziet u mogelijk een kritieke waarschuwing voor het onderdeel Microsoft.Update.Admin. De naam van waarschuwing, beschrijving en herstel alle als weergegeven:  
    - *Fout - sjabloon voor FaultType ResourceProviderTimeout ontbreekt.*

    Deze waarschuwing kan worden genegeerd. 

#### <a name="health-and-monitoring"></a>Status en bewaking
- <!-- 1264761 - IS ASDK -->  Mogelijk ziet u waarschuwingen voor de *Health-domeincontroller* onderdeel dat u hebt de volgende details:  

   Waarschuwing #1:
   - NAAM: De functie van de infrastructuur is slecht
   - ERNST: waarschuwing
   - ONDERDEEL: Health-domeincontroller
   - Beschrijving: De health-domeincontroller Heartbeat-Scanner is niet beschikbaar. Dit kan beïnvloeden statusrapporten en metrische gegevens.  

  Waarschuwing #2:
   - NAAM: De functie van de infrastructuur is slecht
   - ERNST: waarschuwing
   - ONDERDEEL: Health-domeincontroller
   - Beschrijving: De health-domeincontroller veroorzaakt Scanner is niet beschikbaar. Dit kan beïnvloeden statusrapporten en metrische gegevens.

  Beide waarschuwingen kunnen worden genegeerd. Ze worden automatisch gesloten na verloop van tijd.  

#### <a name="marketplace"></a>Marketplace
- Gebruikers de volledige marketplace zonder een abonnement kunnen bladeren en beheeritems zoals plannen en aanbiedingen kunnen zien. Deze items zijn niet-functioneel voor gebruikers.
 
#### <a name="compute"></a>Compute
- <!-- TBD -  IS ASDK --> Instellingen voor virtuele-machineschaalsets schalen zijn niet beschikbaar in de portal. Als tijdelijke oplossing kunt u [Azure PowerShell](https://docs.microsoft.com/azure/virtual-machine-scale-sets/virtual-machine-scale-sets-manage-powershell#change-the-capacity-of-a-scale-set). Vanwege verschillen tussen versies van PowerShell, moet u de `-Name` parameter in plaats van `-VMScaleSetName`.

- <!-- TBD -  IS ASDK --> Wanneer u virtuele machines op de gebruikersportal van Azure-Stack maakt, wordt een onjuist aantal gegevensschijven dat aan een DS-serie VM koppelen kunt in de portal weergegeven. DS-serie VMs aankan zo veel gegevensschijven als de configuratie van de Azure.

- <!-- TBD -  IS ASDK --> Wanneer een installatiekopie van een virtuele machine niet kan worden gemaakt, kan een mislukte item dat u niet verwijderen, worden toegevoegd aan de VM-installatiekopieën compute-blade.

  Als tijdelijke oplossing door een nieuwe VM-installatiekopie te maken met een dummy VHD die kan worden gemaakt via de Hyper-V (New-VHD-pad C:\dummy.vhd-vaste - SizeBytes 1 GB). Dit proces los het probleem dat verhindert dat het mislukte item wordt verwijderd. Vervolgens kunt 15 minuten na het maken van de installatiekopie van het dummy, u met succes verwijderen.

  U kunt vervolgens redownload van de VM-installatiekopie die eerder is mislukt.

- <!-- TBD -  IS ASDK --> Als u een uitbreiding op de implementatie van een VM-inrichting te lang duurt, door gebruikers laten de inrichting time-outwaarde in plaats van bij het stoppen van het proces voor de toewijzing ongedaan maken of verwijderen van de virtuele machine.  

- <!-- 1662991 - IS ASDK --> Diagnostische gegevens van Linux-VM wordt niet ondersteund in Azure-Stack. Wanneer u een Linux-VM met diagnostische gegevens van virtuele machine is ingeschakeld implementeert, mislukt de implementatie. De implementatie mislukt ook als u de Linux-VM basismetrieken via de instellingen voor diagnostische inschakelt. 

#### <a name="networking"></a>Netwerken
- <!-- 1766332 - IS, ASDK --> Onder **Networking**, als u op **VPN-Gateway maken** voor het instellen van een VPN-verbinding, **op basis van beleid** wordt vermeld als een VPN-type. Selecteer deze optie niet. Alleen de **Route gebaseerd** optie wordt ondersteund in Azure-Stack.

- <!-- 2388980 -  IS ASDK --> Nadat een virtuele machine is gemaakt en gekoppeld aan een openbaar IP-adres, kunt u die virtuele machine uit dat IP-adres kan niet loskoppelen. Disassociation lijkt te werken, maar de eerder toegewezen openbaar IP-adres blijft gekoppeld aan de oorspronkelijke virtuele machine.

  Op dit moment moet u alleen nieuwe openbare IP-adressen voor de nieuwe virtuele machines die u maakt.

  Dit gebeurt zelfs als u een nieuwe virtuele machine het IP-adres toewijzen (gewoonlijk aangeduid als een *VIP's wisselen*). Alle toekomstige verbinding probeert te maken via dit resultaat van de IP-adres in van een verbinding met de oorspronkelijke virtuele machine en niet naar het nieuwe bericht.

- <!-- 2292271 - IS ASDK --> Als u een quotumlimiet voor een netwerkbron die deel uitmaakt van een aanbieding en het Plan dat is gekoppeld aan een tenantabonnement verhoogt, wordt de nieuwe limiet niet toegepast op dat abonnement. De nieuwe limiet is echter van toepassing op nieuwe abonnementen die zijn gemaakt nadat het quotum wordt verhoogd. 

  Gebruik een invoegtoepassing plan quotum te verhogen een netwerk wanneer het abonnement al gekoppeld aan een abonnement is om dit probleem omzeilen. Zie voor meer informatie hoe [beschikbaar maken van een plan voor de invoegtoepassing](.\.\azure-stack-subscribe-plan-provision-vm.md#to-make-an-add-on-plan-available).

- <!-- 2304134 IS ASDK --> U kunt een abonnement met DNS-Zone-bronnen of routetabel resources zijn gekoppeld niet verwijderen. Om het abonnement is verwijderd, moet u eerst een DNS-Zone en routetabel resources verwijderen uit het tenantabonnement. 


- <!-- 1902460 -  IS ASDK --> Azure-Stack ondersteunt één *lokale netwerkgateway* per IP-adres. Dit geldt voor alle abonnementen van de tenant. Nadat het maken van de eerste lokale gateway netwerkverbinding, daaropvolgende pogingen tot het maken van een lokale gateway netwerkbron met hetzelfde IP-adres worden geblokkeerd.

- <!-- 16309153 -  IS ASDK --> Op een virtueel netwerk dat is gemaakt met een DNS-Server-instelling van *automatische*, wijzigen voor een aangepaste DNS-Server mislukt. De bijgewerkte instellingen zijn niet doorgevoerd in de virtuele machines in dit Vnet.
 
- <!-- TBD -  IS ASDK --> Azure-Stack biedt geen ondersteuning voor extra netwerkinterfaces toevoegen aan een VM-instantie nadat de virtuele machine is geïmplementeerd. Als de virtuele machine meer dan één netwerkinterface vereist, moeten ze tijdens de implementatie worden gedefinieerd.


#### <a name="sql-and-mysql"></a>SQL- en MySQL 
- <!-- TBD - ASDK --> De database die als host fungeert voor servers moet worden toegewezen voor gebruik door de resource provider en werkbelastingen. U kunt een exemplaar dat wordt gebruikt door andere verbruiker, met inbegrip van App-Services niet gebruiken.

- <!-- IS, ASDK --> Speciale tekens, inclusief spaties en perioden, worden niet ondersteund in de **familie** naam bij het maken van een SKU voor de resourceproviders SQL en MySQL. 

#### <a name="app-service"></a>App Service
- <!-- TBD -  IS ASDK --> Gebruikers moeten de storage resourceprovider registreren voordat ze hun eerste Azure-functie in het abonnement maken.

- <!-- TBD -  IS ASDK --> Om de infrastructuur (werknemers, beheer, front-rollen) uitbreiden, moet u PowerShell gebruiken, zoals beschreven in de release-opmerkingen voor Compute.
 
#### <a name="usage"></a>Gebruik  
- <!-- TBD -  IS ASDK --> Gebruik van openbare IP-adres-gebruiksgegevens meter toont dezelfde *datum/tijd gebeurtenis* waarde voor elke record in plaats van de *TimeDate* tijdstempel die wordt aangegeven wanneer de record is gemaakt. U niet op dit moment kunnen deze gegevens gebruiken om uit te voeren nauwkeurige registratie van het gebruik van openbare IP-adres.

<!--
#### Identity
-->



#### <a name="downloading-azure-stack-tools-from-github"></a>Hulpprogramma's voor Azure Stack downloaden vanuit GitHub
- Wanneer u de *aanroepen webrequest* PowerShell-cmdlet voor het downloaden van de Azure-Stack van hulpprogramma's voor vanuit Github, er een foutbericht:     
    -  *aanroepen webrequest: de aanvraag is afgebroken: kan het beveiligde SSL/TLS-kanaal niet maken.*     

  Deze fout treedt op omdat een recente GitHub ondersteuning afschaffing van de cryptografische standaarden Tlsv1 en Tlsv1.1 (de standaardinstelling voor PowerShell). Zie voor meer informatie [zwakke cryptografische standaarden verwijdering kennisgeving](https://githubengineering.com/crypto-removal-notice/).

<!-- #### Identity -->



## <a name="build-201803021"></a>Build 20180302.1

### <a name="new-features-and-fixes"></a>Nieuwe functies en oplossingen
De nieuwe functies en oplossingen die zijn vrijgegeven voor Azure-Stack geïntegreerde systemen versie 1803 van toepassing op de Azure-Stack Development Kit. Zie de [nieuwe functies](.\.\azure-stack-update-1803.md#new-features) en [problemen opgelost](.\.\azure-stack-update-1803.md#fixed-issues) secties van de Azure-Stack 1803 bijwerken releaseopmerkingen voor meer informatie.  
> [!IMPORTANT]    
> Sommige van de items in de **nieuwe functies** en **problemen opgelost** secties alleen relevant zijn voor Azure-Stack geïntegreerd systemen.

### <a name="changes"></a>Wijzigingen
- De manier wijzigen van de status van een nieuwe aanbieding van *persoonlijke* naar *openbare* of *buiten gebruik gestelde* is gewijzigd. Zie voor meer informatie [maken van een aanbieding](.\.\azure-stack-create-offer.md). 


### <a name="known-issues"></a>Bekende problemen
 
#### <a name="portal"></a>Portal
- De mogelijkheid [een nieuwe ondersteuningsaanvraag openen vanuit de vervolgkeuzelijst](.\.\azure-stack-manage-portals.md#quick-access-to-help-and-support) uit binnen de beheerder van de portal is niet beschikbaar. Gebruik in plaats daarvan de volgende koppeling:     
    - Gebruik voor Azure Stack Development Kit https://aka.ms/azurestackforum.    

- <!-- 2050709 --> In het beheerportal is het niet mogelijk te bewerken opslag metrische gegevens voor Blob-service, service tabel of Queue-service. Wanneer u gaat u naar de opslag en selecteer vervolgens de blob, table of wachtrij service tegel klikt, wordt een nieuwe blade geopend met een grafiek metrische gegevens voor de service. Als u vervolgens bewerken vanaf de bovenkant van de metrische gegevens grafiek tegel selecteert, wordt de blade grafiek bewerken wordt geopend maar geen opties voor het bewerken van metrische gegevens weer.  

- U ziet een **activering vereist** waarschuwingsmelding dat aangeeft dat het registreren van uw Azure-Stack Development Kit noodzakelijk. Dit is verwacht gedrag.

- Gebruiker abonnementen resulteert in een zwevende resources te verwijderen. Als tijdelijke oplossing Gebruikersbronnen of de hele resourcegroep eerst te verwijderen en verwijder vervolgens de gebruikersabonnementen.

- U kunt machtigingen aan uw abonnement met behulp van de portals Azure Stack niet weergeven. Als tijdelijke oplossing PowerShell te gebruiken om machtigingen te controleren.

- In het dashboard van de beheerportal mislukt de Update-tegel om meer informatie over updates weer te geven. U lost dit probleem, klikt u op de tegel te vernieuwen.

-   In het beheerportal ziet u mogelijk een kritieke waarschuwing voor het onderdeel Microsoft.Update.Admin. De naam van waarschuwing, beschrijving en herstel alle als weergegeven:  
    - *Fout - sjabloon voor FaultType ResourceProviderTimeout ontbreekt.*

    Deze waarschuwing kan worden genegeerd. 

- In de beheerportal en gebruikersportal, de overzichtsblade niet kan worden geladen bij het selecteren van de blade overzicht van de storage-accounts die zijn gemaakt met een oudere versie van de API (voorbeeld: 2015-06-15). 

  Als tijdelijke oplossing kunt u PowerShell gebruiken om uit te voeren de **Start ResourceSynchronization.ps1** script toegang om de accountdetails van de opslag te herstellen. [Het script is beschikbaar via GitHub]( https://github.com/Azure/AzureStack-Tools/tree/master/Support/scripts), en moet worden uitgevoerd met service-beheerdersreferenties op de host van development kit als u de ASDK gebruikt.  

- De **servicestatus** blade niet kan worden geladen. Wanneer u de status van de Service-blade opent in de beheerder of de gebruiker-portal, Azure-Stack een fout wordt weergegeven en wordt de informatie niet geladen. Dit is verwacht gedrag. Hoewel u kunt selecteren en servicestatus opent, wordt deze functie is nog niet beschikbaar maar zullen worden uitgevoerd in een toekomstige versie van Azure-Stack.


#### <a name="health-and-monitoring"></a>Status en bewaking
- <!-- 1264761 - IS ASDK -->  Mogelijk ziet u waarschuwingen voor de *Health-domeincontroller* onderdeel dat u hebt de volgende details:  

   Waarschuwing #1:
   - NAAM: De functie van de infrastructuur is slecht
   - ERNST: waarschuwing
   - ONDERDEEL: Health-domeincontroller
   - Beschrijving: De health-domeincontroller Heartbeat-Scanner is niet beschikbaar. Dit kan beïnvloeden statusrapporten en metrische gegevens.  

  Waarschuwing #2:
   - NAAM: De functie van de infrastructuur is slecht
   - ERNST: waarschuwing
   - ONDERDEEL: Health-domeincontroller
   - Beschrijving: De health-domeincontroller veroorzaakt Scanner is niet beschikbaar. Dit kan beïnvloeden statusrapporten en metrische gegevens.

  Beide waarschuwingen kunnen worden genegeerd. Ze worden automatisch gesloten na verloop van tijd.  

- In de Stack van Azure-beheerportal, ziet u mogelijk een kritieke waarschuwing met de naam **in behandeling zijnde vervaldatum van het externe certificaat**.  Deze waarschuwing kan worden genegeerd en heeft invloed op de bewerkingen van de Azure-Stack Development Kit. 


#### <a name="marketplace"></a>Marketplace
- Gebruikers de volledige marketplace zonder een abonnement kunnen bladeren en beheeritems zoals plannen en aanbiedingen kunnen zien. Deze items zijn niet-functioneel voor gebruikers.
 
#### <a name="compute"></a>Compute
- Instellingen voor virtuele-machineschaalsets schalen zijn niet beschikbaar in de portal. Als tijdelijke oplossing kunt u [Azure PowerShell](https://docs.microsoft.com/azure/virtual-machine-scale-sets/virtual-machine-scale-sets-manage-powershell#change-the-capacity-of-a-scale-set). Vanwege verschillen tussen versies van PowerShell, moet u de `-Name` parameter in plaats van `-VMScaleSetName`.

- Wanneer u virtuele machines op de gebruikersportal van Azure-Stack maakt, wordt een onjuist aantal gegevensschijven dat aan een DS-serie VM koppelen kunt in de portal weergegeven. DS-serie VMs aankan zo veel gegevensschijven als de configuratie van de Azure.

- Wanneer een installatiekopie van een virtuele machine niet kan worden gemaakt, kan een mislukte item dat u niet verwijderen, worden toegevoegd aan de VM-installatiekopieën compute-blade.

  Als tijdelijke oplossing door een nieuwe VM-installatiekopie te maken met een dummy VHD die kan worden gemaakt via de Hyper-V (New-VHD-pad C:\dummy.vhd-vaste - SizeBytes 1 GB). Dit proces los het probleem dat verhindert dat het mislukte item wordt verwijderd. Vervolgens kunt 15 minuten na het maken van de installatiekopie van het dummy, u met succes verwijderen.

  U kunt vervolgens redownload van de VM-installatiekopie die eerder is mislukt.

-  Als u een uitbreiding op de implementatie van een VM-inrichting te lang duurt, door gebruikers laten de inrichting time-outwaarde in plaats van bij het stoppen van het proces voor de toewijzing ongedaan maken of verwijderen van de virtuele machine.  

- <!-- 1662991 --> Diagnostische gegevens van Linux-VM wordt niet ondersteund in Azure-Stack. Wanneer u een Linux-VM met diagnostische gegevens van virtuele machine is ingeschakeld implementeert, mislukt de implementatie. De implementatie mislukt ook als u de Linux-VM basismetrieken via de instellingen voor diagnostische inschakelt. 


#### <a name="networking"></a>Netwerken
- Onder **Networking**, als u op **verbinding** voor het instellen van een VPN-verbinding, **VNet-naar-VNet** wordt vermeld als een mogelijke verbindingstype. Selecteer deze optie niet. Op dit moment wordt alleen de **Site-naar-site (IPsec)** optie wordt ondersteund.

- Nadat een virtuele machine is gemaakt en gekoppeld aan een openbaar IP-adres, kunt u die virtuele machine uit dat IP-adres kan niet loskoppelen. Disassociation lijkt te werken, maar de eerder toegewezen openbaar IP-adres blijft gekoppeld aan de oorspronkelijke virtuele machine.

  Op dit moment moet u alleen nieuwe openbare IP-adressen voor de nieuwe virtuele machines die u maakt.

  Dit gebeurt zelfs als u een nieuwe virtuele machine het IP-adres toewijzen (gewoonlijk aangeduid als een *VIP's wisselen*). Alle toekomstige verbinding probeert te maken via dit resultaat van de IP-adres in van een verbinding met de oorspronkelijke virtuele machine en niet naar het nieuwe bericht.



- Azure-Stack ondersteunt één *lokale netwerkgateway* per IP-adres. Dit geldt voor alle abonnementen van de tenant. Nadat het maken van de eerste lokale gateway netwerkverbinding, daaropvolgende pogingen tot het maken van een lokale gateway netwerkbron met hetzelfde IP-adres worden geblokkeerd.

- Op een virtueel netwerk dat is gemaakt met een DNS-Server-instelling van *automatische*, wijzigen voor een aangepaste DNS-Server mislukt. De bijgewerkte instellingen zijn niet doorgevoerd in de virtuele machines in dit Vnet.
 
- Azure-Stack biedt geen ondersteuning voor extra netwerkinterfaces toevoegen aan een VM-instantie nadat de virtuele machine is geïmplementeerd. Als de virtuele machine meer dan één netwerkinterface vereist, moeten ze tijdens de implementatie worden gedefinieerd.



#### <a name="sql-and-mysql"></a>SQL- en MySQL 
- Het kan maximaal één uur voordat gebruikers databases in een nieuwe SQL- of MySQL SKU maken kunnen duren.

- De database die als host fungeert voor servers moet worden toegewezen voor gebruik door de resource provider en werkbelastingen. U kunt een exemplaar dat wordt gebruikt door andere verbruiker, met inbegrip van App-Services niet gebruiken.

- <!-- IS, ASDK --> Speciale tekens, inclusief spaties en perioden, worden niet ondersteund in de **familie** of **laag** namen bij het maken van een SKU voor de resourceproviders SQL en MySQL.

#### <a name="app-service"></a>App Service
- Gebruikers moeten de storage resourceprovider registreren voordat ze hun eerste Azure-functie in het abonnement maken.

- Om de infrastructuur (werknemers, beheer, front-rollen) uitbreiden, moet u PowerShell gebruiken, zoals beschreven in de release-opmerkingen voor Compute.
 
#### <a name="usage"></a>Gebruik  
- Gebruik van openbare IP-adres-gebruiksgegevens meter toont dezelfde *datum/tijd gebeurtenis* waarde voor elke record in plaats van de *TimeDate* tijdstempel die wordt aangegeven wanneer de record is gemaakt. U niet op dit moment kunnen deze gegevens gebruiken om uit te voeren nauwkeurige registratie van het gebruik van openbare IP-adres.
<!--
#### Identity
-->

#### <a name="downloading-azure-stack-tools-from-github"></a>Hulpprogramma's voor Azure Stack downloaden vanuit GitHub
- Wanneer u de *aanroepen webrequest* PowerShell-cmdlet voor het downloaden van de Azure-Stack van hulpprogramma's voor vanuit Github, er een foutbericht:     
    -  *aanroepen webrequest: de aanvraag is afgebroken: kan het beveiligde SSL/TLS-kanaal niet maken.*     

  Deze fout treedt op omdat een recente GitHub ondersteuning afschaffing van de cryptografische standaarden Tlsv1 en Tlsv1.1 (de standaardinstelling voor PowerShell). Zie voor meer informatie [zwakke cryptografische standaarden verwijdering kennisgeving](https://githubengineering.com/crypto-removal-notice/).

  U lost dit probleem, toevoegen `[Net.ServicePointManager]::SecurityProtocol = [Net.SecurityProtocolType]::Tls12` boven aan het script om af te dwingen de PowerShell-console TLSv1.2 gebruiken bij het downloaden van de GitHub-opslagplaatsen.





