---
title: Update voor Azure Stack 1811 | Microsoft Docs
description: Meer informatie over de update 1811 voor geïntegreerde Azure Stack-systemen, met inbegrip van nieuwe functies, bekende problemen en het downloaden van de update.
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
ms.date: 01/30/2019
ms.author: sethm
ms.reviewer: adepue
ms.lastreviewed: 01/25/2019
ms.openlocfilehash: c398e615e943caad40040ff1b87713304fd9f015
ms.sourcegitcommit: 947b331c4d03f79adcb45f74d275ac160c4a2e83
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/05/2019
ms.locfileid: "55746707"
---
# <a name="azure-stack-1811-update"></a>Azure Stack 1811 update

*Van toepassing op: Azure Stack-geïntegreerde systemen*

Dit artikel wordt de inhoud van het updatepakket 1811 beschreven. Het updatepakket bevat verbeteringen, problemen en nieuwe functies voor deze versie van Azure Stack. Dit artikel ook worden bekende problemen in deze release beschreven, en bevat een koppeling, zodat u kunt de update downloaden. Bekende problemen zijn onderverdeeld in problemen direct verband houden met het updateproces en problemen met de build (na de installatie).

> [!IMPORTANT]  
> Dit pakket is alleen voor geïntegreerde Azure Stack-systemen. Dit pakket niet van toepassing op de Azure Stack Development Kit.

## <a name="build-reference"></a>Naslaginformatie over bouwen

De Azure Stack 1811 update build-nummer **1.1811.0.101**.

## <a name="hotfixes"></a>Hotfixes

Azure Stack brengt hotfixes op gezette tijden. Zorg ervoor dat voor het installeren van de [meest recente Azure Stack-hotfix](#azure-stack-hotfixes) voor 1809 voordat u Azure Stack bijwerkt naar 1811.

> [!TIP]  
> Abonneer u op de volgende *RSS* of *Atom* feeds, blijven van het Azure Stack-hotfixes:
> - [RSS](https://support.microsoft.com/app/content/api/content/feeds/sap/en-us/32d322a8-acae-202d-e9a9-7371dccf381b/rss)
> - [Atom](https://support.microsoft.com/app/content/api/content/feeds/sap/en-us/32d322a8-acae-202d-e9a9-7371dccf381b/atom)

### <a name="azure-stack-hotfixes"></a>Hotfixes voor Azure Stack

- **1809**: [KB 4481548 – Azure Stack-hotfix 1.1809.12.114](https://support.microsoft.com/help/4481548/)
- **1811**: Er is geen huidige hotfix beschikbaar.

## <a name="prerequisites"></a>Vereisten

> [!IMPORTANT]
> Tijdens de installatie van de update 1811, moet u ervoor zorgen dat alle exemplaren van de beheerdersportal zijn gesloten. De gebruikersportal kan open blijven, maar de beheerportal moet worden gesloten.

- Bereid u voor uw Azure Stack-implementatie voor de host van de extensie Azure Stack. Het voorbereiden van uw systeem met behulp van de volgende richtlijnen: [Voorbereiden voor de host van de extensie voor Azure Stack](azure-stack-extension-host-prepare.md). 
 
- Installeer de [meest recente Azure Stack-hotfix](#azure-stack-hotfixes) voor 1809 voordat u bijwerkt naar 1811.

- Voordat u begint met de installatie van deze update, voert u [Test AzureStack](azure-stack-diagnostic-test.md) met de volgende parameters om te valideren van de status van uw Azure-Stack en los eventuele operationele problemen gevonden, met inbegrip van alle waarschuwingen en fouten. Ook actieve waarschuwingen bekijken en op te lossen die actie is vereist.  

    ```PowerShell
    Test-AzureStack -Include AzsControlPlane, AzsDefenderSummary, AzsHostingInfraSummary, AzsHostingInfraUtilization, AzsInfraCapacity, AzsInfraRoleSummary, AzsPortalAPISummary, AzsSFRoleSummary, AzsStampBMCSummary, AzsHostingServiceCertificates
    ```

    Als u niet de extensie host-vereisten wordt voldaan hebt, de `Test-AzureStack` uitvoer het volgende bericht weergegeven: 
  
    `To proceed with installation of the 1811 update, you will need to import 
    the SSL certificates required for Extension Host, which simplifies network 
    integration and increases the security posture of Azure Stack. Refer to this 
    link to prepare for Extension Host:
    https://docs.microsoft.com/azure/azure-stack/azure-stack-extension-host-prepare`

- De Azure Stack 1811-update is vereist dat u de verplichte uitbreiding host certificaten correct hebt geïmporteerd in uw Azure Stack-omgeving. Als u wilt doorgaan met installatie van de update 1811, moet u de SSL-certificaten vereist voor de extensie-host importeren. Zie voor het importeren van de certificaten, [in deze sectie](azure-stack-extension-host-prepare.md#import-extension-host-certificates).

    Als u elke waarschuwing negeren en nog steeds voor kiezen om de 1811-update te installeren, wordt de update mislukken in ongeveer 1 uur het volgende bericht weergegeven:   
 
    `The required SSL certificates for the Extension Host have not been found.
    The Azure Stack update will halt. Refer to this link to prepare for 
    Extension Host: https://docs.microsoft.com/azure/azure-stack/azure-stack-extension-host-prepare,
    then resume the update.
    Exception: The Certificate path does not exist: [certificate path here]` 
 
    Nadat u hebt de verplichte uitbreiding host certificaten correct geïmporteerd, kunt u de update 1811 uit de beheerdersportal kunt hervatten. Terwijl Microsoft Azure Stack-operators adviseert voor het plannen van een onderhoudsvenster tijdens het updateproces, een mislukt vanwege de ontbrekende certificaten van de extensie-host moet geen invloed op bestaande workloads of services.  

    Tijdens de installatie van deze update is de gebruikersportal van Azure Stack is niet beschikbaar terwijl de host van de extensie wordt geconfigureerd. De configuratie van de host van de extensie kan maximaal 5 uur duren. Gedurende deze tijd kunt u de status van een update controleren, of de mislukte installatie van een update hervatten met behulp van [Azure Stack Administrator PowerShell of het bevoegde eindpunt](azure-stack-monitor-update.md).

## <a name="new-features"></a>Nieuwe functies

Deze update bevat de volgende nieuwe functies en verbeteringen voor Azure Stack:

- Met deze release de [extensie host](azure-stack-extension-host-prepare.md) is ingeschakeld. De host van de extensie vereenvoudigt de integratie van netwerken en verbetert de beveiligingsstatus van Azure Stack.

- Ondersteuning toegevoegd voor verificatie van apparaten met Active Directory Federated Services (AD FS), bij het gebruik van Azure CLI in het bijzonder. Zie voor meer informatie, [gebruik API-versieprofielen met Azure CLI in Azure Stack](./user/azure-stack-version-profiles-azurecli2.md)

- Ondersteuning toegevoegd voor Service-Principals met behulp van een clientgeheim met Active Directory Federated Services (AD FS). Zie voor meer informatie, [service-principal maken voor AD FS](azure-stack-create-service-principals.md#manage-service-principal-for-ad-fs).

- Deze release voegt ondersteuning toe voor de volgende versies van de API van Azure Storage-Service: **2017-07-29**, **2017-11-09**. Ook is ondersteuning toegevoegd voor de volgende Azure Storage Resource Provider API-versies: **01-05-2016**, **2016-12-01**, **01-06-2017**, en **2017-10-01**. Zie voor meer informatie, [Azure Stack-opslag: Verschillen en overwegingen](./user/azure-stack-acs-differences.md).

- Nieuwe privileged eindpunt opdrachten toegevoegd voor het bijwerken en verwijderen van de principes van de service voor AD FS. Zie voor meer informatie, [service-principal maken voor AD FS](azure-stack-create-service-principals.md#manage-service-principal-for-ad-fs).

- Toegevoegde nieuwe Scale Unit knooppunt bewerkingen waarmee Azure Stack-operators om te starten, stoppen en afsluiten van een scale unit-knooppunt. Zie voor meer informatie, [eenheidacties dat knooppunt schalen in Azure Stack](azure-stack-node-actions.md).

- Een nieuwe eigenschappenblade voor de regio waarin de registratiedetails van de omgeving toegevoegd. U kunt deze informatie kunt weergeven door te klikken op de **regiobeheer** tegel op de standaard-dashboard in de beheerdersportal en vervolgens de optie **eigenschappen**.

- Een nieuwe privileged endpoint-opdracht voor het bijwerken van de BMC-referentie met gebruikersnaam en wachtwoord, dat wordt gebruikt om te communiceren met de fysieke machines toegevoegd. Zie voor meer informatie, [de baseboard management controller \(BMC) referentie](azure-stack-rotate-secrets.md).

- De mogelijkheid voor het Azure-roadmap echter toegang tot het pictogram help en ondersteuning (vraagteken) in de rechterbovenhoek van de beheerder en gebruiker portals, vergelijkbaar met de manier waarop die deze beschikbaar in de Azure-portal is toegevoegd.

- Toegevoegd aan een verbeterd beheer van Marketplace-ervaring voor gebruikers van niet-verbonden. Het uploadproces voor het publiceren van een Marketplace-item in een omgeving zonder verbinding is vereenvoudigd met één stap, in plaats van de installatiekopie en het pakket Marketplace afzonderlijk worden geüpload. Het geüploade product wordt ook worden weergegeven in de beheerblade van de Marketplace. Zie voor meer informatie, [in deze sectie](azure-stack-download-azure-marketplace-item.md#import-the-download-and-publish-to-azure-stack-marketplace-1811-and-higher). 

- Deze release vermindert het vereiste onderhoudsvenster voor geheime rotatie door toe te voegen van de mogelijkheid om te roteren alleen externe certificaten tijdens de [geheime rotatie van Azure Stack](azure-stack-rotate-secrets.md).

- [PowerShell voor Azure Stack](azure-stack-powershell-install.md) is bijgewerkt naar versie 1.6.0. De update biedt ondersteuning voor de nieuwe opslag-gerelateerde functies in Azure Stack. Voor meer informatie, Zie de releaseopmerkingen voor de [Azure Stack-beheermodule 1.6.0 in de PowerShell Gallery](https://www.powershellgallery.com/packages/AzureStack/1.6.0) Zie voor meer informatie over het bijwerken of installeren van PowerShell voor Azure Stack [PowerShell installeren voor Azure Stack](azure-stack-powershell-install.md).

- Beheerde schijven is nu standaard ingeschakeld tijdens het maken van virtuele machines met behulp van de Azure Stack-portal. Zie de [bekende problemen](#known-issues-post-installation) sectie voor de extra stappen die nodig is voor beheerde schijven om te voorkomen dat fouten bij het maken van virtuele machine.

- Deze versie introduceert waarschuwing **herstellen** acties voor de Azure Stack-operators. Sommige waarschuwingen in 1811 bieden een **herstellen** knop in de waarschuwing die u selecteren kunt om het probleem te verhelpen. Zie voor meer informatie, [status en waarschuwingen in Azure Stack controleren](azure-stack-monitor-health.md).

## <a name="fixed-issues"></a>Problemen opgelost

<!-- TBD - IS ASDK --> 
- Er is een waarin de openbare IP-adres meter gebruiksgegevens bleek hetzelfde probleem opgelost **datum/tijd evenement** waarde voor elke record in plaats van de **TimeDate** stempel waarin wordt weergegeven wanneer de record is gemaakt. U kunt nu deze gegevens gebruiken om uit te voeren nauwkeurige accounting van gebruik van openbare IP-adres.

<!-- 3099544 – IS, ASDK --> 
- Er is een probleem dat is opgetreden bij het maken van een nieuwe virtuele machine (VM) met behulp van de Azure Stack-portal opgelost. De VM-grootte veroorzaakt de **USD/maand** kolom om weer te geven een **niet beschikbaar** bericht. Deze kolom wordt niet meer weergegeven. prijzen kolom, dat is het weergeven van de virtuele machine wordt niet ondersteund in Azure Stack.

<!-- 2930718 - IS ASDK --> 
- Een probleem opgelost waarbij de beheerdersportal bij het openen van de details van elk gebruikersabonnement na het sluiten van de blade en te klikken op **Recent**, de naam van het abonnement werden niet weergegeven. De naam van het abonnement wordt nu weergegeven.

<!-- 3060156 - IS ASDK --> 
- Een probleem opgelost met de beheerder en de gebruiker portals: te klikken op de portal-instellingen en selecteer **verwijdert u alle instellingen en privédashboards** niet werkt zoals verwacht en er is een foutmelding weergegeven. Deze optie nu werkt correct.

<!-- 2930799 - IS ASDK --> 
- Een probleem opgelost met de beheerder en de gebruiker portals: onder **alle services**, de asset **DDoS-beschermingsplannen** ten onrechte is vermeld. Het is niet beschikbaar in Azure Stack. De aanbieding is verwijderd.
 
<!--2760466 – IS  ASDK --> 
- Er is een probleem dat is opgetreden bij het installeren van een nieuwe Azure Stack-omgeving, opgelost waarin de waarschuwing die aangeeft **activering vereist** niet is weergegeven. Nu correct wordt weergegeven.

<!--1236441 – IS  ASDK --> 
- Er is een probleem waardoor RBAC-beleidsregels toepassen op een gebruikersgroep bij het gebruik van ADFS opgelost.

<!--3463840 - IS, ASDK --> 
- Er is een probleem opgelost met infrastructuur voor back-ups mislukken vanwege een niet-toegankelijke bestandsserver vanaf het openbare VIP-netwerk. Deze oplossing wordt de back-upservice infrastructuur weer met de infrastructuur voor openbare-netwerk verplaatst. Als u de meest recente toegepast [Azure Stack-hotfix voor 1809](#azure-stack-hotfixes) die dit probleem wordt opgelost, de update 1811 gaat alle verdere wijzigingen kunt aanbrengen. 

<!-- 2967387 – IS, ASDK --> 
- Er is een probleem waarin het account waarmee u zich aanmeldt bij de portal voor Azure Stack-beheerder of gebruiker als weergegeven opgelost **onbekende gebruiker**. Dit bericht wordt weergegeven wanneer het account geen ofwel heeft een **eerste** of **laatste** naam die is opgegeven.   

<!--  2873083 - IS ASDK --> 
- Er is een probleem in welke via de portal te maken van een VM-schaalset (VMSS) veroorzaakt opgelost de **exemplaargrootte** vervolgkeuzelijst niet correct worden geladen bij het gebruik van Internet Explorer. Deze browser nu werkt correct.  

<!-- 3190553 - IS ASDK -->
- Een probleem dat ruis waarschuwingen die aangeven dat een Rolinstantie van de infrastructuur niet beschikbaar is of schaal eenheid knooppunt offline was gegenereerd is opgelost.

<!-- 2724961 - IS ASDK -->
- Fiexed een probleem waarbij de overzichtspagina van de virtuele machine kan niet correct worden weergegeven de grafiek met metrische gegevens van de virtuele machine. 

## <a name="changes"></a>Wijzigingen

- Een nieuwe manier om te bekijken en bewerken van de quota in een plan is geïntroduceerd in 1811. Zie voor meer informatie, [weergeven van een bestaande quotum](azure-stack-quota-types.md#view-an-existing-quota).

<!-- 3083238 IS -->
- Verbeteringen in deze update beveiliging leiden tot een toename van de back-grootte van de directory service-rol. Bijgewerkt met het formaat van de richtlijnen voor de locatie van de externe opslag, Zie de [documentatie over backup van infrastructuur](azure-stack-backup-reference.md#storage-location-sizing). Deze wijziging resulteert in een langere tijd voor het voltooien van de back-up vanwege de grotere grootte-gegevensoverdracht. Deze wijziging heeft gevolgen voor geïntegreerde systemen. 

- De bestaande PEP-cmdlet voor het ophalen van de BitLocker-herstelsleutel is in 1811 van Get-AzsCsvsRecoveryKeys aan Get-AzsRecoveryKeys gewijzigd. Zie voor meer informatie over het ophalen van de BitLocker-herstelsleutel [instructies over het ophalen van de sleutels](azure-stack-security-bitlocker.md).

## <a name="common-vulnerabilities-and-exposures"></a>Veelvoorkomende beveiligingsproblemen en risico 's

Deze update installeert de volgende beveiligingsupdates:  

- [CVE-2018-8256](https://portal.msrc.microsoft.com/en-US/security-guidance/advisory/CVE-2018-8256)
- [CVE-2018-8407](https://portal.msrc.microsoft.com/en-US/security-guidance/advisory/CVE-2018-8407)
- [CVE-2018-8408](https://portal.msrc.microsoft.com/en-US/security-guidance/advisory/CVE-2018-8408)
- [CVE-2018-8415](https://portal.msrc.microsoft.com/en-US/security-guidance/advisory/CVE-2018-8415)
- [CVE-2018-8417](https://portal.msrc.microsoft.com/en-US/security-guidance/advisory/CVE-2018-8417)
- [CVE-2018-8450](https://portal.msrc.microsoft.com/en-US/security-guidance/advisory/CVE-2018-8450)
- [CVE-2018-8471](https://portal.msrc.microsoft.com/en-US/security-guidance/advisory/CVE-2018-8471)
- [CVE-2018-8476](https://portal.msrc.microsoft.com/en-US/security-guidance/advisory/CVE-2018-8476)
- [CVE-2018-8485](https://portal.msrc.microsoft.com/en-US/security-guidance/advisory/CVE-2018-8485)
- [CVE-2018-8544](https://portal.msrc.microsoft.com/en-US/security-guidance/advisory/CVE-2018-8544)
- [CVE-2018-8547](https://portal.msrc.microsoft.com/en-US/security-guidance/advisory/CVE-2018-8547)
- [CVE-2018-8549](https://portal.msrc.microsoft.com/en-US/security-guidance/advisory/CVE-2018-8549)
- [CVE-2018-8550](https://portal.msrc.microsoft.com/en-US/security-guidance/advisory/CVE-2018-8550)
- [CVE-2018-8553](https://portal.msrc.microsoft.com/en-US/security-guidance/advisory/CVE-2018-8553)
- [CVE-2018-8561](https://portal.msrc.microsoft.com/en-US/security-guidance/advisory/CVE-2018-8561)
- [CVE-2018-8562](https://portal.msrc.microsoft.com/en-US/security-guidance/advisory/CVE-2018-8562)
- [CVE-2018-8565](https://portal.msrc.microsoft.com/en-US/security-guidance/advisory/CVE-2018-8565)
- [CVE-2018-8566](https://portal.msrc.microsoft.com/en-US/security-guidance/advisory/CVE-2018-8566)
- [CVE-2018-8584](https://portal.msrc.microsoft.com/en-US/security-guidance/advisory/CVE-2018-8584)

Klik op de hiervoor vermelde koppelingen voor meer informatie over deze beveiligingslekken of Zie Microsoft Knowledge Base-artikelen [4478877](https://support.microsoft.com/help/4478877).

## <a name="known-issues-with-the-update-process"></a>Bekende problemen met het updateproces

- Bij het uitvoeren van de **Get-AzureStackLog** PowerShell-cmdlet nadat het is uitgevoerd **Test AzureStack** in dezelfde sessie bevoegde eindpunt (PEP), **Get-AzureStackLog** is mislukt. U kunt dit probleem omzeilen, sluit u de PEP-sessie die u hebt uitgevoerd **Test AzureStack**, en open vervolgens een nieuwe sessie om uit te voeren **Get-AzureStackLog**.

- Tijdens de installatie van de 1811 bijwerken en zorg ervoor dat alle exemplaren van de beheerdersportal gedurende deze tijd worden afgesloten. De gebruikersportal kan open blijven, maar de beheerportal moet worden gesloten.

- Bij het uitvoeren van [Test AzureStack](azure-stack-diagnostic-test.md), als een van beide de **AzsInfraRoleSummary** of de **AzsPortalApiSummary** test mislukt, wordt u gevraagd om uit te voeren  **Test-AzureStack** met de `-Repair` vlag.  Als u deze opdracht uitvoert, mislukt het met de volgende strekking weergegeven:  `Unexpected exception getting Azure Stack health status. Cannot bind argument to parameter 'TestResult' because it is null.`  Dit probleem wordt opgelost in een toekomstige release.

- Tijdens de installatie van de update 1811, is het gebruik de Azure Stack-portal is niet beschikbaar terwijl de host van de extensie wordt geconfigureerd. De configuratie van de host van de extensie kan maximaal 5 uur duren. Gedurende deze tijd kunt u de status van een update controleren, of de mislukte installatie van een update hervatten met behulp van [Azure Stack Administrator PowerShell of het bevoegde eindpunt](azure-stack-monitor-update.md). 

- Tijdens de installatie van de update 1811 kunnen het portaldashboard van de gebruiker mogelijk niet beschikbaar, en aanpassingen verloren gaan. U kunt het dashboard terugzetten naar de standaardinstelling nadat de update is voltooid door de instellingen voor de openen en te selecteren **standaardinstellingen herstellen**.

- Bij het uitvoeren van [Test AzureStack](azure-stack-diagnostic-test.md), een waarschuwingsbericht wordt weergegeven van de Baseboard Management Controller (BMC) wordt weergegeven. U kunt deze waarschuwing negeren.

- <!-- 2468613 - IS --> Tijdens de installatie van deze update, ziet u mogelijk waarschuwingen met de titel `Error – Template for FaultType UserAccounts.New is missing.` u deze waarschuwingen kan negeren. De waarschuwingen worden automatisch gesloten na de installatie van deze update is voltooid.

- <!-- 3139614 | IS --> Als u een update hebt toegepast op Azure Stack van uw OEM, de **Update beschikbaar** melding mogelijk niet weergegeven in de Azure Stack-beheerdersportal. Voor het installeren van de Microsoft update, downloaden en importeren handmatig met behulp van de instructies die u hier [toepassen van updates in Azure Stack](azure-stack-apply-updates.md).

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

<!-- TBD - IS ASDK --> 
- Verwijderen van zwevende resources leidt van gebruiker-abonnementen. Als tijdelijke oplossing, eerst Gebruikersbronnen of de hele resourcegroep verwijderen en verwijder vervolgens de gebruikersabonnementen.

<!-- TBD - IS ASDK --> 
- U kunt machtigingen aan uw abonnement met behulp van de Azure Stack-portals niet weergeven. Als tijdelijke oplossing gebruiken [PowerShell machtigingen kan verifiëren](/powershell/module/azs.subscriptions.admin/get-azssubscriptionplan).

### <a name="health-and-monitoring"></a>Status en bewaking

<!-- 1264761 - IS ASDK -->  
- Mogelijk ziet u waarschuwingen voor de **Health controller** onderdeel waarvoor u de volgende gegevens:  

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

    Beide waarschuwingen kunnen worden genegeerd. Ze worden automatisch gesloten na verloop van tijd.  

### <a name="compute"></a>Compute

- Bij het maken van een nieuwe Windows virtuele Machine (VM), de **instellingen** blade vereist dat u een openbare binnenkomende poorten selecteren om door te gaan. In 1811, deze instelling is vereist, maar heeft geen effect. Dit komt doordat de functie is afhankelijk van Azure-Firewall, die niet is geïmplementeerd in Azure Stack. U kunt selecteren **geen openbare binnenkomende poorten**, of een van de andere opties om door te gaan met het maken van virtuele machine. De instelling heeft geen effect.

- Bij het maken van een nieuwe Windows virtuele Machine (VM), worden de volgende fout weergegeven:

   `'Failed to start virtual machine 'vm-name'. Error: Failed to update serial output settings for VM 'vm-name'`

   De fout doet zich voor als u diagnostische gegevens over opstarten op een virtuele machine inschakelt, maar uw boot diagnostics storage-account verwijderen. U kunt dit probleem omzeilen, maakt u opnieuw de storage-account met dezelfde naam als u eerder hebt gebruikt.

- Bij het maken van een [Dv2-serie VM](./user/azure-stack-vm-considerations.md#virtual-machine-sizes), D11-14v2 VM's kunnen u 4, 8, 16 en 32 gegevensschijven respectievelijk maken. Het deelvenster van de virtuele machine maken bevat, 8, 16, 32 en 64 gegevensschijven.

- Gebruiksrecords in Azure Stack kunnen onverwachte hoofdlettergebruik; bevatten bijvoorbeeld:

   `{"Microsoft.Resources":{"resourceUri":"/subscriptions/<subid>/resourceGroups/ANDREWRG/providers/Microsoft.Compute/
   virtualMachines/andrewVM0002","location":"twm","tags":"null","additionalInfo":
   "{\"ServiceType\":\"Standard_DS3_v2\",\"ImageType\":\"Windows_Server\"}"}}`

   In dit voorbeeld wordt de naam van de resourcegroep moet **AndrewRG**. U kunt deze inconsistentie negeren.

<!-- 3235634 – IS, ASDK -->
- Het implementeren van VM's met grootten met een **v2** achtervoegsel; bijvoorbeeld, **Standard_A2_v2**, geef het achtervoegsel als **Standard_A2_v2** (kleine letters v). Gebruik geen **Standard_A2_V2** (V hoofdletters). Dit werkt in de globale Azure en is een inconsistentie in Azure Stack.

<!-- 2869209 – IS, ASDK --> 
- Wanneer u de [ **toevoegen AzsPlatformImage** cmdlet](/powershell/module/azs.compute.admin/add-azsplatformimage), moet u de **- OsUri** parameter als het opslagaccount URI waar de schijf is geüpload. Als u het lokale pad van de schijf gebruikt, wordt de cmdlet mislukt met de volgende fout: 

    `Long running operation failed with status 'Failed'`

<!--  2795678 – IS, ASDK --> 
- Wanneer u de portal gebruiken om u te maken van virtuele machines (VM's) in een premium VM-grootte (DS, Ds_v2, FS, FSv2), wordt de virtuele machine gemaakt in een standard storage-account. Het maken van in een standard storage-account heeft geen invloed op functioneel, IOP's, of facturering. U kunt de waarschuwing dat negeren: 

    `You've chosen to use a standard disk on a size that supports premium disks. This could impact operating system performance and is not recommended. Consider using premium storage (SSD) instead.`

<!-- 2967447 - IS, ASDK --> 
- De interface voor het maken van VM scale set (VMSS) biedt 7.2 CentOS gebaseerde als een optie voor implementatie. Omdat deze afbeelding niet beschikbaar op Azure Stack is, selecteer een ander besturingssysteem voor uw implementatie, of u een Azure Resource Manager-sjabloon op te geven van een andere CentOS-installatiekopie die is gedownload vóór implementatie vanuit de marketplace door de de operator.  

<!-- 2724873 - IS --> 
- Bij het gebruik van de PowerShell-cmdlets **Start AzsScaleUnitNode** of **Stop-AzsScaleunitNode** voor het beheren van schaaleenheden, de eerste poging om te starten of stoppen van de schaaleenheid kan mislukken. Als de cmdlet voor de eerste keer uitvoert mislukt, wordt de cmdlet nogmaals uitvoeren. De tweede uitvoering moet de bewerking is voltooid. 

<!-- TBD - IS ASDK --> 
- Als een uitbreiding op een VM-implementatie de inrichting te lang duurt, kunt u de time-out van de inrichting in plaats van bij stoppen van de procedure voor de toewijzing ongedaan of verwijder de virtuele machine.  

<!-- 1662991 IS ASDK --> 
- Diagnostische gegevens over Linux-VM wordt niet ondersteund in Azure Stack. Wanneer u een Linux-VM met VM diagnostics is ingeschakeld implementeren, wordt de implementatie mislukt. De implementatie mislukt ook als u de Linux-VM eenvoudige metrische gegevens via diagnostische instellingen inschakelen.  

<!-- 3507629 - IS, ASDK --> 
- Beheerde schijven maakt twee nieuwe [quotatypen compute](azure-stack-quota-types.md#compute-quota-types) om te beperken van de maximale capaciteit van beheerde schijven die kunnen worden ingericht. Standaard is 2048 GiB toegewezen voor elke quotumtype beheerde schijven. U kunt echter de volgende problemen optreden:

   - Voor de quota die zijn gemaakt vóór de update 1808, weergegeven het quotum voor Managed Disks 0 waarden in de beheerdersportal, hoewel 2048 GiB is toegewezen. U kunt vergroten of verkleinen van de waarde die is gebaseerd op de behoeften van uw werkelijke en de nieuwe ingestelde quotawaarde overschrijft de 2048 GiB-standaard.
   - Als u bijwerkt naar de quotawaarde op 0, is het equivalent zijn aan de standaardwaarde van 2048 GiB. Als tijdelijke oplossing, stelt u de quotawaarde in op 1.

<!-- TBD - IS ASDK --> 
- Na het toepassen van de 1811 bijwerken, kunnen de volgende problemen optreden bij het implementeren van virtuele machines met Managed Disks:

   - Als het abonnement is gemaakt vóór de update 1808, een virtuele machine met Managed Disks kan mislukken met een interne fout. Los de fout op door deze stappen voor elk abonnement uit te voeren:
      1. Ga in de tenantportal naar **abonnementen** en zoek het abonnement. Selecteer **Resourceproviders**en selecteer vervolgens **Microsoft.Compute**, en klik vervolgens op **opnieuw registreren**.
      2. Onder hetzelfde abonnement, gaat u naar **Access Control (IAM)**, en controleer de **AzureStack-DiskRP-Client** rol wordt weergegeven.
   - Als u een omgeving met meerdere tenants hebt geconfigureerd, kan virtuele machines implementeren in een abonnement dat is gekoppeld aan een gast-map mislukken met een interne fout. Volg deze stappen om op te lossen de fout, [in dit artikel](azure-stack-enable-multitenancy.md#registering-azure-stack-with-the-guest-directory) opnieuw configureren van elk van de Gast-mappen.

- Een Ubuntu-18.04 VM gemaakt met SSH-verificatie ingeschakeld kunt u de SSH-sleutels gebruiken om aan te melden. Als tijdelijke oplossing, gebruik van VM-toegang voor de Linux-extensie voor het implementeren van SSH-sleutels na het inrichten of verificatie op basis van wachtwoord gebruiken.

### <a name="networking"></a>Netwerken  

<!-- 1766332 - IS ASDK --> 
- Onder **netwerken**, als u klikt op **VPN-Gateway maken** voor het instellen van een VPN-verbinding, **op basis van beleid** wordt vermeld als een VPN-type. Selecteer deze optie niet. Alleen de **Route op basis van** optie wordt ondersteund in Azure Stack.

<!-- 1902460 - IS ASDK --> 
- Azure Stack biedt ondersteuning voor een enkel *lokale netwerkgateway* per IP-adres. Dit geldt voor alle tenant-abonnementen. Nadat het maken van de eerste lokale gateway netwerkverbinding, volgende pogingen tot het maken van een resource van een lokale netwerkgateway met hetzelfde IP-adres worden geweigerd.

<!-- 16309153 - IS ASDK --> 
- In een virtueel netwerk dat is gemaakt met een DNS-serverinstellingen van **automatische**, wijzigen in een aangepaste DNS-server is mislukt. De bijgewerkte instellingen worden niet gepusht naar virtuele machines in dit Vnet.

<!-- 2529607 - IS ASDK --> 
- Tijdens de Azure Stack *geheim rotatie*, er is een periode waarin openbare IP-adressen niet bereikbaar voor twee tot vijf minuten zijn.

<!-- 2664148 - IS ASDK --> 
-   Kunnen optreden in scenario's waar de tenant er toegang virtuele machines tot heeft met behulp van een S2S-VPN-tunnel, een scenario waarbij verbindingspogingen mislukt als de on-premises-subnet is toegevoegd aan de lokale netwerkgateway nadat de gateway is al gemaakt. 

- In de Azure Stack-portal, wanneer u een statisch IP-adres voor een IP-configuratie die is gekoppeld aan een netwerkadapter die is gekoppeld aan een VM-exemplaar, ziet u een waarschuwingsbericht wordt weergegeven waarin wordt aangegeven 

    `The virtual machine associated with this network interface will be restarted to utilize the new private IP address...`. 

    U kunt dit bericht; negeren. het IP-adres wordt gewijzigd, zelfs als het VM-exemplaar niet opnieuw wordt opgestart.

- In de portal op de **netwerken eigenschappen** blade er is een koppeling voor **effectieve beveiligingsregels** voor elke netwerkadapter. Als u deze koppeling selecteert, een nieuwe blade geopend waarin het foutbericht `Not Found.` deze fout treedt op omdat Azure Stack biedt nog geen ondersteuning voor **effectieve beveiligingsregels**.

- In de portal, als u een inkomende beveiligingsregel toevoegen en selecteer **servicetag** als de bron, verschillende opties worden weergegeven in de **Brontag** lijst die niet beschikbaar voor Azure Stack. Alleen de opties die geldig in Azure Stack zijn zijn als volgt:

    - **Internet**
    - **VirtualNetwork**
    - **AzureLoadBalancer**
  
    De andere opties worden niet ondersteund als bron labels in Azure Stack. Op dezelfde manier als u een uitgaande beveiligingsregel toevoegen en selecteer **servicetag** als doel, dezelfde lijst met opties voor **Brontag** wordt weergegeven. De enige geldige opties zijn hetzelfde als voor **Brontag**, zoals beschreven in de vorige lijst.

- De **New-AzureRmIpSecPolicy** instelling biedt geen ondersteuning voor PowerShell-cmdlet **DHGroup24** voor de `DHGroup` parameter.

- Netwerkbeveiligingsgroepen (nsg's) werken niet in Azure Stack in de dezelfde manier als globale Azure. In Azure, kunt u meerdere poorten instellen op een NSG-regel (met behulp van de portal, PowerShell en Resource Manager-sjablonen). In Azure Stack, kunt u meerdere poorten niet instellen op een NSG-regel via de portal. Gebruik Resource Manager-sjabloon deze extra regels instellen om dit probleem omzeilen.

### <a name="infrastructure-backup"></a>Infrastructuur voor back-up

<!--scheduler config lost, bug 3615401, new issue in 1811,  hectorl-->
<!-- TSG: https://www.csssupportwiki.com/index.php/Azure_Stack/KI/Backup_scheduler_configuration_lost --> 
- Na het inschakelen van automatische back-ups, gaat de scheduler-service onverwacht naar uitgeschakelde status. De back-controllerservice detecteert dat de automatische back-ups zijn uitgeschakeld en een waarschuwing in de beheerdersportal. Deze waarschuwing wordt verwacht wanneer automatische back-ups zijn uitgeschakeld. 
    - Oorzaak: Dit probleem is vanwege een fout in de service die in gegevensverlies van scheduler-configuratie resulteert. Deze fout verandert niet de opslaglocatie, gebruikersnaam, wachtwoord of versleutelingssleutel.   
    - Herstel: Risico's te beperken, open de blade back-controller-instellingen in de infrastructuur voor back-resourceprovider en selecteer **inschakelen automatische back-ups**. Zorg ervoor dat de gewenste frequentie en retentie periode instellen.
    - Exemplaar: Laag 

<!-- ### SQL and MySQL-->

### <a name="app-service"></a>App Service

<!-- 2352906 - IS ASDK --> 
- U moet de storage resourceprovider registreren voordat u uw eerste Azure-functie in het abonnement maakt.


<!-- ### Usage -->

 
<!-- #### Identity -->
<!-- #### Marketplace -->


## <a name="download-the-update"></a>De update downloaden

U kunt de Azure Stack 1811 update-pakket van downloaden [hier](https://aka.ms/azurestackupdatedownload). 

In verbonden scenario's alleen, Azure Stack-implementaties regelmatig controleren op een beveiligde eindpunt en automatisch een melding als een update beschikbaar voor uw cloud is. Zie voor meer informatie, [beheren van updates voor Azure Stack](azure-stack-updates.md#using-the-update-tile-to-manage-updates).

## <a name="next-steps"></a>Volgende stappen

- Het uitvoeren van onderhoud beleid voor geïntegreerde Azure Stack-systemen, en wat u moet doen om te voorkomen dat uw systeem in een ondersteunde status Zie [Azure Stack servicebeleid](azure-stack-servicing-policy.md).  
- Voor het gebruik van de bevoegde eindpunt (PEP) om te controleren en hervatten van updates, Zie [-updates in Azure Stack met behulp van het eindpunt van de bevoegde controleren](azure-stack-monitor-update.md).  
- Zie voor een overzicht van de update management in Azure Stack, [-updates beheren in Azure Stack-overzicht](azure-stack-updates.md).  
- Zie voor meer informatie over het toepassen van updates met Azure Stack [toepassen van updates in Azure Stack](azure-stack-apply-updates.md).  
