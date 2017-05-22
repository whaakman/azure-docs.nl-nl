## <a name="configure-a-deployment-user"></a>Een implementatiegebruiker configureren  

Voor FTP en lokale Git moet u een implementatiegebruiker hebben geconfigureerd op de server om de implementatie te verifiëren.

> [!NOTE]
> Een implementatiegebruiker is vereist voor FTP- en lokale Git-implementatie naar een web-app.
> De `username` en `password` zijn op accountniveau. Ze verschillen van de referenties van uw Azure-abonnement.
>

Voer de opdracht [az appservice web deployment user set](/cli/azure/appservice/web/deployment/user#set) uit om uw referenties voor de implementatie te maken.

```azurecli
az appservice web deployment user set --user-name <username> --password <password>
```

De gebruikersnaam moet uniek zijn en het wachtwoord moet sterk zijn. Als er een ` 'Conflict'. Details: 409`-fout optreedt, wijzigt u de gebruikersnaam. Als er een ` 'Bad Request'. Details: 400`-fout optreedt, kiest u een sterker wachtwoord.

U hoeft deze implementatiegebruiker maar één keer te maken. Vervolgens kunt u deze voor al uw Azure-implementaties gebruiken.

Noteer de gebruikersnaam en het wachtwoord. Deze hebt u in een latere stap weer nodig om de app te implementeren.