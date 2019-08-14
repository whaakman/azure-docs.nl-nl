---
title: Een firewall van de volgende generatie toevoegen in Azure Security Center | Microsoft Docs
description: In dit document wordt beschreven hoe u de Azure Security Center aanbevelingen implementeert u **een volgende generatie firewall** en **stuurt u verkeer alleen via NGFW**.
services: security-center
documentationcenter: na
author: rkarlin
manager: barbkess
editor: ''
ms.assetid: 48b99015-4db8-4ce8-85e4-b544c0fa203e
ms.service: security-center
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 10/28/2018
ms.author: rkarlin
ms.openlocfilehash: 731102037b596091b80fbdfa02a8ff3c111b556e
ms.sourcegitcommit: 0f54f1b067f588d50f787fbfac50854a3a64fff7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/12/2019
ms.locfileid: "60706983"
---
# <a name="add-a-next-generation-firewall-in-azure-security-center"></a>Een firewall van de volgende generatie toevoegen in Azure Security Center
Azure Security Center kan worden aangeraden een firewall van de volgende generatie (NGFW) toe te voegen van een micro soft-partner om uw beveiligings beveiligingen te verhogen. In dit document vindt u een voor beeld van hoe u dit doet.

> [!NOTE]
> In dit document wordt de service geïntroduceerd aan de hand van een voorbeeldimplementatie.  Dit is geen stapsgewijze handleiding.
>
>

## <a name="implement-the-recommendation"></a>De aanbeveling implementeren
1. Selecteer op de Blade aanbevelingen **een volgende generatie firewall toevoegen**.
   ![Een firewall van de volgende generatie toevoegen][1]
2. Selecteer een eind punt in de Blade **een firewall van de volgende generatie toevoegen** .
   ![Een eind punt selecteren][2]
3. Een tweede Blade **firewall van de volgende generatie toevoegen** wordt geopend. U kunt ervoor kiezen om een bestaande oplossing te gebruiken als deze beschikbaar is, of u kunt een nieuwe maken. In dit voor beeld zijn er geen bestaande oplossingen beschikbaar, dus we maken een NGFW.
   ![Een firewall van de volgende generatie maken][3]
4. Als u een NGFW wilt maken, selecteert u een oplossing in de lijst met geïntegreerde partners. In dit voor beeld selecteren we **Check Point**.
   ![De oplossing voor de volgende generatie firewall selecteren][4]
5. De Blade **controle punt** wordt geopend met informatie over de partner oplossing. Selecteer **maken** op de Blade informatie.
   ![Blade firewall gegevens][5]
6. De Blade **virtuele machine maken** wordt geopend. Hier kunt u informatie invoeren die is vereist om een virtuele machine (VM) op te geven waarop de NGFW wordt uitgevoerd. Volg de stappen en geef de vereiste NGFW-informatie op. Selecteer OK om toe te passen.
   ![Een virtuele machine maken om NGFW uit te voeren][6]

## <a name="route-traffic-through-ngfw-only"></a>Het verkeer alleen via de NGFW leiden
Ga terug naar de Blade **aanbevelingen** . Er is een nieuwe vermelding gegenereerd nadat u een NGFW hebt toegevoegd via Security Center, met de naam **route verkeer via NGFW**. Deze aanbeveling wordt alleen gemaakt als u uw NGFW via Security Center hebt geïnstalleerd. Als u Internet gerichte eind punten hebt, raadt Security Center u aan om regels voor netwerk beveiligings groepen te configureren die inkomend verkeer naar uw virtuele machine forceren via uw NGFW.

1. Selecteer inde Blade aanbevelingen **alleen routerings verkeer via NGFW**.
   ![Verkeer alleen via NGFW sturen][7]
2. Hiermee opent u de Blade **route verkeer via NGFW**, die de virtuele machines vermeldt waarnaar u verkeer kunt routeren. Selecteer een VM in de lijst.
   ![Een virtuele machine selecteren][8]
3. Er wordt een Blade voor de geselecteerde virtuele machine geopend waarin gerelateerde regels voor binnenkomende verbindingen worden weer gegeven. Een beschrijving bevat meer informatie over mogelijke volgende stappen. Selecteer **regels voor binnenkomende verbindingen bewerken** om door te gaan met het bewerken van een regel voor binnenkomende verbindingen. De verwachting is dat de **bron** niet is ingesteld op **een** voor de Internet gerichte eind punten die zijn gekoppeld aan de NGFW. Zie [beveiligings regels](../virtual-network/security-overview.md#security-rules)voor meer informatie over de eigenschappen van de regel voor binnenkomende verbindingen.
   ![Regels configureren om de binnenkomende][9]
   regel voor de toegang![te beperken][10]

## <a name="see-also"></a>Zie ook
In dit document wordt uitgelegd hoe u de Security Center aanbeveling ' een volgende generatie firewall toevoegen ' implementeert. Voor meer informatie over NGFWs en de Check Point-partner oplossing raadpleegt u het volgende:

* [Firewall van de volgende generatie](https://en.wikipedia.org/wiki/Next-Generation_Firewall)
* [Controle punt vSEC](https://azuremarketplace.microsoft.com/en-us/marketplace/apps/checkpoint.vsec)

Zie de volgende onderwerpen voor meer informatie over het Beveiligingscentrum:

* [Beveiligings beleid instellen in azure Security Center](tutorial-security-policy.md) --meer informatie over het configureren van beveiligings beleid.
* [Aanbevelingen voor beveiliging in azure Security Center](security-center-recommendations.md) : Ontdek hoe aanbevelingen u helpen uw Azure-resources te beveiligen.
* [Beveiligings status controleren in azure Security Center](security-center-monitoring.md) --meer informatie over het controleren van de status van uw Azure-resources.
* [Managing and responding to security alerts in Azure Security Center](security-center-managing-and-responding-alerts.md) (Beveiligingswaarschuwingen beheren en erop reageren in Azure Security Center): ontdek hoe u beveiligingswaarschuwingen kunt beheren en erop kunt reageren.
* [Partneroplossingen bewaken met Azure Security Center](security-center-partner-solutions.md): leer hoe u de integriteitsstatus van uw partneroplossingen kunt bewaken.
* [Azure Security Center FAQ](security-center-faq.md) (Veelgestelde vragen over Azure Security Center): raadpleeg veelgestelde vragen over het gebruik van de service.
* [Azure-beveiligings blog](https://blogs.msdn.com/b/azuresecurity/) : vind blog berichten over beveiliging en naleving van Azure.

<!--Image references-->
[1]: ./media/security-center-add-next-gen-firewall/add-next-gen-firewall.png
[2]: ./media/security-center-add-next-gen-firewall/select-an-endpoint.png
[3]: ./media/security-center-add-next-gen-firewall/create-new-next-gen-firewall.png
[4]: ./media/security-center-add-next-gen-firewall/select-next-gen-firewall.png
[5]: ./media/security-center-add-next-gen-firewall/firewall-solution-info-blade.png
[6]: ./media/security-center-add-next-gen-firewall/create-virtual-machine.png
[7]: ./media/security-center-add-next-gen-firewall/route-traffic-through-ngfw.png
[8]: ./media/security-center-add-next-gen-firewall/select-vm.png
[9]: ./media/security-center-add-next-gen-firewall/configure-rules-to-limit-access.png
[10]: ./media/security-center-add-next-gen-firewall/edit-inbound-rule.png
