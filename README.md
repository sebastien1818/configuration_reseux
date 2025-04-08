# configuration_reseux

guide AAA: 
Sur **Cisco Packet Tracer**, tu peux simuler un serveur AAA en utilisant un **serveur Cisco** avec les services **TACACS+** ou **RADIUS** activés. Voici un guide pas à pas pour **programmer un serveur AAA** dans Packet Tracer :

---

### **Étape 1 : Ajouter les éléments au réseau**
Ajoute dans ta scène :
- 1 routeur (ex : **Cisco 2911**)
- 1 serveur (type **Server**)
- 1 PC (pour tester l'accès)
- Des connexions (câbles **copper straight-through** entre les équipements et un switch si nécessaire)

---

### **Étape 2 : Configurer le serveur AAA**
1. Clique sur le **Serveur**, puis :
   - Onglet **Config** > **Settings** :
     - Donne une **IP statique** au serveur (ex : 192.168.1.2 /24)
     - **Default Gateway** : IP du routeur (ex : 192.168.1.1)

2. Va dans l’onglet **Services** > **AAA** :
   - Active **AAA Service**
   - Choisis **RADIUS** ou **TACACS+** (ex : RADIUS)
   - Clique sur **Add** pour créer un utilisateur :
     - Username : `admin`
     - Password : `cisco`
   - Clique sur **On** pour activer le service

---

### **Étape 3 : Configurer le routeur (client AAA)**

1. Clique sur le **Routeur**, puis va dans **CLI** :
2. Configure l’interface connectée au serveur :
```bash
enable
configure terminal
interface FastEthernet0/0
ip address 192.168.1.1 255.255.255.0
no shutdown
exit
```

3. Configure le AAA :
```bash
aaa new-model
radius-server host 192.168.1.2 key cisco
aaa authentication login default group radius local
```

4. Active l’authentification sur la ligne console ou vty :
```bash
line vty 0 4
login authentication default
exit
```

---

### **Étape 4 : Tester avec le PC**
1. Attribue une IP au PC dans le même réseau (ex : 192.168.1.3 /24, passerelle 192.168.1.1).
2. Ouvre un **Terminal** sur le PC et tente un accès Telnet ou SSH vers le routeur.
3. Le routeur demandera un **login**. Tu dois utiliser les identifiants définis sur le serveur AAA (`admin / cisco`).

---

Si tu veux, je peux te faire un schéma visuel ou t’envoyer un fichier `.pkt` de Packet Tracer avec tout ça préconfiguré. Tu préfères ?
