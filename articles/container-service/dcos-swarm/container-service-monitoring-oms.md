---
title: Monitor Azure DC/OS-cluster - operationeel Management
description: Een Azure Container Service DC/OS-cluster bewaken met Log Analytics.
services: container-service
author: keikhara
manager: jeconnoc
ms.service: container-service
ms.topic: article
ms.date: 11/17/2016
ms.author: keikhara
ms.custom: mvc
ms.openlocfilehash: 4576d9decc6ba1e01ef39abdb8a3ef89461196e8
ms.sourcegitcommit: 07a09da0a6cda6bec823259561c601335041e2b9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/18/2018
ms.locfileid: "49407789"
---
# <a name="monitor-an-azure-container-service-dcos-cluster-with-log-analytics"></a>Een Azure Container Service DC/OS-cluster bewaken met Log Analytics

Log Analytics is van Microsoft cloud-gebaseerde IT-beheeroplossing waarmee u beheren kunt en beveiligen van uw on-premises en cloudinfrastructuur. Container-oplossing is een oplossing in Log Analytics, dat helpt u bij het weergeven van de container-inventaris, prestaties en logboeken op één locatie. U kunt controleren, containers oplossen door de logboeken in de centrale locatie bekijken en zoeken ruis verbruikt overtollige container op een host.

![](media/container-service-monitoring-oms/image1.png)

Zie voor meer informatie over Container-oplossing, de [Container-oplossing Log Analytics](../../log-analytics/log-analytics-containers.md).

## <a name="setting-up-log-analytics-from-the-dcos-universe"></a>Log Analytics instellen vanuit de DC/OS-universum


In dit artikel wordt ervan uitgegaan dat u een DC/OS hebt geconfigureerd en eenvoudige web-containertoepassingen op het cluster hebt geïmplementeerd.

### <a name="pre-requisite"></a>Vereiste
- [Microsoft Azure-abonnement](https://azure.microsoft.com/free/) -krijgt u een abonnement gratis.  
- Meld u instellingen voor Analytics-werkruimte: Zie 'stap 3"hieronder
- [DC/OS CLI](https://dcos.io/docs/1.8/usage/cli/install/) geïnstalleerd.

1. Klik in het DC/OS-dashboard op universum en zoek naar OMS, zoals hieronder weergegeven.

   >[!NOTE]
   >OMS wordt nu aangeduid als Log Analytics.

 ![](media/container-service-monitoring-oms/image2.png)

2. Klik op **Install**. U ziet een pop-upvenster met de versie-informatie en een **pakket installeren** of **geavanceerde installatie** knop. Wanneer u klikt op **geavanceerde installatie**, die leidt u naar de **OMS specifieke configuratie-eigenschappen** pagina.

 ![](media/container-service-monitoring-oms/image3.png)

 ![](media/container-service-monitoring-oms/image4.png)

3. Hier kunt u wordt gevraagd om in te voeren de `wsid` (de Log Analytics-werkruimte-ID) en `wskey` (de primaire sleutel voor de werkruimte-ID). Om op te halen beide `wsid` en `wskey` u wilt maken van een account bij <https://mms.microsoft.com>.
Volg de stappen om een account te maken. Wanneer u klaar bent het account te maken, moet u eerst uw `wsid` en `wskey` door te klikken op **instellingen**, klikt u vervolgens **verbonden bronnen**, en vervolgens **Linux-Servers**, zoals hieronder weergegeven.

 ![](media/container-service-monitoring-oms/image5.png)

4. Selecteer het aantal exemplaren dat u wilt en klik op de knop 'Bekijken en installeren'. Meestal zult u het aantal exemplaren gelijk zijn aan het aantal VM's in uw cluster agent hebt. Log Analytics-agent voor Linux installeert als afzonderlijke containers op elke virtuele machine die het bedrijf wil verzamelen informatie voor bewaking en logboekregistratie.

   [!INCLUDE [log-analytics-agent-note](../../../includes/log-analytics-agent-note.md)] 

## <a name="setting-up-a-simple-log-analytics-dashboard"></a>Instellen van een eenvoudige Log Analytics-dashboard

Nadat u de Log Analytics-agent voor Linux hebt geïnstalleerd op de virtuele machines, wordt de volgende stap is het instellen van de Log Analytics-dashboard. U kunt het dashboard via Azure-portal instellen.

### <a name="azure-portal"></a>Azure Portal 

Meld u aan bij Azure portal op <https://portal.microsoft.com/>. Ga naar **Marketplace**, selecteer **bewaking en beheer** en klikt u op **Zie alle**. Typ vervolgens `containers` in het zoekvak. Hier ziet u 'containers' in de lijst met zoekresultaten. Selecteer **Containers** en klikt u op **maken**.

![](media/container-service-monitoring-oms/image9.png)

Nadat u op **maken**, wordt u gevraagd u uw werkruimte. Selecteer uw werkruimte of als u een hebt, maakt u een nieuwe werkruimte.

![](media/container-service-monitoring-oms/image10.PNG)

Nadat u uw werkruimte hebt geselecteerd, klikt u op **maken**.

![](media/container-service-monitoring-oms/image11.png)

Raadpleeg voor meer informatie over de oplossing Log Analytics-Container, de [Container-oplossing Log Analytics](../../log-analytics/log-analytics-containers.md).

### <a name="how-to-scale-log-analytics-agent-with-acs-dcos"></a>Het schalen van Log Analytics-agent met ACS DC/OS 

In het geval moet u Log Analytics-agent aan het aantal daadwerkelijke knooppunten hebt geïnstalleerd of u omhoog van virtuele-machineschaalset schalen bent door meer VM toe te voegen, kunt u doen door te schalen de `msoms` service.

U kunt gaat u naar de Marathon of op het tabblad Services van DC/OS-gebruikersinterface en het aantal knooppunten omhoog.

![](media/container-service-monitoring-oms/image12.PNG)

Hierdoor wordt geïmplementeerd naar andere knooppunten die de Log Analytics-agent nog niet hebt geïmplementeerd.

## <a name="uninstall-ms-oms"></a>MS OMS verwijderen

MS OMS invoeren voor het verwijderen van de volgende opdracht uit:

```bash
$ dcos package uninstall msoms
```

## <a name="let-us-know"></a>Laat het ons weten.
Wat werkt? Wat ontbreekt? Wat moet u dit is nuttig voor u? Laat het ons weten op <a href="mailto:OMSContainers@microsoft.com">OMSContainers</a>.

## <a name="next-steps"></a>Volgende stappen

 Nu dat u Log Analytics hebt ingesteld voor het bewaken van uw containers[uw container-dashboard zien](../../log-analytics/log-analytics-containers.md).
