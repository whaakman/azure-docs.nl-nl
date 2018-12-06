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
ms.date: 11/29/2018
ms.author: twhitney
ms.custom: mvc, devcenter
ms.openlocfilehash: 0f6ede488ae118f8df00febda3c53eabb73f2030
ms.sourcegitcommit: 2bb46e5b3bcadc0a21f39072b981a3d357559191
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/05/2018
ms.locfileid: "52890225"
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

In dit artikel laat zien hoe een microservice in een toepassing een upgrade uitvoert. In dit voorbeeld wijzigen we de `WebFrontEnd` service voor het weergeven van een categorie en vergroot de hoeveelheid CPU krijgt. Vervolgens upgraden we de geïmplementeerde service.

## <a name="modify-the-config"></a>De configuratie wijzigen

Wanneer u een Service Fabric-NET-app maakt, Visual studio wordt toegevoegd een **parameters.yaml** -bestand voor elke implementatieomgeving (cloud en lokaal). In deze bestanden, kunt u parameters en waarden die vervolgens kunnen worden verwezen vanuit uw Mesh *.yaml bestanden zoals service.yaml of network.yaml definiëren.  Visual Studio biedt enkele variabelen die voor u, zoals hoeveel CPU die de service kan gebruiken.

We werken de `WebFrontEnd_cpu` parameter voor het bijwerken van de cpu-resources te `1.5` in afwachting die de **WebFrontEnd** service strenger wordt gebruikt.

1. In de **todolistapp** project, onder **omgevingen** > **Cloud**, open de **parameters.yaml** bestand. Wijzig de `WebFrontEnd_cpu`, waarde die moet worden `1.5`. Naam van de parameter wordt voorafgegaan door de naam van de `WebFrontEnd_` als een best practice te onderscheiden van de parameters met dezelfde naam die betrekking hebben op andere services.

    ```xml
    WebFrontEnd_cpu: 1.5
    ```

2. Open de **WebFrontEnd** van het project **service.yaml** bestand onder **WebFrontEnd** > **serviceresources**.

    Houd er rekening mee dat de in `resources:` sectie `cpu:` is ingesteld op `"[parameters('WebFrontEnd_cpu')]"`. Als het project is gebouwd voor de cloud, de waarde voor `'WebFrontEnd_cpu` wordt gemaakt op basis van de **omgevingen** > **Cloud** > **parameters.yaml** bestand en worden `1.5`. Als het project wordt opgebouwd als lokaal wilt uitvoeren, de waarde wordt gemaakt op basis van de **omgevingen** > **lokale** > **parameters.yaml** -bestand, en '0,5' zijn.

> [!Tip]
> Standaard wordt de parameterbestand dat is een peer van het bestand profile.yaml worden gebruikt voor het leveren van de waarden voor dat bestand profile.yaml.
> Bijvoorbeeld, omgevingen > Cloud > parameters.yaml biedt de parameterwaarden voor omgevingen > Cloud > profile.yaml.
>
> U kunt dit wijzigen door het volgende toe te voegen aan het bestand profile.yaml:`parametersFilePath=”relative or full path to the parameters file”` bijvoorbeeld `parametersFilePath=”C:\MeshParms\CustomParameters.yaml”` of `parametersFilePath=”..\CommonParameters.yaml”`

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

Of u maakt een code-upgrade of een configuratie-upgrade (in dit geval we het doen beide), upgrade van uw Service Fabric-NET-app op Azure met de rechtermuisknop op **todolistapp** in Visual Studio en selecteer vervolgens **publiceren...**

Vervolgens ziet u het dialoogvenster **Service Fabric-toepassing publiceren**.

Gebruik de **doelprofiel** vervolgkeuzelijst om het bestand profile.yaml moet worden gebruikt voor deze implementatie te selecteren. We een upgrade uitvoert van de app in de cloud, zodat we selecteren de **cloud.yaml** in de vervolgkeuzelijst die wordt gebruikt de `WebFrontEnd_cpu` waarde van 1,0 gedefinieerd in dat bestand.

![Dialoogvenster in Visual Studio over Service Fabric Mesh-publicatie](./media/service-fabric-mesh-tutorial-deploy-dotnetcore/visual-studio-publish-dialog.png)

Selecteer uw Azure-account en -abonnement. Stel de **locatie** naar de locatie die u hebt gebruikt toen u oorspronkelijk de to-do-app gepubliceerd naar Azure. In dit artikel gebruikt **VS-Oost**.

Stel **resourcegroep** naar de resourcegroep die u hebt gebruikt toen u oorspronkelijk de to-do-app gepubliceerd naar Azure.

Stel **Azure Container Registry** in de azure container registry-naam die u hebt gemaakt toen u oorspronkelijk de to-do-app gepubliceerd naar Azure.

Druk in het dialoogvenster publiceren op de **publiceren** om de upgrade van de to-do-app op Azure.

De voortgang van de upgrade door het selecteren van de **Service Fabric Tools** deelvenster in de Visual Studio **uitvoer** venster. 

Nadat de installatiekopie is gemaakt en naar Azure Container Registry gepusht een **voor de status van** koppeling wordt weergegeven in de uitvoer die u kunt klikken op voor het controleren van de implementatie in Azure portal.

Nadat de upgrade is voltooid, de **Service Fabric Tools** uitvoer wordt weergegeven de IP-adres en poort van uw toepassing in de vorm van een URL.

```json
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