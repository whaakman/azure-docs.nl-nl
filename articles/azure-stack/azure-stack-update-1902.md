---
title: Update voor Azure Stack 1902 | Microsoft Docs
description: Meer informatie over de update 1902 voor geïntegreerde Azure Stack-systemen, met inbegrip van nieuwe functies, bekende problemen en het downloaden van de update.
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
ms.date: 04/03/2019
ms.author: sethm
ms.reviewer: adepue
ms.lastreviewed: 04/03/2019
ms.openlocfilehash: 5971692b3e6447bc790b2e34cf84eae66979f7f5
ms.sourcegitcommit: d83fa82d6fec451c0cb957a76cfba8d072b72f4f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/02/2019
ms.locfileid: "58862077"
---
# <a name="azure-stack-1902-update"></a>Azure Stack 1902-update

*Van toepassing op Geïntegreerde Azure Stack-systemen*

Dit artikel wordt de inhoud van het updatepakket 1902 beschreven. De update bevat verbeteringen, oplossingen en nieuwe functies voor deze versie van Azure Stack. Dit artikel ook worden bekende problemen in deze release beschreven, en bevat een koppeling om de update te downloaden. Bekende problemen zijn onderverdeeld in problemen direct verband houden met het updateproces en problemen met de build (na de installatie).

> [!IMPORTANT]  
> Dit pakket is alleen voor geïntegreerde Azure Stack-systemen. Dit pakket niet van toepassing op de Azure Stack Development Kit.

## <a name="build-reference"></a>Naslaginformatie over bouwen

Het build-nummer van de Azure Stack 1902-update **1.1902.0.69**.

## <a name="hotfixes"></a>Hotfixes

Azure Stack brengt hotfixes op gezette tijden. Zorg ervoor dat voor het installeren van de [meest recente Azure Stack-hotfix](#azure-stack-hotfixes) voor 1901 voordat u Azure Stack bijwerkt naar 1902.

Azure Stack-hotfixes zijn alleen van toepassing op Azure Stack-geïntegreerde systemen; Probeer niet te installeren van hotfixes in de ASDK.

> [!TIP]  
> Abonneer u op de volgende *RSS* of *Atom* feeds, blijven van het Azure Stack-hotfixes:
> - [RSS](https://support.microsoft.com/app/content/api/content/feeds/sap/en-us/32d322a8-acae-202d-e9a9-7371dccf381b/rss)
> - [Atom](https://support.microsoft.com/app/content/api/content/feeds/sap/en-us/32d322a8-acae-202d-e9a9-7371dccf381b/atom)

### <a name="azure-stack-hotfixes"></a>Hotfixes voor Azure Stack

- **1809**: [KB 4481548 – Azure Stack-hotfix 1.1809.12.114](https://support.microsoft.com/help/4481548/)
- **1811**: Er is geen huidige hotfix beschikbaar.
- **1901**: [KB 4495662 – Azure Stack-hotfix 1.1901.3.105](https://support.microsoft.com/help/4495662)
- **1902**: [KB 4494719 – Azure Stack-hotfix 1.1902.2.73](https://support.microsoft.com/help/4494719)

## <a name="prerequisites"></a>Vereisten

> [!IMPORTANT]
> U kunt 1902 installeren rechtstreeks vanuit de [1.1901.0.95 of 1.1901.0.99](azure-stack-update-1901.md#build-reference) loslaat, zonder dat eerst een hotfix 1901 wordt geïnstalleerd. Echter, als u de oudere hebt geïnstalleerd **1901.2.103** hotfix, moet u installeert de nieuwere [1901.3.105 hotfix](https://support.microsoft.com/help/4495662) voordat u doorgaat met de 1902.

- Voordat u begint met de installatie van deze update, voert u [Test AzureStack](azure-stack-diagnostic-test.md) met de volgende parameters om te valideren van de status van uw Azure-Stack en los eventuele operationele problemen gevonden, met inbegrip van alle waarschuwingen en fouten. Ook actieve waarschuwingen bekijken en op te lossen die actie is vereist:

    ```powershell
    Test-AzureStack -Include AzsDefenderSummary, AzsHostingInfraSummary, AzsHostingInfraUtilization, AzsInfraCapacity, AzsInfraRoleSummary, AzsPortalAPISummary, AzsSFRoleSummary, AzsStampBMCSummary, AzsHostingServiceCertificates
    ```

- Wanneer Azure Stack wordt beheerd door System Center Operations Manager (SCOM), zorg ervoor dat u het bijwerken van de [Management Pack voor Microsoft Azure Stack](https://www.microsoft.com/download/details.aspx?id=55184) naar versie 1.0.3.11 voordat u 1902 toepast.

- De indeling van het pakket voor de Azure Stack-update is gewijzigd van **.bin/.exe/.xml** naar **.zip/.xml** vanaf de release 1902. Klanten met verbonden Azure Stack-schaaleenheden ziet de **Update beschikbaar** bericht in de portal. Klanten die niet zijn verbonden kunnen nu gewoon downloaden en importeren van het ZIP-bestand met het bijbehorende XML-bestand.

<!-- ## New features -->

<!-- ## Fixed issues -->

## <a name="improvements"></a>Verbeteringen

- De 1902-build introduceert een nieuwe gebruikersinterface van de beheerder van Azure Stack-portal voor het maken van plannen, aanbiedingen, quota's en aanvullende plannen. Zie voor meer informatie, waaronder schermafbeeldingen, [maken, plannen, aanbiedingen en quota's](azure-stack-create-plan.md).

<!-- 1460884    Hotfix: Adding StorageController service permission to talk to ClusterOrchestrator  Add node -->
- Verbeteringen in de betrouwbaarheid van capaciteitsuitbreiding tijdens toevoegen knooppunt wanneer u overschakelt van de status van de eenheid schaal van 'Expanding opslag' in de status actief.

<!--
1426197 3852583: Increase Global VM script mutex wait time to accommodate enclosed operation timeout    PNU
1399240 3322580: [PNU] Optimize the DSC resource execution on the Host  PNU
1398846 Bug 3751038: ECEClient.psm1 should provide cmdlet to resume action plan instance    PNU
1398818 3685138, 3734779: ECE exception logging, VirtualMachine ConfigurePending should take node name from execution context   PNU
1381018 [1902] 3610787 - Infra VM creation should fail if the ClusterGroup already exists   PNU
-->
- Ter verbetering van pakket integriteit en beveiliging, evenals eenvoudiger beheer voor offline opname, is Microsoft de indeling van het updatepakket uit .exe en .bin bestanden gewijzigd in een ZIP-bestand. De nieuwe indeling wordt toegevoegd voor aanvullende betrouwbaarheid van het unpacking proces dat soms kan leiden tot de voorbereiding van de update negatieve invloed op i/o. De indeling van hetzelfde pakket geldt ook voor het bijwerken van pakketten van uw OEM.
- Om te verbeteren de ervaring van de Azure Stack-operator bij het uitvoeren van Test-AzureStack, operators kunnen nu gewoon gebruiken, ' Test-AzureStack-groep UpdateReadiness ' in plaats van tien extra parameters doorgeven na een Include-instructie.

  ```powershell
    Test-AzureStack -Group UpdateReadiness  
  ```  
  
- Ter verbetering van op de algehele betrouwbaarheid en beschikbaarheid van core infrastructuurservices tijdens het bijwerken, de systeemeigen Update-resourceprovider als onderdeel van de update-actieplan detecteert en aanroepen van de automatische algemene herstelbewerkingen zo nodig. Globale herstel 'herstellen' werkstromen zijn onder andere:
    - Controleren op infrastructuur-VM's die zich in een niet-optimale status en proberen te herstellen van deze zo nodig 
    - Controleren op problemen met de SQL-service als onderdeel van het besturingselement-plan en proberen te herstellen van deze zo nodig
    - Controleer de status van de Software Load Balancer (SLB)-service als onderdeel van de netwerkcontroller (NC) en probeert deze zo nodig herstellen
    - Controleer de status van de netwerkcontroller (NC)-service en probeert te herstellen indien nodig
    - Controleer de status van de EMS Recovery Console Service (ERCS) service fabric-knooppunten en ze herstellen indien nodig
    - Controleer de status van de XRP service fabric-knooppunten en ze herstellen indien nodig
    - Controleer de status van de Azure consistente opslag (ACS) service fabric-knooppunten en ze herstellen indien nodig

<!-- 1460884    Hotfix: Adding StorageController service permission to talk to ClusterOrchestrator  Add node -->
- Verbeteringen in de betrouwbaarheid van capaciteitsuitbreiding tijdens toevoegen knooppunt wanneer u overschakelt van de status van de eenheid schaal van 'Expanding opslag' in de status actief.    

<!-- 
1426690 [SOLNET] 3895478-Get-AzureStackLog_Output got terminated in the middle of network log   Diagnostics
1396607 3796092: Move Blob services log from Storage role to ACSBlob role to reduce the log size of Storage Diagnostics
1404529 3835749: Enable Group Policy Diagnostic Logs    Diagnostics
1436561 Bug 3949187: [Bug Fix] Remove AzsStorageSvcsSummary test from SecretRotationReadiness Test-AzureStack flag  Diagnostics
1404512 3849946: Get-AzureStackLog should collect all child folders from c:\Windows\Debug   Diagnostics 
-->
- Verbeteringen in Azure stack diagnostische hulpprogramma's voor het logboek verzameling betrouwbaarheid en prestaties verbeteren. Aanvullende logboekregistratie voor netwerk-en identiteitsservices. 

<!-- 1384958    Adding a Test-AzureStack group for Secret Rotation  Diagnostics -->
- Verbeteringen in de betrouwbaarheid van Test-AzureStack voor gereedheid van de geheime rotatie testen.

<!-- 1404751    3617292: Graph: Remove dependency on ADWS.  Identity -->
- Verbeteringen in AD Graph-betrouwbaarheid te vergroten bij het communiceren met de Active Directory-omgeving van de klant

<!-- 1391444    [ISE] Telemetry for Hardware Inventory - Fill gap for hardware inventory info   System info -->
- Hardware-inventarisverzameling verbeteringen in Get-AzureStackStampInformation.

- Als u wilt de betrouwbaarheid van bewerkingen die worden uitgevoerd op ERCS infrastructuur, wordt het geheugen voor elk exemplaar ERCS verhoogd van 8 GB tot 12 GB. De installatie van een geïntegreerde Azure Stack-systemen resulteert dit in een toename van 12 GB algemene.

> [!IMPORTANT]
> Als u wilt controleren of het patch- en bijwerkproces zo min mogelijk tenant uitvaltijd leidt, zorg ervoor dat uw Azure Stack-stempel heeft meer dan 12 GB aan beschikbare ruimte in de **capaciteit** blade. U ziet dit geheugen verhogen blijkt uit de **capaciteit** blade na een geslaagde installatie van de update.

## <a name="common-vulnerabilities-and-exposures"></a>Veelvoorkomende beveiligingsproblemen en risico 's

Deze update installeert de volgende beveiligingsupdates:  
- [ADV190005](https://portal.msrc.microsoft.com/en-us/security-guidance/advisory/ADV190006)
- [CVE-2019-0595](https://portal.msrc.microsoft.com/en-us/security-guidance/advisory/CVE-2019-0595)
- [CVE-2019-0596](https://portal.msrc.microsoft.com/en-us/security-guidance/advisory/CVE-2019-0596)
- [CVE-2019-0597](https://portal.msrc.microsoft.com/en-us/security-guidance/advisory/CVE-2019-0597)
- [CVE-2019-0598](https://portal.msrc.microsoft.com/en-us/security-guidance/advisory/CVE-2019-0598)
- [CVE-2019-0599](https://portal.msrc.microsoft.com/en-us/security-guidance/advisory/CVE-2019-0599)
- [CVE-2019-0600](https://portal.msrc.microsoft.com/en-us/security-guidance/advisory/CVE-2019-0600)
- [CVE-2019-0601](https://portal.msrc.microsoft.com/en-us/security-guidance/advisory/CVE-2019-0601)
- [CVE-2019-0602](https://portal.msrc.microsoft.com/en-us/security-guidance/advisory/CVE-2019-0602)
- [CVE-2019-0615](https://portal.msrc.microsoft.com/en-us/security-guidance/advisory/CVE-2019-0615)
- [CVE-2019-0616](https://portal.msrc.microsoft.com/en-us/security-guidance/advisory/CVE-2019-0616)
- [CVE-2019-0618](https://portal.msrc.microsoft.com/en-us/security-guidance/advisory/CVE-2019-0618)
- [CVE-2019-0619](https://portal.msrc.microsoft.com/en-us/security-guidance/advisory/CVE-2019-0619)
- [CVE-2019-0621](https://portal.msrc.microsoft.com/en-us/security-guidance/advisory/CVE-2019-0621)
- [CVE-2019-0623](https://portal.msrc.microsoft.com/en-us/security-guidance/advisory/CVE-2019-0623)
- [CVE-2019-0625](https://portal.msrc.microsoft.com/en-us/security-guidance/advisory/CVE-2019-0625)
- [CVE-2019-0626](https://portal.msrc.microsoft.com/en-us/security-guidance/advisory/CVE-2019-0626)
- [CVE-2019-0627](https://portal.msrc.microsoft.com/en-us/security-guidance/advisory/CVE-2019-0627)
- [CVE-2019-0628](https://portal.msrc.microsoft.com/en-us/security-guidance/advisory/CVE-2019-0628)
- [CVE-2019-0630](https://portal.msrc.microsoft.com/en-us/security-guidance/advisory/CVE-2019-0630)
- [CVE-2019-0631](https://portal.msrc.microsoft.com/en-us/security-guidance/advisory/CVE-2019-0631)
- [CVE-2019-0632](https://portal.msrc.microsoft.com/en-us/security-guidance/advisory/CVE-2019-0632)
- [CVE-2019-0633](https://portal.msrc.microsoft.com/en-us/security-guidance/advisory/CVE-2019-0633)
- [CVE-2019-0635](https://portal.msrc.microsoft.com/en-us/security-guidance/advisory/CVE-2019-0635)
- [CVE-2019-0636](https://portal.msrc.microsoft.com/en-us/security-guidance/advisory/CVE-2019-0636)
- [CVE-2019-0656](https://portal.msrc.microsoft.com/en-us/security-guidance/advisory/CVE-2019-0656)
- [CVE-2019-0659](https://portal.msrc.microsoft.com/en-us/security-guidance/advisory/CVE-2019-0659)
- [CVE-2019-0660](https://portal.msrc.microsoft.com/en-us/security-guidance/advisory/CVE-2019-0660)
- [CVE-2019-0662](https://portal.msrc.microsoft.com/en-us/security-guidance/advisory/CVE-2019-0662)
- [CVE-2019-0663](https://portal.msrc.microsoft.com/en-us/security-guidance/advisory/CVE-2019-0663)

Klik op de hiervoor vermelde koppelingen voor meer informatie over deze beveiligingslekken of Zie Microsoft Knowledge Base-artikelen [4487006](https://support.microsoft.com/en-us/help/4487006).

## <a name="known-issues-with-the-update-process"></a>Bekende problemen met het updateproces

- Bij het uitvoeren van [Test AzureStack](azure-stack-diagnostic-test.md), een waarschuwingsbericht wordt weergegeven van de Baseboard Management Controller (BMC) wordt weergegeven. U kunt deze waarschuwing negeren.

- <!-- 2468613 - IS --> Tijdens de installatie van deze update, ziet u mogelijk waarschuwingen met de titel `Error – Template for FaultType UserAccounts.New is missing.` u deze waarschuwingen kan negeren. De waarschuwingen worden automatisch gesloten na de installatie van deze update is voltooid.

## <a name="post-update-steps"></a>Stappen na het bijwerken

- Na de installatie van deze update toepasselijke hotfixes te installeren. Zie voor meer informatie, [Hotfixes](#hotfixes), evenals onze [beleid onderhoud](azure-stack-servicing-policy.md).  

- Ophalen van de data-at-rest-versleutelingssleutels en sla ze veilig buiten uw Azure Stack-implementatie. Ga als volgt de [instructies over het ophalen van de sleutels](azure-stack-security-bitlocker.md).

## <a name="known-issues-post-installation"></a>Bekende problemen (na de installatie)

Hier volgen na de installatie bekende problemen voor deze buildversie.

### <a name="portal"></a>Portal

<!-- 2930820 - IS ASDK --> 
- In de beheerder en de gebruiker-portals, als u 'Docker', zoekt wordt het item onjuist geretourneerd. Het is niet beschikbaar in Azure Stack. Als u probeert te maken, wordt een blade met een indicatie van de fout weergegeven. 

<!-- 2931230 – IS  ASDK --> 
- Abonnementen die zijn toegevoegd aan een gebruikersabonnement als een aanvullende plan kunnen niet worden verwijderd, zelfs wanneer u het abonnement uit het gebruikersabonnement verwijderen. Het abonnement blijft totdat de abonnementen die verwijzen naar het aanvullende plan worden ook verwijderd. 

<!-- TBD - IS ASDK --> 
- De twee administratieve abonnementstypen die zijn geïntroduceerd in versie 1804 mag niet worden gebruikt. De abonnementstypen zijn **softwarelicentiecontrole abonnement**, en **verbruik abonnement**. Deze abonnementstypen zijn zichtbaar in de nieuwe Azure Stack-omgevingen vanaf versie 1804 maar nog niet klaar voor gebruik. U moet echter ook doorgaan met de **Provider standaard** abonnementstype.

<!-- 3557860 - IS ASDK --> 
- Verwijderen van zwevende resources leidt van gebruiker-abonnementen. Als tijdelijke oplossing, eerst Gebruikersbronnen of de hele resourcegroep verwijderen en verwijder vervolgens de gebruikersabonnementen.

<!-- 1663805 - IS ASDK --> 
- U kunt machtigingen aan uw abonnement met behulp van de Azure Stack-portals niet weergeven. Als tijdelijke oplossing gebruiken [PowerShell machtigingen kan verifiëren](/powershell/module/azs.subscriptions.admin/get-azssubscriptionplan).

<!-- ### Health and monitoring -->

### <a name="compute"></a>Compute

- Bij het maken van een nieuwe Windows virtuele Machine (VM), worden de volgende fout weergegeven:

   `'Failed to start virtual machine 'vm-name'. Error: Failed to update serial output settings for VM 'vm-name'`

   De fout doet zich voor als u diagnostische gegevens over opstarten op een virtuele machine inschakelt, maar uw boot diagnostics storage-account verwijderen. U kunt dit probleem omzeilen, maakt u opnieuw de storage-account met dezelfde naam als u eerder hebt gebruikt.

<!-- 2967447 - IS, ASDK, to be fixed in 1902 -->
- De interface voor het maken van VM scale set (VMSS) biedt 7.2 CentOS gebaseerde als een optie voor implementatie. Omdat deze afbeelding niet beschikbaar op Azure Stack is, selecteer een ander besturingssysteem voor uw implementatie, of u een Azure Resource Manager-sjabloon op te geven van een andere CentOS-installatiekopie die is gedownload vóór implementatie vanuit de marketplace door de de operator.  

<!-- TBD - IS ASDK --> 
- Na het toepassen van de 1902 bijwerken, kunnen de volgende problemen optreden bij het implementeren van virtuele machines met Managed Disks:

   - Als het abonnement is gemaakt vóór de update 1808, een virtuele machine met Managed Disks kan mislukken met een interne fout. Los de fout op door deze stappen voor elk abonnement uit te voeren:
      1. Ga in de tenantportal naar **abonnementen** en zoek het abonnement. Selecteer **Resourceproviders**en selecteer vervolgens **Microsoft.Compute**, en klik vervolgens op **opnieuw registreren**.
      2. Onder hetzelfde abonnement, gaat u naar **Access Control (IAM)**, en Controleer **Azure Stack – beheerde schijf** wordt vermeld.
   - Als u een omgeving met meerdere tenants hebt geconfigureerd, kan virtuele machines implementeren in een abonnement dat is gekoppeld aan een gast-map mislukken met een interne fout. Volg deze stappen om op te lossen de fout, [in dit artikel](azure-stack-enable-multitenancy.md#registering-azure-stack-with-the-guest-directory) opnieuw configureren van elk van de Gast-mappen.

- Een Ubuntu-18.04 VM gemaakt met SSH-verificatie ingeschakeld kunt u de SSH-sleutels gebruiken om aan te melden. Als tijdelijke oplossing, gebruik van VM-toegang voor de Linux-extensie voor het implementeren van SSH-sleutels na het inrichten of verificatie op basis van wachtwoord gebruiken.

### <a name="networking"></a>Netwerken  

<!-- 3239127 - IS, ASDK -->
- In de Azure Stack-portal, wanneer u een statisch IP-adres voor een IP-configuratie die is gekoppeld aan een netwerkadapter die is gekoppeld aan een VM-exemplaar, ziet u een waarschuwingsbericht wordt weergegeven waarin wordt aangegeven 

    `The virtual machine associated with this network interface will be restarted to utilize the new private IP address...`.

    U kunt dit bericht; negeren. het IP-adres wordt gewijzigd, zelfs als het VM-exemplaar niet opnieuw wordt opgestart.

<!-- 3632798 - IS, ASDK -->
- In de portal, als u een inkomende beveiligingsregel toevoegen en selecteer **servicetag** als de bron, verschillende opties worden weergegeven in de **Brontag** lijst die niet beschikbaar voor Azure Stack. Alleen de opties die geldig in Azure Stack zijn zijn als volgt:

  - **Internet**
  - **VirtualNetwork**
  - **AzureLoadBalancer**
  
  De andere opties worden niet ondersteund als bron labels in Azure Stack. Op dezelfde manier als u een uitgaande beveiligingsregel toevoegen en selecteer **servicetag** als doel, dezelfde lijst met opties voor **Brontag** wordt weergegeven. De enige geldige opties zijn hetzelfde als voor **Brontag**, zoals beschreven in de vorige lijst.

- Netwerkbeveiligingsgroepen (nsg's) werken niet in Azure Stack in de dezelfde manier als globale Azure. In Azure, kunt u meerdere poorten instellen op een NSG-regel (met behulp van de portal, PowerShell en Resource Manager-sjablonen). In Azure Stack kan niet u echter meerdere poorten op een NSG-regel via de portal instellen. Als tijdelijke oplossing voor dit probleem, moet u een Resource Manager-sjabloon of PowerShell gebruiken om in te stellen deze aanvullende regels.

<!-- 3203799 - IS, ASDK -->
- Azure Stack biedt geen ondersteuning voor het koppelen van meer dan 4-netwerkinterfaces (NIC's) met een VM-exemplaar vandaag de dag, ongeacht de grootte van het exemplaar.

<!-- ### SQL and MySQL-->

### <a name="app-service"></a>App Service

<!-- 2352906 - IS ASDK --> 
- U moet de storage resourceprovider registreren voordat u uw eerste Azure-functie in het abonnement maakt.


<!-- ### Usage -->

 
<!-- #### Identity -->
<!-- #### Marketplace -->

## <a name="download-the-update"></a>De update downloaden

U kunt de Azure Stack 1902-update-pakket van downloaden [hier](https://aka.ms/azurestackupdatedownload). 

In verbonden scenario's alleen, Azure Stack-implementaties regelmatig controleren op een beveiligde eindpunt en automatisch een melding als een update beschikbaar voor uw cloud is. Zie voor meer informatie, [beheren van updates voor Azure Stack](azure-stack-updates.md#using-the-update-tile-to-manage-updates).

## <a name="next-steps"></a>Volgende stappen

- Zie voor een overzicht van de update management in Azure Stack, [-updates beheren in Azure Stack-overzicht](azure-stack-updates.md).  
- Zie voor meer informatie over het toepassen van updates met Azure Stack [toepassen van updates in Azure Stack](azure-stack-apply-updates.md).
- Het uitvoeren van onderhoud beleid voor geïntegreerde Azure Stack-systemen, en wat u moet doen om te voorkomen dat uw systeem in een ondersteunde status Zie [Azure Stack servicebeleid](azure-stack-servicing-policy.md).  
- Voor het gebruik van de bevoegde eindpunt (PEP) om te controleren en hervatten van updates, Zie [-updates in Azure Stack met behulp van het eindpunt van de bevoegde controleren](azure-stack-monitor-update.md).  
