---
title: Een interne load balancer maken in Azure Portal | Microsoft Docs
description: Meer informatie over hoe u met Azure Portal een interne load balancer maakt in Resource Manager
services: load-balancer
documentationcenter: na
author: KumudD
manager: timlt
editor: 
tags: azure-service-management
ms.assetid: 1ac14fb9-8d14-4892-bfe6-8bc74c48ae2c
ms.service: load-balancer
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 09/25/2017
ms.author: kumud
ms.openlocfilehash: 3be595b03f667cf9700d2f17eb2080aa74f41dd9
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="create-an-internal-load-balancer-in-the-azure-portal"></a>Een interne load balancer maken in Azure Portal

> [!div class="op_single_selector"]
> * [Azure Portal](../load-balancer/load-balancer-get-started-ilb-arm-portal.md)
> * [PowerShell](../load-balancer/load-balancer-get-started-ilb-arm-ps.md)
> * [Azure CLI](../load-balancer/load-balancer-get-started-ilb-arm-cli.md)
> * [Sjabloon](../load-balancer/load-balancer-get-started-ilb-arm-template.md)


[!INCLUDE [load-balancer-basic-sku-include.md](../../includes/load-balancer-basic-sku-include.md)]

[!INCLUDE [load-balancer-get-started-ilb-intro-include.md](../../includes/load-balancer-get-started-ilb-intro-include.md)]

> [!NOTE]
> Azure heeft twee verschillende implementatiemodellen voor het maken van en werken met resources: [Resource Manager en het klassieke model](../azure-resource-manager/resource-manager-deployment-model.md).  Dit artikel bevat informatie over het Resource Manager-implementatiemodel, dat door Microsoft wordt aanbevolen voor de meeste nieuwe implementaties in plaats van het [klassieke implementatiemodel](load-balancer-get-started-ilb-classic-ps.md).

[!INCLUDE [load-balancer-get-started-ilb-scenario-include.md](../../includes/load-balancer-get-started-ilb-scenario-include.md)]

## <a name="get-started-creating-an-internal-load-balancer-using-azure-portal"></a>Aan de slag met het maken van een interne load balancer met behulp van Azure Portal

Gebruik de volgende stappen om een interne load balancer te maken vanuit Azure Portal.

1. Open een browser, ga naar [Azure Portal](http://portal.azure.com) en meld u aan met uw Azure-account.
2. Klik linksboven in het scherm op **Nieuw** > **Netwerken** > **Load balancer**.
3. Voer op de blade **Load balancer maken** een **naam** in voor de load balancer.
4. Klik onder **Schema** op **Intern**.
5. Klik op **Virtueel netwerk** en selecteer het virtuele netwerk waarin u de load balancer wilt maken.

   > [!NOTE]
   > Als u het te gebruiken virtuele netwerk niet ziet, controleert u de **locatie** van de load balancer en past u deze dienovereenkomstig aan.

6. Klik op **Subnet** en selecteer het subnet waarin u de load balancer wilt maken.
7. Klik onder **IP-adrestoewijzing** op **Dynamisch** of **Statisch**, afhankelijk van of u een vast (statisch) IP-adres voor de load balancer wilt of niet.

   > [!NOTE]
   > Als u voor het gebruik van een statisch IP-adres kiest, moet u een adres voor de load balancer opgeven.

8. Geef onder **Resourcegroep** de naam op van een nieuwe resourcegroep voor de load balancer of klik op **Bestaande selecteren** en selecteer een bestaande resourcegroep.
9. Klik op **Maken**.

## <a name="configure-load-balancing-rules"></a>Taakverdelingsregels configureren

Wanneer de load balancer is gemaakt, navigeert u naar de resource van de load balancer om deze te configureren.
Configureer een back-endadresgroep en een test voordat u een taakverdelingsregel configureert.

### <a name="step-1-configure-a-backend-pool"></a>Stap 1: Een back-endgroep configureren

1. Klik in Azure Portal op **Bladeren** > **Load balancers** en vervolgens op de load balancer die u eerder hebt gemaakt.
2. Klik op de blade **Instellingen** op **Back-endgroepen**.
3. Klik op de blade **Back-endadresgroepen** op **Toevoegen**.
4. Voer op de blade **Back-endgroep toevoegen** een **Naam** in voor de back-endgroep en klik op **OK**.

### <a name="step-2-configure-a-probe"></a>Stap 2: Een test configureren

1. Klik in Azure Portal op **Bladeren** > **Load balancers** en vervolgens op de load balancer die u eerder hebt gemaakt.
2. Klik op de blade **Instellingen** op **Tests**.
3. Klik op de blade **Tests** op **Toevoegen**.
4. Voer op de blade **Test toevoegen** een **Naam** in voor de test.
5. Selecteer onder **Protocol** **HTTP** (voor websites) of **TCP** (voor andere TCP-toepassingen).
6. Geef onder **Poort** op welke poort u voor de test wilt gebruiken.
7. Geef onder **Pad** (alleen voor HTTP-tests) het pad op dat u voor de test wilt gebruiken.
8. Geef onder **Interval** op hoe vaak u de toepassing wilt testen.
9. Geef onder **Drempelwaarde voor onjuiste status** op hoeveel pogingen er moeten mislukken voordat de virtuele back-endmachine wordt aangeduid als Niet in orde.
10. Klik op **OK** om de test te maken.

### <a name="step-3-configure-load-balancing-rules"></a>Stap 3: Taakverdelingsregels configureren

1. Klik in Azure Portal op **Bladeren** > **Load balancers** en vervolgens op de load balancer die u eerder hebt gemaakt.
2. Klik op de blade **Instellingen** op **Taakverdelingsregels**.
3. Klik op de blade **Taakverdelingsregels** op **Toevoegen**.
4. Voer op de blade **Taakverdelingsregel toevoegen** een **Naam** in voor de regel.
5. Selecteer onder **Protocol** **HTTP** (voor websites) of **TCP** (voor andere TCP-toepassingen).
6. Geef onder **Poort** op via welke poort clients verbinding maken met de load balancer.
7. Geef onder **Back-endpoort** de poort op die in de back-endgroep moet worden gebruikt (gewoonlijk zijn de load balancer-poort en de back-endpoort hetzelfde).
8. Selecteer onder **Back-endgroep** de back-endgroep die u eerder hebt gemaakt.
9. Selecteer onder **Sessiepersistentie** hoe sessies moeten worden standgehouden.
10. Geef onder **Time-out voor inactiviteit (minuten)** de time-out voor inactiviteit op.
11. Klik onder **Zwevend IP (Direct Server Return)** op **Uitgeschakeld** of **Ingeschakeld**.
12. Klik op **OK**.

## <a name="next-steps"></a>Volgende stappen

[Een distributiemodus voor de load balancer configureren](load-balancer-distribution-mode.md)

[TCP-time-outinstellingen voor inactiviteit voor de load balancer configureren](load-balancer-tcp-idle-timeout.md)

