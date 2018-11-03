---
title: Beheren van rollen in Azure cloudservices met Visual Studio | Microsoft Docs
description: Informatie over het toevoegen en verwijderen van rollen in Azure cloudservices met Visual Studio.
services: visual-studio-online
author: ghogen
manager: douge
assetId: 5ec9ae2e-8579-4e5d-999e-8ae05b629bd1
ms.prod: visual-studio-dev15
ms.technology: vs-azure
ms.custom: vs-azure
ms.workload: azure-vs
ms.topic: conceptual
ms.date: 03/21/2017
ms.author: ghogen
ms.openlocfilehash: 835494f169b29dc746e4acaad8a3155fd63948a8
ms.sourcegitcommit: ada7419db9d03de550fbadf2f2bb2670c95cdb21
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/02/2018
ms.locfileid: "50969256"
---
# <a name="managing-roles-in-azure-cloud-services-with-visual-studio"></a>Beheren van rollen in Azure cloudservices met Visual Studio
Nadat u uw Azure-cloud-service hebt gemaakt, kunt u nieuwe rollen aan toe te voegen of bestaande rollen van het apparaat verwijderen. U kunt ook een bestaand project importeren en deze omzetten in een rol. U kunt bijvoorbeeld importeren van een ASP.NET-toepassing en worden aangewezen als een Webrol.

## <a name="adding-a-role-to-an-azure-cloud-service"></a>Een rol toe te voegen aan een Azure-cloudservice
De volgende stappen begeleiden u bij het toevoegen van de rol van een web- of werkrollen in een Azure-cloud service-project in Visual Studio.

1. Maak of open een Azure-cloud service-project in Visual Studio.

1. In **Solution Explorer**, vouwt u het projectknooppunt

1. Met de rechtermuisknop op de **rollen** knooppunt om het contextmenu weer te geven. Selecteer in het contextmenu **toevoegen**, selecteer een bestaande Webrol of werkrol uit de huidige oplossing, of maak een web- of werkrollen rolproject. Ook kunt u een juiste project, zoals een ASP.NET-toepassing-webproject selecteren en deze koppelen aan een rolproject.

    ![Menuopties voor het toevoegen van een rol aan een Azure-cloud service-project](media/vs-azure-tools-cloud-service-project-managing-roles/add-role.png)

## <a name="removing-a-role-from-an-azure-cloud-service"></a>Een rol verwijderen uit een Azure-cloudservice
De volgende stappen begeleiden u bij het verwijderen van een web- of werkrollen-functie van een Azure-cloud service-project in Visual Studio.

1. Maak of open een Azure-cloud service-project in Visual Studio.

1. In **Solution Explorer**, vouwt u het projectknooppunt

1. Vouw de **rollen** knooppunt.

1. Met de rechtermuisknop op het knooppunt dat u wilt verwijderen en selecteer in het contextmenu **verwijderen**. 

    ![Menuopties voor het toevoegen van een rol in een Azure cloudservice](media/vs-azure-tools-cloud-service-project-managing-roles/remove-role.png)

## <a name="readding-a-role-to-an-azure-cloud-service-project"></a>Opnieuw toe te voegen een rol aan een Azure-cloud service-project
Als u een rol uit uw cloudserviceproject verwijdert maar later besluit om toe te voegen van de rol terug naar het project, worden alleen de declaratie van de rol en de algemene kenmerken, zoals eindpunten en diagnostische gegevens, worden toegevoegd. Er is geen aanvullende bronnen of verwijzingen worden toegevoegd aan de `ServiceDefinition.csdef` bestand of de `ServiceConfiguration.cscfg` bestand. Als u deze informatie toevoegen wilt, moet u deze terug naar deze bestanden handmatig toevoegen.

Bijvoorbeeld, u kunt een service-Webrol verwijderen en later besluit om toe te voegen van deze rol in uw oplossing. Als u dit doet, wordt er een fout optreedt. Om te voorkomen dat deze fout, die u moet toevoegen de `<LocalResources>` element wordt weergegeven in het volgende XML-bestand naar de `ServiceDefinition.csdef` bestand. Gebruik de naam van de functie web service, die u hebt toegevoegd in het project als onderdeel van het naamkenmerk voor de **<LocalStorage>** element. In dit voorbeeld wordt de naam van de functie web service is **WCFServiceWebRole1**.

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
- [De rollen voor een Azure-cloudservice met Visual Studio configureren](vs-azure-tools-configure-roles-for-cloud-service.md)
