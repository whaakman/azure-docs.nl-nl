<!--author=alkohli last changed:02/22/16-->

#### <a name="to-attach-the-sas-cables"></a>De SAS-kabels los te koppelen
1. Identificeer de primaire en de bijlagen EBOD. De twee bijlagen kunnen worden geïdentificeerd door te kijken hun respectieve back vlakken. Zie de volgende afbeelding voor hulp. 
   
    ![Back-vlak van primaire en EBOD behuizingen](./media/storsimple-sas-cable-8600/HCSBackplaneofprimaryandEBODenclosure.png)
   
    **Back-weergave van de primaire en EBOD behuizingen**
   
   | Label | Beschrijving |
   |:--- |:--- |
   | 1 |Primaire behuizing |
   | 2 |EBOD behuizing |
2. Zoek de serienummers op de primaire en de bijlagen EBOD. Het serienummer sticker is op het vorige oor van elke behuizing aangebracht. De serienummers moet identiek zijn op beide behuizingen. [Neem contact op met Microsoft Support](../articles/storsimple/storsimple-contact-microsoft-support.md) onmiddellijk als de serienummers komen niet overeen. Zie de volgende afbeelding worden de serienummers gevonden.
   
    ![Weergave van de achterzijde van behuizing met van serienummer](./media/storsimple-sas-cable-8600/HCSRearviewofenclosureindicatinglocationofserialnumbersticker.png)
   
    **Locatie van het serienummer sticker**
   
   | Label | Beschrijving |
   |:--- |:--- |
   | 1 |Wissen van de behuizing |
3. Gebruik de opgegeven SAS-kabels verbinding maken met de behuizing EBOD de primaire behuizing als volgt:
   
   1. De vier SAS-poorten op de primaire ruimte en de behuizing EBOD identificeren. De SAS-poorten zijn gelabeld als EBOD op de primaire behuizing en overeen met een poort op de behuizing EBOD, zoals weergegeven in de afbeelding hieronder bekabeling SAS.
   2. De opgegeven SAS-kabels gebruiken voor verbinding van de poort-poort EBOD A.
   3. De poort EBOD op de controller 0 moet zijn verbonden met de poort A op EBOD controller 0. De poort EBOD op de controller 1 moet zijn verbonden met de poort A op EBOD controller 1. Zie de volgende afbeelding voor hulp. 
      
      ![SAS bekabeling voor uw apparaat](./media/storsimple-sas-cable-8600/HCSSAScablingforyourdevice.png)
      
      **SAS bekabeling**
      
      | Label | Beschrijving |
      |:--- |:--- |
      | A |Primaire behuizing |
      | B |EBOD behuizing |
      | 1 |Controller 0 |
      | 2 |Controller 1 |
      | 3 |EBOD-Controller 0 |
      | 4 |EBOD Controller 1 |
      | 5, 6 |SAS-poorten op de primaire behuizing (gelabelde EBOD) |
      | 7, 8 |SAS-poorten op EBOD behuizing (A-poort) |

