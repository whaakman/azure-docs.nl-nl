---
title: Uitgebreide metrische gegevens voor Azure virtual machines toevoegen | Microsoft Docs
description: Dit artikel helpt u bij het inschakelen en metrische gegevens over uitgebreide diagnostische gegevens configureren voor uw Azure VM's.
services: cost-management
keywords: ''
author: bandersmsft
ms.author: banders
ms.date: 06/12/2018
ms.topic: conceptual
ms.service: cost-management
manager: dougeby
ms.custom: ''
ms.openlocfilehash: b7e4665dc3579f357ce1e28bf34be35c931736bd
ms.sourcegitcommit: e8f443ac09eaa6ef1d56a60cd6ac7d351d9271b9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/12/2018
ms.locfileid: "35643935"
---
# <a name="add-extended-metrics-for-azure-virtual-machines"></a>Uitgebreide metrische gegevens voor Azure virtual machines toevoegen

Cost Management maakt gebruik van Azure metrische gegevens van uw Azure-VM's om u gedetailleerde informatie over hun resources weer te geven. Metrische gegevens, prestatiemeteritems, ook met de naam wordt gebruikt door Cost Management om rapporten te genereren. Echter Cost Management wordt niet automatisch verzamelen van alle Azure metrische gegevens van Gast-VM's, moet u metrische gegevens verzameling inschakelen. Dit artikel helpt u bij het inschakelen en configureren van aanvullende metrische gegevens voor uw Azure VM's.

Nadat u het verzamelen van metrische gegevens inschakelt, kunt u het volgende doen:

- Op de hoogte wanneer uw virtuele machines hun geheugen, schijf en CPU-limieten zijn bereikt.
- Detecteer trends in gebruik en afwijkingen.
- Uw kosten te beheren door de grootte op basis van gebruik.
- Get kosten-effectieve formaat aanbevelingen van de optimalisatie van Cost Management.

Bijvoorbeeld, als u wilt het % CPU en geheugen-% van uw virtuele Azure-machines bewaken. De metrische gegevens van virtuele Azure-machine komen overeen met _[Host] Percentage CPU_ en _[Gast] geheugenpercentage_.

> [!NOTE]
> Uitgebreide metrische gegevensverzameling wordt alleen ondersteund met Azure-bewaking op gastniveau. Kostenbeheer is niet compatibel met de Log Analytics VM-extensie.

## <a name="verify-that-metrics-are-enabled-on-vms"></a>Controleer of metrische gegevens zijn ingeschakeld op virtuele machines

1. Meld u aan bij Azure Portal op http://portal.azure.com.
2. Onder **virtuele machines**, selecteert u een virtuele machine en klik vervolgens onder **bewaking**, selecteer **metrische gegevens**. Een lijst met beschikbare metrische gegevens wordt weergegeven.
3. Sommige metrische gegevens selecteren en een grafiek voor deze gegevens worden weergegeven.  
    ![Voorbeeld van de metrische gegevens: host percentage CPU](./media/azure-vm-extended-metrics/metric01.png)

In het voorgaande voorbeeld wordt een beperkte set van standaard metrische gegevens zijn beschikbaar voor uw hosts, maar geheugen metrische gegevens zijn niet. Geheugen metrische gegevens maken deel uit van de uitgebreide metrische gegevens. U moet enkele extra stappen voor het inschakelen van uitgebreide metrische gegevens uitvoeren. De volgende informatie helpt u om hen in staat.

## <a name="enable-extended-metrics-in-the-azure-portal"></a>Uitgebreide metrische gegevens in Azure portal inschakelen

Standaard metrische gegevens zijn computer metrische gegevens voor hosts. De _[Host] Percentage CPU_ meetwaarde is een voorbeeld. Er zijn ook eenvoudige metrische gegevens voor de Gast-VM's en uitgebreide metrische gegevens ook genoemd. Voorbeelden van uitgebreide metrische gegevens zijn _[Gast] geheugenpercentage_ en _[] Gastgeheugen beschikbaar_.

Het inschakelen van uitgebreide metrische gegevens is eenvoudig. Voor elke virtuele machine, schakel de bewaking op gastniveau. Wanneer u de bewaking op gastniveau inschakelt, wordt de Azure diagnostics-agent is geïnstalleerd op de virtuele machine. Het volgende proces is hetzelfde voor klassieke en reguliere virtuele machines en hetzelfde voor Windows en Linux-VM's.

Houd er rekening mee dat Azure- en Linux bewaking op gastniveau zijn vereist voor een opslagaccount. Wanneer u bewaking op gastniveau, inschakelt als u een bestaand opslagaccount niet kiezen, is klikt u vervolgens een gemaakt voor u.

### <a name="enable-guest-level-monitoring-on-existing-vms"></a>Bewaking op gastniveau op bestaande virtuele machines inschakelen

1. In **virtuele Machines**, uw lijst met uw virtuele machines weergeven en selecteer vervolgens een virtuele machine.
2. Onder **bewaking**, selecteer **metrische gegevens**.
3. Klik op **diagnostische instellingen**.
4. Klik op de instellingenpagina van diagnostische gegevens **bewaking op gastniveau inschakelen**.  
    ![Gast niveau bewaking inschakelen](./media/azure-vm-extended-metrics/enable-guest-monitoring.png)
5. Na een paar minuten is de Azure diagnostics-agent geïnstalleerd op de virtuele machine. De pagina vernieuwd en de lijst met beschikbare metrische gegevens is bijgewerkt met metrische gegevens voor gasten.  
    ![Uitgebreide metrische gegevens](./media/azure-vm-extended-metrics/extended-metrics.png)

### <a name="enable-guest-level-monitoring-on-new-vms"></a>Bewaking op gastniveau op nieuwe virtuele machines inschakelen

Zorg ervoor dat u selecteert wanneer u nieuwe virtuele machines maakt, **diagnostische gegevens van Guest OS**.

![Gastbesturingssysteem diagnostische gegevens inschakelen](./media/azure-vm-extended-metrics/new-enable-diag.png)

## <a name="resource-manager-credentials"></a>Resource Manager-referenties

Nadat u uitgebreide metrische gegevens inschakelt, zorg ervoor dat Cost Management toegang tot heeft uw [Resource Manager-referenties](activate-subs-accounts.md). Uw referenties zijn vereist voor het beheren van de kosten voor het verzamelen en prestatiegegevens weergegeven voor uw VM's. Ze worden ook gebruikt om te maken van kosten optimalisatie aanbevelingen. Kostenbeheer moet ten minste drie dagen van de prestatiegegevens van een exemplaar om te bepalen of een kandidaat voor een aanbeveling downsizing.

## <a name="enable-vm-metrics-with-a-script"></a>De metrische gegevens van een virtuele machine met een script inschakelen

U kunt metrische gegevens van virtuele machine met Azure PowerShell-scripts inschakelen. Wanneer u veel virtuele machines die u wilt inschakelen, metrische gegevens hebt, kunt u een script kunt gebruiken om het proces te automatiseren. Van de voorbeeldscripts zijn op GitHub op [Azure inschakelen Diagnostics](https://github.com/Cloudyn/azure-enable-diagnostics).

## <a name="view-azure-performance-metrics"></a>Metrische gegevens weergeven-prestaties van Azure

Als u wilt weergeven van metrische gegevens voor prestaties op uw Azure-exemplaren in de Cloudyn-portal, gaat u naar **activa** > **Compute** > **exemplaar Explorer**. In de lijst met VM-exemplaren, vouw een exemplaar en vouw vervolgens een resource om details te bekijken.

![Exemplaar-Explorer](./media/azure-vm-extended-metrics/instance-explorer.png)

## <a name="next-steps"></a>Volgende stappen

- Als u dit nog niet hebt Azure Resource Manager API-toegang al ingeschakeld voor uw accounts, gaat u verder met [Activate Azure-abonnementen en accounts](activate-subs-accounts.md).
