---
title: Update voor Azure Stack 1903 | Microsoft Docs
description: Meer informatie over de update 1903 voor geïntegreerde Azure Stack-systemen, met inbegrip van nieuwe functies, bekende problemen en het downloaden van de update.
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
ms.date: 04/04/2019
ms.author: sethm
ms.reviewer: adepue
ms.lastreviewed: 04/04/2019
ms.openlocfilehash: 2a2e289423eda53d610b2346193f6ee8a30b9c48
ms.sourcegitcommit: f093430589bfc47721b2dc21a0662f8513c77db1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/04/2019
ms.locfileid: "58917682"
---
# <a name="azure-stack-1903-update"></a>Azure Stack 1903 update

*Van toepassing op Geïntegreerde Azure Stack-systemen*

Dit artikel wordt de inhoud van het updatepakket 1903 beschreven. De update bevat verbeteringen, oplossingen en nieuwe functies voor deze versie van Azure Stack. Dit artikel ook worden bekende problemen in deze release beschreven, en bevat een koppeling om de update te downloaden. Bekende problemen zijn onderverdeeld in problemen direct verband houden met het updateproces en problemen met de build (na de installatie).

> [!IMPORTANT]  
> Dit pakket is alleen voor geïntegreerde Azure Stack-systemen. Dit pakket niet van toepassing op de Azure Stack Development Kit.

## <a name="build-reference"></a>Naslaginformatie over bouwen

De Azure Stack 1903 update build-nummer **1.1903.0.35**.

> [!IMPORTANT]
> De nettolading van de 1903 bevat geen een ASDK-release.

## <a name="hotfixes"></a>Hotfixes

Azure Stack brengt hotfixes op gezette tijden. Zorg ervoor dat voor het installeren van de [meest recente Azure Stack-hotfix](#azure-stack-hotfixes) voor 1902 voordat u Azure Stack bijwerkt naar 1903.

Azure Stack-hotfixes zijn alleen van toepassing op Azure Stack-geïntegreerde systemen; Probeer niet te installeren van hotfixes in de ASDK.

> [!TIP]  
> Abonneer u op de volgende *RSS* of *Atom* feeds, blijven van het Azure Stack-hotfixes:
> - [RSS](https://support.microsoft.com/app/content/api/content/feeds/sap/en-us/32d322a8-acae-202d-e9a9-7371dccf381b/rss)
> - [Atom](https://support.microsoft.com/app/content/api/content/feeds/sap/en-us/32d322a8-acae-202d-e9a9-7371dccf381b/atom)

### <a name="azure-stack-hotfixes"></a>Hotfixes voor Azure Stack

- **1902**: [KB 4481548 – Azure Stack-hotfix 1.1902.2.73](https://support.microsoft.com/help/4494719)

## <a name="improvements"></a>Verbeteringen

- De nettolading van de 1903 update bevat een update van Azure Stack-onderdelen die niet het onderliggende besturingssysteem welke hosts Azure Stack zijn. Hierdoor kunnen bepaalde updates worden binnen het bereik. Als gevolg hiervan de verwachte hoelang het duurt voor de update 1903 om te voltooien is minder (ongeveer 16 uur, maar de exacte tijden kunnen verschillen). Deze afname in runtime is specifiek voor de update 1903 en latere updates updates van het besturingssysteem, wat andere runtimes kunnen bevatten. Toekomstige updates bevatten vergelijkbaar richtlijnen over de verwachte tijd voor die de update duurt om te voltooien, afhankelijk van de nettolading die is opgenomen.

- Een probleem opgelost in netwerken, waardoor wijzigingen in de **time-out voor inactiviteit (minuten)** waarde van een **openbaar IP-adres** van kracht. Eerder, wijzigingen in deze waarde zijn genegeerd, zodat, ongeacht eventuele wijzigingen die u hebt gemaakt, wordt de waarde standaard tot vier minuten. Deze instelling bepaalt het aantal minuten dat een TCP-verbinding open zonder afhankelijk te zijn op clients om keepalive berichten te verzenden. Houd er rekening mee exemplaar alleen worden beïnvloed door deze bug niveau openbare IP-adressen, de niet-openbare IP-adressen toegewezen aan een load balancer.

- Verbeteringen wat betreft de betrouwbaarheid van de engine voor het bijwerken, met inbegrip van automatisch herstellen van algemene problemen zodat updates zonder onderbreking toepassen.

- Verbeteringen voor de detectie en herstel van weinig schijfruimte ruimte voorwaarden.

## <a name="prerequisites"></a>Vereisten

> [!IMPORTANT]
> Installeer de [meest recente Azure Stack-hotfix](#azure-stack-hotfixes) voor 1902 (indien aanwezig) voordat u bijwerkt naar 1903.

- Zorg ervoor dat u de nieuwste versie van de [Azure Stack-Capaciteitsplanner](https://aka.ms/azstackcapacityplanner) doen uw workload plannen en vergroten/verkleinen. De nieuwste versie bevat oplossingen voor problemen en beschikt over nieuwe functies die zijn uitgebracht met elke Azure Stack-update.

- Voordat u begint met de installatie van deze update, voert u [Test AzureStack](azure-stack-diagnostic-test.md) met de volgende parameter om te valideren van de status van uw Azure-Stack en los eventuele operationele problemen gevonden, met inbegrip van alle waarschuwingen en fouten. Ook actieve waarschuwingen bekijken en op te lossen die actie is vereist:

    ```PowerShell
   Test-AzureStack -Group UpdateReadiness  
    ```

- Wanneer Azure Stack wordt beheerd door System Center Operations Manager, zorg ervoor dat u het bijwerken van de [Management Pack voor Microsoft Azure Stack](https://www.microsoft.com/download/details.aspx?id=55184) naar versie 1.0.3.11 voordat u 1903 toepast.

- De indeling van het pakket voor de Azure Stack-update is gewijzigd van **.bin/.exe/.xml** naar **.zip/.xml** vanaf de release 1902. Klanten met verbonden Azure Stack-schaaleenheden ziet de **Update beschikbaar** bericht in de portal. Klanten die niet zijn verbonden kunnen nu gewoon downloaden en importeren van het ZIP-bestand met het bijbehorende XML-bestand.

<!-- ## New features -->

<!-- ## Fixed issues -->

<!-- ## Common vulnerabilities and exposures -->

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

<!-- Daniel 3/28 -->
- In de gebruikersportal, wanneer u gaat u naar een blob in een storage-account en probeert te openen **toegangsbeleid** in de navigatiestructuur, het volgende venster niet kan worden geladen.

<!-- Daniel 3/28 -->
- In de gebruikersportal, wanneer u probeert te uploaden van een blob met de **OAuth(preview)** optie, de taak is mislukt met een foutbericht weergegeven. U kunt dit probleem omzeilen, uploaden van de blob met behulp de **SAS** optie.

- Wanneer u bent aangemeld bij de Azure Stack-portals ziet u mogelijk meldingen over de openbare Azure-portal. U kunt deze meldingen veilig negeren als ze momenteel niet van toepassing op Azure Stack (bijvoorbeeld ' 1 nieuwe update - de volgende updates zijn nu beschikbaar: Azure portal April 2019-update').

<!-- ### Health and monitoring -->

### <a name="compute"></a>Compute

- Bij het maken van een nieuwe Windows virtuele Machine (VM), worden de volgende fout weergegeven:

   `'Failed to start virtual machine 'vm-name'. Error: Failed to update serial output settings for VM 'vm-name'`

   De fout doet zich voor als u diagnostische gegevens over opstarten op een virtuele machine inschakelt, maar uw boot diagnostics storage-account verwijderen. U kunt dit probleem omzeilen, maakt u opnieuw de storage-account met dezelfde naam als u eerder hebt gebruikt.

<!-- 2967447 - IS, ASDK, to be fixed in 1902 -->
- Het maken van virtuele-Machineschaalset ervaring biedt 7.2 CentOS gebaseerde als een optie voor implementatie. Omdat deze afbeelding niet beschikbaar op Azure Stack is, selecteer een ander besturingssysteem voor uw implementatie, of u een Azure Resource Manager-sjabloon op te geven van een andere CentOS-installatiekopie die is gedownload vóór implementatie vanuit de marketplace door de de operator.  

<!-- TBD - IS ASDK --> 
- Na het toepassen van de 1903 bijwerken, kunnen de volgende problemen optreden bij het implementeren van virtuele machines met Managed Disks:

   - Als het abonnement is gemaakt vóór de update 1808, een virtuele machine met Managed Disks kan mislukken met een interne fout. Los de fout op door deze stappen voor elk abonnement uit te voeren:
      1. Ga in de tenantportal naar **abonnementen** en zoek het abonnement. Selecteer **Resourceproviders**en selecteer vervolgens **Microsoft.Compute**, en klik vervolgens op **opnieuw registreren**.
      2. Onder hetzelfde abonnement, gaat u naar **Access Control (IAM)**, en Controleer **Azure Stack – beheerde schijf** wordt vermeld.
   - Als u een omgeving met meerdere tenants hebt geconfigureerd, kan virtuele machines implementeren in een abonnement dat is gekoppeld aan een gast-map mislukken met een interne fout. Volg deze stappen om op te lossen de fout, [in dit artikel](azure-stack-enable-multitenancy.md#registering-azure-stack-with-the-guest-directory) opnieuw configureren van elk van de Gast-mappen.

- Een Ubuntu-18.04 VM gemaakt met SSH-verificatie ingeschakeld kunt u de SSH-sleutels gebruiken voor aanmelding bij. Als tijdelijke oplossing, gebruik van VM-toegang voor de Linux-extensie voor het implementeren van SSH-sleutels na het inrichten of verificatie op basis van wachtwoord gebruiken.

- Als u nog geen een Hardware Lifecycle Host (HLH): Voordat u build 1902, moest u Groepsbeleid instellen *Computerconfiguratie\Windows-instellingen\Beveiligingsinstellingen\Lokaal Beleid\beveiligingsopties* naar **verzenden LM en NTLM-gebruik NTLMv2-sessiebeveiliging als heeft onderhandeld over**. Sinds build 1902, moet u deze als laten **niet gedefinieerd** of stel deze in op **alleen verzenden NTLMv2-antwoord** (dit is een standaardwaarde). Anders kunt u zich niet tot stand brengen van een externe PowerShell-sessie en ontvangt u een *toegang is geweigerd* fout:

   ```PowerShell
   PS C:\Users\Administrator> $session = New-PSSession -ComputerName x.x.x.x -ConfigurationName PrivilegedEndpoint  -Credential $cred
   New-PSSession : [x.x.x.x] Connecting to remote server x.x.x.x failed with the following error message : Access is denied. For more information, see the 
   about_Remote_Troubleshooting Help topic.
   At line:1 char:12
   + $session = New-PSSession -ComputerName x.x.x.x -ConfigurationNa ...
   +            ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
      + CategoryInfo          : OpenError: (System.Manageme....RemoteRunspace:RemoteRunspace) [New-PSSession], PSRemotingTransportException
      + FullyQualifiedErrorId : AccessDenied,PSSessionOpenFailed
   ```

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

U kunt de Azure Stack 1903 update-pakket van downloaden [hier](https://aka.ms/azurestackupdatedownload). 

In verbonden scenario's alleen, Azure Stack-implementaties regelmatig controleren op een beveiligde eindpunt en automatisch een melding als een update beschikbaar voor uw cloud is. Zie voor meer informatie, [beheren van updates voor Azure Stack](azure-stack-updates.md#using-the-update-tile-to-manage-updates).

## <a name="next-steps"></a>Volgende stappen

- Zie voor een overzicht van de update management in Azure Stack, [-updates beheren in Azure Stack-overzicht](azure-stack-updates.md).  
- Zie voor meer informatie over het toepassen van updates met Azure Stack [toepassen van updates in Azure Stack](azure-stack-apply-updates.md).
- Het uitvoeren van onderhoud beleid voor geïntegreerde Azure Stack-systemen, en wat u moet doen om te voorkomen dat uw systeem in een ondersteunde status Zie [Azure Stack servicebeleid](azure-stack-servicing-policy.md).  
- Voor het gebruik van de bevoegde eindpunt (PEP) om te controleren en hervatten van updates, Zie [-updates in Azure Stack met behulp van het eindpunt van de bevoegde controleren](azure-stack-monitor-update.md).  
