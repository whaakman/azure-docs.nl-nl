---
title: Een web-app met een sjabloon - Azure Cosmos DB implementeren | Microsoft Docs
description: Informatie over het implementeren van een account voor Azure Cosmos DB, Azure App Service Web Apps en een Voorbeeldwebtoepassing met een Azure Resource Manager-sjabloon.
services: cosmos-db, app-service\web
author: mimig1
manager: jhubbard
editor: monicar
documentationcenter: 
ms.assetid: 087d8786-1155-42c7-924b-0eaba5a8b3e0
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/08/2016
ms.author: mimig
ms.custom: mvc
ms.openlocfilehash: 7ceb4bf97c29a18d6879af55615eea46037c51ce
ms.sourcegitcommit: 7136d06474dd20bb8ef6a821c8d7e31edf3a2820
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/05/2017
---
# <a name="deploy-azure-cosmos-db-and-azure-app-service-web-apps-using-an-azure-resource-manager-template"></a>Azure Cosmos DB en Azure App Service Web Apps met een Azure Resource Manager-sjabloon implementeren
Deze zelfstudie leert u hoe u een Azure Resource Manager-sjabloon implementeren en integreren met [Microsoft Azure Cosmos DB](https://azure.microsoft.com/services/cosmos-db/), een [Azure App Service](http://go.microsoft.com/fwlink/?LinkId=529714) WebApp en een Voorbeeldwebtoepassing.

Met Azure Resource Manager-sjablonen, kunt u eenvoudig automatiseren de implementatie en configuratie van uw Azure-resources.  Deze zelfstudie laat zien hoe een webtoepassing implementeert en Azure DB die Cosmos-accountgegevens verbinding automatisch worden geconfigureerd.

Na het voltooien van deze zelfstudie, kunt u zich de volgende vragen beantwoorden:  

* Hoe kan ik een Azure Resource Manager-sjabloon gebruiken om te implementeren en het integreren van een Azure DB die Cosmos-account en een web-app in Azure App Service?
* Hoe kan ik een Azure Resource Manager-sjabloon gebruiken om te implementeren en het integreren van een account voor Azure Cosmos DB, een web-app in App Service Web Apps en Web Deploy-toepassing?

<a id="Prerequisites"></a>

## <a name="prerequisites"></a>Vereisten
> [!TIP]
> Tijdens deze zelfstudie wordt niet vanuit gegaan ervaring met Azure Resource Manager-sjablonen of JSON, moet die u wilt wijzigen van de waarnaar wordt verwezen-sjablonen of de implementatie-opties, klikt u vervolgens kennis van elk van deze gebieden worden.
> 
> 

Voordat u de instructies in deze zelfstudie, zorg ervoor dat u het volgende hebt:

* Een Azure-abonnement. Azure is een platform op basis van abonnement.  Zie voor meer informatie over het verkrijgen van een abonnement [koopopties](https://azure.microsoft.com/pricing/purchase-options/), [lid biedt](https://azure.microsoft.com/pricing/member-offers/), of [gratis proefversie](https://azure.microsoft.com/pricing/free-trial/).

## <a id="CreateDB"></a>Stap 1: Download de sjabloonbestanden
Laten we beginnen met het downloaden van de sjabloonbestanden we in deze zelfstudie gebruiken.

1. Download de [een account voor Azure Cosmos DB, Web-Apps maken en implementeren van een voorbeeld van een toepassing demo](https://portalcontent.blob.core.windows.net/samples/DocDBWebsiteTodo.json) sjabloon naar een lokale map (bijvoorbeeld C:\Azure Cosmos DBTemplates). Deze sjabloon wordt een Azure DB die Cosmos-account, een App Service-web-app en een webtoepassing implementeren.  De webtoepassing verbinding maken met de Azure DB die Cosmos-account dat wordt deze ook automatisch geconfigureerd.
2. Download de [maken van een Azure DB die Cosmos-account en een voorbeeld van de Web-Apps](https://portalcontent.blob.core.windows.net/samples/DocDBWebSite.json) sjabloon naar een lokale map (bijvoorbeeld C:\Azure Cosmos DBTemplates). Deze sjabloon een Cosmos-DB Azure-account, een App Service web-app implementeert en toepassingsinstellingen van de site te gemakkelijk surface Azure Cosmos DB-verbindingsgegevens worden gewijzigd, maar bevat geen een webtoepassing.  

<a id="Build"></a>

## <a name="step-2-deploy-the-azure-cosmos-db-account-app-service-web-app-and-demo-application-sample"></a>Stap 2: Implementeer de Azure DB die Cosmos-account, de App Service web app en demo toepassing voorbeeld
Nu gaan we onze eerste sjabloon te implementeren.

> [!TIP]
> De sjabloon komt niet overeen voor de web-appnaam en de Azure DB die Cosmos-accountnaam ingevoerd onder een) geldig en b) beschikbaar zijn.  Het is raadzaam dat u de beschikbaarheid van de namen die u van plan bent om op te geven voordat u de implementatie controleren.
> 
> 

1. Meld u aan bij de [Azure Portal](https://portal.azure.com), klikt u op Nieuw en zoek naar 'Sjabloonimplementatie'.
    ![Schermafbeelding van de sjabloonimplementatie van gebruikersinterface](./media/create-website/TemplateDeployment1.png)
2. Selecteer het item van de implementatie van sjabloon en klikt u op **maken** ![schermafbeelding van de sjabloonimplementatie van gebruikersinterface](./media/create-website/TemplateDeployment2.png)
3. Klik op **template bewerken**, plak de inhoud van het sjabloonbestand DocDBWebsiteTodo.json en klikt u op **opslaan**.
   ![Schermafbeelding van de sjabloonimplementatie van gebruikersinterface](./media/create-website/TemplateDeployment3.png)
4. Klik op **parameters bewerken**waarden opgeven voor elk van de verplichte parameters en klikt u op **OK**.  De parameters zijn als volgt:
   
   1. SITENAAM: Hiermee geeft u de naam van de App Service-web-app en wordt gebruikt voor het opstellen van de URL die u gebruiken wilt voor toegang tot de web-app (bijvoorbeeld als u 'mydemodocdbwebapp' opgeeft, wordt de URL waarmee u toegang hebben tot de web-app mydemodocdbwebapp.azurewebsites.net worden).
   2. HOSTINGPLANNAME: Geeft de naam van de hosting App Service-abonnement maken.
   3. LOCATIE: Hiermee geeft u de Azure-locatie op waarin de Azure DB die Cosmos en web app om resources te maken.
   4. DATABASEACCOUNTNAME: Hiermee geeft u de naam van de Azure DB die Cosmos-account maken.   
      
      ![Schermafbeelding van de sjabloonimplementatie van gebruikersinterface](./media/create-website/TemplateDeployment4.png)
5. Kies een bestaande resourcegroep of geef een naam op voor het maken van een nieuwe resourcegroep Kies een locatie voor de resourcegroep.

    ![Schermafbeelding van de sjabloonimplementatie van gebruikersinterface](./media/create-website/TemplateDeployment5.png)
6. Klik op **juridische voorwaarden bekijken**, **aankoop**, en klik vervolgens op **maken** om te beginnen met de implementatie.  Selecteer **vastmaken aan dashboard** zodat de resulterende implementatie gemakkelijk zichtbaar zijn op uw startpagina van Azure portal.
   ![Schermafbeelding van de sjabloonimplementatie van gebruikersinterface](./media/create-website/TemplateDeployment6.png)
7. Wanneer de implementatie is voltooid, wordt de resourcegroepblade wordt geopend.
   ![Schermafbeelding van de resourcegroepblade](./media/create-website/TemplateDeployment7.png)  
8. Voor het gebruik van de toepassing gewoon gaat u naar de web-app-URL (in het bovenstaande voorbeeld kunnen de URL zou zijn http://mydemodocdbwebapp.azurewebsites.net).  Hier ziet u de volgende webtoepassing:
   
   ![Todo-voorbeeldtoepassing](./media/create-website/image2.png)
9. Opwekken en een aantal taken in de web-app maken en ga daarna terug naar de blade met resourcegroepen in de Azure portal. De bron van de Azure DB die Cosmos-account in de lijst met Resources op en klik op **Queryverkenner**.
    ![Schermafbeelding van de samenvatting lens met de web-app gemarkeerd](./media/create-website/TemplateDeployment8.png)  
10. Voer de standaardquery "Selecteer * van c ' en de resultaten controleren.  U ziet dat de query de JSON-weergave van de todo-items die u hebt gemaakt in stap 7 hierboven is opgehaald.  Gerust om te experimenteren met query's. Probeer bijvoorbeeld uitgevoerd Selecteer * van c WHERE c.isComplete = true te retourneren van alle todo-items die zijn gemarkeerd als voltooid.
    
    ![Schermafbeelding van de Queryverkenner en resultaten blades met de queryresultaten](./media/create-website/image5.png)
11. U kunt de ervaring van de Cosmos-DB Azure portal verkennen of wijzigen van de voorbeeldtoepassing Todo.  Wanneer u klaar bent, gaan we een andere sjabloon te implementeren.

<a id="Build"></a> 

## <a name="step-3-deploy-the-document-account-and-web-app-sample"></a>Stap 3: Het voorbeeld Document account en web-app implementeren
Nu gaan we onze tweede sjabloon te implementeren.  Deze sjabloon is nuttig om weer te geven hoe u kunt invoeren Azure Cosmos DB-verbindingsgegevens zoals account eindpunt en de hoofdsleutel in een web-app als toepassingsinstellingen of als een aangepaste verbindingsreeks. Bijvoorbeeld, misschien hebt u uw eigen webtoepassing die u wilt implementeren met een Azure DB die Cosmos-account en hebben de verbindingsgegevens automatisch ingevuld tijdens de implementatie.

> [!TIP]
> De sjabloon komt niet overeen voor de web-appnaam en de Azure DB die Cosmos-accountnaam ingevoerd onder een) geldig en b) beschikbaar zijn.  Het is raadzaam dat u de beschikbaarheid van de namen die u van plan bent om op te geven voordat u de implementatie controleren.
> 
> 

1. In de [Azure Portal](https://portal.azure.com), klikt u op Nieuw en zoek naar 'Sjabloonimplementatie'.
    ![Schermafbeelding van de sjabloonimplementatie van gebruikersinterface](./media/create-website/TemplateDeployment1.png)
2. Selecteer het item van de implementatie van sjabloon en klikt u op **maken** ![schermafbeelding van de sjabloonimplementatie van gebruikersinterface](./media/create-website/TemplateDeployment2.png)
3. Klik op **template bewerken**, plak de inhoud van het sjabloonbestand DocDBWebSite.json en klikt u op **opslaan**.
   ![Schermafbeelding van de sjabloonimplementatie van gebruikersinterface](./media/create-website/TemplateDeployment3.png)
4. Klik op **parameters bewerken**waarden opgeven voor elk van de verplichte parameters en klikt u op **OK**.  De parameters zijn als volgt:
   
   1. SITENAAM: Hiermee geeft u de naam van de App Service-web-app en wordt gebruikt voor het opstellen van de URL die u gebruiken wilt voor toegang tot de web-app (bijvoorbeeld als u 'mydemodocdbwebapp' opgeeft, wordt de URL waarmee u toegang hebben tot de web-app mydemodocdbwebapp.azurewebsites.net worden).
   2. HOSTINGPLANNAME: Geeft de naam van de hosting App Service-abonnement maken.
   3. LOCATIE: Hiermee geeft u de Azure-locatie op waarin de Azure DB die Cosmos en web app om resources te maken.
   4. DATABASEACCOUNTNAME: Hiermee geeft u de naam van de Azure DB die Cosmos-account maken.   
      
      ![Schermafbeelding van de sjabloonimplementatie van gebruikersinterface](./media/create-website/TemplateDeployment4.png)
5. Kies een bestaande resourcegroep of geef een naam op voor het maken van een nieuwe resourcegroep Kies een locatie voor de resourcegroep.

    ![Schermafbeelding van de sjabloonimplementatie van gebruikersinterface](./media/create-website/TemplateDeployment5.png)
6. Klik op **juridische voorwaarden bekijken**, **aankoop**, en klik vervolgens op **maken** om te beginnen met de implementatie.  Selecteer **vastmaken aan dashboard** zodat de resulterende implementatie gemakkelijk zichtbaar zijn op uw startpagina van Azure portal.
   ![Schermafbeelding van de sjabloonimplementatie van gebruikersinterface](./media/create-website/TemplateDeployment6.png)
7. Wanneer de implementatie is voltooid, wordt de resourcegroepblade wordt geopend.
   ![Schermafbeelding van de resourcegroepblade](./media/create-website/TemplateDeployment7.png)  
8. De resource-Web-App in de lijst met Resources op en klik op **toepassingsinstellingen** ![schermafbeelding van de resourcegroep](./media/create-website/TemplateDeployment9.png)  
9. Houd er rekening mee hoe er toepassingsinstellingen voor het eindpunt Azure Cosmos DB en elk van de hoofdsleutels Azure Cosmos DB aanwezig zijn.

    ![Schermopname van toepassingsinstellingen](./media/create-website/TemplateDeployment10.png)  
10. Gerust om door te gaan verkennen van de Azure Portal of voert u een van onze Cosmos Azure DB [voorbeelden](http://go.microsoft.com/fwlink/?LinkID=402386) om uw eigen Azure DB die Cosmos-toepassing te maken.

<a name="NextSteps"></a>

## <a name="next-steps"></a>Volgende stappen
Gefeliciteerd. U kunt Azure Cosmos DB App Service-web-app en een Voorbeeldwebtoepassing met behulp van Azure Resource Manager-sjablonen hebt geïmplementeerd.

* Voor meer informatie over Azure Cosmos DB, klikt u op [hier](http://azure.com/docdb).
* Voor meer informatie over Azure App Service Web apps, klikt u op [hier](http://go.microsoft.com/fwlink/?LinkId=325362).
* Voor meer informatie over Azure Resource Manager-sjablonen, klikt u op [hier](https://msdn.microsoft.com/library/azure/dn790549.aspx).

## <a name="whats-changed"></a>Wat is er gewijzigd
* Als u van Websites wilt overstappen op App Service, raadpleegt u de volgende handleiding: [Azure App Service en de invloed ervan op bestaande Azure Services](http://go.microsoft.com/fwlink/?LinkId=529714)

> [!NOTE]
> Als u aan de slag wilt met Azure App Service voordat u zich aanmeldt voor een Azure-account, gaat u naar [App Service uitproberen](http://go.microsoft.com/fwlink/?LinkId=523751). Hier kunt u direct een tijdelijke web-app maken in App Service. U hebt geen creditcard nodig en u gaat geen verplichtingen aan.
> 
> 

