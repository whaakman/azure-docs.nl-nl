---
title: Visualiseer gegevens uit Azure Data Explorer in Grafana
description: In deze instructies leert u hoe u Azure Data Explorer instellen als een gegevensbron voor Grafana en visualiseren van gegevens uit een voorbeeld-cluster.
services: data-explorer
author: orspod
ms.author: v-orspod
ms.reviewer: mblythe
ms.service: data-explorer
ms.topic: conceptual
ms.date: 12/05/2018
ms.openlocfilehash: 5a9684605de9af1cd9006810d595ae846db01661
ms.sourcegitcommit: 5d837a7557363424e0183d5f04dcb23a8ff966bb
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/06/2018
ms.locfileid: "52975156"
---
# <a name="visualize-data-from-azure-data-explorer-in-grafana"></a>Visualiseer gegevens uit Azure Data Explorer in Grafana

Grafana is een platform voor streaminganalyse waarmee u query's uitvoeren en gegevens visualiseren, klikt u vervolgens maken en delen van dashboards op basis van uw visualisaties. Grafana geeft een Azure Data Explorer *invoegtoepassing*, waarmee u kunt verbinding maken met en Visualiseer gegevens uit Azure Data Explorer. In dit artikel leert u hoe u Azure Data Explorer instellen als een gegevensbron voor Grafana en visualiseren van gegevens uit een voorbeeld-cluster.

## <a name="prerequisites"></a>Vereisten

U moet deze het voltooien van de volgende:

* [Grafana versie 5.3.0 of later](http://docs.grafana.org/installation/) voor uw besturingssysteem

* De [Azure Data Explorer-invoegtoepassing](https://grafana.com/plugins/grafana-azure-data-explorer-datasource/installation) voor Grafana

* Een cluster met de voorbeeldgegevens StormEvents. Zie voor meer informatie, [Quickstart: maken van een cluster van Azure Data Explorer en -database](create-cluster-database-portal.md) en [voorbeeldgegevens worden opgenomen in Azure Data Explorer](ingest-sample-data.md).

    [!INCLUDE [data-explorer-storm-events](../../includes/data-explorer-storm-events.md)]

## <a name="configure-the-data-source"></a>De gegevensbron configureren

U de volgende stappen uitvoeren om Azure Data Explorer configureren als een gegevensbron voor Grafana. We deze stappen in deze sectie nader aan bod:

1. Een Azure Active Directory (Azure AD) service-principal maken. De service-principal wordt gebruikt door het Grafana voor toegang tot de service Azure Data Explorer.

1. Toevoegen van de Azure AD-service-principal naar het *viewers* rol in de database van Azure Data Explorer.

1. Geef de verbindingseigenschappen Grafana op basis van gegevens uit de Azure AD service-principal en test de verbinding.

### <a name="create-a-service-principal"></a>Een service-principal maken

U kunt de service-principal maken in de [Azure-portal](#azure-portal) of met behulp van de [Azure CLI](#azure-cli) opdrachtregelervaring. Ongeacht welke methode u, nadat u waarden ophalen voor vier eigenschappen van de verbinding die u in latere stappen gebruiken gaat is gemaakt.

#### <a name="azure-portal"></a>Azure Portal

1. Voor het maken van de service-principal, volg de instructies in de [documentatie voor Azure portal](/azure/active-directory/develop/howto-create-service-principal-portal).

    1. In de [de toepassing toewijzen aan een rol](/azure/active-directory/develop/howto-create-service-principal-portal#assign-the-application-to-a-role) sectie, een Roltype toewijzen **lezer** met uw Azure Data Explorer-cluster.

    1. In de [waarden ophalen voor het aanmelden](/azure/active-directory/develop/howto-create-service-principal-portal#get-values-for-signing-in) sectie, Kopieer de waarden van de drie eigenschappen in de stappen besproken: **map-ID** (tenant-ID), **toepassings-ID**, en  **Wachtwoord**.

1. Selecteer in de Azure portal, **abonnementen** Kopieer vervolgens de ID voor het abonnement waarin u de service-principal gemaakt.

    ![Abonnements-ID - portal](media/grafana/subscription-id-portal.png)

#### <a name="azure-cli"></a>Azure-CLI

1. Een service-principal maken. Stel een omvang die geschikt is en een type van de rol van `reader`.

    ```azurecli
    az ad sp create-for-rbac --name "https://{UrlToYourGrafana}:{PortNumber}" --role "reader" \
                             --scopes /subscriptions/{SubID}/resourceGroups/{ResourceGroupName}
    ```

    Zie voor meer informatie, [een Azure service-principal maken met Azure CLI](/cli/azure/create-an-azure-service-principal-azure-cli).

1. De opdracht retourneert een resultatenset als volgt uit. Kopieer de waarden van de drie eigenschappen: **appID**, **wachtwoord**, en **tenant**.

    ```json
    {
      "appId": "XXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXXXXX",
      "displayName": "{UrlToYourGrafana}:{PortNumber}",
      "name": "https://{UrlToYourGrafana}:{PortNumber}",
      "password": "XXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXXXXX",
      "tenant": "XXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXXXXX"
    }
    ```

1. Haal een lijst van uw abonnementen.

    ```azurecli
    az account list --output table
    ```

    Kopieer de juiste abonnements-ID.

    ![Abonnements-ID - CLI](media/grafana/subscription-id-cli.png)

### <a name="add-the-service-principal-to-the-viewers-role"></a>De service-principal toevoegen aan de rol viewers

Nu dat u een service-principal hebt, u toevoegen aan de *viewers* rol in de database van Azure Data Explorer. U kunt deze taak onder uitvoeren **machtigingen** in Azure portal of onder **Query** met behulp van een management-opdracht.

#### <a name="azure-portal---permissions"></a>Azure portal - machtigingen

1. In de Azure-portal, gaat u naar uw Azure Data Explorer-cluster.

1. In de **overzicht** sectie, selecteer de database met de voorbeeldgegevens StormEvents.

    ![database selecteren](media/grafana/select-database.png)

1. Selecteer **machtigingen** vervolgens **toevoegen**.

    ![Machtigingen voor database](media/grafana/database-permissions.png)

1. Onder **databasemachtigingen toevoegen**, selecteer de **Viewer** rol vervolgens **Selecteer principals**.

    ![Databasemachtigingen toevoegen](media/grafana/add-permission.png)

1. Zoek naar de service-principal die u hebt gemaakt (in het voorbeeld wordt de principal **mb grafana**). Selecteer vervolgens de principal **Selecteer**.

    ![Machtigingen in de Azure-portal beheren](media/grafana/new-principals.png)

1. Selecteer **Opslaan**.

    ![Machtigingen in de Azure-portal beheren](media/grafana/save-permission.png)

#### <a name="management-command---query"></a>Management command - Query

1. In de Azure-portal, gaat u naar uw Azure Data Explorer-cluster en selecteer **Query**.

    ![Query’s uitvoeren](media/grafana/query.png)

1. Voer de volgende opdracht in het queryvenster. Gebruik de toepassings-ID en tenant-ID van de Azure portal of de CLI.

    ```kusto
    .add database {TestDatabase} viewers ('aadapp={ApplicationID};{TenantID}')
    ```

    De opdracht retourneert een resultatenset als volgt uit. In dit voorbeeld wordt de eerste rij is voor een bestaande gebruiker in de database en de tweede rij is voor de service-principal die zojuist is toegevoegd.

    ![Resultatenset](media/grafana/result-set.png)

### <a name="specify-properties-and-test-the-connection"></a>Geef eigenschappen op en test de verbinding

Met de service-principal die is toegewezen aan de *viewers* rol, nu u eigenschappen opgeven in uw exemplaar van Grafana, en test de verbinding met Azure Data Explorer.

1. Grafana, in het menu links, selecteer in het tandwielpictogram vervolgens **gegevensbronnen**.

    ![Gegevensbronnen](media/grafana/data-sources.png)

1. Selecteer **gegevensbron toevoegen**.

1. Op de **gegevensbronnen / nieuwe** pagina, voer een naam voor de gegevensbron en selecteer het type **Azure Data Explorer Datasource**.

    ![De naam van verbinding en het type](media/grafana/connection-name-type.png)

1. Voer de naam van het cluster in het formulier https://{ClusterName}. {Region}. kusto.windows.net. Voer de overige waarden uit de Azure portal of de CLI. Zie de tabel onder de volgende afbeelding voor een-toewijzing.

    ![Verbindingseigenschappen](media/grafana/connection-properties.png)

    | Grafana-UI | Azure Portal | Azure-CLI |
    | --- | --- | --- |
    | Abonnements-id | ABONNEMENTS-ID | SubscriptionId |
    | Tenant-id | Map-id | tenant |
    | Client-id | Toepassings-id | appId |
    | Clientgeheim | Wachtwoord | wachtwoord |
    | | | |

1. Selecteer **opslaan en testen**.

    Als de test geslaagd is, gaat u naar de volgende sectie. Als u problemen tegenkomt, controleert u de waarden die u hebt opgegeven in de Grafana en Controleer vorige stappen.

## <a name="visualize-data"></a>Gegevens visualiseren

Nu u klaar bent met het Azure Data Explorer configureren als een gegevensbron voor Grafana, is het tijd om gegevens te visualiseren. Laten we hier een eenvoudige voorbeeld zien, maar er is nog veel meer die kunt u doen. We raden [schrijven van query's voor Azure Data Explorer](write-queries.md) voor voorbeelden van andere query's op de voorbeeldgegevensset uit te voeren.

1. Grafana, in het menu links, selecteer in het plus-pictogram vervolgens **Dashboard**.

    ![dashboard maken](media/grafana/create-dashboard.png)

1. Onder de **toevoegen** tabblad **Graph**.

    ![Grafiek toevoegen](media/grafana/add-graph.png)

1. Selecteer in het deelvenster graph **titel** vervolgens **bewerken**.

    ![Deelvenster bewerken](media/grafana/edit-panel.png)

1. Selecteer aan de onderkant van het paneel **gegevensbron** selecteert u de gegevensbron die u hebt geconfigureerd.

    ![Gegevensbron selecteren](media/grafana/select-data-source.png)

1. Kopieer in het Querydeelvenster in de volgende query uit en selecteer vervolgens **uitvoeren**. De query duurbuckets het aantal gebeurtenissen per dag voor de voorbeeldgegevensset.

    ```kusto
    StormEvents
    | summarize event_count=count() by bin(StartTime, 1d)
    ```

    ![Query uitvoeren](media/grafana/run-query.png)

1. De grafiek niet geen resultaten worden weergegeven omdat deze standaard afgestemd op de gegevens van de afgelopen zes uur. Selecteer op het bovenste menu **afgelopen 6 uur**.

    ![Afgelopen zes uur](media/grafana/last-six-hours.png)

1. Een aangepaste bereik die betrekking heeft op 2007, het jaar dat is opgenomen in onze verzameling voorbeeldgegevens StormEvents opgeven. Selecteer **Toepassen**.

    ![Aangepast datumbereik](media/grafana/custom-date-range.png)

    De grafiek toont nu de gegevens van 2007, gerangschikte per dag.

    ![Voltooide grafiek](media/grafana/finished-graph.png)

1. Selecteer in het bovenste menu de opslaan pictogram: ![Pictogram opslaan](media/grafana/save-icon.png).

## <a name="next-steps"></a>Volgende stappen

[Query's schrijven voor Azure Data Explorer](write-queries.md)

[Zelfstudie: Gegevens uit Azure Data Explorer in Power BI visualiseren](visualize-power-bi.md)