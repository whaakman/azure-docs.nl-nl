---
title: Host een statische website in Azure Storage
description: Informatie over het leveren van statische inhoud (HTML, CSS, JavaScript en afbeeldingsbestanden) rechtstreeks uit een container in een Azure Storage GPv2-account.
services: storage
author: normesta
ms.service: storage
ms.topic: article
ms.author: normesta
ms.date: 05/28/2019
ms.openlocfilehash: 61477767c59dd521e3f46db4445238a5a1ea759e
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "67071442"
---
# <a name="host-a-static-website-in-azure-storage"></a>Host een statische website in Azure Storage

U kunt leveren van statische inhoud (HTML, CSS, JavaScript en afbeeldingsbestanden) rechtstreeks uit een container in een Azure Storage GPv2-account. Zie voor meer informatie, [statische website hosting in Azure Storage](storage-blob-static-website.md).

Dit artikel laat u het inschakelen van statische website hosting met behulp van de Azure-portal, Azure CLI of PowerShell.

<a id="portal" />

## <a name="use-the-azure-portal"></a>Azure Portal gebruiken

Zie voor een stapsgewijze zelfstudie [zelfstudie: Een statische website hosten op Blob-opslag](https://docs.microsoft.com/azure/storage/blobs/storage-blob-static-website-host).

Nadat u de statische website hosting inschakelt, kunt u de pagina's van uw site via een browser weergeven met behulp van de openbare URL van de website.

<a id="portal-find-url" />

### <a name="find-the-website-url-by-using-the-azure-portal"></a>De website-URL zoeken met behulp van de Azure-portal

Selecteer in het deelvenster dat naast de overzichtspagina voor de account van uw opslagaccount verschijnt, **statische Website**. De URL van uw site wordt weergegeven in de **primaire eindpunt** veld.

![Azure Storage statische websites metrische gegevens over metrische gegevens](./media/storage-blob-static-website/storage-blob-static-website-url.png)

<a id="cli" />

## <a name="use-the-azure-cli"></a>Azure CLI gebruiken

U kunt statische website hosting met behulp van de [Azure-opdrachtregelinterface (CLI)](https://docs.microsoft.com/cli/azure/?view=azure-cli-latest).

1. Open eerst de [Azure Cloud Shell](https://docs.microsoft.com/azure/cloud-shell/overview?view=azure-cli-latest), of als u hebt [geïnstalleerd](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest) de Azure CLI lokaal, opent u een opdracht-consoletoepassing, zoals Windows PowerShell.

2. Vanuit het opdrachtvenster dat u hebt geopend, de uitbreiding voor de opslag-Preview-versie te installeren.

   ```azurecli-interactive
   az extension add --name storage-preview
   ```

3. Als uw identiteit gekoppeld aan meer dan één abonnement is, stel uw actieve abonnement-abonnement van het opslagaccount die als voor uw statische website host.

   ```azurecli-interactive
   az account set --subscription <subscription-id>
   ```

   Vervang de `<subscription-id>` aanduidingswaarde met de ID van uw abonnement.

4. Statische website hosting inschakelen.

   ```azurecli-interactive
   az storage blob service-properties update --account-name <storage-account-name> --static-website --404-document <error-document-name> --index-document <index-document-name>
   ```

   * Vervang de waarde van de tijdelijke plaatsaanduiding `<storage-account-name>` door de naam van uw opslagaccount.

   * Vervang de `<error-document-name>` tijdelijke aanduiding door de naam van het document fout die wordt weergegeven aan gebruikers wanneer een browser vraagt om een pagina op de site die niet bestaat.

   * Vervang de `<index-document-name>` tijdelijke aanduiding door de naam van de Indexdocument. Dit document wordt vaak 'index.html'.

5. Uploaden van objecten die u wilt de *$web* container uit een bronmap.

   > [!NOTE]
   > Als u Azure Cloud Shell gebruikt, controleert u of toe te voegen een `\` escape-teken met betrekking tot de `$web` container (bijvoorbeeld: `\$web`). Als u een lokale installatie van de Azure CLI gebruikt, hebt u wordt niet gebruikt u het escape-teken.

   In dit voorbeeld wordt ervan uitgegaan dat u opdrachten vanuit Azure Cloud Shell-sessie uitvoert.

   ```azurecli-interactive
   az storage blob upload-batch -s <source-path> -d \$web --account-name <storage-account-name>
   ```

   * Vervang de waarde van de tijdelijke plaatsaanduiding `<storage-account-name>` door de naam van uw opslagaccount.

   * Vervang de `<source-path>` tijdelijke aanduiding met een pad naar de locatie van de bestanden die u wilt uploaden.

   > [!NOTE]
   > Als u een installatie van de locatie van Azure CLI gebruikt, dan kunt u het pad naar een locatie op uw lokale computer (bijvoorbeeld: `C:\myFolder`.
   >
   > Als u Azure Cloud Shell gebruikt, hebt u om te verwijzen naar een bestandsshare die zichtbaar is voor de Cloud Shell. Deze locatie kan worden de bestandsshare van de Cloud zelf of een bestaande bestandsshare die u vanuit de Cloud Shell koppelen delen. Zie voor meer informatie hoe u dit doet, [behouden bestanden in Azure Cloud Shell](https://docs.microsoft.com/azure/cloud-shell/persisting-shell-storage).

<a id="cli-find-url" />

### <a name="find-the-website-url-by-using-the-azure-cli"></a>De website-URL zoeken met behulp van de Azure CLI

U kunt inhoud vanuit een browser weergeven met behulp van de openbare URL van de website.

De URL zoeken met behulp van de volgende opdracht uit:

```azurecli-interactive
az storage account show -n <storage-account-name> -g <resource-group-name> --query "primaryEndpoints.web" --output tsv
```

* Vervang de waarde van de tijdelijke plaatsaanduiding `<storage-account-name>` door de naam van uw opslagaccount.

* Vervang de `<resource-group-name>` aanduidingswaarde met de naam van de resourcegroep.

<a id="powershell" />

## <a name="use-powershell"></a>PowerShell gebruiken

U kunt statische website hosting met behulp van de Azure PowerShell-module.

1. Open een Windows PowerShell-opdrachtvenster.

2. Controleer of u Azure PowerShell-module Az versie 0,7 of hoger.

   ```powershell
   Get-InstalledModule -Name Az -AllVersions | select Name,Version
   ```

   Als u PowerShell wilt installeren of upgraden, raadpleegt u [De Azure PowerShell-module installeren](/powershell/azure/install-Az-ps).

3. Meld u aan bij uw Azure-abonnement met de opdracht `Connect-AzAccount` en volg de instructies op het scherm.

   ```powershell
   Connect-AzAccount
   ```

4. Als uw identiteit gekoppeld aan meer dan één abonnement is, stel uw actieve abonnement-abonnement van het opslagaccount die als voor uw statische website host.

   ```powershell
   $context = Get-AzSubscription -SubscriptionId <subscription-id>
   Set-AzContext $context
   ```

   Vervang de `<subscription-id>` aanduidingswaarde met de ID van uw abonnement.

5. Krijg de opslagaccountcontext op waarin het opslagaccount dat u wilt gebruiken.

   ```powershell
   $storageAccount = Get-AzStorageAccount -ResourceGroupName "<resource-group-name>" -AccountName "<storage-account-name>"
   $ctx = $storageAccount.Context
   ```

   * Vervang de `<resource-group-name>` aanduidingswaarde met de naam van de resourcegroep.

   * Vervang de waarde van de tijdelijke plaatsaanduiding `<storage-account-name>` door de naam van uw opslagaccount.

6. Statische website hosting inschakelen.

   ```powershell
   Enable-AzStorageStaticWebsite -Context $ctx -IndexDocument <index-document-name> -ErrorDocument404Path <error-document-name>
   ```

   * Vervang de `<error-document-name>` tijdelijke aanduiding door de naam van het document fout die wordt weergegeven aan gebruikers wanneer een browser vraagt om een pagina op de site die niet bestaat.

   * Vervang de `<index-document-name>` tijdelijke aanduiding door de naam van de Indexdocument. Dit document wordt vaak 'index.html'.

7. Uploaden van objecten die u wilt de *$web* container uit een bronmap.

    ```powershell
    # upload a file
    set-AzStorageblobcontent -File "<path-to-file>" `
    -Container `$web `
    -Blob "<blob-name>" `
    -Context $ctx
     ```

   * Vervang de `<path-to-file>` aanduidingswaarde met de volledig gekwalificeerde pad naar het bestand dat u wilt uploaden (bijvoorbeeld: `C:\temp\index.html`).

   * Vervang de `<blob-name>` aanduidingswaarde met de naam die u wilt de resulterende blob geven (bijvoorbeeld: `index.html`).

<a id="powershell-find-url" />

### <a name="find-the-website-url-by-using-powershell"></a>De website-URL zoeken met behulp van PowerShell

U kunt inhoud vanuit een browser weergeven met behulp van de openbare URL van de website.

De URL zoeken met behulp van de volgende opdracht uit:

```powershell
 $storageAccount = Get-AzStorageAccount -ResourceGroupName "<resource-group-name>" -AccountName "<storage-account-name>"
Write-Output $storageAccount.PrimaryEndpoints.Web
```

* Vervang de `<resource-group-name>` aanduidingswaarde met de naam van de resourcegroep.

* Vervang de waarde van de tijdelijke plaatsaanduiding `<storage-account-name>` door de naam van uw opslagaccount.

<a id="metrics" />

## <a name="enable-metrics-on-static-website-pages"></a>Metrische gegevens op pagina's met statische website inschakelen

Nadat u de metrische gegevens hebt ingeschakeld, verkeer statistieken voor bestanden in de **$web** container worden gerapporteerd in het dashboard.

1. Klik op **instellingen** > **bewaking** > **metrische gegevens**.

   Metrische gegevens worden gegenereerd door het Inhaken op verschillende metrische gegevens over API's. API-leden die wordt gebruikt binnen een bepaalde periode om te kunnen alleen richten op de leden die als resultaat de gegevens alleen worden weergegeven in de portal. Om ervoor te zorgen dat om te selecteren die nodig zijn lid van de API kunt u, is de eerste stap om uit te breiden het tijdsbestek.

2. Klik op de knop tijdsbestek en selecteer **afgelopen 24 uur** en klik vervolgens op **toepassen**.

   ![Metrische gegevens van Azure Storage statische websites tijdsbereik](./media/storage-blob-static-website/storage-blob-static-website-metrics-time-range.png)

3. Selecteer **Blob** uit de *Namespace* vervolgkeuzelijst.

   ![Azure Storage statische websites metrische gegevens naamruimte](./media/storage-blob-static-website/storage-blob-static-website-metrics-namespace.png)

4. Selecteer vervolgens de **uitgaande** metrische gegevens.

   ![Azure Storage statische websites metrische gegevens over metrische gegevens](./media/storage-blob-static-website/storage-blob-static-website-metrics-metric.png)

5. Selecteer **som** uit de *aggregatie* selector.

   ![Azure aggregatie van opslag statische websites metrische gegevens](./media/storage-blob-static-website/storage-blob-static-website-metrics-aggregation.png)

6. Klik op de **filter toevoegen** knop en kies **API-naam** uit de *eigenschap* selector.

   ![Statische websites metrische API voor de naam van Azure Storage](./media/storage-blob-static-website/storage-blob-static-website-metrics-api-name.png)

7. Selecteer het vak naast **GetWebContent** in de *waarden* selector voor het vullen van het rapport metrische gegevens.

   ![Azure Storage statische websites metrics GetWebContent](./media/storage-blob-static-website/storage-blob-static-website-metrics-getwebcontent.png)

## <a name="next-steps"></a>Volgende stappen

* [Een statische website hosten in Azure Storage](storage-blob-static-website.md)
* [Het Azure CDN gebruiken voor toegang tot blobs met aangepaste domeinen via HTTPS](storage-https-custom-domain-cdn.md)
* [Een aangepaste domeinnaam voor uw blob- en web-eindpunten configureren](storage-custom-domain-name.md)
* [Azure Functions](/azure/azure-functions/functions-overview)
* [Azure App Service](/azure/app-service/overview)
* [Uw eerste serverloze web-app bouwen](https://docs.microsoft.com/azure/functions/tutorial-static-website-serverless-api-with-database)
* [Zelfstudie: Host uw domein in Azure DNS](../../dns/dns-delegate-domain-azure-dns.md)
