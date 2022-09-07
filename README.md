# Copy-Live-Prestashop-Website-from-Server-to-Local
Instructions for cloning an online Prestashop Store to a local machine (localhost)

## How to copy your live Prestashop store 1.7.6 from server to Local

### What I used:

#### Xampp
#### Windows Command Prompt
#### WinSCP

1. Download your Prestashop store files from your live server through FTP or SSH (by generating a tar.gz file) By FTP will be slower. Save them in any folder for now.

2. Export your store database, the easiest way is through phpMyAdmin -> export or by SSH typing:  

    `mysqldump -u dbusername -p dbname > yourdatabase.sql` in the Command Prompt.

3. Inside phpMyAdmin, create a new database.

4. Now, inside phpMyAdmin import your file `yourdatabase.sql` or in the Command Prompt type:

   `mysql -u dbusername -p dbname < yourdatabase.sql`

5. Copy the Prestashop store’s files you downloaded earlier inside the Xampp folder **htdocs** creating a subfolder with the name of your store. If you are not using Xampp, make sure you copy these files inside the */var/www/html/yourprestashopstorename*.

6. Once inside your store folder, open *app/config/parameters.php* and change: database_name, database_user and database_password with the default parameters - database_name will be changed with the name of your newly created database.

7. To create a new local domain for your store in local:

    7.1 Open *C:\Windows\system32\drivers\etc\hosts* and at the bottom of this file add another line with `127.0.0.1 yourdomainname.local`

    7.2 Inside your Xampp installation folder, open *\apache\conf\extra\httpd-vhosts.conf* and add:

       <VirtualHost *:80>
        ServerAdmin your store username/email
        DocumentRoot "C:/Program Files/Xampp/htdocs/yourstore"
        ServerName yourdomainname.local
        ServerAlias yourdomainname.local
       </VirtualHost>

    7.3 Open the Command Prompt and type `ipconfig /flushdns`.
  
Now you can use yourdomainname.local instead of localhost.

8. In ps_configuration table from phpMyAdmin:

    Change PS_SHOP_DOMAIN to yourdomainname.local

    Change PS_SHOP_DOMAIN_SSL to yourdomainname.local

    Change PS_SSL_ENABLED to 0

    In ps_shop_url:

    Change domain to yourdomainname.local

    Change domain_ssl to yourdomainname.local

    Change physical_uri to the PS location if you have any subfolders, otherwise leave just “/”.
    If you have a subfolder, put it always between slashes.

9. Rename .htaccess file inside root  in _.htaccess

10. Access your Back Office by typing yourdomainname.local/adminETC, disable and enable Friendly Urls. 
    Inside Maintenance make sure your store is Enabled. 
    Make sure that the file index.php inside the root of your store has your IP inside the array `$allowed`, if not just add it.
