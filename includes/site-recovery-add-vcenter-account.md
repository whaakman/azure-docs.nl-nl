---
author: rayne-wiselman
ms.service: site-recovery
ms.topic: include
ms.date: 10/26/2018
ms.author: raynew
ms.openlocfilehash: cd18d71d26410767a2d3119c12a1339bdc84bd33
ms.sourcegitcommit: 5839af386c5a2ad46aaaeb90a13065ef94e61e74
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/19/2019
ms.locfileid: "58107138"
---
1. Start CSPSConfigtool.exe op de configuratieserver. Dit bestand is beschikbaar als snelkoppeling op het bureaublad en in de map *installatielocatie*\home\svsystems\bin.
2. Klik op **Accounts beheren** > **Account toevoegen**.

    ![Account toevoegen](./media/site-recovery-add-vcenter-account/credentials1.png)
3. Voeg in **Accountdetails** het account toe dat moet worden gebruikt voor automatische detectie.

    ![Details](./media/site-recovery-add-vcenter-account/credentials2.png)

    > [!Note]
   > Het duurt 15 minuten of langer voordat de accountnaam wordt weergegeven in de portal. Klik op **Configuratieservers** > ***servernaam*** > **Server vernieuwen** om direct bij te werken.
