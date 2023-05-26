# Apache2-Ansible
## Voici les étapes a suivre pour configurer des noms de domaine Apache2 avec Ansible
  ### Etape 1: 
   Installez Ansible sur votre système en utilisant les commandes suivantes:
   
     ```
     sudo apt update
     ```
     ```
     sudo apt install ansible
     ```
     
  Créez un répertoire de travail pour votre projet Ansible.
   
  ### Etape 2: Configuration d'Ansible
  Créez un fichier d'inventaire ( par exemple: inventory.ini) et spécifiez les adresses IP des serveurs dans le format suivant:php
     
     ```
     [apache_servers] 
     server1 ansible_host=<adresse_ip_server1> 
     server2 ansible_host=<adresse_ip_server2>
     ```
     
 Créez un fichier playbook.yml avec les tâches suivantes: yaml
     
     ```
     /*script.yaml*/
     - name : Configure Apache2
       Hosts : apache_servers 
       become : true 
       tasks :
     - name : Installe Apache 2 
       apt : 
         name : apache2
         state : present 
     - name : Copy virtual host configuration template:
             src : templates/virtualhost.conf.j2 
             dest : /etc/apache2/sites-available/{{ ansible_fqdn }}. conf
             notify :
                 - Reload Apache 2
     - name : Enable virtual host apache2_module : 
                 name : "{{ ansible_fqdn }}" 
                 state : present
     - name :Disable default virtual host apache2_module : 
                  name : "000-default"
                  state : absent
       handlers : 
          - name : Reload Apache 2 service: 
                          name: apache2 
                          state: reloaded
        ```
        
 Créez un répertoire "templates" dans votre répertoire def travail et créez un fichier " virtualhost.conf.j2" avec le contenu suivant : apache
 
        ```
        /*template*/
        <VirtualHost *:80> 
         ServerName { { ansible_fqdn }} 
         DocumentRoot /var/www/html
        </VirtualHost>
        ```
        
### Etape 3: Exécution du playbook Ansible
 Accédez au répertoire de travail de votre projet Ansible dans un terminal.
 Exécutez la commande suivante pour exécuter le playbook:
 
    ```
    ansible-playbook -i inventor.ini playbook.yml
    ```
    
### Etape 4: Vérification de la configuration
 Accédez à l'une des adresses IP des serveurs dans votre navigateur web pour vérifier si la configuration d'Apache2 a été correctement appliquée.
    
### Etape 5: Gestion des noms de domaine
 Pour ajouter un nouveau nom de domaine, ajoutez une nouvelle entrée dans le fichier d'inventaire( inventory.ini) avec l'adresse IP du serveur correspondant.
 Exécutez à nouveau le playbook Ansible pour configurer le nouveau nom de domaine.
   
