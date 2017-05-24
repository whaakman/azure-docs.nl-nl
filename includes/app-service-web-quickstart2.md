Gebruik de opdracht [az appservice web create](/cli/azure/appservice/web#create) om de web-app te maken. Vervang in de volgende opdracht de tijdelijke aanduiding `<app_name>` door de naam van een unieke app. De `<app_name>` wordt gebruikt in de standaard-DNS-site voor de web-app. Als `<app_name>` niet uniek is, wordt het foutbericht "Er bestaan al een website met de naam <naam_app>" weergegeven.

```azurecli
az appservice web create --name <app_name> --resource-group myResourceGroup --plan quickStartPlan
```

Wanneer de web-app is gemaakt, toont de Azure CLI soortgelijke informatie als in het volgende voorbeeld:

```json
{
  "availabilityState": "Normal",
  "clientAffinityEnabled": true,
  "clientCertEnabled": false,
  "cloningInfo": null,
  "containerSize": 0,
  "dailyMemoryTimeQuota": 0,
  "defaultHostName": "<app_name>.azurewebsites.net",
  "enabled": true,
  "enabledHostNames": [
    "<app_name>.azurewebsites.net",
    "<app_name>.scm.azurewebsites.net"
  ],
  "gatewaySiteName": null,
  "hostNameSslStates": [
    {
      "hostType": "Standard",
      "name": "<app_name>.azurewebsites.net",
      "sslState": "Disabled",
      "thumbprint": null,
      "toUpdate": null,
      "virtualIp": null
    }
    < JSON data removed for brevity. >
}
```

Blader naar de site (`http://<app_name>.azurewebsites.net`) om uw nieuwe web-app te bekijken.

![app-service-web-service-gemaakt](../articles/app-service-web/media/app-service-web-get-started-nodejs-poc/app-service-web-service-created.png)


## <a name="configure-local-git-deployment"></a>Lokale Git-implementatie configureren

App Service ondersteunt verschillende manieren om inhoud te distribueren naar een web-app, waaronder FTP, lokale Git, GitHub, Visual Studio Team Services en Bitbucket. 

In deze snelstartgids gaat u implementeren met behulp van een lokale Git. Dit betekent dat u implementeert met behulp van een Git-opdracht voor het pushen vanuit een lokale opslagplaats naar een opslagplaats in Azure. 

Gebruik de opdracht [az appservice web source-control config-local-git](/cli/azure/appservice/web/source-control#config-local-git) voor het configureren van lokale Git-toegang tot de web-app.

```azurecli
az appservice web source-control config-local-git --name <app_name> --resource-group myResourceGroup --query url --output tsv
```

De uitvoer heeft de volgende indeling:

```
https://<username>@<app_name>.scm.azurewebsites.net:443/<app_name>.git
```

Sla de weergegeven URI op. U gebruikt deze in de volgende stap. De `<username>` is de [implementatiegebruiker](#configure-a-deployment-user) die u in een vorige stap hebt gemaakt.

## <a name="push-to-azure-from-git"></a>Pushen naar Azure vanaf Git

Voeg een externe Azure-instantie toe aan uw lokale Git-opslagplaats.

```bash
git remote add azure <URI from previous step>
```

Push naar de externe Azure-instantie om uw app te implementeren. U wordt gevraagd naar het wachtwoord dat u eerder hebt gemaakt bij het maken van de implementatiegebruiker. Zorg ervoor dat u het wachtwoord dat u hebt gemaakt bij [Een implementatiegebruiker configureren](#configure-a-deployment-user) gebruikt en niet het wachtwoord dat u gebruikt om u aan te melden bij Azure Portal.

```azurecli
git push azure master
```

De voorgaande opdracht voert informatie uit die lijkt op het volgende voorbeeld:

```bash
Counting objects: 13, done.
Delta compression using up to 4 threads.
Compressing objects: 100% (11/11), done.
Writing objects: 100% (13/13), 2.07 KiB | 0 bytes/s, done.
Total 13 (delta 2), reused 0 (delta 0)
remote: Updating branch 'master'.
remote: Updating submodules.
remote: Preparing deployment for commit id 'cc39b1e4cb'.
remote: Generating deployment script.
remote: Generating deployment script for Web Site
remote: Generated deployment script files
remote: Running deployment command...
remote: Handling Basic Web Site deployment.
remote: KuduSync.NET from: 'D:\home\site\repository' to: 'D:\home\site\wwwroot'
remote: Deleting file: 'hostingstart.html'
remote: Copying file: '.gitignore'
remote: Copying file: 'LICENSE'
remote: Copying file: 'README.md'
remote: Finished successfully.
remote: Running post deployment command(s)...
remote: Deployment successful.
To https://<app_name>.scm.azurewebsites.net/<app_name>.git
 * [new branch]      master -> master
```

### <a name="browse-to-the-app"></a>Bladeren naar de app


Blader naar de geïmplementeerde app:

```
http://<app_name>.azurewebsites.net
```

