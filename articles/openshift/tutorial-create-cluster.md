---
title: 'Zelfstudie: een Azure Red Hat OpenShift-cluster maken | Microsoft Docs'
description: Informatie over het maken van een Microsoft Azure Red Hat OpenShift-cluster met behulp van de Azure CLI
services: container-service
author: TylerMSFT
ms.author: twhitney
manager: jeconnoc
ms.topic: tutorial
ms.service: openshift
ms.date: 05/06/2019
ms.openlocfilehash: 5bc71a2d0f29fed163fb5c93ebd27df7f66a1325
ms.sourcegitcommit: 0ae3139c7e2f9d27e8200ae02e6eed6f52aca476
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/06/2019
ms.locfileid: "65080756"
---
# <a name="tutorial-create-an-azure-red-hat-openshift-cluster"></a>Zelfstudie: Een Azure Red Hat OpenShift-cluster maken

Deze zelfstudie is deel één van een serie. U leert hoe u een Microsoft Azure Red Hat OpenShift cluster maken met de Azure CLI, de schaal vervolgens verwijderen voor het opschonen van resources.

In deel één van de serie, leert u hoe u:

> [!div class="checklist"]
> * Een Azure Red Hat OpenShift-cluster maken

In deze zelfstudiereeks leert u het volgende:
> [!div class="checklist"]
> * Een Azure Red Hat OpenShift-cluster maken
> * [Een Azure Red Hat OpenShift-cluster schalen](tutorial-scale-cluster.md)
> * [Een Azure Red Hat OpenShift-cluster verwijderen](tutorial-delete-cluster.md)

## <a name="prerequisites"></a>Vereisten

Voor u met deze zelfstudie begint:

Zorg ervoor dat u hebt [uw ontwikkelomgeving instellen](howto-setup-environment.md), waaronder:
- Installatie van de meest recente CLI
- Het maken van een tenant
- Het maken van een Azure-toepassing-object
- Het maken van een Active Directory-gebruiker aanmelden bij apps die worden uitgevoerd op het cluster gebruikt.

## <a name="step-1-sign-in-to-azure"></a>Stap 1: Aanmelden bij Azure

Als u de Azure CLI lokaal uitvoert, opent u een opdracht Bash-shell en werken met `az login` zich aanmeldt bij Azure.

```bash
az login
```

 Als u toegang tot meerdere abonnementen hebt, voert u `az account set -s {subscription ID}` vervangen `{subscription ID}` aan het abonnement dat u wilt gebruiken.

## <a name="step-2-create-an-azure-red-hat-openshift-cluster"></a>Stap 2: Een Azure Red Hat OpenShift-cluster maken

Stel in het opdrachtvenster Bash de volgende variabelen:

> [!IMPORTANT]
> De naam van het cluster mag alleen kleine letters bevatten of maken van een cluster mislukt.

```bash
CLUSTER_NAME=<cluster name in lowercase>
```

 Gebruik dezelfde naam voor het cluster dat u hebt gekozen in stap 6 van [maken van nieuwe app-registratie](howto-aad-app-configuration.md#create-a-new-app-registration).

```bash
LOCATION=<location>
```

Kies een locatie om uw cluster te maken. Zie voor een lijst van azure-regio's die ondersteuning biedt voor OpenShift op Azure, [ondersteunde regio's](supported-resources.md#azure-regions). Bijvoorbeeld: `LOCATION=eastus`.

Stel `FQDN` aan de volledig gekwalificeerde naam van het cluster. Deze naam bestaat uit de naam van het cluster, de locatie en `.cloudapp.azure.com` toegevoegd aan het einde. Dit is hetzelfde als de aanmeldings-URL u hebt gemaakt in stap 6 van [maken van nieuwe app-registratie](howto-aad-app-configuration.md#create-a-new-app-registration). Bijvoorbeeld:  

```bash
FQDN=$CLUSTER_NAME.$LOCATION.cloudapp.azure.com
```

Stel `APPID` op de waarde die u hebt opgeslagen in stap 9 van [maken van een nieuwe app-registratie](howto-aad-app-configuration.md#create-a-new-app-registration).  

```bash
APPID=<app ID value>
```

Stel `SECRET` op de waarde die u hebt opgeslagen in stap 6 van [maken van een clientgeheim](howto-aad-app-configuration.md#create-a-client-secret).  

```bash
SECRET=<secret value>
```

Stel `TENANT` aan de tenant-ID-waarde die u hebt opgeslagen in stap 7 van [een nieuwe tenant maken](howto-create-tenant.md#create-a-new-azure-ad-tenant)  

```bash
TENANT=<tenant ID>
```

Maak de resourcegroep voor het cluster. Voer de volgende opdracht uit vanuit de Bash-shell die u gebruikt voor het definiëren van de bovenstaande variabelen:

```bash
az group create --name $CLUSTER_NAME --location $LOCATION
```

### <a name="optional-connect-the-clusters-virtual-network-to-an-existing-virtual-network"></a>Optioneel: Virtueel netwerk van het cluster verbinden met een bestaand virtueel netwerk

Als u niet nodig hebt om het virtuele netwerk (VNET) van het cluster dat u maakt een bestaand VNET verbinding te maken, moet u deze stap overslaan.

Haal eerst de id van het bestaande VNET. De id van het formulier zijn: `/subscriptions/{subscription id}/resourceGroups/{resource group of VNET}/providers/Microsoft.Network/virtualNetworks/{VNET name}`.

Als u niet weet wat de netwerknaam of de resourcegroep die het bestaande VNET behoort, gaat u naar de [blade virtuele netwerken](https://ms.portal.azure.com/#blade/HubsExtension/BrowseResourceBlade/resourceType/Microsoft.Network%2FvirtualNetworks) en klik op het virtuele netwerk. Op de pagina virtueel netwerk wordt weergegeven en wordt de naam van het netwerk en de resourcegroep die hoort bij een lijst.

Definieer een VNET_ID-variabele met de volgende CLI-opdracht in een BASH-shell:

```bash
VNET_ID=$(az network vnet show -n {VNET name} -g {VNET resource group} --query id -o tsv)
```

Bijvoorbeeld: `VNET_ID=$(az network vnet show -n MyVirtualNetwork -g MyResourceGroup --query id -o tsv`

### <a name="create-the-cluster"></a>Het cluster maken

U bent nu klaar om een cluster te maken.

 Als u het virtuele netwerk van het cluster geen verbinding met een bestaand virtueel netwerk, laat u de afsluitende `--vnet-peer-id $VNET_ID` parameter in het volgende voorbeeld.

```bash
az openshift create --resource-group $CLUSTER_NAME --name $CLUSTER_NAME -l $LOCATION --fqdn $FQDN --aad-client-app-id $APPID --aad-client-app-secret $SECRET --aad-tenant-id $TENANT --vnet-peer-id $VNET_ID
```

Na een paar minuten `az openshift create` wordt voltooid en retourneert een JSON-antwoord met de clusterdetails van uw.

> [!NOTE]
> Als er een fout optreedt met de naam van de host is niet beschikbaar, is het mogelijk omdat de clusternaam van uw niet uniek is. Probeer de registratie van uw oorspronkelijke app verwijderen en opnieuw uitvoeren van de stappen in [maken een nieuwe app-registratie] (howto-aad-app-configuration.md#create-a-new-app-registration) (als de laatste stap van het maken van een nieuwe gebruiker, omdat u al hebt gemaakt) met een andere clusternaam op.

## <a name="step-3-sign-in-to-the-openshift-console"></a>Stap 3: Meld u aan bij de console van OpenShift

Nu u kunt zich aanmeldt bij de OpenShift-console voor het nieuwe cluster. De [OpenShift-webconsole](https://docs.openshift.com/dedicated/architecture/infrastructure_components/web_console.html) kunt u visualiseren, bladeren en de inhoud van uw OpenShift-projecten te beheren.

We zich moet aanmelden als de [nieuwe Azure AD-gebruiker](howto-aad-app-configuration.md#create-a-new-active-directory-user) u hebt gemaakt voor het testen. Om dit te doen, moet u een nieuwe browsersessie die nog niet in cache opgeslagen de identiteit die u normaal gesproken kunt aanmelden bij de Azure-portal.

1. Open een *incognito* venster (Chrome) of *InPrivate* venster (Microsoft Edge).
2. Navigeer naar de aanmeldings-URL die u hebt gemaakt in stap 6 van [maken van een nieuwe app-registratie](howto-aad-app-configuration.md#create-a-new-app-registration). Bijvoorbeeld: https://constoso.eastus.cloudapp.azure.com

> [!NOTE]
> De OpenShift-console gebruikt een zelfondertekend certificaat.
> Wanneer u hierom wordt gevraagd in uw browser, de waarschuwing negeren en accepteren van de 'niet-vertrouwd' certificaat.

Meld u aan met de gebruiker en het wachtwoord die u hebt gemaakt in [maken van een nieuwe Active Directory-gebruiker](howto-aad-app-configuration.md#create-a-new-active-directory-user) wanneer de **machtigingen aangevraagd** dialoogvenster wordt weergegeven, selecteert u **toestemming namens uw organisatie**  en vervolgens **accepteren**.

U bent nu aangemeld bij de clusterconsole.

[Schermafbeelding van de console van de cluster OpenShift](./media/aro-console.png)

 Vindt u meer informatie over [met behulp van de console OpenShift](https://docs.openshift.com/dedicated/getting_started/developers_console.html) te maken en installatiekopieën ingebouwd in de [Red Hat OpenShift](https://docs.openshift.com/dedicated/welcome/index.html) documentatie.

## <a name="step-4-install-the-openshift-cli"></a>Stap 4: Installeer de CLI OpenShift

De [OpenShift CLI](https://docs.openshift.com/dedicated/cli_reference/get_started_cli.html) (of *OC extra*) bieden opdrachten voor het beheren van uw toepassingen en hulpprogramma's op lagere niveaus voor interactie met de verschillende onderdelen van uw cluster OpenShift.

Klik op het vraagteken in de rechterbovenhoek van de naam van uw aanmelding in de console van OpenShift en selecteer **opdrachtregelprogramma's**.  Ga als volgt de **nieuwste Release** koppeling downloaden en installeren van de ondersteunde oc CLI voor Linux, MacOS of Windows.

> [!NOTE]
> Als u niet het vraagtekenpictogram in de rechterbovenhoek ziet, selecteert u *Servicecatalogus* of *Application Console* in de bovenste links vervolgkeuzelijst.
>
> Ook kunt u [downloaden van de oc CLI](https://www.okd.io/download.html) rechtstreeks.

De **opdrachtregelprogramma's** pagina vindt u een opdracht van het formulier `oc login https://<your cluster name>.<azure region>.cloudapp.azure.com --token=<token value>`.  Klik op de *naar Klembord kopiëren* knop om te kopiëren van deze opdracht.  In een terminalvenster [uw pad](https://docs.okd.io/latest/cli_reference/get_started_cli.html#installing-the-cli) om op te nemen van de lokale installatie van de hulpprogramma's voor oc. Meld u aan het cluster met behulp van de oc CLI-opdracht die u hebt gekopieerd.

Als de token waarde met behulp van de bovenstaande stappen kan niet worden opgehaald, krijgt u de token waarde uit: `https://<your cluster name>.<azure region>.cloudapp.azure.com/oauth/token/request`.

## <a name="next-steps"></a>Volgende stappen

In dit deel van de zelfstudie hebt u het volgende geleerd:

> [!div class="checklist"]
> * Een Azure Red Hat OpenShift-cluster maken

Ga door naar de volgende zelfstudie:
> [!div class="nextstepaction"]
> [Een Azure Red Hat OpenShift-cluster schalen](tutorial-scale-cluster.md)