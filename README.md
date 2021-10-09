# Linux Server simple login system with mysql
##  Operating System Subject 
###### VMware Ubuntu Server - phpmyadmin - mysql

##  [LEARNING SITE : w3schools](https://www.w3schools.com/)

#### Change to root
<pre>
sudo su
</pre>

### after install your linux-server
<pre>
apt update
apt upgrade
</pre>

##### [More Detail - How To Install the Apache Web Server on Ubuntu 20.04](https://www.digitalocean.com/community/tutorials/how-to-install-the-apache-web-server-on-ubuntu-20-04)

#### install apache2
<pre>
apt install apache2
</pre>
<pre>
ufw allow 'Apache'
</pre>
#### make sure the service is running
<pre>
systemctl status apache2
</pre>
#### Check IP addresses
<pre>
hostname -I
</pre>


#### use mysql
<pre>
mysql
</pre>

#### CREATE DB
<pre>
CREATE database mydatabase;
</pre>

#### USE DATABASE
<pre>
USE mydatabase;
</pre>


### CREATE TABLE
<pre>
CREATE TABLE `member` (
  `UserID` int(3) unsigned zerofill NOT NULL auto_increment,
  `Username` varchar(100) NOT NULL,
  `Password` varchar(100) NOT NULL,
  `Name` varchar(100) NOT NULL,
  `Status` enum('ADMIN','USER') NOT NULL default 'USER',
  PRIMARY KEY  (`UserID`),
  UNIQUE KEY `Username` (`Username`)
) ENGINE=MyISAM  AUTO_INCREMENT=3 ;
</pre>

#### INSERT USER AND ADMIN DATA
<pre>
INSERT INTO `member` VALUES (004, 'root', 'root', 'Mr Root Ubuntu', 'ADMIN');
INSERT INTO `member` VALUES (005, 'codetopanda', 'ctp123', 'MR Codetopand Portdee', 'USER');
</pre>

#### Preparing site
#### Clone git in /var/www/html |  Move all file to /var/www/html | remove simple-linux-server-mysqldb Folder | 
<pre>
git clone https://github.com/lacakp/simple-linux-server-mysqldb.git; mv -v ./simple-linux-server-mysqldb/* ./ ; rm -r simple-linux-server-mysqldb;
</pre>

#### nano to config database
<pre>
if not root -> sudo su
then
----
nano connectdb.php
</pre>



#### Fix phpmyadmin not found (วิธีแก้หา phpmyadmin ไม่เจอ)
#####  [ref : how-to-solve-the-phpmyadmin](https://askubuntu.com/questions/387062/how-to-solve-the-phpmyadmin-not-found-issue-after-upgrading-php-and-apache)
<pre>
sudo ln -s /usr/share/phpmyadmin /var/www/html
</pre>



#### FIX phpmyadmin USER can't login ( แก้ ไม่สามารถเข้า phpmyadmin ได้)

###### mysql
<pre>
sudo mysql -p -u root
</pre>

###### 2 Create USER And assign password
<pre>
CREATE USER 'youruser'@'%' IDENTIFIED BY 'thispass';
</pre>

###### 3 Add root to youruser
<pre>
GRANT ALL PRIVILEGES ON *.* TO 'youruser'@'%' WITH GRANT OPTION;
</pre>


#### Fix  ERROR 1819 (HY000) (แก้ Error policy password เราจะเปลี่ยนให้มัน low)

###### enable the validate_password plugin
<pre>
mysql> SELECT plugin_name, plugin_status FROM information_schema.plugins WHERE plugin_name LIKE 'validate%';
mysql> install plugin validate_password SONAME 'validate_password.so';
</pre>

###### check plugin is activated?
<pre>
mysql> SELECT plugin_name, plugin_status FROM information_schema.plugins WHERE plugin_name LIKE 'validate%';
</pre>


###### set a lower password validation policy
<pre>
mysql> SET GLOBAL validate_password_policy=LOW;
OR
mysql> SET GLOBAL validate_password_policy=0;
</pre>

###### confirm the password validation policy level.
<pre>
SHOW VARIABLES LIKE 'validate_password%';
</pre>

###### Assign password
<pre>
mysql> create user ‘youruser’@’localhost’ IDENTIFIED BY ‘thisisyourpassword’;
</pre>

###### revert to the ‘MEDIUM’ password policy (กลับคืนค่าของ policy)
<pre>
mysql> SET GLOBAL validate_password_policy=MEDIUM;
</pre>



