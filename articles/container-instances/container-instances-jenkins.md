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
ms.openlocfilehash: dbe418db8fb3d73739a30b60703f15b3dc47b291
ms.sourcegitcommit: fa493b66552af11260db48d89e3ddfcdcb5e3152
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/23/2018
---
# <a name="use-azure-container-instances-as-a-jenkins-build-agent"></a>Gebruik Azure Containerexemplaren als een Jenkins bouwen agent

Azure containerexemplaren bieden een op aanvraag, burstable en geïsoleerde omgeving voor het uitvoeren van beperkte werkbelasting. Vanwege deze kenmerken zorg Azure containerexemplaren een geweldig platform voor het uitvoeren van Jenkins build taken op grote schaal. Dit document helpt bij de implementatie en het gebruik van een Jenkins-server die vooraf is geconfigureerd met ACI als build-doel.

Zie voor meer informatie over Azure Containerexemplaren [over Azure Containerexemplaren][about-aci].

## <a name="deploy-jenkins-server"></a>Jenkins server implementeren

Selecteer in de Azure-portal **maken van een resource** en zoek naar **Jenkins**. Selecteer het Jenkins aanbod aan een publisher van **Microsoft** en selecteer **maken**.

Voer de volgende informatie op het formulier basisbeginselen en klik op **OK** wanneer u klaar bent.

- **Naam** - naam voor de implementatie Jenkins.
- **Gebruikersnaam** -deze gebruikersnaam wordt gebruikt als de gebruiker met beheerdersrechten voor de Jenkins virtuele machine.
- **Verificatietype** - SSH openbare sleutel wordt aanbevolen. Indien geselecteerd, kopieer dan in een openbare SSH-sleutel moet worden gebruikt bij het aanmelden bij de Jenkins virtuele machine.
- **Abonnement** -Selecteer een Azure-abonnement.
- **Resourcegroep** : Maak een nieuwe of Selecteer een bestaande resourcegroep.
- **Locatie** -Selecteer een locatie voor de Jenkins-server.

![Basisinstellingen voor Jenkins portal-implementatie](./media/container-instances-jenkins/jenkins-portal-01.png)

Voer in het formulier de overige instellingen op de volgende items:

- **De grootte van** -formaat van de juiste optie voor uw Jenkins virtuele machine.
- **VM-schijftype** -harde schijven (harde schijf) of SSD (solid-state drive) opgeeft voor de Jenkins-server.
- **Virtueel netwerk** -(optioneel) Selecteer virtueel netwerk naar de standaardinstellingen wijzigen.
- **Subnetten** - subnetten te selecteren, Controleer de informatie en selecteer **OK**.
- **Openbaar IP-adres** -het openbare IP-adres te selecteren, kunt u een aangepaste naam geven, SKU en toewijzingsmethode configureren.
- **Domeinnaamlabel** -Geef een waarde op voor het maken van een volledig gekwalificeerde URL naar de Jenkins virtuele machine.
- **Versietype Jenkins** -Selecteer het type van de gewenste versie in de opties: TNS, wekelijkse build of Azure worden geverifieerd.

![Aanvullende Jenkins portal-implementatie-instellingen](./media/container-instances-jenkins/jenkins-portal-02.png)

Selecteer voor service-principal integratie **Auto(MSI)** hebben [Azure Managed Service-identiteit] [ managed-service-identity] automatisch een verificatie-identiteit voor het exemplaar Jenkins maken. Selecteer handmatig naar provider uw eigen service principal-referenties.

Cloud-agents configureren een cloud-gebaseerde platform voor Jenkins taken bouwen. Selecteer ACI voor dit document. Met de agent van de cloud ACI wordt elke Jenkins build-taak uitgevoerd in een exemplaar van Azure-Container.

![Jenkins portal implementatie-instellingen voor cloud-integratie](./media/container-instances-jenkins/jenkins-portal-03.png)

Wanneer u klaar met de integratie-instellingen, klikt u op **OK**, en vervolgens **OK** opnieuw op de validatiesamenvatting. Klik op **maken** op de voorwaarden van de samenvatting van gebruik. De server Jenkins duurt een paar minuten om te implementeren.

## <a name="configure-jenkins"></a>Jenkins configureren

Blader naar de resourcegroep Jenkins in de Azure portal, selecteer de Jenkins virtuele machine en noteer de DNS-naam.

![Jenkins aanmelding instructies](./media/container-instances-jenkins/jenkins-portal-fqdn.png)

De browser de DNS-naam van het Jenkins VM en kopieer de SSH-tekenreeks geretourneerd.

![Jenkins aanmelding instructies](./media/container-instances-jenkins/jenkins-portal-04.png)

Open een terminalsessie op uw ontwikkelsysteem en plakken in de SSH-tekenreeks in de vorige stap. 'Username' is bijgewerkt naar de gebruikersnaam die is opgegeven bij het implementeren van de server Jenkins.

Eenmaal zijn verbonden, voer de volgende opdracht voor het ophalen van de eerste beheerderswachtwoord.

```
sudo cat /var/lib/jenkins/secrets/initialAdminPassword
```

Laat de SSH-sessie en tunnel uitgevoerd en navigeer naar http://localhost:8080 in een browser. Plak de initiële beheerderswachtwoord in het veld zoals te zien is in de volgende afbeelding. Selecteer **doorgaan** wanneer u klaar bent.

![Jenkins ontgrendelen](./media/container-instances-jenkins/jenkins-portal-05.png)

Selecteer **voorgestelde invoegtoepassingen installeren** aanbevolen Jenkins invoegtoepassingen gebruikt om alle te installeren.

![Jenkins-invoegtoepassing installeren](./media/container-instances-jenkins/jenkins-portal-06.png)

Maak een nieuw gebruikersaccount van de beheerder. Dit account wordt gebruikt voor het aan te melden bij en werken met uw Jenkins-exemplaar.

![Jenkins gebruiker maken](./media/container-instances-jenkins/jenkins-portal-07.png)

Selecteer **opslaan en voltooien** wanneer u klaar bent, en vervolgens **aan de slag met Jenkins** om de configuratie te voltooien.

Jenkins is nu geconfigureerd en gereed om te bouwen en implementeren van code. In dit voorbeeld wordt een eenvoudige Java-toepassing gebruikt ter illustratie van een build Jenkins op exemplaren van Azure-Container.

## <a name="create-build-job"></a>Build-taak maken

Wanneer met de installatiekopie van een container als een Jenkins doel maken, moet u een afbeelding met alle tooling nodig is voor een geslaagde build opgeven. Als u wilt opgeven van de installatiekopie van het **beheren Jenkins** > **System configureren** en schuif omlaag naar de **Cloud** sectie. Bijvoorbeeld, werk de waarde van de afbeelding Docker naar `microsoft/java-on-azure-jenkins-slave`.

Hierna klikt u **opslaan** om terug te keren naar het dashboard Jenkins.

![De cloudconfiguratie Jenkins](./media/container-instances-jenkins/jenkins-aci-image.png)

Nu een Jenkins build-taak maken. Selecteer **Nieuw Item**, het build-project, zoals een naam geven `aci-java-demo`, selecteer **Freestyle project**, en klik op **OK**.

![Jenkins-taak maken](./media/container-instances-jenkins/jenkins-new-job.png)

Onder **algemene**, zorg ervoor dat **beperken waar dit project kan worden uitgevoerd** is geselecteerd. Voer `linux` voor de labelexpressie. Deze configuratie zorgt ervoor dat deze build-taak wordt uitgevoerd in de cloud ACI.

![Jenkins-taak maken](./media/container-instances-jenkins/jenkins-job-01.png)

Selecteer onder beheer van gegevensbronnen code, `Git` en voer `https://github.com/spring-projects/spring-petclinic.git` voor de URL van de opslagplaats. Deze GitHub-opslagplaats bevat de voorbeeldcode van de toepassing.

![Broncode aan Jenkins taak toevoegen](./media/container-instances-jenkins/jenkins-job-02.png)

Selecteer onder Build en **toevoegen van een stap build** en selecteer `Invoke top-level Maven targets`. Voer `package` als het doel van de stap build.

![Jenkins bouwen stap toevoegen](./media/container-instances-jenkins/jenkins-job-03.png)

Selecteer **opslaan** wanneer u klaar bent.

## <a name="run-the-build-job"></a>Voer de build-taak

Als u wilt testen van de taak build en houd rekening met de Containerexemplaren Azure als het build-platform, een build handmatig te starten.

Selecteer **nu bouwen** om een taak build te starten. Het duurt enkele minuten duren voordat de taak is gestart, wanneer uitgevoerd, ziet u de status is vergelijkbaar met de volgende afbeeldingen.

![Jenkins bouwen stap toevoegen](./media/container-instances-jenkins/jenkins-job-status.png)

Terwijl de taak wordt uitgevoerd, opent u de Azure-portal en bekijk de resourcegroep Jenkins. U ziet dat er een exemplaar van Azure-Container is gemaakt. Het is in dit exemplaar waarop de taak Jenkins wordt uitgevoerd.

![Jenkins builds in ACI](./media/container-instances-jenkins/jenkins-aci.png)

Zoals Jenkins wordt meer taken dan het geconfigureerde aantal Jenkins Executor (standaard 2) uitgevoerd, worden meerdere exemplaren van de Azure-Container gemaakt.

![Jenkins builds in ACI](./media/container-instances-jenkins/jenkins-aci-multi.png)

Zodra alle build-taken zijn voltooid, worden de instanties van de Azure-container verwijderd.

![Jenkins builds in ACI](./media/container-instances-jenkins/jenkins-aci-none.png)

## <a name="next-steps"></a>Volgende stappen

Voor meer informatie over Jenkins op Azure Zie [Azure en Jenkins][jenkins-azure].

<!-- LINKS - internal -->
[about-aci]: ./container-instances-overview.md
[jenkins-azure]: ../jenkins/overview.md
[managed-service-identity]: ../active-directory/managed-service-identity/overview.md