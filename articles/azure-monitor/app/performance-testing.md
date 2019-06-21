---
title: Prestaties en belasting testen met Azure Application Insights | Microsoft Docs
description: Instellen van prestaties en load tests met Azure Application Insights
services: application-insights
author: mrbullwinkle
manager: carmonm
ms.assetid: 46dc13b4-eb2e-4142-a21c-94a156f760ee
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.topic: conceptual
ms.date: 06/19/2019
ms.reviewer: sdash
ms.author: mbullwin
ms.openlocfilehash: 55d743e32f6db0828317d3764a97bcb35b104dad
ms.sourcegitcommit: 82efacfaffbb051ab6dc73d9fe78c74f96f549c2
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/20/2019
ms.locfileid: "67305187"
---
# <a name="performance-testing"></a>Prestaties testen

> [!NOTE]
> De service voor belastingtests cloud-gebaseerde is afgeschaft. Meer informatie over de afschaffing, de beschikbaarheid van services en andere services kan worden gevonden [hier](https://docs.microsoft.com/azure/devops/test/load-test/overview?view=azure-devops).

Application Insights kunt u voor het genereren van belastingstests voor uw websites. Zoals [beschikbaarheidstests](monitor-web-app-availability.md), kunt u een eenvoudige aanvragen verzenden of [meervoudige aanvragen](availability-multistep.md) van Azure WebTest-agents over de hele wereld. Prestatietests kunnen u maximaal 20.000 gelijktijdige gebruikers te simuleren voor tot 60 minuten.

## <a name="create-an-application-insights-resource"></a>Een Application Insights-resource maken

Als u wilt maken van een prestatietest, moet u eerst een Application Insights-resource maken. Een resource verder met de volgende sectie als u al hebt gemaakt.

Selecteer in de Azure-portal **een resource maken** > **hulpprogramma's voor ontwikkelaars** > **Application Insights** en maak een Application Insights de bron.

## <a name="configure-performance-testing"></a>Prestatietesten configureren

Als dit is de eerste keer is het maken van prestaties testen Selecteer **organisatie ingesteld** en kiest u een Azure DevOps-organisatie moet de bron voor uw prestaties te testen.

Onder **configureren**, gaat u naar **prestatietests** en klikt u op **nieuw** te maken van een test.

![Vul in elk geval de URL van uw website in](./media/performance-testing/new-performance-test.png)

Voor het maken van een eenvoudige prestatietest selecteert u het testtype **handmatig testen** en vul de gewenste instellingen voor de test.

|Instelling| Maximumwaarde
|----------|------------|
| Gebruikersbelasting | 20,000 |
| Duur (minuten)  | 60 |  

Nadat de test is gemaakt, klikt u op **test uitvoeren**.

Zodra de test voltooid is, ziet u de resultaten die op de onderstaande resultaten lijken:

![Testresultaten](./media/performance-testing/test-results.png)

## <a name="configure-visual-studio-web-test"></a>Visual Studio-WebTest configureren

Application Insights testfunctionaliteit geavanceerde prestaties zijn gebaseerd op Visual Studio-prestaties en belasting testen projecten.

![Visual Studio ](./media/performance-testing/visual-studio-test.png)

## <a name="next-steps"></a>Volgende stappen

* [Webtests met meerdere stappen](availability-multistep.md)
* [URL-ping-tests](monitor-web-app-availability.md)