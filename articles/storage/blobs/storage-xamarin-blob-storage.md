---
title: Het Blob Storage gebruiken met Xamarin | Microsoft Docs
description: De Azure Storage-clientbibliotheek voor Xamarin kunnen ontwikkelaars iOS, Android en Windows Store-apps maken met hun eigen gebruikersinterface. Deze zelfstudie laat zien hoe Xamarin gebruiken voor het maken van een toepassing die gebruikmaakt van Azure Blob-opslag.
services: storage
documentationcenter: xamarin
author: michaelhauss
manager: vamshik
editor: tysonn
ms.assetid: 44cb845d-cf78-4942-95b8-952da4f9a2c2
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 05/11/2017
ms.author: michaelhauss
ms.openlocfilehash: c7b4d0e7d7d95f2e3f8c5a97b78c60c52cc862a0
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="how-to-use-blob-storage-from-xamarin"></a>Het Blob Storage gebruiken met Xamarin
[!INCLUDE [storage-selector-blob-include](../../../includes/storage-selector-blob-include.md)]

## <a name="overview"></a>Overzicht
Xamarin kunnen ontwikkelaars gebruikt een gedeelde C# codebase iOS, Android en Windows Store-apps maken met hun eigen gebruikersinterface. Deze zelfstudie laat zien hoe u Azure Blob storage gebruiken met een Xamarin-toepassing. Als u meer informatie over Azure Storage vooraleer de code, Zie [Inleiding tot Microsoft Azure Storage](../common/storage-introduction.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json).

[!INCLUDE [storage-create-account-include](../../../includes/storage-create-account-include.md)]

[!INCLUDE [storage-mobile-authentication-guidance](../../../includes/storage-mobile-authentication-guidance.md)]

## <a name="create-a-new-xamarin-application"></a>Maak een nieuwe Xamarin-toepassing
Voor deze zelfstudie maakt er een app die gericht is op Android, iOS en Windows. Deze app wordt gewoon een container maken en een blob uploadt naar deze container. We gebruiken hiervoor Visual Studio in Windows, maar de dezelfde geleerde lessen kunnen worden toegepast bij het maken van een app met Xamarin Studio op Mac OS.

Volg deze stappen om uw toepassing te maken:

1. Als u dat nog niet gedaan hebt, downloaden en installeren [Xamarin voor Visual Studio](https://www.xamarin.com/download).
2. Open Visual Studio en maakt een lege App (systeemeigen draagbare): **bestand > Nieuw > Project > platformoverschrijdende > lege App(Native Portable)**.
3. Met de rechtermuisknop op uw oplossing in het deelvenster Solution Explorer en selecteer **NuGet-pakketten beheren voor oplossing**. Zoeken naar **WindowsAzure.Storage** en installeer de nieuwste stabiele versie voor alle projecten in uw oplossing.
4. Ontwikkel en voer uw project.

U hebt nu een toepassing waarmee u op een knop die een teller verhoogd.

## <a name="create-container-and-upload-blob"></a>Container maken en uploaden van blob
Vervolgens onder uw `(Portable)` project, voegt u code naar `MyClass.cs`. Deze code maakt een container en een blob uploadt naar deze container. `MyClass.cs`moet eruitzien als in het volgende:

```csharp
using Microsoft.WindowsAzure.Storage;
using Microsoft.WindowsAzure.Storage.Blob;
using System.Threading.Tasks;

namespace XamarinApp
{
    public class MyClass
    {
        public MyClass ()
        {
        }

        public static async Task performBlobOperation()
        {
            // Retrieve storage account from connection string.
            CloudStorageAccount storageAccount = CloudStorageAccount.Parse("DefaultEndpointsProtocol=https;AccountName=your_account_name_here;AccountKey=your_account_key_here");

            // Create the blob client.
            CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();

            // Retrieve reference to a previously created container.
            CloudBlobContainer container = blobClient.GetContainerReference("mycontainer");

            // Create the container if it doesn't already exist.
            await container.CreateIfNotExistsAsync();

            // Retrieve reference to a blob named "myblob".
            CloudBlockBlob blockBlob = container.GetBlockBlobReference("myblob");

            // Create the "myblob" blob with the text "Hello, world!"
            await blockBlob.UploadTextAsync("Hello, world!");
        }
    }
}
```

Zorg ervoor dat 'your_account_name_here' en "your_account_key_here" vervangen door uw werkelijke accountnaam en accountsleutel. 

Uw iOS-, Android- en Windows Phone-projecten alle verwijzingen hebben naar uw draagbare project - wat betekent dat u alle gedeelde code kunt schrijven in een plaats en deze gebruiken voor alle projecten. U kunt nu de volgende regel code toevoegen aan elk project om te profiteren:`MyClass.performBlobOperation()`

### <a name="xamarinappdroid--mainactivitycs"></a>XamarinApp.Droid > MainActivity.cs

```csharp
using Android.App;
using Android.Widget;
using Android.OS;

namespace XamarinApp.Droid
{
    [Activity (Label = "XamarinApp.Droid", MainLauncher = true, Icon = "@drawable/icon")]
    public class MainActivity : Activity
    {
        int count = 1;

        protected override async void OnCreate (Bundle bundle)
        {
            base.OnCreate (bundle);

            // Set our view from the "main" layout resource
            SetContentView (Resource.Layout.Main);

            // Get our button from the layout resource,
            // and attach an event to it
            Button button = FindViewById<Button> (Resource.Id.myButton);

            button.Click += delegate {
                button.Text = string.Format ("{0} clicks!", count++);
            };

            await MyClass.performBlobOperation();
            }
        }
    }
}
```

### <a name="xamarinappios--viewcontrollercs"></a>XamarinApp.iOS > ViewController.cs

```csharp
using System;
using UIKit;

namespace XamarinApp.iOS
{
    public partial class ViewController : UIViewController
    {
        int count = 1;

        public ViewController (IntPtr handle) : base (handle)
        {
        }

        public override async void ViewDidLoad ()
        {
            int count = 1;

            public ViewController (IntPtr handle) : base (handle)
            {
            }

            public override async void ViewDidLoad ()
            {
                base.ViewDidLoad ();
                // Perform any additional setup after loading the view, typically from a nib.
                Button.AccessibilityIdentifier = "myButton";
                Button.TouchUpInside += delegate {
                    var title = string.Format ("{0} clicks!", count++);
                    Button.SetTitle (title, UIControlState.Normal);
                };

                await MyClass.performBlobOperation();
            }

            public override void DidReceiveMemoryWarning ()
            {
                base.DidReceiveMemoryWarning ();
                // Release any cached data, images, etc that aren't in use.
            }
        }
    }
}
```

### <a name="xamarinappwinphone--mainpagexaml--mainpagexamlcs"></a>XamarinApp.WinPhone > MainPage.xaml > MainPage.xaml.cs

```csharp
using Windows.UI.Xaml.Controls;
using Windows.UI.Xaml.Navigation;

// The Blank Page item template is documented at http://go.microsoft.com/fwlink/?LinkId=391641

namespace XamarinApp.WinPhone
{
    /// <summary>
    /// An empty page that can be used on its own or navigated to within a Frame.
    /// </summary>
    public sealed partial class MainPage : Page
    {
        int count = 1;

        public MainPage()
        {
            this.InitializeComponent();

            this.NavigationCacheMode = NavigationCacheMode.Required;
        }

        /// <summary>
        /// Invoked when this page is about to be displayed in a Frame.
        /// </summary>
        /// <param name="e">Event data that describes how this page was reached.
        /// This parameter is typically used to configure the page.</param>
        protected override async void OnNavigatedTo(NavigationEventArgs e)
        {
            int count = 1;

            public MainPage()
            {
                this.InitializeComponent();

                this.NavigationCacheMode = NavigationCacheMode.Required;
            }

            /// <summary>
            /// Invoked when this page is about to be displayed in a Frame.
            /// </summary>
            /// <param name="e">Event data that describes how this page was reached.
            /// This parameter is typically used to configure the page.</param>
            protected override async void OnNavigatedTo(NavigationEventArgs e)
            {
                // TODO: Prepare page for display here.

                // TODO: If your application contains multiple pages, ensure that you are
                // handling the hardware Back button by registering for the
                // Windows.Phone.UI.Input.HardwareButtons.BackPressed event.
                // If you are using the NavigationHelper provided by some templates,
                // this event is handled for you.
                Button.Click += delegate {
                    var title = string.Format("{0} clicks!", count++);
                    Button.Content = title;
                };

                await MyClass.performBlobOperation();
            }
        }
    }
}
```

## <a name="run-the-application"></a>De toepassing uitvoeren
U kunt deze toepassing nu uitvoeren in een Android- of Windows Phone-emulator. U kunt deze toepassing ook uitvoeren in een iOS-emulator, maar hiervoor moet een Mac. Voor specifieke instructies over hoe u dit doet, lees de documentatie voor [Visual Studio verbinding met een Mac](https://developer.xamarin.com/guides/ios/getting_started/installation/windows/connecting-to-mac/)

Als u uw app hebt uitgevoerd, wordt de container gemaakt `mycontainer` in uw opslagaccount. De blob bevatten moeten `myblob`, die de tekst heeft `Hello, world!`. U kunt dit controleren met behulp van de [Microsoft Azure Storage Explorer](http://storageexplorer.com/).

## <a name="next-steps"></a>Volgende stappen
In deze zelfstudie hebt u geleerd hoe een cross-platform-toepassing maken in Xamarin die gebruikmaakt van Azure Storage, specifiek gericht op een scenario in Blob Storage. Echter, u kunt doen veel meer met niet alleen de Blob-opslag, maar ook met tabel-, bestands- en Queue Storage. Raadpleeg de volgende artikelen voor meer informatie:

* [Aan de slag met Azure Blob Storage met .NET](storage-dotnet-how-to-use-blobs.md)
* [Inleiding tot Azure-bestanden](../files/storage-files-introduction.md)
* [Ontwikkelen voor Azure Files met .NET](../files/storage-dotnet-how-to-use-files.md)
* [Aan de slag met Azure Table Storage met .NET](../../cosmos-db/table-storage-how-to-use-dotnet.md)
* [Aan de slag met Azure Queue Storage met .NET](../queues/storage-dotnet-how-to-use-queues.md)

[!INCLUDE [storage-try-azure-tools-blobs](../../../includes/storage-try-azure-tools-blobs.md)]