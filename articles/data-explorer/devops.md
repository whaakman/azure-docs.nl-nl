---
title: Azure DevOps-taak voor Azure Data Explorer
description: In dit onderwerp leert u een release-pijplijn maken en implementeren
services: data-explorer
author: orspod
ms.author: orspodek
ms.reviewer: jasonh
ms.service: data-explorer
ms.topic: conceptual
ms.date: 05/05/2019
ms.openlocfilehash: 0628d5c07d7258cc4d68727c364e65bd81c78e8e
ms.sourcegitcommit: 3d4121badd265e99d1177a7c78edfa55ed7a9626
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/30/2019
ms.locfileid: "66388994"
---
# <a name="azure-devops-task-for-azure-data-explorer"></a>Azure DevOps-taak voor Azure Data Explorer

[Azure DevOps-Services](https://azure.microsoft.com/services/devops/) ontwikkeling biedt hulpprogramma's voor samenwerking, zoals high-performance pijplijnen, gratis persoonlijke Git-opslagplaatsen, configureerbare kanbanborden en uitgebreide mogelijkheden voor continue en geautomatiseerde tests. [Azure pijplijnen](https://azure.microsoft.com/services/devops/pipelines/) is een Azure DevOps-functie waarmee u voor het beheren van CI/CD voor het implementeren van uw code met hoge prestaties pijplijnen die werken met elke taal, het platform en de cloud.
[Azure Data Explorer - Admin-opdrachten](https://marketplace.visualstudio.com/items?itemName=Azure-Kusto.PublishToADX) is wijzigingen in de Azure-pijplijnen-taken waarmee u release-pijplijnen maken en implementeren van uw database met uw Azure Data Explorer-databases. Het is beschikbaar voor gratis uit in de [Visual Studio Marketplace](https://marketplace.visualstudio.com/).

Dit document beschrijft een eenvoudig voorbeeld over het gebruik van de **Azure Data Explorer – Admin opdrachten** taak voor het implementeren van uw schema wijzigingen in uw database. Raadpleeg voor volledige CI/CD-pijplijnen, [documentatie voor Azure DevOps](/azure/devops/user-guide/what-is-azure-devops?view=azure-devops#vsts).

## <a name="prerequisites"></a>Vereisten

* Als u nog geen abonnement op Azure hebt, maak dan een [gratis Azure-account](https://azure.microsoft.com/free/) aan voordat u begint.
* Installatie van de Azure Data Explorer Cluster:
    * Een [cluster van Azure Data Explorer en -database](/azure/data-explorer/create-cluster-database-portal)
    * Maken van Azure Active Directory (Azure AD)-app door [inrichten van een Azure AD-toepassing](/azure/kusto/management/access-control/how-to-provision-aad-app).
    * Toegang verlenen tot uw Azure AD-App op uw Azure Data Explorer-database door [beheren van machtigingen in Azure Data Explorer database](/azure/data-explorer/manage-database-permissions).
* Installatie van de Azure DevOps:
    * [Aanmelden voor een gratis organisatie](/azure/devops/user-guide/sign-up-invite-teammates?view=azure-devops)
    * [Een organisatie maken](/azure/devops/organizations/accounts/create-organization?view=azure-devops)
    * [Een project maken in Azure DevOps](/azure/devops/organizations/projects/create-project?view=azure-devops)
    * [Code met Git](/azure/devops/user-guide/code-with-git?view=azure-devops)

## <a name="create-folders"></a>Mappen maken

Het volgende voorbeeld-mappen maken (*functies*, *beleid*, *tabellen*) in de Git-opslagplaats. Kopieer de bestanden van [hier](https://github.com/Azure/azure-kusto-docs-samples/tree/master/DevOps_release_pipeline) in de respectieve mappen zoals weergegeven onder en sla de wijzigingen. De voorbeeldbestanden zijn opgegeven voor het uitvoeren van de volgende werkstroom.

![Mappen maken](media/devops/create-folders.png)

> [!TIP]
> Wanneer u uw eigen werkstroom maakt, is het raadzaam om uw code idempotent zijn. Gebruik bijvoorbeeld [.create-samenvoegen-tabel](/azure/kusto/management/tables#create-merge-tables) in plaats van [.create tabel](/azure/kusto/management/tables#create-table), en gebruik [.create of alter](/azure/kusto/management/functions#create-or-alter-function) werken in plaats van [.create](/azure/kusto/management/functions#create-function) de functie.

## <a name="create-a-release-pipeline"></a>Een release-pijplijn maken

1. Aanmelden bij uw [Azure DevOps-organisatie](https://dev.azure.com/).
1. Selecteer **pijplijnen** > **Releases** vanuit het menu links en selecteer **nieuwe pijplijn**.

    ![Nieuwe pijplijn](media/devops/new-pipeline.png)

1. De **nieuwe release-pijplijn** venster wordt geopend. In de **pijplijnen** tabblad, in de **selecteert u een sjabloon** deelvenster Selecteer **leeg taak**.

     ![Selecteer een sjabloon](media/devops/select-template.png)

1. Selecteer **fase** knop. In **fase** deelvenster toevoegen de **fasenaam**. Selecteer **opslaan** om op te slaan, uw pijplijn.

    ![De naam van de fase](media/devops/stage-name.png)

1. Selecteer **toevoegen van een artefact** knop. In de **toevoegen van een artefact** deelvenster, selecteert u de opslagplaats waar uw code bestaat, relevante informatie invullen en op **toevoegen**. Selecteer **opslaan** om op te slaan, uw pijplijn.

    ![Een artefact toevoegen](media/devops/add-artifact.png)

1. In de **variabelen** tabblad **+ toevoegen** te maken van een variabele voor **eindpunt-URL** dat wordt gebruikt in de taak. Schrijf de **naam** en de **waarde** van het eindpunt. Selecteer **opslaan** om op te slaan, uw pijplijn. 

    ![Variabele maken](media/devops/create-variable.png)

    Om te zoeken, uw Endpoint_URL, de overzichtspagina van uw **Azure Data Explorer Cluster** in Azure portal de URI van de Azure Data Explorer-cluster bevat. Maken van de URI in de volgende indeling `https://<Azure Data Explorer cluster URI>?DatabaseName=<DBName>`.  Bijvoorbeeld: https:\//kustodocs.westus.kusto.windows.net?DatabaseName=SampleDB

    ![Azure-Data Explorer cluster-URI](media/devops/adx-cluster-uri.png)

## <a name="create-tasks-to-deploy"></a>Maak taken om te implementeren

1. In de **pijplijn** tabblad, klikt u op **1 taak, taak 0** toevoegen taken. 

    ![Taken toevoegen](media/devops/add-task.png)

1. Maak drie taken implementeren **tabellen**, **functies**, en **beleid**, in de aangegeven volgorde. 

1. In de **taken** tabblad **+** door **Agent-taak**. Zoek naar **Azure Data Explorer**. In **Marketplace**, installeert de **Azure Data Explorer – Admin opdrachten** extensie. Selecteer **toevoegen** in **Azure Data Explorer-opdracht uitvoeren**.

     ![Beheerder opdrachten toe te voegen](media/devops/add-admin-commands.png)

1. Klik op **Kusto-opdracht** aan de linkerkant en update de taak met de volgende informatie:
    * **Weergavenaam**: Naam van de taak
    * **Bestandspad**: In de **tabellen** taak, geef */Tables/* .csl omdat de tabel maken van bestanden de *tabel* map.
    * **Eindpunt-URL**: Voer de `EndPoint URL`variabele in de vorige stap hebt gemaakt.
    * Selecteer **Service-eindpunt gebruik** en selecteer **+ nieuw**.

    ![Bijwerktaak Kusto-opdracht](media/devops/kusto-command-task.png)

1. Vul de volgende gegevens in de **verbinding met Azure Data Explorer toevoegen** venster:

    |Instelling  |Voorgestelde waarde  |
    |---------|---------|
    |**Verbindingsnaam**     |    Voer een unieke naam voor deze service-eindpunt     |
    |**Cluster-Url**    |    Waarde kan worden gevonden in de sectie overzicht van uw Azure Data Explorer-Cluster in Azure portal | 
    |**Service-Principal-Id**    |    Voer de AAD-App-ID (gemaakt als vereiste)     |
    |**App-sleutel voor Service-Principal**     |    Voer de AAD-App-sleutel (gemaakt als vereiste)    |
    |**AAD-tenant Id**    |      Voer uw AAD-tenant (bijvoorbeeld microsoft.com, contoso.com...)    |

    Selecteer **alle die leiden tot deze verbinding gebruiken toestaan** selectievakje. Selecteer **OK**.

    ![Serviceverbinding toevoegen](media/devops/add-service-connection.png)

1. Herhaal stap 1-5 andere twee keer implementeren bestanden uit de *functies* en *beleid* mappen. Selecteer **Opslaan**. In de **taken** tabblad, raadpleegt u de drie taken die zijn gemaakt: **Implementeren van tabellen**, **Functions implementeren**, en **implementeren van beleid**.

    ![Alle mappen implementeren](media/devops/deploy-all-folders.png)

1. Selecteer **+ Release** > **maken release** te maken van een release.

    ![Een releaserecord maken](media/devops/create-release.png)

1. In de **logboeken** tabblad, controleert u de status van de implementatie is voltooid.

    ![Implementatie is gelukt](media/devops/deployment-successful.png)

U hebt nu het maken van een release-pijplijn voor de implementatie van drie taken vóór productie.