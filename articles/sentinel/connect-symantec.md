---
title: Symantec ICDX gegevens verbinden met Azure Sentinel Preview | Microsoft Docs
description: Leer hoe u verbinding maken met gegevens van de Symantec ICDX Sentinel van Azure.
services: sentinel
documentationcenter: na
author: rkarlin
manager: rkarlin
editor: ''
ms.assetid: d068223f-395e-46d6-bb94-7ca1afd3503c
ms.service: sentinel
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 04/07/2019
ms.author: rkarlin
ms.openlocfilehash: 0410b052f17a868aed70ce407b9c9fdefbe023df
ms.sourcegitcommit: 2ce4f275bc45ef1fb061932634ac0cf04183f181
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/07/2019
ms.locfileid: "65233636"
---
# <a name="connect-your-symantec-icdx-appliance"></a>Verbinding maken met uw Symantec ICDX-apparaat 

> [!IMPORTANT]
> Azure Sentinel is momenteel in openbare preview.
> Deze preview-versie wordt aangeboden zonder service level agreement en wordt niet aanbevolen voor productieworkloads. Misschien worden bepaalde functies niet ondersteund of zijn de mogelijkheden ervan beperkt. Zie [Supplemental Terms of Use for Microsoft Azure Previews (Aanvullende gebruiksvoorwaarden voor Microsoft Azure-previews)](https://azure.microsoft.com/support/legal/preview-supplemental-terms/) voor meer informatie.

Symantec ICDX connector kunt u eenvoudig alle uw beveiligingslogboeken voor Symantec verbinding te maken met uw Azure Sentinel, dashboards weergeven, aangepaste waarschuwingen maken en onderzoek te verbeteren. Dit biedt u meer inzicht in het netwerk van uw organisatie en verbetert uw mogelijkheden voor beveiliging bewerking. Integratie tussen Symantec ICDX en Azure Sentinel maakt gebruik van REST-API.


> [!NOTE]
> Gegevens worden opgeslagen in de geografische locatie van de werkruimte waarop u werkt met Azure Sentinel.

## <a name="configure-and-connect-symantec-icdx"></a>Configureren en verbinden van Symantec ICDX 

Symantec ICDX kunt integreren en logboeken exporteren rechtstreeks naar Azure Sentinel.

1. Open de beheerconsole ICDX.
2. Selecteer in het navigatiemenu links **configuratie** en vervolgens de **doorstuurservers** tabblad.
3. Klik in de Microsoft Azure Log Analytics-rij, **meer**, gevolgd door **bewerken**. 
4. In de **Microsoft Azure Log Analytics Forwarder** venster, stel het volgende:
    - Aangepaste logboeknaam laat de standaard, SymantecICDX.
    - De werkruimte-ID Kopieer en plak deze in de **klant-id** veld. Kopieer de **primaire sleutel** en plak deze in het veld gedeelde sleutel. U kunt deze waarden kopiëren vanuit Azure Sentinel portal door te selecteren **gegevensconnectors** en vervolgens **Symantec ICDX**.
6. Zoek voor het gebruik van de relevante schema in Log Analytics voor de Symantec ICDX gebeurtenissen, **SymantecICDX_CL**.


## <a name="validate-connectivity"></a>Verbinding valideren

Het duurt al twintig minuten tot de logboeken in Log Analytics wordt weergegeven. 



## <a name="next-steps"></a>Volgende stappen
In dit document hebt u geleerd hoe u verbinding maken met Symantec ICDX Sentinel van Azure. Zie voor meer informatie over Azure Sentinel, de volgende artikelen:
- Meer informatie over het [Krijg inzicht in uw gegevens en mogelijke bedreigingen](quickstart-get-visibility.md).
- Aan de slag [detecteren van bedreigingen met Azure Sentinel](tutorial-detect-threats.md).

