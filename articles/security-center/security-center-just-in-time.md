---
title: Just-in-time-VM-toegang in Azure Security Center | Microsoft Docs
description: Dit document laat zien hoe just-in-time-VM-toegang in Azure Security Center u helpt toegang tot uw Azure virtual machines beheren.
services: security-center
documentationcenter: na
author: rkarlin
manager: MBaldwin
editor: ''
ms.assetid: ''
ms.service: security-center
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 10/10/2018
ms.author: rkarlin
ms.openlocfilehash: 98533e3c1454867ff09c53902f0f575d198452a3
ms.sourcegitcommit: 74941e0d60dbfd5ab44395e1867b2171c4944dbe
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/15/2018
ms.locfileid: "49320336"
---
# <a name="manage-virtual-machine-access-using-just-in-time"></a>Beheer van de virtuele machine toegang met just-in-tijd

Just-in-time virtuele machine (VM) kan toegang worden gebruikt om binnenkomend verkeer naar uw Azure VM's, blootstelling aan aanvallen te verminderen terwijl eenvoudige toegang tot het verbinding maken met virtuele machines wanneer dat nodig is.

> [!NOTE]
> De just in time functie is beschikbaar op de prijscategorie Standard van Security Center.  Bekijk de pagina [Prijzen](security-center-pricing.md) voor meer informatie over de tariefopties van Security Center.
>
>

## <a name="attack-scenario"></a>Aanval

Brute force-aanvallen vaak doelpoorten voor de management als een manier toegang te krijgen tot een virtuele machine. Als dit lukt, kan een aanvaller controle over de virtuele machine en tot stand brengen van een voet achter in uw omgeving.

Een manier om te beperken van blootstelling aan een brute force-aanval is te beperken van de hoeveelheid tijd die een poort geopend is. Beheerpoorten hoeven niet te allen tijde geopend te zijn. Ze hoeven alleen geopend te zijn wanneer u bent verbonden met de VM, bijvoorbeeld om beheer- of onderhoudstaken uit te voeren. Wanneer just-in-time is ingeschakeld, Security Center maakt gebruik van [netwerkbeveiligingsgroep](../virtual-network/security-overview.md#security-rules) (NSG) regels, die toegang tot beheerpoorten beperken, zodat ze niet het doelwit van aanvallers.

![Just-in-time-scenario][1]

## <a name="how-does-just-in-time-access-work"></a>Hoe just-in-time-toegang werkt?

Wanneer JIT is ingeschakeld, wordt binnenkomend verkeer naar de Azure-VM's vergrendeld via Security Center door een NSG-regel te maken. U selecteert de poorten op de VM waarop binnenkomend verkeer wordt vergrendeld. Deze poorten worden beheerd door de just in time-oplossing.

Wanneer een gebruiker vraagt om toegang tot een virtuele machine, controleert Security Center of de gebruiker heeft [Role-Based Access Control (RBAC)](../role-based-access-control/role-assignments-portal.md) machtigingen die het verschaffen van toegang voor schrijven voor de virtuele machine. Als ze schrijfmachtigingen hebben, de aanvraag is goedgekeurd en Security Center automatisch de Netwerkbeveiligingsgroepen (nsg's configureert) om toe te staan van inkomend verkeer op de geselecteerde poorten voor de hoeveelheid tijd die u hebt opgegeven. Nadat de respijtperiode is verlopen, worden de nsg's door Security Center de eerdere status hersteld. Deze verbindingen die al zijn gemaakt niet wordt onderbroken, maar.

> [!NOTE]
> Security Center just-in-time-VM-toegang ondersteunt momenteel alleen VM's die zijn geïmplementeerd via Azure Resource Manager. Voor meer informatie over de klassieke en Resource Manager-implementatiemodel zien [Azure Resource Manager en klassieke implementatie](../azure-resource-manager/resource-manager-deployment-model.md).
>
>

## <a name="using-just-in-time-access"></a>Met behulp van just-in-time-toegang

1. Open het dashboard van **Security Center**.

2. Selecteer in het linkerdeelvenster **Just-in-time-VM-toegang**.

![Tegel Just-in-time-VM-toegang][2]

De **Just-in-time-VM-toegang** venster wordt geopend.

![Tegel Just-in-time-VM-toegang][10]

**Just-In-Time-VM-toegang** biedt informatie over de status van uw VM's:

- **Geconfigureerd**: VM's die zijn geconfigureerd om Just-In-Time-VM-toegang te ondersteunen. De gegevens die worden gepresenteerd is voor de afgelopen week en bevat voor elke virtuele machine van het aantal goedgekeurde aanvragen, de datum van laatste toegang en de tijd en de laatste gebruiker.
- **Aanbevolen**: VM's die Just-In-Time-VM-toegang kunnen ondersteunen maar er niet voor zijn geconfigureerd. U wordt aangeraden dat u just-in-time VM-toegangsbeheer voor deze virtuele machines inschakelt. Zie [configureren van een just in time-toegangsbeleid](#configuring-a-just-in-time-access-policy).
- **Geen aanbeveling**: redenen waarom een VM mogelijk niet wordt aanbevolen zijn:
  - NSG ontbreekt: de Just-In-Time-oplossing vereist dat er een NSG aanwezig is.
  - Klassieke VM: Just-In-Time-VM-toegang van Security Center ondersteunt momenteel alleen VM's die zijn geïmplementeerd via Azure Resource Manager. Een klassieke implementatie wordt niet ondersteund door de just in time-oplossing.
  - Overig: een VM valt in deze categorie als de Just-In-Time-oplossing is uitgeschakeld in het beveiligingsbeleid van het abonnement of de resourcegroep, of als de VM geen openbaar IP-adres heeft en geen NSG heeft.

## <a name="configuring-a-just-in-time-access-policy"></a>Configureren van een just in time-beleid

Selecteer de virtuele machines die u wilt inschakelen:

1. Onder **Just-in-time-VM-toegang**, selecteer de **aanbevolen** tabblad.

  ![Just-in-time-toegang inschakelen][3]

2. Onder **virtuele MACHINE**, selecteert u de virtuele machines die u wilt inschakelen. Hiermee wordt een vinkje naast een virtuele machine geplaatst.
3. Selecteer **JIT inschakelen op virtuele machines**.
4. Selecteer **Opslaan**.

### <a name="default-ports"></a>Standaard-poorten

Hier ziet u de standaard-poorten die door Security Center wordt aanbevolen just-in-time inschakelen.

1. Onder **Just-in-time-VM-toegang**, selecteer de **aanbevolen** tabblad.

  ![Standaard-poorten worden weergegeven][6]

2. Onder **VMs**, selecteert u een virtuele machine. Hiermee wordt een vinkje naast de virtuele machine wordt geplaatst en wordt geopend **JIT-VM-configuratie**. Deze blade wordt weergegeven de standaardpoorten.

### <a name="add-ports"></a>Poorten toevoegen

Onder **JIT-VM-configuratie**, kunt u ook toevoegen en configureren van een nieuwe poort waarop u wilt inschakelen just in time-oplossing.

1. Onder **JIT-VM-configuratie**, selecteer **toevoegen**. Hiermee opent u **poortconfiguratie toevoegen**.

  ![Poortconfiguratie][7]

2. Onder **poortconfiguratie toevoegen**, u de poort, protocoltype, toegestane bron-IP-adressen en maximale aanvraagtijd identificeren.

  Toegestane bron-IP's zijn de IP-adresbereik dat is toegestaan om te profiteren van een goedgekeurde aanvraag.

  Maximale aanvraagtijd is de maximale periode dat een specifieke poort kan worden geopend.

3. Selecteer **OK**.

> [!NOTE]
>Wanneer JIT-VM-toegang is ingeschakeld voor een VM, Azure Security Center maakt weigeren alle regels voor binnenkomend verkeer voor de geselecteerde poorten in de netwerkbeveiligingsgroepen die zijn gekoppeld. De regels is de hoogste prioriteit van uw Netwerkbeveiligingsgroepen of lagere prioriteit dan de bestaande regels die er nog. Dit is afhankelijk van een analyse uitgevoerd door Azure Security Center waarmee wordt bepaald of een regel beveiligd is.
>


## <a name="set-just-in-time-within-a-vm"></a>Just-in-time binnen een virtuele machine instellen

Als u wilt kunt u eenvoudig just-in-time-toegang voor uw virtuele machines implementeert, kunt u een virtuele machine om toe te staan alleen just-in-time toegang rechtstreeks vanuit de virtuele machine instellen.

1. Selecteer in de Azure portal, **virtuele machines**.
2. Klik op de virtuele machine die u wilt beperken tot just-in-time-toegang.
3. In het menu, klikt u op **configuratie**.
4. Onder **Just-in-time-toegang** klikt u op **just-in-time-beleid inschakelen**. 

Hiermee kunt just-in-time-toegang voor de virtuele machine met behulp van de volgende instellingen:

- Windows-servers:
    - RDP-poort 3389
    - toegang tot 3 uur
    - Toegestane bron-IP-adressen is ingesteld op Per aanvraag
- Linux-servers:
    - SSH-poort 22
    - toegang tot 3 uur
    - Toegestane bron-IP-adressen is ingesteld op Per aanvraag
     
Als een virtuele machine al just is-in-time ingeschakeld, wanneer u gaat u naar de configuratiepagina zich kunt u kunt zien dat just-in-time is ingeschakeld en kunt u de koppeling te openen van het beleid in Azure Security Center te bekijken en wijzigen van de instellingen.

![JIT-configuratie in de virtuele machine](./media/security-center-just-in-time/jit-vm-config.png)


## <a name="requesting-access-to-a-vm"></a>Aanvragen van toegang tot een virtuele machine

Om aan te vragen tot een virtuele machine:

1. Onder **Just-in-time-VM-toegang**, selecteer de **geconfigureerd** tabblad.
2. Onder **VMs**, selecteert u de virtuele machines die u toegang wilt inschakelen. Hiermee wordt een vinkje naast een virtuele machine geplaatst.
3. Selecteer **toegang aanvragen**. Hiermee opent u **toegang aanvragen**.

  ![Aanvraag voor toegang tot een virtuele machine][4]

4. Onder **toegang aanvragen**, u voor elke virtuele machine configureren met de poorten te openen, samen met het IP-adres voor een bron die de poort is geopend en de periode waarvoor u de poort is geopend. U kunt aanvragen alleen toegang tot de poorten die zijn geconfigureerd in de just in time-beleid. Elke poort heeft de maximaal toegestane tijd die zijn afgeleid van de just in time-beleid.
5. Selecteer **poorten openen**.

> [!NOTE]
> Wanneer een gebruiker vraagt om toegang tot een virtuele machine, controleert Security Center of de gebruiker heeft [Role-Based Access Control (RBAC)](../role-based-access-control/role-assignments-portal.md) machtigingen die het verschaffen van toegang voor schrijven voor de virtuele machine. Als ze over schrijfmachtigingen beschikt, wordt de aanvraag is goedgekeurd.
>
>

> [!NOTE]
> Als een gebruiker die toegang aanvraagt zich achter een proxy, werkt de optie 'Mijn IP' mogelijk niet. Mogelijk zijn er een nodig voor het definiëren van het volledige bereik van de organisatie.
>
>

## <a name="editing-a-just-in-time-access-policy"></a>Bewerken van een just in time-beleid

U kunt wijzigen van een virtuele machine bestaande just-in-time-beleid door toe te voegen en een nieuwe poort te openen voor die VM te configureren, of door het veranderen van andere parameters met betrekking tot een al beveiligde poort.

Als u wilt bewerken van een bestaande just-in-time-beleid van een virtuele machine, de **geconfigureerd** tabblad wordt gebruikt:

1. Onder **VMs**, selecteert u een virtuele machine naar een poort toevoegen door te klikken op de drie puntjes in de rij voor die VM. Hiermee opent u een menu.
2. Selecteer **bewerken** in het menu. Hiermee opent u **JIT-VM-configuratie**.

  ![Beleid bewerken][8]

3. Onder **JIT-VM-configuratie**, u kunt de bestaande instellingen van een al beveiligde poort voor installatie zonder toezicht bewerken door te klikken op de poort of kunt u **toevoegen**. Hiermee opent u **poortconfiguratie toevoegen**.

  ![Een poort toevoegen][7]

4. Onder **poortconfiguratie toevoegen**, identificeren van de poort en protocol-type, bron-IP's en de maximale aanvraagtijd toegestaan.
5. Selecteer **OK**.
6. Selecteer **Opslaan**.

## <a name="auditing-just-in-time-access-activity"></a>Controle van just-in-time toegangsactiviteit

U krijgt inzicht in VM-activiteiten met zoeken in Logboeken. Logboeken weergeven:

1. Onder **Just-in-time-VM-toegang**, selecteer de **geconfigureerd** tabblad.
2. Onder **VMs**, selecteert u een virtuele machine om informatie over door te klikken op de drie puntjes in de rij voor die virtuele machine weer te geven. Hiermee opent u een menu.
3. Selecteer **activiteitenlogboek** in het menu. Hiermee opent u **activiteitenlogboek**.

  ![Activiteitenlogboek selecteren][9]

  **Activiteitenlogboek** biedt een gefilterde weergave van de vorige bewerkingen voor die virtuele machine samen met de tijd, datum en -abonnement.

U kunt de logboekgegevens downloaden door te selecteren **Klik hier om alle items te downloaden als CSV-bestand**.

Wijzigen van de filters en selecteer **toepassen** een zoeken en het logboek te maken.

## <a name="using-just-in-time-vm-access-via-rest-apis"></a>Met behulp van just-in-time-VM-toegang via REST API 's

De just in time-VM-toegang functie via de API van Azure Security Center kan worden gebruikt. U kunt informatie over de geconfigureerde VM's, nieuwe visuals toevoegen, aanvragen van toegang tot een virtuele machine, en meer, via deze API. Zie [Jit-netwerkbeleid](https://docs.microsoft.com/rest/api/securitycenter/jitnetworkaccesspolicies), voor meer informatie over de just in time REST-API.

## <a name="using-just-in-time-vm-access-via-powershell"></a>Met behulp van just-in-time-VM-toegang via PowerShell 

Gebruik de just in time VM access-oplossing via PowerShell, gebruikt u de officiële Azure Security Center PowerShell-cmdlets en met name `Set-AzureRmJitNetworkAccessPolicy`.

Het volgende voorbeeld wordt een just in time-VM toegangsbeleid op een specifieke virtuele machine en Hiermee stelt u het volgende:
1.  Sluit poort 22 en 3389.
2.  Stel een maximale periode van drie uur voor elk, zodat ze kunnen worden geopend per aanvraag goedgekeurd.
3.  Kan de gebruiker die toegang aanvraagt tot het beheren van de bron-IP-adressen en kan de gebruiker tot stand brengen van een geslaagde sessie op een goedgekeurde just-in-time toegangsaanvraag.

Voer het volgende in PowerShell om dit te doen:

1.  Toewijzen van een variabele waarin de just in time-VM toegangsbeleid voor een virtuele machine:

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

2.  Voeg de virtuele machine just-in-time VM access-beleid op een matrix:
    
        $JitPolicyArr=@($JitPolicy)

3.  Configureer de just in time-VM toegangsbeleid op de geselecteerde virtuele machine:
    
        Set-AzureRmJitNetworkAccessPolicy -Kind "Basic" -Location "LOCATION" -Name "default" -ResourceGroupName "RESOURCEGROUP" -VirtualMachine $JitPolicyArr 

### <a name="requesting-access-to-a-vm"></a>Aanvragen van toegang tot een virtuele machine

In het volgende voorbeeld ziet u een just in time VM access aanvraag naar een specifieke virtuele machine in welke poort 22 wordt aangevraagd, worden geopend voor een specifiek IP-adres en voor een bepaalde hoeveelheid tijd:

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

        Start-AzureRmJitNetworkAccessPolicy -ResourceId "/subscriptions/SUBSCRIPTIONID/resourceGroups/RESOURCEGROUP/providers/Microsoft.Security/locations/LOCATION/jitNetworkAccessPolicies/default" -VirtualMachine $JitPolicyArr

Zie voor meer informatie de documentatie van de PowerShell-cmdlet.


## <a name="next-steps"></a>Volgende stappen
In dit artikel hebt u geleerd hoe just-in-time-VM-toegang in Security Center helpt dat u toegang tot uw Azure virtual machines beheren.

Zie de volgende onderwerpen voor meer informatie over het Beveiligingscentrum:

- [Beveiligingsbeleid instellen](security-center-policies.md) : informatie over het configureren van beveiligingsbeleid voor uw Azure-abonnementen en resourcegroepen.
- [Aanbevelingen voor beveiliging beheren](security-center-recommendations.md) : Leer hoe aanbevelingen helpen u uw Azure-resources te beveiligen.
- [Beveiligingsstatus bewaken](security-center-monitoring.md) : informatie over het bewaken van de status van uw Azure-resources.
- [Beheren en erop reageren op beveiligingswaarschuwingen](security-center-managing-and-responding-alerts.md) : informatie over het beheren van en reageren op beveiligingswaarschuwingen.
- [Partneroplossingen bewaken](security-center-partner-solutions.md) : informatie over het bewaken van de status van uw partneroplossingen.
- [Security Center FAQ](security-center-faq.md) : Raadpleeg Veelgestelde vragen over het gebruik van de service.
- [Azure-beveiligingsblog](https://blogs.msdn.microsoft.com/azuresecurity/): lees blogberichten over de beveiliging en naleving van Azure.


<!--Image references-->
[1]: ./media/security-center-just-in-time/just-in-time-scenario.png
[2]: ./media/security-center-just-in-time/just-in-time.png
[10]: ./media/security-center-just-in-time/just-in-time-access.png
[3]: ./media/security-center-just-in-time/enable-just-in-time-access.png
[4]: ./media/security-center-just-in-time/request-access-to-a-vm.png
[5]: ./media/security-center-just-in-time/activity-log.png
[6]: ./media/security-center-just-in-time/default-ports.png
[7]: ./media/security-center-just-in-time/add-a-port.png
[8]: ./media/security-center-just-in-time/edit-policy.png
[9]: ./media/security-center-just-in-time/select-activity-log.png
