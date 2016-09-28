<properties
   pageTitle="Aan de slag met Azure Batch CLI | Microsoft Azure"
   description="Een korte inleiding in de Batch-opdrachten in Azure CLI voor het beheren van Azure Batch-serviceresources"
   services="batch"
   documentationCenter=""
   authors="mmacy"
   manager="timlt"
   editor=""/>

<tags
   ms.service="batch"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="multiple"
   ms.workload="big-compute"
   ms.date="09/06/2016"
   ms.author="marsma"/>


# Aan de slag met Azure Batch CLI

Met de platformonafhankelijke Azure-opdrachtregelinterface (Azure CLI) kunt u Batch-accounts en -resources beheren, zoals groepen en taken in Linux-, Mac- en Windows-opdrachtshells. Met de Azure-opdrachtregelinterface kunt u veel dezelfde taken uitvoeren die u ook uitvoert met de Batch-API's, Azure Portal en Batch Powershell-cmdlets, en scripts voor deze taken uitvoeren.

Dit artikel is gebaseerd op Azure CLI-versie 0.10.3.

## Vereisten

* [Azure CLI installeren](../xplat-cli-install.md)

* [Koppel de Azure-opdrachtregelinterface aan uw Azure-abonnement](../xplat-cli-connect.md)

* Overschakelen naar de **Resource Manager-modus**: `azure config mode arm`

>[AZURE.TIP] Het wordt aangeraden de Azure CLI-installatie regelmatig bij te werken om te profiteren van service-updates en verbeteringen.

## Opdracht Help

U kunt voor elke opdracht in de Azure-opdrachtregelinterface Help-tekst weergeven door `-h` als enige optie toe te voegen na de opdracht. Bijvoorbeeld:

* Als u hulp nodig hebt bij de opdracht `azure`, voert u in: `azure -h`
* Voor een lijst met alle Batch-opdrachten in de opdrachtregelinterface, gebruikt u: `azure batch -h`
* Als u hulp nodig hebt bij het maken van een Batch-account, voert u in: `azure batch account create -h`

Gebruik bij twijfel de opdrachtregeloptie `-h` voor hulp bij een willekeurige Azure CLI-opdracht.

## Batch-account maken

Gebruik:

    azure batch account create [options] <name>

Voorbeeld:

    azure batch account create --location "West US"  --resource-group "resgroup001" "batchaccount001"

Maakt een nieuw Batch-account met de opgegeven parameters. U moet minstens één locatie, de resourcegroep en de accountnaam opgeven. Als u nog geen resourcegroep hebt, maakt u er een door `azure group create` uit te voeren en een van de Azure-regio‘s op te geven (zoals VS - West) voor de `--location`-optie. Bijvoorbeeld:

    azure group create --name "resgroup001" --location "West US"

> [AZURE.NOTE] De naam van het Batch-account moet uniek zijn in de Azure-regio waarin het account wordt gemaakt. Deze naam mag alleen kleine letters en cijfers bevatten, en moet tussen de 3 en 24 tekens lang zijn. U kunt in namen van Batch-accounts geen speciale tekens gebruiken, zoals `-` of `_`.

### Gekoppeld opslagaccount (autostorage)

U kunt (optioneel) een opslagaccount voor **Algemeen gebruik** koppelen aan uw Batch-account terwijl u dit account maakt. De [toepassingspakketten](batch-application-packages.md)-functie van Batch maakt gebruik van blob-opslag in een gekoppeld opslagaccount voor Algemeen gebruik. Dit geldt ook voor de [Batch-bestandsconventies-.NET](batch-task-output.md)-bibliotheek. Deze optionele functies helpen u bij het implementeren van de toepassingen die uw Batch-taken uitvoeren en het behouden van de gegevens die deze toepassingen produceren.

Als een bestaand Azure Storage-account wilt koppelen aan een Batch-account terwijl u dit account maakt, geeft u de `--autostorage-account-id`-optie op. Voor deze optie is de volledig gekwalificeerde resource-id van het opslagaccount vereist.

Geef eerst de details van het opslagaccount weer:

    azure storage account show --resource-group "resgroup001" "storageaccount001"

Gebruik vervolgens de **URL**-waarde voor de `--autostorage-account-id`-optie. De URL-waarde begint met /abonnementen/ en bevat de abonnement-id en het resourcepad naar het opslagaccount:

    azure batch account create --location "West US"  --resource-group "resgroup001" --autostorage-account-id "/subscriptions/8ffffff8-4444-4444-bfbf-8ffffff84444/resourceGroups/resgroup001/providers/Microsoft.Storage/storageAccounts/storageaccount001" "batchaccount001"

## Batch-account verwijderen

Gebruik:

    azure batch account delete [options] <name>

Voorbeeld:

    azure batch account delete --resource-group "resgroup001" "batchaccount001"

Verwijdert het opgegeven Batch-account. Als u hierom wordt gevraagd, bevestigt u dat u het account wilt verwijderen (het kan enige tijd duren voordat het account is verwijderd).

## Toegangssleutels voor accounts beheren

U hebt een toegangssleutel nodig om [resources te maken en aan te passen](#create-and-modify-batch-resources) in het Batch-account.

### Lijst met toegangssleutels

Gebruik:

    azure batch account keys list [options] <name>

Voorbeeld:

    azure batch account keys list --resource-group "resgroup001" "batchaccount001"

Vermeldt de accountsleutels voor het opgegeven Batch-account.

### Een nieuwe toegangssleutel genereren

Gebruik:

    azure batch account keys renew [options] --<primary|secondary> <name>

Voorbeeld:

    azure batch account keys renew --resource-group "resgroup001" --primary "batchaccount001"

Genereert opnieuw de opgegeven accountsleutel voor het opgegeven Batch-account.

## Batch-resources maken en wijzigen

U kunt de Azure-opdrachtregelinterface gebruiken om Batch-resources zoals groepen, rekenknooppunten en taken te maken, te lezen, bij te werken en te verwijderen (CRUD). Voor deze CRUD-bewerkingen moeten de Batch-accountnaam, de toegangssleutel en het eindpunt worden opgegeven. U kunt deze opgeven met de opties `-a`, `-k` en `-u`, of u kunt [omgevingsvariabelen](#credential-environment-variables) instellen die automatisch worden gebruikt voor de opdrachtregelinterface (indien ingevuld).

### Omgevingsvariabelenreferentie

U kunt de omgevingsvariabelen `AZURE_BATCH_ACCOUNT`, `AZURE_BATCH_ACCESS_KEY` en `AZURE_BATCH_ENDPOINT` instellen in plaats van de opties `-a`, `-k` en `-u` op te geven op de opdrachtregel voor elke opdracht die u uitvoert. De Batch-opdrachtregelinterface gebruikt deze variabelen (indien ingesteld) zodat u de opties `-a`, `-k` en `-u` kunt weglaten. In de rest van dit artikel wordt ervan uitgegaan dat u deze omgevingsvariabelen gebruikt.

>[AZURE.TIP] Vermeld de sleutels met `azure batch account keys list` en geef het eindpunt van het account weer met `azure batch account show`.

### JSON-bestanden

Als u Batch-resources maakt, zoals groepen en taken, kunt u een JSON-bestand opgeven dat de configuratie van de nieuwe resource bevat, in plaats van de bijbehorende parameters op te geven als opdrachtregelopties. Bijvoorbeeld:

`azure batch pool create my_batch_pool.json`

Hoewel u veel bewerkingen voor het maken van resources kunt uitvoeren met alleen opdrachtregelopties, is voor sommige functies een bestand in de JSON-indeling met de resourcedetails vereist. U moet bijvoorbeeld een JSON-bestand gebruiken als u resourcebestanden wilt opgeven voor een starttaak.

Raadpleeg de documentatie op MSDN [Batch REST-API-naslaginformatie][rest_api] om de vereiste JSON te zoeken voor het maken van een resource. Elk onderwerp *Resourcetype* toevoegen bevat een voorbeeld-JSON voor het maken van een resource. Dit voorbeeld kunt u gebruiken als sjabloon voor de JSON-bestanden. De JSON voor het maken van groepen vindt u bijvoorbeeld in [Een groep toevoegen aan een account][rest_add_pool].

>[AZURE.NOTE] Als u een JSON-bestand opgeeft bij het maken van een resource, worden alle andere parameters die u opgeeft op de opdrachtregel, genegeerd.

## Een groep maken

Gebruik:

    azure batch pool create [options] [json-file]

Voorbeeld (virtuele-machineconfiguratie):

    azure batch pool create --id "pool001" --target-dedicated 1 --vm-size "STANDARD_A1" --image-publisher "Canonical" --image-offer "UbuntuServer" --image-sku "14.04.2-LTS" --node-agent-id "batch.node.ubuntu 14.04"

Voorbeeld (Cloud Services-configuratie):

    azure batch pool create --id "pool002" --target-dedicated 1 --vm-size "small" --os-family "4"

Maakt een groep rekenknooppunten in de Batch-service.

Zoals vermeld in het [Batch feature overview](batch-api-basics.md#pool) (Overzicht van Batch-functies) beschikt u over twee opties wanneer u een besturingssysteem selecteert voor de knooppunten in de groep: **Virtuele-machineconfiguratie** en **Cloud Services-configuratie**. Gebruik de `--image-*`-opties om virtuele-machineconfiguratiegroepen te maken en gebruik `--os-family` om Cloud Services-configuratiegroepen te maken. U kunt niet beide opties `--os-family` en `--image-*` opgeven.

U kunt [toepassingspakketten](batch-application-packages.md) voor groepen en de opdrachtregel voor een [starttaak](batch-api-basics.md#start-task) opgeven. Als u resourcebestanden wilt opgeven voor de starttaak, moet u echter in plaats hiervan een [JSON-bestand](#json-files) gebruiken.

Een groep verwijderen met:

    azure batch pool delete [pool-id]

>[AZURE.TIP] Controleer de [lijst met installatiekopieën van virtuele machines](batch-linux-nodes.md#list-of-virtual-machine-images) voor de juiste waarden voor de `--image-*`-opties.

## Een taak maken

Gebruik:

    azure batch job create [options] [json-file]

Voorbeeld:

    azure batch job create --id "job001" --pool-id "pool001"

Voegt een taak toe aan het Batch-account en geeft de groep op waarop de taken worden uitgevoerd.

Een taak verwijderen met:

    azure batch job delete [job-id]

## Lijst met groepen, taken en andere resources

Elk Batch-resourcetype ondersteunt een `list`-opdracht die een query uitvoert voor het Batch-account, en vermeldt een lijst met resources van dit type. U kunt bijvoorbeeld de groepen in uw account vermelden en de taken in een taak:

    azure batch pool list
    azure batch task list --job-id "job001"

### Resources efficiënt vermelden

Voor een snellere uitvoering van query‘s kunt u de componentopties **Selecteren**, **Filteren** en **Uitbreiden** opgeven voor `list`-bewerkingen. Gebruik deze opties om de hoeveelheid gegevens die worden geretourneerd via de Batch-service, te beperken. Omdat filteren aan de serverzijde plaatsvindt, worden alleen de gegevens geretourneerd waarin u bent geïnteresseerd. Gebruik deze componenten om bandbreedte (en dus tijd) te besparen tijdens het uitvoeren van lijstbewerkingen.

Deze query retourneert bijvoorbeeld alleen groepen waarvan de id begint met renderTask:

    azure batch task list --job-id "job001" --filter-clause "startswith(id, 'renderTask')"

De Batch-opdrachtregelinterface ondersteunt alle drie de componenten die worden ondersteund met de Batch-service:

* `--select-clause [select-clause]`  Een subset met eigenschappen retourneren voor elke entiteit
* `--filter-clause [filter-clause]`  Alleen entiteiten retourneren die overeenkomen met de opgegeven OData-expressie
* `--expand-clause [expand-clause]`  Verkrijg de entiteitgegevens in één onderliggende REST-aanroep. Het component Uitbreiden ondersteunt momenteel alleen de eigenschap `stats`.

Zie [Query the Azure Batch service efficiently](batch-efficient-list-queries.md) (Efficiënt query‘s uitvoeren voor de Azure Batch-service) voor details over de drie componenten en het hiermee uitvoeren van lijstquery‘s.

## Beheer van toepassingspakketten

Toepassingspakketten bieden een vereenvoudigde manier om toepassingen te implementeren op de rekenknooppunten in groepen. Met de Azure-opdrachtregelinterface kunt u toepassingspakketten uploaden, pakketversies beheren en pakketten verwijderen.

Een nieuwe toepassing maken en een pakketversie toevoegen:

Een toepassing **maken**:

    azure batch application create "resgroup001" "batchaccount001" "MyTaskApplication"

Een toepassingspakket **toevoegen**:

    azure batch application package create "resgroup001" "batchaccount001" "MyTaskApplication" "1.10-beta3" package001.zip

Het pakket **activeren**:

    azure batch application package activate "resgroup002" "azbatch002" "MyTaskApplication" "1.10-beta3" zip

### Een toepassingspakket implementeren

U kunt een of meer toepassingspakketten voor implementatie opgeven wanneer u een nieuwe groep maakt. Wanneer u een pakket opgeeft tijdens het maken van een groep, wordt dit pakket geïmplementeerd op elk knooppunt dat wordt gekoppeld aan de groep. Pakketten worden ook geïmplementeerd als een knooppunt opnieuw wordt gestart of teruggezet.

Met deze opdracht wordt een pakket opgegeven tijdens het maken van een groep en geïmplementeerd wanneer er een knooppunt wordt gekoppeld aan de nieuwe groep:

    azure batch pool create --id "pool001" --target-dedicated 1 --vm-size "small" --os-family "4" --app-package-ref "MyTaskApplication"

U kunt momenteel niet opgeven welke pakketversie moet worden geïmplementeerd met behulp van opdrachtregelopties. U moet eerst een standaardversie voor de toepassing instellen via de Azure-portal voordat u deze kunt toewijzen aan een groep. Zie hoe u een standaardversie kunt instellen in [Application deployment with Azure Batch application packages](batch-application-packages.md) (Toepassingsimplementatie met Azure Batch-toepassingspakketten). U kunt echter een standaardversie opgeven als u een [JSON-bestand](#json-files) gebruikt in plaats van opdrachtregelopties wanneer u een groep maakt.

>[AZURE.IMPORTANT] U moet [een Azure Storage-account koppelen](#linked-storage-account-autostorage) aan het Batch-account om toepassingspakketten te gebruiken.

## Tips voor probleemoplossing

Deze sectie is bedoeld om u resources te bieden die u kunt gebruiken bij het oplossen van problemen met Azure CLI. Hiermee worden wellicht niet alle problemen opgelost, maar het kan helpen bij het bepalen van een mogelijke oorzaak en u naar de juiste Help-resources verwijzen.

* Gebruik `-h` om **Help-tekst** weer te geven voor elke willekeurige CLI-opdracht

* Gebruik `-v` en `-vv` om **uitgebreide** opdrachtuitvoer weer te geven. `-vv` is nog uitgebreider en geeft de werkelijke REST-aanvragen en -antwoorden weer. Deze schakelopties zijn handig voor het weergeven van de volledige foutuitvoer.

* U kunt **opdrachtuitvoer weergeven als JSON** met de `--json`-optie. `azure batch pool show "pool001" --json` wordt bijvoorbeeld weergegeven als eigenschappen van pool001 in de JSON-indeling. Vervolgens kunt u deze uitvoer kopiëren en aanpassen om te worden gebruikt in een `--json-file` (zie [JSON-bestanden](#json-files) eerder in dit artikel).

* Het [Batch-forum op MSDN][batch_forum] is een geweldig hulpmiddel en wordt nauwkeurig bewaakt door Batch-teamleden. Plaats hier uw vraag als u problemen ondervindt of hulp nodig hebt met een bepaalde bewerking.

* Niet elk Batch-resourcebewerking wordt momenteel ondersteund in de Azure-opdrachtregelinterface. U kunt momenteel bijvoorbeeld niet de *versie* van een toepassingspakket voor een groep opgeven, maar alleen de pakket-id. In dergelijke gevallen moet u mogelijk een `--json-file` opgeven voor de opdracht in plaats van opdrachtregelopties te gebruiken. Zorg ervoor dat u beschikt over de meest recente CLI-versie om te kunnen profiteren van toekomstige verbeteringen.

## Volgende stappen

*  Zie [Application deployment with Azure Batch application packages](batch-application-packages.md) (Toepassingsimplementatie met Azure Batch-toepassingspakketten) voor informatie over het gebruik van deze functie voor het beheren en implementeren van de toepassingen die u uitvoert op Batch-rekenknooppunten.

* Zie [Query the Azure Batch service efficiently](batch-efficient-list-queries.md) (Efficiënt query's uitvoeren op de Azure Batch-service) voor meer informatie over het verminderen van het aantal items en het type informatie dat wordt geretourneerd voor query's naar Batch.

[batch_forum]: https://social.msdn.microsoft.com/forums/azure/en-US/home?forum=azurebatch
[github_readme]: https://github.com/Azure/azure-xplat-cli/blob/dev/README.md
[rest_api]: https://msdn.microsoft.com/library/azure/dn820158.aspx
[rest_add_pool]: https://msdn.microsoft.com/library/azure/dn820174.aspx


<!--HONumber=Sep16_HO3-->


