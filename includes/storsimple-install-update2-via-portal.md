<!--author=alkohli last changed: 02/06/17-->

#### <a name="to-install-an-update-from-the-azure-portal"></a>Een update installeren via Azure Portal

1. Selecteer uw apparaat op de pagina van de StorSimple-service. Navigeer naar **Apparaten** > **Onderhoud**.
2. Klik onderaan de pagina op **Updates zoeken**. Er wordt een taak gemaakt om te zoeken naar beschikbare updates. U ontvangt een melding wanneer de taak is voltooid.
3. In het gedeelte **Software-updates** op dezelfde pagina ziet u de beschikbare software-updates. We raden u aan de releaseopmerkingen te bekijken voordat u een update toepast op uw apparaat.
4. Klik onderaan de pagina op **Updates installeren** en vervolgens op **OK**.
5. Controleer of u de aanbevelingen in het dialoogvenster **Updates installeren** hebt gevolgd en selecteer vervolgens **I understand the above requirement and am ready to upgrade my device** (Ik begrijp de bovenstaande vereiste en ben klaar om mijn apparaat bij te werken) en klik op de selectieknop.
   
    ![Bevestigingsbericht](./media/storsimple-install-update2-via-portal/InstallUpdate12_2M.png)
6. Er wordt een reeks vereiste controles gestart. Deze controles zijn onder andere:
   
   * **Statuscontroles controllers** om te controleren of beide apparaatcontrollers in orde en online zijn.
   * **Statuscontroles hardwareonderdelen** om te controleren of alle hardwareonderdelen op uw StorSimple-apparaat in orde zijn.
   * **DATA 0-controles** om te controleren of DATA 0 is ingeschakeld op uw apparaat. Als deze interface niet is ingeschakeld, schakelt u deze in en probeert u het vervolgens opnieuw.
   * **DATA 2- en DATA 3-controles** om te controleren dat de DATA 2- en DATA 3-netwerkinterfaces zijn uitgeschakeld. Als deze interfaces zijn ingeschakeld, schakelt u deze uit en probeert u vervolgens uw apparaat bij te werken. Deze controle wordt alleen uitgevoerd als u een update uitvoert vanaf een apparaat met GA-software. Voor apparaten met versies 0.1, 0.2 of 0.3 is deze controle niet nodig.
   * **Gatewaycontrole** op elk apparaat waarop een versie ouder dan Update 1 wordt uitgevoerd. Deze controle wordt uitgevoerd op alle apparaten waarop software ouder dan Update 1 wordt uitgevoerd, maar mislukt op apparaten met een gateway die is geconfigureerd voor een andere netwerkinterface dan DATA 0.
     
     De update wordt uitgevoerd zodra alle controles zijn voltooid. U krijgt een melding wanneer de controles worden uitgevoerd.
     
     ![Melding voor controles](./media/storsimple-install-update2-via-portal/InstallUpdate12_3M.png)
     
     Hier volgt een voorbeeld waarin de controles zijn mislukt. U dient te controleren of beide apparaatcontrollers in orde en online zijn. U dient ook de status van de hardwareonderdelen te controleren. In dit voorbeeld vereisen de onderdelen Controller 0 en Controller 1 aandacht. Mogelijk moet u contact opnemen met Microsoft-ondersteuning als u deze problemen niet zelf kunt oplossen.
     
       ![Controles zijn mislukt](./media/storsimple-install-update2-via-portal/HCS_PreUpgradeChecksFailed-include.png)
7. Nadat de controles zijn voltooid, wordt er een bijwerktaak gemaakt. U krijgt een melding wanneer de bijwerktaak is gemaakt.
   
    ![Maken van bijwerktaak](./media/storsimple-install-update2-via-portal/InstallUpdate12_44M.png)
   
    De update wordt vervolgens op het apparaat toegepast.
    
8. Om de voortgang van de bijwerktaak te controleren, klikt u op **Taak weergeven**. Op de pagina **Taken** ziet u de voortgang van de update.
9. De update neemt enkele uren in beslag. Selecteer de bijwerktaak en klik op **Details** om de details van de taak te bekijken.
10. Nadat de taak is voltooid, gaat u naar de pagina **Onderhoud** en scrolt u omlaag naar **Software-updates**.

