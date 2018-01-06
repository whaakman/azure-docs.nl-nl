## <a name="set-up-the-development-environment"></a>De ontwikkelomgeving instellen

Deze sectie helpt bij het instellen van de ontwikkelomgeving. Dit omvat het maken van een ASP.NET MVC-app, een verbinding verbonden services toe te voegen, toevoegen van een domeincontroller en de vereiste naamruimte richtlijnen geven.

### <a name="create-an-aspnet-mvc-app-project"></a>Een ASP.NET MVC-app-project maken

1. Open Visual Studio.

1. Selecteer in het hoofdmenu **bestand** > **nieuw** > **Project**.

1. In de **nieuw Project** dialoogvenster, **Web** > **ASP.NET-webtoepassing (.NET Framework)**. In de **naam** veld **StorageAspNet**. Selecteer **OK**.

    ![Schermopname van dialoogvenster](./media/vs-storage-aspnet-getting-started-setup-dev-env/vs-storage-aspnet-getting-started-setup-dev-env-1.png)

1. In de **nieuwe ASP.NET-webtoepassing** dialoogvenster, **MVC**, en selecteer vervolgens **OK**.

    ![Schermopname van nieuw ASP.NET-webtoepassing dialoogvenster](./media/vs-storage-aspnet-getting-started-setup-dev-env/vs-storage-aspnet-getting-started-setup-dev-env-2.png)

### <a name="use-connected-services-to-connect-to-an-azure-storage-account"></a>Verbonden services gebruiken voor verbinding met een Azure storage-account

1. In **Solution Explorer**, met de rechtermuisknop op het project.

2. Selecteer in het contextmenu **toevoegen** > **Connected-Service**.

1. In de **verbonden Services** dialoogvenster, **Cloud-opslag met Azure Storage**, en selecteer vervolgens **configureren**.

    ![In het dialoogvenster Schermafbeelding van de verbonden Services](./media/vs-storage-aspnet-getting-started-setup-dev-env/vs-storage-aspnet-getting-started-setup-dev-env-3.png)

1. In de **Azure Storage** in het dialoogvenster Selecteer Azure storage-account moet worden gebruikt voor deze zelfstudie. U maakt een nieuwe Azure storage-account selecteren **maken van een nieuw Opslagaccount**, en het formulier te voltooien. Na het selecteren van een bestaand opslagaccount of een nieuwe maken, selecteer **toevoegen**. Visual Studio de NuGet-pakket installeert voor Azure-opslag- en storage-verbindingsreeksen **Web.config**.

> [!TIP]
> Voor informatie over het maken van een opslagaccount met de [Azure-portal](https://portal.azure.com), Zie [een opslagaccount maken](../articles/storage/common/storage-create-storage-account.md#create-a-storage-account).
>
> U kunt ook een opslagaccount maken met behulp van [Azure PowerShell](../articles/storage/common/storage-powershell-guide-full.md), [Azure CLI](../articles/storage/common/storage-azure-cli.md), of [Azure Cloud Shell](../articles/cloud-shell/overview.md).

