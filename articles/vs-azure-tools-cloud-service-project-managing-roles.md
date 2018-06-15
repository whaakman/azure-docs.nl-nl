---
title: Het beheren van rollen in Azure cloudservices met Visual Studio | Microsoft Docs
description: Informatie over het toevoegen en verwijderen van rollen in Azure cloudservices met Visual Studio.
services: visual-studio-online
author: ghogen
manager: douge
assetId: 5ec9ae2e-8579-4e5d-999e-8ae05b629bd1
ms.prod: visual-studio-dev15
ms.technology: vs-azure
ms.workload: azure
ms.topic: conceptual
ms.date: 03/21/2017
ms.author: ghogen
ms.openlocfilehash: d0a2148274cd41654eb789772b3ea46bc4ed01aa
ms.sourcegitcommit: fa493b66552af11260db48d89e3ddfcdcb5e3152
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/23/2018
ms.locfileid: "31793967"
---
# <a name="managing-roles-in-azure-cloud-services-with-visual-studio"></a>Het beheren van rollen in Azure cloudservices met Visual Studio
Nadat u uw Azure-cloud-service hebt gemaakt, kunt u nieuwe rollen aan toe te voegen of bestaande rollen verwijderen. U kunt ook een bestaand project importeren en converteer deze naar een rol. U kunt bijvoorbeeld een ASP.NET-webtoepassing importeren en opgeven dat het een Webrol.

## <a name="adding-a-role-to-an-azure-cloud-service"></a>Een rol toevoegen aan een Azure cloudservice
De volgende stappen helpen u bij een web- of worker-rol toevoegen aan een Azure-cloud service-project in Visual Studio.

1. Maak of open een Azure-cloud service-project in Visual Studio.

1. In **Solution Explorer**, vouwt u het projectknooppunt

1. Met de rechtermuisknop op de **rollen** knooppunt om het contextmenu weer te geven. Selecteer in het contextmenu **toevoegen**, selecteer vervolgens een bestaande Webrol of functie worker vanuit de huidige oplossing of een web- of worker-rol-project maken. Ook kunt u een juiste project, zoals een ASP.NET-toepassing-webproject selecteren en deze koppelen aan een rolproject.

    ![Menuopties voor het toevoegen van een rol aan een Azure-cloud service-project](media/vs-azure-tools-cloud-service-project-managing-roles/add-role.png)

## <a name="removing-a-role-from-an-azure-cloud-service"></a>Een rol verwijderen uit een Azure cloudservice
De volgende stappen helpen u bij een web- of worker-rol verwijderen uit een Azure-cloud service-project in Visual Studio.

1. Maak of open een Azure-cloud service-project in Visual Studio.

1. In **Solution Explorer**, vouwt u het projectknooppunt

1. Vouw de **rollen** knooppunt.

1. Met de rechtermuisknop op het knooppunt dat u wilt verwijderen en selecteer in het contextmenu **verwijderen**. 

    ![Menuopties voor een functie toevoegen aan een Azure cloudservice](media/vs-azure-tools-cloud-service-project-managing-roles/remove-role.png)

## <a name="readding-a-role-to-an-azure-cloud-service-project"></a>Een rol readding aan een Azure-cloud service-project
Als u een rol uit uw cloudserviceproject verwijderen maar later alsnog besluiten om toe te voegen van de rol terug naar het project, worden alleen de declaratie van de rol en basic kenmerken, zoals eindpunten en diagnostische gegevens, toegevoegd. Er is geen aanvullende resources of de verwijzingen worden toegevoegd aan de `ServiceDefinition.csdef` bestand of de `ServiceConfiguration.cscfg` bestand. Als u wilt dat deze gegevens toevoegen, moet u handmatig toevoegen terug naar deze bestanden.

Bijvoorbeeld, u kunt een Webrol service verwijderen en later besluit om toe te voegen van deze rol in uw oplossing. Als u dit doet, treedt er een fout op. Om te voorkomen dat deze fout, die u moet toevoegen de `<LocalResources>` element wordt weergegeven in het volgende XML-bestand naar de `ServiceDefinition.csdef` bestand. Gebruik de naam van de functie web service die u hebt toegevoegd in het project als onderdeel van het kenmerk name voor de **<LocalStorage>** element. In dit voorbeeld wordt de naam van de functie web service is **WCFServiceWebRole1**.

    <WebRole name="WCFServiceWebRole1">
        <Sites>
          <Site name="Web">
            <Bindings>
              <Binding name="Endpoint1" endpointName="Endpoint1" />
            </Bindings>
          </Site>
        </Sites>
        <Endpoints>
          <InputEndpoint name="Endpoint1" protocol="http" port="80" />
        </Endpoints>
        <Imports>
          <Import moduleName="Diagnostics" />
        </Imports>
       <LocalResources>
          <LocalStorage name="WCFServiceWebRole1.svclog" sizeInMB="1000" cleanOnRoleRecycle="false" />
       </LocalResources>
    </WebRole>

## <a name="next-steps"></a>Volgende stappen
- [De rollen configureert voor een Azure-cloudservice met Visual Studio](vs-azure-tools-configure-roles-for-cloud-service.md)
