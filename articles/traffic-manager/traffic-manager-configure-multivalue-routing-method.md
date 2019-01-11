---
title: Routeringsmethode voor verkeer met meerdere waarden configureren in Azure Traffic Manager
description: In dit artikel wordt uitgelegd hoe u Traffic Manager configureren om verkeer te routeren naar A/AAAA-eindpunten.
services: traffic-manager
documentationcenter: ''
author: KumudD
ms.service: traffic-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 09/10/2018
ms.author: kumud
ms.openlocfilehash: 40b266c92a86006746ab5341ac5fa1d785ee6032
ms.sourcegitcommit: d4f728095cf52b109b3117be9059809c12b69e32
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/10/2019
ms.locfileid: "54197165"
---
# <a name="configure-multivalue-routing-method-in-traffic-manager"></a>Routeringsmethode voor met meerdere waarden configureren in Traffic Manager

In dit artikel wordt beschreven hoe u met meerdere waarden verkeersrouteringsmethode configureren. De **met meerdere waarden** verkeer routeringsmethode kunt u meerdere eindpunten in orde terugkeren en verhoogt de betrouwbaarheid van uw toepassing omdat clients meer opties zonder te hoeven hebben te doen van een andere DNS-zoekactie opnieuw uit te voeren. Alleen voor de profielen waarvoor hun eindpunten die zijn opgegeven met behulp van IPv4 of IPv6-adressen is met meerdere waarden routering ingeschakeld. Wanneer een query wordt ontvangen voor dit profiel, worden alle eindpunten in orde geretourneerd op basis van de configureerbare maximale geretourneerde aantal opgegeven. 

>[!NOTE]
> Op dit moment eindpunten toevoegen met behulp van IPv4 of IPv6-adressen wordt alleen ondersteund voor eindpunten van het type **externe** en kan daarom met meerdere waarden routering wordt ook alleen ondersteund voor deze eindpunten.

## <a name="sign-in-to-azure"></a>Aanmelden bij Azure 

Meld u aan bij Azure Portal op https://portal.azure.com.
## <a name="create-a-resource-group"></a>Een resourcegroep maken
Maak een resourcegroep voor het Traffic Manager-profiel.
1. Selecteer in het linkerdeelvenster van de Azure-portal, **resourcegroepen**.
2. In **resourcegroepen**, boven aan de pagina, selecteer **toevoegen**.
3. In **groepsnaam voor Accountresources**, typ een naam *myResourceGroupTM1*. Voor **resourcegroeplocatie**, selecteer **VS-Oost**, en selecteer vervolgens **OK**.

## <a name="create-a-traffic-manager-profile"></a>Een Traffic Manager-profiel maken
Maak een Traffic Manager-profiel met gebruikersverkeer verwezen door deze te verzenden naar het eindpunt met de laagste latentie.

1. Selecteer linksboven in het scherm de optie **Een resource maken** > **Netwerken** > **Traffic Manager-profiel** > **Maken**.
2. In **Traffic Manager-profiel**, invoeren of selecteren, de volgende informatie, accepteer de standaardwaarden voor de overige instellingen en selecteer vervolgens **maken**:
    
    | Instelling                 | Waarde                                              |
    | ---                     | ---                                                |
    | Name                   | Deze naam moet uniek zijn binnen de zone trafficmanager.net en resulteert in de DNS-naam, trafficmanager.net, die wordt gebruikt voor het openen van uw Traffic Manager-profiel.                                   |
    | Routeringsmethode          | Selecteer de **met meerdere waarden** routeringsmethode.                                       |
    | Abonnement            | Selecteer uw abonnement.                          |
    | Resourcegroep          | Selecteer *myResourceGroupTM1*. |
    | Locatie                | Deze instelling verwijst naar de locatie van de resourcegroep en heeft geen invloed op het Traffic Manager-profiel dat wereldwijd wordt geïmplementeerd.                              |
   |        |           | 
  
  ![Een Traffic Manager-profiel maken](./media/traffic-manager-multivalue-routing-method/create-traffic-manager-profile.png)

## <a name="add-traffic-manager-endpoints"></a>Traffic Manager-eindpunten toevoegen

Twee IP-adressen als externe eindpunten toevoegen aan meerdere waarden Traffic Manager-profiel dat u in de vorige stap hebt gemaakt.

1. Zoek in de zoekbalk van de portal de naam van het Traffic Manager-profiel dat u in de vorige sectie hebt gemaakt en selecteer het profiel in de weergegeven resultaten.
2. Klik in **Traffic Manager-profiel**, in de sectie **Instellingen**, op **Eindpunten** en vervolgens op **Toevoegen**.
3. Voer de volgende informatie in of selecteer deze, accepteer de standaardwaarden voor de overige instellingen en selecteer **OK**:

    | Instelling                 | Waarde                                              |
    | ---                     | ---                                                |
    | Type                    | Extern eindpunt                                   |
    | Name           | myEndpoint1                                        |
    | Volledig gekwalificeerde domeinnaam (FQDN) of IP           | Typ het openbare IP-adres van het eindpunt dat u wilt toevoegen aan dit Traffic Manager-profiel                         |
    |        |           |

4. Herhaal stappen 2 en 3 om toe te voegen een ander eindpunt met de naam *myEndpoint2*, voor **volledig gekwalificeerde domeinnaam (FQDN) of IP-**, voer het openbare IP-adres van het tweede eindpunt.
5.  Als beide eindpunten zijn toegevoegd, worden ze weergegeven in **Traffic Manager-profiel**, samen met de controlestatus **Online**.

   ![Traffic Manager-eindpunt toevoegen](./media/traffic-manager-multivalue-routing-method/add-endpoint.png)
 
## <a name="next-steps"></a>Volgende stappen

- Meer informatie over [gewogen verkeersrouteringsmethode](traffic-manager-configure-weighted-routing-method.md).
- Meer informatie over [de routeringsmethode met prioriteit](traffic-manager-configure-priority-routing-method.md).
- Meer informatie over [routeringsmethode voor prestaties](traffic-manager-configure-performance-routing-method.md)
- Meer informatie over [geografische verkeersrouteringsmethode](traffic-manager-configure-geographic-routing-method.md).


