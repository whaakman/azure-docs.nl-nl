---
title: Het oplossen van Azure Data Factory | Microsoft Docs
description: Problemen oplossen met Azure Data Factory. Algemene documenten voor alle externe, controleactiviteiten.
services: data-factory
author: abnarain
manager: craigg
ms.service: data-factory
ms.topic: troubleshooting
ms.date: 6/26/2019
ms.author: abnarain
ms.reviewer: craigg
ms.openlocfilehash: d220730bb2e93e32d00e56ed98f4962ad89eda5a
ms.sourcegitcommit: c0419208061b2b5579f6e16f78d9d45513bb7bbc
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/08/2019
ms.locfileid: "67626315"
---
# <a name="troubleshooting-azure-data-factory"></a>Oplossen van problemen met Azure Data Factory
In dit artikel geeft een lijst met veelgestelde vragen voor probleemoplossing.

- [Azure Databricks (laptop, JAR-bestanden, python)](#azure-databricks)
- [Azure Data Lake Analytics (U-SQL)](#azure-data-lake-analytics-u-sql)
- [Azure Functions](#azure-functions)
- [Aangepast (Azure Batch)](#custom-azure-batch)
- [HDInsight (Spark, Hive, MapReduce, Pig, Hadoop Streaming)](#hdinsight-spark-hive-mapreduce-pig-hadoop-streaming)
- [Webactiviteit](#web-activity)



## <a name="azure-databricks"></a>Azure Databricks
| Foutcode | Foutbericht                                          | Beschrijving van het probleem                             | Mogelijke oplossing / aanbevolen actie                            |
| -------------- | ----------------------------------------------------- | --------------------------------------------------------------| :----------------------------------------------------------- |
| 3200           | Fout 403                                                    | Databricks-toegangstoken is verlopen.                         | Standaard is de Databricks-toegangstoken geldig gedurende 90 dagen.  Maak een nieuw token en bijwerken van de gekoppelde service. |
| 3201           | Vereist veld ontbreekt: settings.task.notebook_task.notebook_path | Ongeldige ontwerpen: Pad van het notitieblok niet correct opgegeven. | Geef pad van het notitieblok in Databricks-activiteit. |
| 3201           | Cluster... bestaat niet                                 | Fout bij schrijven: Databricks-cluster bestaat niet of is verwijderd. | Controleer of het Databricks-cluster bestaat. |
| 3201           | Ongeldige python-bestand URI:... Ga naar de gebruikershandleiding Databricks voor ondersteunde URI-schema's. | Ongeldige ontwerpen                                                | Absolute paden voor werkruimte adressering schema's of 'dbfs:/folder/subfolder/foo.py' voor DBFS opgeslagen bestanden opgeven. |
| 3201           | {0}   Gekoppelde service moet-domein en accessToken hebben als de vereiste eigenschappen | Ongeldige ontwerpen                                                | Controleer of [gekoppelde servicedefinitie](compute-linked-services.md#azure-databricks-linked-service). |
| 3201           | {0}   Gekoppelde service moet opgeven cluster Id van de bestaande of nieuwe clustergegevens voor het maken van | Ongeldige ontwerpen                                                | Controleer of [gekoppelde servicedefinitie](compute-linked-services.md#azure-databricks-linked-service). |
| 3201           | Knooppunttype Standard_D16S_v3 wordt niet ondersteund. Ondersteunde typen:   Standard_DS3_v2, Standard_DS4_v2, Standard_DS5_v2, Standard_D8s_v3, Standard_D16s_v3, Standard_D32s_v3, Standard_D64s_v3, Standard_D3_v2, Standard_D8_v3, Standard_D16_v3, Standard_D32_v3, Standard_D64_v3, Standard_D12_v2, Standard_D13_v2, Standard_D14_v2, Standard_D15_v2, Standard_DS12_v2, Standard_DS13_v2, Standard_DS14_v2, Standard_DS15_v2, Standard_E8s_v3, Standard_E16s_v3, Standard_E32s_v3, Standard_E64s_v3, Standard_L4s, Standard_L8s, Standard_L16s, Standard_L32s, Standard_F4s, Standard_F8s, Standard_F16s, Standard_H16, Standard_F4s_v2, Standard_F8s_v2, Standard_F16s_v2, Standard_F32s_v2, Standard_F64s_v2, Standard_F72s_v2, Standard_NC12, Standard_NC24, Standard_NC6s_v3, Standard_NC12s_v3, Standard_ NC24s_v3, Standard_L8s_v2, Standard_L16s_v2, Standard_L32s_v2, Standard_L64s_v2, Standard_L80s_v2 | Ongeldige ontwerpen                                                | Raadpleeg foutbericht                                          |
| 3201           | Ongeldige notebook_path:... Alleen absolute paden worden momenteel ondersteund. Paden moeten beginnen met '/'. | Ongeldige ontwerpen                                                | Raadpleeg foutbericht                                          |
| 3202           | Er zijn al gemaakt in de afgelopen 3600 seconden, overschrijdt de limiet van-1000 taken:   1000 taak gemaakte per 3600 seconden. | Te veel Databricks wordt uitgevoerd in een uur.                         | Controleer alle pijplijnen die gebruikmaken van deze Databricks-werkruimte voor een taak maken-snelheid.  Als pijplijnen te veel Databricks wordt uitgevoerd in een statistische functie gestart, moet u enkele pijplijnen migreren naar een nieuwe werkruimte. |
| 3202           | Kan de request-object niet parseren: Hebt u verwachte 'sleutel' en 'value' moet worden ingesteld voor JSON-kaart veld base_parameters ' sleutel: '...' ' in plaats daarvan. | Fout bij schrijven: Er is geen waarde opgegeven voor de parameter         | Controleer pijplijn-json en zorg ervoor dat alle parameters in de Notebook baseParameters hebben een niet-lege waarde die is opgegeven. |
| 3202           | Gebruiker: SimpleUserContext {userId =..., naam =user@company.com, orgId =...} is niet gemachtigd voor toegang tot cluster | Gebruikers die gegenereerd van het toegangstoken is niet toegestaan voor toegang tot het Databricks-cluster dat is opgegeven in de gekoppelde service. | Zorg ervoor dat de gebruiker de vereiste machtigingen heeft in de werkruimte.   |
| 3203           | Het cluster wordt beëindigd, niet beschikbaar voor het ontvangen van taken. Los van het cluster op of probeer het later opnieuw. | Cluster is beëindigd.    Dit kan een zeldzame situatie zijn voor interactieve cluster. | Beste manier om te voorkomen dat dit is het gebruik van taak-clusters.             |
| 3204           | Het uitvoeren van de taak is mislukt. Er mag een willekeurig aantal foutberichten van onverwachte clusterstatus activiteit-specifieke bericht.  Meest voorkomende is helemaal geen foutmelding. | N/A                                                          | N/A                                                          |



## <a name="azure-data-lake-analytics-u-sql"></a>Azure Data Lake Analytics (U-SQL)

| Foutcode         | Foutbericht                                                | Beschrijving van het probleem                                          | Mogelijke oplossing / aanbevolen actie                             |
| -------------------- | ------------------------------------------------------------ | ------------------------------------------------------------ | ------------------------------------------------------------ |
| 2709                 | Het toegangstoken is afkomstig van de verkeerde tenant                    | Onjuiste AAD-tenant                                         | De Service-Principal gebruikt voor toegang tot de ADLA behoort tot een andere AAD-tenant. Maak een nieuwe Service-Principal in dezelfde tenant als ADLA-account. |
| 2711,   2705,   2704 | Is niet toegestaan. ACL-verificatie is mislukt. De resource bestaat niet of de gebruiker is niet gemachtigd voor de aangevraagde bewerking niet uitvoeren<br/><br/>Gebruiker kan geen toegang tot datalake store  <br/><br/>Gebruiker is niet geautoriseerd met data lake analytics | De Service-Principal of een certificaat dat is opgegeven heeft geen toegang tot het bestand in de opslag | Zorg ervoor dat de Service-Principal of certificaten die ze voor ADLA-taken bieden toegang heeft tot zowel de ADLA-account als de standaardopslag voor het ADLS voor deze vanuit de hoofdmap. |
| 2711                 | Kan het 'Azure Data Lake Store-bestand of map niet vinden       | Het pad naar bestand USQL is gegaan of de gekoppelde service referenties hebt geen toegang | Controleer of het pad en de referenties die zijn opgegeven in de gekoppelde service |
| 2707                 | Het account van AzureDataLakeAnalytics kan niet worden omgezet. Controleer 'AccountName' en 'DataLakeAnalyticsUri'. | De ADLA-account in de gekoppelde service is onjuist                  | Controleer of dat het juiste account wordt opgegeven             |
| 2703                 | Fout-Id: E_CQO_SYSTEM_INTERNAL_ERROR of elke fout die de begint met ' fout-Id: " | Fout afkomstig is van ADLA                                    | Elke fout die lijkt op het voorbeeld betekent dat de taak is ingediend bij de ADLA en er in het script is mislukt. Het onderzoek moet worden uitgevoerd op ADLA. Als u de portal opent en navigeert u naar de ADLA-account, zoekt u de taak met behulp van ADF activiteit die wordt uitgevoerd van Id (niet de pijplijnuitvoering Id). De taak bevat meer informatie over de fout hebben en helpt om op te lossen. Als de oplossing is niet uitgeschakeld, moet u contact op met het ondersteuningsteam ADLA en geeft u de taak-URL, met inbegrip van de accountnaam van uw en de taak-ID. |
| 2709                 | We kan niet uw taak accepteren op dit moment. Het maximum aantal taken in de wachtrij voor uw account is 200. | Beperking van ADLA                                           | Verminder het aantal verzonden taken aan ADLA door ADF-triggers en gelijktijdigheidsinstellingen voor activiteiten te wijzigen of vergroot de limieten van ADLA |
| 2709                 | Deze taak is geweigerd omdat hiervoor 24 au's en dit account heeft een beheerder gedefinieerd beleid waarmee wordt voorkomen dat een taak met meer dan 5 AU's. | Beperking van ADLA                                           | Verminder het aantal verzonden taken aan ADLA door ADF-triggers en gelijktijdigheidsinstellingen voor activiteiten te wijzigen of vergroot de limieten van ADLA |



## <a name="azure-functions"></a>Azure Functions

| Foutcode | Foutbericht                           | Description                                                  | Mogelijke oplossing / aanbevolen actie                           |
| ------------ | --------------------------------------- | ------------------------------------------------------------ | ------------------------------------------------------------ |
| 3600         | Inhoud van de reactie is niet een geldig jobject is leeggemaakt | Dit betekent dat de Azure-functie die met de naam is niet een JSON-nettolading in het antwoord heeft geretourneerd. ADF Azure functieactiviteit biedt alleen ondersteuning voor de inhoud van de JSON-reactie. | Bijwerken van de Azure-functie als u wilt bijvoorbeeld een geldig JSON-nettolading retourneert een C# functie nieuwe (ActionResult) kunt terugkeren < OkObjectResult (' {`\"Id\":\"123\"`} "); |
| 3600         | Ongeldige HttpMethod: '..'.               | Dit betekent dat HTTP-methode die is opgegeven in de nettolading van de activiteit niet wordt ondersteund door Azure functie-activiteit. | De Http-methoden die worden ondersteund zijn:  <br/>PUT, POST,GET, DELETE, OPTIONS, HEAD, TRACE |



## <a name="custom-azure-batch"></a>Aangepast (Azure Batch)
| Foutcode | Foutbericht                                                | Description                                                  | Mogelijke oplossing / aanbevolen actie                           |
| ------------ | ------------------------------------------------------------ | ------------------------------------------------------------ | ------------------------------------------------------------ |
| 2500         | Klik op onverwachte uitzondering en de uitvoering is mislukt.             | Kan de opdracht of het programma een foutcode geretourneerd niet starten. | Controleer of het uitvoerbare bestand bestaat. Als wordt gestart, controleert u de stdout.txt en stderr.txt geüpload naar het opslagaccount. Het is raadzaam om te verzenden van grote hoeveelheden Logboeken in uw code voor het opsporen van fouten. |
| 2501         | Kan geen toegang tot de batch-account van gebruiker, Controleer de instellingen van de batch-account. | Onjuiste Batch toegang sleutel of groep naam opgegeven.            | U hoeft te controleren of de naam van de groep van toepassingen en toegangssleutel van Batch in de gekoppelde service. |
| 2502         | Kan geen gebruiker opslag met leestoegang-account controle opslagaccountinstellingen. | Onjuiste naam of toegang opslagaccountsleutel opgegeven.       | Controleer of opslag account naam en de toegangssleutel in de gekoppelde service moet. |
| 2504         | Bewerking heeft een ongeldige statuscode 'BadRequest' geretourneerd als     | Te veel bestanden in folderPath als de aangepaste activiteit.  (Totale grootte van resourceFiles mag niet meer dan 32.768 tekens.) | Verwijder onnodige bestanden of ze zip en toevoegen van een opdracht unzip om op te halen, bijvoorbeeld: powershell.exe - nologo - noprofile-opdracht ' & {Add-Type - A 'System.IO.Compression.FileSystem';   [IO.Compression.ZipFile]::ExtractToDirectory ($zipFile, $folder); }" ;  $folder\yourProgram.exe |
| 2505         | Kan geen handtekening voor gedeelde toegang niet maken, tenzij de Accountsleutel referenties worden gebruikt. | Aangepaste activiteiten ondersteunen alleen de opslagaccounts die gebruikmaken van een toegangssleutel. | Raadpleeg de beschrijving                                            |
| 2507         | Pad naar de map bestaat niet of is leeg:...            | Er worden geen bestanden in de storage-account in het opgegeven pad.       | Het mappad mag de uitvoerbare bestanden die u wilt uitvoeren. |
| 2508         | Er zijn dubbele bestanden in resource-map.               | Er zijn meerdere bestanden met dezelfde naam in de verschillende submappen van folderPath. | Aangepaste activiteiten afvlakken mapstructuur onder folderPath.  Als de mapstructuur moet worden bewaard, de bestanden zipt en deze uit te pakken op Azure Batch met een opdracht unzip, bijvoorbeeld: powershell.exe - nologo - noprofile-opdracht ' & {Add-Type - A 'System.IO.Compression.FileSystem';   [IO.Compression.ZipFile]::ExtractToDirectory ($zipFile, $folder); }" ;   $folder\yourProgram.exe |
| 2509         | Batch-url... is ongeldig, het moet zich binnen de Uri-notatie.         | Batch-URL's moeten zijn vergelijkbaar met https:\//mybatchaccount.eastus.batch.azure.com | Raadpleeg de beschrijving                                            |
| 2510         | Er is een fout opgetreden tijdens het verzenden van de aanvraag.               | Batch de URL is ongeldig                                         | Controleer of de URL van de batch.                                            |

## <a name="hdinsight-spark-hive-mapreduce-pig-hadoop-streaming"></a>HDInsight (Spark, Hive, MapReduce, Pig, Hadoop Streaming)

| Foutcode | Foutbericht                                                | Description                                                  | Mogelijke oplossing / aanbevolen actie                           |
| ------------ | ------------------------------------------------------------ | ------------------------------------------------------------ | ------------------------------------------------------------ |
| 2300,   2310 | Hadoop-taken verzenden is mislukt. Fout: De externe naam kan niet worden omgezet. <br/><br/>Het cluster is niet gevonden. | De opgegeven cluster-URI ongeldig is                              | Controleer of het cluster niet is verwijderd, en de opgegeven URI juist is. U kunt de URI in elke browser openen en ziet u de Ambari UI. Als het cluster zich in een vNet, klikt u vervolgens de URI moet de persoonlijke URI en moet u proberen om deze te openen van een virtuele machine die deel uitmaakt van hetzelfde vNet. Meer informatie voor [Virtueelnetwerk in HDInsight](https://docs.microsoft.com/azure/hdinsight/hdinsight-extend-hadoop-virtual-network#directly-connect-to-apache-hadoop-services). |
| 2300         | Hadoop-taken verzenden is mislukt. Job: …, Cluster: …/. Fout: Een taak is geannuleerd. | Er is een time-out opgetreden voor het indienen van de taak.                         | Dit wordt mogelijk probleem met de netwerkverbinding algemene HDInsight of probleem met de netwerkverbinding. Controleer eerst of HDInsight Ambari-Webinterface is beschikbaar via een browser en uw referenties nog steeds geldig zijn. Zorg ervoor dat u dit doen vanuit de virtuele machine/machine waarop de zelf-hostende IR is geïnstalleerd als zelf-hostende IR Probeer de taak van ADF opnieuw in te dienen. Als het nog steeds mislukt, moet u contact op met ADF-team voor ondersteuning. |
| 2300         | Niet-geautoriseerde:   Ambari-gebruikersnaam of wachtwoord is onjuist  <br/><br/>Niet-geautoriseerde:   Gebruikerbeheerder is vergrendeld in de Ambari   <br/><br/>403 -   Forbidden: De toegang is geweigerd | De opgegeven referenties voor de HDInsight onjuist zijn of zijn verlopen | Corrigeer deze en implementeer de gekoppelde service opnieuw. Zorg ervoor dat de referenties werken op HDInsight eerst door het openen van het cluster-URI op elke browser en probeert aan te melden. Als ze niet werkt, kunt u ze kunt herstellen vanuit Azure Portal. |
| 2300,   2310 | 502 - webserver heeft een ongeldig antwoord ontvangen terwijl deze fungeerde als gateway of proxy       <br/>Ongeldige gateway | Fout afkomstig is van HDInsight                               | Deze fout is afkomstig van HDInsight-cluster. Raadpleeg [HDInsight probleemoplosser](https://hdinsight.github.io/ambari/ambari-ui-502-error.html) met veelvoorkomende fouten.    <br/>Voor Spark-clusters deze mogelijk ook veroorzaakt vanwege [dit](https://hdinsight.github.io/spark/spark-thriftserver-errors.html). <br/><br/>[Aanvullende koppeling](https://docs.microsoft.com/azure/application-gateway/application-gateway-troubleshooting-502) |
| 2300         | Hadoop-taken verzenden is mislukt. Taak:..., Cluster:... Fout: {\"fout\":\"kan niet aan de aanvraag van de taak indienen als templeton-service bezig met te veel aanvragen voor indienings-taak is. Wacht enige tijd voordat u de bewerking opnieuw uitvoert. Raadpleeg de config-templeton.parallellism.job.submit gelijktijdige aanvragen te configureren. \  <br/><br/>Hadoop-taken verzenden is mislukt. Taak: 161da5d4-6fa8-4ef4-a240-6b6428c5ae2f, Cluster: https:\//abc-analytics-prod-hdi-hd-trax-prod01.azurehdinsight.net/.   Fout: {\"fout\":\"java.io.IOException: org.apache.hadoop.yarn.exceptions.YarnException: Failed to submit   application_1561147195099_3730 to YARN :   org.apache.hadoop.security.AccessControlException: Wachtrij root.joblauncher al heeft 500 toepassingen, kan niet accepteert verzending van toepassing: application_1561147195099_3730\ | Te veel taken worden op hetzelfde moment worden verzonden naar HDInsight | Houd rekening met het aantal gelijktijdige taken worden verzonden naar de HDI te beperken. Raadpleeg ADF activiteit gelijktijdigheid als ze zijn wordt verzonden door de dezelfde activiteit. De triggers wijzigen zodat de gelijktijdige pijplijnuitvoeringen na verloop van tijd zijn verspreid. Ook verwijzen naar HDInsight docs om de "templeton.parallellism.job.submit" aanpassen zoals fout al aangeeft. |
| 2303,   2347 | Hadoop-taak is mislukt met afsluitcode '5'. Zie 'wasbs://adfjobs@adftrialrun.blob.core.windows.net/StreamingJobs/da4afc6d-7836-444e-bbd5-635fce315997/18_06_2019_05_36_05_050/stderr' voor meer informatie.  <br/><br/>Het uitvoeren van hive is mislukt met foutcode 'UserErrorHiveOdbcCommandExecutionFailure'.   Zie 'wasbs://adfjobs@eclsupplychainblobd.blob.core.windows.net/HiveQueryJobs/16439742-edd5-4efe-adf6-9b8ff5770beb/18_06_2019_07_37_50_477/Status/hive.out' voor meer informatie | De taak is verzonden naar HDInsight en is mislukt op HDInsight | De taak is verzonden naar HDInsight. Deze op het cluster is mislukt. . Open de taak op HDInsight Ambari-gebruikersinterface en open de logboeken er, of open het bestand uit de opslag als de fout bericht punten uit. De details van de fout zich in dat bestand. |
| 2328         | Er is een interne serverfout opgetreden tijdens het verwerken van de aanvraag. Geef op de aanvraag opnieuw proberen of contact opnemen met ondersteuning | Wordt uitgevoerd op HDInsight op aanvraag.                              | Deze fout is afkomstig van HDInsight-service wanneer het inrichten van HDInsight is mislukt. Neem contact op met het HDInsight-team en geef deze de naam van het op aanvraag. |
| 2310         | java.lang.NullPointerException                               | Er is een fout opgetreden bij het indienen van de taak voor het Spark-cluster      | Deze uitzondering is afkomstig van HDInsight en het werkelijke probleem is verborgen.   . Neem contact op met het HDInsight-team voor ondersteuning en geef ze clusternaam en de activiteit die wordt uitgevoerd van tijdsbereik. |
|              | Alle andere fouten                                             |                                                              | Raadpleeg [HDInsight probleemoplosser](../hdinsight/hdinsight-troubleshoot-guide.md) en [Veelgestelde vragen over HDInsight](https://hdinsight.github.io/) |



## <a name="web-activity"></a>Web Activity

| Foutcode | Foutbericht                                                | Description                                                  | Mogelijke oplossing / aanbevolen actie                           |
| ------------ | ------------------------------------------------------------ | ------------------------------------------------------------ | ------------------------------------------------------------ |
| 2108         | Ongeldige HttpMethod: '..'.                                    | Dit betekent dat HTTP-methode die is opgegeven in de nettolading van de activiteit niet wordt ondersteund door Web-activiteit. | De Http-methoden die worden ondersteund zijn: <br/>PUT, POST, GET, DELETE |
| 2108         | Ongeldige serverfout 500                                     | Interne fout op het eindpunt                               | Controleer de functionaliteit van de URL (met Fiddler/Postman): [Fiddler gebruiken om een HTTP-sessie te maken](#how-to-use-fiddler-to-create-an-http-session-of-the-monitored-web-application) |
| 2108         | Niet-geautoriseerde 401                                             | Ontbrekende geldig authenticatie op aanvraag                      | Geef geldige verificatiemethode (token verlopen).   <br/><br/>Controleer de functionaliteit van de URL (met Fiddler/Postman): [Fiddler gebruiken om een HTTP-sessie te maken](#how-to-use-fiddler-to-create-an-http-session-of-the-monitored-web-application) |
| 2108         | Niet-toegestane 403                                                | Ontbrekende vereiste machtigingen                                 | Controleer de gebruikersmachtigingen voor de gebruikte bron.   <br/><br/>Controleer de functionaliteit van de URL (met Fiddler/Postman): [Fiddler gebruiken om een HTTP-sessie te maken](#how-to-use-fiddler-to-create-an-http-session-of-the-monitored-web-application) |
| 2108         | 400-Ongeldige aanvraag                                              | Ongeldige Http-aanvraag                                         | Controleer de URL, een werkwoord en een hoofdtekst van de aanvraag.   <br/><br/>Gebruik Fiddler/Postman voor het valideren van de aanvraag: [Fiddler gebruiken om een HTTP-sessie te maken](#how-to-use-fiddler-to-create-an-http-session-of-the-monitored-web-application) |
| 2108         | 404 niet gevonden                                                | Resource is niet gevonden                                       | Gebruik Fiddler/Postman voor het valideren van de aanvraag: [Fiddler gebruiken om een HTTP-sessie te maken](#how-to-use-fiddler-to-create-an-http-session-of-the-monitored-web-application) |
| 2108         | Service is niet beschikbaar                                          | Service is niet beschikbaar                                       | Gebruik Fiddler/Postman voor het valideren van de aanvraag: [Fiddler gebruiken om een HTTP-sessie te maken](#how-to-use-fiddler-to-create-an-http-session-of-the-monitored-web-application) |
| 2108         | Niet-ondersteund mediatype                                       | Niet-overeenkomende Content-Type met de hoofdtekst van de activiteit Web           | Geef de juiste Content-Type dat overeenkomt met de indeling van de nettolading gebruik Fiddler/Postman voor het valideren van de aanvraag: [Fiddler gebruiken om een HTTP-sessie te maken](#how-to-use-fiddler-to-create-an-http-session-of-the-monitored-web-application) |
| 2108         | De resource die u zoekt voor is verwijderd, de naam ervan gewijzigd of is tijdelijk niet beschikbaar. | De resource is niet beschikbaar                                | Fiddler/Postman gebruiken om te controleren of het eindpunt: [Fiddler gebruiken om een HTTP-sessie te maken](#how-to-use-fiddler-to-create-an-http-session-of-the-monitored-web-application) |
| 2108         | De pagina die u zoekt kan niet worden weergegeven omdat een ongeldige methode (HTTP-woord) wordt gebruikt. | Onjuiste webmethode-activiteit is opgegeven in de aanvraag   | Fiddler/Postman gebruiken om te controleren of het eindpunt: [Fiddler gebruiken om een HTTP-sessie te maken](#how-to-use-fiddler-to-create-an-http-session-of-the-monitored-web-application) |
| 2108         | invalid_payload                                              | De hoofdtekst voor webactiviteit is onjuist                       | Fiddler/Postman gebruiken om te controleren of het eindpunt: [Fiddler gebruiken om een HTTP-sessie te maken](#how-to-use-fiddler-to-create-an-http-session-of-the-monitored-web-application) |

#### <a name="how-to-use-fiddler-to-create-an-http-session-of-the-monitored-web-application"></a>Fiddler gebruiken om een HTTP-sessie van de bewaakte webtoepassing te maken

1. Download en installeer [Fiddler](https://www.telerik.com/download/fiddler)

2. Als uw webtoepassing maakt gebruik van HTTPS:

   1. Open Fiddler

   2. Ga naar **Tools > Fiddler opties** en selecteer in de schermafbeelding hieronder. 

      ![fiddler-opties](media/data-factory-troubleshoot-guide/fiddler-options.png)

3. Als uw toepassing gebruikmaakt van SSL-certificaten, moet u ook het Fiddler-certificaat toevoegen aan uw apparaat.

4. Als u wilt de Fiddler-certificaat toevoegen aan uw apparaat, gaat u naar **extra** > **Fiddler opties** > **HTTPS**  >   **Acties** > **basiscertificaat exporteren naar Desktop** om de Fiddler-certificaat te verkrijgen.

5. Uitschakelen zodat de cache van de browser kan worden verwijderd om te starten van een nieuwe tracering vast te leggen.

6. 1. Ga naar **bestand** > **verkeer vastleggen** of druk op **F12**.
   2. Schakel de cache van uw browser zodat alle items in de cache worden verwijderd en opnieuw worden gedownload moeten.

7. Aanvraag maken: 

8. 1. Klik op het tabblad Composer
   2. De Http-methode en de URL instellen
   3. Headers toevoegen en een corpus aanvragen indien nodig
   4. Klik op uitvoeren

9. Start het vastleggen van verkeer en voltooi de problematische transactie op de pagina.

10. Als voltooid, gaat u naar **bestand** > **opslaan** > **alle sessies**.

Meer informatie over Fiddler [hier](https://docs.telerik.com/fiddler/Configure-Fiddler/Tasks/ConfigureFiddler)

## <a name="next-steps"></a>Volgende stappen

Voor meer hulp bij het vinden van oplossing voor uw probleem volgen hier enkele andere bronnen die u kunt proberen.

*  [Blogs](https://azure.microsoft.com/blog/tag/azure-data-factory/)
*  [Functieverzoeken](https://feedback.azure.com/forums/270578-data-factory)
*  [Video's](https://azure.microsoft.com/resources/videos/index/?sort=newest&services=data-factory)
*  [MSDN-forum](https://social.msdn.microsoft.com/Forums/home?sort=relevancedesc&brandIgnore=True&searchTerm=data+factory)
*  [Stack Overflow](https://stackoverflow.com/questions/tagged/azure-data-factory)
*  [Twitter](https://twitter.com/hashtag/DataFactory)



