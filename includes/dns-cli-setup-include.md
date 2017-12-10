## <a name="set-up-azure-cli-for-azure-dns"></a>Set up Azure CLI for Azure DNS (Azure CLI instellen voor Azure DNS)

### <a name="before-you-begin"></a>Voordat u begint

Controleer voordat u met de configuratie begint of u de volgende items hebt.

* Een Azure-abonnement. Als u nog geen Azure-abonnement hebt, kunt u [uw voordelen als MSDN-abonnee activeren](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) of [u aanmelden voor een gratis account](https://azure.microsoft.com/pricing/free-trial/).
* Installeer de nieuwste versie van de Azure CLI. Deze is beschikbaar voor Windows, Linux en MAC. Lees [Azure-CLI installeren](../articles/cli-install-nodejs.md) voor meer informatie.

### <a name="sign-in-to-your-azure-account"></a>Aanmelden bij uw Azure-account

Open een consolevenster en doorloop de verificatie met uw referenties. Raadpleeg [Log in to Azure from the Azure CLI (Aanmelden bij Azure vanaf de Azure CLI)](/cli/azure/authenticate-azure-cli) voor meer informatie.

```azurecli
azure login
```

### <a name="switch-cli-mode"></a>Naar de CLI-modus schakelen

Azure DNS maakt gebruik van Azure Resource Manager. Zorg ervoor dat u overschakelt naar de CLI-modus om de Azure Resource Manager-opdrachten te kunnen gebruiken.

```azurecli
azure config mode arm
```

### <a name="select-the-subscription"></a>Het abonnement selecteren

Controleer de abonnementen voor het account.

```azurecli
azure account list
```

Kies welk Azure-abonnement u wilt gebruiken.

```azurecli
azure account set "subscription name"
```

### <a name="create-a-resource-group"></a>Een resourcegroep maken

Azure Resource Manager vereist dat er voor alle resourcegroepen een locatie wordt opgegeven. Deze locatie wordt gebruikt als de standaardlocatie voor resources in die resourcegroep. Aangezien alle DNS-resources globaal en niet regionaal zijn, is de keuze van de locatie voor de resourcegroep niet van invloed op Azure DNS.

U kunt deze stap overslaan als u een bestaande resourcegroep gebruikt.

```azurecli
azure group create -n myresourcegroup --location "West US"
```

### <a name="register-resource-provider"></a>Resourceprovider registreren

De Azure DNS-service wordt beheerd door de Microsoft.Network-resourceprovider. Uw Azure-abonnement moet worden geregistreerd voor het gebruik van deze resourceprovider voordat u Azure DNS kunt gebruiken. Dit is een eenmalige bewerking voor elk abonnement.

```azurecli
azure provider register --namespace Microsoft.Network
```

