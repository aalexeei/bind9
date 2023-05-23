#Installing and Configuring Bind9 on Ubuntu

##Step 1: Update System Packages
Before installing Bind9, it's a good practice to update the system's package repositories to ensure you have the latest available packages. Open a terminal and run the following command:

```bash
sudo apt update
```
##Step 2: Install Bind9
To install Bind9, run the following command in the terminal:

```bash
sudo apt install bind9
```
##Step 3: Configure Bind9
Once Bind9 is installed, you need to configure its settings. Here's how you can do it:

Open the Bind9 configuration file using a text editor. In this example, we'll use nano:
```bash
sudo nano /etc/bind/named.conf.options
```
Inside the file, locate the options section and configure the following settings:
```bash
options {
    directory "/var/cache/bind";
    recursion yes;
    allow-recursion { any; };
    listen-on { any; };
    forwarders {
        8.8.8.8;
        8.8.4.4;
    };
};
```
directory: Specifies the directory where Bind9 will store its cache and zone files.
recursion: Enables recursive queries.
allow-recursion: Specifies the IP addresses or networks that are allowed to make recursive queries. In this example, any allows any client to make recursive queries.
listen-on: Defines the IP addresses on which Bind9 will listen for DNS queries. In this example, any listens on all available network interfaces.
forwarders: Sets the IP addresses of DNS servers to which Bind9 will forward unresolved queries. Here, we're using Google's DNS servers as an example.
Save the changes and exit the text editor.

##Step 4: Configure DNS Zones
Bind9 uses zone files to define DNS records. You can create your own zone files or use pre-configured ones. Here's a basic example of configuring a forward zone:

Create a new zone file using a text editor. For example, create a file named example.com.zone:
```bash
sudo nano /etc/bind/zones/db.com
```
Inside the zone file, add the following lines:
```bash
;
; BIND data file for local loopback interface
;
$TTL    604800
@       IN      SOA     com. admin.com. (
                              2         ; Serial
                         604800         ; Refresh
                          86400         ; Retry
                        2419200         ; Expire
                         604800 )       ; Negative Cache TTL
;
@       IN      NS      com.
@       IN      A       ip_dns_server
example     IN      A       ip_server
fm.example  IN      CNAME   example
firmware.example    IN      CNAME   example
```
This example sets up a zone for the example.com domain with two name servers (ns1 and ns2) and an A record for www pointing to 192.168.1.20.
Save the zone file and exit the text editor.

##Step 5: Include Zone Configuration
After creating the zone file, you need to include it in the Bind9 configuration:

Open the Bind9 options file for editing:
```bash
sudo nano /etc/bind/named.conf.local
```

Inside the file, add the following line to include the zone configuration:
```bash
zone "com" {
    type master;
    file "/etc/bind/zones/db.com";
};
```
This line specifies that the example.com domain is a master zone and the zone file is located at /etc/bind/zones/example.com.zone.
Save the changes and exit the text editor.
Step 6: Restart Bind9
Once you have completed the configuration, restart the Bind9 service to apply the changes:

```bash
sudo service bind9 restart
```
Step 7: Verify Bind9 Configuration
To verify that Bind9 is running and configured correctly, you can perform a DNS query. For example, you can use the dig command to query the A record of the www.example.com domain:

```bash
dig www.example.com
```
If Bind9 is configured correctly, you should see the IP address associated with the www record in the DNS response.

Congratulations! You have successfully installed and configured Bind9 on Ubuntu.

Remember to adjust the configurations according to your specific requirements and domain settings.
