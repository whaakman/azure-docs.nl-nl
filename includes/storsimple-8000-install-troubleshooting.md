<!--author=alkohli last changed: 08/29/17-->

## <a name="troubleshooting-update-failures"></a>Problemen oplossen met mislukte updates
**Wat moet ik doen als ik een melding zie dat de controles vóór de upgrade zijn mislukt?**

Als een controle vooraf mislukt, bekijkt u de gedetailleerde meldingsbalk onder aan de pagina. Hierin kunt u zien welke controle vooraf is mislukt. Bijvoorbeeld: u ontvangt een melding dat de statuscontrole van de domeincontroller en de statuscontrole van de hardware-onderdeel is mislukt. Ga naar **Monitor > Hardware health**. U moet ervoor zorgen dat beide domeincontrollers in orde en online zijn. U moet ook om ervoor te zorgen dat alle hardwareonderdelen in de StorSimple-apparaat te zijn in orde in deze blade worden weergegeven. U kunt daarna proberen de updates te installeren. Als het niet lukt om de problemen met de hardwareonderdelen op te lossen, neemt u contact op met Microsoft-ondersteuning voor de volgende stappen.

**Wat moet ik doen als ik de foutmelding "Kan geen updates installeren" zie en de aanbeveling krijg de gids voor het oplossen van problemen met updates te raadplegen om de oorzaak van het probleem vast te stellen?**

Een waarschijnlijke oorzaak hiervan is dat u geen verbinding hebt met de Microsoft Update-servers. Dit is een handmatige controle die moet worden uitgevoerd. Als u geen verbinding meer hebt met de updateserver, mislukt de updatetaak. U kunt de verbinding controleren door de volgende cmdlet uit te voeren vanuit de Windows PowerShell-interface van uw StorSimple-apparaat:

 `Test-Connection -Source <Fixed IP of your device controller> -Destination <Any IP or computer name outside of datacenter>`

Voer de cmdlet uit op beide controllers.

Als blijkt dat u wel verbinding hebt en het probleem blijft bestaan, neemt u contact op met Microsoft-ondersteuning voor de volgende stappen.

**Wat moet ik doen als ik een updatefout krijg tijdens het bijwerken van mijn apparaat naar Update 4 en op beide controllers Update 4 wordt uitgevoerd?**

Vanaf Update 4 gaan de controllers niet in de herstelmodus als op beide controllers dezelfde softwareversie wordt uitgevoerd en er een updatefout optreedt. Deze situatie kan zich voordoen als de hotfix voor de apparaatsoftware (update met eerste prioriteit) op beide domeincontrollers wordt uitgevoerd maar andere hotfixes (tweede en derde prioriteit) nog moeten worden toegepast. Vanaf Update 4 gaan de controllers alleen in de herstelmodus als op de twee controllers verschillende softwareversies worden uitgevoerd. 

Als u een updatefout krijgt en op beide controllers Update 4 wordt uitgevoerd, raden wij u aan een paar minuten te wachten en daarna de update opnieuw uit te voeren. Als de nieuwe poging ook mislukt, neemt u contact op met Microsoft-ondersteuning.
