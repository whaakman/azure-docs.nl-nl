Het is belangrijk om uw virtuele machine (VM) te beveiligen voor de toepassingen die u uitvoert. Beveiligen van uw virtuele machines, kan een of meer Azure-services en functies die betrekking hebben op beveiligde toegang tot uw virtuele machines en veilige opslag van uw gegevens bevatten. Dit artikel bevat informatie waarmee u uw virtuele machine en toepassingen om veilig te houden.

## <a name="antimalware"></a>Antimalware

De liggende moderne bedreiging voor cloudomgevingen is dynamisch, uitbreiding van de druk voor het onderhouden van effectieve bescherming om te voldoen aan het nalevings- en beveiligingsvereisten. [Microsoft Antimalware voor Azure](../articles/security/azure-security-antimalware.md) is een functie gratis real-timebeveiliging die helpt bepalen en virussen, spyware en andere schadelijke software verwijderen. Waarschuwingen kunnen worden geconfigureerd om te waarschuwen wanneer bekende schadelijke of ongewenste software probeert zichzelf te installeren of uitvoeren op de virtuele machine.

## <a name="azure-security-center"></a>Azure Security Center

[Azure Security Center](../articles/security-center/security-center-intro.md) helpt u detecteren, voorkomen van en reageren op bedreigingen voor uw virtuele machines. Security Center biedt geïntegreerde beveiligingsbewaking en beleidsbeheer voor uw Azure-abonnementen, helpt bedreigingen die anders onopgemerkt, en werkt met een uitgebreid ecosysteem van beveiligingsoplossingen te detecteren.

## <a name="encryption"></a>Versleuteling

Voor uitgebreide [Windows VM](../articles/virtual-machines/windows/encrypt-disks.md) en [Linux VM](../articles/virtual-machines/linux/encrypt-disks.md) beveiliging en naleving, virtuele schijven in Azure kunnen worden versleuteld. Virtuele schijven op Windows-VM's zijn versleuteld in rust met Bitlocker. Virtuele schijven op virtuele Linux-machines zijn versleuteld in rust dm-crypt gebruiken. 

Er zijn geen kosten voor het versleutelen van virtuele schijven in Azure. Cryptografische sleutels worden opgeslagen in Azure Key Vault met software-beveiliging, of u kunt importeren of genereren van uw sleutels in Hardware Security Modules (HSM's) die is gecertificeerd voor FIPS 140-2 level 2-standaarden. Deze cryptografische sleutels worden gebruikt voor het versleutelen en ontsleutelen van virtuele schijven gekoppeld aan uw virtuele machine. U beheer behouden over deze cryptografische sleutels en het gebruik ervan kunt controleren. Een Azure Active Directory service-principal biedt een veilige mechanisme voor het uitgeven van deze cryptografische sleutels als virtuele machines van stroom in- of uitschakelen voorzien worden.

## <a name="key-vault-and-ssh-keys"></a>Sleutelkluis en SSH-sleutels

Geheimen en certificaten kunnen worden gemodelleerd als bronnen en geleverd door [Sleutelkluis](../articles/key-vault/key-vault-whatis.md). U kunt Azure PowerShell gebruiken voor het maken van sleutelkluizen voor [VM's van Windows](../articles/virtual-machines/windows/key-vault-setup.md) en Azure CLI voor [virtuele Linux-machines](../articles/virtual-machines/linux/key-vault-setup.md). U kunt ook de versleutelingssleutels maken.

Het toegangsbeleid voor sleutelkluizen verlenen machtigingen voor sleutels, geheimen en certificaten afzonderlijk. U kunt bijvoorbeeld een gebruiker toegang geven tot sleutels, maar geen machtigingen voor geheimen geven. Machtigingen voor toegang tot sleutels, geheimen of certificaten bevinden zich echter op het niveau van de Key Vault. Met andere woorden, [sleutel kluis toegangsbeleid](../articles/key-vault/key-vault-secure-your-key-vault.md) biedt geen ondersteuning voor machtigingen op het object niveau.

Als u verbinding met virtuele machines, moet u cryptografie met openbare sleutels gebruiken voor een veiligere manier aan hen te melden bij. Dit proces omvat een openbare en persoonlijke sleuteluitwisseling zelf in plaats van een gebruikersnaam en het wachtwoord verifiëren met de shellopdracht secure (SSH). Wachtwoorden zijn kwetsbaar voor aanvallen, vooral op Internet gerichte VMs zoals webservers met brute kracht. Met de combinatie van een secure shell (SSH), kunt u een [Linux VM](../articles/virtual-machines/linux/mac-create-ssh-keys.md) die gebruikmaakt van SSH-sleutels voor verificatie, hoeft u de wachtwoorden aan te melden. U kunt ook de SSH-sleutels gebruiken om verbinding te een [Windows VM](../articles/virtual-machines/linux/ssh-from-windows.md) voor een Linux-VM.

## <a name="policies"></a>Beleidsregels

[Azure beleid](../articles/azure-policy/azure-policy-introduction.md) kan worden gebruikt voor het definiëren van het gewenste gedrag voor uw organisatie [VM's van Windows](../articles/virtual-machines/windows/policy.md) en [virtuele Linux-machines](../articles/virtual-machines/linux/policy.md). Een organisatie kan met behulp van beleid afdwingen verschillende conventies en regels in de hele onderneming. Afdwinging van het gewenste gedrag kunt risico's te beperken tijdens bijdragen aan het succes van de organisatie.

## <a name="role-based-access-control"></a>Op rollen gebaseerd toegangsbeheer

Met behulp van [op rollen gebaseerde toegangsbeheer (RBAC)](../articles/active-directory/role-based-access-control-what-is.md), kunt u taken scheiden binnen uw team en de hoeveelheid toegang verlenen aan gebruikers op de virtuele machine die ze nodig hebben voor het uitvoeren van hun taken. In plaats van iedereen geven onbeperkte machtigingen op de virtuele machine, kunt u alleen bepaalde acties. U kunt Toegangsbeheer configureren voor de virtuele machine in de [Azure-portal](../articles/active-directory/role-based-access-control-configure.md), waarbij de [Azure CLI](https://docs.microsoft.com/cli/azure/role), of[Azure PowerShell](../articles/active-directory/role-based-access-control-manage-access-powershell.md).


## <a name="next-steps"></a>Volgende stappen
- Doorloop de stappen voor het bewaken van de beveiliging van de virtuele machine met behulp van Azure Security Center voor [Linux](../articles/virtual-machines/linux/tutorial-azure-security.md) of [Windows](../articles/virtual-machines/windows/tutorial-azure-security.md).