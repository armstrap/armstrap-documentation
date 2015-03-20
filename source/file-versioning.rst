Armstrap Naming and Versioning Convension
=========================================

Format
------

Use the simple file naming convention when creating boards::
        
    <author>_<board-name>_<version>.<extension>

<author>=([0-9][a-z]\\-)*
         - lowercase-alpha-numeric string which can contain dash characters
         - all uppercase characters convert to lowercase characters
         - space characters convert to dash '-' characters
         - invalid characters are ommited
         - usually a company name, username or website domain name

<board-name>=([0-9][a-z]\\-)*
         - lowercase-alpha-numeric string which can contain dash characters
         - all uppercase characters convert to lowercase characters
         - space characters convert to dash '-' characters
         - invalid characters are ommited
         - usually the name of the product
         
<version>=[0-9]*.[0-9]*.[0-9]*
         - three numbers with a period separator
         - <major-version>.<minor-version>.<micro-version>

<extension>=(\\.brd|\\.sch)
         - the given extension of the PCB editor program (*brd* and *sch* are the extension for Cadsoft EAGLE files)


Rules
-----
 
- There are exactly two underscore '_' characters (aka delimeters) in the filename
- **author** must changes when a board forks from one owner/company to another owner/company
- **board-name** usually never changes but can change at time of forking to preserve original 'author'
- **version** must change when a board is submitted to manufacturing
- **version** does not change when **author** changes on a newly forked board
- **board-name** and **version** must exist on the board silkscreen layer (top or bottom).
- *[optional but highly recommended]* **author** must exist on the board silkscreen layer (top or bottom)
- **major-version** is incremented when:
             * the size (dimensions) of the board changes
             * the interface (connectors) to the board change
- **minor-version** is incremented when:
             * any SMT chip is added
             * any SMT chip is removed
             * any SMT has moved
- **micro-version** is incremented when:
             * the silkscreen layer is modified
             * a trace is modified
- **major-version** changing resets **minor-version** and **micro-version** to zero
- **minor-version** changing resets **micro-version** to zero


Remarks
-------

- **major-version** changing usually requires a solder-wave/selective-solder re-tooling, re-stenciling and is the most expensive change.
- **minor-version** changing usually requires no solder-wave/selective-solder re-tooling, but requires re-stenciling.
- **micro-version** changing usually requires no solder-wave/selective-solder re-tooling, no re-stenciling and is generally the cheapest change


Workflow Example
----------------

The Armstrap Eagle Board::

   armstrap_eagle_1.0.0.brd
   
.. Note:: armstrap-org_eagle_1.0.0.brd is also acceptible

The company "VOV Technology" forks the board, keeps armstrap branding but adds its own company logo::
   
   vovtech_armstrap-eagle_1.0.0.brd
   
.. Note:: vovtech-com_armstrap-eagle_1.0.0.brd is also acceptible

The company "VOV Technology" later changes changes a chip on the board but maintains the same board size and interface::

    vovtech_armstrap-eagle_1.1.0.brd
   
The company "VOV Technology" later discovers a silkscreen naming problem and makes a minor change to the silkscreen layer::

   vovtech_armstrap-eagle_1.1.1.brd

The Armstrap community intergrates VOV's changes, removed the VOV branding::

   armstrap_eagle_1.1.0.brd

Community member 'John Smith' forks the Armstrap Eagle board into his own source code repository::

   john-smith_armstrap-eagle_1.1.0.brd


Naming Examples
---------------

- Valid Board Names::
    
    adafruit-com_mintyboost_3.0.0.brd
    sparkfun_weather-shield_1.0.0.brd
    netduino-com_netduino_plus_2.0.1.brd
    
- Invalid Board Names::
    
    mark's_arduino-motor-sheild_2.1.0.brd           // author contains invalid apostrophe character
    arduino-bluetooth-module_1.0.0.brd              // does not have exactly two underscore '_' characters
    supermechanical_twine_1.0.brd                   // missing micro-version
    SparkFun.com_Current-Sensor-Breakout_2.1.0.brd  // author and board name must be lowercase, invalid '.' character in author
    
    
