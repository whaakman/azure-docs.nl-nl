---
title: Zelfstudie - CI/CD van Jenkins voor Azure VM's met Team Services | Microsoft Docs
description: In deze zelfstudie leert u hoe u continue integratie (CI) en continue implementatie (CD) van een Node.js-app kunt instellen met Jenkins voor Azure VM's vanuit Release Management in Visual Studio Team Services of Microsoft Team Foundation Server
author: ahomer
manager: douge
editor: tysonn
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: tutorial
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 10/19/2017
ms.author: ahomer
ms.custom: mvc
ms.openlocfilehash: fc301edf13f8e6874f0b77440e2b0dc01b2a55fc
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/28/2018
---
# <a name="tutorial-deploy-your-app-to-linux-virtual-machines-in-azure-with-using-jenkins-and-visual-studio-team-services"></a>Zelfstudie: Uw app implementeren voor Linux Virtual Machines in Azure met Jenkins en Visual Studio Team Services

Continue integratie en continue implementatie vormen een pijplijn waarmee u uw code kunt maken, vrijgeven en implementeren. Visual Studio Team Services bevat een volledige set met CI/CD automatiseringsfuncties voor de implementatie naar Azure. Jenkins is een populair extern CI/CD-serverhulpprogramma dat ook CI/CD-automatisering bevat. U kunt Team Services en Jenkins samen gebruiken als u de manier waarop u uw cloud-app of -service levert, wilt aanpassen.

In deze zelfstudie gebruikt u Jenkins om een Node.js-web-app te maken. Vervolgens gebruikt u Team Services of Team Foundation Server om deze te implementeren in een [implementatiegroep](https://www.visualstudio.com/docs/build/concepts/definitions/release/deployment-groups/) met virtuele Linux-machines (VM's). In deze zelfstudie leert u procedures om het volgende te doen:

> [!div class="checklist"]
> * De voorbeeld-app downloaden.
> * Jenkins-invoegtoepassingen configureren.
> * Een Jenkins Freestyle-project configureren voor Node.js.
> * Jenkins configureren voor de integratie met Team Services.
> * Een Jenkins-service-eindpunt maken.
> * Een implementatiegroep voor de virtuele Azure-machines maken.
> * Een Team Services-releasedefinitie maken.
> * Handmatige en door CI geactiveerde implementaties uitvoeren.

## <a name="before-you-begin"></a>Voordat u begint

* U hebt toegang nodig tot een Jenkins-server. Zie [Een Jenkins-hoofdserver maken op een virtuele Azure-machine](https://docs.microsoft.com/azure/jenkins/install-jenkins-solution-template) als u nog geen Jenkins-server hebt gemaakt. 

* Aanmelden bij uw Team Services-account (**https://{uwaccount}.visualstudio.com**). 
  U kunt een [gratis Team Services-account](https://go.microsoft.com/fwlink/?LinkId=307137&clcid=0x409&wt.mc_id=o~msft~vscom~home-vsts-hero~27308&campaign=o~msft~vscom~home-vsts-hero~27308) krijgen.

  > [!NOTE]
  > Zie [Verbinding maken met Team Services](https://www.visualstudio.com/docs/setup-admin/team-services/connect-to-visual-studio-team-services) voor meer informatie.

*  U hebt een virtuele Linux-machine nodig voor een implementatiedoel.  Zie [Virtuele Linux-machines maken en beheren met de Azure CLI](https://docs.microsoft.com/azure/virtual-machines/linux/tutorial-manage-vm) voor meer informatie.

*  Open de binnenkomende poort 80 voor uw virtuele machine. Zie [Netwerkbeveiligingsgroepen maken met Azure Portal](https://docs.microsoft.com/azure/virtual-network/virtual-networks-create-nsg-arm-pportal) voor meer informatie.

## <a name="get-the-sample-app"></a>De voorbeeld-app downloaden

U hebt een app, die is opgeslagen in een Git-opslagplaats, nodig voor het implementeren.
Voor deze zelfstudie raden we u aan gebruik te maken van [deze voorbeeld-app die beschikbaar is via GitHub](https://github.com/azooinmyluggage/fabrikam-node). Deze zelfstudie bevat een voorbeeldscript dat wordt gebruikt voor het installeren van Node.js en een toepassing. Als u met uw eigen opslagplaats wilt werken, moet u een vergelijkbaar voorbeeld configureren.

Maak een fork van deze app en noteer de locatie (URL) voor gebruik in latere stappen van deze zelfstudie. Zie [Een fork van een opslagplaats maken](https://help.github.com/articles/fork-a-repo/) voor meer informatie.    

> [!NOTE]
> De app is gemaakt met [Yeoman](http://yeoman.io/learning/index.html). Hierin is gebruikgemaakt van Express, Bower en Grunt. Daarnaast bevat deze een aantal npm-pakketten als afhankelijkheden.
> Het voorbeeld bevat ook een script waarmee Nginx wordt ingesteld en de app wordt geïmplementeerd. Deze wordt uitgevoerd op de virtuele machines. Met name met het script:
> 1. Worden Node, Nginx en PM2 geïnstalleerd.
> 2. Worden Nginx en PM2 geconfigureerd.
> 3. Wordt de Node-app gestart.

## <a name="configure-jenkins-plug-ins"></a>Jenkins-invoegtoepassingen configureren

Eerst moet u twee Jenkins-invoegtoepassingen configureren: **NodeJS** en **Continue implementatie van VS Team Services**.

1. Open uw Jenkins-account en selecteer **Jenkins beheren**.
2. Selecteer op de pagina **Jenkins beheren** de optie **Invoegtoepassingen beheren**.
3. Filter de lijst om de invoegtoepassing **NodeJS** te zoeken en selecteer de optie **installeren zonder opnieuw opstarten**.
    ![De invoegtoepassing NodeJS toevoegen aan Jenkins](media/tutorial-build-deploy-jenkins/jenkins-nodejs-plugin.png)
4. Filter de lijst om de invoegtoepassing **Continue implementatie van VS Team Services** te zoeken en selecteer de optie **Installeren zonder opnieuw opstarten**.
5. Ga terug naar het Jenkins-dashboard en selecteer **Jenkins beheren**.
6. Selecteer **Configuratie van globaal hulpprogramma**. Zoek naar **NodeJS** en selecteer **NodeJS-installaties**.
7. Selecteer de optie **Automatisch installeren** en geef een waarde voor **Naam** op.
8. Selecteer **Opslaan**.

## <a name="configure-a-jenkins-freestyle-project-for-nodejs"></a>Een Jenkins Freestyle-project configureren voor Node.js

1. Selecteer **Nieuw Item**. Geef een itemnaam op.
2. Selecteer **Freestyle-project**. Selecteer **OK**.
3. Selecteer op het tabblad **Broncodebeheer** de optie **Git** en geef de details op van de opslagplaats en de vertakking met uw app-code.    
    ![Een opslagplaats toevoegen aan uw build](media/tutorial-build-deploy-jenkins/jenkins-git.png)
4. Selecteer op het tabblad **Triggers maken** de optie **SCM pollen** en geef het schema `H/03 * * * *` op om de Git-opslagplaats om de drie minuten op wijzigingen te pollen. 
5. Selecteer op het tabblad **Omgeving maken** de optie **Knooppunt opgeven &amp;npm bin/mappad** en selecteer de waarde **NodeJS-installatie**. Laat **npmrc-bestand** ingesteld op **systeemstandaard gebruiken**.
6. Selecteer op de tabblad **Maken** de optie **Shell uitvoeren** en typ de opdracht `npm install` om ervoor te zorgen dat alle afhankelijkheden worden bijgewerkt.


## <a name="configure-jenkins-for-team-services-integration"></a>Jenkins configureren voor de integratie met Team Services

> [!NOTE]
> Zorg ervoor dat het persoonlijke toegangstoken (PAT) dat u gebruikt voor de volgende stappen, de machtiging *Release* (lezen, schrijven, uitvoeren en beheren) in Team Services bevat.
 
1.  Maak een PAT in uw Team Services-account als u dit nog niet hebt. In Jenkins is deze informatie vereist om toegang te krijgen tot uw Team Services-account. Zorg ervoor dat u de tokengegevens opslaat voor latere stappen in deze sectie.
  
    Lees [Hoe maak ik een persoonlijk toegangstoken voor VSTS en TFS?](https://www.visualstudio.com/docs/setup-admin/team-services/use-personal-access-tokens-to-authenticate) voor meer informatie over het genereren van een token.
2. Selecteer op het tabblad **Acties na maken** de optie **Actie na maken toevoegen**. Selecteer **De artefacten archiveren**.
3. Geef voor **Te archiveren bestanden** de optie `**/*` op om alle bestanden op te nemen.
4. Selecteer **Actie na maken toevoegen** als u nog een actie wilt maken.
5. Selecteer **Release activeren in TFS/Team Services**. Geef de URI voor uw Team Services-account op, zoals **https://{uwaccountnaam}.visualstudio.com**.
6. Geef de naam van het **teamproject** op.
7. Kies een naam voor de releasedefinitie. (U maakt deze releasedefinitie later in Team Services.)
8. Kies de referenties om verbinding te maken met uw Team Services of Team Foundation Server-omgeving:
   - Laat **Gebruikersnaam** leeg als u Team Services gebruikt. 
   - Geef een gebruikersnaam en wachtwoord op als u een on-premises versie van Team Foundation Server gebruikt.    
   ![Jenkins-acties na maken configureren](media/tutorial-build-deploy-jenkins/trigger-release-from-jenkins.png)
5. Sla het Jenkins-project op.


## <a name="create-a-jenkins-service-endpoint"></a>Een Jenkins-service-eindpunt maken

Via een service-eindpunt kan Team Services verbinding maken met Jenkins.

1. Open de pagina **Services** in Team Services, open de lijst **Nieuw service-eindpunt** en selecteer **Jenkins**.
   ![Een Jenkins-eindpunt toevoegen](media/tutorial-build-deploy-jenkins/add-jenkins-endpoint.png)
2. Geef een naam voor de verbinding op.
3. Geef de URL van uw Jenkins-server op en selecteer de optie **Niet-vertrouwde SSL-certificaten accepteren**. Een voorbeeld-URL is **http://{UwJenkinsURL}.westcentralus.cloudapp.azure.com**.
4. Geef de gebruikersnaam en het wachtwoord voor uw Jenkins-account op.
5. Selecteer **Verbinding verifiëren** om te controleren of de informatie juist is.
6. Selecteer **OK** om het service-eindpunt te maken.

## <a name="create-a-deployment-group-for-azure-virtual-machines"></a>Een implementatiegroep voor de virtuele Azure-machines maken

U hebt een [implementatiegroep](https://www.visualstudio.com/docs/build/concepts/definitions/release/deployment-groups/) nodig om de Team Services-agent te registreren, zodat de releasedefinitie kan worden geïmplementeerd op uw virtuele machine. Met implementatiegroepen kunt u eenvoudig logische groepen met doelmachines voor implementatie definiëren en de vereiste agent installeren op elke machine.

   > [!NOTE]
   > In de volgende procedure moet u ervoor zorgen dat u de vereiste onderdelen hebt geïnstalleerd en *moet u het script niet uitvoeren met sudo-bevoegdheden.*

1. Open het tabblad **Releases** van de hub **Release&amp; maken**, open **Implementatiegroepen** en selecteer **+ nieuw**.
2. Geef een naam voor de implementatiegroep en een optionele beschrijving op. Selecteer vervolgens **Maken**.
3. Kies het besturingssysteem voor de virtuele machine van uw implementatiedoel. Selecteer bijvoorbeeld **Ubuntu 16.04+**.
4. Selecteer **Een persoonlijk toegangstoken in het script gebruiken voor verificatie**.
5. Selecteer de koppeling **Systeemvereisten**. Installeer de vereisten voor uw besturingssysteem.
6. Selecteer **Script kopiëren naar Klembord** om het script te kopiëren.
7. Meld u aan bij uw virtuele machine van uw implementatiedoel en voer het script uit. Voer het script niet uit met sudo-bevoegdheden.
8. Na de installatie wordt u gevraagd om implementatiegroepstags. Accepteer de standaardwaarden.
9. Controleer in Team Services uw zojuist geregistreerde virtuele machine in **Doelen** onder **Implementatiegroepen**.

## <a name="create-a-team-services-release-definition"></a>Een Team Services-releasedefinitie maken

Met een releasedefinitie geeft u het proces op dat in Team Services wordt gebruikt om de app te implementeren. In dit voorbeeld voert u een shellscript uit.

Een releasedefinitie in Team Services maken:

1. Open het tabblad **Releases** van de hub **Release &amp; maken** en selecteer **Releasedefinitie maken**. 
2. Selecteer de sjabloon **Leeg** door eerst een **Leeg proces** te kiezen.
3. Selecteer in de sectie **Artefacten** de optie **+ artefacten toevoegen** en kies **Jenkins** voor **Brontype**. Selecteer de verbinding voor uw Jenkins-service-eindpunt. Selecteer vervolgens de Jenkins-brontaak en selecteer **Toevoegen**.
4. Selecteer het beletselteken naast **Omgeving 1**. Selecteer **Implementatiegroepsfase toevoegen**.
5. Kies uw implementatiegroep.
5. Selecteer **+** als u een taak wilt toevoegen aan **Implementatiegroepsfase**.
6. Selecteer de taak **Shellscript** en selecteer **Toevoegen**. De taak **Shellscript** bevat de configuratie voor een script dat moet worden uitgevoerd op elke server om Node.js te installeren en de app te starten.
8. Voor **Scriptpad** typt u **$(System.DefaultWorkingDirectory)/Fabrikam-Node/deployscript.sh**.
9. Selecteer **Geavanceerd**, en schakel vervolgens **Werkmap opgeven** in.
10. Voor **Werkmap** typt u **$(System.DefaultWorkingDirectory)/Fabrikam-Node**.
11. Wijzig de naam van de releasedefinitie in de naam die u hebt opgegeven op het tabblad **Acties na maken** van de build in Jenkins. In Jenkins is deze naam vereist om een nieuwe release te activeren wanneer de bronartefacten zijn bijgewerkt.
12. Selecteer **Opslaan** en selecteer **OK** om de releasedefinitie op te slaan.

## <a name="execute-manual-and-ci-triggered-deployments"></a>Handmatige en door CI geactiveerde implementaties uitvoeren

1. Selecteer **+ release** en selecteer **Release maken**.
2. Selecteer de build die u hebt voltooid in de gemarkeerde vervolgkeuzelijst en selecteer **Wachtrij**.
3. Kies de releasekoppeling in het pop-upbericht. Bijvoorbeeld: 'Release **Release 1** is gemaakt'.
4. Open het tabblad **Logboeken** om de uitvoer van de releaseconsole te bekijken.
5. Open in uw browser de URL van een van de servers die u hebt toegevoegd aan de implementatiegroep. Typ bijvoorbeeld **http://{ip-adres-van-uw-server}**.
6. Ga naar de Git-bronopslagplaats en wijzig de inhoud van de kop **h1** in het bestand app/views/index.jade met gewijzigde tekst.
7. Voer uw wijziging door.
8. Na een paar minuten ziet u dat er een nieuwe release is gemaakt op de pagina **Releases** van Team Services of Team Foundation Server. Open de release om te zien welke implementatie wordt uitgevoerd. Gefeliciteerd.

## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie hebt u de implementatie van een app in Azure geautomatiseerd met Jenkins voor de build en Team Services voor de release. U hebt geleerd hoe u:

> [!div class="checklist"]
> * Uw app maakt in Jenkins.
> * Jenkins configureert voor de integratie met Team Services.
> * Een implementatiegroep maakt voor de virtuele Azure-machines.
> * Een releasedefinitie maakt waarmee u de virtuele machines configureert en de app wordt geïmplementeerd.

Ga naar de volgende zelfstudie voor meer informatie over het implementeren van een LAMP-stack (Linux, Apache, MySQL en PHP).

> [!div class="nextstepaction"]
> [LAMP-stack implementeren](tutorial-lamp-stack.md)
