---
title: Lokale Git-implementatie op de Azure App Service
description: Informatie over het inschakelen van lokale Git-implementatie in Azure App Service.
services: app-service
documentationcenter: 
author: dariagrigoriu
manager: cfowler
editor: mollybos
ms.assetid: ac50a623-c4b8-4dfd-96b2-a09420770063
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/14/2016
ms.author: dariagrigoriu
ms.openlocfilehash: 948c54a2e9be2260d0a7d2cce31b67ffbbd23d03
ms.sourcegitcommit: 79683e67911c3ab14bcae668f7551e57f3095425
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/25/2018
---
# <a name="local-git-deployment-to-azure-app-service"></a>Lokale Git-implementatie op de Azure App Service

Deze zelfstudie leert u hoe u uw app implementeert [Azure Web Apps](app-service-web-overview.md) van een Git-opslagplaats op uw lokale computer. App Service ondersteunt deze methode met de **lokale Git** Implementatieoptie wordt gebruikt in de [Azure-portal].

## <a name="prerequisites"></a>Vereisten

Voor deze zelfstudie hebt u het volgende nodig:

* GIT. U kunt de installatie van de binaire downloaden [hier](http://www.git-scm.com/downloads).
* Basiskennis van Git.
* Een Microsoft Azure-account. Als u geen account hebt, kunt u zich [aanmelden voor een gratis proefversie](https://azure.microsoft.com/pricing/free-trial) of [uw voordelen als Visual Studio-abonnee activeren](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details).

> [!NOTE]
> Als u wilt dat aan de slag met Azure App Service voordat u zich aanmeldt voor een Azure-account, gaat u naar [App Service uitproberen](https://azure.microsoft.com/try/app-service/), waar u direct een tijdelijke app kunt maken in App Service. U hebt geen creditcard nodig en u gaat geen verplichtingen aan.
>

## <a name="Step1"></a>Stap 1: Maak een lokale opslagplaats

De volgende taken uitvoeren om een nieuwe Git-opslagplaats maken.

1. Start een opdrachtregelprogramma, zoals **Git Bash** (Windows) of **Bash** (Unix-Shell). Op OS X-systemen u toegang hebt tot de opdrachtregel via de **Terminal** toepassing.
1. Ga naar de map waar de inhoud voor het implementeren van geplaatst worden.
1. Gebruik de volgende opdracht in een nieuwe Git-opslagplaats te initialiseren:

  ```bash
  git init
  ```

## <a name="Step2"></a>Stap 2: Uw inhoud doorvoeren

App Service biedt ondersteuning voor toepassingen die zijn gemaakt in diverse programmeertalen.

1. Als uw opslagplaats al de inhoud niet opnemen, een statisch HTML-bestand toevoegen als volgt; of sla deze stap over:
   * Start een teksteditor en maak een nieuw bestand met de naam **index.html** in de hoofdmap van de Git-opslagplaats
   * De volgende tekst toevoegen als de inhoud van de index.html bestand en sla het: *Hello Git!*
1. Vanaf de opdrachtregel, controleert u of u onder de hoofdmap van de Git-opslagplaats. Gebruik vervolgens de volgende opdracht om bestanden naar uw opslagplaats toevoegen:

        git add -A 
1. Voer vervolgens de wijzigingen aan de opslagplaats met behulp van de volgende opdracht:

        git commit -m "Hello Azure App Service"

## <a name="Step3"></a>Stap 3: De App Service-app-opslagplaats inschakelen

De volgende stappen om in te schakelen van een Git-opslagplaats voor uw App Service-app uitvoeren.

1. Meld u aan bij [Azure-portal].
1. Klik in de weergave van uw App Service-app op **instellingen > implementatiebron**. Klik op **bron kiezen**, klikt u vervolgens op **lokale Git-opslagplaats**, en klik vervolgens op **OK**.

    ![Lokale Git-opslagplaats](./media/app-service-deploy-local-git/local_git_selection.png)

1. Als dit de eerste keer instellen van een opslagplaats in Azure, moet u aanmeldingsreferenties voor het maken. U kunt ze zich aanmelden bij de Azure-opslagplaats en push wijzigingen vanaf uw lokale Git-opslagplaats. Uit uw Web-App-weergave, klikt u op **instellingen > implementatiereferenties**, configureert u uw implementatie gebruikersnaam en wachtwoord. Wanneer u bent klaar, klikt u op **opslaan**.

    ![](./media/app-service-deploy-local-git/deployment_credentials.png)

## <a name="Step4"></a>Stap 4: Implementeer uw project

Gebruik de volgende stappen voor het publiceren van uw app in App Service met lokale Git.

1. In de weergave van uw Web-App in de Azure portal op **instellingen > eigenschappen** voor de **Git-URL**.

    ![](./media/app-service-deploy-local-git/git_url.png)

    **GIT-URL** is de externe verwijzing om te implementeren op van uw lokale opslagplaats. U gebruikt deze URL in de volgende stappen.
1. Via de opdrachtregel, controleert u of u in de hoofdmap van uw lokale Git-opslagplaats.
1. Gebruik `git remote` om toe te voegen van de externe verwijzing die worden vermeld in **Git-URL** uit stap 1. De opdracht ziet er ongeveer als volgt uit:

    ```bash
    git remote add azure https://<username>@localgitdeployment.scm.azurewebsites.net:443/localgitdeployment.git
    ```

   > [!NOTE]
   > De **externe** opdracht voegt een benoemde verwijzing naar een externe opslagplaats. In dit voorbeeld wordt een verwijzing met de naam 'azure' voor uw web-app-opslagplaats gemaakt.
   >

1. De inhoud van uw App service met behulp van de nieuwe push **azure** externe die u hebt gemaakt.

    ```bash
    git push azure master
    ```

    U wordt gevraagd om het wachtwoord dat u eerder hebt gemaakt wanneer u uw referenties voor implementatie in de Azure portal opnieuw. Voer het wachtwoord (Let erop dat Gitbash niet sterretjes naar de console biedt echo als het type van uw wachtwoord). 
1. Ga terug naar uw app in de Azure-portal. Een logboekvermelding van de meest recente push moet worden weergegeven in de **implementaties** weergeven.

    ![](./media/app-service-deploy-local-git/deployment_history.png)

1. Klik op de **Bladeren** knop aan de bovenkant van de webpagina van de app om te controleren of de inhoud is geïmplementeerd.

## <a name="Step5"></a>Problemen oplossen

Hier volgen enkele fouten of problemen meestal opgetreden bij het gebruik van Git publiceren naar een App Service-app in Azure:

---
**Symptoom**:`Unable to access '[siteURL]': Failed to connect to [scmAddress]`

**Oorzaak**: deze fout kan optreden als de app niet actief is.

**Resolutie**: de app te starten in de Azure portal. GIT-implementatie is niet beschikbaar wanneer de Web-App is gestopt.

---
**Symptoom**:`Couldn't resolve host 'hostname'`

**Oorzaak**: deze fout kan optreden als de gegevens hebt ingevoerd bij het maken van de 'azure' externe onjuist is.

**Resolutie**: Gebruik de `git remote -v` opdracht om een lijst van alle afstandsbedieningen, samen met de bijbehorende URL. Controleer of de URL voor de externe 'azure' klopt. Indien nodig, verwijderen en opnieuw maken van deze extern met behulp van de juiste URL.

---
**Symptoom**:`No refs in common and none specified; doing nothing. Perhaps you should specify a branch such as 'master'.`

**Oorzaak**: deze fout kan optreden als u een vertakking tijdens niet opgeeft `git push`, of als u niet hebt ingesteld de `push.default` waarde in `.gitconfig`.

**Resolutie**: uitvoeren van de push-bewerking opnieuw, geven de hoofdvertakking. Bijvoorbeeld:

```bash
git push azure master
```

---
**Symptoom**:`src refspec [branchname] does not match any.`

**Oorzaak**: deze fout kan optreden als u probeert te pushen naar een vertakking dan master in de 'azure' externe.

**Resolutie**: uitvoeren van de push-bewerking opnieuw, geven de hoofdvertakking. Bijvoorbeeld:

```bash
git push azure master
```

---
**Symptoom**:`RPC failed; result=22, HTTP code = 5xx.`

**Oorzaak**: deze fout kan optreden als u probeert om een grote git-opslagplaats via HTTPS.

**Resolutie**: Wijzig de git-configuratie op de lokale computer naar de postBuffer groter maken

```bash
git config --global http.postBuffer 524288000
```

---
**Symptoom**:`Error - Changes committed to remote repository but your web app not updated.`

**Oorzaak**: deze fout kan optreden als u een Node.js-app met een package.json-bestand waarin aanvullende vereiste modules implementeert.

**Resolutie**: aanvullende berichten met 'npm ERR!' moet de aangemelde voorafgaand aan deze fout en aanvullende context kan bieden over de fout. Hier volgen enkele bekende oorzaken van deze fout en de bijbehorende 'npm ERR!' Bericht:

* **Ongeldige package.json-bestand**: npm ERR! Afhankelijkheden kan niet worden gelezen.
* **Systeemeigen module die geen heeft een binaire distributiepunt voor Windows**:

  * `npm ERR! \cmd "/c" "node-gyp rebuild"\ failed with 1`

      OF
  * `npm ERR! [modulename@version] preinstall: \make || gmake\`

## <a name="additional-resources"></a>Aanvullende resources

* [Git-documentatie](http://git-scm.com/documentation)
* [Project Kudu documentatie](https://github.com/projectkudu/kudu/wiki)
* [Doorlopende implementatie in Azure App Service](app-service-continuous-deployment.md)
* [PowerShell voor Azure gebruiken](/powershell/azure/overview)
* [Het gebruik van de Azure-opdrachtregelinterface](../cli-install-nodejs.md)

[Azure Developer Center]: http://www.windowsazure.com/en-us/develop/overview/
[Azure-portal]: https://portal.azure.com
[Git website]: http://git-scm.com
[Installing Git]: http://git-scm.com/book/en/Getting-Started-Installing-Git
[Azure Command-Line Interface]: https://azure.microsoft.com/en-us/documentation/articles/xplat-cli-azure-resource-manager/

[Using Git with CodePlex]: http://codeplex.codeplex.com/wikipage?title=Using%20Git%20with%20CodePlex&referringTitle=Source%20control%20clients&ProjectName=codeplex
[Quick Start - Mercurial]: http://mercurial.selenic.com/wiki/QuickStart
