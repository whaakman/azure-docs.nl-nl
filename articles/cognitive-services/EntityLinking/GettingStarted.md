---
title: Aan de slag met de API voor het koppelen van entiteit | Microsoft Docs
description: Analyseer tekst en een koppeling met de naam entiteiten aan de relevante items in een kennisdatabase met behulp van de API voor het koppelen van entiteit in cognitie Services.
services: cognitive-services
author: DavidLiCIG
manager: wkwok
ms.service: cognitive-services
ms.component: entity-linking-intelligence
ms.topic: article
ms.date: 07/06/2016
ms.author: davl
ms.openlocfilehash: 54c4a3bbb3637c248bd7705ed291633368b542c9
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/23/2018
ms.locfileid: "35344537"
---
# <a name="get-started-with-entity-linking-api-in-c35"></a>Aan de slag met API in C koppelen entiteit&#35;

Microsoft entiteit koppelen is een natuurlijke taal verwerking-hulpprogramma voor het analyseren van tekst en met de naam van de entiteiten koppelen aan de relevante items in een knowledge base. 

Deze zelfstudie behandelt entiteit koppelen met behulp van de clientbibliotheek van entiteit koppelen als een NuGet-pakket. 

### <a name="Prerequisites">Vereisten</a>

- Visual Studio 2015
- API-sleutel van een cognitieve Microsoft-Services
- Ophalen van de client-bibliotheek en voorbeeld
- NuGet-pakket van Microsoft-entiteit koppelen

U kunt de entiteit koppelen Intelligence Service API-clientbibliotheek via downloaden [SDK](https://www.github.com/microsoft/cognitive-entitylinking-windows). Het gedownloade zip-bestand moet worden uitgepakt naar een map van uw keuze, veel gebruikers selecteert u de map Visual Studio 2015.

### <a name="step-1-subscribe-entity-linking-intelligence-service-and-get-your-own-key">Stap 1: Entiteit koppelen Intelligence Service geabonneerd en sleutel ophalen</a>
Voordat u de entiteit koppelen Intelligence Service, moet u zich aanmeldt voor een API-sleutel. Zie [abonnementen](https://www.microsoft.com/cognitive-services/en-us/sign-up). Zowel de primaire en secundaire sleutel kan worden gebruikt in deze zelfstudie.

### <a name="step-2-create-a-new-project-in-visual-studio"> Stap 2: Maak een nieuw project in Visual Studio</a>

Begint met het maken van een nieuw project in Visual Studio. Eerst start Visual Studio 2015 vanuit het Menu Start. Maak vervolgens een nieuw project door te selecteren **→ sjablonen → Visual C# → lege App voor Windows universele → geïnstalleerd** voor uw projectsjabloon:

 ![U universele app](./Images/CreateUWP.png)

### <a name="step-3-add-the-entity-linking-nuget-package-to-your-project">Stap 3: De entiteit koppelen NuGet-pakket toevoegen aan uw project</a>

Entiteit koppelen van cognitieve Services is uitgebracht als een pakket NuGet.org en moet worden geïnstalleerd voordat u deze kunt gebruiken.
Toe te voegen aan uw project, gaat u naar de **Solution Explorer** tabblad, klik met de rechtermuisknop op uw project en selecteer **Nuget-pakketten beheren**.

Eerste in de **NuGet Package Manager** venster Selecteer NuGet.org als uw **pakketbron** in de rechterbovenhoek. Selecteer **Bladeren** Typ 'ProjectOxford.EntityLinking' in de linkerbovenhoek en in het zoekvak. Selecteer de **Microsoft.ProjectOxford.EntityLinking** NuGet-pakket en klikt u op **installeren**.

Vervolgens kunt u zoeken naar Newtonsoft.Json downloaden en installeren. Als u wordt gevraagd om wijzigingen te controleren, klikt u op **OK**. Als u krijgt u de licentievoorwaarden EntityLinking, klikt u op **ik ga akkoord**.

Koppelen van de entiteit is nu geïnstalleerd als onderdeel van uw toepassing. U kunt dit controleren door te controleren dat de ** Microsoft.ProjectOxford.EntityLinking** verwijzing aanwezig is als onderdeel van uw project in Solution Explorer.

 ![Opgenomen nuget-bibliotheek in project](./Images/NugetLibraryInProject.png)
 
### <a name="step-4-add-an-input-and-output-text-block-to-your-apps-xaml">Stap 4: Toevoegen van een invoer en uitvoer tekstblok naar uw app XAML</a>
Navigeer naar ** MainPage.xaml ** in **Solution Explorer**, dubbelklikt u op het bestand dat wordt geopend in een nieuw venster. Voor het gemak, u kunt dubbelklikken op de **XAML** knop in de **Designer** tabblad worden verborgen de **visuele ontwerper** en alle ruimte voor de codeweergave reserveren.

 ![Opgenomen nuget-bibliotheek in project](./Images/UWPMainPage.png)
 
Als een tekstservice is het maken van de beste manier om de functionaliteit visualiseren invoer en een tekstblok uitvoer. Voeg hiervoor de volgende XAML in de **raster**. Deze code wordt toegevoegd voor drie onderdelen, een invoer tekstvak een tekstblok uitvoer en een startknop.
 
 ```XAML
 <Grid Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">
    <Grid.RowDefinitions>
        <RowDefinition Height="*"/>
        <RowDefinition Height="*" />
        <RowDefinition Height="50" />
    </Grid.RowDefinitions>
    <TextBox x:Name="inputBox" Grid.Row="0" TextWrapping="Wrap" Text="Enter a paragraph" Margin="10" AcceptsReturn="True" />
    <TextBlock x:Name="outputBlock" Grid.Row="1" TextWrapping="Wrap" Text="Result will be here" Margin="10" />
    <Button x:Name="button" Grid.Row="2" Content="Get Result" />
</Grid>
 ```
 
### <a name="step-5-proceed-to-add-entity-linking-intelligence-service">Stap 5: Gaat u verder met de entiteit koppelen Intelligence Service toevoegen</a>
 
De gebruikersinterface wordt nu gemaakt. Voordat u de service voor het koppelen van de entiteit, moeten we de handler voor de knop toevoegen. Open **MainPage.xaml** van **Solution Explorer**. Een button_Click-handler toevoegen aan het einde van de knop.
 
 ```XAML
 <Button x:Name="button" Grid.Row="2" Content="Get Result" Click="button_Click" />
 ```
 
Een knop-handler moet worden geïmplementeerd in de code. Open **MainPage.xaml.cs** van **Solution Explorer** voor het implementeren van de knop klik. De EntityLinkingServiceClient is een wrapper voor het ophalen van antwoorden entiteit koppelen. De constructorargument van EntityLinkingServiceClient is de sleutel van het abonnement cognitieve Services. Plakken in de abonnementssleutel die u hebt verkregen in **stap 1** aanroepen van de service voor het koppelen van de entiteit. 

Hieronder vindt u de voorbeeldcode, die wordt de 'wikipediaId' toegevoegd aan het antwoord met behulp van de Service voor het koppelen van entiteit. 
 
 ```csharp
 private async void button_Click(object sender, RoutedEventArgs e)
{
    var text = this.inputBox.Text;
    var client = new EntityLinkingServiceClient("Your subscription key","https://api.labs.cognitive.microsoft.com");
    var linkResponse = await client.LinkAsync(text);
    var result = string.Join(", ", linkResponse.Select(i => i.WikipediaID).ToList());
    this.outputBlock.Text = result;
}
 ```
 
U bent nu klaar om uit te voeren van uw eerste natuurlijke taal entiteit koppelen App wordt verwerkt. Druk op de **F5-toets** te compileren en start de toepassing. Plak tekstfragmenten of alinea's in het invoervak. Druk op de knop 'Resultaat ophalen' en bekijk de geïdentificeerde entiteiten in de uitvoerblok.
 
 ![Resultaat van de UWP-voorbeeld](./Images/DemoCodeResult.png)
 
### <a name="summary">Samenvatting</a>
 
In deze zelfstudie hebt u geleerd hoe een toepassing voor het benutten van entiteit koppelen Intelligence Service clientbibliotheek met slechts een paar regels van C# en XAML code maken. 

