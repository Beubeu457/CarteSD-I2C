Compte rendu projet capteur température,carte SD et écran LCD


Ce projet de 5 heures consiste à afficher dans le moteur série et dans l’écran LCD la température et l’humidité que capte le DHT11. Les données doivent aussi être enregistrées dans une carte SD via un module relié à l’arduino.

Composants : 

Pour réaliser ce projet j’ai eu besoin : 

 D’un module MH-SD Card Module.
 D’une carte SD formaté en FAT16 ou FAT32 (important !).
 D’un écran LCD 1602 avec un module I2C soudé.
 Du capteur de température DHT11.
 D’une arduino mega et de 13 câbles.

Ce projet nécessite des connaissances pour faire fonctionner le capteur DHT11 et l’écran LCD 1602 avec module I2C soudé.

Ce document va aller plus en profondeur pour le module MH-SD Card Module.


MH-SD Card Module :  

Tout d’abord il a fallu analyser le module du port de carte SD. En trouvant une documentation sur internet j’ai pu en comprendre mieux sur son fonctionnement.

Donc j’ai commencé par câbler le module à l’arduino.


			





GND
Va au GND de l’arduino.


3V3 ou 3.3
Va à la borne 3.3 Volt de l’arduino (recommandé).


5V 
Va à la borne 5 Volt de l’arduino.


CS
Va à la borne CS de l’arduino (borne 53 pour arduino mega).


MOSI
Va à la borne MOSI de l’arduino (borne 51 pour arduino mega).


SCK
Va à la borne SCK de l’arduino (borne 52 pour arduino mega).


MISO
Va à la borne MISO de l’arduino (borne 50 pour arduino mega).


GND
Va au GND de l’arduino

Note : Il faut brancher les deux GND du module, ils sont indispensables si vous en brancher un seul, le module ne fonctionnera pas.

Note bis : Le 3.3 Volt est recommandé car le module ne supporte pas correctement le 5 Volt. Utilisé du 5 Volt va certes fonctionner mais va accélérer l’usure du module.

Ensuite une fois le module câbler il faut donc programmer le module pour qu’il puisse reconnaitre la carte SD, lire et écrire des données dessus.

Code en entier sur le site : https://github.com/Beubeu457/CarteSD-I2C.git

Il a fallu télécharger et importer 3 bibliothèques : 
 
    • SD.h
    • LiquidCrystal_I2C.h
    • DHT.h  

Commençons par le code pour le module de la carte SD.
 Les premières lignes déclarent les variables pour localiser le port CS de l’arduino (borne 53).

Elles déclarent aussi “temp.txt” et “hum.txt” qui serviront à enregistrer la température et l’humidité dans deux fichiers texte différents.

La fonction “File” permet de déclarer une variable en tant que fichier.



Sur la capture ci-dessus permet d’initialiser la communication avec l’arduino et vérifier si elle est disponible. 

“SD.begin()” sert justement à initialiser la communication et vérifie la disponibilité de la carte SD. Elle retourne une valeur si la carte est reconnue c’est la raison pour laquelle elle se situe dans un “if()”.



Dans cette capture il y a deux variables différentes l’une s’appelle “SD.open()” cette fonction permet d’ouvrir un fichier texte ou autre ou de le créer si il n’existe pas.

“monFichier.close()” permet de fermer le fichier ouvert et de l’enregistrer, si dans le code je n’aurais pas mis cette fonction, toutes les données ne seraient pas enregistrées.

“FILE_WRITE” permet de dire que je veux écrire dans le fichier. 
Il existe aussi “FILE_READ” qui permet de lire dans le fichier.



Dans cette dernière partie du code pour le module de la carte SD j’ai ajouté les fonctions pour enregistrer les données. On retrouve les fonctions “SD.open()” et “monFichier.close()” mais également “monFichier.print()”, cette fonction sert à écrire les données une fois le fichier ouvert.

La variable “temp” contient la température que le DHT11 à capturer.

Passons maintenant au code du DHT11.



Ces trois lignes servent à définir la borne où les données seront reçues par l’arduino, à définir le type du composant car il existe le DHT11 et DHT22.

Ensuite on retrouvera “dht.begin()” qui donnera l’ordre au capteur de commencer à capturer la température.



Les fonctions “dht.readTemperature()” et “dht.readHumidity()” permettent de relever la température et l’humidité, elles sont associées aux variables “temp” pour température et “hum” pour humidité.

Pour finir, passons à l’écran LCD commandé en I2C.



Cette fonction permet d’initialiser la communication avec l’écran via l’adresse 0x27 de l’écran.



Ces fonctions permettent d'allumer l'auto éclairage de l’écran et d’initialiser encore une fois la communication ainsi de supprimer le texte si il y en a qui sont affichés.


Ces fonctions servent à afficher la dernière humidité et température que le capteur enregistre.







