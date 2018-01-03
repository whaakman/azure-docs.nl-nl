---
title: Maak een aangepaste test - Azure Application Gateway - Azure-Portal | Microsoft Docs
description: Informatie over het maken van een aangepaste test voor Application Gateway met behulp van de portal
services: application-gateway
documentationcenter: na
author: davidmu1
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 33fd5564-43a7-4c54-a9ec-b1235f661f97
ms.service: application-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 04/26/2017
ms.author: davidmu
ms.openlocfilehash: bb77c9b39e1aa89f6411de8ec3b1fca41e954bf2
ms.sourcegitcommit: b5c6197f997aa6858f420302d375896360dd7ceb
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/21/2017
---
# <a name="create-a-custom-probe-for-application-gateway-by-using-the-portal"></a>Een aangepaste test voor de toepassingsgateway maken met behulp van de portal

> [!div class="op_single_selector"]
> * [Azure Portal](application-gateway-create-probe-portal.md)
> * [Azure Resource Manager PowerShell](application-gateway-create-probe-ps.md)
> * [Azure Classic PowerShell](application-gateway-create-probe-classic-ps.md)

In dit artikel kunt u een aangepaste test toevoegen aan een bestaande toepassingsgateway via de Azure portal. Aangepaste tests zijn handig voor toepassingen die de pagina controle van een specifieke status hebben, of voor toepassingen die een geslaagde reactie op de standaardwebtoepassing geen bieden.

## <a name="before-you-begin"></a>Voordat u begint

Als u nog geen een application gateway, gaat u naar [een toepassingsgateway maken](application-gateway-create-gateway-portal.md) voor het maken van een toepassingsgateway met werkt.

## <a name="createprobe"></a>Maken van de test

Tests worden geconfigureerd in een proces via de portal. De eerste stap is het maken van de test. In de tweede stap voegt u de test op de back-end-http-instellingen van de toepassingsgateway.

1. Meld u aan bij [Azure Portal](https://portal.azure.com). Als u nog een account hebt, kunt u zich aanmelden voor een [gratis proefversie van één maand](https://azure.microsoft.com/free)

1. Klik op alle resources in het venster Azure Favorieten voor portal. Klik op de toepassingsgateway in de blade met alle bronnen. Als het abonnement dat u hebt geselecteerd al verschillende bronnen heeft, kunt u partners.contoso.net invoeren in het Filter met de naam... voor eenvoudige toegang tot de toepassingsgateway.

1. Klik op **Probes** en klik op de **toevoegen** om toe te voegen een test.

  ![Test-blade met informatie is ingevuld toevoegen][1]

1. Op de **toevoegen health test** blade Vul de vereiste informatie voor de test en klikt u op wanneer u klaar **OK**.

  |**Instelling** | **Waarde** | **Details**|
  |---|---|---|
  |**Naam**|customProbe|Deze waarde is een beschrijvende naam voor de test die toegankelijk is in de portal.|
  |**Protocol**|HTTP of HTTPS | Het protocol dat gebruikmaakt van de health-test.|
  |**Host**|Internet Explorer contoso.com|Deze waarde is de hostnaam die wordt gebruikt voor de test. Van toepassing alleen als er meerdere locaties is geconfigureerd op Application Gateway, of gebruik anders '127.0.0.1'. Deze waarde verschilt van de naam van de VM-host.|
  |**Pad**|/ of een ander pad|De rest van de volledige url voor de aangepaste test. Een geldig pad begint met '/'. Gebruik voor het standaardpad van http://contoso.com alleen '/' |
  |**Interval (sec)**|30|Hoe vaak de test wordt uitgevoerd om te controleren of health. Het wordt niet aangeraden om in te stellen hoe lager dan 30 seconden.|
  |**Time-out (sec)**|30|De hoeveelheid tijd die de test wordt gewacht voordat een time-out optreedt. De time-outinterval moet hoog genoeg dat een http-aanroep kan worden gemaakt om ervoor te zorgen dat de health-pagina back-end is beschikbaar.|
  |**Drempelwaarde voor onjuiste status**|3|Het aantal mislukte pogingen om te worden niet in orde beschouwd. Een drempelwaarde 0 betekent dat als een controle de back-end mislukt bepaald beschadigd onmiddellijk.|

  > [!IMPORTANT]
  > De hostnaam is niet hetzelfde zijn als servernaam. Deze waarde is de naam van de virtuele host op de toepassingsserver wordt uitgevoerd. De test wordt verzonden naar http://(host name):(port from httpsetting)/urlPath

## <a name="add-probe-to-the-gateway"></a>Test toevoegen aan de gateway

Nu dat de test is gemaakt, is het tijd om toe te voegen aan de gateway. Test-instellingen zijn ingesteld op de back-end-HTTP-instellingen van de toepassingsgateway.

1. Klik op **HTTP-instellingen** in de toepassingsgateway online zetten van het configuratie-blade klikt u op de huidige back-end HTTP-instellingen die worden vermeld in het venster.

  ![venster van de instellingen voor HTTPS][2]

1. Op de **appGatewayBackEndHttpSettings** instellingenblade, Controleer de **gebruik aangepaste test** selectievakje in en kies de test gemaakt in de [maken van de test](#createprobe) sectie over de  **Aangepaste test** vervolgkeuzelijst...
Wanneer voltooid, klikt u op **opslaan** en de instellingen worden toegepast.

De standaard-test controleert de toegang tot de webtoepassing. Nu dat een aangepaste test is gemaakt, gebruikt de toepassingsgateway de aangepaste pad dat is gedefinieerd voor het controleren van de status voor de back-endservers. De toepassingsgateway op basis van de criteria die is gedefinieerd, wordt het pad dat is opgegeven in de test gecontroleerd. Als de aanroep naar de hostnaam: poort / pad resulteert niet in een HTTP 200-399 statusantwoord, de server wordt uitgevoerd buiten de draaihoek nadat de drempelwaarde voor onjuiste status is bereikt. Probing blijft op het exemplaar niet in orde om te bepalen wanneer deze opnieuw in orde is. Wanneer het exemplaar weer aan de orde servergroep is toegevoegd, verkeer begint stromende opnieuw aan en zoeken naar het exemplaar wordt voortgezet met de opgegeven interval van de gebruiker normaal.

## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie over het configureren van SSL-Offloading met Azure Application Gateway, [SSL-Offload configureren](application-gateway-ssl-portal.md)

[1]: ./media/application-gateway-create-probe-portal/figure1.png
[2]: ./media/application-gateway-create-probe-portal/figure2.png

