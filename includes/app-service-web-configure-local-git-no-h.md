Configureer de lokale Git-implementatie naar de webtoepassing met de opdracht [az webapp deployment source config-local-git](/cli/azure/webapp/deployment/source#config-local-git).

App Service ondersteunt verschillende manieren om inhoud te distribueren naar een web-app, waaronder FTP, lokale Git, GitHub, Visual Studio Team Services en Bitbucket. In deze snelstartgids gaat u implementeren met behulp van een lokale Git. Dit betekent dat u implementeert met behulp van een Git-opdracht voor het pushen vanuit een lokale opslagplaats naar een opslagplaats in Azure. 

Vervang in de volgende opdracht *\<app_name>* door de naam van uw webtoepassing.

```azurecli-interactive
az webapp deployment source config-local-git --name <app_name> --resource-group myResourceGroup --query url --output tsv
```

De uitvoer heeft de volgende indeling:

```bash
https://<username>@<app_name>.scm.azurewebsites.net:443/<app_name>.git
```

De `<username>` is de [implementatiegebruiker](#configure-a-deployment-user) die u in een vorige stap hebt gemaakt.

Sla de weergegeven URI op. U gebruikt deze in de volgende stap. 
