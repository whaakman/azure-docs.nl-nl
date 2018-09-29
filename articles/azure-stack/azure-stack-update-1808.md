---
title: Azure Stack 1808 Update | Microsoft Docs
description: Meer informatie over wat is er nieuw in de update 1808 voor geïntegreerde Azure Stack-systemen, met inbegrip van de bekende problemen en waar u de update te downloaden.
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
ms.date: 09/28/2018
ms.author: sethm
ms.reviewer: justini
ms.openlocfilehash: 7278f784316fdbd7170bb69b9414911b8f5b3823
ms.sourcegitcommit: f31bfb398430ed7d66a85c7ca1f1cc9943656678
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/28/2018
ms.locfileid: "47452680"
---
# <a name="azure-stack-1808-update"></a>Azure Stack 1808 update

*Is van toepassing op: Azure Stack-geïntegreerde systemen*

Dit artikel wordt de inhoud van het updatepakket 1808 beschreven. Het updatepakket bevat verbeteringen, problemen en bekende problemen voor deze versie van Azure Stack. In dit artikel bevat ook een koppeling, zodat u kunt de update downloaden. Bekende problemen zijn onderverdeeld in problemen direct verband houden met het updateproces en problemen met de build (na de installatie).

> [!IMPORTANT]  
> Dit pakket is alleen voor geïntegreerde Azure Stack-systemen. Dit pakket niet van toepassing op de Azure Stack Development Kit.

## <a name="build-reference"></a>Naslaginformatie over bouwen

De Azure Stack 1808 update build-nummer **1.1808.0.97**.  

### <a name="new-features"></a>Nieuwe functies

Deze update bevat de volgende verbeteringen voor Azure Stack.

- <!--  2682594   | IS  -->  **Alle Azure Stack-omgevingen gebruik nu de indeling van de tijdzone Coordinated Universal Time (UTC).**  Alle gegevens vastleggen en gerelateerde gegevens nu wordt weergegeven in UTC-notatie. Als u een van een vorige versie die niet is geïnstalleerd met behulp van UTC update, wordt uw omgeving is bijgewerkt voor gebruik van UTC. 

- <!-- 2437250  | IS  ASDK --> **Beheerde schijven worden ondersteund.** U kunt nu beheerde schijven gebruiken in Azure Stack virtuele machines en virtuele-machineschaalsets. Zie voor meer informatie, [Azure Stack Managed Disks: verschillen en overwegingen](/azure/azure-stack/user/azure-stack-managed-disk-considerations).

- <!-- 2563799  | IS  ASDK -->  **Azure Monitor**. Zoals Azure Monitor op Azure biedt Azure Monitor in Azure Stack-infrastructuur op basisniveau metrische gegevens en logboeken voor de meeste services. Zie voor meer informatie, [Azure Monitor in Azure Stack](/azure/azure-stack/user/azure-stack-metrics-azure-data).

- <!-- 2487932| IS -->  **Voorbereiden voor de host van de extensie**. De host van de extensie kunt u helpen bij het beveiligde Azure Stack door het aantal vereiste TCP/IP-poorten te verminderen. U kunt met de update 1808 voorbereiden, bereid u voor uw Azure Stack voor extensie-host. Zie voor meer informatie, [voorbereiden voor de host van de extensie voor Azure Stack](/azure/azure-stack/azure-stack-extension-host-prepare).

- <!-- IS --> **Galerij-items voor Virtual Machine Scale Sets zijn nu ingebouwde**.  Het galerie-item voor virtuele-Machineschaalset is nu beschikbaar in de gebruiker en beheerder portals gemaakt zonder deze te downloaden.  Als u een naar 1808 upgrade is beschikbaar na voltooiing van de upgrade.  

- <!-- IS, ASDK --> **Virtuele-Machineschaalset schalen**.  U kunt de portal gebruiken om [schaal van een virtuele-Machineschaalset](azure-stack-compute-add-scalesets.md#scale-a-virtual-machine-scale-set) (VMSS).    

- <!-- 2489570 | IS ASDK--> **Ondersteuning voor aangepaste configuraties voor beleid voor IPSec/IKE** voor [VPN-gateways in Azure Stack](/azure/azure-stack/azure-stack-vpn-gateway-about-vpn-gateways).

- <!-- | IS ASDK--> **Marketplace-item voor Kubernetes**. U kunt nu met behulp van Kubernetes-clusters implementeren de [Kubernetes Marketplace-item](azure-stack-solution-template-kubernetes-cluster-add.md). Gebruikers kunnen selecteert u het Kubernetes-item en vul een aantal parameters voor het implementeren van een Kubernetes-cluster in Azure Stack. Het doel van de sjablonen is het eenvoudig te voor gebruikers voor setup dev/test Kubernetes-implementaties in een paar stappen.

- <!-- | IS ASDK--> **Blockchain sjablonen**. U kunt nu uitvoeren [Ethereum consortium implementaties](azure-stack-ethereum.md) in Azure Stack. Vindt u drie nieuwe sjablonen in de [Azure Stack Quick Start-sjablonen](https://github.com/Azure/AzureStack-QuickStart-Templates). Hiermee kunt de gebruiker om te implementeren en configureren van een consortium voor meerdere leden Ethereum-netwerk met minimale kennis van Azure en Ethereum. Het doel van de sjablonen is het eenvoudig te voor gebruikers voor setup dev/test Blockchain-implementaties in een paar stappen.

- <!-- | IS ASDK--> **De API-versie profiel 2017-03-09-profiel is bijgewerkt naar 2018-03-01-hybride**. API-profielen opgeven voor de Azure-resource-provider en de API-versie voor Azure REST-eindpunten. Zie voor meer informatie over profielen [beheren API-versieprofielen in Azure Stack](/azure/azure-stack/user/azure-stack-version-profiles).

 ### <a name="fixed-issues"></a>Problemen opgelost
- <!-- IS ASDK--> We het probleem voor het maken van een beschikbaarheidsset in de portal dat heeft geresulteerd in de set met een domein met fouten en een updatedomein van 1 is opgelost. 

- <!-- IS ASDK --> Instellingen voor het schalen van virtuele-machineschaalsets zijn nu beschikbaar in de portal.  

- <!-- 2494144- IS, ASDK --> Het probleem waardoor sommige grootten van de virtuele machines uit de F-serie wordt weergegeven bij het selecteren van een VM-grootte voor de implementatie is opgelost. 

- <!-- IS, ASDK --> Verbeteringen voor prestaties bij het maken van virtuele machines en meer geoptimaliseerd gebruik van de onderliggende opslag.

- **Verschillende oplossingen** voor prestaties, stabiliteit, beveiliging en het besturingssysteem dat wordt gebruikt door Azure Stack.


### <a name="changes"></a>Wijzigingen
- <!-- 1697698  | IS, ASDK --> *Zelfstudies* in de koppeling gebruiker portaldashboard nu relevante artikelen in de online documentatie voor Azure Stack.

- <!-- 2515955   | IS ,ASDK--> *Alle services* vervangt *meer services* in de Azure Stack-beheerder en gebruiker portals. U kunt nu *alle services* als alternatief voor het navigeren in de Azure Stack-portals de dezelfde manier als in de Azure-portals.

- <!-- TBD | IS, ASDK --> *+ Een resource maken* vervangt *+ nieuw* in de Azure Stack-beheerder en gebruiker portals.  U kunt nu *+ een resource maken* als alternatief voor het navigeren in de Azure Stack-portals de dezelfde manier als in de Azure-portals.  

- <!--  TBD – IS, ASDK --> *Basic A* grootten van virtuele machines buiten gebruik worden gesteld voor [het maken van virtuele-machineschaalsets](azure-stack-compute-add-scalesets.md) (VMSS) via de portal. Als u wilt een VMSS te maken met deze grootte, PowerShell of een sjabloon te gebruiken.  

### <a name="common-vulnerabilities-and-exposures"></a>Common Vulnerabilities and Exposures

Deze update installeert de volgende updates:  

- [CVE-2018-0952](https://portal.msrc.microsoft.com/en-US/security-guidance/advisory/CVE-2018-0952)
- [CVE-2018-8200](https://portal.msrc.microsoft.com/en-US/security-guidance/advisory/CVE-2018-8200)
- [CVE-2018-8204](https://portal.msrc.microsoft.com/en-US/security-guidance/advisory/CVE-2018-8204)
- [CVE-2018-8253](https://portal.msrc.microsoft.com/en-US/security-guidance/advisory/CVE-2018-8253)
- [CVE-2018-8339](https://portal.msrc.microsoft.com/en-US/security-guidance/advisory/CVE-2018-8339)
- [CVE-2018-8340](https://portal.msrc.microsoft.com/en-US/security-guidance/advisory/CVE-2018-8340)
- [CVE-2018-8341](https://portal.msrc.microsoft.com/en-US/security-guidance/advisory/CVE-2018-8341)
- [CVE-2018-8343](https://portal.msrc.microsoft.com/en-US/security-guidance/advisory/CVE-2018-8343)
- [CVE-2018-8344](https://portal.msrc.microsoft.com/en-US/security-guidance/advisory/CVE-2018-8344)
- [CVE-2018-8345](https://portal.msrc.microsoft.com/en-US/security-guidance/advisory/CVE-2018-8345)
- [CVE-2018-8347](https://portal.msrc.microsoft.com/en-US/security-guidance/advisory/CVE-2018-8347)
- [CVE-2018-8348](https://portal.msrc.microsoft.com/en-US/security-guidance/advisory/CVE-2018-8348)
- [CVE-2018-8349](https://portal.msrc.microsoft.com/en-US/security-guidance/advisory/CVE-2018-8349)
- [CVE-2018-8394](https://portal.msrc.microsoft.com/en-US/security-guidance/advisory/CVE-2018-8394)
- [CVE-2018-8398](https://portal.msrc.microsoft.com/en-US/security-guidance/advisory/CVE-2018-8398)
- [CVE-2018-8401](https://portal.msrc.microsoft.com/en-US/security-guidance/advisory/CVE-2018-8401)
- [CVE-2018-8404](https://portal.msrc.microsoft.com/en-US/security-guidance/advisory/CVE-2018-8404)
- [CVE-2018-8405](https://portal.msrc.microsoft.com/en-US/security-guidance/advisory/CVE-2018-8405)
- [CVE-2018-8406](https://portal.msrc.microsoft.com/en-US/security-guidance/advisory/CVE-2018-8406)  

Klik op de hiervoor vermelde koppelingen voor meer informatie over deze beveiligingslekken, of Raadpleeg de Microsoft Knowledge Base-artikel [4343887](https://support.microsoft.com/help/4343887).

Deze update bevat ook de oplossing voor het speculatieve uitvoering kant kanaal beveiligingslek bekend als L1 Terminal fouttolerantie (L1TF), dat wordt beschreven in de [Microsoft Security Advisory ADV180018](https://portal.msrc.microsoft.com/en-US/security-guidance/advisory/ADV180018).  

### <a name="prerequisites"></a>Vereisten

- De Azure-Stack installeren [1807 bijwerken](azure-stack-update-1807.md) voordat u de Azure Stack 1808 update toepassen. 

- Installeer de meest recente beschikbare [update of hotfix voor versie 1807](azure-stack-update-1807.md#post-update-steps).  
  > [!TIP]  
  > Abonneer u op de volgende *RRS* of *Atom* feeds, blijven van het Azure Stack Hotfixes:
  > - RRS: https://support.microsoft.com/app/content/api/content/feeds/sap/en-us/32d322a8-acae-202d-e9a9-7371dccf381b/rss ... 
  > - Atom: https://support.microsoft.com/app/content/api/content/feeds/sap/en-us/32d322a8-acae-202d-e9a9-7371dccf381b/atom ...


- Voordat u begint met de installatie van deze update, voert u [Test AzureStack](azure-stack-diagnostic-test.md) met de volgende parameters om te valideren van de status van uw Azure-Stack en los eventuele operationele problemen gevonden, met inbegrip van alle waarschuwingen en fouten. Ook actieve waarschuwingen bekijken en op te lossen die actie is vereist.  

  ```PowerShell
  Test-AzureStack -Include AzsControlPlane, AzsDefenderSummary, AzsHostingInfraSummary, AzsHostingInfraUtilization, AzsInfraCapacity, AzsInfraRoleSummary, AzsPortalAPISummary, AzsSFRoleSummary, AzsStampBMCSummary
  ```   

### <a name="known-issues-with-the-update-process"></a>Bekende problemen met het updateproces

- Bij het uitvoeren van [Test AzureStack](azure-stack-diagnostic-test.md) nadat de update 1808, een waarschuwingsbericht wordt weergegeven van de Baseboard Management Controller (BMC) wordt weergegeven. U kunt deze waarschuwing negeren.

- <!-- 2468613 - IS --> Tijdens de installatie van deze update, ziet u mogelijk waarschuwingen met de titel *fout: sjabloon voor FaultType UserAccounts.New ontbreekt.*  U kunt deze waarschuwingen negeren. Deze waarschuwingen worden automatisch gesloten nadat de installatie van deze update is voltooid.

- <!-- 2489559 - IS --> Probeer niet voor het maken van virtuele machines tijdens de installatie van deze update. Zie voor meer informatie over het beheren van updates [-updates beheren in Azure Stack-overzicht](azure-stack-updates.md#plan-for-updates).

- <!-- 2830461 - IS --> In bepaalde omstandigheden wanneer een update uw aandacht vereist, kan de bijbehorende waarschuwing niet worden gegenereerd. De status van de nauwkeurige, nog steeds worden weergegeven in de portal en wordt niet negatief beïnvloed.

### <a name="post-update-steps"></a>Stappen na het bijwerken
Na de installatie van deze update toepasselijke Hotfixes te installeren. Raadpleeg voor meer informatie de volgende knowledge base-artikelen, evenals onze [beleid onderhoud](azure-stack-servicing-policy.md). 
- [KB 4464226 – Azure Stack Hotfix Azure Stack Hotfix 1.1808.1.99]( https://support.microsoft.com/help/4464226)


## <a name="known-issues-post-installation"></a>Bekende problemen (na de installatie)

Hier volgen na de installatie bekende problemen voor deze buildversie.

### <a name="portal"></a>Portal

- De technische documentatie voor Azure Stack is gericht op de meest recente versie. Vanwege een portal-wijzigingen tussen versies, kunnen wat u ziet wanneer u de Azure Stack-portals afwijken van wat u in de documentatie ziet. 

- <!-- TBD - IS ASDK --> U ziet een leeg dashboard in de portal. Als u wilt herstellen in het dashboard, klikt u op **Dashboard bewerken**, klik met de rechtermuisknop en selecteer **opnieuw ingesteld op standaardstatus**.

- <!-- 2930718 - IS ASDK --> In de beheerdersportal bij het openen van de details van elk gebruikersabonnement na het sluiten van de blade en te klikken op **Recent**, de naam van het abonnement wordt niet weergegeven.

- <!-- 3060156 - IS ASDK --> In de beheerder en de gebruiker portals, te klikken op de portal-instellingen en selecteer **verwijdert u alle instellingen en privédashboards** werkt niet zoals verwacht. Er wordt een foutmelding weergegeven. 

- <!-- 2930799 - IS ASDK --> In de beheerder en de gebruiker-portals, onder **alle services**, de asset **DDoS-beschermingsplannen** ten onrechte wordt weergegeven. Het is niet beschikbaar in Azure Stack. Als u probeert te maken, er een foutbericht weergegeven waarin staat dat de portal kan het marketplace-item niet maken. 

- <!-- 2930820 - IS ASDK --> In de beheerder en de gebruiker-portals, als u 'Docker', zoekt wordt het item onjuist geretourneerd. Het is niet beschikbaar in Azure Stack. Als u probeert te maken, wordt een blade met een indicatie van de fout weergegeven. 

- <!-- 2967387 – IS, ASDK --> Het account waarmee u zich aanmeldt bij de portal voor Azure Stack-beheerder of gebruiker worden weergegeven als **onbekende gebruiker**. Dit treedt op wanneer het account beschikt niet over een een *eerste* of *laatste* naam die is opgegeven. U kunt dit probleem omzeilen, het gebruikersaccount voor de naam van de eerste of laatste te bewerken. U moet vervolgens Meld u af en meld u vervolgens terug naar de portal. 

-  <!--  2873083 - IS ASDK --> Wanneer gebruikt u de portal voor het maken van een virtuele-machineschaalset instellen (VMSS), de *exemplaargrootte* vervolgkeuzelijst niet juist geladen wanneer u Internet Explorer gebruikt. U kunt dit probleem omzeilen, gebruikt u een andere browser tijdens het gebruik van de portal voor het maken van een VMSS.  

- <!-- 2931230 – IS  ASDK --> Abonnementen die zijn toegevoegd aan een gebruikersabonnement als een aanvullende plan kunnen niet worden verwijderd, zelfs wanneer u het abonnement uit het gebruikersabonnement verwijderen. Het abonnement blijft totdat de abonnementen die verwijzen naar het aanvullende plan worden ook verwijderd. 

- <!--2760466 – IS  ASDK --> Wanneer u een nieuwe Azure Stack-omgeving met deze versie installeert, de waarschuwing die aangeeft *activering vereist* mogelijk niet weergegeven. [Activering](azure-stack-registration.md) is vereist voordat u de marketplace-publicatie kunt gebruiken.  

- <!-- TBD - IS ASDK --> De twee administratieve abonnementstypen die waren [geïntroduceerd in versie 1804](azure-stack-update-1804.md#new-features) mag niet worden gebruikt. De abonnementstypen zijn **softwarelicentiecontrole abonnement**, en **verbruik abonnement**. Deze abonnementstypen zijn zichtbaar in de nieuwe Azure Stack-omgevingen vanaf versie 1804 maar nog niet klaar voor gebruik. U moet echter ook doorgaan met de **Provider standaard** abonnementstype.

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

- <!-- 3090289 – IS, ASDK --> Na het toepassen van de 1808 bijwerken, kunnen de volgende problemen optreden bij het implementeren van virtuele machines met Managed Disks:

   1. Als het abonnement is gemaakt vóór de update 1808 implementeren van virtuele machine met Managed Disks mislukken met een interne fout. Los de fout op door deze stappen voor elk abonnement uit te voeren:
      1. Ga in de tenantportal naar **abonnementen** en zoek het abonnement. Klik op **Resourceproviders**, klikt u vervolgens op **Microsoft.Compute**, en klik vervolgens op **opnieuw registreren**.
      2. Onder hetzelfde abonnement, gaat u naar **Access Control (IAM)**, en Controleer **Azure Stack – beheerde schijf** wordt vermeld.
   2. Als u een omgeving met meerdere tenants hebt geconfigureerd, mislukken virtuele machines implementeren in een abonnement dat is gekoppeld aan een gast-map met een interne fout. U kunt de fout oplossen door de volgende stappen uit:
      1. Van toepassing de [1808 Azure Stack-Hotfix](https://support.microsoft.com/help/4465859).
      2. Volg de stappen in [in dit artikel](azure-stack-enable-multitenancy.md#registering-azure-stack-with-the-guest-directory) opnieuw configureren van elk van de Gast-mappen.


- <!-- 2869209 – IS, ASDK --> Wanneer u de [ **toevoegen AzsPlatformImage** cmdlet](https://docs.microsoft.com/powershell/module/azs.compute.admin/add-azsplatformimage?view=azurestackps-1.4.0), moet u de **- OsUri** parameter als het opslagaccount URI waar de schijf is geüpload. Als u het lokale pad van de schijf gebruikt, wordt de cmdlet mislukt met de volgende fout: *langdurige bewerking is mislukt met de status 'Mislukt'*. 

- <!--  2966665 – IS, ASDK --> SSD-gegevensschijven koppelen aan de grootte van de premium beheerde schijf virtuele machines (DS, DSv2, Fs, Fs_V2) mislukt met fout: *schijven voor de virtuele machine 'vmname' Fout bij het bijwerken is mislukt: gevraagde bewerking kan niet worden uitgevoerd omdat het type opslagaccount ' Premium_LRS wordt niet ondersteund voor VM-grootte ' Standard_DS/Ds_V2/FS/Fs_v2)*

   U kunt dit probleem omzeilen, gebruikt u *Standard_LRS* gegevensschijven in plaats van *Premium_LRS schijven*. Het gebruik van *Standard_LRS* gegevensschijven IOP's of de kosten voor facturering niet wijzigen. 

- <!--  2795678 – IS, ASDK --> Wanneer u de portal virtuele machines (VM) maken in een premium VM-grootte (DS, Ds_v2, FS, FSv2) gebruikt, wordt de virtuele machine wordt gemaakt in een standard storage-account. Het maken van in een standard storage-account heeft geen invloed op functioneel, IOP's, of facturering. 

   U kunt gewoon de waarschuwing dat negeren: *u hebt gekozen voor het gebruik van een standaard schijf op een grootte die premium-schijven ondersteunt. Dit kan invloed hebben op prestaties van besturingssysteem en wordt niet aanbevolen. Overweeg het gebruik van premium-opslag (SSD) in plaats daarvan.*

- <!-- 2967447 - IS, ASDK --> De virtuele-machineschaalset (VMSS) aan die conferenties 7.2 CentOS gebaseerde biedt als een optie voor implementatie. Omdat die installatiekopie niet beschikbaar op Azure Stack is, selecteert u een ander besturingssysteem voor uw implementatie of u een ARM-sjabloon op te geven van een andere CentOS-installatiekopie die door de operator voorafgaand aan de implementatie van de marketplace is gedownload.  

- <!-- 2724873 - IS --> Bij het gebruik van de PowerShell-cmdlets **Start AzsScaleUnitNode** of **Stop-AzsScaleunitNode** voor het beheren van schaaleenheden, de eerste poging om te starten of stoppen van de schaaleenheid kan mislukken. Als de cmdlet voor de eerste keer uitvoert mislukt, wordt de cmdlet nogmaals uitvoeren. De tweede uitvoering moet slagen om de bewerking te voltooien. 

- <!-- TBD - IS ASDK --> Wanneer u virtuele machines op de gebruikersportal van Azure Stack maakt, wordt een onjuist aantal gegevensschijven die aan een virtuele machine uit de DS-serie kunt koppelen door de portal weergegeven. DS-serie VM's kan zo veel gegevensschijven bevatten als de Azure-configuratie.

- <!-- TBD - IS ASDK --> Als u een omgeving met meerdere tenants, inrichten van een virtuele machine met een beheerde schijf in een abonnement niet behoren tot de standaardtenant is mislukt met de volgende fout: *de resourcebewerking is voltooid met afgesloten Inrichtingsstatus 'Mislukt'.*

- <!-- TBD - IS ASDK --> Als een uitbreiding op een VM-implementatie de inrichting te lang duurt, laat gebruikers de time-out van de inrichting in plaats van bij stoppen van de procedure voor de toewijzing ongedaan of verwijder de virtuele machine.  

- <!-- 1662991 IS ASDK --> Diagnostische gegevens over Linux-VM wordt niet ondersteund in Azure Stack. Wanneer u een Linux-VM met VM diagnostics is ingeschakeld implementeren, wordt de implementatie mislukt. De implementatie mislukt ook als u de Linux-VM eenvoudige metrische gegevens via diagnostische instellingen inschakelen.  

- <!-- 2724961- IS ASDK --> Wanneer u registreert de **Microsoft.Insight** resourceprovider in instellingen voor abonnement, en een Windows-VM maken met gast OS diagnostische ingeschakeld, wordt de grafiek CPU-Percentage in de overzichtspagina van virtuele machine niet mogelijk om metrische gegevens weer te geven.

   Als u zoekt de grafiek CPU-Percentage voor de virtuele machine, gaat u naar de **metrische gegevens** blade en het weergeven van alle ondersteunde Windows-VM-Gast metrische gegevens.



### <a name="networking"></a>Netwerken  

- <!-- 1766332 - IS ASDK --> Onder **netwerken**, als u klikt op **VPN-Gateway maken** voor het instellen van een VPN-verbinding, **op basis van beleid** wordt vermeld als een VPN-type. Selecteer deze optie niet. Alleen de **Route op basis van** optie wordt ondersteund in Azure Stack.

- <!-- 1902460 - IS ASDK --> Azure Stack biedt ondersteuning voor een enkel *lokale netwerkgateway* per IP-adres. Dit geldt voor alle tenant-abonnementen. Nadat het maken van de eerste lokale gateway netwerkverbinding, volgende pogingen tot het maken van een resource van een lokale netwerkgateway met hetzelfde IP-adres worden geblokkeerd.

- <!-- 16309153 - IS ASDK --> In een Virtueelnetwerk dat is gemaakt met een DNS-Server-instelling van *automatische*, wijzigen naar een aangepaste DNS-Server mislukt. De bijgewerkte instellingen worden niet gepusht naar virtuele machines in dit Vnet.

- <!-- 2702741 -  IS ASDK --> Openbare IP-adressen die zijn geïmplementeerd met behulp van de dynamische toewijzingsmethode zijn niet noodzakelijkerwijs te behouden nadat een Stop-toewijzing is uitgegeven.

- <!-- 2529607 - IS ASDK --> Tijdens de Azure Stack *geheim rotatie*, er is een periode waarin openbare IP-adressen niet bereikbaar voor twee tot vijf minuten zijn.

-   <!-- 2664148 - IS ASDK --> Kunnen optreden in scenario's waar de tenant er toegang hun virtuele machines tot heeft met behulp van een S2S-VPN-tunnel, een scenario waarbij verbindingspogingen mislukt als de on-premises-subnet is toegevoegd aan de lokale netwerkgateway nadat de gateway is al gemaakt. 


<!-- ### SQL and MySQL-->


### <a name="app-service"></a>App Service

- <!-- 2352906 - IS ASDK --> Gebruikers moeten de storage resourceprovider registreren voordat ze hun eerste Azure-functie in het abonnement maken.

- <!-- 2489178 - IS ASDK --> Als u wilt de infrastructuur (werknemers, beheer, front-rollen) uitbreiden, moet u PowerShell gebruiken, zoals beschreven in de opmerkingen bij de release voor Compute.



### <a name="usage"></a>Gebruik  
- <!-- TBD - IS ASDK --> Gebruik openbare IP-adres-gebruiksgegevens meter toont dezelfde *datum/tijd evenement* waarde voor elke record in plaats van de *TimeDate* stempel waarin wordt weergegeven wanneer de record is gemaakt. U kunt deze gegevens op dit moment niet gebruiken om uit te voeren nauwkeurige accounting van gebruik van openbare IP-adres.


<!-- #### Identity -->
<!-- #### Marketplace -->


## <a name="download-the-update"></a>De update downloaden
U kunt de Azure Stack 1808 update-pakket van downloaden [hier](https://aka.ms/azurestackupdatedownload).
  

## <a name="next-steps"></a>Volgende stappen
- Het uitvoeren van onderhoud beleid voor geïntegreerde Azure Stack-systemen, en wat u moet doen om te voorkomen dat uw systeem in een ondersteunde status Zie [Azure Stack servicebeleid](azure-stack-servicing-policy.md).  
- Voor het gebruik van de bevoegde eindpunt (PEP) om te controleren en hervatten van updates, Zie [-updates in Azure Stack met behulp van het eindpunt van de bevoegde controleren](azure-stack-monitor-update.md).  
- Zie voor een overzicht van de update management in Azure Stack, [-updates beheren in Azure Stack-overzicht](azure-stack-updates.md).  
- Zie voor meer informatie over het toepassen van updates met Azure Stack [toepassen van updates in Azure Stack](azure-stack-apply-updates.md).  
