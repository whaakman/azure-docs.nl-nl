Maak in de Cloud Shell implementatiereferenties met de opdracht [az webapp deployment user set](/cli/azure/webapp/deployment/user?view=azure-cli-latest#az_webapp_deployment_user_set). Een implementatiegebruiker is vereist voor FTP- en lokale Git-implementatie naar een webtoepassing. De gebruikersnaam en het wachtwoord staan op accountniveau. _Deze verschillen van de referenties van uw Azure-abonnement._

Vervang in het volgende voorbeeld wordt  *\<username >* en  *\<wachtwoord >* (inclusief haken) met een nieuwe gebruikersnaam en wachtwoord. De gebruikersnaam moet uniek zijn. Het wachtwoord moet ten minste acht tekens lang zijn en minimaal twee van de volgende drie typen elementen bevatten: letters, cijfers, symbolen. 

```azurecli-interactive
az webapp deployment user set --user-name <username> --password <password>
```

Als er een ` 'Conflict'. Details: 409`-fout optreedt, wijzigt u de gebruikersnaam. Als er een ` 'Bad Request'. Details: 400`-fout optreedt, kiest u een sterker wachtwoord.

U hoeft deze implementatiegebruiker maar één keer te maken. Vervolgens kunt u deze voor al uw Azure-implementaties gebruiken.

> [!NOTE]
> Leg de gebruikersnaam en het wachtwoord vast. U hebt ze later nodig bij het implementeren van de webtoepassing.
>
>
