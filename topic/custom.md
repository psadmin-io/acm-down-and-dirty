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

        @@@
        import PT_PC_UTIL:StringMap;
        import PTEM_CONFIG:IEMConfigurationPlugin;
        import PTEM_CONFIG:PTEMHelpMessage;
        import PTEM_CONFIG:PTEMVariableProperty;
        import PTEM_CONFIG:EMConfigurationPlugin;


        class IOTruncateTable extends PTEM_CONFIG:EMConfigurationPlugin
          method getPluginHelpMessage() Returns PTEM_CONFIG:PTEMHelpMessage;
          method getProperties() Returns array of PTEM_CONFIG:PTEMVariableProperty;
          
          method validateVariables(&variables As PT_PC_UTIL:StringMap, &plugin As string) Returns array of PTEM_CONFIG:PTEMHelpMessage;
          method configureEnvironment(&variables As PT_PC_UTIL:StringMap, &plugin As string) Returns array of PTEM_CONFIG:PTEMHelpMessage;
          method validateConfigurations(&variables As PT_PC_UTIL:StringMap, &plugin As string) Returns array of PTEM_CONFIG:PTEMHelpMessage;
          method dependant_plugins() Returns array of string;
          method isInternalPlugin() Returns boolean;
        end-class;


        method getPluginHelpMessage
          /+ Returns PTEM_CONFIG:PTEMHelpMessage +/
          /+ Extends/implements PTEM_CONFIG:EMConfigurationPlugin.getPluginHelpMessage +/
          Local PTEM_CONFIG:PTEMHelpMessage &tempMessage = Null;
          &tempMessage = create PTEM_CONFIG:PTEMHelpMessage(0, 0, "Plugin to run processes", Null);
          Return &tempMessage;
        end-method;

        method configureEnvironment
          /+ &variables as PT_PC_UTIL:StringMap, +/
          /+ &plugin as String +/
          /+ Returns Array of PTEM_CONFIG:PTEMHelpMessage +/
          /+ Extends/implements PTEM_CONFIG:EMConfigurationPlugin.configureEnvironment +/
          
          Local PTEM_CONFIG:PTEMHelpMessage &tempMessage = Null;
          Local array of PTEM_CONFIG:PTEMHelpMessage &helpMsgArray = Null;
          
          rem DEMO Insert Try/Catch code here;
          
          Return &helpMsgArray;
          
        end-method;

        method getProperties
          /+ Returns Array of PTEM_CONFIG:PTEMVariableProperty +/
          /+ Extends/implements PTEM_CONFIG:EMConfigurationPlugin.getProperties +/
          
          rem DEMO insert config validation here;
          
        end-method;

        method dependant_plugins
          /+ Returns Array of String +/
          /+ Extends/implements PTEM_CONFIG:EMConfigurationPlugin.dependant_plugins +/
          Local array of string &dependant_array;
          &dependant_array = CreateArray("");
          Return &dependant_array;
        end-method;

        method isInternalPlugin
          /+ Returns Boolean +/
          /+ Extends/implements PTEM_CONFIG:EMConfigurationPlugin.isInternalPlugin +/
          Return False;
        end-method;

1. Paste in the code for truncating (DEMO)

        @@@
          Local array of string &tableNames;
          Local string &table;
          Local integer &count, &i;

          try
              &tableNames = Split(&variables.get("env.tablelist"), ",");
              For &i = 1 To &tableNames.Len
                SQLExec("truncate table %Table(:1)", @("Record." | &tableNames [&i]));
              End-For;
          catch Exception &ex
              &tempMessage = create PTEM_CONFIG:PTEMHelpMessage(0, 0, &ex.ToString(), Null);
              &helpMsgArray = CreateArray(&tempMessage);
              Return &helpMsgArray;
          end-try;

1. Paste in the code for validating the tablelist property (DEMO)

        @@@
          Local array of PTEM_CONFIG:PTEMVariableProperty &propArray = Null;
          Local PTEM_CONFIG:PTEMVariableProperty &variableProperty = Null;
          
          &variableProperty = create PTEM_CONFIG:PTEMVariableProperty("env.tablelist", "string", True, False, "", 0, 0, "Table List", Null);
          &propArray = CreateArray(&variableProperty);
          Return &propArray;

1. Register `IOTruncateTable` and create a new `Refresh` group
1. Add `IOTruncateTable` to `HCMWIN_REFRESH`
1. Truncate the tables `PSIBLOGHDR,PSIBLOGDATA`

~~~ENDSECTION~~~


!SLIDE bullets

# ACM Plugin Development

* psadmin.io ACM Repository - github.com/psadmin-io
* PeopleTools Idea: ACM Export to YAML (19985)
* PeopleTools Idea: App Designer Copy to File (12562)
* "Hidden" ACM Plugins - peoplesoftmods.com/psadmin/hidden-acm-plugins