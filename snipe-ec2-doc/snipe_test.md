# "Deploying Snipe-IT Inventory Management System on AWS EC2: A Step-by-Step Manual Guide"

## create ec2 instance on aws console
- select ec2 dashboard
- click on launch
- AMI: Ubuntu Server 22.04 LTS (free tier eligible)
- Instance type: t2.micro
- Key pair: create and select key
- Network settings: Create your own security group allowing all traffic SSH traffic from Anywhere for the purpose of this project. Leave everything else as the default settings.
- Configure storage: 25 GiB gp2
- Launch the instance and then connect to it with SSH.

![Screenshot 2023-10-20 101330](https://github.com/nikhilk1699/snipe-it-manual-test/assets/109533285/0781cabc-d0a2-49bb-a760-d3525afd2e05)
![1](https://github.com/nikhilk1699/snipe-it-manual-test/assets/109533285/69af6c5f-b5ae-487e-b79f-27b60b9a7857)
![2](https://github.com/nikhilk1699/snipe-it-manual-test/assets/109533285/775432a4-ea82-4d4d-aed3-13fa1338f4f9)
![3](https://github.com/nikhilk1699/snipe-it-manual-test/assets/109533285/b53c4bfb-3b18-46cb-b749-3ca39781de84)
![4](https://github.com/nikhilk1699/snipe-it-manual-test/assets/109533285/0aa0e063-e32e-4904-872a-8c536f2441cf)
![5](https://github.com/nikhilk1699/snipe-it-manual-test/assets/109533285/2a5de906-4b10-4718-b082-03400abb0df4)
![6](https://github.com/nikhilk1699/snipe-it-manual-test/assets/109533285/338eb4dd-0f73-466c-affc-3dd4a6529093)

### update he packages using root user by running below command
```
apt update
```
### Installation apache2 using shell script
#### vi apache2-setup.sh

```
#!/bin/bash

# Install Apache2
sudo apt install apache2 -y

# Start Apache2 and enable it on boot
systemctl start apache2
systemctl enable apache2

# Check the status of Apache2
systemctl status apache2

# Enable mod_rewrite for Apache2
sudo a2enmod rewrite

# Restart Apache2 to apply the changes
systemctl restart apache2

# Display a message
echo "Apache2 installation and configuration completed."
```

