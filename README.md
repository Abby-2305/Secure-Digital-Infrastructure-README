# Secure Digital Infrastructure

This is the configuration of the virtual machine used for hosting a web server.

## Student number

897888

## Configuration

### Accounts

Three accounts were added: 
- Marketing - sftp access only,
- Design - ssh and sftp access,
- Audit - sftp access only.

One account was deleted:
- sysadmin

### Groups

Groups added:
- sftponly: audit and marketing
- marketingViewer: audit
- designViewer: audit
- webViewer: audit and www-data

### Permissions

- home/marketing:
  - The owner is marketing who has read, write and execute permissions to allow them to read and write to their home directory
  - The owning group is marketingViewer who has read and execute permissions to allow audit to view the marketing teams files.
- home/design:
  - The owner is design who has read, write and execute permissions to allow them to store files in their home directory.
  - The owning group is designViewer who has read and execute permissions to allow audit to read the design accounts files.
- home/audit:
  - The owner is audit who has read and execute permissions to allow them to sign in using the public and private keys.
  - The owning group is audit with zero permissions because no one else needs access.
- srv/web:
  - The owner is marketing with read, write and execute permissions to allow them to read and write to the web server.
  - The owning group is webViewer with read and execute permissions to allow audit to read the web server files and to allow www-data to read the files so that the web pages can be hosted.
  
None of the above directories have any permissions for anyone else to prevent unauthorized access.

### Web server

- The web server directory is directed to from /etc/nginx/sites-enabled/default
- The index.txt file in /srv/www/student is hosted on http://stu-897888-vm1.net.dcs.hull.ac.uk/student/
- Web server files go in the directory /srv/www

### Docker

- A Docker container was created from the provided repository.
- The Docker container is directed to from /etc/nginx/sites-enabled/docker-host.
- The Docker container is hosted on http://docker.stu-897888-vm1.net.dcs.hull.ac.uk
- The Docker container has a restart policy set to unless-stopped to allow it to start automatically.
- The Docker container is set to start in the background.
- Docker container information:
  - Name: stupefied_jang
  - Gateway: 172.18.0.1
  - IP Address: 172.18.02
  - Port: 3000/TCP
  - Image: acw-docker
- A Docker network was created with the name: virtual-network and the driver: bridge, to give the Docker container a static IP address.

## Maintenance

- Update VM: sudo apt update
- View status of nginx: sudo systemctl status nginx
- Restart nginx: sudo systemctl restart nginx
- Check nginx logs: sudo journalctl -u nginx –follow
- Check nginx error logs: cat /var/log/nginx/error.log
- Serve a new directory: create a file in /etc/nginx/sites-enabled

## AI Usage

- Used to understand why the web server wasn’t being hosted despite the server block being correct and ensuring that index.txt files could be used within the configuration file.
- Used to search for commands to do checks on the configuration of the VM to ensure it was implemented correctly.

## Any Other Information
- www-data needs read permissions on any file that needs to be displayed on the web server.
- If you want to restrict another account to only use SFTP and restrict SSH access, add the user to the group sftponly.
