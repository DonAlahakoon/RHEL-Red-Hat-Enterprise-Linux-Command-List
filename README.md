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

>[!Important]
> RPM itself doesn't automatically resolve all package dependencies in the way DNF does.
> You might have to resolve package dependencies manually when using low level RPM commands.
> That is where DNF comes in.

## RHEL Package Manager
- High level package manager used in modern RHEL versions.
- It handles
      - Package installations
      - Package removal
      - Dependency resolution
      - Repository management
      - Package updates
## Important Commands
- `sudo dnf install nginx` Install software
- `sudo dnf remove nginx` Removes software
- `sudo dnf update` Updates installed packages
- `sudo dnf search nginx` Search for a package
- `sudo dnf info nginx` Get package information
- `sudo dnf list installed` List installed packages
- `sudo dnf repolist` List available repositories
- `sudo dnf clean all` Clear cached metadata
- `sudo dnf repoquery --requires nginx` Show package dependencies

## YUM vs DNF
- RHEL used YUM _(yellowdog updater modified)_
>[!Note]
> - RHEL 7 : YUM <br>
> - RHEL 8,9,10 : DNF<br>
> - You might still see YUM commands in modern RHEL environment because YUM is retained as a compatibility interface to DNF.

## What are repositories?
- A repository is where software packages are stored.
- In RHEL two important repository concepts are **BaseOS** and **AppStream**

## 5. RHEL Subscription Management
- This is where RHEL different from other Linux distros
- **If Linux is open source why need to pay for RHEL?** _The answer to that is you pay not for the linux kernel but for the enterprise ecosystem sorrounding the distribution._
- Depending on the subscription it includes Enterprise Technical Support
        - Access to supported software repositories
        - Security Updates
        - Bug fixes
        - Enterprise technical support
        - Lifecycle support
>[!Tip]
> - RHEL provides a utility called a `subscription manager`<br>
> - Check subscription status `sudo subscription-manager status`<br>
> - Register a system `sudo subscription-manager register`<br>
> - Unregister `sudo subscription-manager unregister`<br>
> - Check available repositories `sudo subscription-manager repos --list`<br>
> - Enable a repository `sudo subscription-manager repos --enable=<repository_id>` <br>

### Red Hat Insights
- Red Hat insights is a service that help organizations analyze their RHEL systems, identify potential security and configuration risks and plan maintenance activities.

## 6. SELinux One of the Most Important RHEL topics
- SELinux stands for **security enhanced Linux**.
- It is a security mechanism that provides MAC (mandatory access control)
- Suppose you have a file `-rw-r--r-- 1 root root 1024 app.conf`. Traditional Linux permissions determine who can access this file.
- They consider
      - User Ownership
      - Group Ownership
      - Read Permissions
      - Write Permissions
      - Execute Permissions
- What happens if an application becomes compromised.
- An application might be running under a user that has access to resources that shouldn't normally access.
- SELinux provides an **additional security layer**. It controls what processes can access even when traditional Linux permissions would otherwise allow the operation.
>[!Important]
> - **Traditional Linux Permissions = Discretionary Access Control (DAC)**<br>
> - **SELinux = Mandatory Access Control (MAC)**<br>
> - SELinux adds another layer of security beyond traditional file permissions.<br>

### SELinux Modes
- Enforcing - Policies are actively enforced
- Permissive - Policies are not enforced
- Disabled - SELinux is disabled
        - Check the current mode : `getenforce` Example: `Enforcing`
        - To change to permissive : `sudo setenforce 0`
        - Change to enforcing: `sudo setenforce 1`
NB: Note that these runtime changes don't persist after reboot.
- To configure the persistent mode, you generally modify: `/etc/selinux/config` Example `SELINUX=enforcing`
#### Important SELinux Commands
- Check SELinux status: `sestatus`
- Check file security contexts: `ls -Z`
- Check process security contexts: `ps -eZ`
- Restore default file contexts : `sudo restorecon -Rv /var/www/html`
- Change file context temporarily : `sudo chcon -t httpd_sys_content_t /var/www/html/index.html`
- View SELinux related audit messages : `sudo ausearch -m AVC -ts recent`    
#### A practical DevOps scenario
- Imagine you deploye Nginx application. You configure `/opt/myapp/index.html` you give the Nginx process read permissions, you restart Nginx. However the application returns a permission-denied error.
- You check `ls -l /opt/myapp` everything looks correct. What could be wrong?
- SELinux! The directory might have an incorrect security context. You can investigate using `ls -Z /opt/myapp`. Then inspect SELinux denial logs.
>[!Caution]
> A common mistake is immediately disabling SELinux when application encounters permission errors.

## 7. Firewalld - Firewall management in RHEL
- `Firewalld` provides dynamic firewall management.
- Allow administrators to configure firewall rules without manually managing every underlying firewall rule.
#### Commands
- Check firewall status `sudo systemctl system firewalld`
- Start firewall `sudo systemctl start firewalld`
- Enable firewall at boot `sudo systemctl enable firewalld`
- Check active zones `sudo firewall-cmd --get-active-zones`
- List allowed services `sudo firewall-cmd --list-services`
- List allowed ports `sudo firewall --list-ports`
- Allow http traffic `sudo firewall-cmd --permanent --add-service=http`
- Allow https `sudo firewall-cmd --permanent --add-service=https`
- Allow a specific port `sudo firewall-cmd --permanent --add-port=8080/tcp`
- Reload firewall `sudo firewall-cmd --reload`
- Remove port `sudo firewall-cmd --permanent --remove-port=8080/tcp`
#### Practical Scenario
- Suppose you deploy a Spring Boot application on port 8080. The application is running. `sudo systemctl status myapp` But you can't access it remotely. What do you investigate?
        1. Is the application listening on port 8080.
        2. Is the Linux firewall allowing port 8080.
        3. Is the cloud security group allowing port 8080.
        4. Is the network route correct?
        5. Is the application binding to the correct interface?
NB: Remember opening an AWS security group port doesn't automatically open the same port in the operating system firewall.

## 8. Systemd and service management
- RHEL uses systemd as its service and system initializing manager.
- It manages system services and other units.
- For example
    - Ngnix
    - SSH
    - Docker
    - Podman-related services
    - Database services
#### Commands
- Check service status: `sudo systemctl status nginx`
- Start a service : `sudo systemctl start nginx`
- Stop a service : `sudo systemctl stop nginx`
- Restart : `sudo systemctl restart nginx`
- Reload configuration : `sudo systemctl reload nginx`
- Enable at startup : `sudo systemctl enable nginx`
- Disable at startup : `sudo systemctl disable nginx`
- Enable and start simultaneously : `sudo systemctl enable --now nginx`
- List running services : `systemctl list-units --type=service --state=running`
- 
