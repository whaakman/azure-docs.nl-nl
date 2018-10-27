---
author: alkohli
ms.service: storsimple
ms.topic: include
ms.date: 10/26/2018
ms.author: alkohli
ms.openlocfilehash: d3a810523807a46ca3f410c7cb5e11884f2b06b7
ms.sourcegitcommit: 48592dd2827c6f6f05455c56e8f600882adb80dc
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/26/2018
ms.locfileid: "50164497"
---
<!--author=alkohli last changed: 08/04/17-->

#### <a name="to-install-an-update-from-the-azure-portal"></a>Een update installeren via Azure Portal

1. Selecteer uw apparaat op de pagina van de StorSimple-service.

    ![Apparaat selecteren](./media/storsimple-8000-install-update5-via-portal/update1.png)

2. Navigeer naar **apparaatinstellingen** > **apparaatupdates**.

    ![Klik op bijwerken van apparaten](./media/storsimple-8000-install-update5-via-portal/update2.png)

2. Een melding wordt weergegeven als er nieuwe updates beschikbaar zijn. U kunt ook in de **apparaatupdates** blade, klikt u op **Updates zoeken**. Er wordt een taak gemaakt om te zoeken naar beschikbare updates. U ontvangt een melding wanneer de taak is voltooid.

    ![Klik op bijwerken van apparaten](./media/storsimple-8000-install-update5-via-portal/update3.png)

3. We raden u aan de releaseopmerkingen te bekijken voordat u een update toepast op uw apparaat. Updates, klikt u op **updates installeren**. In de **regelmatig updates bevestigen** blade, controleert u de vereisten om te voltooien voordat u updates toepassen. Schakel het selectievakje in om aan te geven dat u gereed bent voor het bijwerken van het apparaat en klik vervolgens op **installeren**.

    ![Klik op bijwerken van apparaten](./media/storsimple-8000-install-update5-via-portal/update4.png)

6. Er wordt een reeks vereiste controles gestart. Deze controles zijn onder andere:
   
   * **Statuscontroles controllers** om te controleren of beide apparaatcontrollers in orde en online zijn.
   * **Statuscontroles hardwareonderdelen** om te controleren of alle hardwareonderdelen op uw StorSimple-apparaat in orde zijn.
   * **DATA 0-controles** om te controleren of DATA 0 is ingeschakeld op uw apparaat. Als deze interface niet is ingeschakeld, schakelt u deze in en probeert u het vervolgens opnieuw.

    De update wordt gedownload en geïnstalleerd alleen als alle controles zijn voltooid. U krijgt een melding wanneer de controles worden uitgevoerd. Als de apparaatupdates is mislukt, klikt u vervolgens ontvangt u met de oorzaak van de fout. Deze problemen op te lossen en probeer het vervolgens opnieuw. Mogelijk moet u contact opnemen met Microsoft-ondersteuning als u deze problemen niet zelf kunt oplossen.

7. Nadat de apparaatupdates zijn voltooid, wordt een updatetaak gemaakt. U krijgt een melding wanneer de bijwerktaak is gemaakt.
   
    ![Maken van bijwerktaak](./media/storsimple-8000-install-update5-via-portal/update6.png)
   
    De update wordt vervolgens op het apparaat toegepast.

9. De update neemt enkele uren in beslag. Selecteer de bijwerktaak en klik op **Details** om de details van de taak te bekijken.

    ![Maken van bijwerktaak](./media/storsimple-8000-install-update5-via-portal/update8.png)

     U kunt ook de voortgang van de taak voor het bijwerken van **apparaatinstellingen > taken**. Op de **taken** blade ziet u de voortgang van de update.

     ![Maken van bijwerktaak](./media/storsimple-8000-install-update5-via-portal/update7.png)

10. Nadat de taak voltooid is, gaat u naar de **apparaatinstellingen > apparaatupdates**. Versie van de software moet nu worden bijgewerkt.

