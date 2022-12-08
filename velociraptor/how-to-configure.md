# How to Configure

### Instant Velociraptor

If you want to quickly set up Velociraptor, without the need to install multiple VMs for clients, end-servers, etc. then Instant Velociraptor will be perfect. It's a fully functioning Velociraptor system that is only deployed to your local machine. Download the Velociraptor executable from the GitHub pages, then run the command ^/gui.

This will automatically create new server and client configuration files.

* The server only listens on the local loopback interface
* The client then connects to the server over the loopback
* A single administrator user is created with the username 'admin' and password 'password'
