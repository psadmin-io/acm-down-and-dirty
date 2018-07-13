!SLIDE subsection center blue

# Custom ACM Plugins

!SLIDE bullets

# Custom Plugins

* We can write plugins in PeopleCode! 
* Lots of boiler plate code
* Extend `PTEM_CONFIG:EMConfigurationPlugin` App Package
* App Package Name requires `PTEM_CONFIG` at the beginning

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

!SLIDE bullets

# ACM Plugin Development

* Use Component Interfaces where possible
* Direct SQL as a last resort
* Small, reusable plugins
* Leverage existing plugin code

!SLIDE center subsection blue

# Demo

~~~SECTION:notes~~~
Build the truncate tables ACM plugin.

1. Paste in boiler plate code
1. Paste in the code for truncating
1. Paste in the code for validating the properties (tablelist)
~~~ENDSECTION~~~


!SLIDE bullets

# ACM Plugin Development

* psadmin.io ACM Repository - github.com/psadmin-io
* PeopleTools Idea: ACM Export to YAML (19985)
* PeopleTools Idea: App Designer Copy to File (12562)
* "Hidden" ACM Plugins - peoplesoftmods.com/psadmin/hidden-acm-plugins