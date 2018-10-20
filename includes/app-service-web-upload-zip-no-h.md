In de [Azure-portal](https://portal.azure.com), klikt u op **resourcegroepen** > **cloud-shell-storage -\<your_region >**  >   **\<naam_opslagaccount >**.

![Cloud Shell-storage-account zoeken](../articles/app-service/media/app-service-deploy-zip/upload-choose-storage-account.png)

In de **overzicht** pagina van het opslagaccount, selecteer **bestanden**.

Selecteer de automatisch gegenereerde bestandsshare en selecteer **uploaden**. Deze bestandsshare is gekoppeld in de Cloud Shell als `clouddrive`.

![Knop voor uploaden zoeken](../articles/app-service/media/app-service-deploy-zip/upload-select-button.png)

Klik op de kiezer bestand en selecteer het ZIP-bestand en klik vervolgens op **uploaden**. 

In de Cloud Shell gebruiken `ls` om te controleren of u dat het geüploade ZIP-bestand in de standaard zien kunt `clouddrive` delen.

```azurecli-interactive
ls clouddrive
```
