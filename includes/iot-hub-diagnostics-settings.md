### <a name="enable-logging-with-diagnostics-settings"></a>Logboekregistratie met instellingen voor diagnostische gegevens inschakelen

1. Aanmelden bij de [Azure-portal] [ lnk-portal] en navigeer naar uw IoT-Hub.
1. Selecteer **diagnostische instellingen**.
1. Selecteer **diagnostische gegevens inschakelen**.

   ![Diagnostische gegevens inschakelen][1]

1. De diagnostische instellingen voor een naam geven.
1. Kies waar u de logboeken verzenden. U kunt een combinatie van de drie opties selecteren:
   * Archiveren naar een opslagaccount
   * Stream naar een event hub
   * Verzenden met Log Analytics
1. Kies welke bewerkingen u wilt bewaken en logboeken voor deze bewerkingen inschakelen. De bewerkingen die diagnostische instellingen kunnen rapporteren zijn:
   * Verbindingen
   * De apparaattelemetrie
   * Cloud-naar-apparaat-berichten
   * Bewerkingen voor apparaat-id
   * Uploaden van bestanden
   * Berichtroutering
   * Cloud-naar-apparaat twin bewerkingen
   * Apparaat-naar-cloud-twin bewerkingen
   * Dubbele bewerkingen
   * Taakbewerkingen
   * Directe methoden  
1. De nieuwe instellingen opslaan. 

Als u inschakelen op de instellingen voor diagnostische gegevens met PowerShell wilt, gebruikt u de volgende code:

```
Login-AzureRmAccount
Select-AzureRmSubscription -SubscriptionName <subscription that includes your IoT Hub>
Set-AzureRmDiagnosticSetting -ResourceId <your resource Id> -ServiceBusRuleId <your service bus rule Id> -Enabled $true
```

Nieuwe instellingen van kracht in ongeveer 10 minuten. Daarna logboeken worden weergegeven in de geconfigureerde archivering doel op de **diagnostische instellingen** blade. Zie voor meer informatie over het configureren van diagnostische gegevens [verzamelen en gebruiken van de logboekgegevens van uw azure-resources][lnk-diagnostics-settings].

<!-- Images -->
[1]: ./media/iot-hub-diagnostics-settings/turnondiagnostics.png

<!-- Links -->
[lnk-portal]: https://portal.azure.com
[lnk-diagnostics-settings]: ../articles/monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs.md
