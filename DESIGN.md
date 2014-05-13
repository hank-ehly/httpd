* Introduction
  The HTTPD cookbook is a "reboot" of the apache2 cookbook. The goal
  here is go design it from the ground up, with modern "best
  practices" (2014), taking full advantage of the test tooling.

  The cookbook will provide primitives for manipulating various
  aspects of running an apache web server. Ideally, we want to use
  platform native idioms when dealing with a technology, so that
  administrators will have as few surprises as possible. This is a
  challenge to do in a cross-platform way, for various reasons.

  Most platforms package Apache in their own special way and provide
  different defaults out of the box defaults. These idioms are often
  encoded into the package management systems. For example,
  installing an apache module package will oftem drop off configuration in
  specific locations, and have selinux or apparmor policies that will
  break if you deviate from them.

  For bonus points, the configuration file formats, defaults, and
  idioms change from Apache 2.2 to Apache 2.4.
  
  This document serves as a place to document the exploration of these
  idioms, and will be referenced during the implementation of the
  platform providers.
   
* Primitives
  
  ```
  httpd_service 'default' do
    version '2.4'
    listen_addresses ['1.2.3.4', '5.6.7.8']
    listen_ports ['80','443']
    contact 'sysop@computers.biz'
    timeout '400'
    keepalive true
    keepaliverequests '100'
    keepalivetimeout '5'
  end

  httpd_module 'php5' do
    filename 'libphp5.so'
    action :create
  end

  httpd_module 'ssl' do
    action :enable
  end

  httpd_module 'dav' do
    action :disable
  end

  httpd_vhost 'example.computers.biz' do
    servername => 'example.computers.biz',
    port       => '80',
    docroot    => '/var/www/example.computers.biz',
    action :create
  end

  httpd_vhost 'example.computers.biz ssl' do
    servername => 'example.computers.biz'
    port       => '443'
    docroot    => '/var/www/example.computers.biz'
    ssl        => true
    action :create
  end
  ```

* Debian
  