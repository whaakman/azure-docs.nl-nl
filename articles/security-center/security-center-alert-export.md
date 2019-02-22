---
title: Waarschuwing logboeken exporteren naar een Azure Log Analytics-werkruimte | Microsoft Docs
description: In dit artikel wordt uitgelegd hoe u voor het exporteren van de waarschuwingen die zijn gegenereerd door Azure Security Center een Log Analytics-werkruimte.
services: security-center
documentationcenter: na
author: monhaber
manager: mbaldwin
editor: ''
ms.assetid: 2bc67ce5-ec3a-49df-afc3-b4bad5d8ce21
ms.service: security-center
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 2/13/2019
ms.author: monhaber
ms.openlocfilehash: 12b8b376a0ff149cbfafc7fe69dd8da636280de8
ms.sourcegitcommit: a4efc1d7fc4793bbff43b30ebb4275cd5c8fec77
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/21/2019
ms.locfileid: "56653700"
---
# <a name="export-azure-security-center-alerts"></a>Azure Security Center-waarschuwingen exporteren
In dit artikel wordt uitgelegd hoe u de waarschuwingen die worden gegenereerd door Azure Security Center naar een locatie die kan worden gebruikt door andere hulpprogramma's exporteren.

Sommige van de Security Center-klanten hebben moet gebruiken voor de gegevens die worden gegenereerd door Security Center vanaf een centrale locatie die alle samengevoegde en verwerkte gegevens (zoals meldingen voor geconstateerde bedreigingen, aanbevelingen voor beleid voor beveiliging, dekking bevat, uitgedrukt beveiligde score, enzovoort). Hierdoor kunnen de gegevens verder worden geanalyseerd en verwerkt. Of een andere manier via een programma gebruiken voor de gegevens, in plaats van de portal van Security Center kan ook zijn.

Omdat de Azure Log Analytics-werkruimten worden gebruikt voor het opslaan en verwerken van onbewerkte gegevens verzameld van virtuele machines, de **exporteren naar Log Analytics** functie is de oplossing voor deze nodig hebben, door de gegevens aan een werkruimte door de gebruiker gedefinieerde streaming.

## <a name="prerequisites"></a>Vereisten
U moet een Log Analytics-werkruimte. U wordt aangeraden dat u de Log Analytics-werkruimte die u hebt gedefinieerd in de instellingen voor het verzamelen van gegevens. bij het definiëren van een voor het verzamelen van gegevens. [Log Analytics-werkruimte voor het verzamelen van gegevens](security-center-enable-data-collection.md)


## <a name="export-the-alerts"></a>Exporteren van de waarschuwingen
1. Klik op **beveiligingsbeleid**.
1. In de regel van het abonnement dat u wilt gebruiken, klikt u op **instellingen bewerken**.
  ![Instellingen bewerken](./media/security-center-alert-export/edit_settings.png "instellingen bewerken")
1. Klik op **gegevensverzameling**.
1. Schuif omlaag naar **Store Security Center gegevens verrijkt** en klikt u op de aan/uit-optie voor het **op**.

   ![Gegevensoptie Store](./media/security-center-alert-export/store_data_option.png "beveiligingsbeleid")
1. Selecteer de werkruimte die u wilt de waarschuwingen te exporteren.

    > [!NOTE]
    > U kunt waarschuwingen niet exporteren naar de standaardwerkruimte Security Center. Zoals vermeld in de [vereisten](#Prerequisites), moet u een Log Analytics-werkruimte gebruiken.

1. Klik op **opslaan** (aan de bovenkant van het scherm).