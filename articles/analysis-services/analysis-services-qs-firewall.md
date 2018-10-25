---
title: 'Snelstart: Een firewall configureren voor een Analysis Services-server in Azure | Microsoft Docs'
description: Informatie over het configureren van een firewall voor een instantie van Analysis Services-server in Azure.
author: minewiskan
manager: kfile
ms.service: azure-analysis-services
ms.topic: quickstart
ms.date: 10/18/2018
ms.author: owend
ms.reviewer: minewiskan
ms.openlocfilehash: 96141ddeeb73c7483935d5da49b9341436c10c18
ms.sourcegitcommit: 707bb4016e365723bc4ce59f32f3713edd387b39
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/19/2018
ms.locfileid: "49429687"
---
# <a name="quickstart-configure-server-firewall---portal"></a>Snelstart: Een serverfirewall configureren - Portal

Deze snelstart helpt u bij het configureren van een firewall voor uw Azure Analysis Services-server. Een firewall inschakelen en het configureren van IP-adresbereiken voor uitsluitend de computers die toegang tot de server hebben, speelt een belangrijke rol bij het beveiligen van uw server en uw gegevens.

## <a name="prerequisites"></a>Vereisten

- Een Azure Analysis Services-server in uw abonnement. Zie [Snelstart: Een server maken - Portal](analysis-services-create-server.md) of [Snelstart: Een server maken - PowerShell](analysis-services-create-powershell.md) voor meer informatie.
- Een of meer IP-adresbereiken voor clientcomputers (indien nodig).

## <a name="log-in-to-the-azure-portal"></a>Aanmelden bij Azure Portal 

[Meld u aan bij de portal](https://portal.azure.com).

## <a name="configure-a-firewall"></a>Een firewall configureren

1. Klik op de server om de overzichtspagina te openen. 
2. Klik in **INSTELLINGEN** > **Firewall** > **Firewall inschakelen** op **Aan**.
3. Klik op **Aan** bij **Toegang toestaan vanuit Power BI** om toegang tot DirectQuery toe te staan vanuit Power BI-service.  
4. (Optioneel) Geef een of meer IP-adresbereiken op. Typ een naam, begin- en eind-IP-adressen voor elk bereik. 
5. Klik op **Opslaan**.

     ![Firewallinstellingen](./media/analysis-services-qs-firewall/aas-qs-firewall.png)

## <a name="clean-up-resources"></a>Resources opschonen

Wanneer deze niet langer nodig zijn, verwijdert u de IP-adresbereiken of schakelt u de firewall uit.

## <a name="next-steps"></a>Volgende stappen
In deze snelstart hebt u geleerd hoe u een firewall configureert voor uw server. Nu dat u een server hebt en deze hebt beveiligd met een firewall, kunt u een eenvoudig voorbeeldgegevensmodel vanuit de portal toevoegen. Een voorbeeldmodel is handig als u meer wilt weten over het configureren van modeldatabaserollen en het testen van clientverbindingen. Als u meer wilt weten, gaat u verder met de zelfstudie waarin u leert een voorbeeldmodel toe te voegen.

> [!div class="nextstepaction"]
> [Zelfstudie: Een voorbeeldmodel toevoegen aan uw server](analysis-services-create-sample-model.md)
