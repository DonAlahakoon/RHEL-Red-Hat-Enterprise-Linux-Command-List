# RHEL-Red-Hat-Enterprise-Linux-Command-List
<p align="center"><img src="https://www.redhat.com/rhdc/managed-files/poster_new-brand-logo-types.png" width=300/></p>

## Summary
- A Linux distribution developed and maintained by Red Hat which is now part of IBM.
- RHEL is designed primarily for enterprise environment.
- Features
    - Long term stability
    - Enterprise grade security
    - Professional technical support
    - Long term maintenance
    - Hardware and software compatibility
- Common use cases include Enterprise servers, financial institution, Datacenters etc.
- Why RHEL over Ubuntu? **Mainly because of its enterprise support, predictable lifecycle, certification ecosystem security features and centralized management capabilities**

## Specs
- Package format  -> **RPM** _Red Hat Package Manager_ (Both RHEL and Fedora uses same where as Debian and Ubuntu uses DEB)
- Package Manager -> **DNF** _Dandified YUM_ (Both RHEL and Fedora uses same where as Debian and Ubuntu uses APT)
- Security framework -> **SE Linux** (Both RHEL and Fedora where as Debian and Ubuntu uses AppArmor)
- Service manager -> **systemd**
- Container Tooling -> **Podman**

## Some Important Commands
- `rpm -qa` List installed packages
- `rpm -qi nginx` Install package nginx
- `rpm -ql nginx` List files installed by a package
- `rpm -q nginx` Check whether a package is installed
- `rpm -e nginx` Remove nginx package
- `rpm -ivh package.rpm` Install an RPM package
- `rpm -Uvh package.rpm` Update or install a package

