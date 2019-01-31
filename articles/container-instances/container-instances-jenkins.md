---
title: Gebruik Azure Container Instances als een Jenkins build-agent
description: Informatie over het gebruik van Azure Container Instances, zoals een Jenkins-build agent.
services: container-instances
author: dlepow
ms.service: container-instances
ms.topic: article
ms.date: 08/31/2018
ms.author: danlep
ms.openlocfilehash: 85b67fb4fa474bb00b5b7ca66580273671081cdf
ms.sourcegitcommit: 698a3d3c7e0cc48f784a7e8f081928888712f34b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/31/2019
ms.locfileid: "55478184"
---
# <a name="use-azure-container-instances-as-a-jenkins-build-agent"></a>Gebruik Azure Container Instances als een Jenkins build-agent

Azure Container Instances (ACI) biedt een on-demand, ' burstable ' en geïsoleerde omgeving voor het uitvoeren van beperkte workloads. Vanwege deze kenmerken kunt ACI een geweldig platform voor het Jenkins build-taken uitvoeren op grote schaal. Dit artikel helpt bij de implementatie en het gebruik van een Jenkins-server die vooraf is geconfigureerd met de ACI als het doel van een build.

Zie voor meer informatie over Azure Container Instances [over Azure Container Instances][about-aci].

## <a name="deploy-a-jenkins-server"></a>Een Jenkins-server implementeren

1. Selecteer in de Azure portal, **een resource maken** en zoek naar de **Jenkins**. Selecteer de Jenkins-aanbieding met een uitgever van **Microsoft**, en selecteer vervolgens **maken**.

2. Voer de volgende informatie op de **basisbeginselen** vormen en selecteer vervolgens **OK**.

   - **Naam**: Voer een naam voor de Jenkins-implementatie.
   - **Gebruikersnaam**: Voer een naam voor de gebruiker met beheerdersrechten van de Jenkins-virtuele machine.
   - **Verificatietype**: U wordt aangeraden een openbare SSH-sleutel voor verificatie. Als u deze optie selecteert, plak een openbare SSH-sleutel moet worden gebruikt voor het aanmelden bij de Jenkins-virtuele machine.
   - **Abonnement**: Selecteer een Azure-abonnement.
   - **Resourcegroep**: Maak een resourcegroep of selecteer een bestaande resourcegroep.
   - **Locatie**: Selecteer een locatie voor de Jenkins-server.

   ![Basisinstellingen voor de implementatie van de Jenkins-portal](./media/container-instances-jenkins/jenkins-portal-01.png)

3. Op de **extra instellingen** vormen, voert u de volgende items:

   - **Grootte**: Selecteer de optie geschikte grootte voor uw Jenkins-virtuele machine.
   - **Type VM-schijf**: Geef óf een **HDD** (harde schijf) of **SSD** (solid-state drive) voor de Jenkins-server.
   - **Virtueel netwerk**: Selecteer de pijl als u wilt de standaardinstellingen wijzigen.
   - **Subnetten**: Selecteer de pijl, controleert u de gegevens en selecteer **OK**.
   - **Openbaar IP-adres**: Selecteer de pijl Geef een aangepaste naam op voor het openbare IP-adres, het configureren van de SKU en het instellen van de methode voor het toewijzen.
   - **Domeinnaamlabel**: Geef een waarde voor het maken van een volledig gekwalificeerde URL naar de virtuele Jenkins-machine.
   - **Jenkins-releasetype**: Selecteer het type van de gewenste versie in de opties: **LTS**, **wekelijks bouwen**, of **Azure gecontroleerd**.

   ![Aanvullende instellingen voor de implementatie van de Jenkins-portal](./media/container-instances-jenkins/jenkins-portal-02.png)

4. Voor service-principal integratie, selecteert u **Auto(MSI)** hebben [beheerde identiteiten voor een Azure-resources] [ managed-identities-azure-resources] automatisch maken van een verificatie-identiteit voor de Jenkins-exemplaar. Selecteer **handmatig** voor uw eigen referenties van de service-principal.

5. Cloud-agents configureren een cloudplatform voor Jenkins build-taken. Selecteer voor dit artikel **ACI**. Met de ACI-agent, elke Jenkins build-taak wordt uitgevoerd in een containerexemplaar in de cloud.

   ![Cloud-integratie-instellingen voor de implementatie van de Jenkins-portal](./media/container-instances-jenkins/jenkins-portal-03.png)

6. Wanneer u klaar bent met de integratie-instellingen, selecteert u **OK**, en selecteer vervolgens **OK** op het overzicht van de validatie opnieuw. Selecteer **maken** op de **gebruiksvoorwaarden** samenvatting. De Jenkins-server duurt een paar minuten om te implementeren.

## <a name="configure-jenkins"></a>Jenkins configureren

1. In Azure portal, blader naar de Jenkins-resourcegroep, selecteer de virtuele machine van Jenkins en Let op de DNS-naam.

   ![DNS-naam in de details over de virtuele machine van Jenkins](./media/container-instances-jenkins/jenkins-portal-fqdn.png)

2. Blader naar de DNS-naam van de Jenkins-VM en kopieer de SSH-tekenreeks geretourneerd.

   ![Jenkins-aanmelding instructies met SSH-tekenreeks](./media/container-instances-jenkins/jenkins-portal-04.png)

3. Open een terminal-sessie op uw systeem voor de ontwikkeling en plak in de SSH-tekenreeks van de laatste stap. Update `username` naar de gebruikersnaam die u hebt opgegeven bij de implementatie van de Jenkins-server.

4. Na de sessie is verbonden, voer de volgende opdracht uit om op te halen van het eerste beheerderswachtwoord:

   ```
   sudo cat /var/lib/jenkins/secrets/initialAdminPassword
   ```

5. Laat de SSH-sessie en tunnel uitgevoerd en gaat u naar http://localhost:8080 in een browser. Plak het eerste beheerderswachtwoord in het vak en selecteer vervolgens **doorgaan**.

   ![Scherm met het selectievakje in voor het beheerderswachtwoord "Jenkins ontgrendelen"](./media/container-instances-jenkins/jenkins-portal-05.png)

6. Selecteer **Installeer voorgestelde invoegtoepassingen** voor het installeren van alle aanbevolen invoegtoepassingen voor Jenkins.

   ![Scherm met 'Jenkins aanpassen' 'Install suggested plugins' geselecteerd](./media/container-instances-jenkins/jenkins-portal-06.png)

7. Maak een beheerdersaccount van de gebruiker. Dit account wordt gebruikt voor aanmelden bij en werken met uw Jenkins-exemplaar.

   !['Eerste gebruiker met beheerdersrechten maken' scherm, met de referenties die zijn ingevuld](./media/container-instances-jenkins/jenkins-portal-07.png)

8. Selecteer **Save and Finish**, en selecteer vervolgens **Start met behulp van Jenkins** om de configuratie te voltooien.

Jenkins is nu geconfigureerd en gereed om te bouwen en implementeren van code. In dit voorbeeld wordt een eenvoudige Java-toepassing gebruikt ter illustratie van een Jenkins-build van Azure Container Instances.

## <a name="create-a-build-job"></a>Een build-taak maken

Een Jenkins build-taak wordt nu gemaakt om te demonstreren dat jenkins bouwt voort op een Azure container-exemplaar.

1. Selecteer **Nieuw Item**, geef het build-project een naam zoals **aci-demo**, selecteer **Freestyle project**, en selecteer **OK**.

   ![Vak voor de naam van de build-taak en de lijst met projecttypen](./media/container-instances-jenkins/jenkins-new-job.png)

2. Onder **algemene**, zorg ervoor dat **beperken waar dit project kan worden uitgevoerd** is geselecteerd. Voer **linux** voor de expressie label. Deze configuratie zorgt ervoor dat deze build-taak wordt uitgevoerd op de ACI-cloud.

   ![Tabblad 'Algemeen' met informatie over de configuratie](./media/container-instances-jenkins/jenkins-job-01.png)

3. Onder **bouwen**, selecteer **build-stap toevoegen** en selecteer **Shell uitvoeren**. Voer `echo "aci-demo"` als de opdracht.

   ![Tabblad 'Build' met de selecties voor de build-stap](./media/container-instances-jenkins/jenkins-job-02.png)

5. Selecteer **Opslaan**.

## <a name="run-the-build-job"></a>De build-taak uitvoeren

Als u wilt testen van de build-taak en u ziet Azure Container Instances als de build-platform, moet u handmatig een build starten.

1. Selecteer **Build Now** om een build-taak te starten. Het duurt een paar minuten voor de taak wordt gestart. U ziet een status die vergelijkbaar is met de volgende afbeelding:

   ![Gegevens met de status van taak "Build-geschiedenis"](./media/container-instances-jenkins/jenkins-job-status.png)

2. Terwijl de taak wordt uitgevoerd, opent u Azure portal en bekijk de Jenkins-resourcegroep. U ziet dat er een containerexemplaar is gemaakt. De Jenkins-taak wordt uitgevoerd binnen dit exemplaar.

   ![Containerinstantie in de resourcegroep](./media/container-instances-jenkins/jenkins-aci.png)

3. Als Jenkins wordt meer taken dan het geconfigureerde aantal Jenkins Executor (standaard 2) uitgevoerd, worden meerdere containerexemplaren gemaakt.

   ![Containerinstanties van een nieuw gemaakt](./media/container-instances-jenkins/jenkins-aci-multi.png)

4. Nadat alle build-taken hebt voltooid, wordt de containerinstanties zijn verwijderd.

   ![Resourcegroep met container instances is verwijderd](./media/container-instances-jenkins/jenkins-aci-none.png)

## <a name="troubleshooting-the-jenkins-plugin"></a>Problemen met de Jenkins-invoegtoepassing oplossen

Als u problemen ondervindt met de Jenkins-invoegtoepassingen, kunt u in [Jenkins JIRA](https://issues.jenkins-ci.org/) een ticket openen voor het specifieke onderdeel.

## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie over Jenkins op Azure, [Azure en Jenkins][jenkins-azure].

<!-- LINKS - internal -->
[about-aci]: ./container-instances-overview.md
[jenkins-azure]: ../jenkins/overview.md
[managed-identities-azure-resources]: ../active-directory/managed-identities-azure-resources/overview.md