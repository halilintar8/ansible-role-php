# Ansible Role: PHP on CentOS 7

Installs PHP **5.4** or **5.5** or **5.6** or **7.0** or **7.1** on CentOS 7 using `base` or `ius` or `webtatic` or `remi` repositories.

## Requirements

Tested CentOS 7

## Get started

If you came here and you have no idea where to start and **the only think you worry is PHP 5.6 on Centos 7** do the following (copy paste as root)

    # Install Ansible dependencies
    yum install -y python-jinja2 git
    # Clone Ansible source from GitHub
    git clone --recursive git://github.com/ansible/ansible.git /usr/local/src/ansible
    # Register Ansible on your system
    source /usr/local/src/ansible/hacking/env-setup
    # or if you want to run permanently, include in your .bashrc
    # echo "source /usr/local/src/ansible/hacking/env-setup &> /usr/local/src/ansible/ansible.login" >> ~/.bashrc
    # Now you can run ansible (sort of)

    # Define Ansible hosts
    mkdir /etc/ansible
    # We will use the `local` in order to work on the vm you are logged in
    echo "localhost ansible_connection=local" > /etc/ansible/hosts
    # Now you can run ansible on your machine (sort of)

    # Create you first playbook
    mkdir /root/ansible-my-first-playbook /root/ansible-my-first-playbook/roles
    echo "---

    - hosts: all
      roles:
        - ansible-role-php" > /root/ansible-my-first-playbook/php-playbook.yml
    # Clone this project (role)
    git clone https://github.com/tovletoglou/ansible-role-php.git /root/ansible-my-first-playbook/roles/ansible-role-php
    # Run the playbook!
    ansible-playbook /root/ansible-my-first-playbook/php-playbook.yml

## Role Variables

Available variables are listed below, along with default values `defaults/main.yml`

    # Remove any php version before install. (true | false)
    remove_php: true

    # Update php every time you run this role (present | latest)
    yum_state: latest

    # Define if you are using a web server (true | false)
    php_webserver: true

    # Define your web server (httpd | nginx)
    webserver: httpd

    # Select php version (php | php55 | php56 | php70 | php71)
    #
    #   'php'   for: 5.4.x on repo: base --- -------- remi
    #   'php55' for: 5.5.x on repo: ---- ius webtatic remi
    #   'php56' for: 5.6.x on repo: ---- ius webtatic remi
    #   'php70' for: 7.0.x on repo: ---- ius webtatic remi
    #   'php71' for: 7.1.x on repo: ---- --- -------- remi
    #
    # Be extra careful when you choose php version.
    # Not every repository have the corresponding php version. Check the table above.
    php_version: php56

    # Repository (base | ius | webtatic | remi)
    # ius and remi needs epel repo, epel will be installed automatically if you choose one of these two.
    # If you are using php (php 5.4) use the base repo, otherwise I suggest the isu repository.
    repository: ius

    # Add or remove php modules (do not include php itself)
    php_modules:
      - cli
      - common
      - devel
      - fpm
      - gd
      - ldap
      - mbstring
      - mcrypt
      - mysqlnd
      - pdo
      - pear
      - process
      - xml

    # Create CUSTOM.php.ini (true | false)
    create_custom_php_ini: true

    # Give the name you want for the CUSTOM.php.ini (string)
    # Edit the php custom configuration in the templates/custom.php.ini.tpl
    custom_php_ini: custom

    # Create DOMAIN/phpinfo.php test page (true | false)
    create_phpinfo: true

    # Web server folder (string, followed by '/')
    webserver_folder: /var/www/html/

## Dependencies

None, if you don't have a web server (httpd, nginx) change the `defaults/main.yml`  `php_webserver: true` to `false`

## Example Playbook

Installed with galaxy.ansible `ansible-galaxy install tovletoglou.php`

    ---

    - hosts: all
      roles:
        - { role: tovletoglou.php }

Installed as a playbook git submodule  `git clone https://github.com/tovletoglou/ansible-role-php.git playbook/roles/ansible-role-php`

    ---

    - hosts: all
      roles:
        - { role: ansible-role-php }

Sending custom vars

    ---

    - hosts: all
      roles:
        - { role: tovletoglou.php }
      vars:
        remove_php: true
        php_modules:
          - cli
          - common

## License

MIT

## Author Information

Apostolos Tovletoglou [ansible-role-php](https://github.com/tovletoglou/ansible-role-php)
