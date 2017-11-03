---
title: Lokale Git-implementatie op de Azure App Service
description: Informatie over het inschakelen van lokale Git-implementatie in Azure App Service.
services: app-service
documentationcenter: 
author: dariagrigoriu
manager: erikre
editor: mollybos
ms.assetid: ac50a623-c4b8-4dfd-96b2-a09420770063
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/13/2016
ms.author: dariagrigoriu
ms.openlocfilehash: ed0239df7bf1e4d37987aaa929d0c67bec595b30
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="local-git-deployment-to-azure-app-service"></a>Lokale Git-implementatie op de Azure App Service
Deze zelfstudie leert u hoe u uw app implementeert [Azure Web Apps](app-service-web-overview.md) van een Git-opslagplaats op uw lokale computer. App Service ondersteunt deze methode met de **lokale Git** Implementatieoptie wordt gebruikt in de [Azure Portal].  
Veel van de Git-opdrachten die worden beschreven in dit artikel worden automatisch uitgevoerd tijdens het maken van een App Service-app met de [Azure-opdrachtregelinterface] zoals beschreven [hier](app-service-web-get-started-dotnet.md).

## <a name="prerequisites"></a>Vereisten
Voor deze zelfstudie hebt u het volgende nodig:

* GIT. U kunt de installatie van de binaire downloaden [hier](http://www.git-scm.com/downloads).  
* Basiskennis van Git.
* Een Microsoft Azure-account. Als u geen account hebt, kunt u zich [aanmelden voor een gratis proefversie](https://azure.microsoft.com/pricing/free-trial) of [uw voordelen als Visual Studio-abonnee activeren](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details).

> [!NOTE]
> Als u wilt dat aan de slag met Azure App Service voordat u zich aanmeldt voor een Azure-account, gaat u naar [App Service uitproberen](https://azure.microsoft.com/try/app-service/), waar u direct een tijdelijke app kunt maken in App Service. U hebt geen creditcard nodig en u doet geen toezeggingen.  
> 
> 

## <a name="Step1"></a>Stap 1: Maak een lokale opslagplaats
De volgende taken uitvoeren om een nieuwe Git-opslagplaats maken.

1. Start een opdrachtregelprogramma, zoals **GitBash** (Windows) of **Bash** (Unix-Shell). Op OS X-systemen is toegankelijk via de opdrachtregel de **Terminal** toepassing.
2. Ga naar de map waar de inhoud voor het implementeren van geplaatst worden.
3. Gebruik de volgende opdracht in een nieuwe Git-opslagplaats te initialiseren:

```bash  
git init
```

## <a name="Step2"></a>Stap 2: Uw inhoud doorvoeren
App Service biedt ondersteuning voor toepassingen die zijn gemaakt in diverse programmeertalen. 

1. Als uw opslagplaats bevat inhoud overslaan dit punt en verplaatsen naar het punt 2 hieronder. Als uw opslagplaats geen inhoud gewoon bevat nog te vullen met een statische HTML-bestand als volgt: 
   
   * Start een teksteditor en maak een nieuw bestand met de naam **index.html** in de hoofdmap van de Git-opslagplaats
   * De volgende tekst toevoegen als de inhoud voor de index.html bestand en sla het: *Hello Git!*
2. Vanaf de opdrachtregel, controleert u of u onder de hoofdmap van de Git-opslagplaats. Gebruik vervolgens de volgende opdracht om bestanden naar uw opslagplaats toevoegen:

```bash  
git add -A
```
3. Voer vervolgens de wijzigingen aan de opslagplaats met behulp van de volgende opdracht:

```bash  
git commit -m "Hello Azure App Service"
```  

## <a name="Step3"></a>Stap 3: De App Service-app-opslagplaats inschakelen
De volgende stappen om in te schakelen van een Git-opslagplaats voor uw App Service-app uitvoeren.

1. Meld u aan bij de [Azure Portal].
2. Klik op de blade van uw App Service-app **instellingen > implementatiebron**. Klik op **bron kiezen**, klikt u vervolgens op **lokale Git-opslagplaats**, en klik vervolgens op **OK**.  
   
    ![Lokale Git-opslagplaats](./media/app-service-deploy-local-git/local_git_selection.png)
3. Als dit de eerste keer instellen van een opslagplaats in Azure, moet u aanmeldingsreferenties voor het maken. U gebruikt deze aan te melden bij de Azure-opslagplaats en push wijzigingen vanaf uw lokale Git-opslagplaats. Op de blade van uw app, klikt u op **instellingen > implementatiereferenties**, configureert u uw implementatie gebruikersnaam en wachtwoord. Wanneer u bent klaar, klikt u op **opslaan**.
   
    ![](./media/app-service-deploy-local-git/deployment_credentials.png)

## <a name="Step4"></a>Stap 4: Implementeer uw project
Gebruik de volgende stappen voor het publiceren van uw app in App Service met lokale Git.

1. Klik in de blade van uw app in de Azure Portal op **instellingen > eigenschappen** voor de **Git-URL**.
   
    ![](./media/app-service-deploy-local-git/git_url.png)
   
    **GIT-URL** is de externe verwijzing om te implementeren op van uw lokale opslagplaats. U gebruikt deze URL in de volgende stappen.
2. Met behulp van de opdrachtregel, controleert u of u in de hoofdmap van uw lokale Git-opslagplaats.
3. Gebruik `git remote` om toe te voegen van de externe verwijzing die worden vermeld in **Git-URL** uit stap 1. De opdracht ziet er ongeveer als volgt:
   
        git remote add azure https://<username>@localgitdeployment.scm.azurewebsites.net:443/localgitdeployment.git         
   > [!NOTE]
   > De **externe** opdracht voegt een benoemde verwijzing naar een externe opslagplaats. In dit voorbeeld wordt een verwijzing met de naam 'azure' voor uw web-app-opslagplaats gemaakt.
   > 
   > 
4. De inhoud van uw App service met behulp van de nieuwe push **azure** externe u zojuist hebt gemaakt.

```bash  
git push azure master
```
    You will be prompted for the password you created earlier when you reset your deployment credentials in the Azure Portal. Enter the password (note that Gitbash does not echo asterisks to the console as you type your password). 
5. Ga terug naar uw app in de Azure Portal. Een logboekvermelding van de meest recente push moet worden weergegeven in de **implementaties** blade. 
   
    ![](./media/app-service-deploy-local-git/deployment_history.png)
6. Klik op de **Bladeren** knop aan de bovenkant van de app-blade om te controleren of de inhoud is geïmplementeerd. 

## <a name="Step5"></a>Problemen oplossen
Hier volgen enkele fouten of problemen meestal opgetreden bij het gebruik van Git publiceren naar een App Service-app in Azure:

- - -
**Symptoom**: geen toegang mogelijk tot [siteURL]: kan geen verbinding met [scmAddress]

**Oorzaak**: deze fout kan optreden als de app niet actief is.

**Resolutie**: de app te starten in de Azure Portal. GIT-implementatie werkt niet, tenzij de app wordt uitgevoerd. 

- - -
**Symptoom**: host 'naam' niet oplossen

**Oorzaak**: deze fout kan optreden als de gegevens hebt ingevoerd bij het maken van de 'azure' externe onjuist is.

**Resolutie**: Gebruik de `git remote -v` opdracht om een lijst van alle afstandsbedieningen, samen met de bijbehorende URL. Controleer of de URL voor de externe 'azure' klopt. Indien nodig, verwijderen en opnieuw maken van deze extern met behulp van de juiste URL.

- - -
**Symptoom**: Er is geen refs in algemene en er is geen opgegeven; niets te doen. Mogelijk moet u een vertakking zoals 'master' opgeven.

**Oorzaak**: deze fout kan optreden als u niet een vertakking opgeven bij het uitvoeren van een git push-bewerking en de push.default-waarde die wordt gebruikt door Git niet is ingesteld.

**Resolutie**: uitvoeren van de push-bewerking opnieuw, geven de hoofdvertakking. Bijvoorbeeld:

```bash  
git push azure master
```
- - -
**Symptoom**: src refspec [branchname] komt niet overeen.

**Oorzaak**: deze fout kan optreden als u probeert te pushen naar een vertakking dan master in de 'azure' externe.

**Resolutie**: uitvoeren van de push-bewerking opnieuw, geven de hoofdvertakking. Bijvoorbeeld:

```bash  
git push azure master
```
- - -
**Symptoom**: RPC is mislukt; resulteren = 22, HTTP-code = 502.

**Oorzaak**: deze fout kan optreden als u probeert om een grote git-opslagplaats via HTTPS.

**Resolutie**: Wijzig de git-configuratie op de lokale computer naar de postBuffer groter maken

```bash  
git config --global http.postBuffer 524288000
```
- - -
**Symptoom**: fout - de wijzigingen die zijn toegewezen aan externe opslagplaats, maar uw web-app niet bijgewerkt.

**Oorzaak**: deze fout kan optreden als u een Node.js-app met een package.json-bestand waarin aanvullende vereiste modules implementeert.

**Resolutie**: aanvullende berichten met 'npm ERR!' moet de aangemelde voorafgaand aan deze fout en aanvullende context kan bieden over de fout. Hier volgen enkele bekende oorzaken van deze fout en de bijbehorende 'npm ERR!' Bericht:

* **Ongeldige package.json-bestand**: npm ERR! Afhankelijkheden kan niet worden gelezen.
* **Systeemeigen module die geen heeft een binaire distributiepunt voor Windows**:
  
  * npm ERR! \`cmd '/ c"'knooppunt gyp rebuild'\` is mislukt: 1
    
      OF
  * npm ERR! [modulename@version] vooraf: \`zorg || gmake\`

## <a name="additional-resources"></a>Aanvullende resources
* [Git-documentatie](http://git-scm.com/documentation)
* [Project Kudu documentatie](https://github.com/projectkudu/kudu/wiki)
* [Continue implementatie in Azure App Service](app-service-continuous-deployment.md)
* [PowerShell voor Azure gebruiken](/powershell/azure/overview)
* [Het gebruik van de Azure-opdrachtregelinterface](../cli-install-nodejs.md)

[Azure Developer Center]: http://www.windowsazure.com/en-us/develop/overview/
[Azure Portal]: https://portal.azure.com
[Git website]: http://git-scm.com
[Installing Git]: http://git-scm.com/book/en/Getting-Started-Installing-Git
[Azure-opdrachtregelinterface]: https://azure.microsoft.com/en-us/documentation/articles/xplat-cli-azure-resource-manager/

[Using Git with CodePlex]: http://codeplex.codeplex.com/wikipage?title=Using%20Git%20with%20CodePlex&referringTitle=Source%20control%20clients&ProjectName=codeplex
[Quick Start - Mercurial]: http://mercurial.selenic.com/wiki/QuickStart
