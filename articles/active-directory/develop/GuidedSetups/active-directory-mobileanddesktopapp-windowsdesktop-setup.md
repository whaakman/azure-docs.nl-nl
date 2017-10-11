---
title: Azure AD v2 Windows Desktop ophalen gestart - instellingen | Microsoft Docs
description: Hoe Windows Desktop .NET (XAML) toepassingen een API waarvoor toegangstokens door Azure Active Directory-v2-eindpunt kunnen aanroepen
services: active-directory
documentationcenter: dev-center-name
author: andretms
manager: mbaldwin
editor: 
ms.assetid: 820acdb7-d316-4c3b-8de9-79df48ba3b06
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 05/09/2017
ms.author: andret
ms.custom: aaddev
ms.openlocfilehash: 4065727aef04d7969d438c6ef79127bb44568be1
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2017
---
## <a name="set-up-your-project"></a>Instellen van uw project

Deze sectie bevat stapsgewijze instructies voor het maken van een nieuw project te laten zien hoe u een Windows Desktop .NET-toepassing (XAML) worden geïntegreerd met *aanmelden met Microsoft* zodat Web-API's waarvoor een token op te vragen.

De toepassing gemaakt door deze handleiding beschrijft een knop om een grafiek en resultaten weergeven op het scherm een knop Afmelden.

> Voorkeur voor het downloaden van dit voorbeeld Visual Studio-project in plaats daarvan? [Downloaden van een project](https://github.com/Azure-Samples/active-directory-dotnet-desktop-msgraph-v2/archive/master.zip) en doorgaan met de [configuratiestap](#create-an-application-express) voor het configureren van het codevoorbeeld voordat wordt uitgevoerd.


### <a name="create-your-application"></a>Uw toepassing maken
1. In Visual Studio:`File` > `New` > `Project`<br/>
2. Onder *sjablonen*, selecteer`Visual C#`
3. Selecteer `WPF App` (of *WPF-toepassing* afhankelijk van de versie van uw Visual Studio)

## <a name="add-the-microsoft-authentication-library-msal-to-your-project"></a>De Microsoft Authentication Library (MSAL) toevoegen aan uw project
1. In Visual Studio:`Tools` > `Nuget Package Manager` > `Package Manager Console`
2. Kopiëren en plakken in het venster Package Manager-Console het volgende:

```powershell
Install-Package Microsoft.Identity.Client -Pre
```

> Het bovenstaande pakket installeert de Microsoft Authentication Library (MSAL). MSAL verwerkt aanschaf, opslaan in cache en gebruiker toskens gebruikt voor toegang tot API's die zijn beveiligd door Azure Active Directory-v2 te vernieuwen.

## <a name="add-the-code-to-initialize-msal"></a>Voeg de code voor het initialiseren van MSAL
Deze stap helpt u bij het maken van een klasse voor het afhandelen van interactie met MSAL bibliotheek, zoals de verwerking van tokens.

1. Open de `App.xaml.cs` -bestand en voeg de referentie voor MSAL bibliotheek toe aan de klasse:

```csharp
using Microsoft.Identity.Client;
```
<!-- Workaround for Docs conversion bug -->
<ol start="2">
<li>
De klasse App bijwerken met het volgende:
</li>
</ol>

```csharp
public partial class App : Application
{
    //Below is the clientId of your app registration. 
    //You have to replace the below with the Application Id for your app registration
    private static string ClientId = "your_client_id_here";

    public static PublicClientApplication PublicClientApp = new PublicClientApplication(ClientId);

}
```

## <a name="create-your-applications-ui"></a>De gebruikersinterface van uw toepassing maken
De onderstaande sectie wordt beschreven hoe een toepassing een beveiligde back-endserver zoals Microsoft Graph kan een query. Een bestand MainWindow.xaml moet automatisch worden gemaakt als onderdeel van uw project-sjabloon. Dit bestand voor dit bestand openen en volg de onderstaande instructies:

Vervangen van uw toepassing `<Grid>` worden met het volgende:

```xml
<Grid>
    <StackPanel Background="Azure">
        <StackPanel Orientation="Horizontal" HorizontalAlignment="Right">
            <Button x:Name="CallGraphButton" Content="Call Microsoft Graph API" HorizontalAlignment="Right" Padding="5" Click="CallGraphButton_Click" Margin="5" FontFamily="Segoe Ui"/>
            <Button x:Name="SignOutButton" Content="Sign-Out" HorizontalAlignment="Right" Padding="5" Click="SignOutButton_Click" Margin="5" Visibility="Collapsed" FontFamily="Segoe Ui"/>
        </StackPanel>
        <Label Content="API Call Results" Margin="0,0,0,-5" FontFamily="Segoe Ui" />
        <TextBox x:Name="ResultText" TextWrapping="Wrap" MinHeight="120" Margin="5" FontFamily="Segoe Ui"/>
        <Label Content="Token Info" Margin="0,0,0,-5" FontFamily="Segoe Ui" />
        <TextBox x:Name="TokenInfoText" TextWrapping="Wrap" MinHeight="70" Margin="5" FontFamily="Segoe Ui"/>
    </StackPanel>
</Grid>
```
