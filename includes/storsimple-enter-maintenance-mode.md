<!--author=SharS last changed: 12/01/15-->

#### <a name="to-enter-maintenance-mode"></a>Onderhoudsmodus invoeren
1. Kies in het menu van de seriële console optie 1, **aanmelden met volledige toegang**.
2. Typ het wachtwoord. Is het standaardwachtwoord **Wachtwoord1**.
3. Typ het volgende achter de opdrachtprompt
   
     `Enter-HcsMaintenanceMode`
4. Hier ziet u een waarschuwingsbericht weergegeven waarin staat dat onderhoudsmodus wordt verstoord alle i/o-aanvragen en de verbinding met de klassieke Azure portal-server en wordt u gevraagd om bevestiging. Type **Y** onderhoudsmodus invoeren.
   
    Beide domeincontrollers wordt opnieuw opgestart. Wanneer het opnieuw opstarten voltooid is, verschijnt er een ander bericht waarmee wordt aangegeven dat het apparaat in de onderhoudsmodus.

