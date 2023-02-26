![](../Attachments/Pasted%20image%2020230226151148.png)

## DEEL 1: maak de netwerkopstelling en initialiseer de toestellen

## DEEL 2: configureer alle toestellen en controleer de verbindingen

Stap 1: configureer de IPv6 adressen op alle PC’s

![](../Attachments/Pasted%20image%2020230226152212.png)

### Stap 2: configureer de switchen
a. Maak DNS lookup ongedaan.
```
Switch(config)#no ip domain lookup
```
b. Configureer een hostname.
```
Switch(config)#hostname S1
```
c. Wijs volgende domeinnaam toe: ccna-lab.com
```
S1(config)#ip domain name ccna-lab.com
```
d. Encrypteer de plain-text paswoorden.
```
S1(config)#service password-encryption
```
e. Maak een MOTD-banner die de gebruikers waarschuwt: “Toegang voor onbevoegden is verboden”.
```
S1(config)#banner motd "Toegang voor onbevoegden is verboden"
```
f. Maak een lokale user database met een gebruikersnaam admin en paswoord classadm.
```
S1(config)#user admin password classadm
```
g. Configureer class als het privileged EXEC geëncrypteerd paswoord.
```
S1(config)#enable secret class
```
h. Configureer cisco als het console paswoord en maak login mogelijk.
```
S1(config)#line console 0
S1(config-line)#password cisco
S1(config-line)#login
```
i. Maak login op de VTY-lijnen mogelijk door gebruik te maken van de lokale database.
```
S1(config-line)#line vty 0 15
S1(config-line)#password cisco
S1(config-line)#login local
```
j. Genereer een crypto rsa key voor ssh, gebruik makend van een modulus grootte van 1024 bits.
```
S1(config)#crypto key generate rsa

The name for the keys will be: S1.ccna-lab.com

Choose the size of the key modulus in the range of 360 to 2048 for your

General Purpose Keys. Choosing a key modulus greater than 512 may take

a few minutes.

  

How many bits in the modulus [512]: 1024

% Generating 1024 bit RSA keys, keys will be non-exportable...[OK]
```
k. Verander de transport input op alle VTY-lijnen naar alleen SSH en Telnet.
```
S1(config)#line vty 0 15
S1(config-line)#transport input ssh
S1(config-line)#transport input telnet
```
l. Wijs een IPv6 adres toe aan VLAN 1 overeenkomstig de adrestabel.
Voeg hier tussen de runningconfiguration file van S1.