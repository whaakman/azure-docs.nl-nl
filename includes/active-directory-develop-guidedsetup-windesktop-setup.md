---
title: bestand opnemen
description: bestand opnemen
services: active-directory
documentationcenter: dev-center-name
author: andretms
manager: mtillman
editor: ''
ms.assetid: 820acdb7-d316-4c3b-8de9-79df48ba3b06
ms.service: active-directory
ms.devlang: na
ms.topic: include
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 04/19/2018
ms.author: andret
ms.custom: include file
ms.openlocfilehash: d407b015699925a6a3ce6ef9108ace1e9900303f
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/28/2018
ms.locfileid: "32200613"
---
## <a name="set-up-your-project"></a>Instellen van uw project

In deze sectie u een nieuwe project maken om te laten zien hoe u een Windows Desktop .NET-toepassing (XAML) worden geïntegreerd met *aanmelden met Microsoft* zodat de toepassing kan Web-API's die een token vereisen een query.

De toepassing die u met deze handleiding maakt wordt weergegeven een knop die wordt gebruikt voor het aanroepen van een grafiek, een gebied om de resultaten op het scherm weer te geven en een knop Afmelden.

> [!NOTE]
> Voorkeur voor het downloaden van dit voorbeeld Visual Studio-project in plaats daarvan? [Downloaden van een project](https://github.com/Azure-Samples/active-directory-dotnet-desktop-msgraph-v2/archive/master.zip), en ga verder met de [configuratiestap](#register-your-application) het codevoorbeeld configureren voordat u deze uitvoert.
>

Voor het maken van uw toepassing, het volgende doen:
1. Selecteer in Visual Studio **bestand** > **nieuw** > **Project**.
2. Onder **sjablonen**, selecteer **Visual C#**.
3. Selecteer **WPF-App** of **WPF-toepassing**, afhankelijk van de versie van Visual Studio-versie die u gebruikt.

## <a name="add-msal-to-your-project"></a>MSAL toevoegen aan uw project
1. Selecteer in Visual Studio **extra** > **NuGet Package Manager**> **Package Manager Console**.
2. Plak de volgende Azure PowerShell-opdracht in het venster Package Manager-Console:

    ```powershell
    Install-Package Microsoft.Identity.Client -Pre
    ```

    > [!NOTE] 
    > Met deze opdracht installeert Microsoft Authentication Library. MSAL verwerkt ophalen, opslaan in cache en gebruikerstokens die worden gebruikt voor toegang tot de API's die worden beveiligd door Azure Active Directory-v2 vernieuwen.
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

## <a name="create-the-application-ui"></a>De UI-toepassing maken

Deze sectie wordt beschreven hoe een toepassing een beveiligde back-endserver zoals Microsoft Graph kan een query. 

Een *MainWindow.xaml* bestand automatisch moet worden gemaakt als onderdeel van uw project-sjabloon. Dit bestand openen en vervangt u uw toepassing  *\<raster >* knooppunt met de volgende code:

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

