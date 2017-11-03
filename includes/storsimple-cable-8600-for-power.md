<!--author=alkohli last changed: 9/16/15-->


#### <a name="to-cable-your-device-for-power"></a>Op uw apparaat voor power bekabelen
> [!NOTE]
> Beide behuizingen op uw StorSimple-apparaat bevatten redundante PCMs. Voor elke behuizing moeten de PCMs worden geïnstalleerd en is verbonden met verschillende energiebeheerschema bronnen om hoge beschikbaarheid te garanderen.
> 
> 

1. Zorg ervoor dat de energie-switches op alle PCMs op OFF positie.
2. Op de primaire behuizing de stroomkabels een verbinding met beide PCMs. De stroomkabels worden aangeduid in rood in het power kabels diagram hieronder.
3. Zorg ervoor dat de twee PCMs op de primaire behuizing afzonderlijke stroomvoorziening gebruikt.
4. Koppel de stroomkabels tot de macht op de distributiepunten rackeenheden zoals weergegeven in het diagram bekabeling power.
5. Herhaal stappen 2 t/m 4 voor de EBOD behuizing.
6. De behuizing EBOD inschakelen door het spiegelen van de switch power op elke PCM naar de ON-positie.
7. Controleer of dat de behuizing EBOD is ingeschakeld door te controleren dat de groene LED's op de achterkant van de controller EBOD zijn ingeschakeld.
8. De primaire behuizing inschakelen door het spiegelen van elke PCM-switch op de positie op.
9. Controleer of het systeem op door ervoor te zorgen de apparaat-controller die LED 's hebt ingeschakeld.
10. Zorg ervoor dat de verbinding tussen de EBOD-controller en de controller apparaat actief is, door te controleren of de vier LED's naast de SAS-poort op de controller EBOD groen zijn.
    
    > [!IMPORTANT]
    > Om ervoor te zorgen hoge beschikbaarheid voor uw systeem, is het raadzaam dat u strikt voldoen aan de kracht bekabeling schema in het volgende diagram wordt weergegeven.
    > 
    > 
    
    ![Uw apparaat 4U voor power bekabelen](./media/storsimple-cable-8600-for-power/HCSCableYour4UDeviceforPower.png)
    
    **Power bekabeling**
    
    | Label | Beschrijving |
    |:--- |:--- |
    | 1 |Primaire behuizing |
    | 2 |PCM 0 |
    | 3 |PCM 1 |
    | 4 |Controller 0 |
    | 5 |Controller 1 |
    | 6 |EBOD-controller 0 |
    | 7 |EBOD controller 1 |
    | 8 |EBOD behuizing |
    | 9 |PDU 's |

