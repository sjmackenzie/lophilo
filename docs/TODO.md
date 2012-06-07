Security

* add default lophilo user to run services (such as node) and remote login
* disable remote root login but permit password-less login on the serial console

Services

disable NFS, cron, until we really need it

nullok_secure

# useradd lophilo
# strace userdel lophilo

gettimeofday({1337837999, 388}, NULL)   = 0
send(8, "<86>May 24 05:39:59 userdel[1550"..., 71, MSG_NOSIGNAL) = 71
gettimeofday({1337837999, 3527}, NULL)  = 0
--- SIGSEGV (Segmentation fault) @ 0 (0) ---
+++ killed by SIGSEGV +++
Segmentation fault

