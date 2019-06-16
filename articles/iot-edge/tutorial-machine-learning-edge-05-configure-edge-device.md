---
title: Configureren van IoT Edge-apparaat - Machine Learning in Azure IoT Edge | Microsoft Docs
description: Een Azure-virtuele Machine waarop Linux wordt uitgevoerd als een Azure IoT Edge-apparaat die als een transparante gateway fungeert configureren.
author: kgremban
manager: philmea
ms.author: kgremban
ms.date: 06/13/2019
ms.topic: tutorial
ms.service: iot-edge
services: iot-edge
ms.openlocfilehash: 86efbaa053e087885f0dbfea24000781d41ac5d5
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "67057713"
---
# <a name="tutorial-configure-an-iot-edge-device"></a>Zelfstudie: Een IoT Edge-apparaat configureren

> [!NOTE]
> In dit artikel maakt deel uit van een reeks voor een zelfstudie over het gebruik van Azure Machine Learning op IoT Edge. Als u rechtstreeks in dit artikel zijn ontvangen, u wordt aangeraden te beginnen met de [eerste artikel](tutorial-machine-learning-edge-01-intro.md) in de reeks voor de beste resultaten.

In dit artikel configureren we een Azure-Machine waarop Linux om te worden van een Azure IoT Edge-apparaat die als een transparante gateway fungeert wordt uitgevoerd. De transparante gateway-configuratie kunt u apparaten verbinding maken met Azure IoT Hub via de gateway zonder te weten dat de gateway bestaat. Een gebruiker interactie met de apparaten in IoT Hub is op hetzelfde moment, niet op de hoogte van de tussenliggende gateway-apparaat. Uiteindelijk, gebruiken we de transparante gateway randanalyses toevoegen aan ons systeem door het IoT Edge-modules toe te voegen aan de gateway.

De stappen in dit artikel worden doorgaans uitgevoerd door een ontwikkelaar van de cloud.

## <a name="generate-certificates"></a>Certificaten genereren

Voor een apparaat kan werken als een gateway moet kunnen veilig verbinding maken met downstream-apparaten. Azure IoT Edge kunt u een openbare-sleutelinfrastructuur (PKI) gebruiken voor het instellen van beveiligde verbindingen tussen apparaten. In dit geval toestemming we een downstream apparaat verbinding maakt met een IoT Edge-apparaat als een transparante gateway fungeert. Redelijke om beveiliging te handhaven, moet het downstream apparaat de identiteit van de IoT Edge-apparaat bevestigen. Zie voor meer informatie over hoe de certificaten voor het gebruik van IoT Edge-apparaten [informatie over het gebruik Azure IoT Edge certificaat](iot-edge-certs.md).

In deze sectie maakt u de zelfondertekende certificaten met behulp van een Docker-installatiekopie die we vervolgens ontwikkelen en uitvoeren. We hebben gekozen voor een Docker-installatiekopie gebruiken voor het voltooien van deze stap, zoals het aanzienlijk verminderd, het aantal stappen die nodig zijn voor het maken van de certificaten op de Windows-ontwikkelcomputer. Zie [genereren van certificaten met Windows](how-to-create-transparent-gateway.md#generate-certificates-with-windows) voor informatie over het maken van de certificaten zonder gebruik van een container. [Genereren van certificaten met Linux](how-to-create-transparent-gateway.md#generate-certificates-with-linux) heeft de set met instructies die we geautomatiseerd met de Docker-installatiekopie.

1. Aanmelden bij een virtuele ontwikkelmachine.

2. Open een opdrachtprompt en voer de volgende opdracht voor het maken van een map op de virtuele machine.

    ```cmd
    mkdir C:\edgecertificates
    ```

3. Start **Docker voor Windows** vanuit het menu Start van Windows.

4. Open Visual Studio Code.

5. Selecteer **bestand** > **map openen...**  en kies **C:\\bron\\IoTEdgeAndMlSample\\CreateCertificates**.

6. Met de rechtermuisknop op de docker-bestand en kies **Build Image**.

7. In het dialoogvenster, accepteer de standaardwaarde voor de installatiekopie met de naam en het label: **createcertificates:latest**.

8. Wacht tot de build te voltooien.

    > [!NOTE]
    > Mogelijk ziet u een waarschuwing voor een ontbrekende openbare sleutel. Is het veilig om deze waarschuwing negeren. Evenzo, ziet u een beveiligingswaarschuwing weergegeven waarin u wordt aangeraden selectievakje/opnieuw instellen van machtigingen voor de installatiekopie, die veilig worden genegeerd voor deze installatiekopie.

9. Voer de container createcertificates in het terminalvenster van Visual Studio Code.

    ```cmd
    docker run --name createcertificates --rm -v
    C:\edgeCertificates:/edgeCertificates createcertificates
    /edgeCertificates
    ```

10. Docker wordt u gevraagd de toegang tot de **C:\\**  station. Selecteer **delen**.

11. Geef uw referenties in wanneer hierom wordt gevraagd.

12. Zodra de container is voltooid wordt uitgevoerd, controleren op de volgende bestanden in **c:\\edgecertificates**:

    * C:\\edgeCertificates\\certs\\azure-iot-test-only.root.ca.cert.pem
    * C:\\edgeCertificates\\certs\\new-edge-device-full-chain.cert.pem
    * C:\\edgeCertificates\\certificaten\\nieuwe-edge-device.cert.pem
    * C:\\edgeCertificates\\certs\\new-edge-device.cert.pfx
    * C:\\edgeCertificates\\persoonlijke\\nieuwe-edge-device.key.pem

## <a name="upload-certificates-to-azure-key-vault"></a>Certificaten uploaden naar Azure Key Vault

Voor het opslaan van onze certificaten veilig en waardoor deze toegankelijk is vanaf meerdere apparaten, zullen we de certificaten uploaden in Azure Key Vault. Zoals u in de bovenstaande lijst zien kunt, hebben we twee typen certificaatbestanden: PFX en het PEM. Wij zullen de PFX behandelen als Key Vault-certificaten worden geüpload naar de Key Vault. Het PEM-bestanden zijn tekst zonder opmaak en we zullen ze behandelen als Key Vault-geheimen. We gebruiken de Sleutelkluis die is gekoppeld aan de werkruimte van de Azure Machine Learning-service wordt gemaakt door het uitvoeren van de [Azure notitieblokken](tutorial-machine-learning-edge-04-train-model.md#run-azure-notebooks).

1. Uit de [Azure-portal](https://portal.azure.com), gaat u naar de werkruimte van uw Azure Machine Learning-service.

2. Zoek op de overzichtspagina van de werkruimte van de Azure Machine Learning-service, de naam van de **Key Vault**.

    ![Key vault-naam kopiëren](media/tutorial-machine-learning-edge-05-configure-edge-device/find-key-vault-name.png)

3. Op uw ontwikkelcomputer, de certificaten te uploaden naar Key Vault. Vervang **\<subscriptionId\>** en **\<keyvaultname\>** met de gegevens van resources.

    ```powershell
    C:\source\IoTEdgeAndMlSample\CreateCertificates\upload-keyvaultcerts.ps1
    -SubscriptionId <subscriptionId> -KeyVaultName <keyvaultname>
    ```

4. Als u hierom wordt gevraagd, moet u zich aanmelden bij Azure.

5. Het script wordt uitgevoerd voor een paar minuten met uitvoer met een lijst met de nieuwe vermeldingen van de Key Vault.

    ![Key Vault-scriptuitvoer](media/tutorial-machine-learning-edge-05-configure-edge-device/key-vault-entries-output.png)

## <a name="create-iot-edge-device"></a>Een IoT Edge-apparaat maken

Voor een Azure IoT Edge-apparaat verbinding met een IoT-hub, maken we een identiteit voor het apparaat eerst in de hub. We nemen de connection string vanuit de apparaat-id in de cloud en deze gebruiken voor het configureren van de runtime op onze IoT Edge-apparaat. Zodra het apparaat is geconfigureerd en verbinding met de hub maakt, zijn we kan modules implementeren en berichten te verzenden. We kunnen ook de configuratie van de fysieke IoT Edge-apparaat wijzigen door het wijzigen van de configuratie van de bijbehorende apparaat-id van IoT-hub.

Voor deze zelfstudie maken we het nieuwe apparaat-id met behulp van Visual Studio Code. U kunt ook deze stappen met behulp van de [Azure-portal](how-to-register-device-portal.md), of [Azure CLI](how-to-register-device-cli.md).

1. Open Visual Studio Code op uw ontwikkelcomputer.

2. Open de **Azure IoT Hub-apparaten** frame van de weergave van de Visual Studio Code-Verkenner.

3. Klik op het beletselteken en selecteer **IoT Edge-apparaat maken**.

4. Het apparaat een naam geven. Voor uw gemak gebruiken we **aaTurbofanEdgeDevice** , zodat wordt gesorteerd op alle clientapparaten die we eerder hebben gemaakt via het apparaat gebruikmaken van de testgegevens te verzenden.

5. Het nieuwe apparaat wordt weergegeven in de lijst met apparaten.

    ![Nieuwe aaTurbofanEdgeDevice in VS Code explorer weergeven](media/tutorial-machine-learning-edge-05-configure-edge-device/iot-hub-devices-list.png)

## <a name="deploy-azure-virtual-machine"></a>Azure-Machine implementeren

We gebruiken de [Azure IoT Edge op Ubuntu](https://azuremarketplace.microsoft.com/marketplace/apps/microsoft_iot_edge.iot_edge_vm_ubuntu?tab=Overview) installatiekopie van de Azure Marketplace om onze IoT Edge-apparaat voor deze zelfstudie. De Azure IoT Edge op Ubuntu-installatiekopie installeert de meest recente Azure IoT Edge-runtime en de bijbehorende afhankelijkheden bij het opstarten. We implementeren de virtuele machine met een PowerShell-script `Create-EdgeVM.ps1`; een Resource Manager-sjabloon `IoTEdgeVMTemplate.json`; en een shell-script `install packages.sh`.

### <a name="enable-programmatic-deployment"></a>Programmatische implementatie inschakelen

Voor het gebruik van de installatiekopie vanuit de marketplace in een implementatie met scripts, moeten we programmatische implementatie voor de installatiekopie inschakelen.

1. Meld u aan bij Azure Portal.

1. Selecteer **Alle services**.

1. Voer in de zoekbalk typt, en selecteer **Marketplace**.

1. Voer in de zoekbalk typt, en selecteer **Azure IoT Edge op Ubuntu**.

1. Selecteer de **wilt implementeren via een programma? Aan de slag** hyperlink.

1. Selecteer de **inschakelen** knop, klikt u vervolgens **opslaan**.

    ![Programmatische implementatie voor virtuele machine inschakelen](media/tutorial-machine-learning-edge-05-configure-edge-device/deploy-ubuntu-vm.png)

1. U ziet een melding geslaagd.

### <a name="create-virtual-machine"></a>Virtuele machine maken

Voer vervolgens het script voor het maken van de virtuele machine voor uw IoT Edge-apparaat.

1. Open een PowerShell-venster en navigeer naar de **EdgeVM** directory.

    ```powershell
    cd C:\source\IoTEdgeAndMlSample\EdgeVM”
    ```

2. Voer het script voor het maken van de virtuele machine.

    ```powershell
    .\Create-EdgeVm.ps1
    ```

3. Wanneer u hierom wordt gevraagd, moet u waarden opgeven voor elke parameter. Voor het abonnement, resourcegroep en locatie, raden we u gebruikt hetzelfde als u voor alle resources in deze zelfstudie.

    * **Azure-abonnements-ID**: gevonden in de Azure-portal
    * **Naam resourcegroep**: gemakkelijk te onthouden naam voor het groeperen van de resources voor deze zelfstudie
    * **Locatie**: Azure-locatie waar de virtuele machine wordt gemaakt. Bijvoorbeeld, westus2 of northeurope. Zie voor meer informatie, alle [Azure-locaties](https://azure.microsoft.com/global-infrastructure/locations/).
    * **AdminUsername**: de naam voor het beheerdersaccount dat u wilt gebruiken voor het aanmelden bij de virtuele machine
    * **AdminPassword**: het wachtwoord instellen voor de AdminUsername op de virtuele machine

4. Om het script te kunnen zijn voor het instellen van de virtuele machine, moet u zich aanmeldt bij Azure met de referenties die zijn gekoppeld aan het Azure-abonnement dat u gebruikt.

5. Het script wordt bevestigd dat de gegevens voor het maken van uw virtuele machine. Selecteer **y** of **Enter** om door te gaan.

6. Het script wordt uitgevoerd voor enkele minuten als deze wordt uitgevoerd de volgende stappen uit:

    * De resourcegroep te maken als deze niet al bestaat
    * De virtuele machine maken
    * Toevoegen van NSG-uitzonderingen voor de virtuele machine voor 22 (SSH), 5671 (AMQP), 5672 (AMPQ poort) en 443 (SSL)
    * Installeer de [Azure CLI](https://docs.microsoft.com/cli/azure/install-azure-cli-apt?view=azure-cli-latest))

7. Het script voert de SSH-verbindingsreeks voor de verbinding met de virtuele machine. Kopieer de verbindingsreeks voor de volgende stap.

    ![SSH-verbindingsreeks voor de virtuele machine kopiëren](media/tutorial-machine-learning-edge-05-configure-edge-device/vm-ssh-connection-string.png)

## <a name="connect-to-your-iot-edge-device"></a>Verbinding maken met uw IoT Edge-apparaat

De volgende secties voor verschillende configureren virtuele machine van Azure die hebben we gemaakt. De eerste stap is het verbinding maken met de virtuele machine.

1. Open een opdrachtprompt en plak de SSH-verbindingsreeks die u hebt gekopieerd uit de uitvoer van het script. Voer uw eigen gegevens voor de gebruikersnaam, het achtervoegsel en regio op basis van de waarden die u hebt opgegeven naar de PowerShell-script in de vorige sectie.

    ```cmd
    ssh -l <username> iotedge-<suffix>.<region>.cloudapp.azure.com
    ```

2. Wanneer u hierom wordt gevraagd om te valideren de echtheid van de host, typt u **Ja** en selecteer **Enter**.

3. Geef desgevraagd uw wachtwoord.

4. Ubuntu een welkomstbericht wordt weergegeven en vervolgens ziet u een prompt achtige `<username>@<machinename>:~$`.

## <a name="download-key-vault-certificates"></a>Key Vault-certificaten downloaden

Eerder in dit artikel, we certificaten geüpload naar Key Vault, zodat ze beschikbaar zijn voor onze IoT Edge-apparaat en onze leaf-apparaten die een downstream-apparaat dat gebruikmaakt van de IoT Edge-apparaat als een gateway om te communiceren met IoT Hub. We zijn de leaf-apparaten wordt getroffen later in de zelfstudie. In deze sectie, downloadt u de certificaten naar het IoT Edge-apparaat.

1. Van de SSH-sessie op de Linux-machine, moet u zich aanmelden bij Azure met de Azure CLI.

    ```bash
    az login
    ```

1. U wordt gevraagd om te openen in een browser <https://microsoft.com/devicelogin> en geef een unieke code. U kunt deze stappen uitvoeren op uw lokale computer. Sluit het browservenster wanneer u klaar bent verificatie.

1. Wanneer u verifiëren, wordt de Linux-VM zich aanmelden en uw Azure-abonnementen weergeven.

1. De Azure-abonnement dat u wilt gebruiken voor Azure CLI-opdrachten uit.

    ```bash
    az account set --subscription <subscription id>
    ```

1. Maak een map op de virtuele machine voor de certificaten.

    ```bash
    sudo mkdir /edgeMlCertificates
    ```

1. Downloaden van de certificaten die u hebt opgeslagen in de key vault: nieuwe-edge-apparaat-full-chain.cert.pem, nieuw-edge-device.key.pem en azure-iot-test-only.root.ca.cert.pem

    ```bash
    key_vault_name="<key vault name>"
    sudo az keyvault secret download --vault-name $key_vault_name --name new-edge-device-full-chain-cert-pem -f /edgeMlCertificates/new-edge-device-full-chain.cert.pem
    sudo az keyvault secret download --vault-name $key_vault_name --name new-edge-device-key-pem -f /edgeMlCertificates/new-edge-device.key.pem
    sudo az keyvault secret download --vault-name $key_vault_name --name azure-iot-test-only-root-ca-cert-pem -f /edgeMlCertificates/azure-iot-test-only.root.ca.cert.pem
    ```

## <a name="update-the-iot-edge-device-configuration"></a>De configuratie van de IoT Edge-apparaat bijwerken

IoT Edge-runtime maakt gebruik van het bestand /etc/iotedge/config.yaml om vast te leggen van de configuratie ervan. Moeten we drie soorten informatie in dit bestand bijwerken:

* **Apparaatverbindingsreeks**: de verbindingsreeks van de identiteit van dit apparaat in IoT Hub
* **Certificaten:** de certificaten te gebruiken voor verbindingen met downstream-apparaten
* **Hostnaam:** de volledig gekwalificeerde domeinnaam (FQDN) van de virtuele machine IoT Edge-apparaat.

De *Azure IoT Edge op Ubuntu* installatiekopie die wordt gebruikt voor het maken van de IoT Edge-VM wordt geleverd met een shell-script waarmee de config.yaml wordt bijgewerkt met de verbindingsreeks.

1. In Visual Studio Code met de rechtermuisknop op het IoT Edge-apparaat, en selecteer vervolgens **kopie Apparaatverbindingsreeks**.

    ![Kopieer de verbindingsreeks vanuit Visual Studio Code](media/tutorial-machine-learning-edge-05-configure-edge-device/copy-device-connection-string-command.png)

2. Voer de opdracht voor het bijwerken van het bestand config.yaml met de verbindingsreeks van uw apparaat in de SSH-sessie.

    ```bash
    sudo /etc/iotedge/configedge.sh "<your_iothub_edge_device_connection_string>"
    ```

Vervolgens wordt bijgewerkt de certificaten en de hostnaam door de config.yaml rechtstreeks te bewerken.

1. Open het bestand config.yaml.

    ```bash
    sudo nano /etc/iotedge/config.yaml
    ```

2. De sectie certificaten van de config.yaml bijwerken door het verwijderen van de voorloopspaties `#` en het instellen van het pad, zodat het bestand ziet als in het volgende voorbeeld eruit:

    ```yaml
    certificates:
      device_ca_cert: "/edgeMlCertificates/new-edge-device-full-chain.cert.pem"
      device_ca_pk: "/edgeMlCertificates/new-edge-device.key.pem"
      trusted_ca_certs: "/edgeMlCertificates/azure-iot-test-only.root.ca.cert.pem"
    ```

    Zorg ervoor dat de ' certificaten: ' heeft geen witruimte en dat elk van de certificaten wordt voorafgegaan door twee spaties vóór.

    Met de rechtermuisknop op in nano plakt u de inhoud van het Klembord op de huidige cursorpositie. Gebruik de pijltoetsen om te navigeren naar de tekenreeks die u wilt vervangen, verwijdert u de tekenreeks ter vervanging van de tekenreeks, en klik vervolgens met de rechtermuisknop op om te plakken in de buffer.

3. Navigeer naar uw virtuele machine in de Azure-portal. Kopieer de DNS-naam (Fully Qualified Domain Name van de machine) van de **overzicht** sectie.

4. Plak de FQDN-naam in de sectie van de hostnaam van de config.yml. Zorg ervoor dat de naam alleen kleine letters bevatten.

    ```yaml
    hostname: '<machinename>.<region>.cloudapp.azure.com'
    ```

5. Opslaan en sluiten van het bestand (`Ctrl + X`, `Y`, `Enter`).

6. Start de iotedge-daemon opnieuw.

    ```bash
    sudo systemctl restart iotedge
    ```

7. Controleer de status van de IoT Edge-Daemon (na de opdracht, typt u ': q ' om af te sluiten).

    ```bash
    systemctl status iotedge
    ```

8. Als u fouten ziet (voorafgegaan door tekst gekleurd "\[fout\]') in de status controleren daemon-logboeken voor uitgebreide foutgegevens.

    ```bash
    journalctl -u iotedge --no-pager --no-full
    ```

## <a name="next-steps"></a>Volgende stappen

We zojuist een Azure-VM als Azure IoT Edge transparante Gateway configureren. We zijn begonnen door het genereren van testcertificaten voor we geüpload naar Azure Key Vault. Vervolgens wordt een script en Resource Manager-sjabloon gebruikt voor het implementeren van de virtuele machine met 'Ubuntu Server 16.04 LTS + Azure IoT Edge-runtime'-installatiekopie van de Azure marketplace. Het script heeft de extra stap van de Azure CLI-installatie ([Azure CLI installeren met apt](https://docs.microsoft.com/cli/azure/install-azure-cli-apt)). Met de virtuele machine omhoog en uitgevoerd we die zijn verbonden via SSH, aangemeld bij Azure, gedownload van certificaten van Key Vault en verschillende updates aangebracht in de configuratie van de IoT Edge-Runtime door het bestand config.yaml bij te werken. Zie voor meer informatie over het gebruik van IoT Edge als een gateway [hoe een IoT Edge-apparaat kan worden gebruikt als een gateway](iot-edge-as-gateway.md). Zie voor meer informatie over het configureren van een IoT Edge-apparaat als een transparante gateway [configureren van een IoT Edge-apparaat om te fungeren als een transparante gateway](how-to-create-transparent-gateway.md).

Doorgaan met het volgende artikel voor het bouwen van IoT Edge modules.

> [!div class="nextstepaction"]
> [Maken en implementeren van aangepaste IoT Edge-modules](tutorial-machine-learning-edge-06-custom-modules.md)
