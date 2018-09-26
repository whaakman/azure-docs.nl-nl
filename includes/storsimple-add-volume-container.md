<!--author=SharS last changed: 1/7/2016-->

#### <a name="to-add-a-volume-container"></a>Een volumecontainer toevoegen
1. Op de **apparaten** pagina, selecteert u het apparaat, dubbelklik hierop en klik vervolgens op de **volumecontainers** tabblad.
2. Klik op **toevoegen** aan de onderkant van de pagina. In de **volumecontainer maken** dialoogvenster de volgende handelingen uit:
   
   1. Geef een unieke **naam** voor uw volumecontainer. Deze naam kan een maximum van 32 tekens bevatten.
   2. Selecteer een **Opslagaccount** moet worden gekoppeld aan deze volumecontainer. U kunt kiezen uit een bestaand opslagaccount in hetzelfde abonnement of selecteer **meer toevoegen** een storage-account selecteren uit een ander abonnement. U kunt ook het opslagaccount die eerst is gegenereerd toen de service werd gemaakt.
   3. Geef de bandbreedte in zoals **onbeperkt** als u wilt alle beschikbare bandbreedte in beslag nemen of **aangepaste** naar bandbreedtebesturingselementen. Geef een waarde tussen 1 en 1000 Mbps voor een aangepaste bandbreedte. Als u wilt toewijzen op basis van de volgens een schema, kunt u **bandbreedtesjabloon te selecteren**.
   4. Het is raadzaam dat u houden **versleuteling van Cloudopslag inschakelen** geselecteerd voor het versleutelen van de gegevens die u naar de cloud. Versleuteling alleen uitschakelen als u die gebruikmaakt van andere manier om uw gegevens te versleutelen. U kunt de versleuteling-instelling zodra de volumecontainer is gemaakt niet wijzigen.
   5. Geef een **coderingssleutel voor Cloudopslag** die tussen 8 en 32 tekens bevat. Het apparaat gebruikt deze sleutel voor toegang tot de versleutelde gegevens. In de **bevestigen coderingssleutel voor Cloudopslag** en voer de versleutelingssleutel voor cloudopslag nogmaals ter bevestiging. 
   6. Klik op de pijl om door te gaan naar de volgende pagina.
      
      ![Volumecontainer maken met de bandbreedtesjabloon 1](./media/storsimple-add-volume-container/HCS_CreateVCBT1-include.png) 
3. Als u hebt opgegeven **bandbreedtesjabloon te selecteren**, kiest u in de vervolgkeuzelijst van bestaande bandbreedtesjablonen. Controleer de schema-instellingen en klik op het vinkje ![vinkje](./media/storsimple-configure-new-storage-account/HCS_CheckIcon-include.png).
   
    ![Volumecontainer maken met de bandbreedtesjabloon 2](./media/storsimple-add-volume-container/HCS_CreateVCBT2-include.png) 

De volumecontainer wordt opgeslagen en de zojuist gemaakte volumecontainer wordt weergegeven op de **volumecontainer** pagina.

