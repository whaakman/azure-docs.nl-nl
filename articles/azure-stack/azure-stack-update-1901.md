---
title: Update voor Azure Stack 1901 | Microsoft Docs
description: Meer informatie over de update 1901 voor geïntegreerde Azure Stack-systemen, met inbegrip van nieuwe functies, bekende problemen en het downloaden van de update.
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
ms.date: 02/28/2019
ms.author: sethm
ms.reviewer: adepue
ms.lastreviewed: 02/09/2019
ms.openlocfilehash: 18c4fd3fff244ac180ac0129f100a7d5b2472cab
ms.sourcegitcommit: 1afd2e835dd507259cf7bb798b1b130adbb21840
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/28/2019
ms.locfileid: "56984829"
---
# <a name="azure-stack-1901-update"></a>Azure Stack 1901 update

*Van toepassing op: Azure Stack-geïntegreerde systemen*

Dit artikel wordt de inhoud van het updatepakket 1901 beschreven. De update bevat verbeteringen, oplossingen en nieuwe functies voor deze versie van Azure Stack. Dit artikel ook worden bekende problemen in deze release beschreven, en bevat een koppeling om de update te downloaden. Bekende problemen zijn onderverdeeld in problemen direct verband houden met het updateproces en problemen met de build (na de installatie).

> [!IMPORTANT]  
> Dit pakket is alleen voor geïntegreerde Azure Stack-systemen. Dit pakket niet van toepassing op de Azure Stack Development Kit.

## <a name="build-reference"></a>Naslaginformatie over bouwen

De Azure Stack 1901 update build-nummer **1.1901.0.95** of **1.1901.0.99** na 26 februari 2019. Zie de opmerking hieronder:

> [!IMPORTANT]  
> Microsoft heeft een probleem die gevolgen kan hebben dat een 1901 klanten bijwerken van 1811 (1.1811.0.101) gedetecteerd en heeft een bijgewerkte 1901 pakket om het probleem op te lossen: 1.1901.0.99, bijgewerkt van 1.1901.0.95 bouwen. Klanten die al hebt bijgewerkt naar 1.1901.0.95 hoeft geen verdere actie te ondernemen.
>
> Verbonden klanten die zich op 1811 ziet automatisch het nieuwe 1901 (1.1901.0.99)-pakket beschikbaar zijn in de beheerdersportal en moeten installeren wanneer u klaar bent. Niet-verbonden klanten kunnen downloaden en importeren van het nieuwe 1901-pakket met behulp van hetzelfde proces [die hier worden beschreven](azure-stack-apply-updates.md).
>
> Klanten met beide versies van 1901 heeft geen invloed bij het installeren van de volgende volledige of de hotfix-pakket.

## <a name="hotfixes"></a>Hotfixes

Azure Stack brengt hotfixes op gezette tijden. Zorg ervoor dat voor het installeren van de [meest recente Azure Stack-hotfix](#azure-stack-hotfixes) voor 1811 voordat u Azure Stack bijwerkt naar 1901.

Azure Stack-hotfixes zijn alleen van toepassing op Azure Stack-geïntegreerde systemen; Probeer niet te installeren van hotfixes in de ASDK.

> [!TIP]  
> Abonneer u op de volgende *RSS* of *Atom* feeds, blijven van het Azure Stack-hotfixes:
> - [RSS](https://support.microsoft.com/app/content/api/content/feeds/sap/en-us/32d322a8-acae-202d-e9a9-7371dccf381b/rss)
> - [Atom](https://support.microsoft.com/app/content/api/content/feeds/sap/en-us/32d322a8-acae-202d-e9a9-7371dccf381b/atom)

### <a name="azure-stack-hotfixes"></a>Hotfixes voor Azure Stack

- **1809**: [KB 4481548 – Azure Stack-hotfix 1.1809.12.114](https://support.microsoft.com/help/4481548/)
- **1811**: Er is geen huidige hotfix beschikbaar.
- **1901**: Er is geen huidige hotfix beschikbaar.

## <a name="prerequisites"></a>Vereisten

> [!IMPORTANT]
- Installeer de [meest recente Azure Stack-hotfix](#azure-stack-hotfixes) voor 1811 (indien aanwezig) voordat u bijwerkt naar 1901.

- Voordat u begint met de installatie van deze update, voert u [Test AzureStack](azure-stack-diagnostic-test.md) met de volgende parameters om te valideren van de status van uw Azure-Stack en los eventuele operationele problemen gevonden, met inbegrip van alle waarschuwingen en fouten. Ook actieve waarschuwingen bekijken en op te lossen die actie is vereist:

    ```PowerShell
    Test-AzureStack -Include AzsControlPlane, AzsDefenderSummary, AzsHostingInfraSummary, AzsHostingInfraUtilization, AzsInfraCapacity, AzsInfraRoleSummary, AzsPortalAPISummary, AzsSFRoleSummary, AzsStampBMCSummary, AzsHostingServiceCertificates
    ```

- Wanneer Azure Stack wordt beheerd door System Center Operations Manager (SCOM), moet u het Management Pack voor Microsoft Azure Stack bijwerken naar versie 10.0.3.11 voordat u 1901 toepast.

## <a name="new-features"></a>Nieuwe functies

Deze update bevat de volgende nieuwe functies en verbeteringen voor Azure Stack:

- Beheerde installatiekopieën op Azure Stack inschakelen u een beheerde installatiekopie-object maken op een gegeneraliseerde VM (zowel niet-beheerde en beheerde) die u alleen van beheerde maken kunt schijf voortaan VM's. Zie voor meer informatie, [Azure Stack Managed Disks](user/azure-stack-managed-disk-considerations.md#managed-images).

- **AzureRm 2.4.0**
   * **AzureRm.Profile**  
         Foutoplossing: `Import-AzureRmContext` correct deserialiseren van de opgeslagen-token.  
   * **AzureRm.Resources**  
         Foutoplossing: `Get-AzureRmResource` aan query aanvraag insensitively op resourcetype.  
   * **Azure.Storage**  
         Rollup-AzureRm-module bevat nu de al gepubliceerde versie 4.5.0 ondersteunen de **api-versie 2017-07-29**.  
   * **AzureRm.Storage**  
         Rollup-AzureRm-module bevat nu de al gepubliceerde versie 5.0.4 ondersteunen de **api-versie 2017-10-01**.  
   * **AzureRm.Compute**  
         Hiermee stelt u eenvoudige parameter toegevoegd in `New-AzureRMVM` en `NewAzureRMVMSS`, `-ImageName` parameter ondersteunt het opgeven van installatiekopieën die gebruiker.  
   * **AzureRm.Insights**  
         Rollup-AzureRm-module bevat nu de al gepubliceerde versie 5.1.5 ondersteunen de **api-versie 2018-01-01** voor metrische gegevens, metrische definities van de resourcetypen.

- **AzureStack 1.7.0** dit een belangrijke wijziging release. Raadpleeg voor meer informatie over de wijzigingen die fouten veroorzaken https://aka.ms/azspshmigration170
   * **Azs.Backup.Admin-Module**  
         Belangrijke wijziging: Back-up gewijzigd zodat de versleutelingsmodus op basis van een certificaat wordt gebruikt. Ondersteuning voor symmetrische sleutels is afgeschaft.  
   * **Azs.Fabric.Admin-Module**  
         `Get-AzsInfrastructureVolume` is afgeschaft. Gebruik de nieuwe cmdlet `Get-AzsVolume`.  
         `Get-AzsStorageSystem` is afgeschaft.  Gebruik de nieuwe cmdlet `Get-AzsStorageSubSystem`.  
         `Get-AzsStoragePool` is afgeschaft. De `StorageSubSystem` -object bevat de eigenschap capaciteit.  
   * **Azs.Compute.Admin-Module**  
         Foutoplossing: `Add-AzsPlatformImage`, `Get-AzsPlatformImage`: Aanroepen van `ConvertTo-PlatformImageObject` alleen in de successen.  
         BugFix - `Add-AzsVmExtension`, `Get-AzsVmExtension`: Aanroepen van ConvertTo-VmExtensionObject alleen in de successen.  
   * **Azs.Storage.Admin-Module**  
         Opgelost probleem - quotum voor het nieuwe opslag gebruikt de standaardinstellingen als er geen opgegeven.

De verwijzing voor de bijgewerkte modules Zie [Azure Stack-Moduleverwijzing](https://docs.microsoft.com/powershell/azure/azure-stack/overview?view=azurestackps-1.6.0&viewFallbackFrom=azurestackps-1.7.0).

## <a name="fixed-issues"></a>Problemen opgelost

- Er is een probleem waarbij de portal hebt u geleerd een optie voor het maken van op beleid gebaseerde VPN-gateways, die niet worden ondersteund in Azure Stack opgelost. Deze optie is verwijderd uit de portal.

<!-- 16523695 – IS, ASDK -->
- Een probleem opgelost waarbij na het bijwerken van de DNS-instellingen voor het Virtueelnetwerk van **gebruik Azure Stack DNS** naar **aangepaste DNS**, de exemplaren niet zijn bijgewerkt met de nieuwe instelling.

- <!-- 3235634 – IS, ASDK --> Een probleem opgelost in welke implementeren VM's met grootten met een **v2** achtervoegsel; bijvoorbeeld, **Standard_A2_v2**, vereiste op te geven het achtervoegsel als **Standard_A2_v2** () kleine letters v). Als met globale Azure, kunt u nu gebruiken **Standard_A2_V2** (V hoofdletters).

<!-- 2869209 – IS, ASDK --> 
- Er is een probleem opgelost bij het gebruik van de [cmdlet Add-AzsPlatformImage](/powershell/module/azs.compute.admin/add-azsplatformimage), in die u moest gebruiken de **- OsUri** parameter als het opslagaccount URI waar de schijf is geüpload. U kunt nu het lokale pad naar de schijf ook gebruiken.

<!--  2795678 – IS, ASDK --> 
- Er is een probleem dat een waarschuwing gegenereerd wanneer u de portal gebruikt voor het maken van virtuele machines (VM's) in een premium VM-grootte (DS, Ds_v2, FS, FSv2) opgelost. De virtuele machine is gemaakt in een standard storage-account. Hoewel dit heeft geen invloed op functioneel, IOP's of facturering, is de waarschuwing opgelost.

<!-- 1264761 - IS ASDK -->  
- Er is een probleem met opgelost de **Health controller** onderdeel dat is de volgende waarschuwingen worden gegenereerd. De waarschuwingen kunnen veilig worden genegeerd:

    - Waarschuwing #1:
       - NAAM:  De functie van de infrastructuur is niet in orde
       - ERNST: Waarschuwing
       - COMPONENT: De gezondheid van controller
       - BESCHRIJVING: De health-controller Heartbeat-Scanner is niet beschikbaar. Dit kan invloed hebben op statusrapporten en metrische gegevens.  

    - Waarschuwing #2:
       - NAAM:  De functie van de infrastructuur is niet in orde
       - ERNST: Waarschuwing
       - COMPONENT: De gezondheid van controller
       - BESCHRIJVING: De health-controller fouttolerantie Scanner is niet beschikbaar. Dit kan invloed hebben op statusrapporten en metrische gegevens.


<!-- 3507629 - IS, ASDK --> 
- Er is een probleem opgelost bij het instellen van de waarde van Managed Disks quota onder [quotatypen compute](azure-stack-quota-types.md#compute-quota-types) op 0, dit is gelijk aan de standaardwaarde van 2048 GiB. De quotawaarde van nul nu in acht wordt genomen.

<!-- 2724873 - IS --> 
- Er is een probleem opgelost bij het gebruik van de PowerShell-cmdlets **Start AzsScaleUnitNode** of **Stop-AzsScaleUnitNode** voor het beheren van schaaleenheden, waarin u de eerste poging om te starten of stoppen van de schaaleenheid mislukken.

<!-- 2724961- IS ASDK --> 
- Er is een probleem in die u hebt geregistreerd opgelost de **Microsoft.Insight** resourceprovider op de instellingen van het abonnement en een Windows-VM gemaakt met gast OS diagnostische ingeschakeld, maar de grafiek CPU-Percentage in de overzichtspagina van virtuele machine is niet meer weergeven metrische gegevens. De gegevens nu weergegeven correct.

- Een probleem opgelost in die wordt uitgevoerd de **Get-AzureStackLog** -cmdlet kan niet worden voltooid nadat u **Test AzureStack** in dezelfde sessie bevoegde eindpunt (PEP). U kunt nu dezelfde PEP sessie waarin u uitgevoerd **Test AzureStack**.

<!-- bug 3615401, IS -->
- Probleem opgelost met automatische back-ups waarbij de scheduler-service wilt gaan naar uitgeschakeld onverwacht afgesloten. 

<!--2850083, IS ASDK -->
- Verwijderd de **Gateway opnieuw instellen van** knop van de Azure Stack-portal, dit heeft een fout geretourneerd als de knop is geklikt. Deze knop dient geen functie in Azure Stack, zoals Azure Stack is een multitenant-gateway in plaats van toegewezen VM-exemplaren voor elke tenant VPN-Gateway, zodat deze is verwijderd om verwarring te voorkomen. 

<!-- 3209594, IS ASDK -->
- Verwijderd de **effectieve beveiligingsregels** koppelen vanuit de **netwerken eigenschappen** blade als deze functie wordt niet ondersteund in Azure Stack. Met de koppeling die aanwezig zijn, heeft de indruk dat deze functie wordt ondersteund, maar werkt niet. Om te verlichten verwarring, we hebben de koppeling verwijderd.

<!-- 3139614 | IS -->
- Een probleem waarbij opgelost nadat een update is toegepast op Azure Stack van een OEM, de **Update beschikbaar** melding werden niet weergegeven in de Azure Stack-beheerdersportal.

## <a name="changes"></a>Wijzigingen

<!-- 3083238 IS -->
- Verbeteringen in deze update beveiliging leiden tot een toename van de back-grootte van de directory service-rol. Bijgewerkt met het formaat van de richtlijnen voor de locatie van de externe opslag, Zie de [documentatie over backup van infrastructuur](azure-stack-backup-reference.md#storage-location-sizing). Deze wijziging resulteert in een langere tijd voor het voltooien van de back-up vanwege de grotere grootte-gegevensoverdracht. Deze wijziging heeft gevolgen voor geïntegreerde systemen. 

- Vanaf januari 2019, kunt u Kubernetes-clusters op Active Directory Federated Services (AD FS) is geregistreerd, verbonden Azure Stack stempels (toegang tot internet is vereist) implementeren. Volg de instructies [hier](azure-stack-solution-template-kubernetes-cluster-add.md) voor het downloaden van het nieuwe Kubernetes Marketplace-item. Volg de instructies [hier](user/azure-stack-solution-template-kubernetes-adfs.md) een Kubernetes-cluster te implementeren. Houd er rekening mee de nieuwe parameters voor die aangeeft of het doelsysteem toevoegen of AD FS is geregistreerd. Als het AD FS, worden nieuwe velden beschikbaar zijn voor het invoeren van de parameters van de Key Vault waarin de implementatie-certificaat wordt opgeslagen.

   Houd er rekening mee dat zelfs met AD FS-ondersteuning, de implementatie van Kubernetes-clusters is internettoegang vereist.

- Na de installatie van updates of hotfixes met Azure Stack, mag u nieuwe functies geïntroduceerd die moeten worden toegekend aan een of meer toepassingen met identiteiten op nieuwe machtigingen vereist. Deze machtigingen vereist beheerderstoegang tot de basismap en zodat deze automatisch kan niet worden uitgevoerd. Bijvoorbeeld:

   ```powershell
   $adminResourceManagerEndpoint = "https://adminmanagement.<region>.<domain>"
   $homeDirectoryTenantName = "<homeDirectoryTenant>.onmicrosoft.com" # This is the primary tenant Azure Stack is registered to

   Update-AzsHomeDirectoryTenant -AdminResourceManagerEndpoint $adminResourceManagerEndpoint `
     -DirectoryTenantName $homeDirectoryTenantName -Verbose
   ```

- Er is een nieuwe overweging voor het plannen van capaciteit van de Azure Stack nauwkeurig. Met de update 1901 is er nu een limiet voor het totale aantal virtuele Machines dat kan worden gemaakt.  Deze limiet is bedoeld als tijdelijke om te voorkomen dat instabiliteit van de oplossing. De bron van het stabiliteitsprobleem met de op grotere aantallen virtuele machines wordt behandeld, maar een specifieke tijdlijn voor herstel nog niet is vastgesteld. Met de update 1901, is er nu een per Serverlimiet van 60 virtuele machines met een limiet totale oplossing van 700.  Een virtuele machine in Azure Stack-limiet van 8 server zou bijvoorbeeld 480 (8 * 60).  Voor een server 12 tot 16 Azure Stack-oplossing is de limiet 700. Deze limiet is gemaakt blijven alle rekencapaciteit capaciteitsoverwegingen in gedachten, zoals de tolerantie-reserve en de CPU virtueel naar fysieke hoogte-breedteverhouding die een operator wilt behouden op het stempel. Zie de nieuwe versie van de Capaciteitsplanner voor meer informatie.  
In het geval dat de VM scale limiet is bereikt, zou de volgende foutcodes als gevolg hiervan worden geretourneerd: VMsPerScaleUnitLimitExceeded, VMsPerScaleUnitNodeLimitExceeded. 
 

- De Compute-API-versie is nu verhoogd naar 2017-12-01.

- Infrastructuur voor back-up is nu een certificaat vereist met alleen een openbare sleutel (. CER) voor het versleutelen van back-upgegevens. Belangrijke ondersteuning voor symmetrische codering is afgeschaft vanaf 1901. Als back-up van de infrastructuur is geconfigureerd voordat u bijwerkt naar 1901, blijft de versleutelingssleutels op locatie. Hebt u ten minste 2 informatie bijgewerkt met achterwaartse compatibiliteit met ondersteuning voor back-instellingen bijwerken. Zie voor meer informatie, [back-best practices voor Azure Stack-infrastructuur](azure-stack-backup-best-practices.md).

## <a name="common-vulnerabilities-and-exposures"></a>Veelvoorkomende beveiligingsproblemen en risico 's

Deze update installeert de volgende beveiligingsupdates:  

- [CVE-2018-8477](https://portal.msrc.microsoft.com/en-us/security-guidance/advisory/CVE-2018-8477)
- [CVE-2018-8514](https://portal.msrc.microsoft.com/en-us/security-guidance/advisory/CVE-2018-8514)
- [CVE-2018-8580](https://portal.msrc.microsoft.com/en-us/security-guidance/advisory/CVE-2018-8580)
- [CVE-2018-8595](https://portal.msrc.microsoft.com/en-us/security-guidance/advisory/CVE-2018-8595)
- [CVE-2018-8596](https://portal.msrc.microsoft.com/en-us/security-guidance/advisory/CVE-2018-8596)
- [CVE-2018-8598](https://portal.msrc.microsoft.com/en-us/security-guidance/advisory/CVE-2018-8598)
- [CVE-2018-8621](https://portal.msrc.microsoft.com/en-us/security-guidance/advisory/CVE-2018-8621)
- [CVE-2018-8622](https://portal.msrc.microsoft.com/en-us/security-guidance/advisory/CVE-2018-8622)
- [CVE-2018-8627](https://portal.msrc.microsoft.com/en-us/security-guidance/advisory/CVE-2018-8627)
- [CVE-2018-8637](https://portal.msrc.microsoft.com/en-us/security-guidance/advisory/CVE-2018-8637)
- [CVE-2018-8638](https://portal.msrc.microsoft.com/en-us/security-guidance/advisory/CVE-2018-8638)
- [ADV190001](https://portal.msrc.microsoft.com/en-us/security-guidance/advisory/ADV190001)
- [CVE-2019-0536](https://portal.msrc.microsoft.com/en-us/security-guidance/advisory/CVE-2019-0536)
- [CVE-2019-0537](https://portal.msrc.microsoft.com/en-us/security-guidance/advisory/CVE-2019-0537)
- [CVE-2019-0545](https://portal.msrc.microsoft.com/en-us/security-guidance/advisory/CVE-2019-0545)
- [CVE-2019-0549](https://portal.msrc.microsoft.com/en-us/security-guidance/advisory/CVE-2019-0549)
- [CVE-2019-0553](https://portal.msrc.microsoft.com/en-us/security-guidance/advisory/CVE-2019-0553)
- [CVE-2019-0554](https://portal.msrc.microsoft.com/en-us/security-guidance/advisory/CVE-2019-0554)
- [CVE-2019-0559](https://portal.msrc.microsoft.com/en-us/security-guidance/advisory/CVE-2019-0559)
- [CVE-2019-0560](https://portal.msrc.microsoft.com/en-us/security-guidance/advisory/CVE-2019-0560)
- [CVE-2019-0561](https://portal.msrc.microsoft.com/en-us/security-guidance/advisory/CVE-2019-0561)
- [CVE-2019-0569](https://portal.msrc.microsoft.com/en-us/security-guidance/advisory/CVE-2019-0569)
- [CVE-2019-0585](https://portal.msrc.microsoft.com/en-us/security-guidance/advisory/CVE-2019-0585)
- [CVE-2019-0588](https://portal.msrc.microsoft.com/en-us/security-guidance/advisory/CVE-2019-0588)


Klik op de hiervoor vermelde koppelingen voor meer informatie over deze beveiligingslekken of Zie Microsoft Knowledge Base-artikelen [4480977](https://support.microsoft.com/en-us/help/4480977).

## <a name="known-issues-with-the-update-process"></a>Bekende problemen met het updateproces

- Bij het uitvoeren van [Test AzureStack](azure-stack-diagnostic-test.md), als een van beide de **AzsInfraRoleSummary** of de **AzsPortalApiSummary** test mislukt, wordt u gevraagd om uit te voeren  **Test-AzureStack** met de `-Repair` vlag.  Als u deze opdracht uitvoert, mislukt het met de volgende strekking weergegeven:  `Unexpected exception getting Azure Stack health status. Cannot bind argument to parameter 'TestResult' because it is null.`

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
- Na het toepassen van de 1901 bijwerken, kunnen de volgende problemen optreden bij het implementeren van virtuele machines met Managed Disks:

   - Als het abonnement is gemaakt vóór de update 1808, een virtuele machine met Managed Disks kan mislukken met een interne fout. Los de fout op door deze stappen voor elk abonnement uit te voeren:
      1. Ga in de tenantportal naar **abonnementen** en zoek het abonnement. Selecteer **Resourceproviders**en selecteer vervolgens **Microsoft.Compute**, en klik vervolgens op **opnieuw registreren**.
      2. Onder hetzelfde abonnement, gaat u naar **Access Control (IAM)**, en Controleer **AzureStack-DiskRP-Client** wordt vermeld.
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
- Azure Stack biedt geen ondersteuning voor het koppelen van meer dan 4-netwerkinterfaces (NIC's) op een VM-exemplaren vandaag de dag, ongeacht de grootte van het exemplaar.

<!-- ### SQL and MySQL-->

### <a name="app-service"></a>App Service

<!-- 2352906 - IS ASDK --> 
- U moet de storage resourceprovider registreren voordat u uw eerste Azure-functie in het abonnement maakt.


<!-- ### Usage -->

 
<!-- #### Identity -->
<!-- #### Marketplace -->

## <a name="download-the-update"></a>De update downloaden

U kunt de Azure Stack 1901 update-pakket van downloaden [hier](https://aka.ms/azurestackupdatedownload). 

In verbonden scenario's alleen, Azure Stack-implementaties regelmatig controleren op een beveiligde eindpunt en automatisch een melding als een update beschikbaar voor uw cloud is. Zie voor meer informatie, [beheren van updates voor Azure Stack](azure-stack-updates.md#using-the-update-tile-to-manage-updates).

## <a name="next-steps"></a>Volgende stappen

- Zie voor een overzicht van de update management in Azure Stack, [-updates beheren in Azure Stack-overzicht](azure-stack-updates.md).  
- Zie voor meer informatie over het toepassen van updates met Azure Stack [toepassen van updates in Azure Stack](azure-stack-apply-updates.md).
- Het uitvoeren van onderhoud beleid voor geïntegreerde Azure Stack-systemen, en wat u moet doen om te voorkomen dat uw systeem in een ondersteunde status Zie [Azure Stack servicebeleid](azure-stack-servicing-policy.md).  
- Voor het gebruik van de bevoegde eindpunt (PEP) om te controleren en hervatten van updates, Zie [-updates in Azure Stack met behulp van het eindpunt van de bevoegde controleren](azure-stack-monitor-update.md).  
