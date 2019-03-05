---
title: 'Zelfstudie: Een Entity Linking-app bouwen - C#'
titlesuffix: Azure Cognitive Services
description: Met behulp van de Entity Linking-API kunt u tekst analyseren en benoemde entiteiten koppelen aan relevante vermeldingen in een kennisbank.
services: cognitive-services
author: DavidLiCIG
manager: nitinme
ms.service: cognitive-services
ms.subservice: entity-linking-intelligence
ms.topic: tutorial
ms.date: 07/06/2016
ms.author: davl
ms.openlocfilehash: fc1bdd5c6ad4829e22af9922c6749e60f842abaf
ms.sourcegitcommit: a8948ddcbaaa22bccbb6f187b20720eba7a17edc
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/21/2019
ms.locfileid: "56594029"
---
# <a name="tutorial-build-an-entity-linking-app-with-c"></a>Zelfstudie: Een Entity Linking-app bouwen met C#

Entity Linking van Microsoft is een hulpprogramma voor het verwerken van natuurlijk taal waarmee u tekst kunt analyseren en benoemde entiteiten kunt koppelen aan relevante vermeldingen in een kennisbank. 

In deze zelfstudie wordt besproken hoe u entiteiten kunt koppelen door de Entity Linking-clientbibliotheek te gebruiken als een NuGet-pakket. 

### <a name="Prerequisites">Vereisten</a>

- Visual Studio 2015
- Een sleutel voor de Microsoft Cognitive Services-API
- De clientbibliotheek en het voorbeeld downloaden
- NuGet-pakket van Microsoft Entity Linking

U kunt de clientbibliotheek van de Entity Linking Intelligence Service-API downloaden via de [SDK](https://www.github.com/microsoft/cognitive-entitylinking-windows). Het gedownloade ZIP-bestand moet worden uitgepakt naar een map van uw keuze. Veel gebruikers kiezen hiervoor de map Visual Studio 2015.

### <a name="step-1-subscribe-entity-linking-intelligence-service-and-get-your-own-key">Stap 1: Abonneren op Entity Linking Intelligence Service en uw sleutel ophalen</a>
Voordat u Entity Linking Intelligence Service gebruikt, moet u zich aanmelden voor een API-sleutel. Zie [Abonnementen](https://www.microsoft.com/cognitive-services/en-us/sign-up). In deze zelfstudie kan zowel de primaire als de secundaire sleutel worden gebruikt.

### <a name="step-2-create-a-new-project-in-visual-studio"> Stap 2: Een nieuw project maken in Visual Studio</a>

Laten we beginnen met het maken van een nieuw project in Visual Studio. Start eerst Visual Studio 2015 vanuit het menu Start. Maak vervolgens een nieuw project door **Geïnstalleerd → Sjablonen → Visual C# → Windows Universal → Lege app** te selecteren voor uw projectsjabloon:

 ![Een universele app maken](./Images/CreateUWP.png)

### <a name="step-3-add-the-entity-linking-nuget-package-to-your-project">Stap 3: Het NuGet-pakket van Entity Linking toevoegen aan uw project</a>

Entity Linking van Cognitive Services is vrijgegeven als een NuGet.org-pakket en moet worden geïnstalleerd voordat u het kunt gebruiken.
Als u dit pakket wilt toevoegen aan uw project, klikt u op het tabblad **Solution Explorer** met de rechtermuisknop op uw project en selecteert u **NuGet-pakketten beheren**.

Selecteer in het venster **NuGet-pakketbeheer** eerst NuGet.org bij **Pakketbron** in de rechterbovenhoek. Selecteer **Bladeren** in de linkerbovenhoek en typ ProjectOxford.EntityLinking in het zoekvak. Selecteer het NuGet-pakket **Microsoft.ProjectOxford.EntityLinking** en klik op **Installeren**.

Zoek vervolgens naar Newtonsoft.Json en installeer het. Als u wordt gevraagd of u wijzigingen wilt controleren, klikt u op **OK**. Als de licentievoorwaarden voor Entity Linking worden weergegeven, klikt u op **Ik ga akkoord**.

Entity Linking is nu geïnstalleerd als onderdeel van uw toepassing. U kunt dit controleren door te kijken of de verwijzing **Microsoft.ProjectOxford.EntityLinking** aanwezig is als onderdeel van uw project in Solution Explorer.

 ![Opgenomen NuGet-bibliotheek in project](./Images/NugetLibraryInProject.png)
 
### <a name="step-4-add-an-input-and-output-text-block-to-your-apps-xaml">Stap 4: Een in- en uitvoertekstblok toevoegen aan de XAML van uw app</a>
Ga naar **MainPage.xaml** in **Solution Explorer** en dubbelklik op het bestand. Het bestand wordt geopend in een nieuw venster. U kunt voor het gemak dubbelklikken op de knop **XAML** op het tabblad **Ontwerper**. Hierdoor wordt **Visuele ontwerper** verborgen en alle ruimte gereserveerd voor de weergave van code.

 ![Opgenomen NuGet-bibliotheek in project](./Images/UWPMainPage.png)
 
Als tekstservice kunt u de functionaliteit het beste visualiseren door een tekstblok voor invoer en een tekstblok voor uitvoer te maken. Hiertoe voegt u de volgende XAML toe in het **raster**. Met deze code worden drie onderdelen toegevoegd: een tekstvak voor invoer, een tekstblok voor uitvoer en een startknop.
 
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
 
### <a name="step-5-proceed-to-add-entity-linking-intelligence-service">Stap 5: Entity Linking Intelligence Service toevoegen</a>
 
De gebruikersinterface is nu gemaakt. Voordat u de Entity Linking-service gaat gebruiken, moeten we de handler voor klikken op de knop toevoegen. Open **MainPage.xaml** vanuit **Solution Explorer**. Voeg een handler voor klikken op de knop toe aan het einde van de knop.
 
 ```XAML
 <Button x:Name="button" Grid.Row="2" Content="Get Result" Click="button_Click" />
 ```
 
Er moet een handler voor klikken op de knop worden geïmplementeerd in de code. Open **MainPage.xaml.cs** vanuit **Solution Explorer** om de handler voor klikken op de knop te implementeren. EntityLinkingServiceClient is een wrapper voor het ophalen van reacties van Entity Linking. Het constructorargument van EntityLinkingServiceClient is de Cognitive Services-abonnementssleutel. Plak de abonnementssleutel die u hebt gekregen in **stap 1** om Entity Linking Intelligence Service aan te roepen. 

Hieronder vindt u voorbeeldcode, waarmee de 'wikipediaId' aan de reactie wordt toegevoegd met behulp van de Entity Linking-service. 
 
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
 
U bent nu klaar om uw eerste Entity Linking-app voor de verwerking van natuurlijke taal uit te voeren. Druk op de **F5-toets** om de toepassing te compileren en te starten. Plak tekstfragmenten of alinea's in het invoervak. Druk op de knop 'Resultaat ophalen' en bekijk de geïdentificeerde entiteiten in het uitvoerblok.
 
 ![Voorbeeldresultaat van UWP](./Images/DemoCodeResult.png)
 
### <a name="summary">Samenvatting</a>
 
In deze zelfstudie hebt u geleerd hoe u een toepassing maakt om gebruik te kunnen maken van de Entity Linking Intelligence Service-clientbibliotheek, met slechts een paar regels code in C# en XAML. 

