---
title: Monitor Azure DC/OS-cluster - operationeel beheer
description: Een Azure Container Service DC/OS-cluster met de Microsoft Operations Management Suite bewaken.
services: container-service
author: keikhara
manager: timlt
ms.service: container-service
ms.topic: article
ms.date: 11/17/2016
ms.author: keikhara
ms.custom: mvc
ms.openlocfilehash: a675f0b57ed9e5d515cfa79a3a841e0f133fff6f
ms.sourcegitcommit: 5d3e99478a5f26e92d1e7f3cec6b0ff5fbd7cedf
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/06/2017
---
# <a name="monitor-an-azure-container-service-dcos-cluster-with-operations-management-suite"></a>Monitor voor een Azure Container Service DC/OS-cluster met Operations Management Suite

Microsoft Operations Management Suite (OMS) is een cloudoplossing voor IT-beheer van Microsoft waarmee u uw on-premises en cloudinfrastructuur kunt beheren en beveiligen. Container oplossing is een oplossing in OMS Log Analytics, waardoor u de inventaris van de container, prestaties en logboeken weergeven op één locatie. U kunt controleren, containers oplossen door de logboeken in een centrale locatie bekijken en veel ruis veroorzaken verbruikt overtollige container op een host vinden.

![](media/container-service-monitoring-oms/image1.png)

Raadpleeg voor meer informatie over Container oplossing de [Container oplossing Log Analytics](../../log-analytics/log-analytics-containers.md).

## <a name="setting-up-oms-from-the-dcos-universe"></a>Instellen van OMS uit de DC/OS-universe


In dit artikel wordt ervan uitgegaan dat u een DC/OS hebt ingesteld en eenvoudige web containertoepassingen op het cluster hebt geïmplementeerd.

### <a name="pre-requisite"></a>Vereiste
- [Microsoft Azure-abonnement](https://azure.microsoft.com/free/) -u kunt dit gratis ophalen.  
- Setup van Microsoft OMS-werkruimte - Zie 'Stap 3' hieronder
- [DC/OS CLI](https://dcos.io/docs/1.8/usage/cli/install/) geïnstalleerd.

1. Klik in het dashboard DC/OS op Universe en zoek naar OMS zoals hieronder wordt weergegeven.

![](media/container-service-monitoring-oms/image2.png)

2. Klik op **Install**. U ziet een pop up met de OMS-versie-informatie en een **pakket installeren** of **installatie in de geavanceerde** knop. Wanneer u klikt op **installatie in de geavanceerde**, die leidt u naar de **OMS specifieke configuratie-eigenschappen** pagina.

![](media/container-service-monitoring-oms/image3.png)

![](media/container-service-monitoring-oms/image4.png)

3. Hier kunt u gevraagd om in te voeren de `wsid` (de OMS werkruimte-ID) en `wskey` (de OMS primaire sleutel voor de werkruimte-id). Ophalen van beide `wsid` en `wskey` moet u een OMS-account bij maken <https://mms.microsoft.com>. Volg de stappen om een account te maken. Wanneer u klaar bent voor het maken van het account moet u eerst uw `wsid` en `wskey` door te klikken op **instellingen**, klikt u vervolgens **verbonden bronnen**, en vervolgens **Linux-Servers**, zoals hieronder weergegeven.

 ![](media/container-service-monitoring-oms/image5.png)

4. Selecteer de nummer u OMS exemplaren dat u wilt en klik op de knop 'Bekijken en installeren'. Doorgaans wilt u het aantal exemplaren van OMS gelijk zijn aan het aantal van de virtuele machine hebt u in uw cluster agent hebben. OMS-Agent voor Linux is geïnstalleerd als afzonderlijke containers op elke virtuele machine die zij wil verzamelen informatie voor de controle en logboekregistratie.

## <a name="setting-up-a-simple-oms-dashboard"></a>Instellen van een eenvoudige OMS-dashboard

Zodra u de OMS-Agent voor Linux op de virtuele machines hebt geïnstalleerd, wordt de volgende stap is voor het instellen van het dashboard OMS. Er zijn twee manieren om dit te doen: OMS-Portal of Azure-Portal.

### <a name="oms-portal"></a>OMS-Portal 

Aanmelden bij de OMS-portal (<https://mms.microsoft.com>) en Ga naar de **oplossing galerie**.

![](media/container-service-monitoring-oms/image6.png)

Wanneer u naar de **oplossing galerie**, selecteer **Containers**.

![](media/container-service-monitoring-oms/image7.png)

Wanneer u de Container-oplossing hebt geselecteerd, ziet u de tegel op de dashboardpagina van OMS overzicht. Nadat de containergegevens opgenomen is geïndexeerd, ziet u de tegel ingevuld met informatie over de oplossing weergeven tegels.

![](media/container-service-monitoring-oms/image8.png)

### <a name="azure-portal"></a>Azure Portal 

Meld u aan bij de Azure-portal op <https://portal.microsoft.com/>. Ga naar **Marketplace**, selecteer **bewaking + management** en klik op **alle Zie**. Typ vervolgens `containers` in de zoekopdracht. U ziet 'containers' in de zoekresultaten. Selecteer **Containers** en klik op **maken**.

![](media/container-service-monitoring-oms/image9.png)

Nadat u op **maken**, wordt u gevraagt voor uw werkruimte. Selecteer uw werkruimte of als u geen abonnement hebt, maakt u een nieuwe werkruimte.

![](media/container-service-monitoring-oms/image10.PNG)

Nadat u uw werkruimte hebt geselecteerd, klikt u op **maken**.

![](media/container-service-monitoring-oms/image11.png)

Raadpleeg voor meer informatie over de oplossing OMS-Container, de [Container oplossing Log Analytics](../../log-analytics/log-analytics-containers.md).

### <a name="how-to-scale-oms-agent-with-acs-dcos"></a>OMS-Agent met ACS DC/OS schalen 

Als u wilt OMS-agent niet genoeg het aantal werkelijke knooppunt hebt geïnstalleerd of u VMSS zijn schaalt door meer VM toe te voegen, kunt u doen door te schalen de `msoms` service.

Ga naar de Marathon of op het tabblad DC/OS-gebruikersinterface Services en het aantal knooppunten opschalen.

![](media/container-service-monitoring-oms/image12.PNG)

Hiermee implementeert u naar andere knooppunten die nog niet zijn geïmplementeerd op de OMS-agent.

## <a name="uninstall-ms-oms"></a>MS OMS verwijderen

MS OMS invoeren voor het verwijderen van de volgende opdracht:

```bash
$ dcos package uninstall msoms
```

## <a name="let-us-know"></a>Laat ons weten.
Wat werkt? Wat ontbreekt? Wat kan er anders moet u voor deze nuttig zijn voor u? Laat ons weten op <a href="mailto:OMSContainers@microsoft.com">OMSContainers</a>.

## <a name="next-steps"></a>Volgende stappen

 Nu dat u een OMS hebt ingesteld voor het bewaken van uw containers[uw dashboard container zien](../../log-analytics/log-analytics-containers.md).
