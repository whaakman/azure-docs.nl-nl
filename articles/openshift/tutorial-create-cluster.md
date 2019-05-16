---
title: 'Zelfstudie: een Azure Red Hat OpenShift-cluster maken | Microsoft Docs'
description: Informatie over het maken van een Microsoft Azure Red Hat OpenShift-cluster met behulp van de Azure CLI
services: container-service
author: TylerMSFT
ms.author: twhitney
manager: jeconnoc
ms.topic: tutorial
ms.service: openshift
ms.date: 05/14/2019
ms.openlocfilehash: a85397dae67eb06fecbf5e2410f56e2aad808dc8
ms.sourcegitcommit: 36c50860e75d86f0d0e2be9e3213ffa9a06f4150
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/16/2019
ms.locfileid: "65750082"
---
# <a name="tutorial-create-an-azure-red-hat-openshift-cluster"></a>Zelfstudie: Een Azure Red Hat OpenShift-cluster maken

Deze zelfstudie is deel één van een serie. U leert hoe u een Microsoft Azure Red Hat OpenShift cluster maken met de Azure CLI, de schaal vervolgens verwijderen voor het opschonen van resources.

In deel één van de serie, leert u hoe u:

> [!div class="checklist"]
> * Een Azure Red Hat OpenShift-cluster maken

In deze zelfstudiereeks leert u het volgende:
> [!div class="checklist"]
> * Een Azure Red Hat OpenShift-cluster maken
> * [De schaal van een Azure Red Hat OpenShift-cluster wijzigen](tutorial-scale-cluster.md)
> * [Een Azure Red Hat OpenShift-cluster verwijderen](tutorial-delete-cluster.md)

## <a name="prerequisites"></a>Vereisten

> [!IMPORTANT]
> In deze zelfstudie is versie 2.0.65 van de Azure CLI vereist.
>    
> Voordat u Azure Red Hat Openshift gebruiken kunt, moet u kopen van gereserveerde virtuele Azure-machine-instanties, zoals beschreven in [instellen van uw ontwikkelomgeving Azure Red Hat Openshift](howto-setup-environment.md#purchase-azure-virtual-machine-reserved-instances).

Voor u met deze zelfstudie begint:

Zorg ervoor dat u hebt [uw ontwikkelomgeving instellen](howto-setup-environment.md), waaronder:
- Installatie van de meest recente CLI (versie 2.0.65 of hoger)
- Het maken van een tenant als u er nog geen hebt
- Het maken van een Azure-toepassing-object als u er nog niet hebt
- Het maken van een beveiligingsgroep
- Het maken van een Active Directory-gebruiker zich aanmeldt bij het cluster.

## <a name="step-1-sign-in-to-azure"></a>Stap 1: Aanmelden bij Azure

Als u de Azure CLI lokaal uitvoert, opent u een opdracht Bash-shell en werken met `az login` zich aanmeldt bij Azure.

```bash
az login
```

 Als u toegang tot meerdere abonnementen hebt, voert u `az account set -s {subscription ID}` vervangen `{subscription ID}` aan het abonnement dat u wilt gebruiken.

## <a name="step-2-create-an-azure-red-hat-openshift-cluster"></a>Stap 2: Een Azure Red Hat OpenShift-cluster maken

Stel in een Bash-opdrachtvenster de volgende variabelen:

> [!IMPORTANT]
> Kies een naam op voor de u-cluster die uniek is en het maken van alle kleine letters of het cluster zal mislukken.

```bash
CLUSTER_NAME=<cluster name in lowercase>
```

Kies een locatie om uw cluster te maken. Zie voor een lijst van azure-regio's die ondersteuning biedt voor OpenShift op Azure, [ondersteunde regio's](supported-resources.md#azure-regions). Bijvoorbeeld: `LOCATION=eastus`.

```bash
LOCATION=<location>
```

Stel `APPID` op de waarde die u hebt opgeslagen in stap 5 van [maken van een Azure AD-app-registratie](howto-aad-app-configuration.md#create-an-azure-ad-app-registration).  

```bash
APPID=<app ID value>
```

'Groeps-id' ingesteld op de waarde die u hebt opgeslagen in stap 10 van [maken van een Azure AD-beveiligingsgroep](howto-aad-app-configuration.md#create-an-azure-ad-security-group).

```bash
GROUPID=<group ID value>
```

Stel `SECRET` op de waarde die u hebt opgeslagen in stap 8 van [maken van een clientgeheim](howto-aad-app-configuration.md#create-a-client-secret).  

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

Als u niet nodig hebt om het virtuele netwerk (VNET) van het cluster dat u maken met een bestaande VNET via peering verbinding te maken, moet u deze stap overslaan.

Haal eerst de id van het bestaande VNET. De id van het formulier zijn: `/subscriptions/{subscription id}/resourceGroups/{resource group of VNET}/providers/Microsoft.Network/virtualNetworks/{VNET name}`.

Als u niet weet wat de netwerknaam of de resourcegroep die het bestaande VNET behoort, gaat u naar de [blade virtuele netwerken](https://ms.portal.azure.com/#blade/HubsExtension/BrowseResourceBlade/resourceType/Microsoft.Network%2FvirtualNetworks) en klik op het virtuele netwerk. Op de pagina virtueel netwerk wordt weergegeven en wordt de naam van het netwerk en de resourcegroep die hoort bij een lijst.

Definieer een VNET_ID-variabele met de volgende CLI-opdracht in een BASH-shell:

```bash
VNET_ID=$(az network vnet show -n {VNET name} -g {VNET resource group} --query id -o tsv)
```

Bijvoorbeeld: `VNET_ID=$(az network vnet show -n MyVirtualNetwork -g MyResourceGroup --query id -o tsv`

### <a name="create-the-cluster"></a>Het cluster maken

U bent nu klaar om een cluster te maken. De volgende maakt het cluster in de opgegeven Azure AD-tenant, geeft u het object voor Azure AD-app en het geheim te gebruiken als een beveiligings-principal en de beveiligingsgroep die de leden die beheerderstoegang tot het cluster hebben bevat.

Als u **niet** peering van het cluster met een virtueel netwerk, gebruik de volgende opdracht:

```bash
az openshift create --resource-group $CLUSTER_NAME --name $CLUSTER_NAME -l $LOCATION --aad-client-app-id $APPID --aad-client-app-secret $SECRET --aad-tenant-id $TENANT --customer-admin-group-id $GROUPID
```

Als u **zijn** peering van het cluster met een virtueel netwerk, gebruik de volgende opdracht die wordt toegevoegd de `--vnet-peer` vlag:
 
```bash
az openshift create --resource-group $CLUSTER_NAME --name $CLUSTER_NAME -l $LOCATION --aad-client-app-id $APPID --aad-client-app-secret $SECRET --aad-tenant-id $TENANT --customer-admin-group-id $GROUPID --vnet-peer $VNET_ID
```

> [!NOTE]
> Als er een fout optreedt met de naam van de host is niet beschikbaar, is het mogelijk omdat de clusternaam van uw niet uniek is. Probeer de registratie van uw oorspronkelijke app verwijderen en opnieuw uitvoeren van de stappen met een andere clusternaam op in [maken een nieuwe app-registratie] (howto-aad-app-configuration.md#create-a-new-app-registration), weg van de stap voor het maken van een nieuwe groep van gebruikers en beveiligingsgroepen.

Na een paar minuten `az openshift create` wordt voltooid.

### <a name="get-the-sign-in-url-for-your-cluster"></a>Het teken in de URL voor uw cluster ophalen

Haal de URL aan te melden met uw cluster met de volgende opdracht:

```bash
az openshift show -n $CLUSTER_NAME -g $CLUSTER_NAME
```

Zoek de `publicHostName` in de uitvoer, bijvoorbeeld: `"publicHostname": "openshift.xxxxxxxxxxxxxxxxxxxx.eastus.azmosa.io"`

Het teken in de URL voor uw cluster zal worden `https://` gevolgd door de `publicHostName` waarde.  Bijvoorbeeld: `https://openshift.xxxxxxxxxxxxxxxxxxxx.eastus.azmosa.io`.  U gebruikt deze URI in de volgende stap als onderdeel van de omleidings-URI van app-registratie.

## <a name="step-3-update-your-app-registration-redirect-uri"></a>Stap 3: De omleidings-URI van de registratie in app bijwerken

Nu dat u het teken in de URL voor het cluster hebt, stelt u de app-registratie-omleiding gebruikersinterface:

1. Open de [blade App-registraties](https://portal.azure.com/#blade/Microsoft_AAD_IAM/ActiveDirectoryMenuBlade/RegisteredAppsPreview).
2. Klik op uw app-registratie-object.
3. Klik op **Voeg een omleidings-URI**.
4. Zorg ervoor dat **TYPE** is **Web** en stel de **OMLEIDINGS-URI** met behulp van het volgende patroon: `https://<public host name>/oauth2callback/Azure%20AD`. Bijvoorbeeld: `https://openshift.xxxxxxxxxxxxxxxxxxxx.eastus.azmosa.io/oauth2callback/Azure%20AD`
5. Klik op **Opslaan**

## <a name="step-4-sign-in-to-the-openshift-console"></a>Stap 4: Meld u aan bij de console van OpenShift

U bent nu klaar om aan te melden bij de OpenShift-console voor het nieuwe cluster. De [OpenShift-webconsole](https://docs.openshift.com/aro/architecture/infrastructure_components/web_console.html) kunt u visualiseren, bladeren en de inhoud van uw OpenShift-projecten te beheren.

U moet een nieuwe browsersessie die nog niet in cache opgeslagen de identiteit die u normaal gesproken kunt aanmelden bij de Azure-portal.

1. Open een *incognito* venster (Chrome) of *InPrivate* venster (Microsoft Edge).
2. Navigeer naar de aanmeldings-URL die u hierboven, opgehaald, bijvoorbeeld: `https://openshift.xxxxxxxxxxxxxxxxxxxx.eastus.azmosa.io`

Meld u aan met behulp van de naam van de gebruiker die u hebt gemaakt in stap 3 van [maken van een nieuwe Azure Active Directory-gebruiker](howto-aad-app-configuration.md#create-a-new-azure-active-directory-user).

Een **machtigingen aangevraagd** dialoogvenster wordt weergegeven. Klik op **toestemming namens uw organisatie** en klik vervolgens op **accepteren**.

U bent nu aangemeld bij de clusterconsole.

![Schermafbeelding van de console van de cluster OpenShift](./media/aro-console.png)

 Meer informatie over [met behulp van de console OpenShift](https://docs.openshift.com/aro/getting_started/developers_console.html) te maken en installatiekopieën ingebouwd in de [Red Hat OpenShift](https://docs.openshift.com/aro/welcome/index.html) documentatie.

## <a name="step-5-install-the-openshift-cli"></a>Stap 5: Installeer de CLI OpenShift

De [OpenShift CLI](https://docs.openshift.com/aro/cli_reference/get_started_cli.html) (of *OC extra*) bieden opdrachten voor het beheren van uw toepassingen en hulpprogramma's op lagere niveaus voor interactie met de verschillende onderdelen van uw cluster OpenShift.

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
> [De schaal van een Azure Red Hat OpenShift-cluster wijzigen](tutorial-scale-cluster.md)