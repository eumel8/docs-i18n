Zanata
======

`Zanata <https://zanata.org>`__ is a web-based translation platform for
translators, content creators and developers to manage localisation
projects. Installation is divided in server and client.

Server
------

Zanata requires Java, Apache, Wildfly, MySQL, and an e-mail service.
For easy installation we use Puppet, specially `puppet-zanata <https://github.com/openstack-infra/puppet-zanata>`__
All installation steps are done by **root**

Install deb packages::

    apt-get update
    apt install -y git puppet-common python-pip
    pip install pip -U

Clone Git repositories::

    git clone -b dev https://github.com/eumel8/puppet-zanata /etc/puppet/modules/zanata
    git clone -b v1.2.4 https://github.com/biemond/biemond-wildfly /etc/puppet/modules/wildfly
    git clone https://github.com/puppetlabs/puppetlabs-apache /etc/puppet/modules/apache
    git clone -b 3.10.0 https://github.com/puppetlabs/puppetlabs-mysql /etc/puppet/modules/mysql
    git clone https://github.com/puppetlabs/puppetlabs-stdlib /etc/puppet/modules/stdlib
    git clone https://git.openstack.org/openstack-infra/puppet-jeepyb /etc/puppet/modules/jeepyb
    git clone -b v0.5.1 https://github.com/voxpupuli/puppet-archive /etc/puppet/modules/archive
    git clone https://git.openstack.org/openstack-infra/puppet-httpd /etc/puppet/modules/httpd
    git clone https://git.openstack.org/openstack-infra/puppet-logrotate /etc/puppet/modules/logrotate
    git clone https://git.openstack.org/openstack-infra/puppet-vcsrepo /etc/puppet/modules/vcsrepo
    git clone -b 1.1.3 https://github.com/puppetlabs/puppetlabs-firewall /etc/puppet/modules/firewall
    git clone -b 1.5.0 https://github.com/camptocamp/puppet-postfix.git /etc/puppet/modules/postfix

Create MySQL client file::

    cat > /root/.my.cnf << EOF
    [client]
    user=root
    host=localhost
    password=12345678
    EOF

Create Puppet manifest::

    cat > /etc/puppet/manifest/site.pp << EOF
    #!/usr/bin/puppet apply
    
      class {'::mysql::server':
        root_password           => "12345678",
        remove_default_accounts => true,
        create_root_my_cnf      => true,
      }
    
      mysql::db { 'zanata':
        user     => 'zanata',
        password => '12345678',
        host     => 'localhost',
        grant    => ['all'],
        require  => [
          Class['mysql::server'],
          File['/root/.my.cnf'],
        ],
      }
    
      include postfix
    
      class { '::zanata':
        mysql_host                  => 'localhost',
        zanata_db_username          => 'zanata',
        zanata_db_password          => '12345678',
        zanata_openid_provider_url  => 'https://openstackid.openstack.org',
        zanata_listeners            => ['ajp'],
        zanata_admin_users          => 'admin',
        zanata_default_from_address => 'noreply@example.com',
        zanata_main_version         => 4,
        zanata_url                  => 'https://github.com/zanata/zanata-platform/releases/download/platform-4.3.3/zanata-4.3.3-wildfly.zip',
        zanata_checksum             => 'eaf8bd07401dade758b677007d2358f173193d17',
        zanata_wildfly_version      => '10.1.0',
        zanata_wildfly_install_url  => 'https://repo1.maven.org/maven2/org/wildfly/wildfly-dist/10.1.0.Final/wildfly-dist-10.1.0.Final.tar.gz',
      }
    
    
      class { '::zanata::apache':
        vhost_name              => $vhost_name,
        require                 => Class['::zanata']
      }
    
      include logrotate
      logrotate::file { 'console.log':
        log     => '/var/log/wildfly/console.log',
        options => [
                    'daily',
                    'rotate 30',
                    'missingok',
                    'dateext',
                    'copytruncate',
                    'compress',
                    'delaycompress',
                    'notifempty',
                    'maxage 30',
                    ],
        require => Service['wildfly'],
      }
    
      include jeepyb
    
      logrotate::file { 'register-zanata-projects.log':
        log     => '/var/log/register-zanata-projects.log',
        options => [
          'compress',
          'missingok',
          'rotate 30',
          'daily',
          'notifempty',
          'copytruncate',
        ],
      }
    EOF

Execute the manifest::

    /etc/puppet/manifest/site.pp

Check on the web browser if Zanata server is running. Depends on the
`filesystem <https://docs.jboss.org/author/display/WFLY10/AIO+-+NIO+for+messaging+journal>`__
, it's required to configure JBoss::

    cd /opt/wildfly/bin
    ./jboss-cli.sh
    connect
    /subsystem=messaging-activemq/server=default:write-attribute(name=journal-type, value=NIO)
    exit
    systemctl restart wildfly.service

With Puppet the configured admin user is **admin**. That means, if you
start the register process, go through the OpenID login process, you
should use the username **admin** to get admin privileges for your
account.
After login generate your API key in User Settings->Client in your
profile. 
The API key is for authorization on the server::

    cat > ~/.config/zanata.ini << EOF
    [servers]
    zanata.lxd.url=https://zanata.lxd/
    zanata.lxd.username=admin
    zanata.lxd.key=cc8492e637f9f2c161d89bb3fb776783

Now you may setup a new project in Zanata and add languages to the
project. Of course, you should join a language team.
More information for Zanata usage are available on
`OpenStack Contributor Guide <https://docs.openstack.org/contributors/common/i18n.html>`__


Client
------

Thank goodness Zanata client installation is much easier::

    cd /opt
    curl -s http://repo2.maven.org/maven2/org/zanata/zanata-cli/4.3.3/zanata-cli-4.3.3-dist.tar.gz | tar xz
    ln -s /opt/zanata-cli-4.3.3/bin/zanata-cli /usr/local/bin

That's it :-)
