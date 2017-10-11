---
title: Een Jenkins-server maken in Azure
description: Op basis van de sjabloon voor de Jenkins-oplossing een virtuele Linux-machine van Azure installeren en een Java-voorbeeldtoepassing bouwen.
author: mlearned
manager: douge
ms.service: multiple
ms.workload: web
ms.devlang: java
ms.topic: hero-article
ms.date: 08/21/2017
ms.author: mlearned
ms.custom: Jenkins
ms.openlocfilehash: 7bb74f297d52fb25171817175cce64187b397c38
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/29/2017
---
# <a name="create-a-jenkins-server-on-an-azure-linux-vm-from-the-azure-portal"></a>Vanuit Azure Portal een Jenkins-server maken op een Azure-VM met Linux

Deze Quick Start laat zien hoe u [Jenkins](https://jenkins.io) installeert op een virtuele machine met Ubuntu Linux met behulp van de hulpprogramma's en invoegtoepassingen die zijn geconfigureerd om te werken met Azure. Wanneer u klaar bent, beschikt u over een Jenkins-server die wordt uitgevoerd in Azure voor het bouwen van een Java-voorbeeld-app uit [GitHub](https://github.com).

## <a name="prerequisites"></a>Vereisten

* Een Azure-abonnement
* Toegang tot SSH vanaf de opdrachtregel van uw computer (zoals de Bash-shell of [PuTTY](http://www.putty.org/))

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="create-the-jenkins-vm-from-the-solution-template"></a>De virtuele machine met Jenkins maken van de oplossingssjabloon

Open in uw webbrowser de [installatiekopie voor Jenkins in de Marketplace](https://azuremarketplace.microsoft.com/marketplace/apps/azure-oss.jenkins?tab=Overview) en selecteer **Nu downloaden** aan de linkerkant van de pagina. Controleer de prijsinformatie en selecteer **Doorgaan**. Selecteer vervolgens **Maken** om de Jenkins-server te configureren in Azure Portal. 
   
![Dialoogvenster Azure-portal](./media/install-jenkins-solution-template/ap-create.png)

Vul de volgende velden in op het tabblad **Basisinstellingen configureren**:

![Basisinstellingen configureren](./media/install-jenkins-solution-template/ap-basic.png)

* Geef **Jenkins** op bij **Naam**.
* Voer een gebruikersnaam in bij **Gebruikersnaam**. De gebruikersnaam moet voldoen aan [specifieke vereisten](/azure/virtual-machines/linux/faq#what-are-the-username-requirements-when-creating-a-vm).
* Selecteer **Wachtwoord** bij **Verificatietype** en voer een wachtwoord in. Het wachtwoord moet een hoofdletter, een cijfer en één speciaal teken bevatten.
* Geef **myJenkinsResourceGroup** op voor **Resourcegroep**.
* Kies de [Azure-regio](https://azure.microsoft.com/regions/) **VS-Oost** in de vervolgkeuzelijst **Locatie**.

Selecteer **OK** om naar het tabblad **Extra opties configureren** te gaan. Geef een unieke domeinnaam op om de Jenkins-server te identificeren en selecteer **OK**.

![selecteer aanvullende opties](./media/install-jenkins-solution-template/ap-addtional.png)  

 Als de validatie is geslaagd, selecteert u **OK** op het tabblad **Samenvatting**. Selecteer ten slotte **Kopen** om de virtuele Jenkins-machine te maken. U ziet een melding in Azure Portal wanneer de server klaar is:   

![Melding dat de Jenkins-server klaar is](./media/install-jenkins-solution-template/jenkins-deploy-notification-ready.png)

## <a name="connect-to-jenkins"></a>Verbinding maken met Jenkins

Navigeer in uw webbrowser naar de virtuele machine (bijvoorbeeld http://jenkins2517454.eastus.cloudapp.azure.com/). De Jenkins-console is niet toegankelijk via onbeveiligde HTTP. Om die reden bevat de pagina instructies om de console veilig vanaf uw computer te gebruiken via een SSH-tunnel.

![Jenkins ontgrendelen](./media/install-jenkins-solution-template/jenkins-ssh-instructions.png)

Stel de tunnel op de pagina in met behulp van de opdracht `ssh` op de opdrachtregel. Vervang `username` hierbij door de naam van de gebruiker met beheerdersrechten van de virtuele machine die u eerder hebt gekozen bij het instellen van de virtuele machine op basis van de oplossingssjabloon.

```bash
ssh -L 127.0.0.1:8080:localhost:8080 jenkinsadmin@jenkins2517454.eastus.cloudapp.azure.com
```

Nadat u de tunnel hebt gestart, gaat u naar http://localhost:8080/ op uw lokale computer. 

Vraag het initiële wachtwoord op door de volgende opdracht uit te voeren op de opdrachtregel terwijl u via SSH bent verbonden met de Jenkins-VM.

```bash
`sudo cat /var/lib/jenkins/secrets/initialAdminPassword`.
```

Gebruik dit initiële wachtwoord om het Jenkins-dashboard voor de eerste keer te ontgrendelen.

![Jenkins ontgrendelen](./media/install-jenkins-solution-template/jenkins-unlock.png)

Selecteer **Install suggested plugins** op de volgende pagina en maak vervolgens een Jenkins-gebruiker met beheerdersrechten om toegang te krijgen tot het Jenkins-dashboard.

![Jenkins is klaar.](./media/install-jenkins-solution-template/jenkins-welcome.png)

De Jenkins-server is nu klaar voor het bouwen van code.

## <a name="create-your-first-job"></a>Uw eerste taak maken

Selecteer **Create new jobs** in de Jenkins-console, geef het project de naam **mySampleApp**, selecteer **Freestyle project** en selecteer **OK**.

![Een nieuwe taak maken](./media/install-jenkins-solution-template/jenkins-new-job.png) 

Selecteer het tabblad **Source Code Management**, schakel het keuzerondje **Git** in en voer in het veld **Repository URL** de volgende URL in: `https://github.com/spring-guides/gs-spring-boot.git`

![De Git-opslagplaats definiëren](./media/install-jenkins-solution-template/jenkins-job-git-configuration.png) 

Selecteer het tabblad **Build** en selecteer vervolgens **Add build step**, **Invoke Gradle script**. Selecteer **Use Gradle Wrapper** en voer vervolgens `complete` in bij **Wrapper location** en `build` bij **Tasks**.

![De Gradle-wrapper gebruiken om de code te bouwen](./media/install-jenkins-solution-template/jenkins-job-gradle-config.png) 

Selecteer **Advanced..** en voer vervolgens in het veld **Root Build script** de waarde `complete` in. Selecteer **Opslaan**.

![Geavanceerde instellingen opgeven in de stap voor het bouwen van de Gradle-wrapper](./media/install-jenkins-solution-template/jenkins-job-gradle-advances.png) 

## <a name="build-the-code"></a>De code bouwen

Selecteer **Build Now** om de code te compileren en een pakket te maken van de voorbeeld-app. Als de build is voltooid, selecteert u de koppeling **Workspace** voor het project.

![Bladeren naar de werkruimte om het JAR-bestand van de build te zoeken](./media/install-jenkins-solution-template/jenkins-access-workspace.png) 

Navigeer naar `complete/build/libs` en controleer of u daar het bestand `gs-spring-boot-0.1.0.jar` ziet, zodat u weet dat de build is gelukt. De Jenkins-server is nu gereed voor het bouwen van uw eigen projecten in Azure.

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Virtuele Azure-machines toevoegen als Jenkins-agents](jenkins-azure-vm-agents.md)
