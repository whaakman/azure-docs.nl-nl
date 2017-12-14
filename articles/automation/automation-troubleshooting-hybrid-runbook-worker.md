---
title: Problemen oplossen met Azure Automation Hybrid Runbook Worker | Microsoft Docs
description: De symptomen, oorzaken en naamomzetting voor de meest voorkomende problemen met Hybrid Runbook Worker in Azure Automation beschreven.
services: automation
documentationcenter: 
author: georgewallace
manager: jwhit
editor: tysonn
ms.assetid: 02c6606e-8924-4328-a196-45630c2255e9
ms.service: automation
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 07/25/2017
ms.author: magoedte
ms.openlocfilehash: 75f4ac1bc940a2b1d8e4ac6aeac8b80c642489da
ms.sourcegitcommit: fa28ca091317eba4e55cef17766e72475bdd4c96
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/14/2017
---
# <a name="troubleshooting-tips-for-hybrid-runbook-worker"></a>Tips voor probleemoplossing voor hybride Runbook Worker

Dit artikel bevat help het oplossen van problemen die u mogelijk ondervindt met Automation Hybrid Runbook Workers en mogelijke oplossingen om op te lossen ze voorgesteld.

## <a name="a-runbook-job-terminates-with-a-status-of-suspended"></a>Een runbooktaak wordt beëindigd met de status onderbroken

Uw runbook is onderbroken kort na het uitvoeren van deze drie keer. Gelden er bepaalde voorwaarden die het runbook is voltooid, kunnen worden onderbroken en het bijbehorende foutbericht bevat geen aanvullende informatie die aangeeft waarom. Dit artikel bevat stappen voor probleemoplossing voor problemen met betrekking tot fouten bij de uitvoering van de Hybrid Runbook Worker runbook.

Als uw Azure probleem niet wordt besproken in dit artikel, gaat u naar de Azure-forums op [MSDN en de Stack Overflow](https://azure.microsoft.com/support/forums/). U kunt het probleem op deze forums of boeken [ @AzureSupport op Twitter](https://twitter.com/AzureSupport). U kunt ook een ondersteuning van Azure-aanvraag indienen door te selecteren **ondersteuning krijgen** op de [ondersteuning van Azure](https://azure.microsoft.com/support/options/) site.

### <a name="symptom"></a>Symptoom
Runbook-uitvoering mislukt en de volgende fout is geretourneerd, 'de taakactie 'Activeren' kan niet worden uitgevoerd omdat het proces onverwacht is gestopt. De taakactie is 3 maal geprobeerd."

Er zijn verschillende mogelijke oorzaken voor de volgende fout: 

1. De hybride worker zich achter een proxy of firewall
2. De computer die de hybride worker op wordt uitgevoerd heeft minder dan het minimum [hardwarevereisten](automation-offering-get-started.md#hybrid-runbook-worker)  
3. De runbooks verifiëren niet met lokale bronnen

#### <a name="cause-1-hybrid-runbook-worker-is-behind-proxy-or-firewall"></a>1 oorzaak: Hybrid Runbook Worker wordt achter een proxy of firewall
De computer die de hybride Runbook Worker wordt uitgevoerd op zich achter een firewall of proxy-server en uitgaande netwerktoegang niet toegestaan of correct geconfigureerd.

#### <a name="solution"></a>Oplossing
Controleer of dat de computer heeft uitgaande toegang tot de *.azure automation.net op poort 443. 

#### <a name="cause-2-computer-has-less-than-minimum-hardware-requirements"></a>2 oorzaak: De Computer is minder dan de minimale hardwarevereisten
Computers met de hybride Runbook Worker aan de minimale hardwarevereisten voldoen voordat u deze als host voor deze functie. Gebruik anders, afhankelijk van het Resourcegebruik van andere achtergrondprocessen en conflicten veroorzaakt door runbooks tijdens het uitvoeren van de computer wordt er meer dan worden gebruikt en ervoor zorgen dat de runbook-taak vertragingen of time-outs. 

#### <a name="solution"></a>Oplossing
Controleer eerst de computer die moeten worden uitgevoerd van de Hybrid Runbook Worker-functie voldoet aan de minimale hardwarevereisten.  Zo ja, monitor CPU en geheugen gebruik om te bepalen een correlatie tussen de prestaties van Hybrid Runbook Worker-processen en vensters.  Als er geheugen of CPU-belasting, dit kan duiden op hoeft bij te werken of toevoegen van extra processors of geheugen voor het adres van de resource-knelpunt en los de fout te verhogen. Selecteer een andere berekeningsresource die de minimum vereisten en schaal ondersteunen kan wanneer werkbelasting eisen duiden op dat een verhoging is nodig.         

#### <a name="cause-3-runbooks-cannot-authenticate-with-local-resources"></a>3 oorzaak: Runbooks niet verifiëren met lokale bronnen

#### <a name="solution"></a>Oplossing
Controleer de **Microsoft SMA** gebeurtenislogboek voor een overeenkomstige gebeurtenis met beschrijving *Win32 proces is afgesloten met code [4294967295]*.  De oorzaak van deze fout is u dat nog niet hebt geconfigureerd verificatie in uw runbooks of de Run As-referenties voor de Hybrid worker-groep is opgegeven.  Raadpleeg [Runbook-machtigingen](automation-hrw-run-runbooks.md#runbook-permissions) om te bevestigen dat u verificatie juist hebt geconfigureerd voor uw runbooks.  

## <a name="next-steps"></a>Volgende stappen

Zie voor informatie over het oplossen van andere problemen in Automation [het oplossen van veelvoorkomende problemen met Azure Automation](automation-troubleshooting-automation-errors.md) 