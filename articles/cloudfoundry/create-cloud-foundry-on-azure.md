---
title: Cloud Foundry in Azure maken
description: Meer informatie over het instellen van parameters die nodig zijn voor het inrichten van een PCF-cluster voor Cloud Foundry op Azure
services: Cloud Foundry
documentationcenter: CloudFoundry
author: ruyakubu
manager: brunoborges
editor: ruyakubu
ms.assetid: ''
ms.author: ruyakubu
ms.date: 09/13/2018
ms.devlang: ''
ms.service: Cloud Foundry
ms.tgt_pltfrm: multiple
ms.topic: tutorial
ms.workload: web
ms.openlocfilehash: a0a3379a8a2579080d9b686917395feec9cf8f3d
ms.sourcegitcommit: 609c85e433150e7c27abd3b373d56ee9cf95179a
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/03/2018
ms.locfileid: "48250580"
---
# <a name="create-cloud-foundry-on-azure"></a>Cloud Foundry in Azure maken

Deze zelfstudie biedt snelle stappen voor het maken en genereren van parameters voor het inrichten van een PCF-cluster van Pivotal Cloud Foundry op Azure.  De Pivotal Cloud Foundry-oplossing kan worden gevonden door het uitvoeren van een zoekopdracht op Azure [MarketPlace](https://azuremarketplace.microsoft.com/marketplace/apps/pivotal.pivotal-cloud-foundry).

![Alt-tekst voor afbeelding](media/deploy/pcf-marketplace.png "Pivotal Cloud Foundry zoeken in Azure")


## <a name="generate-an-ssh-public-key"></a>Een openbare SSH-sleutel genereren

Er zijn verschillende manieren om een openbare SSH-sleutel te genereren met behulp van Windows, Mac of Linux.

```Bash
ssh-keygen -t rsa -b 2048
```
- Klik hier om [instructies]( https://docs.microsoft.com/azure/virtual-machines/linux/ssh-from-windows) voor uw omgeving te zien.

## <a name="create-a-service-principal"></a>Een service-principal maken

> [!NOTE]
>
> Om een service-principal te maken, is een machtiging voor het eigenaarsaccount vereist.  U kunt bovendien een script schrijven om het maken van de service-principal te automatiseren. Bijvoorbeeld met behulp van Azure CLI [az ad sp create-for-rbac](https://docs.microsoft.com/cli/azure/ad/sp?view=azure-cli-latest).

1. Meld u aan bij uw Azure-account.

    `az login`

    ![Alt-tekst voor afbeelding](media/deploy/az-login-output.png "Aanmelding bij Azure CLI")
 
    Kopieer de waarde 'id' als uw **abonnements-ID** en de **tenantId**-waarde die later zal worden gebruikt.

2. Stel uw standaardabonnement voor deze configuratie in.

    `az account set -s {id}`

3. Maak een AAD-toepassing voor uw PCF en geef een uniek alfanumeriek wachtwoord op.  Sla het wachtwoord op als uw **clientSecret** dat later zal worden gebruikt.

    `az ad app create --display-name "Svc Prinicipal for OpsManager" --password {enter-your-password} --homepage "{enter-your-homepage}" --identifier-uris {enter-your-homepage}`

    Kopieer de waarde 'appId' in de uitvoer als uw **ClientID** die later zal worden gebruikt.

    > [!NOTE]
    >
    > Kies uw eigen toepassingsstartpagina en ID-URI.  Bijvoorbeeld http://www.contoso.com.

4. Maak een service-principal met uw nieuwe 'appId'.

    `az ad sp create --id {appId}`

5. Stel de machtigingsrol van de service-principal in als **Inzender**.

    `az role assignment create --assignee “{enter-your-homepage}” --role “Contributor” `

    Maar u kunt ook gebruikmaken van...

    `az role assignment create --assignee {service-princ-name} --role “Contributor” `

    ![Alt-tekst voor afbeelding](media/deploy/svc-princ.png "Roltoewijzing van de service-principal")

6. Controleer of u zich kunt aanmelden bij uw service-principal met behulp van de appId, het wachtwoord en de tenantid.

    `az login --service-principal -u {appId} -p {your-passward}  --tenant {tenantId}`

7. Maak een .json-bestand in de volgende indeling met behulp van alle bovenstaande waarden voor **abonnements-ID**, **tenantId**, **clientID** en **clientSecret** die u hierboven hebt gekopieerd.  Sla het bestand op.

    ```json
    {
        "subscriptionID": "{enter-your-subscription-Id-here}",
        "tenantID": "{enter-your-tenant-Id-here}",
        "clientID": "{enter-your-app-Id-here}",
        "clientSecret": "{enter-your-key-here}"
    }
    ```

## <a name="get-the-pivotal-network-token"></a>Het Pivotal Network-token ophalen

1. Registreer u of meld u aan bij uw [Pivotal Network](https://network.pivotal.io)-account.
2. Klik op uw profielnaam rechtsboven op de pagina en selecteer **Edit Profile'.
3. Ga naar de onderkant van de pagina en kopieer de **LEGACY API TOKEN**-waarde.  Dit is de waarde van uw **Pivotal Network-token** die later zal worden gebruikt.

## <a name="provision-your-cloud-foundry-on-azure"></a>Cloud Foundry op Azure inrichten

1. Nu hebt u alle parameters die nodig zijn voor het inrichten van uw [Pivotal Cloud Foundry op Azure](https://azuremarketplace.microsoft.com/marketplace/apps/pivotal.pivotal-cloud-foundry)-cluster.
2. Voer de parameters in en maak uw PCF-cluster.

## <a name="verify-the-deployment-and-log-into-the-pivotal-ops-manager"></a>Controleer de implementatie en meld u aan bij Pivotal Ops Manager

1. Er moet nu in uw PCF-cluster een implementatiestatus worden weergegeven.

    ![Alt-tekst voor afbeelding](media/deploy/deployment.png "Status van de Azure-implementatie")

2. Klik op de koppeling **Deployments** in het linkernavigatievenster om referenties voor uw PCF Ops Manager op te halen en klik vervolgens op de **implementatienaam** op de volgende pagina.
3. Klik in het linkernavigatievenster op de koppeling **Outputs** om de URL, de gebruikersnaam en het wachtwoord voor de PCF Ops Manager weer te geven.  De waarde 'OPSMAN-FQDN-naam' is de URL.
 
    ![Alt-tekst voor afbeelding](media/deploy/deploy-outputs.png "Uitvoer voor Cloud Foundry-implementatie")
 
4. Start de URL in een webbrowser en voer de referenties uit de vorige stap in om u aan te melden.

    ![Alt-tekst voor afbeelding](media/deploy/pivotal-login.png "Pivotal-aanmeldingspagina")
         
    > [!NOTE]
    >
    > Als het met Internet Explorer als browser niet lukt vanwege een waarschuwingsbericht dat de site niet veilig is, klikt u op "Meer informatie" en gaat u naar de webpagina.  Klik in Firefox op Advance en voeg de certificering toe om door te gaan.

5. De PCF Ops Manager moet de geïmplementeerde Azure-instanties weergeven. U kunt nu hier beginnen met het implementeren en beheren van uw toepassingen.
               
    ![Alt-tekst voor afbeelding](media/deploy/ops-mgr.png "Azure-instantie geïmplementeerd in Pivotal")
 
