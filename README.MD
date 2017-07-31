keepalived
============

Role for deploy Keepalived - framework for load balancing and high availability.

Example configuration
-------------------------

```yaml
---
keepalived:
- global:
  - notification_email:
    - 'noc@example.com'
    - 'fakepager@example.com'
# From: from address that will be in the header (default is
# keepalived@<local host name>).
    notification_email_from: 'admin@example.com'
# IP address or domain name with optional port number (default 25).
    smtp_server:
    - address: '127.0.0.1'
      port: '25'
# Name to use in HELO messages. Defaults to local host name.
    smtp_helo_name: ''
# SMTP connect timeout, seconds.
    smtp_connect_timeout: '10'
# String identifying the machine (doesn't have to be hostname).
# Default: local host name.
    router_id: 'my_hostname'
# Default is 224.0.0.18.
    vrrp_mcast_group4: ''
# Optional, default 'ff02::12'.
    vrrp_mcast_group6: ''
# Sets the default interface for static addresses, default 'eth0'.
    default_interface: 'vlan3'
# Binding interface, vrrp instance and optional. Note, maxlen, port, ttl and
# group are only available on Linux 4.3 or later.
    lvs_sync_daemon:
# Binding interface.
    - interface: ''
# VRRP instance.
      vrrp_instance: ''
      options:
# Syncid (0 to 255) for lvs syncd.
        id: ''
# Maximum packet length (1..65507).
        maxlen: ''
# UDP port number to use (1..65535).
        port: ''
# Time To Live (1..255).
        ttl: ''
# Multicast group address (IPv4 or IPv6).
        group: ''
# Flush any existing LVS configuration at startup.
    lvs_flush: 'false'
# Melay for second set of gratuitous ARPs after transition to MASTER.
# Default '5', '0' for no second set.
    vrrp_garp_master_delay: '5'
# Mumber of gratuitous ARP messages to send at a time after transition
# to MASTER. Default is '5'.
    vrrp_garp_master_repeat: '5'
# Delay for second set of gratuitous ARPs after lower priority advert received
# when MASTER.
    vrrp_garp_lower_prio_delay: '10'
# Number of gratuitous ARP messages to send at a time after lower priority
# advert received when MASTER.
    vrrp_garp_lower_prio_repeat: '1'
# Minimum time interval for refreshing gratuitous ARPs while MASTER. Seconds,
# default is '0' (no refreshing).
    vrrp_garp_master_refresh: '0'
# Number of gratuitous ARP messages to send at a time while MASTER.
# Default is '1'.
    vrrp_garp_master_refresh_repeat: '1'
# Delay in ms between gratuitous ARP messages sent on an interface. Decimal,
# seconds (resolution usecs). Default is '0'.
    vrrp_garp_interval: '0.001'
# Delay in ms between unsolicited NA messages sent on an interface. Decimal,
# seconds (resolution usecs). Default is '0'.
    vrrp_gna_interval: '0.000001'
# If a lower priority advert is received, don't send another advert. This causes
# adherence to the RFCs. Defaults to 'false', unless strict_mode is set.
    vrrp_lower_prio_no_advert: 'false'
# If we are master and receive a higher priority advert, send an advert (which
# will be lower priority than the other master), before we transition to backup.
# This means that if the other master has garp_lower_priority_repeat set, it
# will resend garp messages. This is to get around the problem of their having
# been two simultaneous masters, and the last GARP messages seen were from us.
    vrrp_higher_prio_send_advert: 'false'
# Set the default VRRP version to use ('2' or '3'). Default is '2'.
    vrrp_version: '2'
# Specify the iptables chain for ensuring a version 3 instance doesn't respond
# on addresses that it doesn't own. It is necessary for the specified chain to
# exist in the iptables and/or ip6tables configuration, and for the chain to be
# called from an appropriate point in the iptables configuration. It will
# probably be necessary to have this filtering after accepting any
# 'ESTABLISHED,RELATED' packets, because IPv4 might select the VIP as the source
# address for outgoing connections. Default chain is 'INPUT'.
    vrrp_iptables:
    - input: 'keepalived_in'
# Or for outbound filtering as well. Note, outbound filtering won't work with
# IPv4, since the VIP can be selected as the source address for an outgoing
# connection. With IPv6 this is unlikely since the addresses are deprecated.
      output: 'keepalived_out'
# Keepalived may have the option to use ipsets in conjunction with iptables. If
# so, then the ipset names can be specified, defaults as below. If no names are
# specified, ipsets will not be used, otherwise any omitted names will be
# constructed by adding '_if' and/or '6' to previously specified names.
    vrrp_ipset:
    - 'keepalived'
    - 'keepalived6'
# The following enables checking that when in unicast mode, the source address
# of a VRRP packet is one of our unicast peers.
    vrrp_check_unicast_src: 'false'
# Checking all the addresses in a received VRRP advert can be time consuming.
# Setting this flag means the check won't be carried out if the advert is from
# the same master router as the previous advert received. Default - don't skip.
    vrrp_skip_check_adv_addr: 'false'
# Enforce strict VRRP protocol compliance. This will prohibit:
# 0 VIPs
# unicast peers
# IPv6 addresses in VRRP version 2
    vrrp_strict: 'false'
# The following 4 options can be used if vrrp or checker processes are timing
# out. This can be seen by a backup vrrp instance becoming master even when the
# master is still running because the master or backup system is too busy to
# process vrrp packets.
# Set the vrrp child process priority. Negative values increase priority.
    vrrp_priority: '' # from '-20' to '19'
# Set the checker child process priority.
    checker_priority: '' # from '-20' to '19'
# Set the vrrp child process non swappable.
    vrrp_no_swap: 'false'
# Set the checker child process non swappable.
    checker_no_swap: 'false'
# Specify socket to use for connecting to SNMP master agent default is
# 'unix:/var/agentx/master'. Unless using a network namespace, when the default
# is 'udp:localhost:705'.
    snmp_socket: 'unix:/var/agentx/master'
# Enable SNMP handling of vrrp element of KEEPALIVED MIB.
    enable_snmp_keepalived: 'false'
# Enable SNMP handling of checker element of KEEPALIVED MIB.
    enable_snmp_checker: 'false'
# Enable SNMP handling of RFC2787 and RFC6527 VRRP MIBs.
    enable_snmp_rfc: 'false'
# Enable SNMP handling of RFC2787 VRRP MIB.
    enable_snmp_rfcv2: 'false'
# Enable SNMP handling of RFC6527 VRRP MIB.
    enable_snmp_rfcv3: 'false'
# Enable SNMP traps.
    enable_traps: 'false'
# Enable the DBus interface.
    enable_dbus: 'false'
# Specify the default username/groupname to run scripts under. If this option is
# not specified, the user defaults to 'keepalived_script' if that user exists,
# otherwise 'root'.
    script_user:
    - username: 'automation'
# If groupname is not specified, it defaults to the user's group
      group: 'robots'
# Don't run scripts configured to be run as root if any part of the path is
# writable by a non-root user.
    enable_script_security: 'false'
# For running keepalived in a separate network namespace (not implemented
# in role because I can't find example configuration):
# Set the network namespace to run in The directory '/var/run/keepalived' will
# be created as an unshared mount point, for example for pid files. syslog
# entries will have '_NAME' appended to the ident. Note: the namespace cannot be
# changed on a configuration reload.
  net_namespace: 'keepalived_ns'
# ipsets wasn't network namespace aware until Linux 3.13, and so if running
# with an earlier version of the kernel, by default use of ipsets is disabled
# if using a namespace and vrrp_ipsets has not been specified. This options
# overrides the default and allows ipsets to be used with a namespace on
# kernels prior to 3.13.
  namespace_with_ipsets: 'false'
# If multiple instances of keepalived are run in the same namespace, this will
# create pid files with NAME as part of the file names, in
# '/var/run/keepalived'. Note: the instance name cannot be changed on a
# configuration reload.
  instance: 'name'
# Create pid files in '/var/run/keepalived'.
  use_pid_dir: 'false'
# Poll to detect media link failure otherwise attempt to use ETHTOOL or MII
# interface.
  linkbeat_use_polling: 'false'
# Keepalived can configure static addresses, routes, and rules. These addresses
# are NOT moved by vrrpd, they stay on the machine. If you already have IPs and
# routes on your machines and your machines can ping each other, you don't need
# this section. The syntax for rules and routes is that same as for ip rule
# add/ip route add. The syntax is the same for virtual addresses and virtual
# routes. If no dev element is specified, it defaults to 'default_interface'
# (default 'eth0').
- static_ipaddress:
  - '192.168.1.1/24 dev eth0 scope global'
- static_routes:
  - '192.168.2.0/24 via 192.168.1.100 dev eth0'
  - '192.168.100.0/24 table 6909 nexthop via 192.168.101.1 dev wlan0 onlink weight 1 nexthop via 192.168.101.2 dev wlan0 onlink weight 2'
  - '192.168.200.0/24 dev p33p1.2 table 6909 tos 0x04 protocol bird scope link priority 12 mtu 1000 hoplimit 100 advmss 101 rtt 102 rttvar 103 reordering 104 window 105 cwnd 106 ssthresh lock 107 realms PQA/0x14 rto_min 108 initcwnd 109 initrwnd 110 features ecn'
  - '2001:470:69e9:1:2::4 dev p33p1.2 table 6909 tos 0x04 protocol bird scope link priority 12 mtu 1000 hoplimit 100 advmss 101 rtt 102 rttvar 103 reordering 104 window 105 cwnd 106 ssthresh lock 107 rto_min 108 initcwnd 109 initrwnd 110 features ecn'
- static_rules:
  - 'from 192.168.2.0/24 table 1'
  - 'to 192.168.2.0/24 table 1'
  - 'from 192.168.28.0/24 to 192.168.29.0/26 table small iif p33p1 oif wlan0 tos 22 fwmark 24/12 preference 39 realms 30/20 goto 40'
  - 'to 1:2:3:4:5:6:7:0/112 from 7:6:5:4:3:2::/96 table 6908 uidrange 10000-19999'
# VRRP scripts:
- vrrp_script:
# Adds a script to be executed periodically. Its exit code will be recorded for
# all VRRP instances which are monitoring it.
  - name: 'script_name'
# Path of the script to execute.
    script: '/usr/bin/vrrp_script.py'
# Seconds between script invocations, default: '1'.
    interval: '1'
# Seconds after which script is considered to have failed.
    timeout: '10'
# Adjust priority by this weight, default: '0'.
    weight: '0'
# Required number of successes for OK transition.
    rise: '1'
# Required number of successes for KO transition.
    fall: '3'
# Assume script initially is in failed state.
    init_fail: 'false'
# user/group names to run script under. Group default to group of user.
    user:
    - username: 'automation'
      group: 'robots'
  - name: 'check_sshd'
    script: 'killall -0 sshd' # cheaper than pidof
    interval: '2' # check every 2 seconds
    weight: '-4'# default prio -4 if KO
    fall: '2' # require 2 failures for KO
    rise: '2' # require 2 successes for OK
  - name: 'check_haproxy'
    script: 'killall -0 haproxy'
    interval: '2'
  - name: 'check_http_port'
    script: '</dev/tcp/127.0.0.1/80' # connects and exits
    interval: '1'
    weight: '-2' # default prio -2 if connect fails
  - name: 'check_https_port'
    script: '</dev/tcp/127.0.0.1/443'
    interval: '1'
    weight: '-2'
  - name: 'check_smtp_port'
    script: '</dev/tcp/127.0.0.1/25'
    interval: '1'
    weight: '-2'
# VRRP synchronization groups:
- vrrp_sync_group:
  - name: 'VG_1'
    group:
# Name of the 'vrrp_instance'.
    - 'inside_network'
    - 'outside_network'
# Notify scripts and alerts are optional. The username and groupname specify the
# user and group under which the scripts should be run. If username is
# specified, the group defaults to the group of the user. If username is not
# specified, they default to the global 'script_user' and 'script_group' to
# MASTER transition.
    notify_master:
    - exec: '/path/to_master.sh'
      user: 'username'
      group: 'groupname'
# To BACKUP transition.
    notify_backup:
    - exec: '/path/to_backup.sh'
      user: 'username'
      group: 'groupname'
# FAULT transition.
    notify_fault:
    - exec: '/path/fault.sh VG_1'
      user: 'username'
      group: 'groupname'
# For ANY state transition. "notify" script is called AFTER the 'notify_*'
# scripts and is executed with 4 arguments provided by Keepalived (so don't
# include parameters in the notify line).
# Arguments:
# $1 = "GROUP"|"INSTANCE".
# $2 = name of the group or instance.
# $3 = target state of transition: ("MASTER"|"BACKUP"|"FAULT").
# $4 = priority value.
    notify:
    - exec: '/path/notify.sh'
      user: 'username'
      group: 'groupname'
# Send email notification during state transition, using addresses in
# 'global_defs'.
    smtp_alert: 'false'
# All VRRP share the same tracking config.
    global_tracking: 'false'
# VRRP gratuitous ARP and unsolicited neighbour advert delay groups. Specifies
# the setting of delays between sending gratuitous ARPs and unsolicited
# neighbour advertisements. This is intended for when an upstream switch is
# unable to handle being flooded with ARPs/NAs. Use interface when the limits
# apply on the single physical interface. Use interfaces when a group of
# interfaces are linked to the same switch and the limits apply to the switch as
# a whole. If the global 'vrrp_garp_interval' and/or 'vrrp_gna_interval' are
# set, any interfaces that aren't specified in a 'garp_group' will inherit the
# global settings.
  - name: 'G1'
    group:
    - 'VI_5'
    - 'VI_6'
    - 'VI_7'
    - 'VI_8'
    - 'VI_9'
    notify_master:
    - exec: '/usr/local/bin/vrrp.back arg1 arg2'
    notify_backup:
    - exec: '/usr/local/bin/vrrp.mast arg1 arg2'
    notify_fault:
    - exec: '/usr/local/bin/vrrp.fault arg1 arg2'
  - name: 'G2'
    group:
    - 'VI_3'
    - 'VI_4'
- garp_group:
# Sets the interval between Gratuitous ARP (in seconds,
# resolution microseconds).
  - garp_interval: '0.5'
# Sets the default interval between unsolicited NA (in seconds,
# resolution microseconds).
    gna_interval: '0.5'
# The physical interface to which the intervals apply.
    interface: ''
# A list of interfaces accross which the delays are aggregated.
    interfaces:
    - 'vlan130'
    - 'vlan3333'
    - 'vlan600'
# VRRP instances.
# Describes the movable IP for each instance of a group in 'vrrp_sync_group'.
# Here are described two IPs (on 'inside_network' and on 'outside_network'), on
# machine "my_hostname", which belong to the group VG_1 and which will
# transition together on any state change.
- vrrp_instance:
  - name: 'inside_network'
# Initial state - 'MASTER' or 'BACKUP'. As soon as the other machine(s) come up,
# an election will be held and the machine with the highest priority will become
# MASTER. So the entry here doesn't matter a whole lot.
    state: 'MASTER'
# Interface for inside_network, bound by vrrp.
    interface: 'eth0'
# Use VRRP Virtual MAC.
    use_vmac: 'VMAC_INTERFACE'
# Send/Recv VRRP messages from base interface instead of VMAC interface.
    vmac_xmit_base: 'false'
# Force instance to use IPv6 (when mixed IPv4 and IPv6 config).
    native_ipv6: 'false'
# Ignore VRRP interface faults (default is 'false').
    dont_track_primary: 'false'
# Optional, monitor these as well. To to FAULT state if any of these go down.
    track_interface:
    - interface: 'eth0'
    - interface: 'eth1'
    - interface: 'eth2'
# From '-254' to '254'.
      weight: '100'
# Add a tracking script to the interface (<SCRIPT_NAME> is the name of the
# 'vrrp_script' entry).
    track_script:
# Is the name of the 'vrrp_script' entry.
    - name: 'script_name'
    - name: '2'
# From '-254' to '254'.
      weight: '100'
# Default IP for binding vrrpd is the primary IP on interface. If you want to
# hide the location of vrrpd, use this IP as src_addr for multicast or unicast
# vrrp packets. Since it's multicast, vrrpd will get the reply packet no matter
# what src_addr is used.
    mcast_src_ip: 'IPADDR'
    unicast_src_ip: 'IPADDR'
# VRRP version to run on interface. Default is global parameter 'vrrp_version'.
    version: '2' # '2' or '3'
# Do not send VRRP adverts over a VRRP multicast group. Instead it sends
# adverts to the following list of ip addresses using unicast. It can be cool
# to use the VRRP FSM and features in a networking environment where multicast
# is not supported! # IP addresses specified can be IPv4 as well as IPv6.
    unicast_peer:
    - 'IPADDR'
# Interface specific settings, same as global parameters. Default to global
# parameters.
    garp_master_delay: '10'
    garp_master_repeat: '1'
    garp_lower_prio_delay: '10'
    garp_lower_prio_repeat: '1'
    garp_master_refresh: '60'
    garp_master_refresh_repeat: '2'
    garp_interval: '100'
    gna_interval: '100'
    lower_prio_no_advert: 'false'
    higher_prio_send_advert: 'false'
# Arbitrary unique number from 0 to 255. Used to differentiate multiple
# instances of vrrpd running on the same NIC (and hence same socket).
    virtual_router_id: '51'
# For electing MASTER, highest priority wins. To be MASTER, make this 50 more
# than on other machines.
    priority: '100'
# VRRP Advert interval in seconds (e.g. 0.92) (use default).
    advert_int: '1'
# Authentication was removed from the VRRPv2 specification by RFC3768 in 2004.
# Use of this option is non-compliant and can cause problems; avoid using if
# possible, except when using unicast, where it can be helpful.
    authentication:
# PASS - Simple password
# AH - IPSEC
    - type: 'PASS'
# Password for accessing vrrpd. Should be the same on all machines. Only the
# first eight (8) characters are used.
      password: '1234'
# Addresses add|del on change to MASTER, to BACKUP. With the same entries on
# other machines, the opposite transition will be occurring.
    virtual_ipaddress:
    - '192.168.200.17/24 dev eth1'
    - '192.168.200.18/24 dev eth2 label eth2:1'
# VRRP IP excluded from VRRP. For cases with large numbers (e.g. 200) of IPs on
# the same interface. To decrease the number of packets sent in adverts, you
# can exclude most IPs from adverts. The IPs are add|del as for
# 'virtual_ipaddress'. Can also be used if you want to be able to add a mixture
# of IPv4 and IPv6 addresses, since all addresses in 'virtual_ipaddress' must be
# of the same family.
    virtual_ipaddress_excluded:
    - '<IPADDR>/<MASK> brd <IPADDR> dev <STRING> scope <SCOPE>'
    - '<IPADDR>/<MASK> brd <IPADDR> dev <STRING> scope <SCOPE>'
# Set the 'promote_secondaries' flag on the interface to stop other addresses in
# the same CIDR being removed when 1 of them is removed. For example if
# '10.1.1.2/24' and '10.1.1.3/24' are both configured on an interface, and one
# is removed, unless 'promote_secondaries' is set on the interface the other
# address will also be removed.
    prompte_secondaries: 'false'
# Routes add|del when changing to MASTER, to BACKUP. See 'static_routes' for
# more details.
    virtual_routes:
    - 'src 192.168.100.1 to 192.168.109.0/24 via 192.168.200.254 dev eth1'
    - '192.168.110.0/24 via 192.168.200.254 dev eth1'
    - '192.168.111.0/24 dev eth2'
    - '192.168.112.0/24 via 192.168.100.254'
    - '192.168.113.0/24 via 192.168.200.254 or 192.168.100.254 dev eth1'
    - 'blackhole 192.168.114.0/24'
    - '0.0.0.0/0 gw 192.168.0.1 table 100' # To set a default gateway into table 100
# Rules add|del when changing to MASTER, to BACKUP. See 'static_rules' for more
# details.
    virtual_rules:
    - 'from 192.168.2.0/24 table 1'
    - 'to 192.168.2.0/24 table 1'
# VRRPv3 has an Accept Mode to allow the virtual router when not the address
# owner to receive packets addressed to a VIP. This is the default setting
# unless strict mode is set. As an extension, this also works for VRRPv2
# (RFC 3768 doesn't define an accept mode).
# Accept packets to non address-owner.
    accept: 'false'
# Drop packets to non address-owner.
    no_accept: 'false'
# VRRP will normally preempt a lower priority machine when a higher priority
# machine comes online. 'nopreempt' allows the lower priority machine to
# maintain the master role, even when a higher priority machine comes back
# online. For this to work, the initial state of this entry must be BACKUP.
    nopreempt: 'false'
# See description of global 'vrrp_skip_check_adv_addr', which sets the default
# value. Defaults to 'vrrp_skip_check_adv_addr'. Default is 'true'.
    skip_check_adv_addr: 'true'
# See description of global 'vrrp_strict'. If 'vrrp_strict' is not specified, it
# takes the value of 'vrrp_strict'. If 'strict_mode' without a parameter is
# specified, it defaults to 'true'.
    strict_mode: 'false'
# Seconds after startup or seeing a lower priority master until preemption (if
# not disabled by "nopreempt"). For this to work, the initial state of this
# entry must be BACKUP. Range: 0 (default) to 1000.
    preempt_delay: '0'
# Debug level is a number in the range 0 to 4.
    debug: '0'
# Notify scripts, alert as above.
    notify_master:
    - exec: '/path/notify_master.sh'
      user: 'username'
      group: 'groupname'
    notify_backup:
    - exec: '/path/notify_backup.sh'
      user: 'username'
      group: 'groupname'
    notify_fault:
    - exec: '/path/notify_fault.sh'
      user: 'username'
      group: 'groupname'
# Executed when stopping vrrp.
    notify_stop:
    - exec: '/path/notify_stop.sh'
      user: 'username'
      group: 'groupname'
    notify:
    - exec: '/path/notify_stop.sh'
      user: 'username'
      group: 'groupname'
    smtp_alert: 'true'
  - name: 'VI_1'
    state: 'MASTER'
    interface: 'eth0'
    virtual_router_id: '51'
    priority: '100'
    virtual_ipaddress:
    - '192.168.200.18/25'
    track_interface:
    - interface: 'eth1'
      weight: '2'
    - interface: 'eth2'
      weight: '-2'
    - interface: 'eth3'
    track_script:
    - name: 'check_sshd' # use default weight from the script
    - name: 'check_haproxy'
      weight: '2' # +2 if process is present
    - name: 'check_http_port'
    - name: 'check_https_port'
    - name: 'check_smtp_port'
  - name: 'VI_2'
    state: 'MASTER'
    interface: 'eth1'
    virtual_router_id: '51'
    priority: '100'
    virtual_ipaddress:
    - '192.168.201.18/26'
    track_interface:
    - interface: 'eth0'
      weight: '2' # prio = +2 if UP
    - interface: 'eth2'
      weight: '-2' # prio = -2 if DOWN
    - interface: 'eth3' # no weight, fault if down
    track_script:
    - name: 'check_haproxy'
      weight: '2'
    - name: 'check_http_port'
    - name: 'check_https_port'
    - name: 'check_smtp_port'
  - name: 'VI_3'
    interface: 'eth0'
    virtual_router_id: '53'
    priority: '100'
    virtual_ipaddress:
    - '192.168.200.19/27'
  - name: 'VI_4'
    interface: 'eth1'
    virtual_router_id: '54'
    priority: '100'
    virtual_ipaddress:
    - '192.168.201.19/28'
  - name: 'VI_5'
    state: 'MASTER'
    interface: 'eth0'
    virtual_router_id: '55'
    priority: '100'
    virtual_ipaddress:
    - '192.168.200.20/27'
  - name: 'VI_6'
    state: 'MASTER'
    interface: 'eth0'
    virtual_router_id: '56'
    priority: '100'
    virtual_ipaddress:
    - '192.168.200.21/27'
  - name: 'VI_7'
    state: 'MASTER'
    interface: 'eth0'
    virtual_router_id: '57'
    priority: '100'
    virtual_ipaddress:
    - '192.168.200.22/27'
  - name: 'VI_8'
    state: 'MASTER'
    interface: 'eth0'
    virtual_router_id: '58'
    priority: '100'
    virtual_ipaddress:
    - '192.168.200.23/27'
  - name: 'VI_9'
    state: 'MASTER'
    interface: 'eth0'
    virtual_router_id: '59'
    priority: '100'
    virtual_ipaddress:
    - '192.168.200.24/27'
# Parameters used for SSL_GET check. If none of the parameters are specified,
# the SSL context will be auto generated.
- ssl:
  - password: 'password'
    ca: '/path/to_ca_file'
    certificate: '/path/to_certificate_file'
    key: '/path/to_key_file'
# Virtual server groups. This groups allows a service on a real_server to belong
# to multiple virtual services and to only be health checked once. Only for
# very large LVSs.
- virtual_server_group:
  - name: 'VSG_1'
# VIP port
# <IPADDR RANGE> has the form XXX.YYY.ZZZ.WWW-VVV eg 192.168.200.1-10, range
# includes both .1 and .10 address.
    data:
    - '10.0.0.1 8080'
    - '10.0.0.2 80'
    - '192.168.200.1 1358'
    - '192.168.200.3-10 80'
    - 'fwmark 1'
    - 'fwmark 2'
# Virtual servers.
- virtual_server:
  - instance: 'VSG_1' # 'virtual_server_group' or data from 'virtual_server_group'
# Delay timer for service polling.
    delay_loop: '6'
# LVS scheduler
# rr - Round-Robin
# wrr - Weighted Round-Robin
# lc - Least-Connection
# wlc - Weighted Least-Connection
# lblc - Locality-Based Least-Connection
# dh - Destination Hash
# sh - Source Hash
    lb_algo: 'rr'
# Enable hashed entry.
    hashed: 'false'
# Enable flag-1 for scheduler (-b flag-1 in ipvsadm).
    flag_1: 'false'
# Enable flag-2 for scheduler (-b flag-2 in ipvsadm).
    flag_2: 'false'
# Enable flag-3 for scheduler (-b flag-3 in ipvsadm).
    flag_3: 'false'
# Enable sh-port for sh scheduler (-b sh-port in ipvsadm).
    sh_port: 'false'
# Enable sh-fallback for sh scheduler (-b sh-fallback in ipvsadm).
    sh_fallback: 'false'
# Enable One-Packet-Scheduling for UDP (-O in ipvsadm).
    ops: 'false'
# LVS forwarding method ('NAT', 'DR' or 'TUN').
    lb_kind: 'NAT'
# LVS persistence engine name.
    persistence_engine: 'lvs_name'
# LVS persistence timeout in seconds, default 6 minutes.
    persistence_timeout: '360'
# LVS granularity mask (-M in ipvsadm).
    persistence_granularity: '255.255.255.255'
# L4 protocol ('TCP', 'UDP' or 'SCTP').
    protocol: 'TCP'
# If VS IP address is not set, suspend healthchecker's activity.
    ha_suspend: 'false'
# VirtualHost string for HTTP_GET or SSL_GET.
    virtualhost: 'example.com'
# On daemon startup assume that all RSs are down and healthchecks failed. This
# helps to prevent false positives on startup. Alpha mode is disabled by
# default.
    alpha: 'false'
# On daemon shutdown consider quorum and RS down notifiers for execution, where
# appropriate. Omega mode is disabled by default.
    omega: 'false'
# Minimum total weight of all live servers in the pool necessary to operate VS
# with nob quality regression. Defaults to 1.
    quorum: '1'
# Tolerate this much weight units compared to the nominal quorum, when
# considering quorum gain or loss. A flap dampener. Defaults to 0.
    hysteresis: '0'
# Script to execute when quorum is gained.
    quorum_up: '/path/quorum_up.sh'
# Script to execute when quorum is lost.
    quorum_down: '/path/quorum_down.sh'
# IP family for a fwmark service ('inet' or 'inet6').
    ip_family: 'inet'
# RS to add when all realservers are down.
    sorry_server:
    - address: '192.168.100.100'
      port: '80'
# Applies 'inhibit_on_failure' behaviour to the preceding 'sorry_server'
# directive.
    sorry_server_inhibit: 'false'
# Setup real servers:
    real_server:
    - address: '192.168.100.2'
      port: '80'
# Relative weight to use, default: '1'.
      weight: '2'
# Set weight to 0 when healthchecker detects failure.
      inhibit_on_failure: 'false'
# Script to execute when healthchecker. Considers service as up.
      notify_up: '/path/notify_up.sh'
# Script to execute when healthchecker considers service as down.
      notify_down: '/path/notify_down.sh'
# Maximum number of connections to server.
      uthreshold: '2'
# Minimum number of connections to server.
      lthreshold: '1'
# Pick one healthchecker from 'HTTP_GET', 'SSL_GET', 'TCP_CHECK', 'SMTP_CHECK',
# 'DNS_CHECK', 'MISC_CHECK'.
      checks:
# HTTP and SSL healthcheckers.
      - check_method: 'HTTP_GET' # or 'SSL_GET'
# Number of get retries.
        nb_get_retry: '3'
# Delay before retry.
        delay_before_retry: '2'
# Optional random delay to start the initial check for maximum N seconds. Useful
# to scatter multiple simultaneous checks to the same RS. Enabled by default,
# with the maximum at 'delay_loop'. Specify '0' to disable.
        warmup: ''
# Optional IP address to connect to. The default is the realserver IP.
        connect_ip: 'ipaddr'
# Optional port to connect to. The default is the realserver port.
        connect_port: '8080'
# Optional interface to use to originate the connection.
        bindto: 'ipaddr'
# Optional source port to originate the connection from.
        bind_port: '80'
# Optional connection timeout in seconds. The default is 5 seconds.
        connect_timeout: '5'
# Optional fwmark to mark all outgoing checker packets with.
        fwmark: ''
# An list of url's to test.
        urls:
# path: '/' or path: '/mrtg2/'.
        - path: '/testurl/test.jsp'
# Digest computed with genhash.
          digest: '9b3a0c85a887a256d6939da88aabd8cd'
# Status code returned in the HTTP header. Default is any 2xx value.
          status_code: ''
        - path: '/testurl2/test.jsp'
          digest: '640205b7b0fc66c1ea91c463fac6334c'
# TCP healthchecker.
      - check_method: 'TCP_CHECK'
# Optional random delay to start the initial check for maximum N seconds. Useful
# to scatter multiple simultaneous checks to the same RS. Enabled by default,
# with the maximum at delay_loop. Specify 0 to disable.
        warmup: ''
# Retry count to make additional checks if check of an alive server fails.
# Default is '1'.
        retry: '1'
# Delay in seconds before retrying. Default is '1'.
        delay_before_retry: '1'
# Optional IP address to connect to. The default is the realserver IP.
        connect_ip: 'ipaddr'
# Optional port to connect to. The default is the realserver port.
        connect_port: '8080'
# Optional interface to use to originate the connection.
        bindto: 'ipaddr'
# Optional source port to originate the connection from.
        bind_port: '80'
# Optional connection timeout in seconds. The default is 5 seconds.
        connect_timeout: '5'
# Optional fwmark to mark all outgoing checker packets with.
        fwmark: ''
# An list of url's to test.
      - check_method: 'SMTP_CHECK'
# Optional random delay to start the initial check for maximum N seconds. Useful
# to scatter multiple simultaneous checks to the same RS. Enabled by default,
# with the maximum at delay_loop. Specify 0 to disable.
        warmup: ''
# Retry count to make additional checks if check of an alive server fails.
# Default is '1'.
        retry: '1'
# Delay in seconds before retrying. Default is '1'.
        delay_before_retry: '1'
# Optional string to use for the SMTP HELO request.
        helo_name: 'helo.example.com'
# Optional IP address to connect to. The default is the realserver IP.
        connect_ip: 'ipaddr'
# Optional port to connect to. The default is the realserver port.
        connect_port: '8080'
# Optional interface to use to originate the connection.
        bindto: 'ipaddr'
# Optional source port to originate the connection from.
        bind_port: '80'
# Optional connection timeout in seconds. The default is 5 seconds.
        connect_timeout: '5'
# Optional fwmark to mark all outgoing checker packets with.
        fwmark: ''
# An list of hosts to test.
        hosts:
# Optional IP address to connect to. The default is the realserver IP.
        - connect_ip: '172.16.1.12'
# Optional port to connect to. The default is the realserver port.
          connect_port: '25'
# Optional interface to use to originate the connection.
          bindto: '172.16.1.2'
# Optional source port to originate the connection from.
          bind_port: ''
# Optional connection timeout in seconds. The default is 5 seconds.
          connect_timeout: '10'
# Optional fwmark to mark all outgoing checker packets with.
          fwmark: ''
        - connect_ip: '172.16.1.12'
          bindto: '172.16.1.2'
          connect_port: '25'
        - connect_ip: '64.233.167.100'
          connect_port: '587'
# DNS healthchecker.
      - check_method: 'DNS_CHECK'
# DNS query type (A, NS, CNAME, SOA, MX, TXT, AAAA). The default is SOA.
        query_type: 'SOA'
# Domain name to use for the DNS query. The default is . (dot).
        name: 'google.com'
# Number of times to retry a failed check. The default is '3' times.
        retry: '3'
# Optional IP address to connect to. The default is the realserver IP.
        connect_ip: 'ipaddr'
# Optional port to connect to. The default is the realserver port.
        connect_port: '8080'
# Optional interface to use to originate the connection.
        bindto: 'ipaddr'
# Optional source port to originate the connection from.
        bind_port: '80'
# Optional connection timeout in seconds. The default is 5 seconds.
        connect_timeout: '5'
# Optional fwmark to mark all outgoing checker packets with.
        fwmark: ''
# MISC healthchecker, run a program.
      - check_method: 'MISC_CHECK'
# External script or program.
        misc_path: '/path/to_script.sh'
# Script execution timeout.
        misc_timeout: '10'
# Optional random delay to start the initial check for maximum N seconds. Useful
# to scatter multiple simultaneous checks to the same RS. Enabled by default,
# with the maximum at delay_loop. Specify 0 to disable.
        warmup: ''
# If set, the exit code from healthchecker is used to dynamically adjust the
# weight as follows:
# exit status 0: svc check success, weight unchanged.
# exit status 1: svc check failed.
# exit status 2-255: svc check success, weight changed to 2 less than exit
# status. For example: exit status of 255 would set weight to 253.
        misc_dynamic: ''
# Specify the username/groupname that the script should be run under. If
# 'group' is not specified, the group of the user is used.
        misc_user:
        - username: 'automation'
          group: 'robots'
```
