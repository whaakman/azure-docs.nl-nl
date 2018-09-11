---
title: Next generatiefirewall toevoegen in Azure Security Center | Microsoft Docs
description: Dit document ziet u hoe u de aanbevelingen voor Azure Security Center implementeren **Next Generation Firewall toevoegen** en **Route traffice via ngfw STUREN alleen**.
services: security-center
documentationcenter: na
author: TerryLanfear
manager: MBaldwin
editor: ''
ms.assetid: 48b99015-4db8-4ce8-85e4-b544c0fa203e
ms.service: security-center
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 02/02/2017
ms.author: terrylan
ms.openlocfilehash: ff5c667318f961dddcab02cb1b2b679285d8c790
ms.sourcegitcommit: f3bd5c17a3a189f144008faf1acb9fabc5bc9ab7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/10/2018
ms.locfileid: "44294014"
---
# <a name="add-a-next-generation-firewall-in-azure-security-center"></a>Next Generatiefirewall toevoegen in Azure Security Center
Azure Security Center kan raden u aan next generatiefirewall (NGFW) via een Microsoft-partner uw beveiligingen verhogen. In dit document wordt u via een voorbeeld van hoe u dit doet.

> [!NOTE]
> In dit document wordt de service geïntroduceerd aan de hand van een voorbeeldimplementatie.  Dit is geen stapsgewijze handleiding.
>
>

## <a name="implement-the-recommendation"></a>De aanbeveling voor het implementeren
1. In de **aanbevelingen** Selecteer **Next Generation Firewall toevoegen**.
   ![Een firewall van de volgende generatie toevoegen][1]
2. In de **Next Generation Firewall toevoegen** blade, selecteert u een eindpunt.
   ![Selecteer een eindpunt][2]
3. Een tweede **Next Generation Firewall toevoegen** blade wordt geopend. U kunt een bestaande oplossing gebruiken als beschikbaar of u kunt een nieuwe groep maken. In dit voorbeeld zijn er geen bestaande oplossingen beschikbaar, zodat we een NGFW maken.
   ![Next Generatiefirewall maken][3]
4. Voor het maken van een NGFW selecteert u een oplossing uit de lijst met geïntegreerde partners. In dit voorbeeld selecteren we **Check Point**.
   ![Selecteer Next Generation Firewall-oplossing][4]
5. De **Check Point** blade wordt geopend zodat u meer informatie over de partneroplossing. Selecteer **maken** in de blade informatie.
   ![Blade firewall-informatie][5]
6. De **maken van virtuele machine** blade wordt geopend. Hier kunt u vereiste informatie op voor een virtuele machine (VM) die wordt uitgevoerd de NGFW instellen. Volg de stappen en de vereiste NGFW gegevens verstrekken. Klik op OK om toe te passen.
   ![Virtuele machine om uit te voeren NGFW maken][6]

## <a name="route-traffic-through-ngfw-only"></a>Verkeer alleen via NGFW sturen
Ga terug naar de **aanbevelingen** blade. Een nieuwe vermelding is gegenereerd nadat u een NGFW via Security Center, met de naam toegevoegd **routeren van verkeer alleen via NGFW**. Deze aanbeveling wordt alleen gemaakt als u uw NGFW via Security Center hebt geïnstalleerd. Als u internetgerichte eindpunten hebt, raadt Security Center aan dat u configureert Network Security Group-regels waarmee binnenkomend verkeer op uw virtuele machine via de NGFW afgedwongen.

1. In de **blade aanbevelingen**, selecteer **routeren van verkeer alleen via NGFW**.
   ![Verkeer alleen via NGFW sturen][7]
2. Hiermee opent u de blade **routeren van verkeer alleen via NGFW**, die een lijst met virtuele machines die u kunt verkeer te routeren. Selecteer een VM in de lijst.
   ![Selecteer een virtuele machine][8]
3. Een blade voor de geselecteerde virtuele machine met de verwante binnenkomende regels wordt geopend. Een beschrijving biedt u meer informatie over de Vervolgstappen die u mogelijk. Selecteer **binnenkomende regels bewerken** om door te gaan met het bewerken van een binnenkomende regel. De verwachting is dat **bron** niet is ingesteld op **eventuele** voor de internetgerichte eindpunten die is gekoppeld met de NGFW. Zie voor meer informatie over de eigenschappen van de binnenkomende regel, [beveiligingsregels](../virtual-network/security-overview.md#security-rules).
   ![Configureer regels om toegang te beperken][9]
   ![binnenkomende regel bewerken][10]

## <a name="see-also"></a>Zie ook
Dit document hebt u geleerd hoe u voor het implementeren van de aanbeveling van Security Center 'Next Generatiefirewall toevoegen'. Voor meer informatie over NGFWs en de partneroplossing Check Point, Zie de volgende:

* [Next Generation Firewall](https://en.wikipedia.org/wiki/Next-Generation_Firewall)
* [Check Point vSEC](https://azuremarketplace.microsoft.com/en-us/marketplace/apps/checkpoint.vsec)

Zie de volgende onderwerpen voor meer informatie over het Beveiligingscentrum:

* [Beveiligingsbeleid instellen in Azure Security Center](security-center-policies.md) --informatie over het configureren van beveiligingsbeleid.
* [Aanbevelingen voor beveiliging in Azure Security Center beheren](security-center-recommendations.md) --Leer hoe aanbevelingen helpen u uw Azure-resources te beveiligen.
* [Beveiligingsstatus bewaken in Azure Security Center](security-center-monitoring.md) --informatie over het bewaken van de status van uw Azure-resources.
* [Managing and responding to security alerts in Azure Security Center](security-center-managing-and-responding-alerts.md) (Beveiligingswaarschuwingen beheren en erop reageren in Azure Security Center): ontdek hoe u beveiligingswaarschuwingen kunt beheren en erop kunt reageren.
* [Partneroplossingen bewaken met Azure Security Center](security-center-partner-solutions.md): leer hoe u de integriteitsstatus van uw partneroplossingen kunt bewaken.
* [Azure Security Center FAQ](security-center-faq.md) (Veelgestelde vragen over Azure Security Center): raadpleeg veelgestelde vragen over het gebruik van de service.
* [Azure-Beveiligingsblog](http://blogs.msdn.com/b/azuresecurity/) : Lees blogberichten over de Azure-beveiliging en naleving.

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
