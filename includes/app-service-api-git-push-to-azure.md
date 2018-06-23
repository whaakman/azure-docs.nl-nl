De Azure CLI gebruiken om de externe implementatie-URL voor uw API-toepassing op te halen. Vervang in de volgende opdracht *\<app_name>* door de naam van uw webtoepassing.

```azurecli-interactive
az webapp deployment source config-local-git --name <app_name> --resource-group myResourceGroup --query url --output tsv
```

Configureer uw lokale Git-implementatie zodat deze naar de externe instantie kan pushen.

```bash
git remote add azure <URI from previous step>
```

Push naar de externe Azure-instantie om uw app te implementeren. U wordt gevraagd naar het wachtwoord dat u eerder hebt gemaakt bij het maken van de implementatiegebruiker. Zorg ervoor dat u het wachtwoord dat u eerder in de Quick Start hebt gemaakt en niet het wachtwoord die u aan te melden bij de Azure-portal invoeren.

```bash
git push azure master
```
