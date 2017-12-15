---
title: Uw eerste Java-web-app in Azure maken
description: In dit artikel leest u hoe u door het implementeren van een eenvoudige Java-app leert hoe u web-apps uitvoert in App Service.
services: app-service\web
documentationcenter: 
author: rmcmurray
manager: routlaw
editor: 
ms.assetid: 8bacfe3e-7f0b-4394-959a-a88618cb31e1
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: java
ms.topic: quickstart
ms.date: 11/08/2017
ms.author: cephalin;robmcm
ms.custom: mvc, devcenter
ms.openlocfilehash: d44fff1e59198d662356c4d7739c05e538ba57b9
ms.sourcegitcommit: 62eaa376437687de4ef2e325ac3d7e195d158f9f
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/22/2017
---
# <a name="create-your-first-java-web-app-in-azure"></a>Uw eerste Java-web-app in Azure maken

Azure [Web Apps](app-service-web-overview.md) biedt een uiterst schaalbare webhostingservice met self-patchfunctie. In deze quickstart leert u hoe u een Java-web-app implementeert in App Service met behulp van de [Eclipse IDE voor Java EE-ontwikkelaars](http://www.eclipse.org/).

Wanneer u deze quickstart hebt voltooid en uw toepassing in een webbrowser bekijkt, ziet uw toepassing er ongeveer als volgt uit:

!['Hello Azure'! voorbeeld van web-app](./media/app-service-web-get-started-java/browse-web-app-1.png)

## <a name="prerequisites"></a>Vereisten

Deze onderdelen moeten zijn geïnstalleerd om deze quickstart te kunnen voltooien:

* De gratis <a href="http://www.eclipse.org/downloads/" target="_blank">Eclipse IDE voor Java EE-ontwikkelaars</a>. Deze quickstart maakt gebruik van Eclipse Neon.
* De <a href="/java/azure/eclipse/azure-toolkit-for-eclipse-installation" target="_blank">Azure Toolkit voor Eclipse</a>.

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="create-a-dynamic-web-project-in-eclipse"></a>Een dynamisch webproject maken in Eclipse

Selecteer in Eclipse **Bestand** > **Nieuw** > **Dynamisch webproject**.

Geef het project in het dialoogvenster **Nieuw dynamisch webproject** de naam **MyFirstJavaOnAzureWebApp** en selecteer **Voltooien**.
   
![Het dialoogvenster Nieuw dynamisch webproject](./media/app-service-web-get-started-java/new-dynamic-web-project-dialog-box.png)

### <a name="add-a-jsp-page"></a>Een JSP-pagina toevoegen

Geef Projectverkenner weer als dat nu niet het geval is.

![Java EE-werkruimte voor Eclipse](./media/app-service-web-get-started-java/pe.png)

Vouw in Projectverkenner het project **MyFirstJavaOnAzureWebApp** uit.
Klik met de rechtermuisknop op **WebContent** en selecteer vervolgens **Nieuw** > **JSP-bestand**.

![Menu voor een nieuw JSP-bestand in Projectverkenner](./media/app-service-web-get-started-java/new-jsp-file-menu.png)

In het dialoogvenster **Nieuw JSP-bestand**:

* Noem het bestand **index.jsp**.
* Selecteer **Voltooien**.

  ![Het dialoogvenster Nieuw JSP-bestand](./media/app-service-web-get-started-java/new-jsp-file-dialog-box-page-1.png)

Vervang in het bestand index.jsp het element `<body></body>` door de volgende code:

```jsp
<body>
<h1><% out.println("Hello Azure!"); %></h1>
</body>
```

Sla de wijzigingen op.

## <a name="publish-the-web-app-to-azure"></a>De web-app publiceren in Azure

Klik in Projectverkenner met de rechtermuisknop op het project en selecteer vervolgens **Azure** > **Publiceren als Azure Web App**.

![Het contextmenu Publiceren als Azure Web App](./media/app-service-web-get-started-java/publish-as-azure-web-app-context-menu.png)

Laat in het dialoogvenster **Azure-aanmelding** de optie **Interactief** ingeschakeld en selecteer **Aanmelden**.

Volg de instructies om u aan te melden.

### <a name="deploy-web-app-dialog-box"></a>Het dialoogvenster Web-app implementeren

Nadat u zich hebt aangemeld bij uw Azure-account, wordt het dialoogvenster **Web-app implementeren** weergegeven.

Selecteer **Maken**.

![Het dialoogvenster Web-app implementeren](./media/app-service-web-get-started-java/deploy-web-app-dialog-box.png)

### <a name="create-app-service-dialog-box"></a>Het dialoogvenster App Service maken

Het dialoogvenster **App Service** wordt weergegeven met standaardwaarden. De numerieke waarde **170602185241** in de volgende afbeelding is anders in het dialoogvenster dat u ziet.

![Het dialoogvenster App Service maken](./media/app-service-web-get-started-java/cas1.png)

In het dialoogvenster **App Service maken**:

* Gebruik de gegenereerde naam voor de web-app. Deze naam moet uniek zijn binnen Azure. De naam is onderdeel van het URL-adres voor de web-app. Als de web-app bijvoorbeeld de naam **MyJavaWebApp** heeft, is de URL *myjavawebapp.azurewebsites.net*.
* Gebruik standaard-webcontainer.
* Selecteer een Azure-abonnement.
* Op het tabblad **App Service-plan**:

  * **Nieuwe maken**: gebruik de standaardwaarde, wat de naam van het App Service-plan is.
  * **Locatie**: selecteer **West-Europa** of een locatie bij u in de buurt.
  * **Prijscategorie**: selecteer een gratis variant. Voor een functiebeschrijving zie [App Service-prijzen](https://azure.microsoft.com/pricing/details/app-service/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio).

   ![Het dialoogvenster App Service maken](./media/app-service-web-get-started-java/create-app-service-dialog-box.png)

[!INCLUDE [app-service-plan](../../includes/app-service-plan.md)]

### <a name="resource-group-tab"></a>Het tabblad Resourcegroep

Selecteer het tabblad **Resourcegroep**. Gebruik de gegenereerde standaardwaarde voor de resourcegroep.

![Het tabblad Resourcegroep](./media/app-service-web-get-started-java/create-app-service-resource-group.png)

[!INCLUDE [resource-group](../../includes/resource-group.md)]

Selecteer **Maken**.

<!--
### The JDK tab

Select the **JDK** tab. Keep the default, and then select **Create**.

![Create App Service plan](./media/app-service-web-get-started-java/create-app-service-specify-jdk.png)
-->

De web-app wordt gemaakt in Azure Toolkit en de voortgang wordt in een dialoogvenster weergegeven.

![Het voortgangsvenster voor het maken van een App Service](./media/app-service-web-get-started-java/create-app-service-progress-bar.png)

### <a name="deploy-web-app-dialog-box"></a>Het dialoogvenster Web-app implementeren

Selecteer **Implementeren in hoofdmap** in het dialoogvenster **Web-app implementeren**. Als u een app-service hebt in *wingtiptoys.azurewebsites.net* en u niet in de hoofdmap implementeert, wordt de web-app met de naam **MyFirstJavaOnAzureWebApp** geïmplementeerd in *wingtiptoys.azurewebsites.net/MyFirstJavaOnAzureWebApp*.

![Het dialoogvenster Web-app implementeren](./media/app-service-web-get-started-java/deploy-web-app-to-root.png)

In het dialoogvenster ziet u de selecties voor Azure, JDK en webcontainer.

Selecteer **Implementeren** om de web-app in Azure te publiceren.

Als het publiceren is voltooid, selecteert u de koppeling **Gepubliceerd** in het dialoogvenster **Azure-activiteitenlogboek**.

![Het dialoogvenster Azure-activiteitenlogboek](./media/app-service-web-get-started-java/aal.png)

Gefeliciteerd. De web-app is nu geïmplementeerd in Azure. 

!['Hello Azure'! voorbeeld van web-app](./media/app-service-web-get-started-java/browse-web-app-1.png)

## <a name="update-the-web-app"></a>De web-app bijwerken

Wijzig de tekst van de JSP-voorbeeldcode.

```jsp
<body>
<h1><% out.println("Hello again Azure!"); %></h1>
</body>
```

Sla de wijzigingen op.

Klik in Projectverkenner met de rechtermuisknop op het project en selecteer vervolgens **Azure** > **Publiceren als Azure Web App**.

Het dialoogvenster **Web-app implementeren** wordt geopend, met de app-service die u eerder hebt gemaakt. 

> [!NOTE] 
> Selecteer **Implementeren in hoofdmap** telkens wanneer u gaat publiceren. 
> 

Selecteer de web-app en selecteer **Implementeren** om de wijzigingen te publiceren.

Wanneer de koppeling **Publiceren** wordt weergegeven, selecteert u deze om naar de web-app te bladeren en de wijzigingen te bekijken.

## <a name="manage-the-web-app"></a>De web-app beheren

Ga naar <a href="https://portal.azure.com" target="_blank">Azure Portal</a> om de web-app te zien die u hebt gemaakt.

Selecteer **Resourcegroepen** in het linkermenu.

![Portalnavigatie naar resourcegroepen](media/app-service-web-get-started-java/rg.png)

Selecteer de resourcegroep. De pagina bevat de resources die u in deze quickstart hebt gemaakt.

![Resourcegroep myResourceGroup](media/app-service-web-get-started-java/rg2.png)

Selecteer de web-app (**webapp 170602193915** in de voorgaande afbeelding).

De pagina **Overzicht** wordt weergegeven. Deze pagina geeft u een overzicht van hoe de app presteert. Hier kunt u algemene beheertaken uitvoeren, zoals bladeren, stoppen, starten, opnieuw starten en verwijderen. De tabbladen aan de linkerkant van de pagina tonen de verschillende configuraties die u kunt openen. 

![App Service-pagina in Azure Portal](media/app-service-web-get-started-java/web-app-blade.png)

[!INCLUDE [clean-up-section-portal-web-app](../../includes/clean-up-section-portal-web-app.md)]

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Aangepast domein toewijzen](app-service-web-tutorial-custom-domain.md)
