# Ansible Role: PHP on CentOS 7

Installs PHP **5.4** or **5.5** or **5.6** or **7.0** or **7.1** on CentOS 7 using `base` or `ius` or `webtatic` or `remi` repositories.

## Requirements

Tested CentOS 7

## Get started

If you came here and you have no idea where to start and **the only think you worry is PHP 5.6 on Centos 7** do the following (copy paste as root)

    yum install -y python-jinja2 git
    git clone --recursive git://github.com/ansible/ansible.git /usr/local/src/ansible
    source /usr/local/src/ansible/hacking/env-setup
    # or include in .bashrc
    # echo "source /usr/local/src/ansible/hacking/env-setup &> /usr/local/src/ansible/ansible.login" >> ~/.bashrc
    # Now you can run ansible (sort of)

    mkdir /etc/ansible
    echo "localhost ansible_connection=local" > /etc/ansible/hosts
    # Now you can run ansible on your machine (sort of)

    # Create you first playbook
    mkdir /root/ansible-my-first-playbook /root/ansible-my-first-playbook/roles
    echo "---
    
    - hosts: all
      roles:
        - ansible-role-php" > /root/ansible-my-first-playbook/php-playbook.yml
    
    git clone --recursive https://github.com/tovletoglou/ansible-playbook-aegir-centos.git
    cd ansible-playbook-aegir-centos
    ansible-playbook php-playbook.yml

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

    # Add or remove php modules
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

    # Repository list. Do not change.
    repo_list:
      - name: "base"
        repo_package: ""
        repo_gpg_key_url: ""

      - name: "ius"
        repo_package: "https://centos7.iuscommunity.org/ius-release.rpm"
        #repo_package: "ius-release"
        repo_gpg_key_url: "/etc/pki/rpm-gpg/IUS-COMMUNITY-GPG-KEY"

      - name: "remi"
        repo_package: "http://rpms.remirepo.net/enterprise/remi-release-7.rpm"
        #repo_package: "remi-release"
        repo_gpg_key_url: "/etc/pki/rpm-gpg/RPM-GPG-KEY-remi"

      - name: "webtatic"
        repo_package: "https://mirror.webtatic.com/yum/el7/webtatic-release.rpm"
        #repo_package: "webtatic-release"
        repo_gpg_key_url: "/etc/pki/rpm-gpg/RPM-GPG-KEY-webtatic-el7"
