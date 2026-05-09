===========================================
Mercury Load Balancer (Infrastructure) Docs
===========================================

`Mercury Infrastructure <https://mercury-docs.readthedocs.io/>`_ is a flexible load-balancing and proxying solution. This section covers the configuration for the infrastructure nodes, separate from the Mercury Payment API.

Below is a detailed breakdown of each configurable section in the Mercury configuration file.

Global Settings
---------------

Settings are defined in the `[settings]` block:

- **`manage_network_interfaces`**:  
  Allows Mercury to add VIPs to network interfaces. This is required for internal proxying or integration with HAProxy.  
  **Default**: "yes"  
  **Values**: "yes", "no"

- **`enable_proxy`**:  
  Enables the internal proxy for load balancing. Disable this option if using external proxy programs or DNS-only setups.  
  **Default**: "yes"  
  **Values**: "yes", "no"

Logging
-------

Log settings are defined in the `[logging]` block:

- **`level`**:  
  Specifies the log verbosity.  
  **Default**: "info"  
  **Values**: "debug", "info", "warn", "error"

- **`output`**:  
  Determines where the log information is written.  
  **Default**: "/var/log/mercury"  
  **Values**: "stdout", file path

Web
---

Web interface settings are defined in the `[web]` block:

- **`binding`**:  
  IP address for the web interface to listen on.  
  **Default**: "0.0.0.0"

- **`port`**:  
  Port for the web interface.  
  **Default**: 9001  

- **TLS Settings**:  
  Defined in `[web.tls]` for enabling SSL with certificates.

- **Authentication**:  
  Configurable options for local or LDAP-based authentication are provided in `[web.auth]`.

Cluster
-------

Cluster settings are defined in the `[cluster]` block:

- **Cluster Binding**:  
  Defines the name, IP address, and authentication key for cluster communication:
  - `name`
  - `addr`
  - `authkey`

- **Settings**:  
  Includes configuration for timeouts, retry intervals, ping intervals, and the port for cluster communication.

- **TLS Configuration**:  
  Optional SSL settings are available for secure cluster communication.

- **Nodes**:  
  An array of load balancer nodes to form a cluster.

DNS
---

DNS settings are defined in the `[dns]` block:

- **`binding`**:  
  IP address for the DNS service to listen on.  
  **Default**: "0.0.0.0"

- **`port`**:  
  Port for the DNS service.  
  **Default**: 53

- **`allow_forwarding`**:  
  CIDRs allowed for DNS forwarding requests.

- **`allow_requests`**:  
  Supported DNS request types. Examples include "A", "CNAME", "TXT".

TLS Attributes
--------------

TLS settings apply to various sections like web, cluster, listener, backend, and health checks:

- **Version Control**:  
  Configure the minimum and maximum TLS versions.

- **Cipher Suites**:  
  Specify cipher preferences for secure connections.

- **Certificates**:  
  Define paths to SSL certificates and keys.

- **Client Authentication**:  
  Options for requiring client certificates.

Supported TLS versions include **TLS 1.2**, **TLS 1.3**, and legacy versions. Recommended cipher suites for HTTP/2 support include `TLS_ECDHE_RSA_WITH_AES_128_GCM_SHA256`.

Additional Notes
----------------

- **LDAP Authentication**:  
  Enabling LDAP disables local authentication, requiring an LDAP-authenticated account.

- **Recommended TLS Cipher**:  
  Use `TLS_ECDHE_RSA_WITH_AES_128_GCM_SHA256` for HTTP/2 support.

- **Curve Preferences**:  
  Supported curves include CurveP256, CurveP384, CurveP521, and X25519.

Context for Mercury Usage
-------------------------

Mercury is designed for scenarios where robust load balancing, clustering, or DNS services are required. Its configuration flexibility makes it suitable for environments ranging from simple proxy setups to complex clustered infrastructures. Understanding and tailoring these settings ensures the system runs securely and efficiently.
