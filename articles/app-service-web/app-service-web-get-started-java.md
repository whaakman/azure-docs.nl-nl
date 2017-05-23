---
title: Uw eerste Java-web-app in vijf minuten in Azure maken | Microsoft Docs
description: Ontdek door implementatie van een simpele Java-toepassing hoe eenvoudig het is om web-apps in App Service uit te voeren.
services: app-service\web
documentationcenter: 
author: rmcmurray
manager: erikre
editor: 
ms.assetid: 8bacfe3e-7f0b-4394-959a-a88618cb31e1
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: java
ms.topic: hero-article
ms.date: 04/17/2017
ms.author: cephalin;robmcm
ms.translationtype: Human Translation
ms.sourcegitcommit: 9ae7e129b381d3034433e29ac1f74cb843cb5aa6
ms.openlocfilehash: 75e51ca45a899c6b6fa123346aa3c5860fd1600d
ms.contentlocale: nl-nl
ms.lasthandoff: 05/08/2017


---
# <a name="create-your-first-java-web-app-in-azure-in-five-minutes"></a>Uw eerste Java-web-app in vijf minuten in Azure maken

[!INCLUDE [app-service-web-selector-get-started](../../includes/app-service-web-selector-get-started.md)] 

Met deze Quickstart leert u in een paar minuten hoe u uw eerste Java-web-app implementeert in [Azure App Service](../app-service/app-service-value-prop-what-is.md). Wanneer u klaar bent met deze zelfstudie, beschikt u over een eenvoudige op Java gebaseerde web-app die wordt uitgevoerd in de cloud.

![Naar de web-app bladeren](./media/app-service-web-get-started-java/browse-web-app-1.png)

## <a name="before-you-begin"></a>Voordat u begint

In deze zelfstudie leest u hoe u Eclipse IDE voor Java EE Developers gebruikt om een Java-web-app te maken en te implementeren in Azure. Als u Eclipse nog niet hebt geïnstalleerd, kunt u het programma gratis downloaden van http://www.eclipse.org/.

In de stappen van deze zelfstudie wordt gebruikgemaakt van de [Azure Toolkit voor Eclipse](/azure/azure-toolkit-for-eclipse). Dit vereenvoudigt de publicatie van Java-web-apps in Azure. Zie [Installing the Azure Toolkit for Eclipse](/azure/azure-toolkit-for-eclipse-installation) (De Azure Toolkit voor Eclipse installeren) voor meer informatie over het installeren van de toolkit.

> [!NOTE]
>
> U kunt ook [IntelliJ IDEE](https://www.jetbrains.com/idea/) van JetBrains gebruiken om de stappen in deze zelfstudie uit te voeren. Mogelijk wijken enkele stappen iets af voor deze ontwikkelomgeving, hoewel er ook een [Azure Toolkit voor IntelliJ](/azure/azure-toolkit-for-intellij) is die u kunt gebruiken om het publicatieproces voor die IDE te vereenvoudigen.
>

U hebt ook een Azure-abonnement nodig om de stappen in deze zelfstudie uit te voeren. Als u nog geen Azure-abonnement hebt, kunt u [uw voordelen als MSDN-abonnee activeren](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) of [u aanmelden voor een gratis Azure-account](https://azure.microsoft.com/pricing/free-trial/).

## <a name="create-a-dynamic-web-project-in-eclipse"></a>Een dynamisch webproject maken in Eclipse

Selecteer in Eclipse het menu **Bestand**, klikt vervolgens op **Nieuw** en dan op **Dynamisch webproject**.

Geef het project in het dialoogvenster **Nieuw dynamisch webproject** de naam **MyFirstJavaOnAzureWebApp** en selecteer **Voltooien**.
   
![Het dialoogvenster Dynamisch webproject](./media/app-service-web-get-started-java/new-dynamic-web-project-dialog-box.png)

> [!NOTE]
>
> Als u een lokale runtime-omgeving, zoals [Apache Tomcat](https://tomcat.apache.org/), hebt geïnstalleerd, kunt u dat opgeven in het veld **Doelruntime**.
>

Wanneer het dynamische webproject is gemaakt, voegt u een nieuwe JSP-pagina toe door het project uit te breiden. Daartoe klikt u in de Projectverkenner met de rechtermuisknop op de map **WebContent** en klikt u op **Nieuw** en vervolgens op **JSP-bestand**.

![Het menu Nieuw JSP-bestand](./media/app-service-web-get-started-java/new-jsp-file-menu.png)

In het dialoogvenster Nieuw JSP-bestand noemt u het bestand **index.jsp**, laat u de bovenliggende map ingesteld op **MyFirstJavaOnAzureWebApp/WebContent** en klikt u op **Volgende**.

![Het dialoogvenster Nieuw JSP-bestand](./media/app-service-web-get-started-java/new-jsp-file-dialog-box-page-1.png)

Op de tweede pagina van het dialoogvenster Nieuw JSP-bestand noemt u het bestand **index.jsp**, laat u de bovenliggende map ingesteld op **MyFirstJavaOnAzureWebApp/WebContent** en klikt u op **Voltooien**.

![Het dialoogvenster Nieuw JSP-bestand](./media/app-service-web-get-started-java/new-jsp-file-dialog-box-page-2.png)

Als de nieuwe pagina in Eclipse wordt geopend, vervangt u de bestaande sectie `<body></body>` door de volgende code:

```jsp
<body>
<h1><% out.println("Hello Azure!"); %></h1>
</body>
```

Sla de wijzigingen op de pagina op.

## <a name="publish-your-web-app-to-azure"></a>De web-app publiceren in Azure

Voor de implementatie van de web-app in Azure gaat u gebruikmaken van een aantal functies uit de Azure Toolkit voor Eclipse.

Gebruik een van de volgende methoden om het publicatieproces te starten:

* Klik met de rechtermuisknop in de **Projectverkenner** van Eclipse op uw project. Klik op **Azure** en vervolgens op **Publiceren als Azure Web App**.

   ![Het contextmenu van Publiceren als Azure Web App](./media/app-service-web-get-started-java/publish-as-azure-web-app-context-menu.png)

* Klik op het pictogram **Publiceren** op de werkbalk van Eclipse en klik vervolgens op **Publiceren als Azure Web App**.

   ![Het keuzemenu Publiceren als Azure Web App](./media/app-service-web-get-started-java/publish-as-azure-web-app-drop-down-menu.png)

Als u nog niet ben aangemeld bij uw Azure-account, wordt u gevraagd om dit te doen. Volg hiervoor de volgende stappen:

1. Er zijn twee verschillende opties om u aan te melden bij uw Azure-account. Voor deze zelfstudie kiest u **Interactief**.

   ![Het aanmeldingsvenster van Azure](./media/app-service-web-get-started-java/azure-signin-dialog-box.png)

1. Voer uw referenties voor Azure in en klik op **Aanmelden**.

   ![Het aanmeldingsvenster van Azure](./media/app-service-web-get-started-java/azure-login-dialog-box.png)

1. Kies uw Azure-abonnementen en klik op **Selecteren**.

   ![Het aanmeldingsvenster van Azure](./media/app-service-web-get-started-java/select-azure-subscriptions-dialog-box.png)

> [!NOTE]
>
> Gedetailleerde instructies voor de aanmeldopties **Interactief** en **Automatisch** vindt u in het artikel [Azure Sign In Instructions for the Azure Toolkit for Eclipse](https://go.microsoft.com/fwlink/?linkid=846174) (Azure-aanmeldinstructies voor de Azure Toolkit voor Eclipse).
>

Wanneer u bent aangemeld bij uw Azure-account, wordt het dialoogvenster **Web-app implementeren** weergegeven. Als dit de eerste keer is dat u een web-app in Azure publiceert, worden er nog geen App Services weergegeven. In dat geval, of als u een nieuwe App Service wilt maken, bestaat de volgende stap uit het maken van een nieuwe App Service. Daartoe klikt u op **Maken**.

![Het dialoogvenster Web App implementeren](./media/app-service-web-get-started-java/deploy-web-app-dialog-box.png)

Wanneer het dialoogvenster **App Service maken** wordt geopend, moet u eerst de volgende gegevens opgeven:

* Een unieke naam voor uw web-app: dit wordt het DNS-adres van uw web-app. Zo wordt **MyJavaWebApp** het adres *myjavawebapp.azurewebsites.net*.

* Welke webcontainer uw web-app gaat gebruiken, bijvoorbeeld **nieuwste Tomcat 8.5**.

* Uw Azure-abonnement.

   ![Het dialoogvenster App Service maken](./media/app-service-web-get-started-java/create-app-service-dialog-box.png)

Als u nog geen App Service-abonnement hebt of als u een nieuwe serviceabonnement wilt maken, moet u de volgende informatie opgeven:

* Een unieke naam voor het nieuwe serviceabonnement. Deze naam wordt weergegeven wanneer u later web-apps publiceert met de Azure Toolkit. Dit is ook de naam die in [Azure Portal](https://portal.azure.com) wordt weergegeven wanneer u uw account beheert.

* De geografische locatie waar uw serviceabonnement wordt gemaakt.

* De prijscategorie voor uw serviceabonnement.

   ![Een App Service-abonnement maken](./media/app-service-web-get-started-java/create-app-service-plan.png)

Klik vervolgens op het tabblad **Resourcegroep**. Als u nog geen resourcegroepen hebt of als u een nieuwe resourcegroep wilt maken, moet u een unieke naam voor de nieuwe resourcegroep opgeven. Kies anders een bestaande resourcegroep in de vervolgkeuzelijst.

![Een App Service-abonnement maken](./media/app-service-web-get-started-java/create-app-service-resource-group.png)

Klik tot slot op het tabblad **JDK**. Er worden diverse opties weergegeven waarmee ontwikkelaars JDK's (Java Developer Kits) van derden of aangepaste JDK's kunnen opgeven, maar voor deze zelfstudie kiest u de optie **Standaard**. Klik vervolgens op **Maken**.

![Een App Service-abonnement maken](./media/app-service-web-get-started-java/create-app-service-specify-jdk.png)

Nu wordt de nieuwe app-service in Azure Toolkit gemaakt. De voortgang wordt in een dialoogvenster weergegeven.

![De voortgangsbalk voor het maken van een App Service](./media/app-service-web-get-started-java/create-app-service-progress-bar.png)

Wanneer de nieuwe app-service is gemaakt, hoeft u alleen nog te kiezen of u de web-app wilt implementeren in de hoofdmap van de nieuwe website. Als u bijvoorbeeld een app-service hebt in *wingtiptoys.azurewebsites.net* en u niet in de hoofdmap implementeert, wordt uw web-app met de naam **MyFirstJavaOnAzureWebApp** geïmplementeerd in *wingtiptoys.azurewebsites.net/MyFirstJavaOnAzureWebApp*.

![Web-app implementeren in de hoofdmap](./media/app-service-web-get-started-java/deploy-web-app-to-root.png)

Wanneer u alle voorgaande stappen hebt voltooid, klikt u op **Implementeren** om de web-app te publiceren in Azure.

![Web-app implementeren in Azure](./media/app-service-web-get-started-java/deploy-web-app-to-azure.png)

Gefeliciteerd. U hebt uw web-app nu geïmplementeerd in Azure. Nu kunt u de web-app op de Azure-website bekijken:

![Naar de web-app bladeren](./media/app-service-web-get-started-java/browse-web-app-1.png)

## <a name="updating-your-web-app"></a>De web-app beveiligen

Wanneer u de web-app hebt gepubliceerd in Azure, is het bijwerken van de web-app veel eenvoudiger. De volgende stappen helpen u bij het publiceren van wijzigingen in de web-app.

Wijzig eerst de bestaande voorbeeld-JSP-code door de titel te vervangen door de datum van vandaag:

```jsp
<%@ page
    language="java"
    contentType="text/html; charset=ISO-8859-1"
    pageEncoding="ISO-8859-1"
    import="java.text.SimpleDateFormat"
    import="java.util.Date" %>
<!DOCTYPE html PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN" "http://www.w3.org/TR/html4/loose.dtd">
<html>
<head>
<meta http-equiv="Content-Type" content="text/html; charset=ISO-8859-1">
<% SimpleDateFormat date = new SimpleDateFormat("yyyy/MM/dd"); %>
<title><% out.println(date.format(new Date())); %></title>
</head>
<body>
<h1><% out.println("Hello Azure!"); %></h1>
</body>
</html>
```

Wanneer u de wijzigingen hebt opgeslagen, klikt u met de rechtermuisknop in de **Projectverkenner** van Eclipse. Klik op **Azure** en vervolgens op **Publiceren als Azure Web App**.

![Bijgewerkte web-app publiceren](./media/app-service-web-get-started-java/publish-updated-web-app-context-menu.png)

Wanneer het dialoogvenster **Web-app implementeren** wordt weergegeven, wordt uw app-service hierin vermeld. Als u de web-app wilt bijwerken, selecteert u de app-service en klikt u op **Implementeren** om uw wijzigingen te publiceren.

![Web-app implementeren in Azure](./media/app-service-web-get-started-java/deploy-web-app-to-azure.png)

> [!NOTE]
>
> Als u de web-app implementeert in de hoofdmap van uw app-service, moet u telkens wanneer u de wijzigingen publiceert, opnieuw de optie **Implementeren in hoofdmap** inschakelen.
>

Nadat de wijzigingen zijn gepubliceerd, ziet u in uw browser dat de paginatitel is gewijzigd in de datum van vandaag.

![Naar de web-app bladeren](./media/app-service-web-get-started-java/browse-web-app-2.png)

## <a name="clean-up-resources"></a>Resources opschonen

Als u de web-app wilt verwijderen, gebruikt u de **Azure Explorer** in de Azure Toolkit. Als de weergave **Azure Explorer** nog niet zichtbaar is in Eclipse, gebruikt u de volgende stappen om deze te openen:

1. Klik op **Venster** en daarna achtereenvolgens op **Weergave tonen** en **Andere**.

   ![Het menu Weergave tonen](./media/app-service-web-get-started-java/show-azure-explorer-view-1.png)

2. Selecteer in het dialoogvenster **Weergave tonen** de optie **Azure Explorer** en klik op **OK**.

   ![Het dialoogvenster Weergave tonen](./media/app-service-web-get-started-java/show-azure-explorer-view-2.png)

Als u de web-app wilt verwijderen vanuit de Azure Explorer, moet u het knooppunt **Web Apps** uitvouwen, met de rechtermuisknop op uw web-app klikken en **Verwijderen** kiezen.

![Web-app verwijderen](./media/app-service-web-get-started-java/delete-web-app-context-menu.png)

Wanneer u wordt gevraagd of u de web-app wilt verwijderen, klikt u op **OK**.

## <a name="next-steps"></a>Volgende stappen

Voor meer informatie over de Azure Toolkits voor Java-IDE's klikt u op de volgende koppelingen:

* [Azure Toolkit voor Eclipse (dit artikel)](../azure-toolkit-for-eclipse.md)
  * [What's New in the Azure Toolkit for Eclipse](../azure-toolkit-for-eclipse-whats-new.md) (Nieuw in de Azure Toolkit voor Eclipse)
  * [Installing the Azure Toolkit for Eclipse](../azure-toolkit-for-eclipse-installation.md) (De Azure Toolkit voor Eclipse installeren)
  * [Sign In Instructions for the Azure Toolkit for Eclipse](https://go.microsoft.com/fwlink/?linkid=846174) (Aanmeldingsinstructies voor de Azure Toolkit voor Eclipse)
* [Azure Toolkit for IntelliJ](../azure-toolkit-for-intellij.md) (Azure Toolkit voor IntelliJ)
  * [What's New in the Azure Toolkit for IntelliJ](../azure-toolkit-for-intellij-whats-new.md) (Nieuw in de Azure Toolkit voor IntelliJ)
  * [Installing the Azure Toolkit for IntelliJ](../azure-toolkit-for-intellij-installation.md) (De Azure Toolkit voor IntelliJ installeren)
  * [Sign In Instructions for the Azure Toolkit for IntelliJ](https://go.microsoft.com/fwlink/?linkid=846179) (Aanmeldingsinstructies voor de Azure Toolkit voor IntelliJ)

Voor meer informatie over het gebruik van Azure met Java raadpleegt u het [Azure Java-ontwikkelaarscentrum](https://azure.microsoft.com/develop/java/) en de [Java Tools voor Visual Studio Team Services](https://java.visualstudio.com/).

