---
title: Just-in-time-VM-toegang in Azure Security Center | Microsoft Docs
description: Dit document laat zien hoe just-in-time-VM-toegang in Azure Security Center helpt u bepalen de toegang tot uw Azure virtual machines.
services: security-center
documentationcenter: na
author: monhaber
manager: barbkess
editor: ''
ms.assetid: 671930b1-fc84-4ae2-bf7c-d34ea37ec5c7
ms.service: security-center
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 6/17/2019
ms.author: v-mohabe
ms.openlocfilehash: eb9366acf82c94bdf99c4d4f0c7c6bdf4f51e06d
ms.sourcegitcommit: 2d3b1d7653c6c585e9423cf41658de0c68d883fa
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/20/2019
ms.locfileid: "67295038"
---
# <a name="manage-virtual-machine-access-using-just-in-time"></a>Beheer van de virtuele machine toegang met just-in-time

Toegang tot de virtuele machine (VM) van de Just-in-time (JIT) kan worden gebruikt om binnenkomend verkeer naar uw Azure VM's, blootstelling aan aanvallen te verminderen terwijl eenvoudige toegang tot het verbinding maken met virtuele machines wanneer dat nodig is.

> [!NOTE]
> De just-in-time-functie is beschikbaar op de prijscategorie Standard van Security Center.  Bekijk de pagina [Prijzen](security-center-pricing.md) voor meer informatie over de tariefopties van Security Center.


> [!NOTE]
> Security Center just-in-time-VM toegang krijgen tot op dit moment ondersteunt alleen VM's geïmplementeerd via Azure Resource Manager. Voor meer informatie over de klassieke en Resource Manager-implementatiemodel zien [Azure Resource Manager en klassieke implementatie](../azure-resource-manager/resource-manager-deployment-model.md).

## <a name="attack-scenario"></a>Aanval

Brute force-aanvallen vaak doelpoorten voor de management als een manier toegang te krijgen tot een virtuele machine. Als dit lukt, kan een aanvaller controle over de virtuele machine en tot stand brengen van een voet achter in uw omgeving.

Een manier om te beperken van blootstelling aan een brute force-aanval is te beperken van de hoeveelheid tijd die een poort geopend is. Beheerpoorten hoeven niet te allen tijde geopend te zijn. Ze hoeven alleen geopend te zijn wanneer u bent verbonden met de VM, bijvoorbeeld om beheer- of onderhoudstaken uit te voeren. Wanneer just-in-time is ingeschakeld, Security Center maakt gebruik van [netwerkbeveiligingsgroep](../virtual-network/security-overview.md#security-rules) (NSG) en Azure firewallregels, die toegang tot beheerpoorten beperken, zodat ze niet het doelwit van aanvallers.

![Just-in-time-scenario](./media/security-center-just-in-time/just-in-time-scenario.png)

## <a name="how-does-jit-access-work"></a>Hoe werkt de toegang tot JIT?

Wanneer just-in-time is ingeschakeld, wordt de status van Security Center vergrendelt binnenkomend verkeer naar uw Azure VM's met het maken van een NSG-regel. U selecteert de poorten op de VM waarop binnenkomend verkeer wordt vergrendeld. Deze poorten worden beheerd door de just-in-time-oplossing.

Wanneer een gebruiker vraagt om toegang tot een virtuele machine, controleert Security Center of de gebruiker heeft [Role-Based Access Control (RBAC)](../role-based-access-control/role-assignments-portal.md) machtigingen die ze aanvragen is toegang tot een virtuele machine toestaan. Als de aanvraag is goedgekeurd, configureert Security Center automatisch de Netwerkbeveiligingsgroepen (nsg's) en de Firewall van Azure om toe te staan van inkomend verkeer op de geselecteerde poorten en de aangevraagde bron-IP-adressen of adresbereiken voor de hoeveelheid tijd die is opgegeven. Nadat de respijtperiode is verlopen, worden de nsg's door Security Center de eerdere status hersteld. Deze verbindingen die al zijn gemaakt niet wordt onderbroken, maar.

 > [!NOTE]
 > Als een aanvraag voor JIT-toegang is goedgekeurd voor een VM uit achter een Firewall voor Azure, wordt Security Center automatisch vervolgens de regels van de NSG en de firewall. Voor de hoeveelheid tijd die is opgegeven, kunnen de regels voor binnenkomend verkeer naar de geselecteerde poorten en de aangevraagde bron-IP-adressen of bereiken. Wanneer het tijd voltooid is, wordt de firewall en NSG-regels van Security Center hersteld naar de eerdere status.


## <a name="permissions-needed-to-configure-and-use-jit"></a>Vereiste machtigingen om te configureren en gebruiken van JIT

| Zodat een gebruiker: | Machtigingen om in te stellen|
| --- | --- |
| Configureren of te bewerken van een JIT-beleid voor een virtuele machine | *Deze acties toewijzen aan de rol:*  Op het bereik van een abonnement of resourcegroep die is gekoppeld aan de virtuele machine: ```Microsoft.Security/locations/jitNetworkAccessPolicies/write``` Op het bereik van een abonnement of resourcegroep of een virtuele machine: ```Microsoft.Compute/virtualMachines/write``` | 
| ||
|Aanvraag voor JIT-toegang tot een VM | *Deze acties aan de gebruiker toewijzen:*  Op het bereik van een abonnement of resourcegroep die is gekoppeld aan de virtuele machine:  ```Microsoft.Security/locations/{the_location_of_the_VM}/jitNetworkAccessPolicies/initiate/action``` Op het bereik van een abonnement of resourcegroep of een virtuele machine: ```Microsoft.Compute/virtualMachines/read``` |


## <a name="configure-jit-on-a-vm"></a>JIT configureren op een virtuele machine

Er zijn 3 manieren om een JIT-beleid configureren op een virtuele machine:

- [JIT-toegang configureren in Azure Security Center](#jit-asc)
- [JIT-toegang configureren op een Azure VM-beheerblade](#jit-vm)
- [Een JIT-beleid op een virtuele machine via een programma configureren](#jit-program)

## <a name="configure-jit-in-asc"></a>JIT in ASC configureren

Ontkoppelen van ASC, kunt u een beleid en de aanvraag voor toegang tot JIT aan een virtuele machine met behulp van een JIT-beleid configureren


### JIT-toegang configureren op een virtuele machine in ASC <a name="jit-asc"></a>

1. Open het dashboard van **Security Center**.

2. Selecteer in het linkerdeelvenster **Just-in-time-VM-toegang**.

    ![Just-in-time VM access tegel](./media/security-center-just-in-time/just-in-time.png)

    De **Just-in-time-VM-toegang** venster wordt geopend.

      ![Just-in-time-toegang inschakelen](./media/security-center-just-in-time/enable-just-in-time.png)

    **Just-in-time-VM-toegang** bevat informatie over de status van uw virtuele machines:

    - **Geconfigureerd** -VM's die zijn geconfigureerd ter ondersteuning van just-in-time-VM-toegang. De gegevens die worden gepresenteerd is voor de afgelopen week en bevat voor elke virtuele machine van het aantal goedgekeurde aanvragen, de datum van laatste toegang en de tijd en de laatste gebruiker.
    - **Aanbevolen** -VM's die just-in-time-VM-toegang kunnen ondersteunen, maar zijn niet geconfigureerd op. U wordt aangeraden dat u toegangsbeheer voor just-in-time-VM voor deze virtuele machines inschakelen. 
    - **Geen aanbeveling**: redenen waarom een VM mogelijk niet wordt aanbevolen zijn:
      - Ontbrekende NSG - de just-in-time-oplossing vereist een NSG aanwezig.
      - Klassieke VM - Beveiligingscentrum just-in-time-VM-toegang ondersteunt momenteel alleen VM's die zijn geïmplementeerd via Azure Resource Manager. Een klassieke implementatie wordt niet ondersteund door de just-in-time-oplossing. 
      - Overig: een virtuele machine is in deze categorie als de just-in-time-oplossing is uitgeschakeld in het beveiligingsbeleid van het abonnement of de resourcegroep, of als de virtuele machine ontbreekt in een openbaar IP-adres en geen NSG aanwezig is.

3. Selecteer de **aanbevolen** tabblad.

4. Onder **virtuele MACHINE**, klikt u op de virtuele machines die u wilt inschakelen. Hiermee wordt een vinkje naast een virtuele machine geplaatst.

5. Klik op **JIT inschakelen op virtuele machines**.
   -. Deze blade wordt weergegeven de standaardpoorten aanbevolen door Azure Security Center:
      - 22 - SSH
      - 3389 - RDP
      - 5985 - WinRM 
      - 5986 - WinRM
6. U kunt ook aangepaste poorten configureren:

      1. Klik op **Toevoegen**. De **poortconfiguratie toevoegen** venster wordt geopend.
      2. Voor elke poort die u wilt configureren, zowel standaard en aangepaste, kunt u de volgende instellingen aanpassen:

    - **Type protocol**-het protocol dat is toegestaan op deze poort wanneer een aanvraag is goedgekeurd.
    - **Toegestane bron-IP-adressen**-het IP-adresbereiken die zijn toegestaan op deze poort wanneer een aanvraag is goedgekeurd.
    - **Maximale aanvraagtijd**-de maximale periode gedurende welke een specifieke poort kan worden geopend.

     3. Klik op **OK**.

1. Klik op **Opslaan**.

> [!NOTE]
>Wanneer JIT-VM-toegang is ingeschakeld voor een virtuele machine, maakt Azure Security Center 'niet al het binnenkomende verkeer toestaan' regels voor de geselecteerde poorten in de netwerkbeveiligingsgroepen die zijn gekoppeld en de Firewall van Azure met het. Als andere regels waren voor de geselecteerde poorten is gemaakt, klikt u vervolgens hebben de bestaande regels voorrang op de nieuwe regels voor 'alle binnenkomend verkeer toestaan'. Als er geen bestaande regels op de geselecteerde poorten zijn, hebben de nieuwe regels voor 'alle binnenkomend verkeer toestaan' top voorrang in de Netwerkbeveiligingsgroepen en de Firewall van Azure.


## <a name="request-jit-access-via-asc"></a>Aanvraag voor JIT-toegang via ASC

Om aan te vragen tot een virtuele machine via de ASC:

1. Onder **Just-in-time-VM-toegang**, selecteer de **geconfigureerd** tabblad.

2. Onder **virtuele Machine**, klikt u op de virtuele machines die u wilt toegang aanvragen voor. Hiermee wordt een vinkje naast de virtuele machine geplaatst.


    - Het pictogram in de **Verbindingsdetails** kolom geeft aan of JIT is ingeschakeld op de NSG of de FW. Als er op beide ingeschakeld, wordt alleen het pictogram van de Firewall weergegeven.

    - De **Verbindingsdetails** kolom bevat de juiste informatie vereist voor het verbinding maken met de virtuele machine, evenals geeft aan dat de poorten geopend.

      ![Just-in-time-toegang aanvragen](./media/security-center-just-in-time/request-just-in-time-access.png)

3. Klik op **toegang aanvragen**. De **toegang aanvragen** venster wordt geopend.

      ![JIT-details](./media/security-center-just-in-time/just-in-time-details.png)

4. Onder **toegang aanvragen**, voor elke virtuele machine, configureert u de poorten die u wilt openen en de bron-IP-adressen die de poort in wordt geopend en de periode waarvoor u de poort is geopend. Dit is alleen mogelijk voor het aanvragen van toegang tot de poorten die zijn geconfigureerd in de just-in-time-beleid. Elke poort heeft een maximale toegestane tijd die is afgeleid van de just-in-time-beleid.

5. Klik op **poorten openen**.

> [!NOTE]
> Als een gebruiker die toegang aanvraagt achter een proxy, de optie wordt **Mijn IP** werkt mogelijk niet. Mogelijk moet u het volledige IP-adresbereik van de organisatie definiëren.

## <a name="edit-a-jit-access-policy-via-asc"></a>Een JIT-toegangsbeleid via ASC bewerken

U kunt de bestaande just-in-time-beleid van een virtuele machine wijzigen door het toevoegen en configureren van een nieuwe poort om te beveiligen voor die VM, of door andere instellingen met betrekking tot een al beveiligde poort wijzigen.

Bewerk een bestaand just-in-time-beleid van een virtuele machine als volgt:
1. In de **geconfigureerd** tabblad onder **VMs**, selecteert u een virtuele machine waarop een poort toevoegen door te klikken op de drie puntjes in de rij voor die VM. 

1. Selecteer **Bewerken**.
1. Onder **JIT-VM-configuratie**, kunt u de bestaande instellingen van een al beveiligde poort bewerken of toevoegen van een nieuwe aangepaste poort. 
  ![JIT-vm-toegang](./media/security-center-just-in-time/edit-policy.png)

## <a name="audit-jit-access-activity-in-asc"></a>Activiteit van JIT-toegang in ASC controleren

U krijgt inzicht in VM-activiteiten met zoeken in Logboeken. Logboeken weergeven:

1. Onder **Just-in-time-VM-toegang**, selecteer de **geconfigureerd** tabblad.
2. Onder **VMs**, selecteer een virtuele machine om informatie over door te klikken op de drie puntjes in de rij voor die virtuele machine weer te geven en selecteer **activiteitenlogboek** in het menu. De **activiteitenlogboek** wordt geopend.

   ![Activiteitenlogboek selecteren](./media/security-center-just-in-time/select-activity-log.png)

   **Activiteitenlogboek** biedt een gefilterde weergave van de vorige bewerkingen voor die virtuele machine samen met de tijd, datum en -abonnement.

U kunt de logboekgegevens downloaden door te selecteren **Klik hier om alle items te downloaden als CSV-bestand**.

De filters wijzigen en klik op **toepassen** een zoeken en het logboek te maken.



## JIT-toegang configureren op een Azure VM-beheerblade <a name="jit-vm"></a>

Voor uw gemak, kunt u verbinding maken met een virtuele machine met behulp van JIT rechtstreeks vanuit binnen de VM-blade in Azure.

### <a name="configure-jit-access-on-a-vm-via-the-azure-vm-blade"></a>JIT-toegang configureren op een virtuele machine via de Azure VM-blade

Als u wilt kunt u eenvoudig just-in-time-toegang voor uw virtuele machines implementeert, kunt u een virtuele machine om toe te staan alleen just-in-time toegang rechtstreeks vanuit de virtuele machine instellen.

1. Selecteer in de Azure portal, **virtuele machines**.
2. Klik op de virtuele machine die u wilt beperken tot just-in-time-toegang.
3. In het menu, klikt u op **configuratie**.
4. Onder **Just-in-time-toegang** klikt u op **just-in-time-beleid inschakelen**. 

Hiermee kunt just-in-time-toegang voor de virtuele machine met behulp van de volgende instellingen:

- Windows-servers:
    - RDP-poort 3389
    - drie uur van de maximale toegestane toegang
    - Toegestane bron-IP-adressen is ingesteld op een
- Linux-servers:
    - SSH-poort 22
    - drie uur van de maximale toegestane toegang
    - Toegestane bron-IP-adressen is ingesteld op een
     
Als een virtuele machine al just is-in-time ingeschakeld, wanneer u gaat u naar de configuratiepagina zich kunt u kunt zien dat just-in-time is ingeschakeld en kunt u de koppeling te openen van het beleid in Azure Security Center te bekijken en wijzigen van de instellingen.

![JIT-configuratie in de virtuele machine](./media/security-center-just-in-time/jit-vm-config.png)

### <a name="request-jit-access-to-a-vm-via-the-azure-vm-blade"></a>Aanvraag voor JIT-toegang tot een VM via de Azure VM-blade

In de Azure-portal, wanneer u probeert verbinding maken met een virtuele machine, controleert Azure om te zien als u een just-in-time-toegangsbeleid geconfigureerd op deze VM hebt.

- Als u een JIT-beleid dat is geconfigureerd op de virtuele machine hebt, kunt u klikken op **toegang aanvragen** waarmee u kunt toegang in overeenstemming met het JIT-beleid instellen voor de virtuele machine. 

  >![JIT-aanvraag](./media/security-center-just-in-time/jit-request.png)

  De toegang wordt aangevraagd met de volgende standaardparameters:

  - **bron-IP**: 'Any' (*) (kan niet worden gewijzigd)
  - **tijdsbereik**: drie uur (kan niet worden gewijzigd)  <!--Isn't this set in the policy-->
  - **poortnummer** RDP-poort 3389 voor Windows / -poort 22 voor Linux (kan worden gewijzigd)

    > [!NOTE]
    > Nadat een aanvraag is goedgekeurd voor een virtuele machine beveiligd door de Firewall van Azure, biedt Security Center de gebruiker met de details van de juiste verbinding (van de poorttoewijzing van de tabel DNAT) als u wilt gebruiken voor verbinding met de virtuele machine.

- Als u geen JIT geconfigureerd op een virtuele machine hebt, wordt u gevraagd naar een JIT-beleid configureren dat deze.

  ![JIT-prompt](./media/security-center-just-in-time/jit-prompt.png)

## Een JIT-beleid op een virtuele machine via een programma configureren  <a name="jit-program"></a>

U kunt instellen en gebruiken van just-in-time via REST-API's en via PowerShell.

## <a name="jit-vm-access-via-rest-apis"></a>JIT-VM-toegang via REST API 's

De functie voor just-in-time VM toegang kan worden gebruikt via de API van Azure Security Center. U kunt informatie over de geconfigureerde VM's, nieuwe visuals toevoegen, aanvragen van toegang tot een virtuele machine, en meer, via deze API. Zie [Jit-netwerkbeleid](https://docs.microsoft.com/rest/api/securitycenter/jitnetworkaccesspolicies), voor meer informatie over de REST-API van just-in-time.

## <a name="jit-vm-access-via-powershell"></a>JIT-VM-toegang via PowerShell

Als u de just-in-time VM access-oplossing via PowerShell, gebruikt u de officiële Azure Security Center PowerShell-cmdlets en met name `Set-AzJitNetworkAccessPolicy`.

Het volgende voorbeeld wordt een toegangsbeleid voor just-in-time-VM wordt ingesteld op een specifieke virtuele machine en Hiermee stelt u het volgende:

1.  Sluit poort 22 en 3389.

2.  Stel een maximale periode van drie uur voor elk, zodat ze kunnen worden geopend per aanvraag goedgekeurd.
3.  Kan de gebruiker die toegang aanvraagt tot de bron-IP-adressen beheren en kan de gebruiker een geslaagde sessie op een toegangsverzoek goedgekeurd just-in-time-tot stand brengen.

Voer het volgende in PowerShell om dit te doen:

1.  Een variabele waarin de just-in-time VM-toegangsbeleid voor een virtuele machine toewijzen:

        $JitPolicy = (@{
         id="/subscriptions/SUBSCRIPTIONID/resourceGroups/RESOURCEGROUP/providers/Microsoft.Compute/virtualMachines/VMNAME"
        ports=(@{
             number=22;
             protocol="*";
             allowedSourceAddressPrefix=@("*");
             maxRequestAccessDuration="PT3H"},
             @{
             number=3389;
             protocol="*";
             allowedSourceAddressPrefix=@("*");
             maxRequestAccessDuration="PT3H"})})

2.  Voeg in het toegangsbeleid VM just-in-time VM naar een matrix:
    
        $JitPolicyArr=@($JitPolicy)

3.  Het toegangsbeleid voor just-in-time-VM configureren op de geselecteerde virtuele machine:
    
        Set-AzJitNetworkAccessPolicy -Kind "Basic" -Location "LOCATION" -Name "default" -ResourceGroupName "RESOURCEGROUP" -VirtualMachine $JitPolicyArr 

#### <a name="request-access-to-a-vm-via-powershell"></a>Aanvraag voor toegang tot een virtuele machine via PowerShell

In het volgende voorbeeld ziet u een aanvraag voor toegang tot just-in-time-VM naar een specifieke virtuele machine in welke poort 22 wordt aangevraagd, worden geopend voor een specifiek IP-adres en voor een bepaalde hoeveelheid tijd:

Voer het volgende in PowerShell:
1.  De eigenschappen van de virtuele machine aanvraag-toegang configureren

        $JitPolicyVm1 = (@{
          id="/SUBSCRIPTIONID/resourceGroups/RESOURCEGROUP/providers/Microsoft.Compute/virtualMachines/VMNAME"
        ports=(@{
           number=22;
           endTimeUtc="2018-09-17T17:00:00.3658798Z";
           allowedSourceAddressPrefix=@("IPV4ADDRESS")})})
2.  Voeg de aanvraagparameters van de VM-toegang in een matrix:

        $JitPolicyArr=@($JitPolicyVm1)
3.  Verzenden van de aanvraag voor toegang (Gebruik de resource-ID u hebt verkregen in stap 1)

        Start-AzJitNetworkAccessPolicy -ResourceId "/subscriptions/SUBSCRIPTIONID/resourceGroups/RESOURCEGROUP/providers/Microsoft.Security/locations/LOCATION/jitNetworkAccessPolicies/default" -VirtualMachine $JitPolicyArr

Zie voor meer informatie de documentatie van de PowerShell-cmdlet.

## <a name="next-steps"></a>Volgende stappen
In dit artikel hebt u geleerd hoe just-in-time-VM-toegang in Security Center helpt u bepalen de toegang tot uw Azure virtual machines.

Zie de volgende onderwerpen voor meer informatie over het Beveiligingscentrum:

- [Beveiligingsbeleid instellen](tutorial-security-policy.md) : informatie over het configureren van beveiligingsbeleid voor uw Azure-abonnementen en resourcegroepen.
- [Aanbevelingen voor beveiliging beheren](security-center-recommendations.md) : Leer hoe aanbevelingen helpen u uw Azure-resources te beveiligen.
- [Beveiligingsstatus bewaken](security-center-monitoring.md) : informatie over het bewaken van de status van uw Azure-resources.
- [Beheren en erop reageren op beveiligingswaarschuwingen](security-center-managing-and-responding-alerts.md) : informatie over het beheren van en reageren op beveiligingswaarschuwingen.
- [Partneroplossingen bewaken](security-center-partner-solutions.md) : informatie over het bewaken van de status van uw partneroplossingen.
- [Security Center FAQ](security-center-faq.md) : Raadpleeg Veelgestelde vragen over het gebruik van de service.
- [Azure-beveiligingsblog](https://blogs.msdn.microsoft.com/azuresecurity/): lees blogberichten over de beveiliging en naleving van Azure.

