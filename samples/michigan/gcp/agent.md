   1  sudo apt-get install nginx-light -y
    2  sudo nano /var/www/html/index.nginx-debian.html
    3  cd /var/www/html
    4  ls
    5  nano index.nginx-debian.html
    6  sudo nano index.nginx-debian.html
    7  vi index.nginx-debian.html
    8  sudo nano index.nginx-debian.html
    9  curl http://localhost/
   10  exit
   11  sudo apt search php
   12  sudo apt-get install mysql-server -y
   13  sudo service mysql restart
   14  sudo mysql_secure_installation
   15  sudo nanp /etc/apt/sources.list
   16  sudo nano /etc/apt/sources.list
   17  sudo apt-get update
   18  sudo apt-get install php5-fpm php5-mysql
   19  sudo nano /etc/php5/fpm/php.ini
   20  nano systemctl restart php5-fpm
   21  sudo systemctl restart php5-fpm
   22  sudo nano /etc/nginx/sites-available/default
   23  cd /var/www/html
   24  ls
   25  nano index.html
   26  sudo nginx -t
   27  sudo nano /etc/nginx/sites-available/default
   28  sudo nginx -t
   29  sudo system ctl reload nginx
   30  sudo systemctl reload nginx
   31  cd /var/www/html
   32  sudo nano info.php
   33  ls
   34  vi info.php
   35  tail -f /var/log/nginx/error.log
   36  vi  /var/log/nginx/error.log
   37  vi /etc/nginx/sites-available/default
   38  cd /etc/
   39  ls
   40  ls -l /var/run/php-fpm/php-fpm.sock
   41  sudo ls -l /var/run/php-fpm/php-fpm.sock
   42  cd /var/www/html
   43  ls
   44  rm index.html
   45  sudo rm index.html
   46  ls
   47  vi index.nginx-debian.html
   48  sudo vi index.php
   49  ls
   50  sudo vi index.nginx-debian.html
   51  cd /var/www/html
   52  ls
   53  sudo vi index.php
   54  sudo vi index.nginx-debian.html
   55  ls
   56  sudo nano index.nginx-debian.html
   57  sudo vi index.php
   58  history 50
   59  sudo systemctl reload nginx
   60  ls
   61  rm index.nginx-debian.html
   62  sudo index.nginx-debian.html
   63  sudo rm index.nginx-debian.html
   64  ls
   65  sudo systemctl reload nginx
   66  cd ../
   67  ls
   68  mkdir ini
   69  sudo mkdir config
   70  ls
   71  cd config
   72  ls
   73  sudo vi web-db.ini
   74  cd ../html
   75  sudo vi index.php 
   76  cd ../config/web-db.ini 
   77  sudo vi ../config/web-db.ini
   78  sudo vi index.php 
   79  sudo nano index.php 
   80  exit
   81  ps aux
   82  ps aux | grep php
   83  history 20
   84  cd /var/www/hhhp
   85  cd /var/www/html
   86  ls
   87  tail /var/log/nginx/error.log
   88  cd /var/log
   89  ls
   90  tail /var/log/syslog
   91  tail /var/log/nginx/error.log
   92  cd /etc
   93  ls
   94  cd php5
   95  ls
   96  cd fpm
   97  ls
   98  cd pool.d
   99  ls
  100  vi www.conf
  101  sudo vi www.conf
  102  vi www.conf
  103  cd /etc/init.d/
  104  ls
  105  /etc/init.d/php5-fpm restart
  106  /etc/init.d/php-fpm restart
  107  service php5-fpm restart
  108  sudo systemctl reload php5-fpm
  109  sudo systemctl reload php-fpm
  110  sudo systemctl reload php5-fpm
  111  sudo systemctl reload nginx
  112  sudo systemctl reload php5-fpmhistory 20
  113  history 30
  114  tail /var/log/nginx/error.log
  115  cd /var/www/html
  116  ls
  117  tail /var/log/syslog
  118  history 50
  119  sudo systemctl reload php5-fpm
  120  sudo systemctl reload nginx
  121  sudo systemctl reload php5-fpm
  122  tail /var/log/syslog
  123  tail /var/log/nginx/error.log
  124  sudo systemctl restart php5-fpm
  125  systemctl status php5-fpm.service
  126  sudo systemctl status php5-fpm.service
  127  history 20
  128  cd /var/log
  129  ls
  130  tail php5-fpm.log
  131  ls
  132  sudo tail php5-fpm.log
  133  cd /var/run/php
  134  cd /var
  135  ls
  136  cd run
  137  ls
  138  history 30
  139  history 100
  140  sudo vi /etc/php5/fpm/pool.d/www.conf
  141  systemctl status php5-fpm.service
  142  sudo systemctl restart php5-fpm
  143  systemctl status php5-fpm.service
  144  sudo tail /var/log/php5-fpm.log
  145  tail /var/log/nginx/error.log
  146  systemctl reload nginx
  147  sudo systemctl reload nginx
  148  sudo tail /var/log/php5-fpm.log
  149  history 20
  150  sudo vi /etc/php5/fpm/pool.d/www.conf
  151  cd /var/run
  152  ls
  153  sudo vi /etc/php5/fpm/pool.d/www.conf
  154  sudo systemctl restart php5-fpm
  155  tail /var/log/nginx/error.log
  156  sudo vi /etc/php5/fpm/pool.d/www.conf
  157  cd /etc/nginx/
  158  ls
  159  cd sites-enabled
  160  ls
  161  sudo vi default
  162  sudo nginx -t
  163  sudo systemctl reload nginx
  164  sudo vi /etc/php5/fpm/pool.d/www.conf
  165  tail /var/log/nginx/error.log
  166  sudo vi /etc/php5/fpm/pool.d/www.conf
  167  history 30
  168  sudo systemctl restart php5-fpm
  169  systemctl status php5-fpm.service
  170  cd ~
  171  sudo rm /var/www/html/info.php
  172  exit
  173  curl -sSO https://dl.google.com/cloudagents/install-monitoring-agent.sh
  174  sudo bash install-monitoring-agent.sh
  175  curl -sSO https://dl.google.com/cloudagents/install-logging-agent.sh
  176  sudo bash install-logging-agent.sh
  177  exit
  178  ping www.google.com
  179  exit
  180  sudo service nginx start
  181  sudo service start
  182  cd /var
  183  ls
  184  cd www
  185  ls
  186  cd config/
  187  ls
  188  cd ../html/
  189  ls
  190  nano index.php 
  191  sudo service --
  192  sudo service nginx restart
  193  exit
  194  ping 10.0.0.2
  195  gcloud compute firewall-rules list --filter="network:temt"
  196  exit
  197  ping privatenet-bastion
  198  ping 10.0.0.2
  199  exit
  200  sudo apt-get install -y nginx-light
  201  curl http://www.google.com
  202  sudo apt-get install -y host
  203  host www.wikipedia.org 8.8.8.8
  204  ping -c 5 www.google.com
  205  exit
  206  sudo systemctl restart nginx
  207  dpkg -l google-fluentd
  208  curl -sSO https://dl.google.com/cloudagents/install-logging-agent.sh
  209  sudo bash install-logging-agent.sh
  210  dpkg -l google-fluentd
  211  dpkg -l stackdriver-agent
  212  curl -sSO https://dl.google.com/cloudagents/install-monitoring-agent.sh
  213  sudo bash install-monitoring-agent.sh
  214  ls
  215  dpkg -l stackdriver-agent
  216  (cd /etc/nginx/conf.d/ && sudo curl -O https://raw.githubusercontent.com/Stackdriver/stackdriver-agent-service-configs/master/etc/nginx
/conf.d/status.conf)
  217  history 40
  218  sudo systemctl rload nginx
  219  sudo systemctl reload nginx
  220  (cd /opt/stackdriver/collectd/etc/collectd.d/ && sudo curl -O https://raw.githubusercontent.com/Stackdriver/stackdriver-agent-service-c
onfigs/master/etc/collectd.d/nginx.conf)
  221  sudo service stackdriver-agent restart
  222  exit
  223  history 20
  224  sudo systemctl restart nginx
  225  exit
  226  sudo systemctl reload nginx
  227  exit
  228  history 40
  229  sudo service stackdriver-agent restart
