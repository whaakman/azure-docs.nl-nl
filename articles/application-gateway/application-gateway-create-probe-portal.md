---
title: Een aangepaste test - Azure Application Gateway - Azure Portal maken | Microsoft Docs
description: Informatie over het maken van een aangepaste test voor Application Gateway met behulp van de portal
services: application-gateway
documentationcenter: na
author: vhorne
manager: jpconnock
editor: ''
tags: azure-resource-manager
ms.assetid: 33fd5564-43a7-4c54-a9ec-b1235f661f97
ms.service: application-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 04/26/2017
ms.author: victorh
ms.openlocfilehash: 90d576fd00a39f7e871cbe0922ce131dfbe38ff0
ms.sourcegitcommit: d83fa82d6fec451c0cb957a76cfba8d072b72f4f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/02/2019
ms.locfileid: "58862162"
---
# <a name="create-a-custom-probe-for-application-gateway-by-using-the-portal"></a>Een aangepaste test maken voor Application Gateway met behulp van de portal

> [!div class="op_single_selector"]
> * [Azure Portal](application-gateway-create-probe-portal.md)
> * [Azure Resource Manager PowerShell](application-gateway-create-probe-ps.md)
> * [Azure Classic PowerShell](application-gateway-create-probe-classic-ps.md)

In dit artikel, kunt u een aangepaste test toevoegen aan een bestaande toepassingsgateway via Azure portal. Aangepaste tests zijn handig voor toepassingen waarvoor een specifieke statuscontrolepagina of voor toepassingen die een geslaagd antwoord op de standaard web-App bieden.

## <a name="before-you-begin"></a>Voordat u begint

Als u een application gateway niet al hebt, gaat u naar [een toepassingsgateway maken](application-gateway-create-gateway-portal.md) om een toepassingsgateway om te werken met te maken.

## <a name="createprobe"></a>De test maken

Tests worden geconfigureerd in een proces in twee stappen via de portal. De eerste stap is het maken van de test. In de tweede stap, kunt u de test toevoegen aan de back-end-http-instellingen van de toepassingsgateway.

1. Meld u aan bij [Azure Portal](https://portal.azure.com). Als u nog een account hebt, kunt u zich aanmelden voor een [gratis proefversie van één maand](https://azure.microsoft.com/free)

1. Klik op alle resources in het Azure portal Favorieten. Klik op de application gateway in de blade alle resources. Als het abonnement dat u geselecteerd, al verschillende resources heeft, kunt u partners.contoso.net invoeren in het Filter op naam... voor eenvoudige toegang tot de toepassingsgateway.

1. Klik op **tests** en klikt u op de **toevoegen** om toe te voegen een test.

   ![Test-blade met informatie invullen toevoegen][1]

1. Op de **test toevoegen** blade, vul de vereiste gegevens voor de test en klikt u op wanneer u klaar bent **OK**.

   |**Instelling** | **Value** | **Details**|
   |---|---|---|
   |**Name**|customProbe|Deze waarde is een beschrijvende naam in de test die toegankelijk is in de portal.|
   |**Protocol**|HTTP of HTTPS | Het protocol dat gebruikmaakt van de statustest.|
   |**Host**|i.e contoso.com|Deze waarde is de naam van de host die wordt gebruikt voor de test. Van toepassing alleen als er meerdere sites is geconfigureerd in Application Gateway, anders gebruiken '127.0.0.1'. Deze waarde verschilt van de naam van de VM-host.|
   |**Pad**|/ of een ander pad|De rest van de volledige url voor de aangepaste test. Een geldig pad begint met '/'. Voor het standaardpad van http:\//contoso.com gewoon gebruiken '/' |
   |**Interval (seconden)**|30|Hoe vaak de test wordt uitgevoerd om te controleren op status. Het wordt niet aanbevolen om in te stellen, des te lager dan 30 seconden.|
   |**Time-out (seconden)**|30|De hoeveelheid tijd die de test moet voordat een time-out optreedt wachten. De time-outinterval moet hoog genoeg waarop een http-aanroep om ervoor te zorgen dat de health-pagina back-end beschikbaar kan worden gesteld.|
   |**Drempelwaarde voor onjuiste status**|3|Het aantal mislukte pogingen om te worden als slecht beschouwd. Een drempelwaarde 0 betekent dat als een controle van gatewayservicestatus is mislukt de back-end wordt onmiddellijk niet in orde bepaald.|

   > [!IMPORTANT]
   > De hostnaam is niet hetzelfde als de servernaam van de. Deze waarde is de naam van de virtuele host die wordt uitgevoerd op de toepassingsserver. De test wordt verzonden naar http://(host name):(port from httpsetting)/urlPath

## <a name="add-probe-to-the-gateway"></a>Test toevoegen aan de gateway

Nu dat de test is gemaakt, is het tijd toe te voegen aan de gateway. Test-instellingen worden ingesteld op de back-end-http-instellingen van de toepassingsgateway.

1. Klik op **HTTP-instellingen** in de toepassingsgateway om de blade configuratie op de huidige back-end http-instellingen die worden vermeld in het venster.

   ![venster voor HTTPS-instellingen][2]

1. Op de **appGatewayBackEndHttpSettings** instellingenblade, Controleer de **gebruik aangepaste test** selectievakje in en kies de test die u de [maken van de test](#createprobe) sectie over de  **Aangepaste test** vervolgkeuzelijst...
   Als u klaar bent, klikt u op **opslaan** en de instellingen worden toegepast.

De standaard-test controleert de toegang tot de web-App. Nu er een aangepaste test is gemaakt, wordt de toepassingsgateway het aangepaste pad gedefinieerd voor het controleren van de status voor de back-endservers. De toepassingsgateway op basis van de criteria die is gedefinieerd, wordt het pad dat is opgegeven in de test gecontroleerd. Als de aanroep naar de host: Port / pad resulteert niet in een HTTP 200-399 statusantwoord, de server is uit rotatie gehaald nadat de drempelwaarde voor onjuiste status is bereikt. Scannen blijft op het exemplaar niet in orde om te bepalen wanneer deze weer in orde. Wanneer het exemplaar terug naar de server is in orde groep is toegevoegd, verkeer ingaande naar deze opnieuw en scannen met het exemplaar wordt voortgezet met opgegeven interval als normale gebruiker.

## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie over het configureren van SSL-Offloading met Azure Application Gateway, [SSL-Offload configureren](application-gateway-ssl-portal.md)

[1]: ./media/application-gateway-create-probe-portal/figure1.png
[2]: ./media/application-gateway-create-probe-portal/figure2.png

