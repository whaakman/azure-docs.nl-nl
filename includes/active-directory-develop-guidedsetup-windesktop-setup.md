---
title: bestand opnemen
description: bestand opnemen
services: active-directory
documentationcenter: dev-center-name
author: jmprieur
manager: CelesteDG
editor: ''
ms.service: active-directory
ms.devlang: na
ms.topic: include
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 09/17/2018
ms.author: jmprieur
ms.custom: include file
ms.openlocfilehash: 2a7734f729c4b1db7e8c0b4571e8792373ee11ae
ms.sourcegitcommit: dec7947393fc25c7a8247a35e562362e3600552f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/19/2019
ms.locfileid: "58203179"
---
## <a name="set-up-your-project"></a>Uw project instellen

In deze sectie maakt u een nieuw project om u te laten zien hoe u een Windows Desktop .NET-toepassing (XAML) integreren met *aanmelden met Microsoft* zodat de toepassing query kunt uitvoeren op Web-API's waarvoor een token.

De toepassing die u met deze handleiding maakt geeft een knop die wordt gebruikt voor het aanroepen van een grafiek, een ruimte om de resultaten op het scherm weer te geven en een knop Afmelden.

> [!NOTE]
> Voorkeur voor het downloaden van dit voorbeeld Visual Studio-project in plaats daarvan? [Een project hebt gedownload](https://github.com/Azure-Samples/active-directory-dotnet-desktop-msgraph-v2/archive/master.zip), en gaat u naar de [configuratiestap](#register-your-application) het codevoorbeeld configureren voordat u deze uitvoert.
>

Voor het maken van uw toepassing, het volgende doen:

1. Selecteer in Visual Studio **Bestand** > **Nieuw** > **Project**.
2. Onder **sjablonen**, selecteer **Visual C#**.
3. Selecteer **WPF-App (.NET Framework)**, afhankelijk van de versie van Visual Studio-versie die u gebruikt.

## <a name="add-msal-to-your-project"></a>MSAL toevoegen aan uw project

1. Selecteer in Visual Studio **Tools** > **NuGet Package Manager**> **Package Manager Console**.
2. Plak de volgende Azure PowerShell-opdracht in het venster Package Manager Console:

    ```powershell
    Install-Package Microsoft.Identity.Client
    ```

    > [!NOTE] 
    > Met deze opdracht installeert Microsoft Authentication Library. MSAL verwerkt ophalen, opslaan in cache en vernieuwen van gebruikerstokens die worden gebruikt voor toegang tot de API's die worden beveiligd door Azure Active Directory v2.0
    >

## <a name="add-the-code-to-initialize-msal"></a>Voeg de code voor het initialiseren van MSAL

In deze stap maakt u een klasse voor het afhandelen van interactie met MSAL, zoals de verwerking van tokens.

1. Open de *App.xaml.cs* bestand en voeg de referentie voor MSAL toe aan de klasse:

    ```csharp
    using Microsoft.Identity.Client;
    ```
   <!-- Workaround for Docs conversion bug -->

2. De klasse app bijwerken met het volgende:

    ```csharp
    public partial class App : Application
    {
        //Below is the clientId of your app registration. 
        //You have to replace the below with the Application Id for your app registration
        private static string ClientId = "your_client_id_here";

        public static PublicClientApplication PublicClientApp = new PublicClientApplication(ClientId);

    }
    ```

## <a name="create-the-application-ui"></a>Gebruikersinterface van de toepassing maken

Deze sectie wordt beschreven hoe een toepassing query kunt uitvoeren op een beveiligde back-end-server, zoals Microsoft Graph. 

Een *MainWindow.xaml* bestand automatisch moet worden gemaakt als onderdeel van de projectsjabloon, maken. Dit bestand openen en vervolgens vervangen van uw toepassing  *\<raster >* knooppunt met de volgende code:

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
