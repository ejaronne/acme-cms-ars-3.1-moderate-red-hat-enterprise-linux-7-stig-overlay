# acme-cms-ars-3.1-moderate-red-hat-enterprise-linux-7-stig-overlay
ACME-tailored InSpec profile overlay to validate the secure configuration of Red Hat Enterprise Linux 7 against [DISA's](https://iase.disa.mil/stigs/Pages/index.aspx) Red Hat Enterprise Linux 7 STIG Version 2 Release 6 tailored for [CMS ARS 3.1](https://www.cms.gov/Research-Statistics-Data-and-Systems/CMS-Information-Technology/InformationSecurity/Info-Security-Library-Items/ARS-31-Publication.html) for CMS systems categorized as Moderate.

## Getting Started  
It is intended and recommended that InSpec and this profile overlay be run from a __"runner"__ host (such as a DevOps orchestration server, an administrative management system, or a developer's workstation/laptop) against the target remotely over __ssh__.

__For the best security of the runner, always install on the runner the _latest version_ of InSpec and supporting Ruby language components.__ 

Latest versions and installation options are available at the [InSpec](http://inspec.io/) site.

## Tailoring to Your Environment

The following inputs may be configured in an inputs ".yml" file for the profile to run correctly for your specific environment. More information about InSpec inputs can be found in the [InSpec Profile Documentation](https://www.inspec.io/docs/reference/profiles/).

```
# Used by InSpec checks V-71849, V-71855, V-72037
# InSpec Tests that are known to consistently have long run times (V-71849, V-71855, V-72037) can be disabled with this attribute
# Acceptable values: false, true
# (default: false)
disable_slow_controls: 

# Used by InSpec check V-71849
# list of system files that should be allowed to change from an rpm verify point of view
rpm_verify_perms_except: []

# Used by InSpec check V-71855
# list of system files that should be allowed to change from an rpm verify point of view
rpm_verify_integrity_except: []

# Used by InSpec check V-72211 (default: false)
# Do NOT set to 'true' UNLESS the server is documented as being used as a log aggregation server. 
log_aggregation_server: 

# Used by InSpec check V-72047 (default: [])
# Known application groups that are allowed to have world-writeable files or directories
application_groups: []

# Used by InSpec check V-72307 (default: false)
# Do NOT set to 'true' UNLESS use of X Windows System is documented and approved. 
x11_enabled: 

# Accounts of known managed users (Array)
user_accounts: []

# System accounts that support approved system activities. (Array) (defaults shown below)
known_system_accounts:
  [
    "root",
    "bin",
    "daemon",
    "adm",
    "lp",
    "sync",
    "shutdown",
    "halt",
    "mail",
    "operator",
    "nobody",
    "systemd-bus-proxy",
  ]

# V-71965, V-72417, V-72433
# (enabled or disabled)
smart_card_status: "enabled"

# V-72051/V-72209
# The path to the logging package
log_pkg_path: "/etc/rsyslog.conf"

# V-72011, V-72015, V-72017, V-72019, V-72021, V-72023, V-72025
# V-72027, V-72029, V-72031, V-72033, V-72035, V-72037, V-72059
# Users exempt from home directory-based controls in array
# format
exempt_home_users: []

# V-71961
# main grub boot config file
grub_main_cfg: "/boot/grub2/grub.cfg"

# superusers for grub boot ( array )
grub_superusers: ["root"]

# grub boot config files
grub_user_boot_files: ["/boot/grub2/user.cfg"]

# V-71963
# superusers for efi boot ( array )
efi_superusers: ["root"]

# efi boot config files
efi_user_boot_files: ["/boot/efi/EFI/redhat/user.cfg"]

# main efi boot config file
efi_main_cfg: "/boot/efi/EFI/redhat/grub.cfg"

# V-71971
# system accounts that support approved system activities
admin_logins: []

# V-77819
# should dconf have smart card authentication
multifactor_enabled: "true"

# V-72317
# approved configured tunnels prepended with word 'conn'
# Example: ['conn myTunnel']
approved_tunnels: []

# V-72039
# Is the target expected to be a virtual machine
virtual_machine: false

```
## Long Running Controls

There are a few long running controls that take anywhere from 3 minutes to 10 minutes or more to run. In an ongoing or CI/CD pipelne this may not be ideal. We have supplied an 
input (mentioned above in the user-defined inputs) in the profile to allow you to 'skip' these controls to account for these situations.

The input `disable_slow_controls (bool: false)` can be set to `true` or `false` as needed in a <name_of_your_input_file>.yml file.

* `V-71849` (~3 minutes)
* `V-71855` (~3 minutes)
* `V-72037` (10+ minutes)

## Running This Overlay Directly from Github

Against a remote target using ssh with escalated privileges (i.e., InSpec installed on a separate runner host)
```bash
# How to run 
inspec exec https://github.com/ejaronne/acme-cms-ars-3.1-moderate-red-hat-enterprise-linux-7-stig-overlay/archive/main.tar.gz -t ssh://TARGET_USERNAME:TARGET_PASSWORD@TARGET_IP:TARGET_PORT --sudo --sudo-password=<SUDO_PASSWORD_IF_REQUIRED> --input-file <path_to_your_input_file/name_of_your_input_file.yml> --reporter=cli json:<path_to_your_output_file/name_of_your_output_file.json> 
```

Against a remote target using a pem key with escalated privileges (i.e., InSpec installed on a separate runner host)
```bash
# How to run 
inspec exec https://github.com/ejaronne/acme-cms-ars-3.1-moderate-red-hat-enterprise-linux-7-stig-overlay/archive/main.tar.gz -t ssh://TARGET_USERNAME@TARGET_IP:TARGET_PORT --sudo -i <your_PEM_KEY> --input-file <path_to_your_input_file/name_of_your_input_file.yml> --reporter=cli json:<path_to_your_output_file/name_of_your_output_file.json>  
```

Against a local Red Hat host with escalated privileges (i.e., InSpec installed on the target)
```bash
# How to run
sudo inspec exec https://github.com/ejaronne/acme-cms-ars-3.1-moderate-red-hat-enterprise-linux-7-stig-overlay/archive/main.tar.gz --input-file <path_to_your_input_file/name_of_your_input_file.yml> --reporter=cli json:<path_to_your_output_file/name_of_your_output_file.json> 
```
### Different Run Options

  [Full exec options](https://docs.chef.io/inspec/cli/#options-3)

## Running This Overlay from a local Archive copy
If your runner is not always expected to have direct access to GitHub, use the following steps to create an archive bundle of this overlay and all of its dependent tests:

(Git is required to clone the InSpec profile using the instructions below. Git can be downloaded from the [Git](https://git-scm.com/book/en/v2/Getting-Started-Installing-Git) site.) 

```
mkdir profiles
cd profiles
git clone https://github.com/ejaronne/acme-cms-ars-3.1-moderate-red-hat-enterprise-linux-7-stig-overlay.git
inspec archive acme-cms-ars-3.1-moderate-red-hat-enterprise-linux-7-stig-overlay
sudo inspec exec <name of generated archive> --input-file <path_to_your_input_file/name_of_your_input_file.yml> --reporter=cli json:<path_to_your_output_file/name_of_your_output_file.json> 
```

For every successive run, follow these steps to always have the latest version of this overlay and dependent profiles:

```
cd acme-cms-ars-3.1-moderate-red-hat-enterprise-linux-7-stig-overlay
git pull
cd ..
inspec archive acme-cms-ars-3.1-moderate-red-hat-enterprise-linux-7-stig-overlay --overwrite
sudo inspec exec <name of generated archive> --input-file <path_to_your_input_file/name_of_your_input_file.yml> --reporter=cli json:<path_to_your_output_file/name_of_your_output_file.json> 
```

## Using Heimdall for Viewing the JSON Results

The JSON results output file can be loaded into __[heimdall-lite](https://heimdall-lite.mitre.org/)__ for a user-interactive, graphical view of the InSpec results. 

The JSON InSpec results file may also be loaded into a __[full heimdall server](https://github.com/mitre/heimdall)__, allowing for additional functionality such as to store and compare multiple profile runs.

## Authors


## Special Thanks
* The SIMP Project Team
* Aaron Lippold
* Sam Cornwell

## Contributing and Getting Help
To report a bug or feature request, please open an [issue](https://github.com/ejaronne/cms-ars-3.1-moderate-red-hat-enterprise-linux-7-stig-overlay/issues/new).

## License
This is licensed under the [Apache 2.0](https://www.apache.org/licenses/LICENSE-2.0) license. 

### NOTICE
DISA STIGs are published by DISA IASE, see: https://iase.disa.mil/Pages/privacy_policy.aspx
