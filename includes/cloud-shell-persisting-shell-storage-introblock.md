# <a name="persist-files-in-azure-cloud-shell"></a>Bestanden in Azure Cloud Shell behouden
Cloudshell gebruikmaakt van Azure File storage om bestanden in verschillende sessies persistent te maken.

## <a name="set-up-a-clouddrive-file-share"></a>Een bestandsshare clouddrive instellen
Op de eerste start vraagt Cloud Shell u om te koppelen van een nieuwe of bestaande bestandsshare om bestanden in verschillende sessies persistent te maken.

> [!NOTE]
> Maken Bash en PowerShell delen dezelfde bestandsshare. Slechts één bestandsshare kan worden gekoppeld aan het automatisch koppelen in Cloud Shell.

### <a name="create-new-storage"></a>Maken van nieuwe opslag

Wanneer u basisinstellingen en selecteert u alleen een abonnement, maakt Cloud Shell drie resources uit uw naam in de ondersteunde regio die het dichtst bij u:
* Resourcegroep: `cloud-shell-storage-<region>`
* Storage-account: `cs<uniqueGuid>`
* Bestandsshare: `cs-<user>-<domain>-com-<uniqueGuid>`

![De instelling voor Cloudabonnement](../articles/cloud-shell/media/persisting-shell-storage/basic-storage.png)

De bestandsshare koppelt als `clouddrive` in uw `$Home` directory. Dit is een eenmalige actie en de bestandsshare koppelt automatisch in de volgende sessies. 

> [!NOTE]
> Voor beveiliging, moet elke gebruiker hun eigen opslagaccount inrichten.  Voor op rollen gebaseerd toegangsbeheer (RBAC), moeten gebruikers Inzender-toegang hebben of boven op de opslag account niveau.

In Bash, bevat de bestandsshare ook een installatiekopie van 5 GB die automatisch voor u die gemaakt wordt zich blijft voordoen gegevens in uw `$Home` directory. 

### <a name="use-existing-resources"></a>Gebruik bestaande bronnen

U kunt bestaande resources koppelen met behulp van de geavanceerde optie. Wanneer de opslag-setup-prompt wordt weergegeven, selecteert u **geavanceerde instellingen weergeven** om extra opties weer te geven. De vervolgkeuzemenu's worden voor uw toegewezen Cloud Shell-regio en lokaal redundante opslag en geografisch redundante opslag-accounts gefilterd.

Bestandsshares ontvangen een 5 GB-installatiekopie gemaakt voor u om vast te leggen uw `$Home` directory.

![De instelling van de Resource](../articles/cloud-shell/media/persisting-shell-storage/advanced-storage.png)

### <a name="restrict-resource-creation-with-an-azure-resource-policy"></a>Beperken van het maken van resources aan een Azure-resource-beleid
Storage-accounts die u in de Cloud Shell maakt worden gemarkeerd met `ms-resource-usage:azure-cloud-shell`. Als u wilt weigeren van gebruikers van het storage-accounts maken in Cloud Shell, maakt u een [Azure resourcebeleid voor tags](../articles/azure-policy/json-samples.md) die worden geactiveerd door deze specifieke tag.

## <a name="supported-storage-regions"></a>Ondersteunde storage-regio 's
Gekoppelde Azure storage-accounts moeten bevinden zich in dezelfde regio als de Cloud Shell-machine die u hebt te koppelen.

U kunt uw toegewezen regio zoeken:
* De opmerking in het dialoogvenster 'Geavanceerde instellingen' weergeven
* Verwijzen naar de naam van het opslagaccount voor u gemaakt (bijvoorbeeld: `cloud-shell-storage-westus`)
* Voer `env` en zoekt u de variabele `ACC_LOCATION`

Cloud Shell machines bevinden zich in de volgende regio's:
|Onderwerp|Regio|
|---|---|
|Noord- en Zuid-Amerika|VS-Oost, VS Zuid-centraal, VS-West|
|Europa|Noord-Europa, West-Europa|
|Azië en Stille Oceaan|India-centraal, Zuidoost-Azië|

