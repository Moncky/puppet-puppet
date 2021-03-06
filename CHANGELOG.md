# Changelog

## 5.0.0
* New or changed parameters:
    * Add new server_* parameters for Puppet Server 2.x configuration options,
      including whitelists for admin/CA clients and Ruby/SSL options
    * Add server_puppetserver_version parameter, which should be set if not
      using the latest version of Puppet Server for correct configuration
    * Add server_use_legacy_auth_conf parameter for Puppet Server 2.0-2.1
      compatibility with pre-HOCON auth configs (GH-372)
    * Add server_ip for configuring the listen IP (puppetserver only)
    * Add server_main_template parameter for separate server puppet.conf lines
    * Add passenger_min_instances and passenger_pre_start for passenger tuning
    * Add client_certname to set a custom client certificate name (GH-378)
    * Allow server_common_modules_path to be unset to disable basemodulepath
    * Remove passenger_max_pool which had no effect
* Other features:
    * Support Puppet Server 2.x, defaulting to configuration for 2.4 and 2.5
    * Use puppetserver by default with AIO packages
    * Permit access to resource_type API for smart proxy support
* Other changes and fixes:
    * Paths to Puppet directories and configuration files updated for AIO
      agent and server locations
    * Use ip_to_cron from voxpupuli/extlib (GH-391)
    * Respect server_certname for Puppet Server SSL paths
    * Move default manifest creation to server config (GH-365)
    * Fix hiera_config location for Puppet 4.0-4.4
    * Fix ordering of server SSL directory before private_keys subdirectory
    * Fix ordering of foreman/foreman_proxy users to after server config
    * Fix puppet::server::env modulepath default to follow basedir parameter
    * Move server parameters and validation to puppet::server
    * Remove autosign from main puppet.conf section
    * Remove management of namespaceauth.conf
* Compatibility warnings:
    * The autosign parameter now takes only the path to the autosign file or
      a boolean. An additional parameter, autosign_mode, was added to set the
      file mode of the autosign file/script.
    * Support for Puppet 3.0.x has been removed, 3.1.0 or higher is required

## 4.3.2
* Other changes and fixes:
    * Add EL5 to service management conditionals (GH-404)

## 4.3.1
* Other changes and fixes:
    * set hiera_config correctly on puppet 4
    * let puppetdb_conf notify the puppetmaster service

## 4.3.0
* New or changed parameters:
    * Add server_git_repo_mode, group and user parameters for repo ownership
    * Add systemd.timer value to runmode parameter to run the agent from
      systemd timers, add systemd_cmd and systemd_unit_name parameters
    * Add unavailable_runmodes parameter to limit which _other_ runmodes are
      not possible when configuring the agent
* Other features:
    * Support Ubuntu 16.04
* Other changes and fixes:
    * Support Puppet 3.0 minimum
    * Use lower case FQDN to access Foreman from ENC/report processors (#8389)
    * Move reports setting to main puppet.conf section (GH-311)
    * Expose v1 /status endpoint in auth.conf (GH-338)
    * Update Puppet 3.8.x package name on FreeBSD
    * Fix default systemd and cron commands with AIO package (GH-340)
    * Fix ownership of environment.conf (GH-349, GH-350)
    * Support Fedora 21, remove Debian 6 (Squeeze)

## 4.2.0
* New or changed parameters:
    * Add codedir parameter, for Puppet code directory
    * Add package_source parameter to provide package location on Windows
    * Add dir_owner/dir_group parameters for base Puppet agent dir ownership
    * Add various server_jvm parameters to manage Puppet Server JVM settings
    * Add autosign parameter to override autosign.conf location or script
    * Add server_default_manifest parameters to manage the Puppet master's
      default manifest
    * Add server_ssl_dir_manage parameter to control presence of ssl_dir
* Other features:
    * Add Puppet agent AIO support
    * Manage Puppet 4 on FreeBSD
* Other changes and fixes:
    * Ensure server_manifest_path directory exists
    * Disable generation of Puppet CA when server_ca parameter is false
    * Fix parameter names in README example

## 4.1.0
* New or changed parameters:
    * Add sharedir parameter to configure /usr/share/puppet location
    * Add manage\_packages parameter to change whether to manage agent,
      master, both packages (true) or none (false)
* Other features:
    * Support Puppet master setup on FreeBSD
* Other changes and fixes:
    * Explicitly set permissions and ownership where necessary to stop
      site-wide defaults applying

## 4.0.1
* Update auth.conf for Puppet 4 API v3 endpoints
* Expand $ssldir in puppet.conf
* List incompatibility with puppetlabs/puppetdb 5.x

## 4.0.0
* New or changed parameters:
    * Add server\_http\_* parameters to configure the master to listen on HTTP
      for reverse proxy scenarios
    * Add server_version parameter to control package version of Puppet master
    * Add server\_environment\_timeout parameter to control caching of all
      environments
    * Add environment parameter to set the default Puppet agent environment
* Other features:
    * Replace theforeman/concat_native with puppetlabs/concat
    * Reload, not restart the Puppet agent service where possible
* Other changes and fixes:
    * Add documentation on environment parameters used with R10K
    * Set mode/owner/group on common module directories
    * Fix incorrect additional_settings documentation
    * Fix server_node_terminus behaviour under future parser
    * Fix generation of SSL certificates with restrictive umask
    * Fix default location of classes.txt to statedir
    * Do not set configtimeout under Puppet 4
    * Test under future parser and Puppet 4

## 3.0.0
* New or changed parameters:
    * Add additional_settings, agent_additional_settings and
      server_additional_settings parameters to manage miscellaneous main, agent
      and master configuration options respectively
    * Add ca_port parameter to change Puppet CA port
    * Add listen_to parameter to control auth.conf entries for kick/run
    * Add module_repository parameter to change puppet module server
    * Add prerun/postrun_command parameters to run command after Puppet run
    * Add puppetfactsource parameter, set default to work with SRV records
    * Add remove_lock parameter to control auto-enabling of Puppet agent
    * Add server_foreman parameter to control Foreman/Puppet master integration
    * Add server_puppetdb_* parameters for PuppetDB client configuration
    * Add server_parser parameter to change default Puppet parser
    * Add server_rack_arguments parameter to control Puppet master startup
    * Add server_request_timeout parameter to change Foreman ENC/report
      processor timeouts (#9286)
    * Add service_name parameter to override Puppet agent service name
    * Add owner, group, mode parameters to puppet::env
* Other features:
    * Make Foreman integration optional, no longer rely on foreman::params
    * theforeman/foreman module dependency is now optional, add it manually if
      you require Foreman integration (incompatible change)
    * theforeman/git module dependency optional, add it manually if enabling
      server_git_repo (incompatible change)
    * Add PuppetDB integration, configuring the master to send data to it
    * Add support for managing agent on FreeBSD
    * Add support for managing agent on Windows
    * Enable CRL checking for Apache 2.4 virtual host
* Other changes and fixes:
    * Improvements for Puppet 4 and future parser support
    * Manage mode on Rack application directories
    * Move directory env configuration to main section
    * Chain Foreman integration to ensure it refreshes the Puppet master
    * Fix config_version being set with directory envs, causing warning
    * Fix facts/receive_facts compatibility with theforeman/foreman 3.0.0
    * Fix puppetmaster variable definition under strict variables
    * Fix metadata quality, pin dependencies
    * Refreshed README

## 2.3.1
* Ensure that the Puppet master runs with UTF-8 locale under Rack (GH-196)

## 2.3.0
* Add server_implementation parameter to support Puppet Server
* Update SSL/TLS virtual host settings to latest recommendations
* Add syslogfacility parameter
* Add auth_allowed parameter
* Fix missing notify when Passenger is disabled (GH-183)
* Fix git warning shown by post-receive hook
* Fix order of git-shell installation for user shell
* Fix site.pp message to be clearer

## 2.2.1
* Fix relationship specification for early Puppet 2.7 releases

## 2.2.0
* Add support for directory environments, used by default on Puppet 3.6+
    * server_dynamic_environments is deprecated when
      server_directory_environments is enabled, set $server_environments = []
      instead for a similar effect
* Add puppetmaster parameter to override server setting
* Add server_environments_group and mode parameters for ownership of
  environments
* Add dns_alt_names parameter to add alternative DNS names to certs
* Add agent splaylimit and usecacheonfailure parameters
* Add hiera_config parameter
* Add use_srv_records, srv_domain and pluginsource parameters
* Masterless envs can set $runmode to 'none' to disable service and cron
* Fix SSL certificate/key filenames for uppercase hostnames (#6352)
* Ensure foreman_proxy service is refreshed after SSL certs change
* Fix stdin and stderr buffering in git post-receive hook
* Add error checking to git commands in git post-receive hook
* Typo fix in puppet.conf

## 2.1.2
* Remove Puppet agent '--disable' lock file on Debian
* Treat puppet-lint warnings as failures

## 2.1.1
* Add server_strict_variables parameter
* Update auth.conf from Puppet 3.5
* Ensure /etc/default/puppet has START=yes on Debian
* Set explicit ownership and mode on puppet.conf
* Move show_diff from agent section to main for puppet apply
* Pin to Rake 10.2.0 on Ruby 1.8

## 2.1.0
* Add a server_ca_proxy parameter for real Puppet CA hostname
* Add a allow_any_crl parameter to allow access to the CRL (#4345)
* Update to puppetlabs-apache 1.0
* Remove template source from header for Puppet 3.5 compatibility
* Only show ca_server if non-empty
* Fix missing dependency on foreman module
* Fix Modulefile specification for Forge compatibility
* Fix puppet::server::env with config_version set
* Ensure apache::mod::passenger is included
* Update puppet agent service name for Fedora 19
* Refactor puppet::config

## 2.0.0
* Switch to puppetlabs-apache from theforeman-apache
* Split agent configuration into puppet::agent::*
* Move $puppet::server_vardar into server::install
* Puppet 2.6 support removed
* Add class parameters to puppet::server::passenger
* Specify site.pp file mode to workaround PUP-1255
* Fix stdlib dependency for librarian-puppet
* Drop Puppet 3.0 and 3.1 tests
* Update tests for rspec-puppet 1.0.0

## 1.4.0
* Use concat to build puppet.conf and environment sections (Mickaël Canévet)
* Add classfile parameter (Mickaël Canévet)
* Add server_certname parameter for puppetmaster certname (Mickaël Canévet)
* Set cron hour and minutes according to runinterval (Mickaël Canévet)
* Add cron_cmd parameter (Mickaël Canévet)
* Add configtimeout parameter (Mickaël Canévet)
* Notify agent service when configs change
* Fix SSL parameter pass-through for Foreman puppetmaster setup
* Change fixture URLs from git:// to https:// (Guido Günther)
