Language Features
=================

Step implementations
--------------------

Every `step <../gauge_terminologies/steps.md>`__ needs to have a
language specific implementation that gets executed on the spec
execution.

Simple step
^^^^^^^^^^^

**Step name**

::

  * Say "hello" to "gauge"

**Implementation**

.. code-block:: java
  :caption: C#

  // The Method can be written in **any C# class** as long as it is part of the project. 
  public class StepImplementation {

     [Step("Say <greeting> to <product name>")]
     public void HelloWorld(string greeting, string name) {
         // Step implementation
     }

  } 

.. code-block:: java
  :caption: Java

  // This Method can be written in any java class as long as it is in classpath.

  public class StepImplementation {

     @Step("Say <greeting> to <product name>")
     public void helloWorld(String greeting, String name) {
         // Step implementation
     }

  } 
 
.. code-block:: ruby 
  :caption: Ruby 

  step 'Say <greeting> to <product name>' do |greeting, name| 
   # Code for the step 
  end 

Step with table
^^^^^^^^^^^^^^^

Step:
~~~~~

::

  * Create following "hobbit" characters
  |id |name   |
  |---|-------|
  |123|frodo  |
  |456|bilbo  |
  |789|samwise|

Implementation:
~~~~~~~~~~~~~~~

.. code-block:: java
  :caption: C#
   
  // Here Table is a custom data structure defined by gauge. 
  // This is available by adding a reference to the Gauge.CSharp.Lib.
  // Refer : http://nuget.org/packages/Gauge.CSharp.Lib/ 

  public class Users {

     [Step("Create following <role> users <table>")]
     public void HelloWorld(string role, Table table) {
         // Step implementation
     }

  } 

.. code-block:: java
  :caption: Java

  // Table is a custom data structure defined by gauge. 
  public class Users {

    @Step("Create following <race> characters <table>")
    public void createCharacters(String type, Table table) {
        // Step implementation
    }

  } 

.. code-block:: ruby
  :caption: Ruby

  # Here table is a custom data structure defined by gauge-ruby.

  step 'Create following <race> characters <table>' do |role, table| 
    puts table.rows 
    puts table.columns 
  end 


Execution hooks
---------------

Test execution hooks can be used to run arbitrary test code as different
levels during the test suite execution.

Implementation
^^^^^^^^^^^^^^

.. code-block:: java
  :caption: C# 

  public class ExecutionHooks
  { 

    [BeforeSuite] 
    public void BeforeSuite() {
      // Code for before suite 
    }

    [AfterSuite]
    public void AfterSuite() {
      // Code for after suite
    }

    [BeforeSpec]
    public void BeforeSpec() {
      // Code for before spec
    }

    [AfterSpec]
    public void AfterSpec() {
      // Code for after spec
    }

    [BeforeScenario]
    public void BeforeScenario() {
      // Code for before scenario
    }

    [AfterScenario]
    public void AfterScenario() {
      // Code for after scenario
    }

    [BeforeStep]
    public void BeforeStep() {
      // Code for before step
    }

    [AfterStep]
    public void AfterStep() {
      // Code for after step
    }

  } 

.. code-block:: java
  :caption: Java

  public class ExecutionHooks {

    @BeforeSuite public void BeforeSuite() {
       // Code for before suite 
    }

    @AfterSuite
    public void AfterSuite() {
       // Code for after suite
    }

    @BeforeSpec
    public void BeforeSpec() {
       // Code for before spec
    }

    @AfterSpec
    public void AfterSpec() {
       // Code for after spec
    }

    @BeforeScenario
    public void BeforeScenario() {
       // Code for before scenario
    }

    @AfterScenario
    public void AfterScenario() {
       // Code for after scenario
    }

    @BeforeStep
    public void BeforeStep() {
       // Code for before step
    }

    @AfterStep
    public void AfterStep() {
       // Code for after step
    }

  } 

.. code-block:: ruby
  :caption: Ruby

  before_suite do 
    # Code for before suite 
  end

  after_suite do 
    # Code for after suite 
  end

  before_spec do 
    # Code for before spec 
  end

  after_spec do 
    # Code for after spec 
  end

  before_scenario do 
    # Code for before scenario 
  end

  after_scenario do 
    # Code for after scenario 
  end

  before_step do 
    # Code for before step 
  end

  after_step do 
    # Code for after step 
  end 


By default, Gauge clears the state after each scenario so that new
objects are created for next scenario execution. You can
`configure <../advanced_readings/managing_environments.html#gauge_clear_state_level>`__
to change the level at which Gauge clears cache.

Data Store
----------

Data (Objects) can be shared in steps defined in different classes at
runtime using DataStores exposed by Gauge.

There are 3 different types of DataStores based on the lifecycle of when
it gets cleared.

ScenarioStore
^^^^^^^^^^^^^

This data store keeps values added to it in the lifecycle of the
scenario execution. Values are cleared after every scenario executes

.. code-block:: java
   :caption: C#

   using Gauge.CSharp.Lib;

   // Adding value 
   var scenarioStore = DataStoreFactory.ScenarioDataStore;
   scenarioStore.Add("element-id", "455678");

   // Fetching Value 
   var elementId = (string) scenarioStore.Get("element-id");

   // avoid type cast by using generic Get 
   var anotherElementId = scenarioStore.Get("element-id"); 

.. code-block:: java
  :caption: Java

  import com.thoughtworks.gauge.datastore.*; 

  // Adding value 
  DataStore scenarioStore = DataStoreFactory.getScenarioDataStore(); 
  scenarioStore.put("element-id", "455678");

  // Fetching Value 
  String elementId = (String) scenarioStore.get("element-id");

.. code-block:: ruby
  :caption: Ruby

   // Adding value 
   scenario_store = DataStoreFactory.scenario_datastore; 
   scenario_store.put("element-id", "455678");


   // Fetching Value 
   element_id = scenario_store.get("element-id"); 


SpecStore
^^^^^^^^^

This data store keeps values added to it during the lifecycle of the
specification execution. Values are cleared after every specification
executes

.. code-block:: java
  :caption: C#

  using Gauge.CSharp.Lib;

  // Adding value 
  var specStore = DataStoreFactory.SpecDataStore;
  specStore.Add("element-id", "455678");

  // Fetching Value 
  var elementId = (string) specStore.Get("element-id");

  // avoid type cast by using generic Get 
  var anotherElementId = specStore.Get("element-id"); 
  
.. code-block:: java
  :caption: Java

  // Import Package import
  com.thoughtworks.gauge.datastore.*;

  // Adding value DataStore specStore =
  DataStoreFactory.getSpecDataStore(); 
  specStore.put("key", "455678");

  // Fetching value DataStore specStore =
  String elementId = (String) specStore.get("key"); 

.. code-block:: ruby
  :caption: Ruby

  // Adding value 
  spec_store = DataStoreFactory.spec_datastore;
  spec_store.put("element-id", "455678");

  // Fetching Value 
  element_id = spec_store.get("element-id"); 

SuiteStore
^^^^^^^^^^

This data store keeps values added to it during the lifecycle of entire
suite execution. Values are cleared after entire suite execution.

Warning: SuiteStore is not advised to be used when executing specs
in parallel. The values are not retained between parallel streams of
execution.

.. code-block::java
  :caption:C#

  using Gauge.CSharp.Lib;

  // Adding value var suiteStore = DataStoreFactory.SuiteDataStore;
  suiteStore.Add("element-id", "455678");

  // Fetching Value var suiteStore = DataStoreFactory.SuiteDataStore; var
  elementId = (string) suiteStore.Get("element-id");

  // avoid type cast by using generic Get var anotherElementId =
  suiteStore.Get("element-id"); 

.. code-block:: java
  :caption: Java

   // Import Package import
  com.thoughtworks.gauge.datastore.*;

  // Adding value 
  DataStore suiteStore = DataStoreFactory.getSuiteDataStore(); 
  suiteStore.put("element-id", "455678");

  // Fetching value 
  DataStore suiteStore = DataStoreFactory.getSuiteDataStore(); 
  String elementId = (String) suiteStore.get("element-id"); 

.. code-block:: ruby
  :caption: Ruby

  // Adding value 
  suite_store = DataStoreFactory.suite_datastore;
  suite_store.put("element-id", "455678");

  // Fetching Value 
  suite_store = DataStoreFactory.suite_datastore;
  element_id = suite_store.get("element-id"); 

Custom messages in reports
--------------------------

Custom messages/data can be added to execution reports using the below
API from the step implementations or hooks.

These messages will appear under steps in the execution reports.

.. code-block:: java
  :caption: C#

  GaugeMessages.WriteMessage("Custom message for report");
  var id = "4567"; 
  GaugeMessages.WriteMessage("User id is {0}", id); 
 
.. code-block:: java
  :caption: Java

  Gauge.writeMessage("Custom message for report");
  String id = "4567"; 
  Gauge.writeMessage("User id is %s", id);

.. code-block:: ruby
  :caption: Ruby

  Gauge.write_message("Custom message for report")
  id = "4567" 
  Gauge.write_message("User id is" + id)

Enum as Step parameter
----------------------

The constant values of an Enum data type can be used as parameters to a
Step. However, the type of parameter should match the Enum name itself
in step implementation.

Step:

::

  * Navigate towards "SOUTH"

Implementation:

.. code-block:: java
  :caption: Java

  public enum Direction { NORTH, SOUTH, EAST, WEST; }

  @Step("Navigate towards ") 
  public void navigate(Direction direction) {
     //  code here 
  }

Configuration
-------------

General
^^^^^^^

Configuration properties set here will be available to the test
execution as environment variables. Please see
`Environments <#advanced_readings/managing_environments.md>`__ for more
details.

Properties are defined in the following format.

::

   sample_key = sample_value

Java
^^^^

Java Specific configuration changes can be made in the
``env/default/java.properties`` file.

gauge_java_home
~~~~~~~~~~~~~~~~~

Specify an alternate Java home if you want to use a custom version.

Example:

::

   gauge_java_home = PATH_TO_JAVA_HOME

gauge_custom_build_path
~~~~~~~~~~~~~~~~~~~~~~~


Note: IntelliJ out directory will be usually auto-detected.

Use this property if you need to override the build path for the
project.

Example:

::

   gauge_custom_build_path = PATH_TO_CUSTOM_BUILDPATH

gauge_additional_libs
~~~~~~~~~~~~~~~~~~~~~

Specify the directory where additional libraries are kept.

-  You can specify multiple directory names separated with a comma ``,``
-  ``libs`` directory in the gauge project is added by default.

Example:

::

   gauge_additional_libs = libs/*, PATH_TO_NEW_LIBRARY

gauge_jvm_args
~~~~~~~~~~~~~~

Specify the JVM arguments passed to java while launching.

gauge_clear_state_level
~~~~~~~~~~~~~~~~~~~~~~~

Specify the level at which cached objects should get removed while execution. Possible values for this property 
are ``suite``, ``spec`` and ``scenario``. By default, Gauge clears state at scenario level.

Example:

  ::

    gauge_clear_state_level = spec

This clears the objects after the execution of each specification, so
that new objects are created for next execution.

CSharp
^^^^^^

CSharp Specific configuration changes can be made in the ``env/default/default.properties`` file.

gauge_reports_dir
~~~~~~~~~~~~~~~~~

-  The path to the gauge reports directory.
-  Should be either relative to the project directory or an absolute path.

Example:

::

   gauge_reports_dir = reports

overwrite_reports
~~~~~~~~~~~~~~~~~

-  Set as false if gauge reports should not be overwritten on each execution.
-  A new time-stamped directory will be created on each execution.

Example:

  ::

     overwrite_reports = true

     screenshot\_on\_failure

Set to false to disable screenshots on failure in reports.

Example:

  ::

     screenshot_on_failure = true

Ruby
^^^^

The default Ruby properties are similar to that of the CSharp
properties. 



