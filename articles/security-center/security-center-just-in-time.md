---
title: Alleen in tijd virtuele machine toegang heeft tot in Azure Security Center | Microsoft Docs
description: Dit document wordt gedemonstreerd hoe alleen in de tijd toegang van de virtuele machine in Azure Security Center helpt u bij toegang tot uw virtuele machines in Azure beheren.
services: security-center
documentationcenter: na
author: TerryLanfear
manager: MBaldwin
editor: 
ms.assetid: 
ms.service: security-center
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/06/2018
ms.author: terrylan
ms.openlocfilehash: f1ea31d1081bc263cf85cf4dcc3d73d4cc0b842d
ms.sourcegitcommit: 168426c3545eae6287febecc8804b1035171c048
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/08/2018
---
# <a name="manage-virtual-machine-access-using-just-in-time"></a>Virtuele machine toegang met behulp van in de tijd beheren

Alleen bij tijd virtuele machine (VM) kan toegang worden gebruikt vergrendelen binnenkomend verkeer naar uw Azure VM's, blootstelling aan aanvallen te verminderen terwijl er eenvoudig toegang krijgen tot het verbinding maken met virtuele machines wanneer deze nodig is.

> [!NOTE]
> De NET tijdig functie is beschikbaar op de prijscategorie Standard van Security Center.  Zie [prijzen](security-center-pricing.md) voor meer informatie over Security Center de prijscategorie.
>
>

## <a name="attack-scenario"></a>Aanval

Brute force-aanvallen vaak doelpoorten management als een manier toegang te krijgen tot een virtuele machine. Als dit lukt, kan een aanvaller besturen van de virtuele machine en een voet achter de deur tot stand brengen in uw omgeving.

Een manier om te beperken van blootstelling aan een beveiligingsaanval is om te beperken van de hoeveelheid tijd die een poort geopend is. Beheerpoorten hoeven niet te allen tijde geopend te zijn. Ze hoeven alleen geopend te zijn wanneer u bent verbonden met de VM, bijvoorbeeld om beheer- of onderhoudstaken uit te voeren. Wanneer u in de tijd is ingeschakeld, wordt gebruikt door Security Center [Netwerkbeveiligingsgroep](../virtual-network/virtual-networks-nsg.md) (NSG) regels, die toegang tot beheerpoorten beperken zodat ze niet worden gericht door aanvallers.

![Alleen bij tijd scenario][1]

## <a name="how-does-just-in-time-access-work"></a>Hoe werkt alleen bij het toegang in uitvoeringstijd combinatie?

Wanneer JIT is ingeschakeld, wordt binnenkomend verkeer naar de Azure-VM's vergrendeld via Security Center door een NSG-regel te maken. Selecteert u de poorten op de virtuele machine waarmee binnenkomend verkeer wordt worden vergrendeld. Deze poorten worden beheerd door de zojuist in tijdoplossing.

Wanneer een gebruiker toegang tot een virtuele machine aanvraagt, Security Center controleert of de gebruiker [op rollen gebaseerde toegangsbeheer (RBAC)](../active-directory/role-based-access-control-configure.md) machtigingen die schrijftoegang voor de virtuele machine bieden. Als ze schrijftoegang hebben, de aanvraag is goedgekeurd en Security Center automatisch de Netwerkbeveiligingsgroepen (nsg's configureert) zodat binnenkomend verkeer op de poorten voor de hoeveelheid tijd die u hebt opgegeven. Nadat de respijtperiode is verlopen, wordt het nsg's door Security Center naar de vorige staat hersteld.

> [!NOTE]
> Security Center alleen bij het VM-time-toegang ondersteunt momenteel alleen virtuele machines die zijn geïmplementeerd via Azure Resource Manager. Voor meer informatie over het klassieke en het Resource Manager-implementatiemodel Zie [Azure Resource Manager versus klassieke implementatie](../azure-resource-manager/resource-manager-deployment-model.md).
>
>

## <a name="using-just-in-time-access"></a>Met behulp van alleen bij het time-toegang

1. Open het dashboard van **Security Center**.

2. Selecteer in het linkerdeelvenster **Just in time VM toegang**.

![Alleen in de tijd VM toegang tegel][2]

De **Just in time VM toegang** venster wordt geopend.

![Alleen in de tijd VM toegang tegel][10]

**Just-In-Time-VM-toegang** biedt informatie over de status van uw VM's:

- **Geconfigureerd**: VM's die zijn geconfigureerd om Just-In-Time-VM-toegang te ondersteunen. De gegevens weergegeven voor de afgelopen week en bevat voor elke VM die het aantal goedgekeurde aanvragen, de datum van laatste toegang en de tijd en de laatste gebruiker.
- **Aanbevolen**: VM's die Just-In-Time-VM-toegang kunnen ondersteunen maar er niet voor zijn geconfigureerd. Het is raadzaam dat u alleen bij het VM-toegangsbeheer tijd voor deze virtuele machines inschakelt. Zie [een configureren in het toegangsbeleid tijd](#configuring-a-just-in-time-access-policy).
- **Geen aanbeveling**: redenen waarom een VM mogelijk niet wordt aanbevolen zijn:
  - NSG ontbreekt: de Just-In-Time-oplossing vereist dat er een NSG aanwezig is.
  - Klassieke VM: Just-In-Time-VM-toegang van Security Center ondersteunt momenteel alleen VM's die zijn geïmplementeerd via Azure Resource Manager. Een klassieke implementatie wordt niet ondersteund door de zojuist in tijdoplossing.
  - Overig: een VM valt in deze categorie als de Just-In-Time-oplossing is uitgeschakeld in het beveiligingsbeleid van het abonnement of de resourcegroep, of als de VM geen openbaar IP-adres heeft en geen NSG heeft.

## <a name="configuring-a-just-in-time-access-policy"></a>Configureren van een in het toegangsbeleid tijd

Selecteer de virtuele machines die u wilt inschakelen:

1. Onder **Just in time VM toegang**, selecteer de **aanbevolen** tabblad.

  ![Time-toegang inschakelen][3]

2. Onder **virtuele MACHINE**, selecteert u de virtuele machines die u wilt inschakelen. Hiermee wordt het selectievakje naast een virtuele machine geplaatst.
3. Selecteer **JIT op virtuele machines inschakelen**.
4. Selecteer **Opslaan**.

### <a name="default-ports"></a>Standaard-poorten

Hier ziet u de standaard-poorten die door Security Center aanbevolen inschakelen in de tijd.

1. Onder **Just in time VM toegang**, selecteer de **aanbevolen** tabblad.

  ![Standaardpoorten weergeven][6]

2. Onder **VMs**, selecteert u een virtuele machine. Hiermee plaatst u het selectievakje naast de virtuele machine en Hiermee opent u **JIT VM-configuratie**. Deze blade geeft de standaardpoorten.

### <a name="add-ports"></a>Poorten toevoegen

Onder **JIT VM-configuratie**, kunt u ook toevoegen en configureren van een nieuwe poort waarop u wilt de zojuist inschakelen in tijdoplossing.

1. Onder **JIT VM-configuratie**, selecteer **toevoegen**. Hiermee opent u **toevoegen poortconfiguratie**.

  ![Poortconfiguratie][7]

2. Onder **toevoegen poortconfiguratie**, u de poort, protocoltype, toegestane bron-IP-adressen en maximale aanvraagtijd identificeren.

  Toegestane bron-IP-adressen zijn de IP-adresbereiken mag krijgen van toegang op een verzoek goedgekeurd.

  Maximale aanvraagtijd is de maximale tijdsduur dat een specifieke poort kan worden geopend.

3. Selecteer **OK**.

## <a name="requesting-access-to-a-vm"></a>Aanvragen van toegang tot een virtuele machine

Om toegang te vragen voor een virtuele machine:

1. Onder **Just in time VM toegang**, selecteer de **geconfigureerde** tabblad.
2. Onder **VMs**, selecteert u de virtuele machines die u toegang wilt inschakelen. Hiermee wordt het selectievakje naast een virtuele machine geplaatst.
3. Selecteer **toegang aanvragen**. Hiermee opent u **toegang aanvragen**.

  ![Toegang vragen tot een virtuele machine][4]

4. Onder **toegang aanvragen**, u configureert voor elke virtuele machine de poorten te openen samen met de bron-IP die de poort is geopend en de periode waarvoor u de poort is geopend. U kunt alleen toegang tot de poorten die zijn geconfigureerd in de zojuist aanvragen in tijd beleid. Elke poort heeft de maximaal toegestane tijd die is afgeleid van de zojuist in tijd beleid.
5. Selecteer **poorten openen**.

## <a name="editing-a-just-in-time-access-policy"></a>Een bewerking in het toegangsbeleid tijd

U kunt wijzigen van een virtuele machine bestaande net in tijd beleid door het toevoegen en configureren van een nieuwe poort voor die VM openen of door andere parameters die betrekking hebben op een al beveiligde poort wijzigen.

Om te bewerken van een bestaande alleen bij het beleid van de tijd van een VM de **geconfigureerde** tabblad wordt gebruikt:

1. Onder **VMs**, selecteert u een virtuele machine een poort toevoegen door te klikken op de drie puntjes binnen de rij voor die VM. Hiermee opent u een menu.
2. Selecteer **bewerken** in het menu. Hiermee opent u **JIT VM-configuratie**.

  ![Beleid bewerken][8]

3. Onder **JIT VM-configuratie**, u kunt de bestaande instellingen van een poort al beveiligde voor installatie zonder toezicht bewerken door te klikken op de poort of kunt u **toevoegen**. Hiermee opent u **toevoegen poortconfiguratie**.

  ![Een poort toevoegen][7]

4. Onder **toevoegen poortconfiguratie**, de poort herkennen, protocoltype bron-IP-adressen en de maximale duur van aanvraag toegestaan.
5. Selecteer **OK**.
6. Selecteer **Opslaan**.

## <a name="auditing-just-in-time-access-activity"></a>Controleren in activiteit voor het openen

U kunt Verkrijg inzicht in de VM-activiteiten logboek zoekactie. Logboeken bekijken:

1. Onder **Just in time VM toegang**, selecteer de **geconfigureerde** tabblad.
2. Onder **VMs**, selecteert u een virtuele machine om informatie te bekijken over door te klikken op de drie puntjes binnen de rij voor die VM. Hiermee opent u een menu.
3. Selecteer **activiteitenlogboek** in het menu. Hiermee opent u **activiteitenlogboek**.

  ![Activiteitenlogboek selecteren][9]

  **Activiteitenlogboek** biedt een gefilterde weergave van de vorige bewerkingen voor die VM samen met de tijd, datum en -abonnement.

  ![Logboek van de activiteit weergeven][5]

U kunt de informatie in het foutenlogboek downloaden door het selecteren van **Klik hier om alle items downloaden als CSV**.

Wijzigen van de filters en selecteer **toepassen** voor het maken van een zoekopdracht en logboekbestanden.

## <a name="using-just-in-time-vm-access-via-powershell"></a>Met behulp van alleen in de tijd VM toegang via PowerShell

Als u wilt gebruiken de zojuist in tijdoplossing via PowerShell, zorg ervoor dat u hebt de [nieuwste](/powershell/azure/install-azurerm-ps) Azure PowerShell-versie.
Als u dit doet, moet u voor het installeren van de [nieuwste](https://aka.ms/asc-psgallery) Azure Security Center van de PowerShell gallery.

### <a name="configuring-a-just-in-time-policy-for-a-vm"></a>Configureren van een in tijd beleid voor een virtuele machine

Voor het configureren van een in tijd beleid op een specifieke virtuele machine, moet u deze opdracht uitvoeren in uw PowerShell-sessie: Set-ASCJITAccessPolicy.
Voer de cmdlet-documentatie voor meer informatie.

### <a name="requesting-access-to-a-vm"></a>Aanvragen van toegang tot een virtuele machine

Voor toegang tot een specifieke virtuele machine die wordt beveiligd met de zojuist in tijdoplossing, moet u deze opdracht uitvoeren in uw PowerShell-sessie: aanroepen ASCJITAccess.
Voer de cmdlet-documentatie voor meer informatie.

## <a name="next-steps"></a>Volgende stappen
In dit artikel hebt u geleerd hoe alleen in de tijd VM-toegang in Security Center helpt dat u toegang tot uw virtuele machines in Azure beheren.

Zie de volgende onderwerpen voor meer informatie over het Beveiligingscentrum:

- [Beveiligingsbeleid instellen](security-center-policies.md) : informatie over het beveiligingsbeleid voor uw Azure-abonnementen en resourcegroepen configureren.
- [Aanbevelingen voor beveiliging beheren](security-center-recommendations.md) : Leer hoe aanbevelingen u uw Azure-resources te beveiligen.
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
