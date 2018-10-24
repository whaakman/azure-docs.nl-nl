---
title: Zelfstudie - Upgrade een net van Azure Service Fabric-toepassing | Microsoft Docs
description: Meer informatie over het upgraden van een Service Fabric-toepassing met Visual Studio
services: service-fabric-mesh
documentationcenter: .net
author: tylerMSFT
manager: jeconnoc
editor: ''
ms.assetid: ''
ms.service: service-fabric-mesh
ms.devlang: azure-cli
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 09/18/2018
ms.author: twhitney
ms.custom: mvc, devcenter
ms.openlocfilehash: 7985c8e9e26126040d842ded998a953281daa2ae
ms.sourcegitcommit: 5c00e98c0d825f7005cb0f07d62052aff0bc0ca8
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/24/2018
ms.locfileid: "49953549"
---
# <a name="tutorial-learn-how-to-upgrade-a-service-fabric-application-using-visual-studio"></a>Zelfstudie: Meer informatie over het upgraden van een Service Fabric-toepassing met Visual Studio

Deze zelfstudie is deel vier van een reeks en ziet u hoe u een net van Azure Service Fabric-toepassing rechtstreeks vanuit Visual Studio een upgrade uitvoert. De upgrade bevat zowel een update van code en een configuratie-update. U ziet dat de stappen voor het upgraden en publiceren vanuit Visual Studio hetzelfde zijn.

In deze zelfstudie leert u het volgende:
> [!div class="checklist"]
> * Een Service Fabric-NET-service upgraden met behulp van Visual Studio

In deze zelfstudiereeks leert u het volgende:
> [!div class="checklist"]
> * [Een Service Fabric Mesh-app maken in Visual Studio](service-fabric-mesh-tutorial-create-dotnetcore.md)
> * [Fouten opsporen in een Service Fabric Mesh-app die wordt uitgevoerd in uw lokale ontwikkelcluster](service-fabric-mesh-tutorial-debug-service-fabric-mesh-app.md)
> * [Een Service Fabric Mesh-app implementeren](service-fabric-mesh-tutorial-deploy-service-fabric-mesh-app.md)
> * Upgrade van een Service Fabric-NET app
> * [Service Fabric Mesh-bronnen opschonen](service-fabric-mesh-tutorial-cleanup-resources.md)

[!INCLUDE [preview note](./includes/include-preview-note.md)]

## <a name="prerequisites"></a>Vereisten

Voor u met deze zelfstudie begint:

* Als u de to-do-app nog niet hebt geïmplementeerd, volgt u de instructies in [Zelfstudie: een Service Fabric Mesh-webtoepassing implementeren](service-fabric-mesh-tutorial-deploy-service-fabric-mesh-app.md).

## <a name="upgrade-a-service-fabric-mesh-service-by-using-visual-studio"></a>Een Service Fabric-NET-service upgraden met behulp van Visual Studio

In dit artikel laat zien hoe een microservice in een toepassing onafhankelijk upgraden.  In dit voorbeeld wijzigen we de `WebFrontEnd` service om een taakcategorie weer te geven. Vervolgens upgraden we de geïmplementeerde service.

## <a name="modify-the-config"></a>De configuratie wijzigen

Upgrades kunnen worden veroorzaakt door wijzigingen in de code en/of wijzigingen in de configuratie.  Om te introduceren een configuratiewijziging, opent u de `WebFrontEnd` van het project `service.yaml` bestand (dit is onder de **serviceresources** knooppunt).

In de `resources:` sectie, wijzigt u `cpu:` van 0,5 1.0, in afwachting dat de web-front-end wordt intensief gebruikt. Het moet er nu uitzien als volgt:

```xml
              ...
              resources:
                requests:
                  cpu: 1.0
                  memoryInGB: 1
              ...
```

## <a name="modify-the-model"></a>Wijzigen van het model

Om te introduceren een codewijziging, Voeg een `Category` eigenschap in op de `ToDoItem` klasse de `ToDoItem.cs` bestand.

```csharp
public class ToDoItem
{
    public string Category { get; set; }
    ...
}
```

Werk vervolgens de `Load()` methode, in hetzelfde bestand naar de categorie instellen op een standaardtekenreeks:

```csharp
public static ToDoItem Load(string description, int index, bool completed)
{
    ToDoItem newItem = new ToDoItem(description)
    {
        Index = index,
        Completed = completed, 
        Category = "none"    // <-- add this line
    };

    return newItem;
}
```

## <a name="modify-the-service"></a>De service wijzigen

De `WebFrontEnd` project is een ASP.NET Core-toepassing met een webpagina waarop de taak lijst met items worden weergegeven. In de `WebFrontEnd` project, open `Index.cshtml` en voeg de volgende twee regels om weer te geven van de taak categorie hieronder, wordt aangegeven:

```HTML
<div>
    <table class="table-bordered">
        <thead>
            <tr>
                <th>Description</th>
                <th>Category</th>           @*add this line*@
                <th>Done?</th>
            </tr>
        </thead>
        <tbody>
            @foreach (var item in Model.Items)
            {
                <tr>
                    <td>@item.Description</td>
                    <td>@item.Category</td>           @*add this line*@
                    <td>@item.Completed</td>
                </tr>
            }
        </tbody>
    </table>
</div>
```

Ontwikkel en voer de app om te controleren of er een nieuwe categoriekolom in de webpagina wordt weergegeven met een lijst met taken.

## <a name="upgrade-the-app-from-visual-studio"></a>Upgrade van de app vanuit Visual Studio

Ongeacht of u een code-upgrade of een upgrade van een configuratie wilt maken (in dit geval we voeren beide), upgrade uitvoeren voor uw Service Fabric-NET-app op Azure met de rechtermuisknop op **todolistapp** in Visual Studio en selecteer **publiceren ...**

Vervolgens ziet u het dialoogvenster **Service Fabric-toepassing publiceren**.

![Dialoogvenster in Visual Studio over Service Fabric Mesh-publicatie](./media/service-fabric-mesh-tutorial-deploy-dotnetcore/visual-studio-publish-dialog.png)

Selecteer uw Azure-account en -abonnement. Zorg ervoor dat **locatie** is ingesteld op de locatie die u hebt gebruikt toen u oorspronkelijk de to-do-app gepubliceerd naar Azure. In dit artikel gebruikt **VS-Oost**.

Zorg ervoor dat **resourcegroep** is ingesteld op de resourcegroep die u hebt gebruikt toen u oorspronkelijk de to-do-app gepubliceerd naar Azure.

Zorg ervoor dat **Azure Container Registry** is ingesteld op de azure container registry-naam die u hebt gemaakt toen u oorspronkelijk de to-do-app gepubliceerd naar Azure.

Druk in het dialoogvenster publiceren op de **publiceren** om de upgrade van de to-do-app op Azure.

U kunt de voortgang van de upgrade door het selecteren van de **Service Fabric Tools** deelvenster in de Visual Studio **uitvoer** venster. Nadat de upgrade is voltooid, de **Service Fabric Tools** uitvoer wordt weergegeven de IP-adres en poort van uw toepassing in de vorm van een URL.

```json
Packaging Application...
Building Images...
Web1 -> C:\Code\ServiceFabricMeshApp\ToDoService\bin\Any CPU\Release\netcoreapp2.0\ToDoService.dll
Uploading the images to Azure Container Registy...
Deploying application to remote endpoint...
The application was deployed successfully and it can be accessed at http://10.000.38.000:20000.
```

Open een webbrowser en navigeer naar de URL om de website in Azure uitgevoerd te zien worden. U ziet nu een webpagina waarop een categoriekolom bevat.

## <a name="next-steps"></a>Volgende stappen

In dit deel van de zelfstudie hebt u het volgende geleerd:
> [!div class="checklist"]
> * Upgrade uitvoeren van een Service Fabric-NET-app met behulp van Visual Studio

Ga door naar de volgende zelfstudie:
> [!div class="nextstepaction"]
> [Service Fabric Mesh-bronnen opschonen](service-fabric-mesh-tutorial-cleanup-resources.md)