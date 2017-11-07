## <a name="prepare-your-intel-nuc"></a>Uw Intel NUC voorbereiden

U voltooit de installatie van de hardware, moet u:

- Verbinding maken met uw NUC Intel de voeding die is opgenomen in het pakket.
- Verbinding maken met uw NUC Intel via een Ethernet-kabel met het netwerk.

U hebt nu de hardware-instellingen van uw gatewayapparaat Intel NUC voltooid.

### <a name="sign-in-and-access-the-terminal"></a>Aanmelden en toegang tot de terminal

U hebt twee opties voor toegang tot een terminal omgeving op uw NUC Intel:

- Als u een toetsenbord en de monitor die zijn verbonden met uw NUC Intel hebt, kunt u de shell rechtstreeks openen. De standaardreferenties zijn gebruikersnaam **hoofdmap** en het wachtwoord **hoofdmap**.

- Toegang tot de shell op uw Intel NUC gebruik van SSH op uw computer.

#### <a name="sign-in-with-ssh"></a>Meld u aan met SSH

Als u wilt aanmelden met SSH, moet u het IP-adres van uw NUC Intel. Als u een toetsenbord en de monitor die zijn verbonden met uw NUC Intel hebt, gebruikt de `ifconfig` opdracht de IP-adres vinden. U kunt ook verbinding maken met uw router voor het weergeven van de adressen van apparaten in uw netwerk.

Aanmelden met gebruikersnaam **hoofdmap** en het wachtwoord **hoofdmap**.

#### <a name="optional-share-a-folder-on-your-intel-nuc"></a>Optioneel: Een map op uw NUC Intel delen

U kunt desgewenst een map op uw NUC Intel delen met uw bureaublad omgeving. Delen van een map, kunt u gebruikmaken van uw voorkeur bureaublad teksteditor (zoals [Visual Studio Code](https://code.visualstudio.com/) of [Sublime Text](http://www.sublimetext.com/)) voor het bewerken van bestanden op uw NUC Intel in plaats van `nano` of `vi`.

Als u wilt delen een map met Windows, een Samba-server in de Intel NUC te configureren. De SFTP-server ook gebruiken op de Intel NUC met een SFTP-client op uw computer.
