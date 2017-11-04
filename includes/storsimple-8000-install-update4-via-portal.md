<!--author=alkohli last changed: 07/07/17-->

#### <a name="to-install-an-update-from-the-azure-portal"></a>Een update installeren via Azure Portal

1. Selecteer uw apparaat op de pagina van de StorSimple-service.

    ![Selecteer het apparaat](./media/storsimple-8000-install-update4-via-portal/update1.png)

2. Navigeer naar **apparaatinstellingen** > **apparaatupdates**.

    ![Klik op apparaat-updates](./media/storsimple-8000-install-update4-via-portal/update2.png)

2. Er wordt een melding weergegeven als er nieuwe updates beschikbaar zijn. U kunt ook in de **apparaatupdates** blade, klikt u op **Updates zoeken**. Er wordt een taak gemaakt om te zoeken naar beschikbare updates. U ontvangt een melding wanneer de taak is voltooid.

    ![Klik op apparaat-updates](./media/storsimple-8000-install-update4-via-portal/update3.png)

3. We raden u aan de releaseopmerkingen te bekijken voordat u een update toepast op uw apparaat. Updates, klikt u op **updates installeren**. In de **bevestigen regelmatig updates** blade, Raadpleeg de vereisten om te voltooien voordat u updates toepassen. Schakel het selectievakje in om aan te geven dat u klaar bent voor het apparaat bijwerken en klik vervolgens op **installeren**.

    ![Klik op apparaat-updates](./media/storsimple-8000-install-update4-via-portal/update4.png)

6. Er wordt een reeks vereiste controles gestart. Deze controles zijn onder andere:
   
   * **Statuscontroles controllers** om te controleren of beide apparaatcontrollers in orde en online zijn.
   * **Statuscontroles hardwareonderdelen** om te controleren of alle hardwareonderdelen op uw StorSimple-apparaat in orde zijn.
   * **DATA 0-controles** om te controleren of DATA 0 is ingeschakeld op uw apparaat. Als deze interface niet is ingeschakeld, schakelt u deze in en probeert u het vervolgens opnieuw.

    De update is gedownload en wordt alleen geïnstalleerd als alle controles zijn voltooid. U krijgt een melding wanneer de controles worden uitgevoerd. Als de prechecks mislukken, vervolgens krijgt u met de oorzaak van de fout. Deze problemen op te lossen en probeer het opnieuw. Mogelijk moet u contact opnemen met Microsoft-ondersteuning als u deze problemen niet zelf kunt oplossen.

7. Nadat de prechecks zijn voltooid, wordt een updatetaak gemaakt. U krijgt een melding wanneer de bijwerktaak is gemaakt.
   
    ![Maken van bijwerktaak](./media/storsimple-8000-install-update4-via-portal/update6.png)
   
    De update wordt vervolgens op het apparaat toegepast.

9. De update neemt enkele uren in beslag. Selecteer de bijwerktaak en klik op **Details** om de details van de taak te bekijken.

    ![Maken van bijwerktaak](./media/storsimple-8000-install-update4-via-portal/update8.png)

     U kunt ook de voortgang van de update-taak uit **apparaatinstellingen > taken**. Op de **taken** blade ziet u de voortgang van de update.

     ![Maken van bijwerktaak](./media/storsimple-8000-install-update4-via-portal/update7.png)

10. Nadat de taak voltooid is, gaat u naar de **apparaatinstellingen > apparaatupdates**. Versie van de software moet nu worden bijgewerkt.

    ![Maken van bijwerktaak](./media/storsimple-8000-install-update4-via-portal/update9.png)

