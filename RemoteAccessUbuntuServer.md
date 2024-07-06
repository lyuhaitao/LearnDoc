Remotely access the server on Ubuntu.

- MySQL

- Jupyter Notebook

- Jupyter Lab

- Django Web Server

- nginx Web Server

- uWSGI Web Server

# Jupyter Web Server

```conda
jupyter lab --ip 10.152.42.36 --port 8877 --no-browser
```

```conda
jupyter notebook --ip 10.152.42.36 --port 8877 --no-browser
```

# Remotely Access MySQL by IP

1. Edit the MySQL configuration file:

open the MySQL configuration file to allow remote connections.

```bash
sudo vim /etc/mysql/mysql.conf.d/mysqld.cnf
```

2. Bind to the server's IP address

Look for the line that starts with bind-address and change it to the server’s IP address (or 0.0.0.0 to bind to all IP addresses):

```ini
bind-address = 0.0.0.0 or 10.152.42.36
```

Save the file and exit the editor

3. Restart the MySQL service

Apply the changes by restarting the MySQL service

```bash
sudo systemctl restart mysql
```

4. Create a user for remote access

Log in to MySQL and create a user that can connect from any host. Replace `your_username` and `your_password` with the desired username and password.

```bash
mysql -u root -p
```

Inside the MySQL prompt, run:

```sql
CREATE USER 'your_username'@'%' IDENTIFIED BY 'your_password';
GRANT ALL PRIVILEGES ON *.* TO 'your_username'@'%' WITH GRANT OPTION;
FLUSH PRIVILEGES;
EXIT;
```

5. Allow MySQL through the firewall

If you are using `ufw`, allow MySQL through the firewall.

```bash
sudo ufw allow 3306/tcp
```

6. Connect to the MySQL server remotely

From the remote machine, you can now connect to the MySQL server using the following command:

```bash
mysql -u your_username -p -h 10.152.42.36
```
