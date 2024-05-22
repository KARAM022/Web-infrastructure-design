# Infrastructure Web Simple

Ceci est une infrastructure web avec un seul serveur qui héberge le site web accessible via `www.foobar.com`.

<img src="https://i.imgur.com/3FsEkkc.png" />

Quand un utilisateur tape une adresse web dans son navigateur, par exemple `www.foobar.com`, le navigateur crée une requête qui va au routeur, puis au `serveur DNS`, qui la convertit en une adresse IP. Ensuite, la requête est envoyée au `serveur` avec l'adresse IP correspondante. Le serveur web gère les requêtes et les réponses, tandis que le serveur d'application interagit avec la base de code et la base de données pour générer la réponse appropriée.

<h3>Qu'est-ce qu'un serveur ?</h3>
<ul>
    <li>Un serveur est un système informatique ou une application logicielle qui fournit des services, des ressources ou des fonctionnalités aux clients via un réseau.</li>
</ul>
<h3>Quel est le rôle du nom de domaine ?</h3>
<ul>
    <li>Le nom de domaine sert d'adresse lisible par l'homme pour le site web, dirigeant les utilisateurs vers l'adresse IP du serveur.</li>
</ul>
<h3>Quel type d'enregistrement DNS <code>www</code> est dans <code>www.foobar.com</code></h3>
<ul>
    <li>L'enregistrement DNS pour <code>www</code> dans <code>www.foobar.com</code> est généralement un enregistrement CNAME (Canonical Name), aliasant le domaine au nom de domaine principal.</li>
</ul>
<h3>Quel est le rôle du serveur web ?</h3>
<ul>
    <li>Le rôle du serveur web est de gérer les requêtes HTTP entrantes et de fournir le contenu web aux navigateurs des utilisateurs.</li>
</ul>
<h3>Quel est le rôle du serveur d'application ?</h3>
<ul>
    <li>Le serveur d'application interprète les requêtes de contenu dynamique, interagit avec la base de code et la base de données, et génère du contenu HTML pour que le serveur web puisse le fournir.</li>
</ul>
<h3>Quel est le rôle de la base de données ?</h3>
<ul>
    <li>La base de données stocke et gère les données dynamiques du site web.</li>
</ul>
<h3>Avec quoi le serveur communique-t-il avec l'ordinateur de l'utilisateur demandant le site web ?</h3>
<ul>
    <li>Le serveur communique avec l'ordinateur de l'utilisateur via internet en utilisant le protocole HTTP ou HTTPS pour fournir le contenu web demandé.</li>
</ul>

<h2>Les problèmes de cette infrastructure sont :</h2>
<p><b>SPOF</b> (Single Point Of Failure) est essentiellement un défaut dans la conception, la configuration ou la mise en œuvre d'un système, d'un circuit ou d'un composant qui pose un risque potentiel car il pourrait entraîner une situation où une seule panne ou défaut cause l'arrêt complet du système.</p>
<p><b>Temps d'arrêt lors de la maintenance</b> chaque fois qu'une structure ou un nœud dans le système doit être réparé, tout le système doit être arrêté pendant la maintenance. Ensuite, les requêtes des clients ne peuvent pas être traitées pendant cette période.</p>
<p><b>Ne peut pas évoluer en cas de trop de trafic entrant</b> car il n'est pas possible d'évoluer le service avec des serveurs supplémentaires en tant que sauvegarde, ce qui peut entraîner une panne de la page web et des requêtes des clients, lorsque le trafic dépasse la capacité des serveurs.</p>

# Infrastructure Web Distribuée

<img width='100%' src="https://i.imgur.com/AJqLu7d.png" />

Nous avons ajouté un serveur principal supplémentaire avec ses composants (serveur web, serveur d'application, etc.), un serveur secondaire et un équilibrage de charge.

L'équilibrage de charge fonctionne avec l'algorithme `round-robin` et offre une configuration `active-active`, distribuant les requêtes également entre les deux serveurs, 50% chacun, simultanément.

<h2>Les problèmes de cette infrastructure sont :</h2>
<p><b>SPOF dans l'équilibrage de charge.</b> Si l'équilibrage de charge échoue, cela perturberait la distribution du trafic vers les deux serveurs, rendant le système partiellement ou totalement inaccessible.</p>
<p><b>L'absence de pare-feu et de HTTPS</b> présente des vulnérabilités de sécurité importantes dans le système. Sans pare-feu, les serveurs sont exposés à d'éventuelles attaques malveillantes de sources non autorisées. Un pare-feu agit comme une barrière entre les serveurs et l'internet, filtrant le trafic entrant et sortant selon des règles de sécurité prédéfinies pour empêcher l'accès non autorisé et protéger contre diverses menaces telles que les logiciels malveillants, les tentatives d'intrusion et les attaques par déni de service.
De plus, l'absence de chiffrement HTTPS signifie que les données transmises entre les serveurs et les clients sont susceptibles d'être interceptées et écoutées. Cela pose un risque important, surtout lorsque des informations sensibles telles que des identifiants de connexion, des données personnelles ou des informations financières sont échangées.</p>
<p><b>L'absence de surveillance</b> pose un risque de sécurité important car elle empêche de détecter et de répondre rapidement aux incidents de sécurité, aux problèmes de performance et aux anomalies du système.</p>

# Infrastructure Web Sécurisée et Surveillée

<img width='100%' src='https://i.imgur.com/E7gDPV7.png'>

Dans cette infrastructure, nous avons résolu les problèmes de sécurité en mettant en œuvre plusieurs mesures. Premièrement, nous avons ajouté un certificat SSL pour chiffrer la communication, garantissant que les requêtes ne sont pas lisibles par quiconque les intercepte. Deuxièmement, nous avons déployé trois pare-feux pour agir comme des barrières entre les serveurs et l'internet. Ces pare-feux filtrent le trafic entrant et sortant en fonction de règles de sécurité prédéfinies, empêchant ainsi l'accès non autorisé et protégeant contre les menaces telles que les logiciels malveillants, les tentatives d'intrusion et les attaques par déni de service. De plus, nous avons mis en place des clients de surveillance pour garantir le maintien de normes de haute qualité et de cohérence. Ces clients de surveillance aident à améliorer continuellement la performance des ressources.

<h2>Les problèmes de cette infrastructure sont :</h2>
<p>Bien que <b>terminer SSL au niveau de l'équilibreur de charge</b> puisse offrir certains avantages, tels qu'une amélioration des performances du serveur et de l'évolutivité, cela introduit également des risques de sécurité qui doivent être soigneusement pris en compte et atténués.</p>
<p><b>Avoir un seul serveur MySQL</b> capable d'accepter des écritures crée un point de défaillance unique, des limitations d'évolutivité et un risque de perte de données.</p>
<p><b>Avoir des serveurs avec tous les mêmes composants (base de données, serveur web et serveur d'application)</b> peut entraîner des problèmes tels que le manque de spécialisation, des points de défaillance uniques, des limitations d'évolutivité, une flexibilité limitée et une complexité accrue dans la maintenance et les mises à jour.</p>

# Mise à l'Échelle

<img width='100%' src='https://i.imgur.com/1bsI98z.png'>

Dans l'infrastructure "Mise à l'échelle", nous avons ajouté un serveur avec tous les composants (serveur web, serveur d'application, etc.) et un équilibrage de charge avec l'algorithme 'Weighted Round Robin' distribuant 33% des requêtes au nouveau serveur et 66% des requêtes à l'autre équilibrage de charge. L'autre équilibrage de charge distribue également les requêtes entre les deux autres serveurs, en maintenant la sécurité.