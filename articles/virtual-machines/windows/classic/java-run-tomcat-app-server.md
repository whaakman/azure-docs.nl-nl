---
title: Java-toepassingsserver uitgevoerd op een klassieke Azure-virtuele machine | Microsoft Docs
description: Deze zelfstudie maakt gebruik van resources die zijn gemaakt met het klassieke implementatiemodel en laat zien hoe een virtuele Windows-machine maken en configureren voor het uitvoeren van de Apache Tomcat-toepassingsserver.
services: virtual-machines-windows
documentationcenter: java
author: rmcmurray
manager: erikre
editor: 
tags: azure-service-management
ms.assetid: d627aa09-f7d6-4239-8110-f8fc5111b939
ms.service: virtual-machines-windows
ms.workload: web
ms.tgt_pltfrm: vm-windows
ms.devlang: Java
ms.topic: article
ms.date: 03/16/2017
ms.author: robmcm
ms.openlocfilehash: 6e02f42613808bcb13c0057e9f8fcc1c02273e77
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2017
---
# <a name="how-to-run-a-java-application-server-on-a-virtual-machine-created-with-the-classic-deployment-model"></a>Een Java-toepassingsserver uitvoeren op een virtuele machine die is gemaakt volgens het klassieke implementatiemodel
> [!IMPORTANT]
> Azure heeft twee verschillende implementatiemodellen voor het maken en werken met resources: [Resource Manager en Classic](../../../resource-manager-deployment-model.md). In dit artikel bevat informatie over met behulp van het klassieke implementatiemodel. U doet er verstandig aan voor de meeste nieuwe implementaties het Resource Manager-model te gebruiken. Zie voor een Resource Manager-sjabloon voor het implementeren van een webapp met Java 8 en Tomcat [hier](https://azure.microsoft.com/documentation/templates/201-web-app-java-tomcat/).

Met Azure, kunt u een virtuele machine zodat u de server gebruiken. Als u bijvoorbeeld kunt een virtuele machine uitgevoerd op Azure worden geconfigureerd voor het hosten van Java-toepassingsserver, zoals Apache Tomcat.

Na het voltooien van deze handleiding hebt u een goed begrip van hoe u een virtuele machine uitgevoerd op Azure maken en configureren voor het uitvoeren van Java-toepassingsserver. U leert en de volgende taken uitvoeren:

* Het maken van een virtuele machine waarvoor een Java Development Kit (JDK) al is geïnstalleerd.
* Klik hier voor meer informatie over het extern aanmelden met uw virtuele machine.
* Klik hier voor meer informatie over het installeren van een Java-toepassingsserver--Apache Tomcat--op de virtuele machine.
* Het maken van een eindpunt voor uw virtuele machine.
* Klik hier voor meer informatie over het openen van een poort in de firewall voor de toepassingsserver.

De resultaten van de installatie is voltooid in Tomcat uitgevoerd op een virtuele machine.

![Virtuele machine met Apache Tomcat][virtual_machine_tomcat]

[!INCLUDE [create-account-and-vms-note](../../../../includes/create-account-and-vms-note.md)]

## <a name="to-create-a-virtual-machine"></a>Een virtuele machine maken
1. Meld u aan bij [Azure Portal](https://portal.azure.com).  
2. Klik op **nieuw**, klikt u op **Compute**, klikt u vervolgens op **alle** in de **aanbevolen apps**.
3. Klik op **JDK**, klikt u op **JDK 8** in de **JDK** deelvenster.  
   Installatiekopieën van virtuele machine die ondersteuning bieden voor **JDK 6** en **JDK 7** beschikbaar zijn als u oudere toepassingen die niet gereed in JDK 8 uit te voeren.
4. Selecteer in het deelvenster JDK 8 **klassieke**, klikt u vervolgens op **maken**.
5. In de **basisbeginselen** blade:
   1. Geef een naam voor de virtuele machine.
   2. Voer een naam voor de beheerder in de **gebruikersnaam** veld. Houd er rekening mee deze naam en het bijbehorende wachtwoord dat volgt op in het volgende veld. U moet ze wanneer u extern op de virtuele machine aanmelden.
   3. Voer een wachtwoord in de **nieuw wachtwoord** veld en Bevestig het in de **wachtwoord bevestigen** veld. Dit wachtwoord is voor de Administrator-account.
   4. Selecteer de relevante **abonnement**.
   5. Voor de **resourcegroep**, klikt u op **nieuw** en voer de naam van een nieuwe resourcegroep. Of klik op **gebruik bestaande** en selecteer een van de beschikbare resourcegroepen.
   6. Selecteer een locatie waarin de virtuele machine zich, zoals bevindt **Zuid-centraal VS**.
6. Klik op **Volgende**.
7. In de **de grootte van de virtuele machine-installatiekopie** blade Selecteer **A1 standaard** of een andere juiste installatiekopie.
8. Klik op **Selecteren**.

9. In de **instellingen** blade, klikt u op **OK**. U kunt de standaardwaarden die is verstrekt door Azure gebruiken.  
10. In de **samenvatting** blade, klikt u op **OK**.

## <a name="to-remotely-sign-in-to-your-virtual-machine"></a>Extern aanmelden bij uw virtuele machine
1. Meld u aan bij [Azure Portal](https://portal.azure.com).
2. Klik op **virtuele machines (klassiek)**. Klik indien nodig op **meer services** in de linkerbenedenhoek onder de servicecategorieën. De **virtuele machines (klassiek)** vermelding wordt vermeld in de **Compute** groep.
3. Klik op de naam van de virtuele machine die u aanmelden wilt bij.
4. Nadat de virtuele machine is gestart, wordt in een menu aan de bovenkant van het deelvenster verbindingen toestaat.
5. Klik op **Verbinden**.
6. Reageren op de vragen naar behoefte verbinding maken met de virtuele machine. Normaal gesproken opslaan of open het RDP-bestand dat de verbindingsgegevens bevat. Mogelijk moet de url: poort als het laatste deel van de eerste regel van het RDP-bestand kopiëren en plakken in een toepassing voor externe aanmelding.

## <a name="to-install-a-java-application-server-on-your-virtual-machine"></a>Java-toepassingsserver installeren op uw virtuele machine
U kunt een Java-toepassingsserver kopiëren naar uw virtuele machine of kunt u een Java-toepassingsserver via een installatieprogramma installeren.

Deze zelfstudie wordt Tomcat als de Java-toepassingsserver installeren.

1. Wanneer u bent aangemeld met uw virtuele machine, open een browsersessie op [Apache Tomcat](http://tomcat.apache.org/download-80.cgi).
2. Dubbelklik op de koppeling voor **32-bits/64-bits Windows-Service Installer**. Met deze techniek kunnen installeert Tomcat als een Windows-service.
3. Wanneer u wordt gevraagd, kiest u het installatieprogramma uitvoeren.
4. Binnen de **Apache Tomcat Setup** wizard, volg de aanwijzingen voor het installeren van Tomcat. Voor de doeleinden van deze zelfstudie is het fijn accepteer de standaardwaarden. Wanneer u bereikt de **voltooien van de Wizard Setup van Apache Tomcat** in het dialoogvenster kunt u eventueel controleren **uitvoeren Apache Tomcat** hebben Tomcat nu starten. Klik op **voltooien** de Tomcat-installatieproces te voltooien.

## <a name="to-start-tomcat"></a>Tomcat starten

U kunt Tomcat handmatig starten door te openen vanaf de opdrachtprompt op de virtuele machine en de opdracht uit te voeren **net&nbsp;start&nbsp;Tomcat8**.

Wanneer Tomcat wordt uitgevoerd, kunt u Tomcat openen door te voeren van de URL <http://localhost: 8080> in de browser van de virtuele machine.

Om te zien Tomcat vanaf externe computers wordt uitgevoerd, moet u een eindpunt worden gemaakt en een poort openen.

## <a name="to-create-an-endpoint-for-your-virtual-machine"></a>Een eindpunt voor uw virtuele machine maken
1. Meld u aan bij [Azure Portal](https://portal.azure.com).
2. Klik op **virtuele machines (klassiek)**.
3. Klik op de naam van de virtuele machine die uw Java-toepassingsserver wordt uitgevoerd.
4. Klik op **Eindpunten**.
5. Klik op **Add**.
6. In de **eindpunt toevoegen** in het dialoogvenster:
   1. Geef een naam voor het eindpunt; bijvoorbeeld: **HttpIn**.
   2. Selecteer **TCP** voor het protocol.
   3. Geef **80** voor de openbare poort.
   4. Geef **8080** voor de particuliere poort.
   5. Selecteer **uitgeschakelde** zwevend IP-adres.
   6. Laat de toegangsbeheerlijst is.
   7. Klik op de **OK** om het dialoogvenster sluiten en het eindpunt te maken.

## <a name="to-open-a-port-in-the-firewall-for-your-virtual-machine"></a>Een poort openen in de firewall voor uw virtuele machine
1. Meld u aan met uw virtuele machine.
2. Klik op **Windows Start**.
3. Klik op **Configuratiescherm**.
4. Klik op **systeem en beveiliging**, klikt u op **Windows Firewall**, en klik vervolgens op **geavanceerde instellingen**.
5. Klik op **regels voor binnenkomende verbindingen**, en klik vervolgens op **nieuwe regel**.  
   ![Nieuwe regel binnenkomende verbindingen][NewIBRule]
6. Voor de **regeltype**, selecteer **poort**, en klik vervolgens op **volgende**.  
   ![Nieuwe regel voor binnenkomende verbindingen poort][NewRulePort]
7. Op de **protocollen en poorten** Schakel in het scherm **TCP**, geef **8080** als de **specifieke lokale poort**, en klik vervolgens op  **Volgende**.  
  ![Nieuwe regel binnenkomende verbindingen][NewRuleProtocol]
8. Op de **actie** Schakel in het scherm **de verbinding toestaan**, en klik vervolgens op **volgende**.
   ![Nieuwe regel voor binnenkomende verbindingen actie][NewRuleAction]
9. Op de **profiel** scherm **domein**, **persoonlijke**, en **openbare** zijn geselecteerd en klik vervolgens op **volgende**.
   ![Nieuwe regel voor binnenkomende verbindingen profiel][NewRuleProfile]
10. Op de **naam** scherm, Geef een naam voor de regel, zoals **HttpIn** (naam van de regel is niet vereist echter aan de naam van het eindpunt), en klik vervolgens op **voltooien**.  
    ![De naam van een nieuwe regel voor binnenkomende verbindingen][NewRuleName]

Op dit moment uw website Tomcat worden weergegeven op een externe browser. Typ een URL van het formulier in de browser adres venster  **http://*uw\_DNS\_naam*. cloudapp.net**, waarbij ***uw\_DNS\_naam*** de DNS-naam die u hebt opgegeven toen u de virtuele machine hebt gemaakt.

## <a name="application-lifecycle-considerations"></a>Toepassing lifecycle overwegingen
* Kan u uw eigen toepassing webarchief (WAR) maken en toe te voegen aan de **webapps** map. Bijvoorbeeld, een elementaire Java Service pagina (JSP) dynamic webproject maken en exporteren als een WAR-bestand. Kopieer vervolgens het WAR naar de Apache Tomcat **webapps** map op de virtuele machine en vervolgens uit te voeren in een browser.
* Wanneer de Tomcat-service is geïnstalleerd, is deze standaard ingesteld om handmatig te starten. U kunt deze automatisch wordt gestart met behulp van de module Services in te schakelen. Start de Services-module door te klikken op **Start Windows**, **Systeembeheer**, en vervolgens **Services**. Dubbelklik op de **Apache Tomcat** service en stel **opstarttype** naar **automatische**.

    ![Instellen van een service op automatisch starten][service_automatic_startup]

    Het voordeel dat automatisch wordt gestart Tomcat is dat het wordt gestart wanneer de virtuele machine opnieuw wordt opgestart (bijvoorbeeld nadat software-updates waarvoor opnieuw opstarten zijn geïnstalleerd).

## <a name="next-steps"></a>Volgende stappen
U kunt meer informatie over andere services die u wilt opnemen met uw Java-toepassingen (zoals Azure Storage, servicebus en SQL-Database). Bekijk de informatie die beschikbaar zijn op de [Java Developer Center](https://azure.microsoft.com/develop/java/).

[virtual_machine_tomcat]:media/java-run-tomcat-app-server/WA_VirtualMachineRunningApacheTomcat.png

[service_automatic_startup]:media/java-run-tomcat-app-server/WA_TomcatServiceAutomaticStart.png









[NewIBRule]:media/java-run-tomcat-app-server/NewInboundRule.png
[NewRulePort]:media/java-run-tomcat-app-server/NewRulePort.png
[NewRuleProtocol]:media/java-run-tomcat-app-server/NewRuleProtocol.png
[NewRuleAction]:media/java-run-tomcat-app-server/NewRuleAction.png
[NewRuleName]:media/java-run-tomcat-app-server/NewRuleName.png
[NewRuleProfile]:media/java-run-tomcat-app-server/NewRuleProfile.png


<!-- Deleted from the "To create an ednpoint for your virtual mache" 3/17/2017,
     to use the new portal.
6. In the **Add endpoint** dialog box, ensure **Add standalone endpoint** is selected, and then click **Next**.
7. In the **New endpoint details** dialog box:
-->
