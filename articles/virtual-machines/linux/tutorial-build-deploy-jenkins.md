---
title: CI/CD van Jenkins voor virtuele machines in Azure met teamservices | Microsoft Docs
description: Continue integratie (CI) en continue implementatie (CD) van een Node.js-app instellen met behulp van Jenkins virtuele Azure-machines van Release Management in Visual Studio Team Services of Microsoft Team Foundation Server
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
ms.openlocfilehash: bfda0475b58556db1236c8b051c59393384720f7
ms.sourcegitcommit: b07d06ea51a20e32fdc61980667e801cb5db7333
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/08/2017
---
# <a name="deploy-your-app-to-linux-vms-by-using-jenkins-and-team-services"></a>Uw app implementeren op Linux VM's met behulp van Jenkins en Team Services

Continue integratie (CI) en continue implementatie (CD) vormen een pijplijn waarmee u kunt bouwen, vrijgeven en implementeer uw code. Visual Studio Team Services biedt een complete, volledig functionele aantal CI/CD automation-hulpprogramma's voor implementatie naar Azure. Jenkins is een populair van derden CI/CD servergebaseerde hulpprogramma dat ook CI/CD automation biedt. U kunt Team Services en Jenkins samen gebruiken om aan te passen hoe u uw cloud-app of service leveren.

In deze zelfstudie maakt u Jenkins gebruiken voor het bouwen van een Node.js-web-app. U Team Services of Team Foundation Server vervolgens te implementeren om te gebruiken een [implementatiegroep](https://www.visualstudio.com/docs/build/concepts/definitions/release/deployment-groups/) die virtuele Linux-machines (VM's) bevat.

U gaat het volgende doen:

> [!div class="checklist"]
> * Haal de voorbeeld-app.
> * Configureer Jenkins invoegtoepassingen.
> * Configureer een project Jenkins Freestyle voor Node.js.
> * Jenkins configureren voor integratie met Team Services.
> * Maak een Jenkins service-eindpunt.
> * Maak een implementatiegroep voor de virtuele machines in Azure.
> * Een definitie van de release Team Services maken.
> * Handmatige en CI geactiveerd implementaties uitvoeren.

## <a name="before-you-begin"></a>Voordat u begint

* U moet toegang tot een Jenkins-server. Als u een server Jenkins nog geen hebt gemaakt, raadpleegt u [maken van een model Jenkins op een virtuele machine van Azure](https://docs.microsoft.com/azure/jenkins/install-jenkins-solution-template). 

* Aanmelden bij uw Team Services-account (**https://{youraccount}.visualstudio.com**). 
  U krijgt een [gratis account Team Services](https://go.microsoft.com/fwlink/?LinkId=307137&clcid=0x409&wt.mc_id=o~msft~vscom~home-vsts-hero~27308&campaign=o~msft~vscom~home-vsts-hero~27308).

  > [!NOTE]
  > Zie voor meer informatie [verbinding maken met het Team Services](https://www.visualstudio.com/docs/setup-admin/team-services/connect-to-visual-studio-team-services).

*  U moet een virtuele Linux-machine voor het implementatiedoel van een.  Zie voor meer informatie [maken en beheren van virtuele Linux-machines met de Azure CLI](https://docs.microsoft.com/azure/virtual-machines/linux/tutorial-manage-vm).

*  Open de binnenkomende poort 80 voor uw virtuele machine. Zie voor meer informatie [netwerkbeveiligingsgroepen met de Azure portal maken](https://docs.microsoft.com/azure/virtual-network/virtual-networks-create-nsg-arm-pportal).

## <a name="get-the-sample-app"></a>De voorbeeldapp downloaden

U moet een app te implementeren en opgeslagen in een Git-opslagplaats.
Voor deze zelfstudie wordt aangeraden dat u [deze voorbeeldapp beschikbaar is via GitHub](https://github.com/azooinmyluggage/fabrikam-node). Deze zelfstudie bevat een voorbeeldscript dat wordt gebruikt voor het installeren van Node.js en een toepassing. Als u werken met uw eigen opslagplaats wilt, moet u een voorbeeld van een vergelijkbare configureren.

Maak een fork van deze app en noteer de locatie (URL) voor gebruik in latere stappen van deze zelfstudie. Zie voor meer informatie [een opslagplaats vertakken](https://help.github.com/articles/fork-a-repo/).    

> [!NOTE]
> De app is gemaakt met [Yeoman](http://yeoman.io/learning/index.html). Snelle en bower knorvis wordt gebruikt. En sommige pakketten npm als afhankelijkheden heeft.
> Het voorbeeld bevat ook een script dat Nginx ingesteld en de app wordt geïmplementeerd. Deze wordt uitgevoerd op de virtuele machines. In het bijzonder het script:
> 1. Knooppunt, Nginx en PM2 installeert.
> 2. Hiermee configureert u Nginx en PM2.
> 3. Start de app knooppunt.

## <a name="configure-jenkins-plug-ins"></a>Jenkins invoegtoepassingen configureren

Eerst moet u twee Jenkins invoegtoepassingen configureren: **NodeJS** en **continue implementatie van VS Team Services**.

1. Open uw account Jenkins en selecteer **Jenkins beheren**.
2. Op de **beheren Jenkins** pagina **invoegtoepassingen beheren**.
3. Filter de lijst vinden de **NodeJS** invoegtoepassing en selecteer de **installeren zonder opnieuw opstarten** optie.
    ![De invoegtoepassing NodeJS toe te voegen aan Jenkins](media/tutorial-build-deploy-jenkins/jenkins-nodejs-plugin.png)
4. Filter de lijst vinden de **tegenover Team Services continue implementatie** invoegtoepassing en selecteer de **installeren zonder opnieuw opstarten** optie.
5. Ga terug naar de Jenkins dashboard en selecteer **Jenkins beheren**.
6. Selecteer **globale hulpprogramma configuratie**. Zoeken naar **NodeJS** en selecteer **NodeJS installaties**.
7. Selecteer de **automatisch installeren** optie en voer vervolgens een **naam** waarde.
8. Selecteer **Opslaan**.

## <a name="configure-a-jenkins-freestyle-project-for-nodejs"></a>Configureren van een project Jenkins Freestyle voor Node.js

1. Selecteer **Nieuw Item**. Voer de itemnaam van een.
2. Selecteer **Freestyle project**. Selecteer **OK**.
3. Op de **Source Code Management** tabblad **Git** en voer de details van de opslagplaats en de vertakking die uw app-code bevatten.    
    ![Een opslagplaats toevoegen aan uw build](media/tutorial-build-deploy-jenkins/jenkins-git.png)
4. Op de **bouwen Triggers** tabblad **Poll SCM** en voert u de planning `H/03 * * * *` voor het pollen van de Git-opslagplaats voor wijzigingen elke drie minuten. 
5. Op de **bouwen omgeving** tabblad **bieden knooppunt &amp; npm bin / map pad** en selecteer de **NodeJS installatie** waarde. Laat **npmrc bestand** ingesteld op **system standaard**.
6. Op de **bouwen** tabblad **shell uitvoeren** en voer de opdracht `npm install` om ervoor te zorgen dat alle afhankelijkheden moeten worden bijgewerkt.


## <a name="configure-jenkins-for-team-services-integration"></a>Jenkins configureren voor integratie met Team Services

> [!NOTE]
> Zorg ervoor dat het persoonlijke toegangstoken (PAT) u voor de volgende stappen gebruikt bevat de *Release* (lezen, schrijven, uitvoeren en beheren) toestemming in Team Services.
 
1.  Maak een PAT in uw Team Services-account als u er nog geen hebt. Jenkins vereist dat deze gegevens voor toegang tot uw Team Services-account. Zorg ervoor dat de token informatie opgeslagen voor toekomstige stappen in deze sectie.
  
    Lees meer informatie over het genereren van een token, [hoe maak ik een persoonlijk toegangstoken voor VSTS en TFS?](https://www.visualstudio.com/docs/setup-admin/team-services/use-personal-access-tokens-to-authenticate).
2. In de **na build acties** tabblad **na build actie toevoegen**. Selecteer **archiveren van de artefacten**.
3. Voor **bestanden te archiveren**, voer `**/*` alle bestanden opnemen.
4. Voor het maken van een andere actie selecteert **na build actie toevoegen**.
5. Selecteer **activeren release in TFS/Team Services**. Voer de URI voor uw account Team Services, zoals **https://{your-account-name}.visualstudio.com**.
6. Voer de **teamproject** naam.
7. Kies een naam voor de release-definitie. (U maakt deze release-definitie later in Team Services.)
8. Kies de referenties voor verbinding met uw Team Services of Team Foundation Server-omgeving:
   - Laat **gebruikersnaam** leeg als u Team Services gebruikt. 
   - Voer een gebruikersnaam en wachtwoord in als u een on-premises versie van Team Foundation Server.    
   ![Jenkins na build acties configureren](media/tutorial-build-deploy-jenkins/trigger-release-from-jenkins.png)
5. Het project Jenkins opslaan.


## <a name="create-a-jenkins-service-endpoint"></a>Een Jenkins service-eindpunt maken

Een service-eindpunt kunt Team Services verbinding maken met Jenkins.

1. Open de **Services** pagina in een Team Services, open de **nieuwe Service-eindpunt** lijst, en selecteer **Jenkins**.
   ![Een eindpunt Jenkins toevoegen](media/tutorial-build-deploy-jenkins/add-jenkins-endpoint.png)
2. Voer een naam voor de verbinding.
3. Voer de URL van uw server Jenkins en selecteer de **accepteren van niet-vertrouwde certificaten voor SSL** optie. Een voorbeeld-URL is **http://{YourJenkinsURL}.westcentralus.cloudapp.azure.com**.
4. Geef de gebruikersnaam en wachtwoord voor uw account Jenkins.
5. Selecteer **verbinding controleren** om te controleren of de informatie juist is.
6. Selecteer **OK** om de service-eindpunt te maken.

## <a name="create-a-deployment-group-for-azure-virtual-machines"></a>Maak een implementatiegroep voor Azure virtual machines

U moet een [implementatiegroep](https://www.visualstudio.com/docs/build/concepts/definitions/release/deployment-groups/) de Team Services-agent registreren zodat de definitie van de versie kan worden geïmplementeerd met uw virtuele machine. Implementatiegroepen maakt het eenvoudig om logische groepen doelmachines voor implementatie te definiëren en de vereiste agent installeren op elke machine.

   > [!NOTE]
   > In de volgende procedure moet u voor het installeren van de vereisten en *het script niet uitvoeren met sudo-machtigingen.*

1. Open de **Releases** tabblad van de **bouwen &amp; Release** hub, open **implementatiegroepen**, en selecteer **+ nieuw**.
2. Voer een naam voor de implementatiegroep en een optionele beschrijving. Selecteer vervolgens **Maken**.
3. Kies het besturingssysteem voor de virtuele doelmachine van uw implementatie. Selecteer bijvoorbeeld **Ubuntu 16.04 +**.
4. Selecteer **gebruiken een persoonlijk toegangstoken in het script voor verificatie**.
5. Selecteer de **systeemvereisten** koppeling. De vereisten voor het besturingssysteem installeren.
6. Selecteer **script kopiëren naar Klembord** kopiëren van het script.
7. Aanmelden bij uw implementatie doel-virtuele machine en voer het script. Het script niet worden uitgevoerd met sudo-bevoegdheden.
8. Na de installatie wordt u gevraagd voor implementatie group-tags. Accepteer de standaardinstellingen.
9. Schakel in het Team Services voor uw zojuist geregistreerde virtuele machine in **doelen** onder **Implementatiegroepen**.

## <a name="create-a-team-services-release-definition"></a>Een definitie van de release Team Services maken

De definitie van een release Hiermee geeft u het proces dat Team Services gebruikt om de app te implementeren. In dit voorbeeld moet u een shell-script uitvoeren.

De definitie van de release in Team Services maken:

1. Open de **Releases** tabblad van de **bouwen &amp; Release** hub en selecteer **maken release definitie**. 
2. Selecteer de **leeg** sjabloon door te beginnen met kiezen een **leeg proces**.
3. In de **artefacten** sectie **+ artefacten toevoegen** en kies **Jenkins** voor **gegevensbrontype**. Selecteer uw verbinding Jenkins service-eindpunt. Selecteer de taak van de bron Jenkins en schakel **toevoegen**.
4. Schakel de drie puntjes naast **omgeving 1**. Selecteer **toevoegen groep implementatiefase**.
5. Kies uw implementatiegroep.
5. Selecteer  **+**  toevoegen van een taak voor het **groep implementatiefase**.
6. Selecteer de **Shell-Script** taak en selecteer **toevoegen**. De **Shell-Script** taak bevat de configuratie voor een script uit te voeren op elke server om te installeren van Node.js en de app te starten.
8. Voor **scriptpad**, voer **$(System.DefaultWorkingDirectory)/Fabrikam-Node/deployscript.sh**.
9. Selecteer **Geavanceerd**, en schakel vervolgens **werkmap opgeven**.
10. Voor **werkmap**, voer **$(System.DefaultWorkingDirectory) / Fabrikam knooppunt**.
11. Bewerk de naam van de definitie van de versie in de naam die u hebt opgegeven op de **na build acties** tabblad van de build in Jenkins. Jenkins vereist deze naam kunnen voor het activeren van een nieuwe release wanneer de bron-artefacten zijn bijgewerkt.
12. Selecteer **opslaan** en selecteer **OK** om op te slaan de release-definitie.

## <a name="execute-manual-and-ci-triggered-deployments"></a>Handmatige en CI geactiveerd implementaties uitvoeren

1. Selecteer **+ Release** en selecteer **maken Release**.
2. Selecteer de versie die u hebt voltooid in de gemarkeerde vervolgkeuzelijst en selecteer **wachtrij**.
3. Kies de release-koppeling in het pop-upbericht. Bijvoorbeeld: ' Release **Release 1** is gemaakt. "
4. Open de **logboeken** tabblad bekijkt u de uitvoer van de release-console.
5. Open de URL van een van de servers die u hebt toegevoegd aan de implementatiegroep van uw in uw browser. Voer bijvoorbeeld **http://{your-server-ip-address}**.
6. Ga naar de bron Git-opslagplaats en wijzigen van de inhoud van de **h1** in de kop van het bestand app/views/index.jade met gewijzigde tekst.
7. Uw wijzigingen worden doorgevoerd.
8. Na een paar minuten ziet u een nieuwe release gemaakt op de **Releases** pagina van het Team Services of Team Foundation Server. Open de release voor de implementatie plaatsvinden. Gefeliciteerd.

## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie maakt u de implementatie van een app in Azure met behulp van Jenkins voor samenstellen en Team Services voor release geautomatiseerd. U hebt geleerd hoe u:

> [!div class="checklist"]
> * Uw app in Jenkins maken.
> * Jenkins configureren voor integratie met Team Services.
> * Maak een implementatiegroep voor de virtuele machines in Azure.
> * Maak de definitie van een versie die u configureert u de virtuele machines en de app wordt geïmplementeerd.

Voor meer informatie over het implementeren van een licht (Linux, Apache, MySQL en PHP) stapelen, Ga naar de volgende zelfstudie.

> [!div class="nextstepaction"]
> [LAMP-stack implementeren](tutorial-lamp-stack.md)
