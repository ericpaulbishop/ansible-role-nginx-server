## Ansible Nginx Role - Overview
 
Ansible role which installs and configures Nginx, from source.

This is based largely on the anxs/nginx ansible role with some major differences/improvements:

- Installation from source is the only available installation option
- Support for Passenger and GeoIP modules, as well as those supported by anxs/nginx
- Properly removes config files if reinstalled without modules that were previously present
- site configuration with reasonable SSL settings by default when ssl cert/key are specified
- Site configuration that can set multiple directives with the same name
- Monit support is controlled by nginx_monit_protection parameter and properly enabled/disabled by this variable
- Latest compile parameters are not saved until after installation, so if something goes wrong we know what current binary was built with
- Modules are enabled/disabled by single parameters, not list of modules, so single modules can be enabled/disabled relative to defaults without specifying entire list of modules
- Modules are enabled/disabled without specifying entire configure flag, just true/false


#### Requirements & Dependencies

##### Ansible

This role has been tested on Ansible 1.5 and above

##### Platforms

Currently it's been developed for, and tested on Debian. It is assumed to work on Ubuntu and other Debian distributions as well.


#### Variables

##### build parameters
- `nginx_version` - the version of Nginx to install
- `nginx_url` - URL for the Nginx source (versioned). By default it will get it from `nginx_source_version`
- `nginx_prefix` - prefix for installing nginx from source (versioned)
- `nginx_conf_path` - location of the main config file (in `nginx_dir` by default)
- `nginx_default_configure_flags` - the default configure flags (before adding the modules), it is not recommended that you change this
- `nginx_configure_flags` - a full list of the configure flags (including modules), do not change this unless you're sure you know what you are doing

The following variables can be used to enable/disable modules to include in the build by setting them to true:

- `nginx_include_http_stub_status_module` - Enabled (true) by default
- `nginx_include_http_ssl_module` - Enabled (true) by default
- `nginx_include_openssl` - Build ssl module by linking to latest openssl source, not system openssl library - Enabled (true) by default
- `nginx_include_http_gzip_static_module` - Enabled (true) by default
- `nginx_include_upload_progress_module` - Enabled (true) by default
- `nginx_include_headers_more_module` - Enabled (true) by default
- `nginx_include_http_auth_request_module` - Enabled (true) by default
- `nginx_include_http_echo_module` - Enabled (true) by default
- `nginx_include_google_perftools_module` - Enabled (true) by default
- `nginx_include_ipv6_module` - Enabled (true) by default
- `nginx_include_http_real_ip_module` - Enabled (true) by default
- `nginx_include_http_spdy_module` - Enabled (true) by default
- `nginx_include_http_perl_module` - Enabled (true) by default
- `nginx_include_http_sub_module` - Enabled (true) by default
- `nginx_include_naxsi_module` - Enabled (true) by default
- `nginx_include_ngx_pagespeed_module` - Enabled (true) by default
- `nginx_include_geoip_module` - Disabled (false) by default
- `nginx_include_passenger_module` - Disabled (false) by default
- `nginx_include_passenger_module` - Disabled (false) by default
- `nginx_include_mail_pop3_module` - Disabled (false) by default
- `nginx_include_mail_imap_module` - Disabled (false) by default
- `nginx_include_mail_smtp_module` - Disabled (false) by default



##### global configuration parameters (nginx.conf)

- `nginx_user` - user Nginx will run as
- `nginx_uid` - the uid for this user
- `nginx_group` - Nginx group
- `nginx_gid` - the gid for this group
- `nginx_dir` - location of the Nginx configuration (conf, sites-available, sites-enabled, ...)
- `nginx_www_dir` - location of the www root for Nginx sites
- `nginx_log_dir` - location of the Nginx logs
- `nginx_pid` - location of the Nginx PID file
- `nginx_worker_processes` - sets the number of worker processes
- `nginx_daemon_disable` - whether the daemon should be disabled which can be set to yes or no
- `nginx_worker_rlimit_nofile` - used for config value of `worker_rlimit_nofile`. Can replace any "ulimit -n" command. The value depend on your usage (cache or not) but must always be superior than worker_connections. Set to `null` to ignore
- `nginx_error_log_options` - option flags for the error_log
- `nginx_error_log_filename` - filename for the error log
- `nginx_worker_connections` - sets the number of worker connections
- `nginx_multi_accept` - used for config value of events { multi_accept }. Try to accept() as many connections as possible. Can be set to yes or no
- `nginx_charset` - used to specify an explicit default charset (say, 'utf-8', 'off'…)
- `nginx_disable_access_log` - whether or not to disable the access log, yes or no
- `nginx_access_log_options` - option flags for the access_log
- `nginx_server_tokens` - whether to send the Nginx version number in error pages and Server header, on or off
- `nginx_event` - used for config value of events { use }. Set the event-model. By default nginx looks for the most suitable method for your OS.
- `nginx_sendfile` - directive to activate or deactivate the usage of sendfile(), on or off
- `nginx_keepalive` - option whether to use the timeout options (below). Only the value "on" will include them
- `nginx_keepalive_timeout` - assigns the timeout for keep-alive connections with the client
- `nginx_client_body_timeout` - sets the read timeout for the request body from client
- `nginx_client_header_timeout` - specifies how long to wait for the client to send a request header
- `nginx_send_timeout` - specifies the response timeout to the client; it does not apply to the entire transfer but, rather, only between two subsequent client-read operations
- `nginx_buffers` - option whether to use the buffer options (below). Only the value "on" will include them
- `client_body_buffer_size` - specifies the client request body buffer size
- `client_header_buffer_size` - sets the headerbuffer size for the request header from client
- `client_max_body_size` - specifies the maximum accepted body size of a client request, as indicated by the request header Content-Length. Set to 0 to disable
- `large_client_header_buffers` - assigns the maximum number and size of buffers for large headers to read from client request
- `nginx_server_names_hash_bucket_size` - assigns the size of basket in the hash-tables of the names of servers. This value by default depends on the size of the line of processor cache
- `nginx_types_hash_max_size` -
- `nginx_types_hash_bucket_size` -
- `nginx_proxy_read_timeout` - defines a timeout (between two successive read operations) for reading a response from the proxied server.
- `nginx_enable_rate_limiting` - enable rate limiting, yes or no
- `nginx_rate_limiting_zone_name` - sets the shared memory zone
- `nginx_rate_limiting_backoff` - sets the maximum burst size of requests
- `nginx_rate_limit` - sets the rate (e.g. 1r/s)
- `nginx_access_logs` - a list of access log formats, filenames and options

        nginx_access_logs:
          - name: "main"
            format: '$remote_addr - $remote_user [$time_local] "$request" $status $body_bytes_sent "$http_referer" "$http_user_agent"'
            options: null
            filename: "access.log"
- `nginx_default_root` - the directory to place the default site
- `nginx_default_enable` - whether or not to actually enable the defaul site



##### Sites

There is a possibility to configure a list of servers to be available (not yet enabled) as well. Just provide a list of dictionaries according to the following format:

```yaml
nginx_sites:
  - server:
      name: foo
      listen: 8080
      server_name: localhost
      locations:
        - location:
            match: /
            try_files: "$uri $uri/ /index.html"
            sendfile: "on"
  - server:
      name: bar
      listen: 8888
      server_name: webmail.localhost
      locations:
        - location:
            match: /
            try_files: "$uri $uri/ /index.html"
        - location:
            match: /images/
            try_files: "$uri $uri/ /index.html"
```

To enable or disable specific sites you can add prior used `server_name` attribute to the variables `nginx_enabled_sites` and `nginx_disabled_sites`.

```yaml
nginx_enabled_sites:
  - localhost
```

```yaml
nginx_disabled_sites:
  - webmail.localhost
```

##### Monit Support
You can put Nginx under monit monitoring protection, by setting `nginx_monit_protection: true`


##### Module specific parameters

###### gzip module
- 'nginx_gzip' - whether to use gzip, can be "on" or "off"
- 'nginx_gzip_http_version'
- 'nginx_gzip_comp_level'
- 'nginx_gzip_proxied'
- 'nginx_gzip_vary'
- 'nginx_gzip_buffers'
- 'nginx_gzip_min_length'
- 'nginx_gzip_types'
- 'nginx_gzip_disable'

###### http_stub_status module
- `nginx_remote_ip_var`
- `nginx_authorized_ips`

###### http_gzip_static module
- `nginx_gzip_static` - whether to use gzip_static, can be on or off

###### upload_progress module
- `nginx_upload_progress_version` - version of the upload_progress module
- `nginx_upload_progress_javascript_output`- sets output in javascript. The default is true for backwards compatibility
- `nginx_upload_progress_zone_name` - assigns one name which will be used to store the per-connection tracking information. The default is proxied
- `nginx_upload_progress_zone_size` - assigns the zone size in bytes. Default is 1m (1 megabyte)

###### headers_more module
- `nginx_headers_more_version` - version of the headers_more module

###### http_auth_request module
- `nginx_auth_request_release` - the release number of the http_auth_request module

###### http_echo module
- `nginx_echo_version` - version of the http_echo module

###### http_realip module
- `nginx_realip_header` - Sets the header to use for the RealIp Module; only accepts "X-Forwarded-For" or "X-Real-IP"
- `nginx_realip_addresses` - Sets the addresses to use for the http_realip configuration
- `nginx_realip_real_ip_recursive` - If recursive search is enabled, the original client address that matches one of the trusted addresses is replaced by the last non-trusted address sent in the request header field. Can be on "on" or "off". The default is "off"

###### naxsi module
- `nginx_naxsi_version` - version of the naxsi module

#### Thanks

To the contributors:
- [Jean-Denis Vauguet](https://github.com/chikamichi)


#### Testing
This project comes with a VagrantFile, this is a fast and easy way to test changes to the role, fire it up with `vagrant up`

See [vagrant docs](https://docs.vagrantup.com/v2/) for getting setup with vagrant


#### License

Licensed under the MIT License. See the LICENSE file for details.


#### Feedback, bug-reports, requests, ...

Are [welcome](https://github.com/ANXS/nginx/issues)!
