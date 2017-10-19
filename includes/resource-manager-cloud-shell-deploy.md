## <a name="deploy-template-from-cloud-shell"></a>Sjabloon implementeren vanuit Cloud Shell

U kunt [Cloud Shell](../articles/cloud-shell/overview.md) gebruiken om uw sjabloon te implementeren. U moet de sjabloon echter eerst in de bestandsshare voor Cloud Shell laden. Als u Cloud Shell niet hebt gebruikt, raadpleegt u [Overzicht van Azure Cloud Shell](../articles/cloud-shell/overview.md) voor informatie over het instellen.

1. Meld u aan bij [Azure Portal](https://portal.azure.com).

1. Selecteer de Cloud Shell-resourcegroep. Het naampatroon is `cloud-shell-storage-<region>`.

   ![Resourcegroep selecteren](./media/resource-manager-cloud-shell-deploy/select-cs-resource-group.png)

1. Selecteer het opslagaccount voor Cloud Shell.

   ![Opslagaccount selecteren](./media/resource-manager-cloud-shell-deploy/select-storage.png)

1. Selecteer **Bestanden**.

   ![Bestanden selecteren](./media/resource-manager-cloud-shell-deploy/select-files.png)

1. Selecteer de bestandsshare voor Cloud Shell. Het naampatroon is `cs-<user>-<domain>-com-<uniqueGuid>`.

   ![Bestandsshare selecteren](./media/resource-manager-cloud-shell-deploy/select-file-share.png)

1. Selecteer **Map toevoegen**.

   ![Map toevoegen](./media/resource-manager-cloud-shell-deploy/select-add-directory.png)

1. Geef deze de naam **Sjablonen** en selecteer **OK**.

   ![Map een naam geven](./media/resource-manager-cloud-shell-deploy/name-templates.png)

1. Selecteer een nieuwe map.

   ![Map selecteren](./media/resource-manager-cloud-shell-deploy/select-templates.png)

1. Selecteer **Uploaden**.

   ![Uploaden selecteren](./media/resource-manager-cloud-shell-deploy/select-upload.png)

1. Zoek de sjabloon en upload deze.

   ![Bestand uploaden](./media/resource-manager-cloud-shell-deploy/upload-files.png)

1. Open de prompt.

   ![Cloud Shell openen](./media/resource-manager-cloud-shell-deploy/start-cloud-shell.png)
