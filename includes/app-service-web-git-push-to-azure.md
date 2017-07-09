## <a name="push-to-azure-from-git"></a>Pushen naar Azure vanaf Git

Voeg een externe Azure-instantie toe aan uw lokale Git-opslagplaats.

```bash
git remote add azure <URI from previous step>
```

Push naar de externe Azure-instantie om uw app te implementeren. U wordt gevraagd naar het wachtwoord dat u eerder hebt gemaakt bij het maken van de implementatiegebruiker. Zorg ervoor dat u het wachtwoord dat u hebt gemaakt bij [Een implementatiegebruiker configureren](#configure-a-deployment-user) gebruikt en niet het wachtwoord dat u gebruikt om u aan te melden bij Azure Portal.

```bash
git push azure master
```

De voorgaande opdracht voert informatie uit die lijkt op het volgende voorbeeld:
