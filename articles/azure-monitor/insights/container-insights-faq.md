---
title: Azure Monitor voor containers Veelgestelde vragen | Microsoft Docs
description: Azure Monitor voor containers is een oplossing die de status van uw AKS-clusters en exemplaren van de Container in Azure controleert. In dit artikel vindt u antwoorden op veelgestelde vragen.
services: azure-monitor
author: mgoedtel
manager: carmonm
editor: tysonn
ms.service: azure-monitor
ms.topic: article
ms.workload: infrastructure-services
ms.date: 04/17/2019
ms.author: magoedte
ms.openlocfilehash: 70712b68cc26a461f702850dbf6064b65b4ff130
ms.sourcegitcommit: bf509e05e4b1dc5553b4483dfcc2221055fa80f2
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/22/2019
ms.locfileid: "59999719"
---
# <a name="azure-monitor-for-containers-frequently-asked-questions"></a>Azure Monitor voor containers Veelgestelde vragen

Dit Microsoft-FAQ is een lijst met veelgestelde vragen over Azure Monitor voor containers. Als u aanvullende vragen over de oplossing hebt, gaat u naar de [discussieforum](https://feedback.azure.com/forums/34192--general-feedback) en plaats uw vraag. Wanneer u een vraag is vaak wordt gevraagd, toevoegen we deze aan dit artikel zodat snel en eenvoudig kunnen worden gevonden.

## <a name="why-dont-i-see-data-in-my-log-analytics-workspace"></a>Waarom zie ik gegevens niet in mijn werkruimte van Log Analytics?

Als u niet om te zien van alle gegevens in de Log Analytics-werkruimte aan een bepaalde tijd dagelijks, mogelijk u bereikt de standaardlimiet van 500 MB of de dagelijkse limiet voor het controleren van de hoeveelheid gegevens voor het verzamelen van dagelijks opgegeven. Als de limiet voor de dag wordt voldaan, wordt het verzamelen van gegevens stopt en alleen op de volgende dag hervat. Als u wilt het gegevensgebruik van uw bekijken en bijwerken naar een andere prijscategorie op basis van uw Verwachte gebruikspatronen, Zie [gegevensgebruik en-kosten](../platform/manage-cost-storage.md). 

## <a name="what-are-the-container-states-specified-in-the-containerinventory-table"></a>Wat zijn de Staten container is opgegeven in de tabel ContainerInventory?

De tabel ContainerInventory bevat informatie over containers gestopt en wordt uitgevoerd. De tabel wordt gevuld met een werkstroom in de agent die de docker voor alle containers (wordt uitgevoerd en is gestopt) query's en verzendt die gegevens van de Log Analytics-werkruimte.
 
## <a name="how-do-i-resolve-missing-subscription-registration-error"></a>Hoe los ik **ontbrekende abonnementsregistratie** fout?

Als u de foutmelding **ontbrekende abonnementen registreren voor Microsoft.OperationsManagement**, u het kunt oplossen door de resourceprovider registreren **Microsoft.OperationsManagement** in de het abonnement waarin de werkruimte is gedefinieerd. De documentatie voor hoe u dit doet, kan worden gevonden [hier](../../azure-resource-manager/resource-manager-register-provider-errors.md).

## <a name="is-there-support-for-rbac-enabled-aks-clusters"></a>Is er ondersteuning voor RBAC AKS clusters ingeschakeld?

De Container Monitoring-oplossing biedt geen ondersteuning voor RBAC, maar dit wordt ondersteund met Azure Monitor for Containers. De pagina met details van de oplossing kan niet de juiste informatie weergegeven op de blades die gegevens voor deze clusters weergeven.

## <a name="how-do-i-enable-log-collection-for-containers-in-the-kube-system-namespace-through-helm"></a>Hoe schakel ik logboekverzameling voor containers in de naamruimte kube-systeem via Helm?

De logboekverzameling van containers in de naamruimte kube-systeem is standaard uitgeschakeld. Logboekverzameling kan worden ingeschakeld door het instellen van een omgevingsvariabele op de omsagent. Zie voor meer informatie de [Azure Monitor voor containers](https://github.com/helm/charts/tree/master/incubator/azuremonitor-containers) GitHub-pagina. 

## <a name="how-do-i-update-the-omsagent-to-the-latest-released-version"></a>Hoe kan ik de omsagent bijwerken naar de meest recente releaseversie?

Als u wilt weten hoe u de agent bij te werken, Zie [agentbeheer](container-insights-manage-agent.md).

## <a name="how-do-i-enable-multi-line-logging"></a>Hoe kan ik meerdere regels logboekregistratie inschakelen?

Op dit moment meerdere regels logboekregistratie biedt geen ondersteuning voor Azure Monitor voor containers, maar er zijn oplossingen beschikbaar. U kunt alle services om te schrijven in JSON-indeling configureren en Docker/Moby wordt te schrijven als een enkele regel.

U kunt bijvoorbeeld uw logboek verpakken als een JSON-object, zoals wordt weergegeven in het voorbeeld hieronder voor een voorbeeld van een node.js-toepassing:

```
console.log(json.stringify({ 
      "Hello": "This example has multiple lines:",
      "Docker/Moby": "will not break this into multiple lines",
      "and you will receive":"all of them in log analytics",
      "as one": "log entry"
      }));
```

Deze gegevens eruit als in het volgende voorbeeld in Azure Monitor voor Logboeken wanneer u een query uitvoeren voor:

```
LogEntry : ({“Hello": "This example has multiple lines:","Docker/Moby": "will not break this into multiple lines", "and you will receive":"all of them in log analytics", "as one": "log entry"}

```

Voor een gedetailleerde Kijk op het probleem, raadpleegt u de volgende [link naar github](https://github.com/moby/moby/issues/22920).

## <a name="how-do-i-resolve-azure-ad-errors-when-i-enable-live-logs"></a>Hoe kan ik Azure AD-fouten oplossen wanneer ik live Logboeken inschakelen? 

U ziet mogelijk de volgende fout: **Het antwoord op de url die is opgegeven in de aanvraag komt niet overeen met de antwoord-URL's geconfigureerd voor de toepassing: ' < toepassings-ID\>'**. De oplossing op te lossen kunt u vinden in het artikel [container logboeken realtime met Azure Monitor voor containers weergeven](container-insights-live-logs.md#configure-aks-with-azure-active-directory). 

## <a name="why-cant-i-upgrade-cluster-after-onboarding"></a>Waarom niet kan ik een upgrade uitvoeren cluster na de onboarding?

Als nadat u Azure Monitor voor containers voor een AKS-cluster hebt ingeschakeld, u de Log Analytics-werkruimte verwijdert de gegevens is verzonden met het cluster, bij het upgraden van het cluster zal mislukken. Om dit te omzeilen, moet u uitschakelen, controle en vervolgens weer inschakelen die verwijst naar een andere geldige werkruimte in uw abonnement. Wanneer u de clusterupgrade opnieuw uitvoert probeert, moet worden verwerkt en voltooid.  

## <a name="next-steps"></a>Volgende stappen

Om te beginnen met controleren van uw AKS-cluster, Bekijk [hoe zorgen voor onboarding Azure controleren voor containers](container-insights-onboard.md) om te begrijpen van de vereisten en de beschikbare methoden bewaking wilt inschakelen. 