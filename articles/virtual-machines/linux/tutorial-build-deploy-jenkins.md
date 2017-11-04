---
title: CI/CD van Jenkins voor virtuele machines in Azure met VSTS | Microsoft Docs
description: Instellen van continue integratie (CI) en continue implementatie (CD) van een Node.js-app Jenkins met virtuele Azure-machines van Release Management in Visual Studio Team Services (VSTS) of Microsoft Team Foundation Server (TFS)
author: ahomer
manager: douge
editor: tysonn
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: tutorial
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 10/19/2017
ms.author: ahomer
ms.custom: mvc
ms.openlocfilehash: d5c15d6ab36a8454d1c69bac498be89b990c7e33
ms.sourcegitcommit: dfd49613fce4ce917e844d205c85359ff093bb9c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/31/2017
---
# <a name="deploy-your-app-to-linux-vms-using-jenkins-and-vsts"></a>Uw app implementeren in een virtuele Linux-machines met behulp van Jenkins en VSTS

Continue integratie (CI) en continue implementatie (CD) is een pijplijn waarmee u kunt bouwen, vrijgeven en implementeer uw code. Visual Studio Team Services (VSTS) biedt een complete, volledig functionele aantal CI/CD automation-hulpprogramma's voor implementatie naar Azure. Jenkins is een populaire 3rd derden CI/CD servergebaseerde hulpprogramma dat ook CI/CD automation biedt. U kunt beide samen gebruiken om aan te passen hoe u uw cloud-app of service leveren.

In deze zelfstudie maakt u Jenkins gebruiken voor het bouwen een **Node.js-web-app**, en VSTS of Team Foundation Server (TFS) te implementeren naar een [implementatiegroep](https://www.visualstudio.com/docs/build/concepts/definitions/release/deployment-groups/) met virtuele Linux-machines.

U gaat het volgende doen:

> [!div class="checklist"]
> * De voorbeeldapp downloaden
> * Jenkins plugins configureren
> * Configureren van een project Jenkins Freestyle voor Node.js
> * Jenkins configureren voor integratie VSTS
> * Een Jenkins service-eindpunt maken
> * Een implementatiegroep voor de virtuele machines in Azure maken
> * Een definitie van de release VSTS maken
> * Handmatige en CI geactiveerd implementaties uitvoeren

## <a name="before-you-begin"></a>Voordat u begint

* U moet toegang tot een Jenkins-server. Als u een server Jenkins nog geen hebt gemaakt, raadpleegt u [maken van een model Jenkins op een virtuele Machine van Azure](https://docs.microsoft.com/en-us/azure/jenkins/install-jenkins-solution-template). 

* Aanmelden bij uw account VSTS (`https://{youraccount}.visualstudio.com`). 
  U krijgt een [gratis account VSTS](https://go.microsoft.com/fwlink/?LinkId=307137&clcid=0x409&wt.mc_id=o~msft~vscom~home-vsts-hero~27308&campaign=o~msft~vscom~home-vsts-hero~27308).

  > [!NOTE]
  > Zie voor meer informatie [verbinding maken met VSTS](https://www.visualstudio.com/docs/setup-admin/team-services/connect-to-visual-studio-team-services).

*  U moet een virtuele Linux-machine voor het implementatiedoel van een.  Zie voor meer informatie [maken en beheren van virtuele Linux-machines met de Azure CLI](https://docs.microsoft.com/en-us/azure/virtual-machines/linux/tutorial-manage-vm).

*  Open de binnenkomende poort 80 voor uw virtuele machine.  Zie voor meer informatie [netwerkbeveiligingsgroepen met de Azure portal maken](https://docs.microsoft.com/en-us/azure/virtual-network/virtual-networks-create-nsg-arm-pportal).

## <a name="get-the-sample-app"></a>De voorbeeldapp downloaden

U moet een app implementeren in een Git-opslagplaats opgeslagen.
Voor deze zelfstudie wordt aangeraden u [deze voorbeeldapp beschikbaar is via GitHub](https://github.com/azooinmyluggage/fabrikam-node).  Deze zelfstudie bevat een voorbeeld van een script gebruikt voor het installeren van Node.js en een toepassing.  Als u werken met uw eigen opslagplaats wilt, moet u een voorbeeld van een vergelijkbare configureren.

1. Maak een fork van deze app en noteer de locatie (URL) voor gebruik in latere stappen van deze zelfstudie.  Zie voor meer informatie [vertakken een opslagplaats](https://help.github.com/articles/fork-a-repo/)    

> [!NOTE]
> De app is gemaakt met [Yeoman](http://yeoman.io/learning/index.html); hierbij **Express**, **bower**, en **grunt**; en of hieraan sommige **npm** pakketten als afhankelijkheden.
> Het voorbeeld bevat ook een script dat Nginx ingesteld en de app wordt geïmplementeerd. Deze wordt uitgevoerd op de virtuele machines. In het bijzonder installeert het script knooppunt, Nginx en PM2; Hiermee configureert u Nginx en PM2; vervolgens wordt de app knooppunt gestart.

## <a name="configure-jenkins-plugins"></a>Jenkins plugins configureren

Eerst moet u twee Jenkins invoegtoepassingen voor **NodeJS** en **continue implementatie van VS Team Services**.

1. Open uw Jenkins-account en kies **Jenkins beheren**.
2. In de **beheren Jenkins** pagina **invoegtoepassingen beheren**.
3. Filter de lijst vinden de **NodeJS** invoegtoepassing en kies de **installeren zonder opnieuw opstarten** optie.
    ![De invoegtoepassing NodeJS toe te voegen aan Jenkins](media/tutorial-build-deploy-jenkins/jenkins-nodejs-plugin.png)
4. Filter de lijst vinden de **tegenover Team Services continue implementatie** invoegtoepassing en kies de **installeren zonder opnieuw opstarten** optie.
5. Ga terug naar het dashboard Jenkins en kies **Jenkins beheren**.
6. Kies **globale hulpprogramma configuratie**.  Zoeken naar **NodeJS** en klik op **NodeJS installaties**.
7. Schakel de **automatisch installeren** optie en voer vervolgens een **naam** waarde.
8. Klik op **Opslaan**.

## <a name="configure-a-jenkins-freestyle-project-for-nodejs"></a>Configureren van een project Jenkins Freestyle voor Node.js

1. Klik op **Nieuw Item**.  Voer een **itemnaam**.
2. Kies **Freestyle project**.  Klik op **OK**.
3. In de **Source Code Management** tabblad **Git** en voer de details van de opslagplaats en de vertakking die uw app-code bevat.    
    ![Een opslagplaats toevoegen aan uw build](media/tutorial-build-deploy-jenkins/jenkins-git.png)
4. In de **bouwen Triggers** tabblad **Poll SCM** en voert u de planning `H/03 * * * *` voor het pollen van de Git-opslagplaats voor wijzigingen elke drie minuten. 
5. In de **bouwen omgeving** tabblad **bieden knooppunt &amp; npm bin / map pad** en selecteer de **NodeJS installatie** waarde. Laat **npmrc bestand** ingesteld op "systeemstandaard gebruiken".
6. In de **bouwen** Kies **shell uitvoeren** en voer de opdracht `npm install` om te controleren of alle afhankelijkheden zijn bijgewerkt.


## <a name="configure-jenkins-for-vsts-integration"></a>Jenkins configureren voor integratie VSTS

  > [!NOTE]
  Zorg ervoor dat het persoonlijke toegangstoken (PAT) u voor de volgende stappen gebruikt bevat de **Release (lezen, schrijven, uitvoeren en beheren) toestemming in VSTS**.
 
1.  Maak een PAT in uw account VSTS als u er nog geen hebt. Deze gegevens voor toegang tot uw account VSTS voor Jenkins vereist.  Zorg ervoor dat u **opslaan** token-informatie voor toekomstige stappen in deze sectie.
  Lees [hoe maak ik een persoonlijk toegangstoken voor VSTS en TFS](https://www.visualstudio.com/docs/setup-admin/team-services/use-personal-access-tokens-to-authenticate) voor informatie over het genereren van een.
2. In de **na build acties** tabblad **na build actie toevoegen**. Kies **archiveren van de artefacten**.
3. Voor **bestanden te archiveren**, voer `**/*` alle bestanden opnemen.
4. Klik op om een andere actie **na build actie toevoegen**.
5. Kies **release in TFS/Team Services activeren**, voer de uri voor uw account VSTS zoals: `https://{your-account-name}.visualstudio.com`.
6. Voer de **teamproject** naam.
7. Kies een naam voor de **definitie release** (u maakt deze release-definitie later in VSTS).
8. Kies de referenties voor verbinding met uw omgeving VSTS of TFS.  Laat de **gebruikersnaam** leeg als u VSTS gebruikt.
   Voer een **gebruikersnaam en wachtwoord** als u een on-premises versie van TFS.    
    ![Jenkins na build acties configureren](media/tutorial-build-deploy-jenkins/trigger-release-from-jenkins.png)
5. **Sla** het jenkins-project.

## <a name="create-a-jenkins-service-endpoint"></a>Een Jenkins service-eindpunt maken

Een service-eindpunt kunt VSTS verbinding maken met Jenkins.

1. Open de **Services** pagina in VSTS, open de **nieuwe Service-eindpunt** lijst en kies **Jenkins**.
    ![Een eindpunt Jenkins toevoegen](media/tutorial-build-deploy-jenkins/add-jenkins-endpoint.png)
2. Voer een naam voor de verbinding.
3. Voer de URL van uw server Jenkins en maatstreepjes de **accepteren van niet-vertrouwde certificaten voor SSL** optie.  Een voorbeeld-URL is:`http://{YourJenkinsURL}.westcentralus.cloudapp.azure.com`
4. Voer de **gebruikersnaam en wachtwoord** voor uw account Jenkins.
5. Kies **verbinding controleren** om te controleren of de informatie juist is.
6. Kies **OK** om de service-eindpunt te maken.

## <a name="create-a-deployment-group-for-azure-virtual-machines"></a>Maak een implementatiegroep voor Azure Virtual Machines

U moet een [implementatiegroep](https://www.visualstudio.com/docs/build/concepts/definitions/release/deployment-groups/) registreren van de agent VSTS zodat de release-definitie op uw virtuele machine implementeren kunt.  Implementatiegroepen kunnen u eenvoudig om logische groepen doelmachines voor implementatie te definiëren en de vereiste agent installeren op elke machine.

   > [!NOTE]
   > Zorg ervoor dat u de vereiste onderdelen installeren in de volgende stappen en **sudo-machtigingen voor het script niet uitvoeren.**

1. Open de **Releases** tabblad van de **bouwen &amp; Release** hub, open vervolgens **implementatiegroepen**, en kies **+ nieuw**.
2. Voer een naam voor de implementatiegroep en een optionele beschrijving.
   Kies vervolgens **maken**.
3. Kies het besturingssysteem voor de virtuele doelmachine van uw implementatie.  Bijvoorbeeld, kiezen **Ubuntu 16.04 +**.
4. Maatstreepjes de **gebruiken een persoonlijk toegangstoken in het script voor verificatie**.
5. Selecteer de **systeemvereisten** koppeling.  De vereisten voor het besturingssysteem installeren.
6. Selecteer de **script kopiëren naar Klembord** kopiëren van het script.
7. **Meld u bij** voor uw implementatie doel-virtuele machine en **uitvoeren** het script.  **Geen** het script worden uitgevoerd met sudo-machtigingen.
8. Na de installatie wordt u gevraagd voor implementatie group-tags.  Accepteer de standaardinstellingen.
9. Schakel in VSTS, voor uw zojuist geregistreerde virtuele machine in **doelen** onder **Implementatiegroepen**.

## <a name="create-a-vsts-release-definition"></a>Een definitie van de release VSTS maken

De definitie van een release Hiermee geeft u het proces VSTS wordt uitgevoerd voor het implementeren van de app.  In dit voorbeeld moet u een shell-script uitvoeren.

De definitie van de release in VSTS maken:

1. Open de **Releases** op de **bouwen &amp; Release** hub, en kies **maken release definitie**. 
2. Selecteer de **leeg** sjabloon door te kiezen beginnen met een **leeg proces**.
3. In de **artefacten** sectie, klikt u op **+ artefacten toevoegen** en kies **Jenkins** voor **gegevensbrontype**. Selecteer uw verbinding Jenkins service-eindpunt. Vervolgens selecteert u de taak van de bron Jenkins en kies **toevoegen**.
4.  Klik op het weglatingsteken naast **omgeving 1**.  Klik op **toevoegen groep implementatiefase**.
5.  Kies uw **implementatiegroep**.
5. Klik op  **+**  toevoegen van een taak voor de **groep implementatiefase**.
6. Kies de **Shell-Script** taak en klikt u op **toevoegen**.    
    De **Shell-Script** taak wordt gebruikt voor de configuratie voor een script uit te voeren op elke server voor het installeren van Node.js en de app te starten. Als volgt configureren:
8. **Pad naar een script**:     
    `$(System.DefaultWorkingDirectory)/Fabrikam-Node/deployscript.sh`
9.  Klik op **Geavanceerd**, en schakel vervolgens **werkmap opgeven**.
10.  **Werkmap**:`$(System.DefaultWorkingDirectory)/Fabrikam-Node`
11. Bewerk de naam van de definitie van de versie op de naam die u hebt opgegeven in de **na build acties** tabblad van de build in Jenkins. Jenkins vereist deze naam kunnen voor het activeren van een nieuwe release wanneer de bron-artefacten zijn bijgewerkt.
12. Klik op **opslaan**, en klik op **OK** om op te slaan de release-definitie.

## <a name="execute-manual-and-ci-triggered-deployments"></a>Handmatige en CI geactiveerd implementaties uitvoeren

1. Kies **+ Release** en selecteer **maken Release**.
2. Selecteer de build u voltooid in de vervolgkeuzelijst gemarkeerde en kies **wachtrij**.
3. Kies de release-koppeling in het pop-upbericht. Bijvoorbeeld: ' Release **Release 1** is gemaakt. "
4. Open de **logboeken** tabblad bekijkt u de uitvoer van de release-console.
5. Open de URL van een van de servers die u hebt toegevoegd aan de implementatiegroep van uw in uw browser. Voer bijvoorbeeld`http://{your-server-ip-address}`
6. Ga naar de bron Git-opslagplaats en wijzigen van de inhoud van de **h1** in de kop van het bestand app/views/index.jade met gewijzigde tekst.
7. **Doorvoeren** uw wijziging.
8. Na een paar minuten ziet u een nieuwe release gemaakt in de **Releases** pagina van VSTS of TFS. Open de release voor de implementatie plaatsvinden. Gefeliciteerd.

## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie maakt u de implementatie van een app in Azure maken met Jenkins build en VSTS voor release geautomatiseerd. U hebt geleerd hoe u:

> [!div class="checklist"]
> * Uw app in Jenkins maken
> * Jenkins configureren voor integratie VSTS
> * Een implementatiegroep voor de virtuele machines in Azure maken
> * De definitie van een versie die de virtuele machines configureert en implementeert de app maken

Voorafgaande aan de volgende zelfstudie voor meer informatie over het implementeren van een licht stack (Linux, Apache, MySQL en PHP).

> [!div class="nextstepaction"]
> [LAMP-stack implementeren](tutorial-lamp-stack.md)