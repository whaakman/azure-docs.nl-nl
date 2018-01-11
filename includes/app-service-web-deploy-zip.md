## <a name="deploy-uploaded-zip-file"></a>Geüpload ZIP-bestand implementeren

Implementeer in de Cloud Shell het geüploade ZIP-bestand in uw web-app met de opdracht [az webapp deployment source config-zip](/cli/azure/webapp/deployment/source?view=azure-cli-latest#az_webapp_deployment_source_config_zip). Vergeet niet *\<app_name>* te vervangen door de naam van uw web-app.

```azurecli-interactive
az webapp deployment source config-zip --resource-group myResouceGroup --name <app_name> --src clouddrive/myAppFiles.zip
```

Met deze opdracht worden de bestanden en mappen uit het ZIP-bestand geïmplementeerd in de standaardmap voor de App Service-toepassing (`\home\site\wwwroot`) en wordt de app opnieuw opgestart. Als er een aanvullend aangepast opbouwproces is geconfigureerd, wordt dit ook uitgevoerd.
