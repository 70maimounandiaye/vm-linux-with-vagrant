# Configuration Serveur Ubuntu avec Vagrant

Ce projet démontre la mise en place et la configuration d'une machine virtuelle Ubuntu en utilisant Vagrant et VirtualBox. Il est conçu pour créer un environnement de développement reproductible avec des configurations réseau et des allocations de ressources spécifiques.

## Prérequis

- [VirtualBox](https://www.virtualbox.org/)
- [Vagrant](https://www.vagrantup.com/)
- Ressources système suffisantes (minimum 2 Go de RAM recommandé)
- **Note** : Assurez-vous que Docker Desktop n'est pas en cours d'exécution car il peut entrer en conflit avec VirtualBox

## Fonctionnalités

- Box Ubuntu Trusty64 (14.04 LTS)
- Configuration automatisée de VM utilisant l'Infrastructure as Code
- Configuration réseau personnalisée avec redirection de ports
- Configuration de dossiers partagés pour le développement
- Fonctionnement en mode headless
- Gestion de l'allocation des ressources

## Configuration Réseau

- Redirection de ports : Port hôte 8082 → Port invité 8080
- Réseau privé : IP statique (192.168.33.10)
- Accès réseau public activé

## Allocation des Ressources

- Mémoire : 1024 Mo de RAM
- Interface graphique : Désactivée (mode headless)
- Mises à jour des box : Désactivées par défaut

## Mise en Route

1. Clonez ce dépôt
2. Naviguez vers le répertoire du projet
3. Démarrez la VM :
   ```bash
   vagrant up
   ```

## Commandes Vagrant de Base

```bash
# Initialiser un nouveau projet Vagrant
vagrant init

# Démarrer la VM
vagrant up

# Se connecter à la VM en SSH
vagrant ssh

# Arrêter la VM
vagrant halt

# Supprimer la VM
vagrant destroy
```

## Structure des Dossiers

```
.
├── Vagrantfile           # Fichier de configuration VM
└── tomcatwebapps/       # Dossier partagé avec la VM
```

## Dépannage

Si vous rencontrez l'erreur "exit code 1" lors de `vagrant up` :

1. Vérifiez que VirtualBox est correctement installé et à jour
2. Assurez-vous que Docker Desktop n'est pas en cours d'exécution
3. Vérifiez la disponibilité des ressources système
4. Exécutez VirtualBox et Vagrant avec les privilèges administrateur
5. Consultez VBoxHardening.log pour des informations détaillées sur l'erreur

## Configuration du Vagrantfile

```ruby
Vagrant.configure("2") do |config|
  config.vm.define "server" do |webserver|
    webserver.vm.box = "ubuntu/trusty64"
    webserver.vm.box_check_update = false
    webserver.vm.network "forwarded_port", guest: 8080, host: 8082
    webserver.vm.network "private_network", type: "static", ip: "192.168.33.10"
    webserver.vm.network "public_network"
    webserver.vm.synced_folder "./tomcatwebapps", "/opt/tomcat/webapps"
    
    webserver.vm.provider "virtualbox" do |vb|
      vb.gui = false
      vb.name = "webserver-tomcat"
      vb.memory = "1024"
    end
  end
end
```

## Contribution

N'hésitez pas à forker ce dépôt et à soumettre des pull requests. Pour des changements majeurs, veuillez d'abord ouvrir une issue pour discuter des modifications que vous souhaitez apporter.

## Licence

Ce projet est sous licence MIT - voir le fichier LICENSE pour plus de détails.

## Auteur

Maimouna Ndiaye

## Remerciements

- Professeur : Ngor Seck
- Cours DevOps - Février 2025