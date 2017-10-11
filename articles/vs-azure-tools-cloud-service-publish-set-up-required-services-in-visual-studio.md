---
title: Voorbereiden op publiceert of implementeert vanuit Visual Studio een Azure-toepassing | Microsoft Docs
description: Meer informatie over de procedures voor services voor cloud- en storage-account instellen en configureren van uw Azure-toepassing.
services: visual-studio-online
documentationcenter: na
author: kraigb
manager: ghogen
editor: 
ms.assetid: 92ee2f9e-ec49-4c7a-900d-620abe5e9d8a
ms.service: multiple
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: multiple
ms.date: 11/11/2016
ms.author: kraigb
ms.openlocfilehash: cc4fb87e559f554634ae062a59bee31f0831da64
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/29/2017
---
# <a name="prepare-to-publish-or-deploy-an-azure-application-from-visual-studio"></a>Voorbereiden op publiceert of implementeert vanuit Visual Studio een Azure-toepassing
## <a name="overview"></a>Overzicht
Voordat u een cloudserviceproject publiceren kunt, moet u de volgende services instellen:

* Een **cloudservice** uw rollen uitvoeren in de Azure-omgeving
* Een **opslagaccount** die toegang biedt tot de services Blob, wachtrijen en tabellen.

Gebruik de volgende procedures om deze services instellen en configureren van uw toepassing

## <a name="create-a-cloud-service"></a>Een cloudservice maken
Voor het publiceren van een service in de cloud naar Azure, moet u eerst een cloudservice, die wordt uitgevoerd van de rollen in de Azure-omgeving te maken. Kunt u een cloudservice in de [klassieke Azure-portal](http://go.microsoft.com/fwlink/?LinkID=213885), zoals beschreven in de sectie **maken van een cloudservice met behulp van de klassieke Azure portal**verderop in dit onderwerp. U kunt ook een cloudservice maken in Visual Studio met behulp van de wizard publiceren.

### <a name="to-create-a-cloud-service-by-using-visual-studio"></a>Een cloudservice maken met behulp van Visual Studio
1. Open het snelmenu voor het Azure-project en kies **publiceren**.

    ![VST_PublishMenu](./media/vs-azure-tools-cloud-service-publish-set-up-required-services-in-visual-studio/vst-publish-menu.png)
2. Als u dit nog niet hebt aangemeld, aanmelden met uw gebruikersnaam en wachtwoord voor de Microsoft-account of organisatie-account dat is gekoppeld aan uw Azure-abonnement.
3. Kies de **volgende** knop om door te gaan naar de **instellingen** pagina.

    ![Publishing algemene instellingen van de Wizard](./media/vs-azure-tools-cloud-service-publish-set-up-required-services-in-visual-studio/publish-settings-page.png)
4. In de **Cloudservices** Kies **nieuw**. De **Azure-Services maken** dialoogvenster wordt weergegeven.
5. Voer de naam van uw cloudservice. De naam maakt deel uit van de URL voor uw service en moet daarom globaal uniek zijn. De naam is niet hoofdlettergevoelig.

### <a name="to-create-a-cloud-service-by-using-the-azure-classic-portal"></a>Een cloudservice maken met behulp van de klassieke Azure portal
1. Aanmelden bij de [klassieke Azure-portal](http://go.microsoft.com/fwlink/?LinkId=253103) op de website van Microsoft.
2. (optioneel) Kies voor een lijst van cloud-services die u al hebt gemaakt, de Cloud Services-koppeling aan de linkerkant van de pagina.
3. Kies de  **+**  pictogram in de linkerbenedenhoek hoek en kies vervolgens **Cloudservice** op het menu dat verschijnt. Een ander scherm met twee opties **snelle invoer** en **aangepast maken**, wordt weergegeven. Als u ervoor kiest **snelle invoer**, kunt u een cloudservice maken door te geven van de URL en de regio waar deze fysiek worden gehost. Als u ervoor kiest **aangepast maken**, kunt u een cloudservice onmiddellijk publiceren door te geven van een pakket (.cspkg-bestand), een configuratiebestand (.cscfg) en een certificaat. Aangepast maken is niet vereist als u van plan bent uw cloudservice publiceren met behulp van de **publiceren** opdracht in een Azure-project. De **publiceren** opdracht is beschikbaar in het snelmenu voor een Azure-project.
4. Kies **snelle invoer** later uw cloudservice publiceren met behulp van Visual Studio.
5. Geef een naam voor uw cloudservice. De volledige URL weergegeven naast de naam.
6. Kies de regio waar de meeste gebruikers zich bevinden in de lijst.
7. Aan de onderkant van het venster, kies de **Cloudservice maken** koppeling.

## <a name="create-a-storage-account"></a>Een opslagaccount maken
Een opslagaccount biedt toegang tot de services Blob, wachtrijen en tabellen. U kunt een opslagaccount maken met behulp van Visual Studio of de [klassieke Azure-portal](http://go.microsoft.com/fwlink/?LinkId=253103).

### <a name="to-create-a-storage-account-by-using-visual-studio"></a>Een opslagaccount maken met behulp van Visual Studio
1. In **Solution Explorer**, open het snelmenu voor het **opslag** knooppunt, en kies vervolgens **Storage-Account maken**.

    ![Een nieuw Azure storage-account maken](./media/vs-azure-tools-cloud-service-publish-set-up-required-services-in-visual-studio/IC744166.png)
2. Selecteer of typ de volgende informatie voor het nieuwe opslagaccount in de **Storage-Account maken** in het dialoogvenster.

   * De Azure-abonnement dat u wilt toevoegen de storage-account.
   * De naam die u wilt gebruiken voor het nieuwe opslagaccount.
   * De regio of affiniteitsgroep (zoals VS-West of Oost-Azië).
   * Het type dat u wilt gebruiken voor het opslagaccount, zoals geografisch redundante replicatie.
3. Als u bent klaar, kiest u **maken**. Het nieuwe opslagaccount wordt weergegeven in de **opslag** in lijst **Server Explorer**.

### <a name="to-create-a-storage-account-by-using-the-azure-classic-portal"></a>Een opslagaccount maken met behulp van de klassieke Azure portal
1. Aanmelden bij de [klassieke Azure-portal](http://go.microsoft.com/fwlink/?LinkId=253103) op de website van Microsoft.
2. (Optioneel) U kunt uw opslagaccounts bekijken de **opslag** koppeling in het paneel aan de linkerkant van de pagina.
3. Kies in de linkerbenedenhoek van de pagina de  **+**  pictogram.
4. Kies in het menu dat verschijnt, **opslag**, en kies vervolgens **snelle invoer**.
5. Geeft het storage-account een naam die in een unieke url resulteren.
6. Geef een naam op uw cloudservice. De volledige URL weergegeven naast de naam.
7. Kies een regio waar de meeste gebruikers zich bevinden in de lijst met regio's.
8. Geef op of u wilt inschakelen, geo-replicatie. Als u geo-replicatie inschakelt, wordt uw gegevens opgeslagen op verschillende fysieke locaties te voorkomen dat verloren zijn gegaan. Deze functie kunt u opslag duurder, maar u kunt de kosten reduceren doordat geografische locatie bij het maken van het opslagaccount in plaats van het onderdeel later toevoegen. Zie voor meer informatie [Geo-replicatie](http://go.microsoft.com/fwlink/?LinkId=253108).
9. Aan de onderkant van het venster, kies de **Storage-Account maken** koppeling.

Nadat u uw storage-account maakt, ziet u de URL's die u gebruiken kunt voor toegang tot bronnen in elk van de Azure storage-services en de primaire en secundaire sneltoetsen voor uw account. U kunt deze sleutels gebruiken om aanvragen ten opzichte van de opslagservices te verifiëren.

> [!NOTE]
> De secundaire toegangssleutel biedt dezelfde toegangsrechten als de primaire toegangssleutel voor uw opslagaccount en wordt gegenereerd als een back-up van uw primaire toegangssleutel verdacht. Bovendien is het raadzaam dat u de toegangssleutels regelmatig opnieuw genereert. Een instelling van de tekenreeks gebruiken van de secundaire sleutel terwijl u de primaire sleutel opnieuw genereren en vervolgens kunt u deze voor het gebruik van de primaire sleutel in opnieuw gegenereerd tijdens het genereren van de secundaire sleutel wijzigen, kunt u wijzigen.
>
>

## <a name="configure-your-app-to-use-services-provided-by-the-storage-account"></a>Configureer uw app om de services geleverd door de storage-account gebruiken
Functies die toegang heeft tot de storage-services voor het gebruik van de Azure storage-services die u hebt gemaakt, moet u configureren. Om dit te doen, kunt u meerdere configuraties voor uw Azure-project. Standaard worden twee gemaakt in uw Azure-project. U kunt met behulp van serviceconfiguraties met meerdere dezelfde verbindingstekenreeks gebruiken in uw code maar hebt een andere waarde voor een verbindingsreeks in de serviceconfiguratie van elke. Bijvoorbeeld, kunt u één serviceconfiguratie voor het uitvoeren en fouten opsporen in uw toepassing lokaal via de Azure-opslagemulator en de configuratie van een andere service aan uw toepassing publiceren in Azure. Zie voor meer informatie over configuraties [configureren van uw Azure-Project met behulp van serviceconfiguraties met meerdere](vs-azure-tools-multiple-services-project-configurations.md).

### <a name="to-configure-your-application-to-use-services-that-the-storage-account-provides"></a>Voor het configureren van uw toepassing services waarmee het storage-account gebruiken
1. Open uw Azure-oplossing in Visual Studio. Klik in Solution Explorer het snelmenu voor elke rol in uw Azure-project die toegang heeft tot de storage-services te openen en kies **eigenschappen**. Een pagina met de naam van de rol wordt weergegeven in de Visual Studio-editor. De pagina wordt weergegeven voor de velden voor de **configuratie** tabblad.
2. Kies in de eigenschappenpagina voor de rol **instellingen**.
3. In de **serviceconfiguratie** kiest u de naam van de configuratie van de service die u wilt bewerken. Als u wijzigingen aanbrengen voor alle serviceconfiguraties voor deze rol wilt, kunt u kiezen **alle configuraties**.  Zie de sectie voor meer informatie over het bijwerken van configuraties **verbindingsreeksen voor Storage-Accounts beheren** in het onderwerp [de rollen configureren voor een Azure Cloud Service met Visual Studio](vs-azure-tools-configure-roles-for-cloud-service.md).
4. Voor het wijzigen van instellingen voor een verbindingstekenreeks, kies de **...** knop naast de instelling. De **Create Storage Connection String** dialoogvenster wordt weergegeven.
5. Onder **verbinding maken via**, kies de **uw abonnement** optie.
6. In de **abonnement** kiest u uw abonnement. Als de lijst met abonnementen niet die u wilt opnemen, kiest u de **publicatie-Downloadinstellingen** koppeling.
7. In de **accountnaam** Kies de naam van uw opslagaccount. Azure-hulpprogramma's verkrijgt opslagaccountreferenties automatisch via het .publishsettings-bestand. Als uw opslagaccountreferenties handmatig opgeven, kiest u de **referenties handmatig worden ingevoerd** optie en ga verder met deze procedure. U krijgt de opslagaccountnaam en de primaire sleutel van de [klassieke Azure-portal](http://go.microsoft.com/fwlink/p/?LinkID=213885). Als u niet wilt opgeven van uw opslagaccount die instellingen handmatig, kies de **OK** knop om het dialoogvenster te sluiten.
8. Kies de **storage-account invoeren** referenties koppeling.
9. In de **accountnaam** Voer de naam van uw opslagaccount.

   > [!NOTE]
   > Meld u aan bij de [klassieke Azure-portal](http://go.microsoft.com/fwlink/?LinkID=213885), en kies vervolgens de **opslag** knop. De portal bevat een overzicht van de storage-accounts. Als u een account kiest, wordt er een pagina voor het geopend. Op deze pagina kunt u de naam van het opslagaccount. Als u van een eerdere versie van de klassieke portal gebruikmaakt, de naam van uw storage-account wordt weergegeven in de **Opslagaccounts** weergeven. Als u wilt kopiëren deze naam, markeer deze in de **eigenschappen** venster hiervan weergeven en kies vervolgens de toetsen Ctrl + C. U kunt de naam in Visual Studio plakken, kies de **accountnaam** tekst vak in en kies vervolgens de toetsen Ctrl + V.
   >
   >
10. In de **accountsleutel** vak, Voer uw primaire sleutel of kopiëren en plakken van de [klassieke Azure-portal](http://go.microsoft.com/fwlink/?LinkID=213885).
     Deze sleutel kopiëren:

    1. Kies onder aan de pagina voor het account de juiste opslag, de **sleutels beheren** knop.
    2. Op de **sleutels toegang beheren** pagina, selecteert u de tekst van de primaire toegangssleutel en kies vervolgens de toetsen Ctrl + C.
    3. Plak in hulpprogramma's van Azure, de sleutel in de **accountsleutel** vak.
    4. U moet een van de volgende opties om te bepalen hoe de service toegang tot het opslagaccount selecteren:

       * **HTTP gebruiken**. Dit is de standaard optie. Bijvoorbeeld `http://<account name>.blob.core.windows.net`.
       * **Gebruik van HTTPS** voor een beveiligde verbinding. Bijvoorbeeld `https://<accountname>.blob.core.windows.net`.
       * **Geef aangepaste eindpunten** voor elk van de drie services. U kunt deze eindpunten vervolgens naar het veld voor de specifieke service typen.

         > [!NOTE]
         > Als u aangepaste eindpunten maakt, kunt u een complexere verbindingsreeks kunt maken. Wanneer u deze tekenreeksindeling gebruikt, kunt u de service-eindpunten voor opslag met een aangepaste domeinnaam die u hebt geregistreerd voor uw opslagaccount met de Blob-service opgeven. Ook kunt u alleen toegang tot blob-bronnen in een enkele container via een shared access signature verlenen. Zie voor meer informatie over het maken van aangepaste eindpunten [Azure Storage-verbindingsreeksen configureren](storage/common/storage-configure-connection-string.md).
         >
         >
11. Deze verbinding tekenreeks als wijzigingen wilt opslaan, kies de **OK** knop en kies vervolgens de **opslaan** op de werkbalk. Nadat u deze wijzigingen hebt opgeslagen, kunt u de waarde van deze verbindingsreeks verkrijgen in uw code met behulp van [GetConfigurationSettingValue](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleenvironment.getconfigurationsettingvalue.aspx). Wanneer u uw toepassing in Azure publiceren, kiest u de configuratie van de service met de Azure storage-account voor de verbindingsreeks. Nadat de toepassing is gepubliceerd, Controleer of de toepassing werkt zoals verwacht op basis van de Azure storage-services

## <a name="next-steps"></a>Volgende stappen
Zie voor meer informatie over apps publiceren naar Azure vanuit Visual Studio, [publiceren van een Cloudservice met de Azure-hulpprogramma's](vs-azure-tools-publishing-a-cloud-service.md).
