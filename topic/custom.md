!SLIDE subsection center blue

# Custom ACM Plugins

!SLIDE bullets

# Custom Plugins

1. We can write plugins in PeopleCode! 
1. Lots of boiler plate code
1. Extend `PTEM_CONFIG:EMConfigurationPlugin` App Package
1. App Package Name requires `PTEM_CONFIG` at the beginning

!SLIDE bullets

# Custom Plugins

    @@@
    class CustomPlugin extends PTEM_CONFIG:EMConfigurationPlugin
       method getPluginHelpMessage() Returns PTEM_CONFIG:PTEMHelpMessage;
       method getProperties() Returns array of PTEM_CONFIG:PTEMVariableProperty;
       method dependant_plugins() Returns array of string;
       method isInternalPlugin() Returns boolean;
    end-class;

~~~SECTION:notes~~~
To create a custom plugin, create a new Application Package that starts with `PTEM_CONFIG_`. Create a class that extends `PTEM_CONFIG:EMConfigurationPlugin` and the four required methods.

ACM often uses CI's where possible to ensure that business logic is used when configuring the system.
~~~ENDSECTION~~~

!SLIDE center subsection blue

# Demo

!SLIDE bullets

# ACM Plugin Development

1. psadmin.io ACM Repository - github.com/psadmin-io
1. PeopleTools Idea: ACM Export to YAML (19985)
1. PeopleTools Idea: App Designer Copy to File (12562)
1. "Hidden" ACM Plugins - peoplesoftmods.com/psadmin/hidden-acm-plugins

