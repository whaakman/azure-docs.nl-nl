---
author: rayne-wiselman
ms.service: site-recovery
ms.topic: include
ms.date: 10/26/2018
ms.author: raynew
ms.openlocfilehash: 290bca5f3839356817651096e6a274c868a5a268
ms.sourcegitcommit: 48592dd2827c6f6f05455c56e8f600882adb80dc
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/26/2018
ms.locfileid: "50166201"
---
1. Start CSPSConfigtool.exe op de configuratieserver. Dit bestand is beschikbaar als snelkoppeling op het bureaublad en in de map *installatielocatie*\home\svsystems\bin.
2. Klik op **Accounts beheren** > **Account toevoegen**.

    ![Account toevoegen](./media/site-recovery-add-vcenter-account/credentials1.png)
3. Voeg in **Accountdetails** het account toe dat moet worden gebruikt voor automatische detectie.

    ![Details](./media/site-recovery-add-vcenter-account/credentials2.png)

    > [!Note]
  > Het duurt 15 minuten of langer voordat de accountnaam wordt weergegeven in de portal. Klik op **Configuratieservers** > ***servernaam*** > **Server vernieuwen** om direct bij te werken.
