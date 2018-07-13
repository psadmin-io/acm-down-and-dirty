!SLIDE center subsection blue

# ACM and DPK

!SLIDE bullets

# ACM and DPK

* ACM is fully supported with DPK
* Uses configuration in YAML files
* Dynamically generates ACM template files
* Does not support custom ACM Plugs App Packages

~~~SECTION:notes~~~
The ACM runs for DPK do not use the database templates, so you have to enter the ACM data in `psft_customizations.yaml`.

Also, if you have custom plugins, you need to load them into `PTEM_CONFIG` App Package. That value is hard-coded in the DPK code.
~~~ENDSECTION~~~

!SLIDE bullets

# ACM and YAML

Define Configuration

1. `component_preboot_setup_list:` Hash
1. `component_postboot_setup_list:` Hash


Define Order of Execution

1. `component_preboot_setup_order:` Array
1. `component_preboot_setup_order:` Array

!SLIDE bullets

# ACM and YAML

    @@@yaml
    component_preboot_setup_list:
      web_profile:
        run_control_id:    webprofile
        os_user:           "%{hiera('domain_user')}"

        db_settings: ...

        acm_plugin_list:
          PTWebProfileConfig:
            env.webprofilename: "%{hiera('pia_webprofile_name')}"
            env.helpurl:        "http://www.oracle.com/pls/topic/lookup?id=%CONTEXT_ID%&ctx=%{hiera('help_uri')}"

!SLIDE bullets

# Controlling ACM Runs

There are two variables that let you turn ACM on/off in YAML

1. `run_preboot_config_setup: false`
1. `run_postboot_config_setup: false`

!SLIDE bullets

# Controlling ACM Runs

1. Use Custom Facts to control ACM Runs

        @@@yaml
        run_preboot_config_setup: "%{::acm_preboot}"

1. Override fact with Environment Variable:

        @@@powershellconsole
        PS> $env:FACTER_acm_preboot="true"

!SLIDE bullets

# Controlling ACM Runs

You can also add/remove the ACM profiles from DPK roles

1. `Class['::pt_profile::pt_tools_preboot_config'] ->`
1. `Class['::pt_profile::pt_tools_postboot_config'] ->`

!SLIDE center subsection blue

# Demo

~~~SECTION:notes~~~
Use the ACM to change the `auditPWD` value on the Web Profile

1. Open up `psft_configuration.yaml` and add

        @@@yaml
        env.propertyname:                 'EnablePNSubscriptions,auditPWD'
        env.validationtype:               '1,3'
        env.longvalue:                    'true,dayoffNew'

1. Run Puppet for ACM Preboot only

        @@@powershellconsole
        PS> puppet apply -e "include ::pt_profile::pt_tools_preboot_config" --confdir c:\psft\dpk\puppet -d

~~~ENDSECTION~~~
