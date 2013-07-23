##adsuck experiment

This is a repository where I keep my hosts file and filter file for adsuck.
I intend to block ton of hosts and unblock only if I see the necessity.
Why would someone want to load third party ads and loose time that should be spent to load the page.

####Adsuck

[Adsuck](https://opensource.conformal.com/wiki/Adsuck)  is a small DNS server that spoofs blacklisted addresses and forwards all other queries. The idea is to be able to prevent connections to undesirable sites such as ad servers, crawlers, etc. It can be used locally, for the road warrior, or on the network perimeter in order to protect local machines from malicious sites.
adsuck replies to bad addresses with a spoofed DNS packet that has the NXdomain
flag set.  This in effect prevents the application that is resolving the address
from trying to connect to this address.  Addresses that are not matched are forwarded to the normal nameserver, as provided by resolv.conf(5).

Note that when applications try to be smart and resolve an address with the local
domain name appended, it will still spoof the answer.

All non-spoofed responses are cached for the duration of the provided DNS TTL
(Time To Live).  The cache will be purged when adsuck receives a HUP or USR1 signal.  See the SIGNALS section for more details.

Adsuck is written in C, is lightweight, and extremely fast (faster than the hosts file and faster than a web proxy).
NOTE: Adsuck doesn't cache the response longer than the TTL. If you want to have something that keeps cache internet addresses you should take a look at dnsmask.

####Setting adsuck
* create a chroot directory for adsuck with 755 permissions and owner root (e.g. /var/adsuck) 
* Create a _adsuck user and make its home directory the chroot directory so the program can acess the files in it.
* Create a _adsuck group so you can also edit the files in it.
* create 3 files in the /var/adsuck, one called filter, one called hosts, and one called resolv.conf
* move what was in /etc/resolv.conf to /var/adsuck/resolv.conf
* add the regex to block to /var/adsuck/filter
* add the domains to block to /var/adsuck/hosts
* modify /etc/resolv.conf to nameserver 127.0.0.1
* add prepend domain-name-servers 127.0.0.1; to dhclient.conf
* /usr/bin/adsuck -D -c /var/adsuck -f /resolv.conf /hosts -r filter this should be started as a daemon.
* If you use a Firefox variant edit this config:browser.xul.error_pages.enabled and turn it to false to not show the annoying part of the page that didn't load.


####The Filter File
The filter file contains Regex. If those regex matches the domain to resolve then the domain gets blocked.
Do not leave an empty line in this file otherwise all the websites will get filtered.

####The hosts File
This contains hosts to block.
You can get hosts file from :
* http://www.mvps.org/winhelp2002/hosts.txt
* http://rlwpx.free.fr/WPFF/hosts.htm
* http://pgl.yoyo.org/adservers/serverlist.php?showintro=0;hostformat=hosts
* http://someonewhocares.org/hosts/ (funny one)
* Or directly from this repo.

####The testing idea
I intend to block a huge list of fishy websites and unblock bit by bit what I really need.
Contributions are highly welcomed.
If you want to contribute you can check what is loaded in a page using the NET tool of firebug.
