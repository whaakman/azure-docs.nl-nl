---
title: Uitgebreide metrische gegevens voor Azure virtual machines toevoegen | Microsoft Docs
description: In dit artikel helpt u bij het inschakelen en configureren van uitgebreide diagnostische gegevens metrische gegevens voor uw Azure VM's.
services: cost-management
keywords: ''
author: bandersmsft
ms.author: banders
ms.date: 06/07/2018
ms.topic: conceptual
ms.service: cost-management
manager: dougeby
ms.custom: ''
ms.openlocfilehash: 58245478cf49c030c435b487e233bbc893a2b9a3
ms.sourcegitcommit: 6f6d073930203ec977f5c283358a19a2f39872af
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/11/2018
ms.locfileid: "35296353"
---
# <a name="add-extended-metrics-for-azure-virtual-machines"></a>Uitgebreide metrische gegevens voor Azure virtual machines toevoegen

Kosten Management maakt gebruik van Azure metrische gegevens van uw Azure VM's om weer te geven dat u gedetailleerde informatie over hun resources. Metrische gegevens, prestatiemeteritems, ook wel wordt gebruikt door kostenbeheer om rapporten te genereren. Echter kostenbeheer wordt niet automatisch alle Azure metrische gegevens verzamelen van Gast-VM's, moet u metrische verzameling inschakelen. In dit artikel helpt u bij het inschakelen en configureren van aanvullende diagnostische gegevens metrische gegevens voor uw Azure VM's.

Nadat u metrische verzameling inschakelt, kunt u:

- Weten wanneer uw virtuele machines hun geheugen, schijf- en CPU-limieten zijn bereikt.
- Trends in gebruik en afwijkingen gedetecteerd.
- De kosten beheren door sizing volgens de informatie over het gebruik.
- Kosten effectieve sizing optimalisatie aanbevelingen uit het beheer van kosten worden opgehaald.

U wilt bewaken van de CPU-percentage en geheugen-% van uw Azure VM's. De virtuele machine van Azure metrische gegevens komen overeen met _[Host] Percentage CPU_ en _[Gast] geheugenpercentage_.

## <a name="verify-that-metrics-are-enabled-on-vms"></a>Controleer of metrische gegevens zijn ingeschakeld op virtuele machines

1. Meld u aan bij Azure Portal op http://portal.azure.com.
2. Onder **virtuele machines**, selecteert u een virtuele machine en klik vervolgens onder **bewaking**, selecteer **metrische gegevens**. Een lijst met beschikbare metrische gegevens weergegeven.
3. Selecteer enkele metrische gegevens en een grafiek voor deze gegevens worden weergegeven.  
    ![Voorbeeld metriek – host percentage CPU](./media/azure-vm-extended-metrics/metric01.png)

Een beperkte set van standaard metrische gegevens beschikbaar zijn voor uw hosts in het voorgaande voorbeeld, maar geheugen metrische gegevens zijn niet. Metrische gegevens geheugen uitmaken deel van uitgebreide metrische gegevens. Een aantal extra stappen uitvoeren om uitgebreide metrische gegevens inschakelen, moet u uitvoeren. De volgende informatie helpt u bij deze wilt inschakelen.

## <a name="enable-extended-metrics-in-the-azure-portal"></a>Uitgebreide metrische gegevens in de Azure portal inschakelen

Standaard metrische gegevens zijn host computer metrische gegevens. De _[Host] Percentage CPU_ metriek is een voorbeeld. Er zijn ook basismetrieken voor gast-VM's en uitgebreide metrische gegevens ook genoemd. Voorbeelden van uitgebreide metrische gegevens zijn _[Gast] geheugenpercentage_ en _[] Gastgeheugen beschikbaar_.

Het inschakelen van uitgebreide metrische gegevens is eenvoudig. Voor elke VM gastniveau-controle inschakelen. Wanneer u gastniveau monitoring inschakelt, wordt de Azure diagnostics-agent is geïnstalleerd op de virtuele machine. Het volgende proces is hetzelfde voor klassieke en reguliere VM's en hetzelfde voor Windows en Linux-machines.

### <a name="enable-guest-level-monitoring-on-existing-vms"></a>Gastniveau bewaking op bestaande virtuele machines inschakelen

1. In **virtuele Machines**, een lijst met uw virtuele machines en selecteer vervolgens een virtuele machine.
2. Onder **bewaking**, selecteer **metrische gegevens**.
3. Klik op **diagnostische instellingen**.
4. Klik op de pagina van de instellingen voor diagnostische gegevens **gastniveau bewaking inschakelen**. Virtuele Linux-machines vereisen een bestaand opslagaccount. Als u een opslagaccount niet voor een virtuele machine van Windows kiezen, is voor u een gemaakt.  
    ![Gast niveau bewaking inschakelen](./media/azure-vm-extended-metrics/enable-guest-monitoring.png)
5. Na een paar minuten, is de Azure diagnostics-agent geïnstalleerd op de virtuele machine. Vernieuw de pagina en de lijst met beschikbare metrische gegevens is bijgewerkt met de Gast metrische gegevens.  
    ![Uitgebreide metrische gegevens](./media/azure-vm-extended-metrics/extended-metrics.png)

### <a name="enable-guest-level-monitoring-on-new-vms"></a>Gastniveau controle op de nieuwe virtuele machines inschakelen

Zorg ervoor dat u selecteert wanneer u nieuwe virtuele machines maakt, **Gastbesturingssysteem diagnostics**. Virtuele Linux-machines vereisen een bestaand opslagaccount. Als u een opslagaccount niet voor een virtuele machine van Windows kiezen, is voor u een gemaakt.

![Gastbesturingssysteem diagnostische gegevens inschakelen](./media/azure-vm-extended-metrics/new-enable-diag.png)

## <a name="resource-manager-credentials"></a>Resource Manager-referenties

Nadat u uitgebreide metrische gegevens inschakelen, zorg ervoor dat kostenbeheer toegang tot heeft uw [Resource Manager referenties](activate-subs-accounts.md). Uw referenties zijn vereist voor het beheren van de kosten voor het verzamelen en prestatiegegevens weergegeven voor uw virtuele machines. Deze worden ook gebruikt voor het maken van kosten optimalisatie aanbevelingen. Beheer van kosten moet ten minste drie dagen van de prestatiegegevens van een exemplaar om te bepalen of deze geschikt is voor een aanbeveling downsizing.

## <a name="enable-vm-metrics-with-a-script"></a>Metrische gegevens van virtuele machine met een script inschakelen

U kunt metrische gegevens van virtuele machine met Azure PowerShell-scripts inschakelen. Wanneer u veel virtuele machines die u wilt inschakelen, metrische gegevens hebt, kunt u een script kunt gebruiken om het proces te automatiseren. Voorbeeldscripts zijn op GitHub op [Azure inschakelen Diagnostics](https://github.com/Cloudyn/azure-enable-diagnostics).

## <a name="view-azure-performance-metrics"></a>Metrische gegevens weergeven Azure prestaties

Als u wilt weergeven maatstaven voor prestaties voor uw Azure-exemplaren in de portal Cloudyn, gaat u naar **activa** > **Compute** > **exemplaar Explorer**. Vouw een exemplaar in de lijst met VM-instanties, en vouw een resource om details te bekijken.

![Exemplaar Explorer](./media/azure-vm-extended-metrics/instance-explorer.png)

## <a name="next-steps"></a>Volgende stappen

- Als u dit nog niet hebt Azure Resource Manager-API-toegang al ingeschakeld voor uw accounts, gaat u verder met [activeren Azure-abonnementen en accounts](activate-subs-accounts.md)
