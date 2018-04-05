## <a name="deploy-template-from-cloud-shell"></a>Sjabloon implementeren vanuit Cloud Shell

U kunt [Cloud Shell](../articles/cloud-shell/overview.md) gebruiken om uw sjabloon te implementeren. U moet echter eerst uw sjabloon laden in het opslagaccount voor uw Cloud-Shell. Als u Cloud Shell niet hebt gebruikt, raadpleegt u [Overzicht van Azure Cloud Shell](../articles/cloud-shell/overview.md) voor informatie over het instellen.

1. Meld u aan bij [Azure Portal](https://portal.azure.com).

1. Selecteer de Cloud Shell-resourcegroep. Het naampatroon is `cloud-shell-storage-<region>`.

   ![Resourcegroep selecteren](./media/resource-manager-cloud-shell-deploy/select-cs-resource-group.png)

1. Selecteer het opslagaccount voor Cloud Shell.

   ![Opslagaccount selecteren](./media/resource-manager-cloud-shell-deploy/select-storage.png)

1. Selecteer **Blobs**.

   ![Selecteer de blobs](./media/resource-manager-cloud-shell-deploy/select-blobs.png)

1. Selecteer **+ Container**.

   ![Container toevoegen](./media/resource-manager-cloud-shell-deploy/add-container.png)

1. Geef uw container een naam en een toegangsniveau. De voorbeeldsjabloon in dit artikel bevat geen gevoelige informatie, dus toestaan anonieme leestoegang. Selecteer **OK**.

   ![Container waarden opgeven](./media/resource-manager-cloud-shell-deploy/provide-container-values.png)

1. Selecteer de container die u hebt gemaakt.

   ![Selecteer de nieuwe container](./media/resource-manager-cloud-shell-deploy/select-container.png)

1. Selecteer **Uploaden**.

   ![Blob uploaden](./media/resource-manager-cloud-shell-deploy/upload-blob.png)

1. Zoek de sjabloon en upload deze.

   ![Bestand uploaden](./media/resource-manager-cloud-shell-deploy/find-and-upload-template.png)

1. Nadat deze is geüpload, selecteer de sjabloon.

   ![Nieuwe sjabloon selecteren](./media/resource-manager-cloud-shell-deploy/select-new-template.png)

1. Kopieer de URL.

   ![URL kopiëren](./media/resource-manager-cloud-shell-deploy/copy-url.png)

1. Open de prompt.

   ![Cloud Shell openen](./media/resource-manager-cloud-shell-deploy/start-cloud-shell.png)
