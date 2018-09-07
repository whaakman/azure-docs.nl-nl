Het is belangrijk om uw virtuele machine (VM) te beveiligen voor de toepassingen die u uitvoert. Beveiligen van uw virtuele machines, kunt u een of meer Azure-services en functies die betrekking hebben op beveiligde toegang tot uw VM's en beveiligde opslag van uw gegevens opnemen. In dit artikel bevat informatie waarmee u uw virtuele machine en toepassingen om veilig te houden.

## <a name="antimalware"></a>Antimalware

De moderne bedreigingen voor cloudomgevingen is dynamisch, vergroten de druk voor het onderhouden van effectieve bescherming om te voldoen aan de naleving en beveiligingsvereisten. [Microsoft Antimalware voor Azure](../articles/security/azure-security-antimalware.md) is een gratis realtime-beveiliging-functie waarmee u kunt herkennen en verwijderen van virussen, spyware en andere schadelijke software. Waarschuwingen kunnen worden geconfigureerd om te waarschuwen wanneer bekende schadelijke of ongewenste software probeert zichzelf te installeren of uitvoeren op uw virtuele machine.

## <a name="azure-security-center"></a>Azure Security Center

[Azure Security Center](../articles/security-center/security-center-intro.md) helpt voorkomen, detecteren en reageren op bedreigingen voor uw virtuele machines. Security Center biedt geïntegreerde beveiligingsbewaking en beleidsbeheer voor uw Azure-abonnementen, helpt bedreigingen die anders onopgemerkt, en werkt met een uitgebreid ecosysteem van beveiligingsoplossingen te detecteren.

## <a name="encryption"></a>Versleuteling

Voor uitgebreide [Windows VM](../articles/virtual-machines/windows/encrypt-disks.md) en [Linux-VM](../articles/virtual-machines/linux/encrypt-disks.md) beveiliging en naleving, virtuele schijven in Azure kunnen worden versleuteld. Virtuele schijven op Windows-VM's worden in rust versleuteld met Bitlocker. Virtuele schijven op virtuele Linux-machines worden in rust versleuteld met behulp van dm-crypt. 

Er zijn geen kosten voor het versleutelen van virtuele schijven in Azure. Cryptografische sleutels worden opgeslagen in Azure Key Vault met behulp van software-beveiliging, of u kunt importeren of genereer uw sleutels in Hardware Security Modules (HSM's) gecertificeerd voor FIPS 140-2 level 2 standaarden. Deze cryptografische sleutels worden gebruikt voor het versleutelen en ontsleutelen van virtuele schijven die zijn gekoppeld aan uw virtuele machine. U behoudt de controle van deze cryptografische sleutels en het gebruik ervan kunt controleren. Een Azure Active Directory service-principal biedt een veilige mechanisme voor het uitgeven van deze cryptografische sleutels als VM's worden aangestuurd in of uit.

## <a name="key-vault-and-ssh-keys"></a>Key Vault en SSH-sleutels

Geheimen en certificaten kunnen worden gemodelleerd als resources en geleverd door [Key Vault](../articles/key-vault/key-vault-whatis.md). U kunt Azure PowerShell gebruiken om te maken van sleutelkluizen voor [Windows VMs](../articles/virtual-machines/windows/key-vault-setup.md) en de Azure CLI voor [virtuele Linux-machines](../articles/virtual-machines/linux/key-vault-setup.md). U kunt ook de versleutelingssleutels maken.

Toegangsbeleid van Key vault toekennen machtigingen voor sleutels, geheimen en certificaten afzonderlijk. U kunt bijvoorbeeld een gebruiker toegang geven tot sleutels, maar geen machtigingen voor geheimen geven. Machtigingen voor toegang tot sleutels, geheimen of certificaten bevinden zich echter op het niveau van de Key Vault. Met andere woorden, [key vault-toegangsbeleid](../articles/key-vault/key-vault-secure-your-key-vault.md) biedt geen ondersteuning voor machtigingen op objectniveau.

Als u verbinding met virtuele machines, moet u cryptografie met openbare sleutels voor een veiligere manier om te melden bij deze. Dit proces omvat een openbare en persoonlijke sleutel exchange met behulp van de opdracht SSH (secure shell) om te verifiëren zelf in plaats van een gebruikersnaam en wachtwoord. Wachtwoorden zijn vatbaar voor brute-force-aanvallen, met name op Internet gerichte VM's, zoals webservers. Met de combinatie van een secure shell (SSH), kunt u een [Linux-VM](../articles/virtual-machines/linux/mac-create-ssh-keys.md) die gebruikmaakt van SSH-sleutels voor verificatie, hoeft u het wachtwoord aan te melden. U kunt ook SSH-sleutels gebruiken verbinding maken vanaf een [Windows VM](../articles/virtual-machines/linux/ssh-from-windows.md) met een Linux VM.

## <a name="managed-identities-for-azure-resources"></a>Beheerde identiteiten voor Azure-resources

Een veelvoorkomende uitdaging bij het bouwen van cloud-apps is het beheren van de referenties in uw code voor verificatie bij cloudservices. Het is belangrijk dat de referenties veilig worden bewaard. In het ideale geval worden de referenties nooit weergegeven op werkstations van ontwikkelaars en niet ingecheckt in broncodebeheer. Azure Key Vault biedt een manier voor het veilig opslaan van referenties, geheimen en andere sleutels, maar uw code moet worden geverifieerd bij Key Vault om ze op te halen. 

De beheerde identiteit voor de functie Azure-resources in Azure Active Directory (Azure AD) lost dit probleem. De functie biedt Azure-services met een automatisch beheerde identiteit in Azure AD. U kunt de identiteit gebruiken voor verificatie bij alle services die ondersteuning bieden voor Azure AD-verificatie, inclusief Key Vault, zonder referenties in de code.  De code die wordt uitgevoerd op een virtuele machine kunt u een token aanvragen bij twee eindpunten die alleen toegankelijk vanuit de virtuele machine zijn. Voor meer informatie over deze service gedetailleerde, Controleer de [beheerde identiteiten voor een Azure-resources](../articles/active-directory/managed-identities-azure-resources/overview.md) overzichtspagina.   

## <a name="policies"></a>Beleidsregels

[Beleid voor Azure](../articles/azure-policy/azure-policy-introduction.md) kan worden gebruikt voor het definiëren van het gewenste gedrag voor uw organisatie [Windows VMs](../articles/virtual-machines/windows/policy.md) en [virtuele Linux-machines](../articles/virtual-machines/linux/policy.md). Met behulp van beleid, kan het afdwingen van een organisatie verschillende conventies en regels in de hele onderneming. Afdwinging van het gewenste gedrag kunt risico's te beperken tijdens een bijdrage levert aan het succes van de organisatie.

## <a name="role-based-access-control"></a>Op rollen gebaseerd toegangsbeheer

Met behulp van [op rollen gebaseerd toegangsbeheer (RBAC)](../articles/role-based-access-control/overview.md), kunt u taken scheiden binnen uw team en alleen de mate van toegang verlenen aan gebruikers op de virtuele machine die ze nodig hebben om hun werk te kunnen. In plaats van zodat iedereen onbeperkte machtigingen op de virtuele machine, kunt u alleen bepaalde acties toestaan. U kunt Toegangsbeheer configureren voor de virtuele machine in de [Azure-portal](../articles/role-based-access-control/role-assignments-portal.md), met de [Azure CLI](https://docs.microsoft.com/cli/azure/role), of[Azure PowerShell](../articles/role-based-access-control/role-assignments-powershell.md).


## <a name="next-steps"></a>Volgende stappen
- Doorloop de stappen voor het bewaken van de beveiliging van virtuele machines met behulp van Azure Security Center voor [Linux](../articles/virtual-machines/linux/tutorial-azure-security.md) of [Windows](../articles/virtual-machines/windows/tutorial-azure-security.md).