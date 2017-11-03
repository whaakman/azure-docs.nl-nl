---
title: Alleen in tijd virtuele machine toegang heeft tot in Azure Security Center | Microsoft Docs
description: Dit document vindt u via hoe alleen in de tijd VM openen in Azure Security Center helpt u toegang tot uw virtuele machines in Azure.
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
ms.date: 10/04/2017
ms.author: terrylan
ms.openlocfilehash: c715afe55a3aedd5c4f826bc34c3c56e167d2f82
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="manage-virtual-machine-access-using-just-in-time-preview"></a>Beheer van de virtuele machine toegang met alleen in de tijd (Preview)

Alleen bij tijd virtuele machine (VM) kan toegang worden gebruikt vergrendelen binnenkomend verkeer naar uw Azure VM's, blootstelling aan aanvallen te verminderen terwijl er eenvoudig toegang krijgen tot het verbinding maken met virtuele machines wanneer deze nodig is.

> [!NOTE]
> De just-in tijd functie in preview is en beschikbaar zijn op de prijscategorie Standard van Security Center.  Zie [prijzen](security-center-pricing.md) voor meer informatie over Security Center de prijscategorie.
>
>

## <a name="attack-scenario"></a>Aanval

Brute force-aanvallen vaak doelpoorten management als een manier toegang te krijgen tot een virtuele machine. Als dit lukt, kan een aanvaller besturen van de virtuele machine en een voet achter de deur tot stand brengen in uw omgeving.

Een manier om te beperken van blootstelling aan een beveiligingsaanval is om te beperken van de hoeveelheid tijd die een poort geopend is. Poorten voor beheertaken hoeft niet te allen tijde moeten worden gebruikt. Ze hoeft alleen te worden gebruikt terwijl u bent verbonden met de virtuele machine, zoals beheer- of taken uit te voeren. Wanneer u in de tijd is ingeschakeld, wordt gebruikt door Security Center [Netwerkbeveiligingsgroep](../virtual-network/virtual-networks-nsg.md) (NSG) regels, die toegang tot beheerpoorten beperken zodat ze niet worden gericht door aanvallers.

![Alleen bij tijd scenario][1]

## <a name="how-does-just-in-time-access-work"></a>Hoe werkt alleen bij het toegang in uitvoeringstijd combinatie?

Wanneer JIT is ingeschakeld, wordt binnenkomend verkeer naar de Azure-VM's vergrendeld via Security Center door een NSG-regel te maken. Selecteert u de poorten op de virtuele machine waarmee binnenkomend verkeer wordt worden vergrendeld. Deze poorten worden beheerd door de zojuist in tijdoplossing.

Wanneer een gebruiker toegang tot een virtuele machine aanvraagt, Security Center controleert of de gebruiker [op rollen gebaseerde toegangsbeheer (RBAC)](../active-directory/role-based-access-control-configure.md) machtigingen die schrijftoegang voor de virtuele machine bieden. Als ze schrijftoegang hebben, de aanvraag is goedgekeurd en Security Center automatisch de Netwerkbeveiligingsgroepen (nsg's configureert) zodat binnenkomend verkeer op de poorten voor de hoeveelheid tijd die u hebt opgegeven. Nadat de respijtperiode is verlopen, wordt het nsg's door Security Center naar de vorige staat hersteld.

> [!NOTE]
> Security Center alleen bij het VM-time-toegang ondersteunt momenteel alleen virtuele machines die zijn geïmplementeerd via Azure Resource Manager. Voor meer informatie over het klassieke en het Resource Manager-implementatiemodel Zie [Azure Resource Manager versus klassieke implementatie](../azure-resource-manager/resource-manager-deployment-model.md).
>
>

## <a name="using-just-in-time-access"></a>Met behulp van alleen bij het time-toegang

De **Just in time VM toegang** tegel onder **Security Center** toont het aantal virtuele machines die zijn geconfigureerd voor alleen bij het toegang in uitvoeringstijd en het aantal goedgekeurd toegangsaanvragen in de afgelopen week.

![Alleen in de tijd VM toegang tegel][2]

Selecteer de **Just in time VM toegang** tegel en **Just in time VM toegang** wordt geopend.

![Alleen in de tijd VM toegang tegel][10]

**Alleen bij het toegang in uitvoeringstijd VM** bevat informatie over de status van uw virtuele machines:

- **Geconfigureerd** -virtuele machines die zijn geconfigureerd om te ondersteunen alleen bij het VM-time-toegang. De gegevens weergegeven voor de afgelopen week en bevat voor elke VM die het aantal goedgekeurde aanvragen, de datum van laatste toegang en de tijd en de laatste gebruiker.
- **Aanbevolen** -VM's die alleen bij het toegang in uitvoeringstijd VM kunnen ondersteunen, maar niet is geconfigureerd voor. Het is raadzaam dat u alleen bij het VM-toegangsbeheer tijd voor deze virtuele machines inschakelt. Zie [een configureren in het toegangsbeleid tijd](#configuring-a-just-in-time-access-policy).
- **Geen aanbeveling** -redenen die kunnen leiden tot een virtuele machine niet aan te bevelen zijn:
  - NSG - de zojuist ontbreekt in de tijd oplossing vereist een NSG aan aanwezig zijn.
  - Klassieke VM - Security Center alleen bij het VM-time-toegang ondersteunt momenteel alleen virtuele machines die zijn geïmplementeerd via Azure Resource Manager. Een klassieke implementatie wordt niet ondersteund door de zojuist in tijdoplossing.
  - Andere - een virtuele machine is in deze categorie als de zojuist tijdig oplossing is uitgeschakeld in het beveiligingsbeleid van het abonnement of de resourcegroep, of dat ontbreekt een openbaar IP-adres en geen een NSG in plaats van de virtuele machine.

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
