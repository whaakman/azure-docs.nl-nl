Configureer de lokale Git-implementatie naar de API-app met de opdracht [az webapp source-control config-local-git](/cli/azure/appservice/web/source-control#config-local-git).   

App Service ondersteunt verschillende manieren om inhoud te distribueren naar een web-app, waaronder FTP, lokale Git, GitHub, Visual Studio Team Services en Bitbucket. In deze snelstartgids gaat u implementeren met behulp van een lokale Git. Dit betekent dat u implementeert met behulp van een Git-opdracht voor het pushen vanuit een lokale opslagplaats naar een opslagplaats in Azure.  

Gebruik het script hieronder om de implementatiereferenties op accountniveau in te stellen die u gaat gebruiken om de code te pushen. Zorg ervoor dat u uw eigen waarden gebruikt voor de gebruikersnaam en het wachtwoord.   

```azurecli-interactive
az webapp deployment user set --user-name <desired user name> --password <desired password>
```
