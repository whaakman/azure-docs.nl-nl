Voeg in het lokale terminalvenster een externe Azure-instantie toe aan uw lokale Git-opslagplaats. Vervang _&lt;paste\_copied\_url\_here>_ door de URL van de externe Git-instantie die u hebt opgeslagen vanuit [Een web-app maken](#create).

```bash
git remote add azure <deploymentLocalGitUrl-from-create-step>
```

Push naar de externe Azure-instantie om uw app te implementeren met de volgende opdracht. Zorg er als u om een wachtwoord wordt gevraagd voor dat u het wachtwoord dat u hebt gemaakt bij [Een implementatiegebruiker configureren](#configure-a-deployment-user) gebruikt en niet het wachtwoord dat u gebruikt om u aan te melden bij Azure Portal.

```bash
git push azure master
```

Het kan enkele minuten duren voor deze opdracht is uitgevoerd. De opdracht geeft informatie weer die lijkt op het volgende voorbeeld:
