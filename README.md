# Simple web stack

This is a one server web infrastructure that hosts the website that is reachable via `www.foobar.com`.

<img src="https://i.imgur.com/3FsEkkc.png" />


When a user types a website into their browser for example`www.foobar.com`, the browser creates a request which goes to the router, and then to the `DNS server`, which converts it to an IP address.
Next, the request is sent to the `server` with the corresponding IP address. The web server handles the requests and responses, while the application server interacts with the code base and the database to generate the appropriate response.


<h3>What is a server ?</h3>
<ul>
    <li>A server is a computer system or software application that provides services, resources, or functionality to clients over a network.</li>
</ul>
<h3>What is the role of the domain name ?</h3>
<ul>
    <li>The domain name serves as a human-readable address for the website, directing users to the server's IP address.</li>
</ul>
<h3>What type of DNS record <code>www</code> is in <code>www.foobar.com</code></h3>
<ul>
    <li>The DNS record for <code>www</code> in <code>www.foobar.com</code> is typically a CNAME (Canonical Name) record, aliasing the domain to the main domain name.</li>
</ul>
<h3>What is the role of the web server ?</h3>
<ul>
    <li>The web server's role is to handle incoming HTTP requests and deliver web content to users' browsers.</li>
</ul>
<h3>What is the role of the application server ?</h3>
<ul>
    <li>The application server interprets dynamic content requests, interacts with the code base and database, and generates HTML content for the web server to deliver.</li>
</ul>
<h3>What is the role of the database ?</h3>
<ul>
    <li>The database stores and manages the website's dynamic data.</li>
</ul>
<h3>What is the server using to communicate with the computer of the user requesting the website ?</h3>
<ul>
    <li>The server communicates with the user's computer over the internet using the HTTP or HTTPS protocol to deliver requested web content.</li>
</ul>

<h2>The issues with this infrastructure are:</h2>
<p><b>SPOF</b> (Single Point Of Failure) is essentially a flaw in the design, configuration, or implementation of a system, circuit, or component that poses a potential risk because it could lead to a situation in which just one malfunction or fault causes the whole system to stop working.</p>
<p><b>Downtime when maintenance needed</b> whenever some structure or node in the system needs to be repaired, the whole system has to be shut down, while the maintenance is done. Then, client requests cannot be attended during this period of time.</p>
<p><b>Cannot scale if too much incoming traffic</b> because there is no possibility to scale the service with additional servers as backup. Leading to a possible breakdown of the web page and client requests, as traffic surpasess servers capacity.</p>



# Distributed web infrastructure

<img width='100%' src="https://i.imgur.com/AJqLu7d.png">


We added one more primary server with its components (web server, application server, etc.), a secondary server, and a load balancer.


The load balancer works with the `round-robin algorithm` and offers an `active-active setup`, distributing requests equally between the two servers, 50% each, simultaneously.

<h2>The issues with this infrastructure are:</h2>
<p><b>SPOF in the load balancer.</b> If the load balancer fails, it would disrupt the traffic distribution to the two servers, rendering the system partially or fully inaccessible.</p>
<p><b>The absence of a firewall and HTTPS</b> presents significant security vulnerabilities in the system. Without a firewall, the servers are exposed to potential malicious attacks from unauthorized sources. A firewall acts as a barrier between the servers and the internet, filtering incoming and outgoing traffic based on predefined security rules to prevent unauthorized access and protect against various threats such as malware, intrusion attempts, and denial-of-service attacks.
Furthermore, the lack of HTTPS encryption means that data transmitted between the servers and clients is susceptible to interception and eavesdropping. This poses a significant risk, especially when sensitive information such as login credentials, personal details, or financial data is being exchanged.</p>
<p><b>The absence of monitoring</b> poses a significant security risk as it hampers the ability to detect and respond to security incidents, performance issues, and system anomalies promptly.</p>



# Secured and monitored web infrastructure

<img width='100%' src='https://i.imgur.com/E7gDPV7.png'>

In this infrastructure, we addressed security issues by implementing several measures. Firstly, we added an SSL certificate to encrypt communication, ensuring that requests are unreadable to anyone intercepting them. Secondly, we deployed three firewalls to act as barriers between the servers and the internet. These firewalls filter incoming and outgoing traffic based on predefined security rules, thereby preventing unauthorized access and protecting against threats such as malware, intrusion attempts, and denial-of-service attacks. Additionally, we implemented monitoring clients to ensure the maintenance of high-quality standards and consistency. These monitoring clients help in continuously improving resource performance.

<h2>The issues with this infrastructure are:</h2>
<p>While <b>terminating SSL at the load balancer level</b> can offer some benefits, such as improved server performance and scalability, it also introduces security risks that need to be carefully considered and mitigated.</p>
<p><b>Having only one MySQL server</b> capable of accepting writes creates a single point of failure, scalability limitations, and a risk of data loss.</p>
<p><b>Having servers with all the same components (database, web server, and application server)</b> can lead to issues such as lack of specialization, single points of failure, scalability limitations, limited flexibility, and increased complexity in maintenance and upgrades.</p>



# Scale up

<img width='100%' src='https://i.imgur.com/1bsI98z.png'>


In the "Scale up" infrastructure, we added a server with all the components (web server, application server, etc.) and a load balancer with the 'Weighted Round Robin' algorithm distributing 33% of the requests to the new server and 66% of the requests to the other load balancer. The other load balancer equally distributes requests between the other two servers, maintaining security.