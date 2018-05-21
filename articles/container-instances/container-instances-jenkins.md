---
title: Gebruik Azure Containerexemplaren als een Jenkins bouwen agent
description: Informatie over het Azure Containerexemplaren gebruiken terwijl een Jenkins agent maakt.
services: container-instances
author: neilpeterson
manager: jeconnoc
ms.service: container-instances
ms.topic: article
ms.date: 04/20/2018
ms.author: nepeters
ms.openlocfilehash: 4df230c8306a3876e94a5e9ada5e7408f134ba26
ms.sourcegitcommit: b6319f1a87d9316122f96769aab0d92b46a6879a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/20/2018
---
# <a name="use-azure-container-instances-as-a-jenkins-build-agent"></a>Gebruik Azure Containerexemplaren als een Jenkins bouwen agent

Azure Container exemplaren (ACI) biedt een op aanvraag, burstable en geïsoleerde omgeving voor het uitvoeren van beperkte workloads. Vanwege deze kenmerken maakt ACI een geweldig platform voor het uitvoeren van Jenkins build taken op grote schaal. Dit artikel helpt bij de implementatie en het gebruik van een Jenkins-server die vooraf geconfigureerd met ACI als build-doel.

Zie voor meer informatie over Azure Containerexemplaren [over Azure Containerexemplaren][about-aci].

## <a name="deploy-a-jenkins-server"></a>Jenkins-server implementeren

1. Selecteer in de Azure-portal **maken van een resource** en zoek naar **Jenkins**. Selecteer het Jenkins aanbod aan een publisher van **Microsoft**, en selecteer vervolgens **maken**.

2. Voer de volgende informatie op de **basisbeginselen** vormen en selecteer vervolgens **OK**.

   - **Naam**: Voer een naam voor de implementatie Jenkins.
   - **Gebruikersnaam**: Voer een naam voor de beheerder van de Jenkins virtuele machine.
   - **Verificatietype**: raadzaam een openbare SSH-sleutel voor verificatie. Als u deze optie selecteert, plakken in een openbare SSH-sleutel moet worden gebruikt voor het aanmelden bij de Jenkins virtuele machine.
   - **Subscription**: selecteer een Azure-abonnement.
   - **Resourcegroep**: maak een nieuwe resourcegroep of selecteer een bestaande.
   - **Locatie**: Selecteer een locatie voor de Jenkins-server.

   ![Basisinstellingen voor Jenkins portal-implementatie](./media/container-instances-jenkins/jenkins-portal-01.png)

3. Op de **extra instellingen** vormen, voert u de volgende items:

   - **De grootte van**: de juiste sizing optie voor uw Jenkins virtuele machine.
   - **VM-schijftype**: Geef ofwel **HDD** (harde schijf) of **SSD** (solid-state drive) voor de Jenkins-server.
   - **Virtueel netwerk**: Selecteer de pijl als u wilt de standaardinstellingen wijzigen.
   - **Subnetten**: Selecteer de pijl, Controleer de informatie en selecteer **OK**.
   - **Openbaar IP-adres**: Selecteer de pijl Geef een aangepaste naam op voor het openbare IP-adres, het configureren van de SKU en het instellen van de methode voor het toewijzen.
   - **Domeinnaamlabel**: Geef een waarde op voor het maken van een volledig gekwalificeerde URL naar de Jenkins virtuele machine.
   - **Versietype Jenkins**: Selecteer het type van de gewenste versie in de opties: **LTS**, **wekelijks bouwen**, of **Azure geverifieerd**.

   ![Aanvullende instellingen voor Jenkins portal-implementatie](./media/container-instances-jenkins/jenkins-portal-02.png)

4. Selecteer voor service-principal integratie **Auto(MSI)** hebben [Azure Managed Service-identiteit] [ managed-service-identity] automatisch een verificatie-identiteit voor de Jenkins maken exemplaar. Selecteer **handmatige** uw eigen service principal-referenties op te geven.

5. Cloud-agents configureren een cloud-gebaseerde platform voor Jenkins taken bouwen. Selecteer voor dit artikel **ACI**. Met de ACI cloud-agent wordt elke Jenkins build-taak uitgevoerd in een exemplaar van de container.

   ![Cloud-instellingen voor Jenkins portal-implementatie](./media/container-instances-jenkins/jenkins-portal-03.png)

6. Wanneer u met de integratie-instellingen bent klaar, selecteert u **OK**, en selecteer vervolgens **OK** opnieuw op de validatiesamenvatting. Selecteer **maken** op de **gebruiksvoorwaarden** samenvatting. De server Jenkins duurt een paar minuten om te implementeren.

## <a name="configure-jenkins"></a>Jenkins configureren

1. Blader naar de resourcegroep Jenkins in de Azure portal, selecteer de Jenkins virtuele machine en noteer de DNS-naam.

   ![DNS-naam in de details over de Jenkins virtuele machine](./media/container-instances-jenkins/jenkins-portal-fqdn.png)

2. Blader naar de DNS-naam van de VM Jenkins en kopieer de SSH-tekenreeks geretourneerd.

   ![Jenkins aanmelding instructies met SSH-reeks](./media/container-instances-jenkins/jenkins-portal-04.png)

3. Open een terminalsessie op uw ontwikkelsysteem en plak in de SSH-tekenreeks in de vorige stap. Update `username` naar de gebruikersnaam die u hebt opgegeven bij de implementatie van de server Jenkins.

4. Na de sessie is verbonden, voer de volgende opdracht voor het ophalen van de eerste beheerderswachtwoord:

   ```
   sudo cat /var/lib/jenkins/secrets/initialAdminPassword
   ```

5. Laat de SSH-sessie en tunnel uitgevoerd en Ga naar http://localhost:8080 in een browser. De eerste beheerderswachtwoord plak in het vak en selecteer vervolgens **doorgaan**.

   ![Scherm 'Ontgrendelen Jenkins' met het selectievakje uit voor het administrator-wachtwoord](./media/container-instances-jenkins/jenkins-portal-05.png)

6. Selecteer **voorgestelde invoegtoepassingen installeren** aanbevolen Jenkins invoegtoepassingen gebruikt om alle te installeren.

   ![Scherm met 'Jenkins aanpassen' 'Voorgestelde invoegtoepassingen installeren' geselecteerd](./media/container-instances-jenkins/jenkins-portal-06.png)

7. Maak een beheerdersaccount van de gebruiker. Dit account wordt gebruikt voor het aanmelden bij en werken met uw Jenkins-exemplaar.

   !['De eerste gebruiker die beheerder maken' scherm met de referenties die zijn ingevuld](./media/container-instances-jenkins/jenkins-portal-07.png)

8. Selecteer **opslaan en voltooien**, en selecteer vervolgens **aan de slag met Jenkins** om de configuratie te voltooien.

Jenkins is nu geconfigureerd en gereed om te bouwen en implementeren van code. In dit voorbeeld wordt een eenvoudige Java-toepassing gebruikt ter illustratie van een build Jenkins op exemplaren van Azure-Container.

## <a name="create-a-build-job"></a>Een build-taak maken

Wanneer u een kopie van de container als een Jenkins doel bouwen, moet u een afbeelding met alle tooling nodig is voor een geslaagde build opgeven. De installatiekopie opgeven:

1. Selecteer **beheren Jenkins** > **System configureren** en schuif omlaag naar de **Cloud** sectie. Bijvoorbeeld, werk de waarde van de afbeelding Docker naar **microsoft/java-op-azure-jenkins-slave**.

   Wanneer u bent klaar, selecteert u **opslaan** om terug te keren naar het dashboard Jenkins.

   ![De cloudconfiguratie Jenkins](./media/container-instances-jenkins/jenkins-aci-image.png)

2. Nu een Jenkins build-taak maken. Selecteer **Nieuw Item**, het build-project, zoals een naam geven **aci-java-demo**, selecteer **Freestyle project**, en selecteer **OK**.

   ![Vak voor de naam van de build-taak en de lijst met projecttypen](./media/container-instances-jenkins/jenkins-new-job.png)

3. Onder **algemene**, zorg ervoor dat **beperken waar dit project kan worden uitgevoerd** is geselecteerd. Voer **linux** voor de labelexpressie. Deze configuratie zorgt ervoor dat deze build-taak wordt uitgevoerd in de cloud ACI.

   !['Algemene' tabblad met configuratiedetails](./media/container-instances-jenkins/jenkins-job-01.png)

4. Onder **Source Code Management**, selecteer **Git** en voer **https://github.com/spring-projects/spring-petclinic.git** voor de URL van de opslagplaats. Deze GitHub-opslagplaats bevat de voorbeeldcode van de toepassing.

   ![Tabblad 'Bron Code Management' met code-informatie van gegevensbron](./media/container-instances-jenkins/jenkins-job-02.png)

5. Onder **bouwen**, selecteer **toevoegen build stap** en selecteer **aanroepen op het hoogste niveau Maven doelen**. Voer **pakket** als het doel van de stap build.

   ![Tabblad 'Maken' met de selecties voor de build-stap](./media/container-instances-jenkins/jenkins-job-03.png)

6. Selecteer **Opslaan**.

## <a name="run-the-build-job"></a>Voer de build-taak

Als u wilt testen van de taak build en houd rekening met de Containerexemplaren Azure als het build-platform, een build handmatig te starten.

1. Selecteer **nu bouwen** om een taak build te starten. Het duurt enkele minuten duren voordat de taak is gestart. U ziet een status die vergelijkbaar is met de volgende afbeelding:

   ![Informatie "Bouwen geschiedenis" met de status van taak](./media/container-instances-jenkins/jenkins-job-status.png)

2. Terwijl de taak wordt uitgevoerd, de Azure portal openen en kijk in de resourcegroep Jenkins. U ziet dat er een exemplaar van de container is gemaakt. De taak Jenkins wordt binnen dit exemplaar uitgevoerd.

   ![Exemplaar van de container in de resourcegroep](./media/container-instances-jenkins/jenkins-aci.png)

3. Zoals Jenkins wordt meer taken dan het geconfigureerde aantal Jenkins Executor (standaard 2) uitgevoerd, worden meerdere containerexemplaren gemaakt.

   ![Nieuw gemaakte containerexemplaren](./media/container-instances-jenkins/jenkins-aci-multi.png)

4. Nadat alle build taken hebt voltooid, wordt de container-instanties zijn verwijderd.

   ![Resourcegroep met de containerexemplaren verwijderd](./media/container-instances-jenkins/jenkins-aci-none.png)

## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie over Jenkins op Azure, [Azure en Jenkins][jenkins-azure].

<!-- LINKS - internal -->
[about-aci]: ./container-instances-overview.md
[jenkins-azure]: ../jenkins/overview.md
[managed-service-identity]: ../active-directory/managed-service-identity/overview.md