---
title: 'Zelfstudie: DevOps met Azure Portal | Microsoft Docs'
description: Informatie over de verschillende DevOps-werkstromen in Azure Portal.
services: azure-portal
documentationcenter: 
author: mlearned
manager: douge
editor: mlearned
ms.assetid: 4f1c5bc1-c732-4d35-b5df-0fd68e547d38
ms.service: multiple
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: hero-article
ms.date: 06/05/2016
ms.author: mlearned
ms.openlocfilehash: b590fb06a3dba8aec66a380217269e1ca39bb5e7
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="tutorial-devops-with-the-azure-portal"></a>Zelfstudie: DevOps met Azure Portal
Het Azure-platform bevat tal van flexibele DevOps-werkstromen. In deze zelfstudie leert u hoe u de mogelijkheden van Azure Portal inzet om toepassingen te ontwikkelen, te testen en te implementeren, en om problemen in toepassingen op te lossen of actieve toepassingen te beheren. Deze zelfstudie is gericht op het volgende:

1. Een webtoepassing maken en continue implementatie inschakelen
2. Een app ontwikkelen en testen
3. Een app bewaken en problemen oplossen
4. Algemene taken voor toepassingsbeheer

## <a name="creating-a-web-app-and-enabling-continuous-deployment"></a>Een webtoepassing maken en continue implementatie inschakelen
Maak met [Azure App Service](https://azure.microsoft.com/services/app-service/) een webtoepassing die u in de rest van deze handleiding gaat gebruiken. In eerste instantie schakelt u continue implementatie in vanuit de opslagplaats van de broncode naar de actieve Azure-omgeving.

1. Aanmelden bij Azure Portal
2. Kies **App Services** &gt; **Pictogram toevoegen** en voer een naam in. Kies uw abonnement en maak een nieuwe resourcegroep die als de container voor de service zal fungeren.
   
   Met resourcegroepen kunt u diverse aspecten van de oplossing, zoals facturering, implementaties en controle, als één groep via [Azure Resource Manager](../azure-resource-manager/resource-group-overview.md) beheren.
   
   ![image1][image1]
3. Na enkele ogenblikken is de app-service gemaakt. Neem even de tijd om de verschillende menu-opties voor de service in de portal te verkennen.
   
   ![image2][image2]    
4. Klik op de URL. Let op de keuze aan hulpprogramma's en opslagplaatsen. U kunt ook de talen en frameworks van uw keuze gebruiken, waaronder .NET, Java en Ruby.
   
   ![image3][image3]    
5. Dankzij Azure Portal is continue implementatie een gemakkelijk proces dat uit slechts enkele eenvoudige stappen bestaat. Kies in Azure Portal instellingen met het pictogram voor de app-service die u zojuist hebt gemaakt.
   
   ![image4][image4]
   
   Blader vanuit de blade die wordt geopend, naar rechts en scrol naar het publicatiegedeelte.
   
   ![image5][image5]
6. Configureer vervolgens een aantal instellingen om continue implementatie voor de app in te schakelen. Klik op Implementatiebron en vervolgens op Bron kiezen. Let op de verschillende opties die u voor opslagplaatsbronnen hebt.
   
   ![image6][image6]
7. Kies voor dit voorbeeld GitHub. U kunt ook een andere opslagplaats kiezen en de autorisatiereferenties instellen.
   
   ![image7][image7]
8. Nadat de opslagplaats is geautoriseerd, kunt u kiezen welk project en welke vertakking u wilt implementeren. Hieronder ziet u een aantal fictieve voorbeelden.
   
   ![image8][image8]
9. Klik op OK nadat u het project en de vertakking hebt gekozen. Nu moeten er meldingen over een implementatie binnen beginnen te komen.
   
   ![image9][image9]
10. Ga terug naar GitHub om de webhook te zien die is gemaakt om de opslagplaats van het besturingselement te integreren met Azure. Dankzij Azure Portal bestaat integratie met GitHub uit slechts enkele eenvoudige stappen.
    
    ![image10][image10]
11. Om continue implementatie te demonstreren, gaat u snel wat inhoud aan de opslagplaats toevoegen. Voeg als eenvoudig voorbeeld een voorbeeldtekstbestand toe aan een GitHub-opslagplaats. U kunt .NET, Ruby, Python of een ander type toepassing met App Service gebruiken. Voeg gerust een tekstbestand, ASP.NET MVC-, Java- of Ruby-toepassing aan de gewenste opslagplaats toe.
    
    ![image11][image11]
12. Nadat de wijzigingen aan de opslagplaats zijn doorgegeven, ziet u in het meldingengebied van de portal dat er een nieuwe implementatie is gestart. Klik op Synchroniseren als u de wijzigingen niet snel genoeg ziet nadat u ze hebt doorgevoerd in de opslagplaats.
    
    ![image12][image12]
13. Als u op dit punt probeert de pagina voor de app-service te laden, wordt mogelijk de fout 403 weergegeven. In dit voorbeeld komt dat omdat er geen standaarddocumentconfiguratie is, zoals een bestand als index.htm of default.html. U kunt dit snel verhelpen met deze functies in Azure Portal.  Kies in Azure Portal de optie Instellingen &gt; Toepassingsinstellingen.
    
     ![image13][image13]
14. Er wordt nu een blade met toepassingsinstellingen geopend. Voer de naam van de pagina SamplePage.html in en klik op Opslaan. Neem even de tijd om de overige instellingen te verkennen.
    
    ![image14][image14]
15. Als u dit wilt, vernieuwt u de browser-URL om ervoor te zorgen dat de verwachte wijzigingen zeker worden weergegeven. In dit geval wordt de pagina nu gevuld met een eenvoudige tekst. Elke extra wijziging in de opslagplaats leidt tot een nieuwe automatische implementatie.
    
    ![image15][image15]
    
    Het inschakelen van continue implementatie met Azure Portal is heel eenvoudig. U kunt ook complexere releasepijplijnen maken en vele andere technieken met bestaand broncodebeheer en continue integratiesystemen gebruiken om in Azure te implementeren, zoals het gebruik van systemen voor automatisch build- en releasebeheer.

## <a name="develop-and-test-an-app"></a>Een app ontwikkelen en testen
Vervolgens brengt u enkele wijzigingen in de code aan en gaat u deze wijzigingen snel implementeren. U gaat ook een aantal prestatietests voor de webtoepassing instellen.

1. Kies in Azure Portal de optie App Services in het navigatiedeelvenster en ga naar uw App Service.
   
   ![image16][image16]
2. Klik op Hulpprogramma’s
   
   ![image17][image17]
3. Let op de categorie Ontwikkelen onder Hulpprogramma’s. Hier vindt u enkele nuttige hulpprogramma’s die u de mogelijkheid bieden met apps te werken zonder Azure Portal te verlaten. Klik op Console.
   
   ![image18][image18]
4. In het consolevenster kunt u live opdrachten geven voor uw app. Typ de opdracht dir en druk op Enter. Opdrachten waarvoor verhoogde bevoegdheden zijn vereist, werken echter niet.
   
   ![image19][image19]
5. Ga terug naar de categorie Ontwikkelen en kies Visual Studio Online. Opmerking: Visual Studio Online heet nu Visual Studio Team Services.
   
   ![image20][image20]
6. Schakel in-browser bewerken voor uw app in.
   
   ![image21][image21]
7. Er wordt een webservice-extensie voor uw app geïnstalleerd. Met extensies kunt u snel en eenvoudig functionaliteit aan apps in Azure toevoegen. Bekijk in de schermafbeelding hieronder enkele andere typen bestandsextensies die beschikbaar zijn.
   
   ![image22][image22]
8. Klik na installatie van de extensie Visual Studio Online op Start.
   
   ![image23][image23]
9. Er wordt nu een browsertabblad geopend waarop u rechtstreeks in de browser een ontwikkelings-IDE ziet. In het voorbeeld hieronder wordt Chrome gebruikt.
   
   ![image24][image24]
10. U kunt verschillende activiteiten uitvoeren. Zo kunt u bestanden bewerken, bestanden en mappen toevoegen, en inhoud van de live site downloaden. Voer een snelle bewerking uit in het bestand SamplePage.html.
    
    ![image25][image25]
11. De wijzigingen worden automatisch binnen enkele tellen opgeslagen. Als u terugkeert naar de pagina, kunt u de wijzigingen zien. Let op: dit soort live bewerkingen is doorgaans niet geschikt voor productieomgevingen. Met deze hulpprogramma’s kunt u echter heel gemakkelijk kleine wijzigingen aanbrengen in ontwikkel- en testomgevingen.
    
    ![image26][image26]
    
    ![image27][image27]
12. Ga terug naar de blade Hulpprogramma’s en klik onder de categorie Ontwikkelen op Prestatietests.
    
    ![image28][image28]
13. U moet een Team Services-account instellen. Zie [Een Team Services-account maken](https://www.visualstudio.com/docs/setup-admin/team-services/sign-up-for-visual-studio-team-services) voor meer informatie
14. Klik op Nieuw om een nieuwe prestatietest te maken.
    
    ![image29][image29]
    
    Configureer de verschillende waarden en klik onder in het dialoogvenster op Test uitvoeren om de prestatietest te starten.
    
    ![image30][image30]
    
    ![image31][image31]
15. Zodra de test is gestart, kunt u de status controleren.
    
    ![image32][image32]
    
    Als de test is voltooid, kunt u op het resultaat klikken om meer informatie weer te geven.
    
    ![image33][image33]
16. In dit voorbeeld hebt u een kleine test gemaakt. Er zijn dus niet veel gegevens om te analyseren, maar in deze weergave kunt u verschillende metrische gegevens zien en de test opnieuw uitvoeren. Met Azure Portal is het maken, uitvoeren en analyseren van webprestatietests een eenvoudig proces. In onderstaande schermafbeeldingen worden de prestatiegegevens weergegeven.
    
    ![image34][image34]
    
    ![image35][image35]
    
    ![image36][image36]

## <a name="monitoring-and-troubleshooting-an-app"></a>Een app bewaken en problemen oplossen
Azure biedt veel opties voor het bewaken van actieve toepassingen en het oplossen van problemen.

1. Kies Hulpprogramma’s in Azure Portal voor onze webtoepassing.
   
   ![image37][image37]
2. Let onder de categorie Problemen oplossen op alle mogelijke hulpprogramma's waarmee u potentiële problemen met een actieve app kunt oplossen. U kunt onder andere live HTTP-verkeer bewaken, zelfherstel inschakelen en logboeken weergeven.
   
   ![image38][image38]
3. Kies Metrische sitegegevens om snel een weergave te krijgen van bepaalde HTTP-codes.
   
   ![image39][image39]
4. Kies Diagnostics as a Service. Kies het toepassingstype en klik op Uitvoeren.
   
   ![image40][image40]
   
   De gegevens worden verzameld.
   
   ![image41][image41]
5. Kies het relevante logboek om potentiële problemen te diagnosticeren. Als u alle beschikbare gegevensopties, zoals HTTP-logboeken, wilt weergeven, moet u logboekregistratie inschakelen.
   
   ![image42][image42]
   
   Als u op het bestand Geheugendump klikt, kunt u het analyserapport DebugDiag downloaden en analyseren om potentiële problemen op te sporen.
   
   ![image43][image43]
6. Voor meer gegevens moet u aanvullende logboekregistratie inschakelen. Ga in Azure Portal naar de webtoepassing en kies Instellingen.
   
   ![image44][image44]
7. Blader omlaag naar de categorie Functies en kies Diagnostische logboeken.
   
      ![image45][image45]
8. Let op de verschillende opties voor logboekregistratie. Schakel Webserverlogboeken in en klik op Opslaan.
   
   ![image46][image46]
9. Ga terug naar het gebied Hulpprogramma's voor de app en kies Diagnostics as a service. Klik op Uitvoeren om de gegevensverzameling opnieuw uit te voeren.
   
   ![image47][image47]
10. Als de instelling HTTP-logboekregistratie is ingeschakeld, ziet u nu gegevens die zijn verzameld voor HTTP-logboeken.
    
    ![image48][image48]
11. Als u op het logboek voor HTML-logboekbestand klikt, wordt er een opgemaakt rapport geproduceerd dat u verder kunt onderzoeken.
    
    ![image49][image49]
12. Ga terug naar de sectie Hulpprogramma's in Azure Portal voor de app. Blader naar de sectie Hulpprogramma's en kies Procesverkenner.
    
    ![image50][image50]
13. Kies Procesverkenner als u gegevens over actieve processen wilt bekijken. Hieronder ziet u dat u vanuit Azure Portal processen gedetailleerder kunt bekijken en zelfs processen kunt stoppen.
    
    ![image51][image51]
    
    ![image52][image52]
14. Ga terug naar de blade Instellingen aan de linkerkant. Klik op Nieuw ondersteuningsverzoek.
    
    ![image53][image53]
15. Op de blade aan de rechterkant kunt u meer informatie over de problemen invullen, contactgegevens invoeren en zelfs diagnostische gegevens uploaden. Dankzij Azure Portal is het werken met Microsoft Ondersteuning een naadloze ervaring.
    
    ![image54][image54]
    
    ![image55][image55]
    
    Azure Portal biedt krachtige en vertrouwde hulpmiddelen waarmee u actieve toepassingen kunt bewaken en problemen kunt oplossen. U kunt ook snel actie ondernemen met taken als het recyclen van processen en het in- en uitschakelen van verschillende gegevensverzamelingen. U kunt zelfs professionele ondersteuning van Microsoft integreren.

## <a name="general-application-management"></a>Algemeen toepassingsbeheer
Bij het beheer van toepassingen moet u vaak tal van activiteiten uitvoeren, zoals de configuratie van back-upstrategieën, de implementatie en het beheer van id-providers, en de configuratie van op rollen gebaseerd toegangsbeheer. Net als bij de andere DevOps-ervaringen integreert het Azure-platform deze taken rechtstreeks in de portal.

1. U moet back-ups configureren om webtoepassingen te beschermen tegen gegevensverlies. Ga naar het gebied Instellingen van uw webtoepassing.
   
   ![image56][image56]
2. Blader op de blade aan de rechterkant naar de categorie Functies.
   
    ![image57][image57]
3. Kies Back-ups. Rechts wordt een blade geopend.
   
   ![image58][image58]
4. Klik op Configureren en kies een opslagaccount op de blade aan de rechterkant.
   
   ![image59][image59]
5. Nu kunt u een opslagcontainer voor uw back-ups maken en kiezen. Klik onder aan de blade op Maken. Selecteer de container.
   
   ![image60][image60]
6. Nadat u de container hebt gekozen, kunt u schema's configureren en back-ups voor uw databases configureren. Klik voor dit scenario op het pictogram Opslaan.
   
    ![image61][image61]
7. Na het opslaan gaat u weer naar de blade aan de linkerkant voor Back-ups. Klik op Nu back-up maken om een back-up te maken van de toepassing.
   
    ![image62][image62]
8. Binnen enkele tellen ziet u dat er een back-up wordt gemaakt. In onderstaande schermafbeelding ziet u de optie Nu herstellen.
   
    ![image63][image63]
9. Klik op Nu herstellen en bekijk de opties op de blade aan de rechterkant. U kunt een geschikte back-up kiezen en het systeem eenvoudig herstellen naar een eerdere status. Met Azure Portal kunt u snel een eenvoudige strategie voor herstel na noodgeval maken voor de app.
   
    ![image64][image64]
10. Ga terug naar de blade Instellingen aan de linkerkant en kies onder Functies de optie Verificatie/autorisatie.
    
     ![image65][image65]
11. Klik op de blade aan de rechterkant op App Service-verificatie. Let op de verschillende opties die u voor populaire providers kunt configureren.
    
     ![image66][image66]
12. Kies de gewenste provider en bekijk de opties voor het bereik. U kunt een App-id en App-geheim opgeven en snel Facebook-verificatie voor de app inschakelen. Azure Portal biedt verificatie als kant-en-klare oplossing voor apps.
    
     ![image67][image67]
13. Ga terug naar de blade Instellingen en kies onder de categorie Resourcebeheer de optie Gebruikers.
    
     ![image68][image68]
14. Bekijk op de blade aan de rechterkant de verschillende opties voor het toevoegen van rollen en gebruikers. Vanuit Azure Portal kunt u eenvoudig RBAC (op rollen gebaseerd toegangsbeheer) voor de toepassing beheren.
    
     ![image69][image69]

## <a name="summary"></a>Samenvatting
In deze zelfstudie hebt u kennisgemaakt met enkele krachtige mogelijkheden van het Azure-platform: het snel kunnen inschakelen van continue implementatie voor een webtoepassing, het uitvoeren van verschillende ontwikkel- en testactiviteiten, het bewaken van live toepassingen en het oplossen van problemen, en tot slot het beheren belangrijke strategieën, zoals herstel na noodgevallen, identiteiten en op rollen gebaseerd toegangsbeheer. Het Azure-platform biedt een geïntegreerde ervaring voor deze DevOps-werkstromen en u kunt efficiënt werken doordat u taken in context uitvoert.

## <a name="next-steps"></a>Volgende stappen
* Azure Resource Manager is belangrijk voor het inschakelen van DevOps op het Azure-platform.  Ga naar [Overzicht van Azure Resource Manager](../azure-resource-manager/resource-group-overview.md) voor meer informatie.
* Ga naar [Een app implementeren in Azure App Service](../app-service/app-service-deploy-local-git.md) voor meer informatie over de implementatie van Azure App Service

[image1]: ./media/tutorial-azureportal-devops/image1.png
[image2]: ./media/tutorial-azureportal-devops/image2.png
[image3]: ./media/tutorial-azureportal-devops/image3.png
[image4]: ./media/tutorial-azureportal-devops/image4.png
[image5]: ./media/tutorial-azureportal-devops/image5.png
[image6]: ./media/tutorial-azureportal-devops/image6.png
[image7]: ./media/tutorial-azureportal-devops/image7.png
[image8]: ./media/tutorial-azureportal-devops/image8.png
[image9]: ./media/tutorial-azureportal-devops/image9.png
[image10]: ./media/tutorial-azureportal-devops/image10.png
[image11]: ./media/tutorial-azureportal-devops/image11.png
[image12]: ./media/tutorial-azureportal-devops/image12.png
[image13]: ./media/tutorial-azureportal-devops/image13.png
[image14]: ./media/tutorial-azureportal-devops/image14.png
[image15]: ./media/tutorial-azureportal-devops/image15.png
[image16]: ./media/tutorial-azureportal-devops/image16.png
[image17]: ./media/tutorial-azureportal-devops/image17.png
[image18]: ./media/tutorial-azureportal-devops/image18.png
[image19]: ./media/tutorial-azureportal-devops/image19.png
[image20]: ./media/tutorial-azureportal-devops/image20.png
[image21]: ./media/tutorial-azureportal-devops/image21.png
[image22]: ./media/tutorial-azureportal-devops/image22.png
[image23]: ./media/tutorial-azureportal-devops/image23.png
[image24]: ./media/tutorial-azureportal-devops/image24.png
[image25]: ./media/tutorial-azureportal-devops/image25.png
[image26]: ./media/tutorial-azureportal-devops/image26.png
[image27]: ./media/tutorial-azureportal-devops/image27.png
[image28]: ./media/tutorial-azureportal-devops/image28.png
[image29]: ./media/tutorial-azureportal-devops/image29.png
[image30]: ./media/tutorial-azureportal-devops/image30.png
[image31]: ./media/tutorial-azureportal-devops/image31.png
[image32]: ./media/tutorial-azureportal-devops/image32.png
[image33]: ./media/tutorial-azureportal-devops/image33.png
[image34]: ./media/tutorial-azureportal-devops/image34.png
[image35]: ./media/tutorial-azureportal-devops/image35.png
[image36]: ./media/tutorial-azureportal-devops/image36.png
[image37]: ./media/tutorial-azureportal-devops/image37.png
[image38]: ./media/tutorial-azureportal-devops/image38.png
[image39]: ./media/tutorial-azureportal-devops/image39.png
[image40]: ./media/tutorial-azureportal-devops/image40.png
[image41]: ./media/tutorial-azureportal-devops/image41.png
[image42]: ./media/tutorial-azureportal-devops/image42.png
[image43]: ./media/tutorial-azureportal-devops/image43.png
[image44]: ./media/tutorial-azureportal-devops/image44.png
[image45]: ./media/tutorial-azureportal-devops/image45.png
[image46]: ./media/tutorial-azureportal-devops/image46.png
[image47]: ./media/tutorial-azureportal-devops/image47.png
[image48]: ./media/tutorial-azureportal-devops/image48.png
[image49]: ./media/tutorial-azureportal-devops/image49.png
[image50]: ./media/tutorial-azureportal-devops/image50.png
[image51]: ./media/tutorial-azureportal-devops/image51.png
[image52]: ./media/tutorial-azureportal-devops/image52.png
[image53]: ./media/tutorial-azureportal-devops/image53.png
[image54]: ./media/tutorial-azureportal-devops/image54.png
[image55]: ./media/tutorial-azureportal-devops/image55.png
[image56]: ./media/tutorial-azureportal-devops/image56.png
[image57]: ./media/tutorial-azureportal-devops/image57.png
[image58]: ./media/tutorial-azureportal-devops/image58.png
[image59]: ./media/tutorial-azureportal-devops/image59.png
[image60]: ./media/tutorial-azureportal-devops/image60.png
[image61]: ./media/tutorial-azureportal-devops/image61.png
[image62]: ./media/tutorial-azureportal-devops/image62.png
[image63]: ./media/tutorial-azureportal-devops/image63.png
[image64]: ./media/tutorial-azureportal-devops/image64.png
[image65]: ./media/tutorial-azureportal-devops/image65.png
[image66]: ./media/tutorial-azureportal-devops/image66.png
[image67]: ./media/tutorial-azureportal-devops/image67.png
[image68]: ./media/tutorial-azureportal-devops/image68.png
[image69]: ./media/tutorial-azureportal-devops/image69.png
