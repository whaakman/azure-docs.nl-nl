---
title: Update voor Azure Stack 1809 | Microsoft Docs
description: Meer informatie over wat is er nieuw in de update 1809 voor geïntegreerde Azure Stack-systemen, met inbegrip van de bekende problemen en waar u de update te downloaden.
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
ms.date: 10/26/2018
ms.author: sethm
ms.reviewer: justini
ms.openlocfilehash: 161b54620bd8a3c9994228a30bd7495717064168
ms.sourcegitcommit: 48592dd2827c6f6f05455c56e8f600882adb80dc
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/26/2018
ms.locfileid: "50158463"
---
# <a name="azure-stack-1809-update"></a>Azure Stack 1809 update

*Is van toepassing op: Azure Stack-geïntegreerde systemen*

Dit artikel wordt de inhoud van het updatepakket 1809 beschreven. Het updatepakket bevat verbeteringen, problemen en bekende problemen voor deze versie van Azure Stack. In dit artikel bevat ook een koppeling, zodat u kunt de update downloaden. Bekende problemen zijn onderverdeeld in problemen direct verband houden met het updateproces en problemen met de build (na de installatie).

> [!IMPORTANT]  
> Dit pakket is alleen voor geïntegreerde Azure Stack-systemen. Dit pakket niet van toepassing op de Azure Stack Development Kit.

## <a name="build-reference"></a>Naslaginformatie over bouwen

De Azure Stack 1809 update build-nummer **1.1809.0.90**.  

### <a name="new-features"></a>Nieuwe functies

Deze update bevat de volgende verbeteringen voor Azure Stack:

- <!--  2712869   | IS  ASDK -->  **Azure Stack syslog-client (algemene beschikbaarheid)** deze client kunt u het doorsturen van controles, waarschuwingen en -logboeken met betrekking tot de Azure Stack-infrastructuur naar een syslog-server of security information en event management (SIEM) software extern naar Azure Stack. De syslog-client biedt nu ondersteuning voor de poort waarop de syslog-server luistert op te geven.

   Met deze release de syslog-client is algemeen beschikbaar en deze kan worden gebruikt in een productieomgeving.

   Zie voor meer informatie, [Azure Stack syslog doorsturen](azure-stack-integrate-security.md).

- U kunt nu [Verplaats de resource registratie](azure-stack-registration.md#move-a-registration-resource) op Azure tussen resourcegroepen zonder dat u opnieuw wilt registreren. Cloud Solution Providers (CSP's) kunnen ook verplaatst u de registratie bron tussen abonnementen, zolang de nieuwe en het oude abonnementen worden toegewezen aan dezelfde CSP-partner-ID. Dit heeft geen invloed op de bestaande toewijzingen van de klant-tenant. 

### <a name="fixed-issues"></a>Problemen opgelost

<!-- TBD - IS ASDK -->
- In de portal is de geheugen-grafiek reporting capaciteit vrij/gebruikt nu nauwkeurig. U kunt nu het aantal virtuele machines kunt maken meer betrouwbare wijze voorspellen.

<!-- TBD - IS ASDK --> 
- Er is een probleem waarin u virtuele machines gemaakt op de gebruikersportal van Azure Stack en de portal weergegeven een onjuist aantal gegevensschijven die aan een virtuele machine uit de DS-serie kunt koppelen opgelost. DS-serie VM's kan zo veel gegevensschijven bevatten als de Azure-configuratie.

- De volgende beheerde schijf problemen zijn opgelost in 1809 en ook zijn opgelost in de 1808 [Azure Stack-Hotfix 1.1808.5.110](https://support.microsoft.com/help/4468920/): 

   <!--  2966665 – IS, ASDK --> 
   - Het probleem opgelost in welke koppelen SSD-gegevensschijven aan premium-grootte beheerde schijf virtuele machines (DS, DSv2, Fs, Fs_V2) is mislukt met een fout opgetreden: *schijven voor de virtuele machine 'vmname' Fout bij het bijwerken is mislukt: gevraagde bewerking kan niet worden uitgevoerd omdat opslagaccounttype 'Premium_LRS' wordt niet ondersteund voor VM-grootte ' Standard_DS/Ds_V2/FS/Fs_v2)*. 
   
   - Het maken van een beheerde schijf-VM met behulp van **createOption**: **Attach** mislukt met de volgende fout: *langdurige bewerking is mislukt met de status 'Mislukt'. Aanvullende informatie:' interne uitvoering is een fout opgetreden.'*
   ErrorCode: InternalExecutionError ErrorMessage: Er is een interne uitvoeringsfout opgetreden.
   
   Dit probleem is opgelost.

- <!-- 2702741 -  IS, ASDK --> Probleem opgelost in welke openbare IP-adressen die zijn geïmplementeerd met behulp van de dynamische toewijzing methode zijn niet gegarandeerd te behouden nadat een Stop-toewijzing is uitgegeven. Ze blijven nu behouden.

- <!-- 3078022 - IS, ASDK --> Als een virtuele machine stop-deallocated voordat u 1808 is kan deze niet worden opnieuw worden toegewezen na de 1808 update.  Dit probleem is opgelost in 1809. Exemplaren die in deze status zijn en kunnen niet worden gestart kunnen worden gestart in 1809 met deze oplossing. Het probleem voorkomt ook dat dit probleem opnieuw optreedt.

### <a name="changes"></a>Wijzigingen

Geen.

### <a name="common-vulnerabilities-and-exposures"></a>Common Vulnerabilities and Exposures

Deze update installeert de volgende beveiligingsupdates:  

- [ADV180022](https://portal.msrc.microsoft.com/en-US/security-guidance/advisory/ADV180022)
- [CVE-2018-0965](https://portal.msrc.microsoft.com/en-US/security-guidance/advisory/CVE-2018-0965)
- [CVE-2018-8271](https://portal.msrc.microsoft.com/en-US/security-guidance/advisory/CVE-2018-8271)
- [CVE-2018-8320](https://portal.msrc.microsoft.com/en-US/security-guidance/advisory/CVE-2018-8320)
- [CVE-2018-8330](https://portal.msrc.microsoft.com/en-US/security-guidance/advisory/CVE-2018-8330)
- [CVE-2018-8332](https://portal.msrc.microsoft.com/en-US/security-guidance/advisory/CVE-2018-8332)
- [CVE-2018-8333](https://portal.msrc.microsoft.com/en-US/security-guidance/advisory/CVE-2018-8333)
- [CVE-2018-8335](https://portal.msrc.microsoft.com/en-US/security-guidance/advisory/CVE-2018-8335)
- [CVE-2018-8392](https://portal.msrc.microsoft.com/en-US/security-guidance/advisory/CVE-2018-8392)
- [CVE-2018-8393](https://portal.msrc.microsoft.com/en-US/security-guidance/advisory/CVE-2018-8393)
- [CVE-2018-8410](https://portal.msrc.microsoft.com/en-US/security-guidance/advisory/CVE-2018-8410)
- [CVE-2018-8411](https://portal.msrc.microsoft.com/en-US/security-guidance/advisory/CVE-2018-8411)
- [CVE-2018-8413](https://portal.msrc.microsoft.com/en-US/security-guidance/advisory/CVE-2018-8413)
- [CVE-2018-8419](https://portal.msrc.microsoft.com/en-US/security-guidance/advisory/CVE-2018-8419)
- [CVE-2018-8420](https://portal.msrc.microsoft.com/en-US/security-guidance/advisory/CVE-2018-8420)
- [CVE-2018-8423](https://portal.msrc.microsoft.com/en-US/security-guidance/advisory/CVE-2018-8423)
- [CVE-2018-8424](https://portal.msrc.microsoft.com/en-US/security-guidance/advisory/CVE-2018-8424)
- [CVE-2018-8433](https://portal.msrc.microsoft.com/en-US/security-guidance/advisory/CVE-2018-8433)
- [CVE-2018-8434](https://portal.msrc.microsoft.com/en-US/security-guidance/advisory/CVE-2018-8434)
- [CVE-2018-8435](https://portal.msrc.microsoft.com/en-US/security-guidance/advisory/CVE-2018-8435)
- [CVE-2018-8438](https://portal.msrc.microsoft.com/en-US/security-guidance/advisory/CVE-2018-8438)
- [CVE-2018-8439](https://portal.msrc.microsoft.com/en-US/security-guidance/advisory/CVE-2018-8439)
- [CVE-2018-8440](https://portal.msrc.microsoft.com/en-US/security-guidance/advisory/CVE-2018-8440)
- [CVE-2018-8442](https://portal.msrc.microsoft.com/en-US/security-guidance/advisory/CVE-2018-8442)
- [CVE-2018-8443](https://portal.msrc.microsoft.com/en-US/security-guidance/advisory/CVE-2018-8443)
- [CVE-2018-8446](https://portal.msrc.microsoft.com/en-US/security-guidance/advisory/CVE-2018-8446)
- [CVE-2018-8449](https://portal.msrc.microsoft.com/en-US/security-guidance/advisory/CVE-2018-8449)
- [CVE-2018-8453](https://portal.msrc.microsoft.com/en-US/security-guidance/advisory/CVE-2018-8453)
- [CVE-2018-8455](https://portal.msrc.microsoft.com/en-US/security-guidance/advisory/CVE-2018-8455)
- [CVE-2018-8462](https://portal.msrc.microsoft.com/en-US/security-guidance/advisory/CVE-2018-8462)
- [CVE-2018-8468](https://portal.msrc.microsoft.com/en-US/security-guidance/advisory/CVE-2018-8468)
- [CVE-2018-8472](https://portal.msrc.microsoft.com/en-US/security-guidance/advisory/CVE-2018-8472)
- [CVE-2018-8475](https://portal.msrc.microsoft.com/en-US/security-guidance/advisory/CVE-2018-8475)
- [CVE-2018-8481](https://portal.msrc.microsoft.com/en-US/security-guidance/advisory/CVE-2018-8481)
- [CVE-2018-8482](https://portal.msrc.microsoft.com/en-US/security-guidance/advisory/CVE-2018-8482)
- [CVE-2018-8484](https://portal.msrc.microsoft.com/en-US/security-guidance/advisory/CVE-2018-8484)
- [CVE-2018-8486](https://portal.msrc.microsoft.com/en-US/security-guidance/advisory/CVE-2018-8486)
- [CVE-2018-8489](https://portal.msrc.microsoft.com/en-US/security-guidance/advisory/CVE-2018-8489)
- [CVE-2018-8490](https://portal.msrc.microsoft.com/en-US/security-guidance/advisory/CVE-2018-8490)
- [CVE-2018-8492](https://portal.msrc.microsoft.com/en-US/security-guidance/advisory/CVE-2018-8492)
- [CVE-2018-8493](https://portal.msrc.microsoft.com/en-US/security-guidance/advisory/CVE-2018-8493)
- [CVE-2018-8494](https://portal.msrc.microsoft.com/en-US/security-guidance/advisory/CVE-2018-8494)
- [CVE-2018-8495](https://portal.msrc.microsoft.com/en-US/security-guidance/advisory/CVE-2018-8495)
- [CVE-2018-8497](https://portal.msrc.microsoft.com/en-US/security-guidance/advisory/CVE-2018-8497)

Klik op de hiervoor vermelde koppelingen voor meer informatie over deze beveiligingslekken of Zie Microsoft Knowledge Base-artikelen [4457131](https://support.microsoft.com/help/4457131) en [4462917](https://support.microsoft.com/help/4462917).

### <a name="prerequisites"></a>Vereisten

- De meest recente Azure Stack-Hotfix voor 1808 installeren voordat u 1809 toepast. Zie voor meer informatie, [KB 4468920 – Azure Stack Hotfix Azure Stack Hotfix 1.1808.5.110](https://support.microsoft.com/en-us/help/4468920).

  > [!TIP]  
  > Abonneer u op de volgende *RRS* of *Atom* feeds, blijven van het Azure Stack Hotfixes:
  > - RRS: https://support.microsoft.com/app/content/api/content/feeds/sap/en-us/32d322a8-acae-202d-e9a9-7371dccf381b/rss ... 
  > - Atom: https://support.microsoft.com/app/content/api/content/feeds/sap/en-us/32d322a8-acae-202d-e9a9-7371dccf381b/atom ...


- Voordat u begint met de installatie van deze update, voert u [Test AzureStack](azure-stack-diagnostic-test.md) met de volgende parameters om te valideren van de status van uw Azure-Stack en los eventuele operationele problemen gevonden, met inbegrip van alle waarschuwingen en fouten. Ook actieve waarschuwingen bekijken en op te lossen die actie is vereist.  

  ```PowerShell
  Test-AzureStack -Include AzsControlPlane, AzsDefenderSummary, AzsHostingInfraSummary, AzsHostingInfraUtilization, AzsInfraCapacity, AzsInfraRoleSummary, AzsPortalAPISummary, AzsSFRoleSummary, AzsStampBMCSummary
  ``` 

### <a name="known-issues-with-the-update-process"></a>Bekende problemen met het updateproces

- Bij het uitvoeren van [Test AzureStack](azure-stack-diagnostic-test.md) nadat de update 1809, een waarschuwingsbericht wordt weergegeven van de Baseboard Management Controller (BMC) wordt weergegeven. U kunt deze waarschuwing negeren.

- <!-- 2468613 - IS --> Tijdens de installatie van deze update, ziet u mogelijk waarschuwingen met de titel *fout: sjabloon voor FaultType UserAccounts.New ontbreekt.*  U kunt deze waarschuwingen negeren. Deze waarschuwingen worden automatisch gesloten nadat de installatie van deze update is voltooid.

- <!-- 2489559 - IS --> Probeer niet voor het maken van virtuele machines tijdens de installatie van deze update. Zie voor meer informatie over het beheren van updates [-updates beheren in Azure Stack-overzicht](azure-stack-updates.md#plan-for-updates).

### <a name="post-update-steps"></a>Stappen na het bijwerken

*Er zijn geen stappen na het bijwerken van update 1809.*

<!-- After the installation of this update, install any applicable Hotfixes. For more information view the following knowledge base articles, as well as our [Servicing Policy](azure-stack-servicing-policy.md).  
 - [Link to KB]()  
 -->

## <a name="known-issues-post-installation"></a>Bekende problemen (na de installatie)

Hier volgen na de installatie bekende problemen voor deze buildversie.

### <a name="portal"></a>Portal

- De technische documentatie voor Azure Stack is gericht op de meest recente versie. Vanwege een portal-wijzigingen tussen versies, kunnen wat u ziet wanneer u de Azure Stack-portals afwijken van wat u in de documentatie ziet.

<!-- 2930718 - IS ASDK --> 
- In de beheerdersportal bij het openen van de details van elk gebruikersabonnement na het sluiten van de blade en te klikken op **Recent**, de naam van het abonnement wordt niet weergegeven.

<!-- 3060156 - IS ASDK --> 
- In de beheerder en de gebruiker portals, te klikken op de portal-instellingen en selecteer **verwijdert u alle instellingen en privédashboards** werkt niet zoals verwacht. Er wordt een foutmelding weergegeven. 

<!-- 2930799 - IS ASDK --> 
- In de beheerder en de gebruiker-portals, onder **alle services**, de asset **DDoS-beschermingsplannen** ten onrechte wordt weergegeven. Het is niet beschikbaar in Azure Stack. Als u probeert te maken, er een foutbericht weergegeven waarin staat dat de portal kan het marketplace-item niet maken. 

<!-- 2930820 - IS ASDK --> 
- In de beheerder en de gebruiker-portals, als u 'Docker', zoekt wordt het item onjuist geretourneerd. Het is niet beschikbaar in Azure Stack. Als u probeert te maken, wordt een blade met een indicatie van de fout weergegeven. 

<!-- 2967387 – IS, ASDK --> 
- Het account waarmee u zich aanmeldt bij de portal voor Azure Stack-beheerder of gebruiker worden weergegeven als **onbekende gebruiker**. Dit bericht wordt weergegeven wanneer het account beschikt niet over een een *eerste* of *laatste* naam die is opgegeven. U kunt dit probleem omzeilen, het gebruikersaccount voor de naam van de eerste of laatste te bewerken. U moet vervolgens Meld u af en meld u vervolgens terug naar de portal.  

<!--  2873083 - IS ASDK --> 
-  Wanneer gebruikt u de portal voor het maken van een virtuele-machineschaalset instellen (VMSS), de *exemplaargrootte* vervolgkeuzelijst niet juist geladen wanneer u Internet Explorer gebruikt. U kunt dit probleem omzeilen, gebruikt u een andere browser tijdens het gebruik van de portal voor het maken van een VMSS.  

<!-- 2931230 – IS  ASDK --> 
- Abonnementen die zijn toegevoegd aan een gebruikersabonnement als een aanvullende plan kunnen niet worden verwijderd, zelfs wanneer u het abonnement uit het gebruikersabonnement verwijderen. Het abonnement blijft totdat de abonnementen die verwijzen naar het aanvullende plan worden ook verwijderd. 

<!--2760466 – IS  ASDK --> 
- Wanneer u een nieuwe Azure Stack-omgeving met deze versie installeert, de waarschuwing die aangeeft *activering vereist* mogelijk niet weergegeven. [Activering](azure-stack-registration.md) is vereist voordat u de marketplace-publicatie kunt gebruiken.  

<!-- TBD - IS ASDK --> 
- De twee administratieve abonnementstypen die zijn geïntroduceerd in versie 1804 mag niet worden gebruikt. De abonnementstypen zijn **softwarelicentiecontrole abonnement**, en **verbruik abonnement**. Deze abonnementstypen zijn zichtbaar in de nieuwe Azure Stack-omgevingen vanaf versie 1804 maar nog niet klaar voor gebruik. U moet echter ook doorgaan met de **Provider standaard** abonnementstype.

<!-- TBD - IS ASDK --> 
- Verwijderen van zwevende resources leidt van gebruiker-abonnementen. Als tijdelijke oplossing, eerst Gebruikersbronnen of de hele resourcegroep verwijderen en verwijder vervolgens gebruikersabonnementen.

<!-- TBD - IS ASDK --> 
- U kunt machtigingen aan uw abonnement met behulp van de Azure Stack-portals niet weergeven. Als tijdelijke oplossing, moet u PowerShell gebruiken om machtigingen te controleren.


### <a name="health-and-monitoring"></a>Status en bewaking

<!-- TBD - IS -->
- U ziet mogelijk de volgende waarschuwingen herhaaldelijk worden weergegeven en klik vervolgens op uw Azure Stack-systeem verdwijnen:
   - *Rolinstantie infrastructuur niet beschikbaar*
   - *Schaal eenheid knooppunt is offline*
   
  Voer de [Test AzureStack](azure-stack-diagnostic-test.md) cmdlet om te controleren of de status van de instanties van de infrastructuur en schaal eenheid knooppunten. Als er geen problemen zijn gedetecteerd door [Test AzureStack](azure-stack-diagnostic-test.md), kunt u deze waarschuwingen negeren. Als er een probleem is gedetecteerd, kunt u proberen om de infrastructuur-rolinstantie of knooppunt met de beheerportal of PowerShell te starten.

<!-- 1264761 - IS ASDK -->  
- Mogelijk ziet u waarschuwingen voor de **Health controller** onderdeel waarvoor u de volgende gegevens:  

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


<!-- 2812138 | IS --> 
- U ziet mogelijk een waarschuwing voor de **opslag** onderdeel dat de volgende gegevens heeft:

   - NAAM: Storage service interne communicatiefout  
   - ERNST: kritiek  
   - COMPONENT: opslag  
   - Beschrijving: Storage service-interne communicatiefout is opgetreden bij het verzenden van aanvragen naar de volgende knooppunten.  

    De waarschuwing kan veilig worden genegeerd, maar moet u de waarschuwing handmatig sluit.

<!-- 2368581 - IS, ASDK --> 
- Azure Stack-operators, als u een waarschuwing voor een beperkte hoeveelheid geheugen ontvangt en virtuele machines van tenants niet te implementeren met een **fout bij het maken van infrastructuur-VM**, is het mogelijk dat de Azure Stack-stempel heeft te weinig geheugen beschikbaar. Gebruik de [Azure Stack Capacity Planner](https://gallery.technet.microsoft.com/Azure-Stack-Capacity-24ccd822) naar het beste informatie over de beschikbare capaciteit voor uw workloads.

### <a name="compute"></a>Compute

<!-- 3235634 – IS, ASDK -->
- Het implementeren van VM's met grootten met een **v2** achtervoegsel; bijvoorbeeld, **Standard_A2_v2**, geef het achtervoegsel als **Standard_A2_v2** (kleine letters v). Gebruik geen **Standard_A2_V2** (V hoofdletters). Dit werkt in de globale Azure en is een inconsistentie in Azure Stack.

<!-- 3099544 – IS, ASDK --> 
- Wanneer u een nieuwe virtuele machine (VM) met behulp van de Azure Stack-portal maakt, en u de VM-grootte selecteert, wordt de kolom USD/maand weergegeven met een **niet beschikbaar** bericht. Deze kolom mag niet weergegeven. prijzen kolom, dat is het weergeven van de virtuele machine wordt niet ondersteund in Azure Stack.

<!-- 2869209 – IS, ASDK --> 
- Wanneer u de [ **toevoegen AzsPlatformImage** cmdlet](https://docs.microsoft.com/powershell/module/azs.compute.admin/add-azsplatformimage?view=azurestackps-1.4.0), moet u de **- OsUri** parameter als het opslagaccount URI waar de schijf is geüpload. Als u het lokale pad van de schijf gebruikt, wordt de cmdlet mislukt met de volgende fout: *langdurige bewerking is mislukt met de status 'Mislukt'*. 

<!--  2795678 – IS, ASDK --> 
- Wanneer u de portal virtuele machines (VM) maken in een premium VM-grootte (DS, Ds_v2, FS, FSv2) gebruikt, wordt de virtuele machine wordt gemaakt in een standard storage-account. Het maken van in een standard storage-account heeft geen invloed op functioneel, IOP's, of facturering. 

   U kunt gewoon de waarschuwing dat negeren: *u hebt gekozen voor het gebruik van een standaard schijf op een grootte die premium-schijven ondersteunt. Dit kan invloed hebben op prestaties van besturingssysteem en wordt niet aanbevolen. Overweeg het gebruik van premium-opslag (SSD) in plaats daarvan.*

<!-- 2967447 - IS, ASDK --> 
- De interface voor het maken van VM scale set (VMSS) biedt 7.2 CentOS gebaseerde als een optie voor implementatie. Omdat deze afbeelding niet beschikbaar op Azure Stack is, selecteert u een ander besturingssysteem voor uw implementatie of u een Azure Resource Manager-sjabloon op te geven van een andere CentOS-installatiekopie die door de operator voorafgaand aan de implementatie van de marketplace is gedownload.  

<!-- 2724873 - IS --> 
- Bij het gebruik van de PowerShell-cmdlets **Start AzsScaleUnitNode** of **Stop-AzsScaleunitNode** voor het beheren van schaaleenheden, de eerste poging om te starten of stoppen van de schaaleenheid kan mislukken. Als de cmdlet voor de eerste keer uitvoert mislukt, wordt de cmdlet nogmaals uitvoeren. De tweede uitvoering moet slagen om de bewerking te voltooien. 

<!-- TBD - IS ASDK --> 
- Als een uitbreiding op een VM-implementatie de inrichting te lang duurt, laat gebruikers de time-out van de inrichting in plaats van bij stoppen van de procedure voor de toewijzing ongedaan of verwijder de virtuele machine.  

<!-- 1662991 IS ASDK --> 
- Diagnostische gegevens over Linux-VM wordt niet ondersteund in Azure Stack. Wanneer u een Linux-VM met VM diagnostics is ingeschakeld implementeren, wordt de implementatie mislukt. De implementatie mislukt ook als u de Linux-VM eenvoudige metrische gegevens via diagnostische instellingen inschakelen.  

<!-- 2724961- IS ASDK --> 
- Wanneer u registreert de **Microsoft.Insight** resourceprovider in instellingen voor abonnement, en een virtuele Windows-machine maken met gast OS diagnostische ingeschakeld, de CPU-Percentage grafiek in de VM-overzichtspagina metrische gegevens niet weergeven.

   Metrische gegevens om gegevens te vinden, zoals de CPU-Percentage voor de virtuele machine, gaat u naar het venster metrische gegevens en weergeven van alle de ondersteunde Windows-VM metrische gegevens voor gasten.



### <a name="networking"></a>Netwerken  

<!-- 1766332 - IS ASDK --> 
- Onder **netwerken**, als u klikt op **VPN-Gateway maken** voor het instellen van een VPN-verbinding, **op basis van beleid** wordt vermeld als een VPN-type. Selecteer deze optie niet. Alleen de **Route op basis van** optie wordt ondersteund in Azure Stack.

<!-- 1902460 - IS ASDK --> 
- Azure Stack biedt ondersteuning voor een enkel *lokale netwerkgateway* per IP-adres. Dit geldt voor alle tenant-abonnementen. Nadat het maken van de eerste lokale gateway netwerkverbinding, volgende pogingen tot het maken van een resource van een lokale netwerkgateway met hetzelfde IP-adres worden geblokkeerd.

<!-- 16309153 - IS ASDK --> 
- In een Virtueelnetwerk dat is gemaakt met een DNS-Server-instelling van *automatische*, wijzigen naar een aangepaste DNS-Server mislukt. De bijgewerkte instellingen worden niet gepusht naar virtuele machines in dit Vnet.

<!-- 2529607 - IS ASDK --> 
- Tijdens de Azure Stack *geheim rotatie*, er is een periode waarin openbare IP-adressen niet bereikbaar voor twee tot vijf minuten zijn.

<!-- 2664148 - IS ASDK --> 
-   Kunnen optreden in scenario's waar de tenant er toegang hun virtuele machines tot heeft met behulp van een S2S-VPN-tunnel, een scenario waarbij verbindingspogingen mislukt als de on-premises-subnet is toegevoegd aan de lokale netwerkgateway nadat de gateway is al gemaakt. 


<!-- ### SQL and MySQL-->


### <a name="app-service"></a>App Service

<!-- 2352906 - IS ASDK --> 
- Gebruikers moeten de storage resourceprovider registreren voordat ze hun eerste Azure-functie in het abonnement maken.


### <a name="usage"></a>Gebruik  

<!-- TBD - IS ASDK --> 
- Openbare IP-adres meter gegevens over gebruik bevat dezelfde *datum/tijd evenement* waarde voor elke record in plaats van de *TimeDate* stempel waarin wordt weergegeven wanneer de record is gemaakt. U kunt deze gegevens op dit moment niet gebruiken om uit te voeren nauwkeurige accounting van gebruik van openbare IP-adres.


<!-- #### Identity -->
<!-- #### Marketplace -->


## <a name="download-the-update"></a>De update downloaden

U kunt de Azure Stack 1809 update-pakket van downloaden [hier](https://aka.ms/azurestackupdatedownload).
  

## <a name="next-steps"></a>Volgende stappen

- Het uitvoeren van onderhoud beleid voor geïntegreerde Azure Stack-systemen, en wat u moet doen om te voorkomen dat uw systeem in een ondersteunde status Zie [Azure Stack servicebeleid](azure-stack-servicing-policy.md).  
- Voor het gebruik van de bevoegde eindpunt (PEP) om te controleren en hervatten van updates, Zie [-updates in Azure Stack met behulp van het eindpunt van de bevoegde controleren](azure-stack-monitor-update.md).  
- Zie voor een overzicht van de update management in Azure Stack, [-updates beheren in Azure Stack-overzicht](azure-stack-updates.md).  
- Zie voor meer informatie over het toepassen van updates met Azure Stack [toepassen van updates in Azure Stack](azure-stack-apply-updates.md).  
