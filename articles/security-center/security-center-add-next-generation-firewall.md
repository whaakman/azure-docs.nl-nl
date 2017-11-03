---
title: Next generation firewall toevoegen in Azure Security Center | Microsoft Docs
description: Dit document ziet u hoe de Azure Security Center aanbevelingen implementeren ** toevoegen een volgende generatie Firewall ** en ** Route traffice via NGFW alleen **.
services: security-center
documentationcenter: na
author: TerryLanfear
manager: MBaldwin
editor: 
ms.assetid: 48b99015-4db8-4ce8-85e4-b544c0fa203e
ms.service: security-center
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 02/02/2017
ms.author: terrylan
ms.openlocfilehash: 30589d0a943517c03394a3aae7c03c8094e78c1f
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="add-a-next-generation-firewall-in-azure-security-center"></a>Next Generation Firewall toevoegen in Azure Security Center
Azure Security Center kan aanbevelen next generation firewall (NGFW) toe te voegen uit een Microsoft-partner te verhogen uw beveiligingsinstellingen. Dit document vindt u via een voorbeeld van hoe u dit doet.

> [!NOTE]
> In dit document wordt de service geïntroduceerd aan de hand van een voorbeeldimplementatie.  Dit is geen stapsgewijze handleiding.
>
>

## <a name="implement-the-recommendation"></a>De aanbeveling implementeren
1. In de **aanbevelingen** blade Selecteer **Next Generation Firewall toevoegen**.
   ![Een firewall van de volgende generatie toevoegen][1]
2. In de **Next Generation Firewall toevoegen** blade, selecteert u een eindpunt.
   ![Selecteer een eindpunt][2]
3. Een tweede **Next Generation Firewall toevoegen** blade wordt geopend. U kunt een bestaande oplossing gebruiken indien beschikbaar of u kunt een nieuwe maken. In dit voorbeeld zijn er geen bestaande oplossingen beschikbaar, dus we een NGFW maken.
   ![Next Generation Firewall maken][3]
4. Selecteer een oplossing uit de lijst met geïntegreerde partners voor het maken van een NGFW. In dit voorbeeld selecteren we **Check Point**.
   ![Selecteer Next Generation Firewall-oplossing][4]
5. De **Check Point** blade wordt geopend zodat u informatie over de partneroplossing. Selecteer **maken** in de blade informatie.
   ![Firewall-informatie-blade][5]
6. De **virtuele machine maken** blade wordt geopend. Hier kunt u met een virtuele machine (VM) die wordt uitgevoerd de NGFW ronddraaien vereiste informatie. Volg de stappen en geef de vereiste informatie op NGFW. Klik op OK om toe te passen.
   ![Virtuele machine om uit te voeren NGFW maken][6]

## <a name="route-traffic-through-ngfw-only"></a>Het verkeer alleen via de NGFW leiden
Ga terug naar de **aanbevelingen** blade. Een nieuwe vermelding is gegenereerd nadat u een NGFW via Security Center, aangeroepen toegevoegd **routeren van verkeer via NGFW**. Deze aanbeveling wordt alleen gemaakt als u uw NGFW door Security Center geïnstalleerd. Als u internetgerichte eindpunten hebt, raadt Security Center aan de Netwerkbeveiligingsgroep regels waarmee binnenkomend verkeer naar uw virtuele machine via uw NGFW forceren te configureren.

1. In de **blade aanbevelingen**, selecteer **routeren van verkeer via NGFW**.
   ![Verkeer alleen via NGFW sturen][7]
2. Hiermee opent u de blade **routeren van verkeer via NGFW**, die een lijst met virtuele machines die u kunt het verkeer naar versturen. Selecteer een virtuele machine in de lijst.
   ![Selecteer een virtuele machine][8]
3. Een blade voor de geselecteerde virtuele machine met de bijbehorende regels voor binnenkomende verbindingen wordt geopend. Een beschrijving biedt u meer informatie over het mogelijk de volgende stappen. Selecteer **binnenkomende regels bewerken** om door te gaan met het bewerken van een inkomende regel. De verwachting is dat **bron** niet is ingesteld op **eventuele** voor de internetgerichte eindpunten gekoppeld aan de NGFW. Zie voor meer informatie over de eigenschappen van de binnenkomende regel, [NSG-regels](../virtual-network/virtual-networks-nsg.md#nsg-rules).
   ![Configureer regels om toegang te beperken][9]
   ![binnenkomende regel bewerken][10]

## <a name="see-also"></a>Zie ook
Dit document hebt u geleerd hoe u implementeert de aanbeveling Security Center "Next Generation Firewall toevoegen". Zie de volgende onderwerpen voor meer informatie over NGFWs en de partneroplossing Check Point:

* [Next Generation Firewall](https://en.wikipedia.org/wiki/Next-Generation_Firewall)
* [Check Point vSEC](https://azure.microsoft.com/marketplace/partners/checkpoint/check-point-r77-10/)

Zie de volgende onderwerpen voor meer informatie over het Beveiligingscentrum:

* [Beveiligingsbeleid instellen in Azure Security Center](security-center-policies.md) --informatie over het configureren van beveiligingsbeleid.
* [Aanbevelingen voor beveiliging in Azure Security Center beheren](security-center-recommendations.md) --Leer hoe aanbevelingen u uw Azure-resources te beveiligen.
* [Beveiligingsstatus bewaken in Azure Security Center](security-center-monitoring.md) --informatie over het bewaken van de status van uw Azure-resources.
* [Managing and responding to security alerts in Azure Security Center](security-center-managing-and-responding-alerts.md) (Beveiligingswaarschuwingen beheren en erop reageren in Azure Security Center): ontdek hoe u beveiligingswaarschuwingen kunt beheren en erop kunt reageren.
* [Partneroplossingen bewaken met Azure Security Center](security-center-partner-solutions.md): leer hoe u de integriteitsstatus van uw partneroplossingen kunt bewaken.
* [Azure Security Center FAQ](security-center-faq.md) (Veelgestelde vragen over Azure Security Center): raadpleeg veelgestelde vragen over het gebruik van de service.
* [Azure-Beveiligingsblog](http://blogs.msdn.com/b/azuresecurity/) --Lees blogberichten over Azure-beveiliging en naleving.

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
