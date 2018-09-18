---
title: Azure Stack 1807 Update | Microsoft Docs
description: Meer informatie over wat is er nieuw in de update 1807 voor geïntegreerde Azure Stack-systemen, met inbegrip van de bekende problemen en waar u de update te downloaden.
services: azure-stack
documentationcenter: ''
author: sethmanheim
manager: femila
editor: ''
ms.assetid: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/17/2018
ms.author: sethm
ms.reviewer: justini
ms.openlocfilehash: b46e365ea3903a8f2c805717df13d361c823148d
ms.sourcegitcommit: 776b450b73db66469cb63130c6cf9696f9152b6a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/18/2018
ms.locfileid: "45985593"
---
# <a name="azure-stack-1807-update"></a>Azure Stack 1807 update

*Is van toepassing op: Azure Stack-geïntegreerde systemen*

Dit artikel wordt de inhoud van het updatepakket 1807 beschreven. Deze update bevat verbeteringen, correcties en bekende problemen voor deze versie van Azure Stack en waar u de update te downloaden. Bekende problemen zijn onderverdeeld in problemen direct verband houden met het updateproces en problemen met de build (na de installatie).

> [!IMPORTANT]  
> Dit pakket is alleen voor geïntegreerde Azure Stack-systemen. Dit pakket niet van toepassing op de Azure Stack Development Kit.

## <a name="build-reference"></a>Naslaginformatie over bouwen

De Azure Stack 1807 update build-nummer **1.1807.0.76**.  

### <a name="new-features"></a>Nieuwe functies

Deze update bevat de volgende verbeteringen voor Azure Stack.

- <!-- 1658937 | ASDK, IS --> **Starten van back-ups volgens een vooraf gedefinieerd schema** -als een apparaat, Azure Stack kan nu automatisch activeren infrastructuur back-ups regelmatig om te voorkomen van menselijk handelen. Azure Stack wordt ook automatisch opschonen van de externe share voor back-ups die ouder dan de opgegeven bewaarperiode zijn. Zie voor meer informatie, [back-up inschakelen voor Azure Stack met PowerShell](azure-stack-backup-enable-backup-powershell.md).

- <!-- 2496385 | ASDK, IS -->  **Toegevoegde tijd in de totale back-uptijd voor gegevensoverdracht.** Zie voor meer informatie, [back-up inschakelen voor Azure Stack met PowerShell](azure-stack-backup-enable-backup-powershell.md).

-   <!-- 1702130 | ASDK, IS -->  **Back-up externe capaciteit wordt nu de juiste capaciteit van de externe share.** (Dit was eerder programmeren tot 10 GB.) Zie voor meer informatie, [back-up inschakelen voor Azure Stack met PowerShell](azure-stack-backup-enable-backup-powershell.md).

- <!-- 2508488 |  IS   -->  **Capaciteit** door [extra scale unit knooppunten toe te voegen](azure-stack-add-scale-node.md).

- <!-- 2753130 |  IS, ASDK   -->  **Azure Resource Manager-sjablonen bieden nu ondersteuning voor het element voorwaarde** -u kunt nu een resource in een Azure Resource Manager-sjabloon met behulp van een voorwaarde implementeren. U kunt de sjabloon voor het implementeren van een resource op basis van een voorwaarde, zoals het evalueren van als een parameterwaarde bevindt ontwerpen. Zie voor meer informatie over het gebruik van een sjabloon als een voorwaarde [een resource voorwaardelijk implementeren](https://docs.microsoft.com/azure/architecture/building-blocks/extending-templates/conditional-deploy) en [sectie met variabelen van Azure Resource Manager-sjablonen](https://docs.microsoft.com/azure/azure-resource-manager/resource-manager-templates-variables) in de documentatie van Azure. 

   U kunt ook sjablonen [resources implementeren op meer dan één abonnement of resourcegroep](https://docs.microsoft.com/azure/azure-resource-manager/resource-manager-cross-resource-group-deployment).  

- <!--2753073 | IS, ASDK -->  **Ondersteuning voor de versie van de Microsoft.COMPUTE API-resource is bijgewerkt** ondersteuning voor API-versie 2017-10-01, 2015-06-15 voor Azure Stack-netwerkbronnen.  Ondersteuning voor resource-versies tussen 10-01-2017 en 15-06-2015 is niet opgenomen in deze release, maar worden opgenomen in een toekomstige release.  Raadpleeg [overwegingen voor Azure Stack-netwerken](user/azure-stack-network-differences.md) voor functionaliteit verschillen.

- <!-- 2272116 | IS, ASDK   -->  **Azure Stack is ondersteuning toegevoegd voor omgekeerde DNS-zoekacties voor dergelijke infrastructuur-eindpunten voor Azure Stack** (dat is voor de portal, adminportal, beheer en adminmanagement). Hiermee kunt Azure Stack Extern eindpunt namen worden omgezet vanaf een IP-adres.

- <!-- 2780899 |  IS, ASDK   --> **Azure Stack biedt nu ondersteuning voor extra netwerkinterfaces toevoegen aan een bestaande virtuele machine.**  Deze functionaliteit is beschikbaar via de portal, PowerShell en CLI. Zie voor meer informatie, [toevoegen of verwijderen van netwerkinterfaces](https://docs.microsoft.com/azure/virtual-network/virtual-network-network-interface-vm) in de documentatie van Azure. 

- <!-- 2222444 | IS, ASDK   -->  **Verbeteringen in de nauwkeurigheid en zeer flexibel zijn aangebracht aan netwerken gebruik meters**.  Netwerk gebruik meters zijn nu meer nauwkeurige en houden rekening onderbroken abonnementen, perioden van de serviceonderbreking en racevoorwaarden.

- <!-- 2753080 | IS -->  **Beschikbare melding bijwerken.** Azure Stack-implementaties wordt nu periodiek controleren op een beveiligde eindpunt en bepalen of een update beschikbaar voor uw cloud is verbonden. Deze melding wordt weergegeven in de tegel Update wanneer deze na het handmatig controleren en importeren van een nieuwe update. Meer informatie over [beheren van updates voor Azure Stack](azure-stack-updates.md).

- <!-- 2297790 | IS, ASDK -->  **Verbeteringen in de Azure Stack-syslog-client (preview-functie)**. Deze client kunt het doorsturen van controle en logboeken met betrekking tot de Azure Stack-infrastructuur naar een syslog-server of security information en event management (SIEM) software buiten Azure Stack. De syslog-client ondersteunt nu het TCP-protocol met tekst zonder opmaak of TLS 1.2-versleuteling, de laatste is de standaardconfiguratie. U kunt de TLS-verbinding configureren met alleen-server of wederzijdse verificatie.

  Als u wilt configureren hoe de syslog-client communiceert (zoals protocol, versleuteling en -verificatie) met de syslog-server, gebruikt u de *Set SyslogServer* cmdlet. Deze cmdlet is beschikbaar via het eindpunt van de bevoegde (PEP).

  Als u wilt toevoegen de client-side-certificaat voor een wederzijdse verificatie van de syslog-client TLS 1.2, door de cmdlet Set-SyslogClient in de PEP te gebruiken.

  Met deze Preview-versie, kunt u een veel groter aantal controles en waarschuwingen zien. 

  Omdat deze functie nog in preview is, niet vertrouwen op deze in productie-omgevingen.

  Zie voor meer informatie, [Azure Stack syslog doorsturen](azure-stack-integrate-security.md).

<!-- - | ####### | IS, ASDK |  **Azure Resource Manager includes the region name.** With this release, objects retrieved from the Azure Resource Manager will now include the region name attribute. If an existing PowerShell script directly passes the object to another cmdlet, the script may produce an error and fail. This is Azure Resource Manager compliant behavior, and requires the calling client to subtract the region attribute. For more information about the Azure Resource Manager see [Azure Resource Manager Documentation](https://docs.microsoft.com/azure/azure-resource-manager/). verify 8-10 mdb -->

- <!-- TBD | IS, ASDK -->  **Wijzigingen in functionaliteit voor gedelegeerde Providers.** Beginnen met 1807 de gedelegeerde Providers model is vereenvoudigd om beter zijn afgestemd op het Azure-wederverkoper-model en gedelegeerde Providers niet mogelijk om te maken van andere gedelegeerde Providers, in feite plat maken van het model en het aanbrengen van de Provider overgedragen Deze functie is beschikbaar op één niveau. Zodat de overgang naar het nieuwe model en het beheer van de abonnementen kunnen nu de gebruiker-abonnementen worden verplaatst tussen nieuwe of bestaande overgedragen Provider-abonnementen die deel uitmaken van dezelfde Directory-tenant. Gebruiker-abonnementen die behoren tot het abonnement van de Provider standaard kunnen ook worden verplaatst naar de Providerabonnementen overgedragen in dezelfde Directory-tenant.  Zie voor meer informatie [aanbiedingen in Azure Stack delegeren](azure-stack-delegated-provider.md).

- <!-- 2536808 IS ASDK --> **Aanmaaktijd van de virtuele machine verbeterd** voor virtuele machines die zijn gemaakt met de installatiekopieën die u vanuit de Azure marketplace downloaden.

- <!-- TBD | IS, ASDK -->  **Verbeteringen in het Azure Stack Capacity Planner gebruik**. De Azure Stack [Capacity Planner](http://aka.ms/azstackcapacityplanner) biedt nu een vereenvoudigde ervaring voor het invoeren van S2D-cache en S2D capaciteit bij het definiëren van de oplossing SKU's. De limiet van 1000 virtuele machine is verwijderd.


### <a name="fixed-issues"></a>Problemen opgelost

- <!-- TBD | ASDK, IS --> Er zijn verschillende verbeteringen aangebracht in het updateproces zodat deze betrouwbaarder. Bovendien zijn correcties aangebracht aan onderliggende infrastructuur, die zo weinig mogelijk potentiële downtime voor workloads tijdens het bijwerken.

- <!--2292271 | ASDK, IS --> We een probleem opgelost waarbij een gewijzigde quotumlimiet is niet van toepassing op bestaande abonnementen. Nu, wanneer u een quotumlimiet voor een netwerkbron die deel uitmaakt van een aanbieding verhogen en Plan dat is gekoppeld aan een gebruikersabonnement, de nieuwe limiet van toepassing op de bestaande abonnementen, evenals de nieuwe abonnementen.

- <!-- 448955 | IS ASDK --> U kunt nu activiteitenlogboeken voor systemen die zijn geïmplementeerd in een tijdzone UTC + N is opvragen.    

- <!-- 2319627 |  ASDK, IS --> Controle vooraf voor de back-upconfiguratie parameters (pad/gebruikersnaam/wachtwoord/Encryption Key) wordt niet langer onjuiste instellingen ingesteld op de back-upconfiguratie. (Voorheen onjuiste instellingen zijn ingesteld in de back-up en back-up, klikt u vervolgens mislukken wanneer ze worden geactiveerd.)

- <!-- 2215948 |  ASDK, IS --> De lijst met back-up wordt nu vernieuwd wanneer u de back-up handmatig van de externe share verwijderen.

- <!-- 2332636 | IS -->  Update naar deze versie niet meer Hiermee stelt u de standaardeigenaar van het abonnement van de provider standaard op de ingebouwde **CloudAdmin** gebruiker bij de implementatie met AD FS.

- <!-- 2360715 |  ASDK, IS -->  Bij het instellen van datacenter-integratie, u geen toegang meer tot de AD FS-bestand met metagegevens van een Azure-bestandsshare. Zie voor meer informatie, [AD FS-integratie instellen door op te geven van bestand met federatieve metagegevens](azure-stack-integrate-identity.md#setting-up-ad-fs-integration-by-providing-federation-metadata-file). 

- <!-- 2388980 | ASDK, IS --> We een probleem opgelost dat gebruikers van een bestaande openbare IP-adres toegewezen die was eerder zijn toegewezen aan een netwerkinterface of Load Balancer met een nieuwe netwerkinterface of Load Balancer.  

- <!-- 2551834 - IS, ASDK --> Wanneer u selecteert *overzicht* voor een opslagaccount in de beheerder of de gebruiker-portals, de *Essentials* deelvenster nu correct wordt weergegeven de verwachte gegevens. 

- <!-- 2551834 - IS, ASDK --> Wanneer u selecteert *Tags* voor een opslagaccount in de beheerder of de gebruiker-portals, nu de gegevens correct wordt weergegeven.

- <!-- TBD - IS ASDK --> Deze versie van Azure Stack kunt u het probleem waardoor de toepassing van updates voor stuurprogramma's van OEM-extensiepakketten.

-   <!-- 2055809- IS ASDK --> We vastgesteld dat een probleem dat u niet het verwijderen van virtuele machines op de blade compute wanneer de virtuele machine kan niet worden gemaakt.  

- <!--  2643962 IS ASDK -->  De waarschuwing voor *geheugencapaciteit van lage* langer ten onrechte wordt weergegeven.

- **Verschillende oplossingen** voor prestaties, stabiliteit, beveiliging en het besturingssysteem dat wordt gebruikt door Azure Stack.


<!-- ### Additional releases timed with this update    -->

### <a name="common-vulnerabilities-and-exposures"></a>Common Vulnerabilities and Exposures
Azure Stack maakt gebruik van Server Core-installaties van Windows Server 2016 naar host sleutel-infrastructuur. Deze release wordt de volgende updates voor Windows Server 2016 geïnstalleerd op de infrastructuurservers voor Azure Stack: 
- [CVE-2018-8206](https://portal.msrc.microsoft.com/en-us/security-guidance/advisory/CVE-2018-8206)
- [CVE-2018-8222](https://portal.msrc.microsoft.com/en-us/security-guidance/advisory/CVE-2018-8222)
- [CVE-2018-8282](https://portal.msrc.microsoft.com/en-us/security-guidance/advisory/CVE-2018-8282)
- [CVE-2018-8304](https://portal.msrc.microsoft.com/en-us/security-guidance/advisory/CVE-2018-8304)
- [CVE-2018-8307](https://portal.msrc.microsoft.com/en-us/security-guidance/advisory/CVE-2018-8307)
- [CVE-2018-8308](https://portal.msrc.microsoft.com/en-us/security-guidance/advisory/CVE-2018-8308) 
- [CVE-2018-8309](https://portal.msrc.microsoft.com/en-us/security-guidance/advisory/CVE-2018-8309)
- [CVE-2018-8313](https://portal.msrc.microsoft.com/en-us/security-guidance/advisory/CVE-2018-8313)  

Klik op de hiervoor vermelde koppelingen voor meer informatie over deze beveiligingslekken of Zie Microsoft Knowledge Base-artikelen [4338814](https://support.microsoft.com/help/4338814) en [4345418](https://support.microsoft.com/help/4345418).



## <a name="before-you-begin"></a>Voordat u begint

### <a name="prerequisites"></a>Vereisten

- De Azure-Stack installeren [1805 bijwerken](azure-stack-update-1805.md) voordat u de Azure Stack 1807 update toepassen.  Er is geen update 1806.  

- Installeer de meest recente beschikbare [update of hotfix voor versie 1805](azure-stack-update-1805.md#post-update-steps).  
  > [!TIP]  
  > Abonneer u op de volgende *RRS* of *Atom* feeds, blijven van het Azure Stack Hotfixes:
  > - RRS: https://support.microsoft.com/app/content/api/content/feeds/sap/en-us/32d322a8-acae-202d-e9a9-7371dccf381b/rss ... 
  > - Atom: https://support.microsoft.com/app/content/api/content/feeds/sap/en-us/32d322a8-acae-202d-e9a9-7371dccf381b/atom ...


- Voordat u begint met de installatie van deze update, voert u [Test AzureStack](azure-stack-diagnostic-test.md) met de volgende parameters om te valideren van de status van uw Azure-Stack en los eventuele operationele problemen gevonden, met inbegrip van alle waarschuwingen en fouten. Ook actieve waarschuwingen bekijken en op te lossen die actie is vereist.  

  ```PowerShell
  Test-AzureStack -Include AzsControlPlane, AzsDefenderSummary, AzsHostingInfraSummary, AzsHostingInfraUtilization, AzsInfraCapacity, AzsInfraRoleSummary, AzsPortalAPISummary, AzsSFRoleSummary, AzsStampBMCSummary
  ``` 

### <a name="known-issues-with-the-update-process"></a>Bekende problemen met het updateproces

- <!-- 2468613 - IS --> Tijdens de installatie van deze update, ziet u mogelijk waarschuwingen met de titel *fout: sjabloon voor FaultType UserAccounts.New ontbreekt.*  U kunt deze waarschuwingen negeren. Deze waarschuwingen worden automatisch gesloten nadat de installatie van deze update is voltooid.

- <!-- 2489559 - IS --> Probeer niet voor het maken van virtuele machines tijdens de installatie van deze update. Zie voor meer informatie over het beheren van updates [-updates beheren in Azure Stack-overzicht](azure-stack-updates.md#plan-for-updates).

- <!-- 2830461 - IS --> In bepaalde omstandigheden wanneer een update uw aandacht vereist, kan de bijbehorende waarschuwing niet worden gegenereerd. De status van de nauwkeurige, nog steeds worden weergegeven in de portal en wordt niet negatief beïnvloed.

### <a name="post-update-steps"></a>Stappen na het bijwerken

- <!-- 2933866 – IS --> **Verbeterde status voor de mislukte update-installaties.** Deze versie introduceert twee categorieën in de nieuwe status voor Operators meer details over mislukte update-installaties. De twee categorieën zijn *PreparationFailed*, en *InstallationFailed*. Na de installatie van deze versie, ziet u mogelijk gegevens voor de vorige update-installatiefouten herzien om deze nieuwe categorieën weer te geven. 

<!-- *There are no post-update steps for update 1807.* -->

<!-- After the installation of this update, install any applicable Hotfixes. For more information view the following knowledge base articles, as well as our [Servicing Policy](azure-stack-servicing-policy.md).  
 - Link to KB  
 -->

## <a name="known-issues-post-installation"></a>Bekende problemen (na de installatie)

Hier volgen na de installatie bekende problemen voor deze buildversie.

### <a name="portal"></a>Portal

- De mogelijkheid om te [een nieuwe ondersteuningsaanvraag openen in de vervolgkeuzelijst](azure-stack-manage-portals.md#quick-access-to-help-and-support) binnen de beheerder van de portal is niet beschikbaar. Voor Azure Stack in plaats daarvan gebruik geïntegreerde systemen, de volgende koppeling: [ https://aka.ms/newsupportrequest ](https://aka.ms/newsupportrequest).

- <!-- 2931230 – IS  ASDK --> Abonnementen die zijn toegevoegd aan een gebruikersabonnement als een aanvullende plan kunnen niet worden verwijderd, zelfs wanneer u het abonnement uit het gebruikersabonnement verwijderen. Het abonnement blijft totdat de abonnementen die verwijzen naar het aanvullende plan worden ook verwijderd. 

- <!--2760466 – IS  ASDK --> Wanneer u een nieuwe Azure Stack-omgeving met deze versie installeert, de waarschuwing die aangeeft *activering vereist* mogelijk niet weergegeven. [Activering](azure-stack-registration.md) is vereist voordat u de marketplace-publicatie kunt gebruiken.  

- <!-- TBD - IS ASDK --> De twee administratieve abonnementstypen die waren [geïntroduceerd in versie 1804](azure-stack-update-1804.md#new-features) mag niet worden gebruikt. De abonnementstypen zijn **softwarelicentiecontrole abonnement**, en **verbruik abonnement**. Deze abonnementstypen zijn zichtbaar in de nieuwe Azure Stack-omgevingen vanaf versie 1804 maar nog niet klaar voor gebruik. U moet echter ook doorgaan met de **Provider standaard** abonnementstype.

- <!-- 2403291 - IS ASDK --> Hebt u mogelijk geen gebruik van de horizontale schuifbalk langs de onderkant van de beheerder en gebruiker portals. Als u geen toegang de horizontale schuifbalk tot, blijven gebruiken de breadcrumbs om naar een vorige blade in de portal navigeren zijn door de naam van de blade te selecteren dat u wilt weergeven in de breadcrumb-lijst gevonden aan de bovenkant van de portal.

  ![Breadcrumb](media/azure-stack-update-1804/breadcrumb.png)

- <!-- TBD - IS --> Het is mogelijk niet mogelijk om te berekenen of opslaan resources weergeven in de beheerdersportal. De oorzaak van dit probleem is een fout opgetreden tijdens de installatie van de update die ervoor zorgt dat de update worden niet correct gerapporteerd als geslaagd. Als dit probleem optreedt, moet u contact op met Microsoft Customer Support Services voor ondersteuning.

- <!-- TBD - IS --> U ziet een leeg dashboard in de portal. Als u wilt herstellen in het dashboard, selecteert u het tandwielpictogram in de rechterbovenhoek van de portal en selecteer vervolgens **standaardinstellingen herstellen**.

- <!-- TBD - IS ASDK --> Verwijderen van zwevende resources leidt van gebruiker-abonnementen. Als tijdelijke oplossing, eerst Gebruikersbronnen of de hele resourcegroep verwijderen en verwijder vervolgens gebruikersabonnementen.

- <!-- TBD - IS ASDK --> U kunt machtigingen aan uw abonnement met behulp van de Azure Stack-portals niet weergeven. Als tijdelijke oplossing, moet u PowerShell gebruiken om machtigingen te controleren.


### <a name="health-and-monitoring"></a>Status en bewaking
- <!-- 1264761 - IS ASDK -->  Mogelijk ziet u waarschuwingen voor de **Health controller** onderdeel waarvoor u de volgende gegevens:  

   Waarschuwing #1:
   - NAAM: De functie van de infrastructuur is niet in orde
   - ERNST: waarschuwing
   - ONDERDEEL: De gezondheid van controller
   - Beschrijving: De health-controller Heartbeat-Scanner is niet beschikbaar. Dit kan invloed hebben op statusrapporten en metrische gegevens.  

  Waarschuwing #2:
   - NAAM: De functie van de infrastructuur is niet in orde
   - ERNST: waarschuwing
   - ONDERDEEL: De gezondheid van controller
   - Beschrijving: De health-controller fouttolerantie Scanner is niet beschikbaar. Dit kan invloed hebben op statusrapporten en metrische gegevens.

  Beide waarschuwingen kunnen worden genegeerd en wordt automatisch gesloten na verloop van tijd.  


- <!-- 2812138 | IS --> U ziet mogelijk een waarschuwing voor **opslag** onderdeel waarvoor u de volgende gegevens:

   - NAAM: Storage service interne communicatiefout  
   - ERNST: kritiek  
   - COMPONENT: opslag  
   - Beschrijving: Storage service-interne communicatiefout is opgetreden bij het verzenden van aanvragen naar de volgende knooppunten.  

    De waarschuwing kan veilig worden genegeerd, maar moet u de waarschuwing handmatig sluit.

- <!-- 2368581 - IS. ASDK --> Azure Stack-operators, als u een waarschuwing voor een beperkte hoeveelheid geheugen ontvangt en virtuele machines van tenants niet te implementeren met een **fout bij het maken van infrastructuur-VM**, is het mogelijk dat de Azure Stack-stempel heeft te weinig geheugen beschikbaar. Gebruik de [Azure Stack Capacity Planner](https://gallery.technet.microsoft.com/Azure-Stack-Capacity-24ccd822) naar het beste informatie over de beschikbare capaciteit voor uw workloads.


### <a name="compute"></a>Compute

- <!-- 2724873 - IS --> Bij het gebruik van de PowerShell-cmdlets **Start AzsScaleUnitNode** of **Stop-AzsScaleunitNode** voor het beheren van schaaleenheden, de eerste poging om te starten of stoppen van de schaaleenheid kan mislukken. Als de cmdlet voor de eerste keer uitvoert mislukt, wordt de cmdlet nogmaals uitvoeren. De tweede uitvoering moet slagen om de bewerking te voltooien. 

- <!-- 2494144 - IS, ASDK --> Bij het selecteren van een VM-grootte voor de implementatie van een virtuele machine, sommige F-serie VM-grootten zijn niet zichtbaar als onderdeel van de grootte selector bij het maken van een virtuele machine. De volgende VM-grootten worden niet weergegeven in de kiezer: *F8s_v2*, *F16s_v2*, *F32s_v2*, en *F64s_v2*.  
  Als tijdelijke oplossing, gebruikt u een van de volgende methoden om een VM te implementeren. In elke methode moet u om op te geven van de VM-grootte die u wilt gebruiken.

  - **Azure Resource Manager-sjabloon:** wanneer u een sjabloon gebruikt, stelt de *vmSize* in de sjabloon zodat deze overeenkomt met de VM-grootte die u wilt gebruiken. Bijvoorbeeld, de volgende vermelding wordt gebruikt voor het implementeren van een virtuele machine die gebruikmaakt van de *F32s_v2* grootte:  

    ```
        "properties": {
        "hardwareProfile": {
                "vmSize": "Standard_F32s_v2"
        },
    ```  
  - **Azure CLI:** kunt u de [az vm maken](https://docs.microsoft.com/cli/azure/vm?view=azure-cli-latest#az-vm-create) opdracht en geeft u de VM-grootte als een parameter, die vergelijkbaar is met `--size "Standard_F32s_v2"`.

  - **PowerShell:** met PowerShell kunt u [New-AzureRMVMConfig](https://docs.microsoft.com/powershell/module/azurerm.compute/new-azurermvmconfig?view=azurermps-6.0.0) met de parameter die aangeeft van de VM-grootte, die vergelijkbaar is met `-VMSize "Standard_F32s_v2"`.


- <!-- TBD - IS ASDK --> Instellingen voor vergroten/verkleinen voor schaalsets voor virtuele machines zijn niet beschikbaar in de portal. Als tijdelijke oplossing, kunt u [Azure PowerShell](https://docs.microsoft.com/azure/virtual-machine-scale-sets/virtual-machine-scale-sets-manage-powershell#change-the-capacity-of-a-scale-set). Vanwege de verschillen tussen versies van PowerShell, moet u de `-Name` parameter in plaats van `-VMScaleSetName`.

- <!-- TBD - IS --> Bij het maken van een beschikbaarheidsset in de portal door te gaan naar **nieuw** > **Compute** > **beschikbaarheidsset**, kunt u alleen maken een beschikbaarheidsset met een foutdomein en updatedomein van 1. Als tijdelijke oplossing, bij het maken van een nieuwe virtuele machine, maak de beschikbaarheidsset met behulp van PowerShell, CLI, of rechtstreeks vanuit de portal.

- <!-- TBD - IS ASDK --> Wanneer u virtuele machines op de gebruikersportal van Azure Stack maakt, wordt een onjuist aantal gegevensschijven die aan een virtuele machine uit de DS-serie kunt koppelen door de portal weergegeven. DS-serie VM's kan zo veel gegevensschijven bevatten als de Azure-configuratie.

- <!-- TBD - IS ASDK --> Als een uitbreiding op een VM-implementatie de inrichting te lang duurt, laat gebruikers de time-out van de inrichting in plaats van bij stoppen van de procedure voor de toewijzing ongedaan of verwijder de virtuele machine.  

- <!-- 1662991 IS ASDK --> Diagnostische gegevens over Linux-VM wordt niet ondersteund in Azure Stack. Wanneer u een Linux-VM met VM diagnostics is ingeschakeld implementeren, wordt de implementatie mislukt. De implementatie mislukt ook als u de Linux-VM eenvoudige metrische gegevens via diagnostische instellingen inschakelen.  

- <!-- 2724961- IS ASDK --> Wanneer u registreert de **Microsoft.Insight** resourceprovider in instellingen voor abonnement, en een virtuele Windows-machine maken met gast OS diagnostische ingeschakeld, de overzichtspagina van de virtuele machine metrische gegevens niet weergeven. 

   Als u wilt zoeken metrische gegevens, zoals het diagram CPU-Percentage voor de virtuele machine, gaat u naar de **metrische gegevens** blade en het weergeven van alle ondersteunde Windows-VM-Gast metrische gegevens.

### <a name="networking"></a>Netwerken  

- <!-- 1766332 - IS ASDK --> Onder **netwerken**, als u klikt op **VPN-Gateway maken** voor het instellen van een VPN-verbinding, **op basis van beleid** wordt vermeld als een VPN-type. Selecteer deze optie niet. Alleen de **Route op basis van** optie wordt ondersteund in Azure Stack.

- <!-- 1902460 - IS ASDK --> Azure Stack biedt ondersteuning voor een enkel *lokale netwerkgateway* per IP-adres. Dit geldt voor alle tenant-abonnementen. Nadat het maken van de eerste lokale gateway netwerkverbinding, volgende pogingen tot het maken van een resource van een lokale netwerkgateway met hetzelfde IP-adres worden geblokkeerd.

- <!-- 16309153 - IS ASDK --> In een Virtueelnetwerk dat is gemaakt met een DNS-Server-instelling van *automatische*, wijzigen naar een aangepaste DNS-Server mislukt. De bijgewerkte instellingen worden niet gepusht naar virtuele machines in dit Vnet.

- <!-- 2702741 -  IS ASDK --> Openbare IP-adressen die zijn geïmplementeerd met behulp van de dynamische toewijzingsmethode zijn niet noodzakelijkerwijs te behouden nadat een Stop-toewijzing is uitgegeven.

- <!-- 2529607 - IS ASDK --> Tijdens de Azure Stack *geheim rotatie*, er is een periode waarin openbare IP-adressen niet bereikbaar voor twee tot vijf minuten zijn.

-   <!-- 2664148 - IS ASDK --> Kunnen optreden in scenario's waar de tenant er toegang hun virtuele machines tot heeft met behulp van een S2S-VPN-tunnel, een scenario waarbij verbindingspogingen mislukt als de on-premises-subnet is toegevoegd aan de lokale netwerkgateway nadat de gateway is al gemaakt. 


### <a name="sql-and-mysql"></a>SQL- en MySQL



- <!-- No Number - IS, ASDK -->  Speciale tekens, inclusief spaties en perioden, worden niet ondersteund in de **familie** naam wanneer u een SKU voor de SQL- en MySQL-resourceproviders. 

- <!-- TBD - IS --> Alleen de resourceprovider wordt ondersteund voor het maken van items op servers die SQL-host of MySQL. Items die zijn gemaakt op een host-server die niet zijn gemaakt door de resourceprovider kunnen leiden tot een niet-overeenkomende staat.  

> [!NOTE]  
> <!-- TBD - IS --> Nadat u hebt bijgewerkt naar deze versie van Azure Stack, kunt u echter ook doorgaan met de SQL- en MySQL-resourceproviders die u eerder hebt geïmplementeerd.  U wordt aangeraden dat u SQL- en MySQL bijwerken wanneer er een nieuwe versie beschikbaar. Net als Azure Stack, updates toepassen op SQL- en MySQL resourceproviders sequentieel worden verwerkt. Als u versie 1804, versie 1805 eerst toe te passen en vervolgens bijwerken naar 1807.  
>  
> De installatie van deze update heeft geen invloed op het huidige gebruik van SQL- of MySQL resourceproviders door uw gebruikers. 
> Ongeacht de versie van de resourceproviders die u gebruikt, de gegevens van uw gebruikers in hun databases is niet aangeraakt en toegankelijk blijft.  

### <a name="app-service"></a>App Service

- <!-- 2352906 - IS ASDK --> Gebruikers moeten de storage resourceprovider registreren voordat ze hun eerste Azure-functie in het abonnement maken.

- <!-- 2489178 - IS ASDK --> Als u wilt de infrastructuur (werknemers, beheer, front-rollen) uitbreiden, moet u PowerShell gebruiken, zoals beschreven in de opmerkingen bij de release voor Compute.

- <!-- TBD - IS ASDK --> App Service kan alleen worden geïmplementeerd in de *standaard providerabonnement* op dit moment. In een toekomstige update App Service wordt geïmplementeerd in de nieuwe *softwarelicentiecontrole abonnement* die is geïntroduceerd in Azure Stack 1804. Wanneer de meting wordt ondersteund voor gebruik, worden alle bestaande implementaties worden gemigreerd naar dit nieuwe abonnementstype.


### <a name="usage"></a>Gebruik  
- <!-- TBD - IS ASDK --> Gebruik openbare IP-adres-gebruiksgegevens meter toont dezelfde *datum/tijd evenement* waarde voor elke record in plaats van de *TimeDate* stempel waarin wordt weergegeven wanneer de record is gemaakt. U kunt deze gegevens op dit moment niet gebruiken om uit te voeren nauwkeurige accounting van gebruik van openbare IP-adres.


<!-- #### Identity -->
<!-- #### Marketplace -->


## <a name="download-the-update"></a>De update downloaden
U kunt de Azure Stack 1807 update-pakket van downloaden [hier](https://aka.ms/azurestackupdatedownload).


## <a name="next-steps"></a>Volgende stappen
- Het uitvoeren van onderhoud beleid voor geïntegreerde Azure Stack-systemen, en wat u moet doen om te voorkomen dat uw systeem in een ondersteunde status Zie [Azure Stack servicebeleid](azure-stack-servicing-policy.md).  
- Voor het gebruik van de bevoegde eindpunt (PEP) om te controleren en hervatten van updates, Zie [-updates in Azure Stack met behulp van het eindpunt van de bevoegde controleren](azure-stack-monitor-update.md).  
- Zie voor een overzicht van de update management in Azure Stack, [-updates beheren in Azure Stack-overzicht](azure-stack-updates.md).  
- Zie voor meer informatie over het toepassen van updates met Azure Stack [toepassen van updates in Azure Stack](azure-stack-apply-updates.md).  
