
  
# Coding rules and ideas

This article contains ideas and rules for setting up a project from structure to naming conventions for scripts and assets.

<a name="toc"></a>
## Table of Contents



> 1. [Project Structure](#structure)
> 1. [Scripts](#scripts)
> 1. [Asset Naming Conventions](#assetnamingconventions)
> 1. [Asset Workflows](#assetworkflows)


<a name="structure"></a>
## 1. Project Structure
The directory structure style of a project should be considered law. Asset naming conventions and content directory structure go hand in hand, and a violation of either causes unneeded chaos.


<pre>
Assets
    <a name="#structure-developers">_Developers</a>(Use a `_`to keep this folder at the top)
        DeveloperName
            (Work in progress assets)
    <a name="structure-top-level">ProjectName</a>
	    +Art
		    - 3D
			    - Animation
				    - Character
					    - Arthas
					    - Humanoids
					    - Survivals (can be more spesific)
			    - Models
				    - Guns
					    - Kriss Vector
						    - Textures
						    - Prefab
						    - etc...
					    - M4
					    - AK47
		    - 2D
			    - UI
				    - Ingame
				    - Lobby
				    - If there are so many atlases, there can be more folders like Trade, Arena, and etc.
		    - VFX
			    - Categories (If there are so many prefabs in different categories, if there total 5-10 vfx no need to create this folder)
				    - Prefabs
				    - Resource Files
			- Materials
			- Textures
		+ Audio (this folder have the same folder rules above)
			- SFX
				- Ingame
					- Characters
						- Male
						- Female
						- or more spesific like Arthas, Illidan
				- Lobby
			- Music (If there are so many prefabs in different categories, if there total 5-10 of music no need to create sub-category folders)
				- Ingame 
				- Lobby
		+ Code
			- DOTS
				- Authoring
				- Data
				- System
					+ Character (sub-folder)
					+ Minion (sub-folder)
					+ Zombie (sub-folder)
					- ... .cs
					- ... .cs
			- Scripts
				- Abilities
				- Data
				- Dump
				- Interfaces
				- Managers
				- SO (scriptable objects)
				- UI
					- Data
					- Groups
					- Prefabs
		+ Editor
		+ Prefabs
			- Entities
			- Managers
			- UI
			- Zombies/Characters (If there are so many objects, they need to be split. for example; Characters -> Zombies or Characters -> Female -> Zombies)
		+ Scenes (If there aren't so many scenes, no need to be split Scenes folder will be enough)
			- Subscenes
			- Levels, and etc.
		+ Shaders
	+ScriptableObjects(as data instance, not script)
    +Plugins(ThirdPartySDK, just the tools)
    +Resources (just the objects needs to load with Resources Load, this folder have the same folder rules above)
    +DLCs/Mini-Games/extras
</pre>




### Sections

> 1.1 [Folder Names](#structure-folder-names)

> 1.2 [Top-Level Folders](#structure-top-level)

> 1.3 [Developer Folders](#structure-developers)

> 1.4 [Scenes](#scenes)

> 1.5 [Define Ownership](#structure-ownership)

> 1.6 [`Assets` and `AssetTypes`](#structure-assettypes)

> 1.7 [Large Sets](#structure-large-sets)

> 1.8 [Material Library](#structure-material-library)

> 1.9 [Scene Structure](#scene-structure)


<a name="1.1"></a>
<a name="structure-folder-names"><a>
### 1.1 Folder Names
These are common rules for naming any folder in the content structure.

<a name="1.1.1"></a>
#### Always Use [PascalCase](#terms-cases)
PascalCase refers to starting a name with a capital letter and then instead of using spaces, every following word also starts with a capital letter. For example, `DesertEagle`, `RocketPistol`, and `ASeriesOfWords`.

<a name="1.1.2"></a>
#### Never Use Spaces
Never use spaces, spaces can cause various engineering tools and batch processes to fail. Ideally your project's root also contains no spaces and is located somewhere such as `D:\Project` instead of `C:\Users\My Name\My Documents\Unity Projects`.

<a name="1.1.3"></a>
#### Never Use Unicode Characters And Other Symbols
If one of your game characters is named 'Zoë', its folder name should be `Zoe`. Unicode characters can be worse than [Spaces](#1.1.2) for engineering tools and some parts applications don't support Unicode characters in paths either.

Related to this, if your project has and your computer's user name has a Unicode character (i.e. your name is `Zoë`), any project located in your `My Documents` folder will suffer from this issue. Often simply moving your project to something like `D:\Project` will fix these mysterious issues.

Using other characters outside `a-z`, `A-Z`, and `0-9` such as `@`, `-`, `_`, `,`, `*`, and `#` can also lead to unexpected and hard to track issues on other platforms, source control, and weaker engineering tools. 

<a name="1.2"></a>
<a name="structure-top-level"><a>
### 1.2 Use A Top Level Folder For Project Specific Assets
All of a project's assets should exist in a folder named after the project. For example, if your project is named 'Generic Shooter', _all_ of it's content should exist in `Assets/GenericShooter`.

> The `Developers` folder is not for assets that your project relies on and therefore is not project specific. See [Developer Folders](#1.3) for details about this.

There are multiple reasons for this approach.

<a name="1.2.1"></a>
#### No Global Assets
When assets are allowed to exist outside of a project folder it often becomes much harder to enforce a strict structure layout as assets not in a folder encourages the bad behavior of not having to organize assets.

Every asset should have a purpose, otherwise it does not belong in a project. If an asset is an experimental test and shouldn't be used by the project it should be put in a [`Developer`](#1.3) folder.

<a name="1.2.3"></a>
#### Samples, Templates, and 3rd Party Content Are Risk-Free
if a team member decides to add sample content, template files, or assets they bought from a 3rd party, it is guaranteed that these new assets will not interfere with the project in any way unless your project's top level folder is not uniquely named.

You can not trust 3rd party content to fully conform to the [top level folder rule](#1.2). There exist many assets that have the majority of their content in a top level folder but also have possibly modified Unity sample content as well as level files polluting the global `Assets` folder.

Also, we should use Assembly definitions for 3rd party contents, scripts and plugins.

#### DLC, Mini-Games, and Patches Are Easily Maintained
DLC or multiple sub-projects associated with the project that may either be migrated out or simply not cooked in a build, assets relating to these projects should have their own separate top level content folder. This make cooking DLC separate from main project content far easier. Sub-projects can also be migrated in and out with minimal effort. If you need to change a material of an asset or add some very specific asset override behavior in a patch, you can easily put these changes in a patch folder and work safely without the chance of breaking the core project.

<a name="1.3"></a>
<a name="structure-developers"></a>
### 1.3 Use Developers Folder For Local Testing
During a project's development, it is very common for team members to have a sort of 'sandbox' where they can experiment freely without risking the core project. Because this work may be ongoing, these team members may wish to put their assets on a project's source control server. Not all teams require the use of Developer folders, but the ones that do use them often run into a common problem with assets submitted to source control.

It is very easy for a team member to accidentally use assets that are not ready for use which will cause issues once those assets are removed. For example, an artist may be iterating on a modular set of static meshes and still working on getting their sizing and grid snapping correct. If a world builder sees these assets in the main project folder, they might use them all over a level not knowing they could be subject to incredible change and/or removal. This causes massive amounts of re-working by everyone on the team to resolve.

If these modular assets were placed in a Developer folder, the world builder should never have had a reason to use them and the whole issue would never happen.

Once the assets are ready for use, an artist simply has to move the assets into the project-specific folder. This is essentially 'promoting' the assets from experimental to production.

<a name="scenes"></a>
### 1.4 All [Scene](#terms-level-map) Files Belong In A Folder Called Scenes
Scene files are incredibly special and it is common for every project to have its own map naming system, especially if they work with sub-scenes or streaming scenes. No matter what system of map organization is in place for the specific project, all scenes should belong in `Assets/ProjectNameName/Scenes`.

Being able to tell someone to open a specific map without having to explain where it is is a great time saver and general 'quality of life' improvement. It is common for scenes to be within sub-folders of `Scenes`, such as `Scenes/Campaign1/` or `Scenes/Arenas`, but the most important thing here is that they all exist within `Assets/ProjectNameName/Scenes`.

This also simplifies the job of cooking for engineers. Wrangling scenes for a build process can be extremely frustrating if they have to dig through arbitrary folders for them. If a team's scenes are all in one place, it is much harder to accidentally not cook a map in a build. It also simplifies lighting build scripts as well QA processes.

<a name="1.5"></a>
<a name="structure-ownership"></a>
### 1.5 Ownership
Define ownership of zone/assets/features. Some assets like scenes or prefabs do not handle simultaneous changes by multiple people very well, creating conflict. Having a single person who can change (or give the right to change) a given asset helps to avoid that problem. Always work on your developer folder which needs to have its own Assembly Definition. Create a variant of the scenes for yourself to work on without changing the original one. While working on a general system/service, stay in contact with your team and make sure no one else doing the same thing on that system/service.

<a name="1.6"></a>
<a name="structure-material-library"></a>
### 1.6 Material Library

If there are master materials, layered materials, or any form of reusable materials or textures that do not belong to any subset of assets, these assets should be located in `Assets/ProjectName/Art/Materials`.

This way all 'global' materials have a place to live and are easily located.

Any testing or debug materials should be within `Materials/Debug`. This allows debug materials to be easily stripped from a project before shipping and makes it incredibly apparent if production assets are using them if reference errors are shown.

<a name="1.7"></a>
<a name="scene-structure"></a>
## 1.7 Scene Structure
Next to the project’s hierarchy, there’s also scene hierarchy. As before, we’ll present you a template. You can adjust it to your needs. Use named empty game objects as scene folders. Becareful with it, do not use dynamic objects in a folder object it will cause performance issues. Dynamic objects should be parent-free in the scene. Use the names exactly how they are here.

<pre>
--->SubScenes (it's not a folder object, it's just a brace to make the scene more organized)
--->Managers
	All the managers/singletons
--->Scene
	+ Cameras
	+ World
		+ Lights
		+ Terrains
		+ Props
		+ Volumes
		+ Static objects, like Houses or zones
	+ Gameplay
		+ Actors
		+ Items
--->UI
--->Dynamic  (it's not a folder object, it's just a brace to make the scene more organized)

</pre>

Here is an example; In the screenshot, Dynamic and Subscene objects are just pins, not folder objects, to separate/organize objects.

[Example screenshot](https://prnt.sc/fdf_tb2mY-zz)

 - All empty objects should be located at 0,0,0 with default rotation and scale.
 - When you’re instantiating an object in runtime, do not populate the "--->Dynamic" object.

**[⬆ Back to Top](#table-of-contents)**

<a name="scripts"></a>

## 2. Scripts

This section will focus on C# classes and their internals. When possible, style rules conform to Microsoft's C# standard. The most important rule in this section is all the names can be long but **must be unambiguous, and descriptive.**


### Sections
> 2.1 [Class Organization](#classorganization)

> 2.2 [Compiling](#compiling)

> 2.3 [Variables](#variables)

> 2.4 [Functions](#functions)

<a name="classorganization"></a>
### 2.1 Class Organization
Source files should contain only one public type, although multiple internal classes are allowed.

Source files should be given the name of the public class in the file.

Class members should be alphabetized, and grouped into sections:

* `Constructors, Enums, Structs, and etc.`
* `Constant Fields`
* `Static Fields`
* `Events, Delegates, Actions`
* `Properties, Methods that returns values`
* `Fields`
* `LifeCycle Methods` (Awake, OnEnable, OnDisable, OnDestroy), Use them in # execution order.
* `Public Methods`
* `Private Methods`
* `Public Listeners`
* `Private Listeners`

Within each of these groups order by access:

* `public`
* `internal`
* `protected`
* `private`

```
public class temp : MonoBehaviour
{
    /* ------------------------------------------ */

    //Public & Serializefield variables are in the same category
    #region Public-Serializable-Variables

	public delegate void DayChangedEventHandler(object sender, DateTime e);

    public event DayChangedEventHandler DayChangedEvent;  

    /// <summary>  
    /// Public and Serializable variables should have a summary and tooltip
    /// </summary>
    [Tooltip("Public variables set in the Inspector, should have a Tooltip")]
    public const string ConnectionString = "databaseConnectionLink";
    public static string ConnectionString = "databaseConnectionLink";
    public string ConnectionString { get; private set; }
    [Serializefield] string ConnectionString = "databaseConnectionLink";
    
    //If you can, use private set.   
    
    public string ConnectionString(string temp)
    {
        return "databaseConnectionLink";
    }

    #endregion

    /* ------------------------------------------ */

    #region Internal-Variables

    internal const string ConnectionString = "databaseConnectionLink";
    internal static string ConnectionString = "databaseConnectionLink";
    internal string ConnectionString = "databaseConnectionLink";
    internal string ConnectionString { get; set; }

    internal string ConnectionString(string temp)
    {
        return "databaseConnectionLink";
    }

    internal delegate void DayChangedEventHandler(object sender, DateTime e);

    internal event DayChangedEventHandler DayChangedEvent;

    #endregion

    /* ------------------------------------------ */

    #region Protected-Variables

    protected const string _connectionString = "databaseConnectionLink";
    protected static string _connectionString = "databaseConnectionLink";
    protected string _connectionString = "databaseConnectionLink";
    protected string _connectionString { get; set; }

    protected string _connectionString(string temp)
    {
        return "databaseConnectionLink";
    }

    protected delegate void _dayChangedEventHandler(object sender, DateTime e);

    protected event _dayChangedEventHandler DayChangedEvent;

    #endregion

    /* ------------------------------------------ */

    #region Private-Variables

    private const string _connectionString = "databaseConnectionLink";
    private static string _connectionString = "databaseConnectionLink";
    private string _connectionString = "databaseConnectionLink";
    private string _connectionString { get; set; }

    private string _connectionString(string temp)
    {
        return "databaseConnectionLink";
    }

    private delegate void _dayChangedEventHandler(object sender, DateTime e);

    private event _dayChangedEventHandler DayChangedEvent;

    #endregion

    /* ------------------------------------------ */

    #region LifeCycle

    private void Awake()
    {
    }

    private void OnEnable()
    {
    }

    private void Start()
    {
    }

    private void Update()
    {
    }
    
	private void OnDisable()
    {
    }
    
	private void OnDestroy()
    {
    }

    #endregion

    /* ------------------------------------------ */

    #region Frontend

    public void DoSomethingFrontEnd()
    {
    }

    public void DoSomething2FrontEnd()
    {
    }

    /* ------------------------------------------ */

    internal void DoSomethingFrontend()
    {
    }

    internal void DoSomething2Frontend()
    {
    }

    #endregion

    /* ------------------------------------------ */

    #region Backend

    protected void DoSomethingBackend()
    {
    }

    protected void DoSomething2Backend()
    {
    }

    /* ------------------------------------------ */

    private void DoSomethingBackend()
    {
    }

    private void DoSomething2Backend()
    {
    }

    #endregion

    /* ------------------------------------------ */

    #region IEnumators

    public IEnumerator IEDoSomething()
    {
    }

    /* ------------------------------------------ */

    private IEnumerator IEDoSomething()
    {
    }

    #endregion

    /* ------------------------------------------ */

    #region Tasks

    public Task DoSomething()
    {
        
    }
    
    /* ------------------------------------------ */

    private Task DoSometing()
    {
        
    }

    #endregion
    
    /* ------------------------------------------ */

}
```


#### All Public Functions Should Have A Summary

Simply, any function that has an access modifier of Public should have its summary filled out. 

```
/// <summary>
/// Fire the gun, duh -,-
/// </summary>
public void Fire()
{
	// Fire the gun. :P
}
```


#### Commenting
Comments should be used to describe intention, algorithmic overview, and/or logical flow.
It would be ideal if from reading the comments alone someone other than the author could understand a function’s intended behavior and general operation.

While there are no minimum comment requirements and certainly some very small routines need no commenting at all, it is hoped that most routines will have comments reflecting the programmer’s intent and approach.

##### Comment Style
Place the comment on a separate line, not at the end of a line of code.

Begin comment text with an uppercase letter.

End comment text with a period.

Insert one space between the comment delimiter (//) and the comment text, as shown in the following example.

The // (two slashes) style of comment tags should be used in most situations. Where ever possible, place comments above the code instead of beside it. Here are some examples:
```
        // Sample comment above a variable.
        private int _myInt = 5;
```

#### Regions
The `#region` directive enables you to collapse and hide sections of code in C# files. The ability to hide code selectively makes your files more manageable and easier to read. Use it to organize the code/logic by categories such as Back-end, Front-end, LifeCycle, Public methods, Private Methods.

```
#region "This is the code to be collapsed"
    Private components As System.ComponentModel.Container
#endregion
```

#### Spacing
Do use a single space after a comma between function arguments.

Example: `Console.In.Read(myChar, 0, 1);`
* Do not use a space after the parenthesis and function arguments.
* Do not use spaces between a function name and parenthesis.
* Do not use spaces inside brackets.


<a name="compiling"></a>
### 2.2 Compiling
All scripts should compile with zero warnings and zero errors. You should fix script warnings and errors immediately as they can quickly cascade into very scary unexpected behavior.

Do *not* submit broken scripts to feature's branch. If you must store them on source control, shelve them or submit to your private branch if you have one.



<a name="variables"></a>
### 2.3 Variables
The words `variable` and `property` may be used interchangeably.

#### Variable Naming

##### Naming Conventions

Naming convention rules by access modifiers:

* `public` -> `UpperCamelCase` -> `ConnectionString`
* `internal` -> `UpperCamelCase` -> `ConnectionString`
* `protected` -> `"_" + lowerCamelCase` -> `_connectionString`
* `private` -> `"_" + lowerCamelCase` -> `_connectionString`
* `local` -> `lowerCamelCase` -> `connectionString`


##### Considered Context
All variable names must not be redundant with their context as all variable references in the class will always have context.

###### Considered Context Examples:
Consider a Class called `PlayerCharacter`.

**Bad**

* `PlayerScore`
* `PlayerKills`
* `MyTargetPlayer`
* `MyCharacterName`
* `CharacterSkills`
* `ChosenCharacterSkin`
* `MovSpeed`

All of these variables are named redundantly. It is implied that the variable is representative of the `PlayerCharacter` it belongs to because it is `PlayerCharacter` that is defining these variables.

**Good**

* `Score`
* `Kills`
* `Target` || `CurrentTarget`
* `Name`
* `Skills`
* `Skin` || `CurrentSkin`
* `MovementSpeed`

#### Variable Access Level
In C#, variables have a concept of access level. Public means any code outside the class can access the variable. Protected means only the class and any child classes can access this variable internally. Private means only this class and no child classes can access this variable.
Variables should only be made public if necessary.

Prefer to use the attribute `[SerializeField]` instead of making a variable public.

##### Local Variables
Define the variable with the type instead of using var.

**Bad**

```
var var1 = "Some string.";
var var2 = 27;
var var3 = ExampleClass.ResultSoFar();

for (var i = 0; i < bountyHunterFleets.Length; ++i) {};
```

**Good**

```
string  var1 = "Some string.";
int var2 = 27;
int var3 = ExampleClass.ResultSoFar();

for (int i = 0; i < bountyHunterFleets.Length; ++i) {};
```


<a name="publicvariables"></a>
##### Public Variables

The value of a class will not be changed by any class, and will not be assigned. If the data needs to be updated, the class will do it, public data will be used only to be referenced. The changes will be made within the class's own methods. Each class is responsible for updating its data and references. For example;


```
public class Health
{
	public float CurrentHealth;
	public void IncreaseHealth(int amount) 
	{
		CurrentHealth += amount;
	}
}
```



##### How and when use Hungarian notation
Do _not_ use Hungarian notation with _value types_; use it with _reference types_ such as UI elements. Use a prefix, minimum 3 characters. You will find more details about it in [here](#4). For example;
```
// Correct
int counter;
string name;
 
// Avoid
int iCounter;
string strName;

// Correct
[SerializeField] Text TxtNickname;
[SerializeField] Button BtnPlay;
 
// Avoid
[SerializeField] Text TNickname || TextNickname;
[SerializeField] Button ButtonPlay;
```

#### Variables accessible in the Editor

##### Tooltips 
All Serializable variables should have a description in their `[Tooltip]` fields that explains how changing this value affects the behavior of the script.

##### Variable Slider And Value Ranges
All Serializable variables should make use of slider and value ranges if there is ever a value that a variable should _not_ be set to.

Example: A script that generates fence posts might have an editable variable named `PostsCount` and a value of -1 would not make any sense. Use the range fields `[Range(min, max)]` to mark 0 as a minimum.

If an editable variable is used in a Construction Script, it should have a reasonable Slider Range defined so that someone can not accidentally assign it a large value that could crash the editor.

A Value Range only needs to be defined if the bounds of a value are known. While a Slider Range prevents accidental large number inputs, an undefined Value Range allows a user to specify a value outside the Slider Range that may be considered 'dangerous' but still valid.

#### Variable Types

##### Booleans

###### Boolean Prefix
All booleans should be named in PascalCase but prefixed with a verb.

Example: Use `isDead` and `hasItem`, **not** `Dead` and `Item`.

###### Boolean Names
All booleans should be named as descriptive adjectives when possible if representing general information.

Try to not use verbs such as `isRunning`. Verbs tend to lead to complex states.

###### Boolean Complex States
Do not use booleans to represent complex and/or dependent states. This makes state adding and removing complex and no longer easily readable. Use an enumeration instead.

Example: When defining a weapon, do **not** use `isReloading` and `isEquipping` if a weapon can't be both reloading and equipping. Define an enumeration named `WeaponState` and use a variable with this type named `WeaponState` instead. This makes it far easier to add new states to weapons.

##### Enums
Enums use PascalCase and use singular names for enums and their values. Exception: bit field enums should be plural. Enums can be placed outside the class space to provide global access.

Example: 
```
public enum WeaponType
{
    Knife,
    Gun
}

// Enum can have multiple values
[Flags]
public enum Dockings
{
	None = 0,
	Top = 1,
}

public WeaponType Weapon
```

##### Arrays
Arrays follow the same naming rules as above, but should be named as a plural noun.

Example: Use `Targets`, `Hats`, and `EnemyPlayers`, not `TargetList`, `HatArray`, `EnemyPlayerArray`.

##### Interfaces
Interfaces are led with a capital `I` then followed with PascalCase.

Example: ```public interface ICanEat { }```


<a name="functions"></a>
### 3.4 Functions, Events, and Event Dispatchers
This section describes how you should author functions, events, and event dispatchers. Everything that applies to functions also applies to events, unless otherwise noted.

#### Function Naming
The naming of functions, events, and event dispatchers is critically important. Based on the name alone, certain assumptions can be made about functions. For example:

* Is it a pure function?
* Is it fetching state information?
* Is it a handler?
* What is its purpose?

These questions and more can all be answered when functions are named appropriately.

<a name="function-verbrule"></a>
#### All Functions Should Be Verbs
All functions and events perform some form of action, whether its getting info, calculating data, or causing something to explode. Therefore, all functions should start with verbs. They should be worded in the present tense whenever possible. They should also have some context as to what they are doing.

Good examples:

* `Fire` - Good example if in a Character / Weapon class, as it has context. Bad if in a Barrel / Grass / any ambiguous class.
* `Jump` - Good example if in a Character class, otherwise, needs context.
* `Explode`
* `ReceiveMessage`
* `SortPlayerArray`
* `GetArmOffset`
* `GetCoordinates`
* `UpdateTransforms`
* `EnableBigHeadMode`
* `IsEnemy` - ["Is" is a verb.](http://writingexplained.org/is-is-a-verb)

Bad examples:

* `Dead` - Is Dead? Will deaden?
* `Rock`
* `ProcessData` - Ambiguous, these words mean nothing.
* `PlayerState` - Nouns are ambiguous.
* `Color` - Verb with no context, or ambiguous noun.

#### Functions Returning Bool Should Ask Questions
When writing a function that does not change the state of or modify any object and is purely for getting information, state, or computing a yes/no value, it should ask a question. This should also follow [the verb rule](#function-verbrule).

This is extremely important as if a question is not asked, it may be assumed that the function performs an action and is returning whether that action succeeded.

Good examples:

* `IsDead`
* `IsOnFire`
* `IsAlive`
* `IsSpeaking`
* `IsHavingAnExistentialCrisis`
* `IsVisible`
* `HasWeapon` - ["Has" is a verb.](http://grammar.yourdictionary.com/parts-of-speech/verbs/Helping-Verbs.html)
* `WasCharging` - ["Was" is past-tense of "be".](http://grammar.yourdictionary.com/parts-of-speech/verbs/Helping-Verbs.html) Use "was" when referring to 'previous frame' or 'previous state'.
* `CanReload` - ["Can" is a verb.](http://grammar.yourdictionary.com/parts-of-speech/verbs/Helping-Verbs.html)

Bad examples:

* `Fire` - Is on fire? Will fire? Do fire?
* `OnFire` - Can be confused with event dispatcher for firing.
* `Dead` - Is dead? Will deaden?
* `Visibility` - Is visible? Set visibility? A description of flying conditions?

#### Functions Responsible For Actions of UI Elements
When using the regular functions for the UI elements, it mostly becomes harder to handle it on debugging. So, that's why we are using some special tags for them. In that way, we assume that the function references are made by hand on the inspector that's why we should carefully inspect that function and expect some independent triggers besides the references we found on the IDE.

Example for Buttons:

* `FunBuy` ->  `FunX`

Example for Inputfields:

* `OnInputNicknameValueChanged` -> `OnInputXValueChanged`

Example for Dropdowns:

* `OnDrpNicknameValueChanged` -> `OnDrpXValueChanged`

Example for Toggles:

* `OnTgNicknameValueChanged` -> `OnTgXValueChanged`


#### Event Handlers and Dispatchers Should Start With `On`
Any function that handles an event or dispatches an event should start with `On` and continue to follow [the verb rule](#function-verbrule).

Good examples:

* `OnDeath` 
* `OnPickup`
* `OnReceiveMessage`
* `OnMessageRecieved`
* `OnTargetChanged`
* `OnClick`
* `OnLeave`

Bad examples:

* `OnData`
* `OnTarget`


#### Don't Search(Find) References In Run-time

Any kind of finding method is forbidden, stay away from them. Make sure you assigned all dependencies by hand in the inspector or with constructor methods for non-monobehaviour classes. You can use the ContextMenu attribute if you want. ContextMenu methods must be hidden in the region called "EditorHelpers".


#### Use "For" Instead of "Foreach"

Avoid using `foreach` for loops, it's a little more expensive than `for` and creates garbage.

#### Language Integrated Query (LINQ)

Linq is a powerful way to create queries but it can be expensive. Be careful with Linq don't use it in loops and repetitive methods.


**[⬆ Back to Top](#table-of-contents)**

<a name="assetnamingconventions"></a>
<a name="3"></a>

## 3. Asset Naming Conventions
Naming conventions should be treated as law. A project that conforms to a naming convention is able to have its assets managed, searched, parsed, and maintained with incredible ease.

Most things are prefixed with the prefix generally being an acronym of the asset type followed by an underscore.

**Assets use [PascalCase](#cases)**

<a name="base-asset-name"></a>
<a name="3.1"></a>
### 3.1 Base Asset Name - `Prefix_BaseAssetName_Variant_Suffix`
All assets should have a _Base Asset Name_. A Base Asset Name represents a logical grouping of related assets. Any asset that is part of this logical group 
should follow the standard of  `Prefix_BaseAssetName_Variant_Suffix`.

Keeping the pattern `Prefix_BaseAssetName_Variant_Suffix` in mind and using common sense is generally enough to warrant good asset names. Here are some detailed rules regarding each element.

`Prefix` and `Suffix` are to be determined by the asset type through the following [Asset Name Modifier](#asset-name-modifiers) tables.

`BaseAssetName` should be determined by short and easily recognizable name related to the context of this group of assets. For example, if you had a character named Bob, all of Bob's assets would have the `BaseAssetName` of `Bob`.

For unique and specific variations of assets, `Variant` is either a short and easily recognizable name that represents logical grouping of assets that are a subset of an asset's base name. For example, if Bob had multiple skins these skins should still use `Bob` as the `BaseAssetName` but include a recognizable `Variant`. An 'Evil' skin would be referred to as `Bob_Evil` and a 'Retro' skin would be referred to as `Bob_Retro`.

For unique but generic variations of assets, `Variant` is a two digit number starting at `01`. For example, if you have an environment artist generating nondescript rocks, they would be named `Rock_01`, `Rock_02`, `Rock_03`, etc. Except for rare exceptions, you should never require a three digit variant number. If you have more than 100 assets, you should consider organizing them with different base names or using multiple variant names.

Depending on how your asset variants are made, you can chain together variant names. For example, if you are creating flooring assets for an Arch Viz project you should use the base name `Flooring` with chained variants such as `Flooring_Marble_01`, `Flooring_Maple_01`, `Flooring_Tile_Squares_01`.

If the file type consists of a single word, you need to use the first three letters of it like in the examples;


| Asset Type               | Asset Name   |
| ------------------------ | ------------ |
| Audio            | Aud_M4_Shoot       |
| Material                 | Mat_Bob_Standard_Head        |
| Texture (Diffuse/Albedo) | Tex_Bob_Standard_D      |
| Texture (Normal)         | Tex_Bob_Standard_N      |
| Texture (Evil Diffuse)   | Tex_Bob_Evil_D |


If it consists of multiple words, you need to use the first letters from it. For example;


| Asset Type               | Asset Name   |
| ------------------------ | ------------ |
| Skeletal Mesh            | SM_Bob       |
| Material Instance                 | MI_Bob_Standard_Body        |
| Particle System | PS_Blood_Splash      |
| Static Mesh (01)         | SM_Rock_01   |
| Static Mesh (02)         | SM_Rock_02   |
| Static Mesh (03)         | SM_Rock_03   |
| Cube Render Target      | RTC_Temp     |
| Texture Light Profile   | TLP_Temp     |






<a name="asset-name-modifiers"></a>
### 3.2 Asset Name Modifiers

When naming an asset use these tables to determine the prefix and suffix to use with an asset's [Base Asset Name](#base-asset-name).

#### Sections

> 3.2.1 [Most Common](#anc-common)

> 3.2.2 [Animations](#anc-animations)

> 3.2.3 [Artificial Intelligence](#anc-ai)

> 3.2.4 [Prefabs](#anc-prefab)

> 3.2.5 [Materials](#anc-materials)

> 3.2.6 [Textures](#anc-textures)

> 3.2.7 [Miscellaneous](#anc-misc)

> 3.2.8 [Physics](#anc-physics)

> 3.2.9 [Audio](#anc-audio)

> 3.2.10 [User Interface](#anc-ui)

> 3.2.11 [Effects](#anc-effects)

<a name="anc-common"></a>
#### Most Common

| Asset Type              | Prefix     | Suffix     | Notes                            |
| ----------------------- | ---------- | ---------- | -------------------------------- |
| Level / Scene           |  Sce_          |            | [Should be in a folder called Scenes.](#scenes) e.g. `Scenes/Sce_C17_Parking_Garage.unity` |
| Prefab                  | Pre_       |            |                                  |
| Material                | Mat_         |            |                                  |
| Static Mesh             | SM_       |            |                                  |
| Skeletal Mesh           | SM_       |            |                                  |
| Texture                 | Tex_         | _?         | See [Textures](#anc-textures)    |
| Particle System         | PS_       |            |                                  |

<a name="anc-models"></a>

#### 3.2.1a 3D Models (FBX Files)

| Asset Type    | Prefix | Suffix | Notes |
| ------------- | ------ | ------ | ----- |
| Characters    | Cha_    |        |       |
| Vehicles      | Veh_    |        |       |
| Weapons       | Wea_    |        |       |
| Static Mesh   | SM_    |        |       |
| Skeletal Mesh | SM_    |        |       |
| Skeleton      | Ske_  |        |       |
| Rig           | Rig_   |        |       |

#### 3.2.1b 3d Models (3ds Max)

All meshes in 3ds Max are lowercase to differentiate them from their FBX export.

| Asset Type    | Prefix | Suffix      | Notes                                   |
| ------------- | ------ | ----------- | --------------------------------------- |
| Mesh          | Mes_       | _mesh_lod0* | Only use LOD suffix if model uses LOD's |
| Mesh Collider | MC_       | _collider   |                                         |

<a name="anc-animations"></a>

#### 3.2.2 Animations 
| Asset Type           | Prefix | Suffix | Notes |
| -------------------- | ------ | ------ | ----- |
| Animation       | Ani_     |        |       |
| Animation Controller | AC_    |        |       |
| Avatar Mask          | AM_    |        |       |
| Morph Target         | MT_    |        |       |

<a name="anc-ai"></a>
#### 3.2.3 Artificial Intelligence

| Asset Type              | Prefix     | Suffix     | Notes                            |
| ----------------------- | ---------- | ---------- | -------------------------------- |
| AI Controller           | AIC_     |            |                                  |
| Behavior Tree           | BT_      |            |                                  |
| Blackboard              | BB_       |            |                                  |
| Decorator               | BTDecorator_ |          |                                  |
| Service                 | BTService_ |            |                                  |
| Task                    | BTTask_  |            |                                  |
| Environment Query       | EQS_     |            |                                  |
| EnvQueryContext         | EQS_     | Context    |                                  |

<a name="anc-prefab"></a>
#### 3.2.4 Prefabs

| Asset Type              | Prefix     | Suffix     | Notes                            |
| ----------------------- | ---------- | ---------- | -------------------------------- |
| Prefab         | Pre_       |            |                                  |
| Prefab Instance         | PI_       |            |                                  |
| Scriptable Object       | SO_    |        |  |

<a name="anc-materials"></a>

#### 3.2.5 Materials
| Asset Type        | Prefix | Suffix | Notes |
| ----------------- | ------ | ------ | ----- |
| Material          | Mat_     |        |       |
| Material Instance | MI_    |        |       |
| Physical Material | PM_    |        |       |

<a name="anc-textures"></a>

#### 3.2.6 Textures
| Asset Type              | Prefix     | Suffix     | Notes                            |
| ----------------------- | ---------- | ---------- | -------------------------------- |
| Texture                 | Tex_         |            |                                  |
| Texture (Diffuse/Albedo/Base Color)| Tex_ | _D      |                                  |
| Texture (Normal)        | Tex_         | _N         |                                  |
| Texture (Roughness)     | Tex_         | _R         |                                  |
| Texture (Alpha/Opacity) | Tex_         | _A         |                                  |
| Texture (Ambient Occlusion) | Tex_     | _AO      |                                  |
| Texture (Bump)          | Tex_         | _B         |                                  |
| Texture (Emissive)      | Tex_         | _E         |                                  |
| Texture (Mask)          | Tex_         | _M         |                                  |
| Texture (Specular)      | Tex_         | _S         |                                  |
| Texture Cube            | TC_       |            |                                  |
| Media Texture           | MT_       |            |                                  |
| Render Target           | RT_       |            |                                  |
| Cube Render Target      | RTC_     |            |                                  |
| Texture Light Profile   | TLP_     |            |                                  |


<a name="anc-misc"></a>

#### 3.2.7 Miscellaneous

| Asset Type                      | Prefix | Suffix | Notes |
| ------------------------------- | ------ | ------ | ----- |
| Universal Render Pipeline Asset | URPA_   |        |       |
| Post Process Volume Profile     | PPVP_    |        |       |
| User Interface                  | UI_    |        |       |

<a name="anc-physics"></a>
#### 3.2.8 Physics

| Asset Type        | Prefix | Suffix | Notes |
| ----------------- | ------ | ------ | ----- |
| Physical Material | PM_    |        |       |

<a name="anc-audio"></a>

#### 3.2.9 Audio

| Asset Type     | Prefix | Suffix | Notes                                                        |
| -------------- | ------ | ------ | ------------------------------------------------------------ |
| Audio Clip     | Aud_     |        |                                                              |
| Audio Mixer    | AM_   |        |                                                              |

<a name="anc-ui"></a>
#### 3.2.10 User Interface
| Asset Type       | Prefix | Suffix | Notes |
| ---------------- | ------ | ------ | ----- |
| Font             | Font_  |        |       |
| Texture (One sprite) | Spr_     |    |       |
| Texture (Sprite atlas) | Tex_     | _GUI   |       |

<a name="anc-effects"></a>
#### 3.2.11 Effects
| Asset Type      | Prefix | Suffix | Notes |
| --------------- | ------ | ------ | ----- |
| Particle System | PS_    |        |       |
**[⬆ Back to Top](#table-of-contents)**


<a name="assetworkflows"></a>
## 4. Asset Workflows

This section describes best practices for creating and importing assets usable in Unity.

<a name="toc"></a>
### Sections

> 4.1 [3ds Max](#3dsmax)
>
> 4.2 [Textures](#textures)
>
> 4.3 [Audio](#audio)
>
> 4.4 [Sprite](#sprite)



<a name="3dsmax"></a>
### 4.1 3ds Max

Unity guide on importing from 3ds Max:

https://docs.unity3d.com/2017.4/Documentation/Manual/HOWTO-ImportObjectMax.html

Unity tutorial on the FBX Exporter Package for FBX roundtrip:

https://learn.unity.com/project/3ds-max-to-unity-pipeline

#### Setting up 3ds Max

Unity uses 1 unit = 1 meter. Setup 3ds Max to use Meters by going to ```Customize/Units Setup/System Unit Setup``` and set to 1 Unit = 1 Meter. Using the correct scale is very important for correct Physics / GI / and VR interaction.

Animation frame rate in 3ds Max should be set to 30fps. The ```Time Configuration``` dialog box has 3ds Max's FPS settings

##### Working with Small Objects

* Set ```Customize > Customize User Interface > Mouse Wheel Zoom Increment``` to 0.1m to stop over zooming

* Turn on Viewport Clipping and set the slider on the side of the viewport to be able to zoom in on small meshes. (https://knowledge.autodesk.com/support/3ds-max/learn-explore/caas/sfdcarticles/sfdcarticles/Viewport-Clipping.html)

#### Modeling in 3ds Max

* Follow the [asset naming convention](#anc-models)
* Avoid super long thin triangles (Speeds up tile based renderers & helps with proper GI baking)
* Use Area and Angle Weighted Mesh Normals (Unity Import Setting or Create in 3ds Max)

#### Exporting from 3ds Max into Unity

##### Export Settings:

- Triangulate On
- Tangents and Binormals Off
- Smoothing Groups On
- Preserve edge orientation On
- Units - Automatic Off / Scene Units converted to Meters
- Axis Conversion Z-up

Models created in 3ds Max use a different coordinate system then Unity. Models need to have their pivot point rotated +90 degrees on the X axis to import into Unity correctly.

To do this quickly, open the MaxScript editor, paste this code and select and drag this code on to a Toolbar in 3ds Max to create a button that will run this script. It applies a Xform modifier to rotate the pivot before exporting.

```
fn RotateCreationPivot obj rot =
(
select obj
modPanel.addModToSelection (XForm ()) ui:on
obj.modifiers[#XForm].gizmo.rotation += rot as quat
rotate obj (inverse rot as quat)
)
RotateCreationPivot $ (eulerToQuat(eulerAngles 90 0 0))
```


Script to rotate all objects in 3ds Max scene for export

```
(
    mapped fn ProcessObjectsForUnity node =
    (
        resetxform node
        tm = rotatexmatrix 90
        tm.row4 = node.pos
        node.transform = tm
        node.objectoffsetrot = eulerangles -90 0 0
    )
    
    ProcessObjectsForUnity geometry
)
```

* Batch Exporter for 3ds Max (http://www.strichnet.com/improving-the-fbx-workflow-between-3ds-max-and-unity3d/)

##### Exporting CAT Animation to FBX

Bind normal bones to the CAT rig for use in skinning and exporting

###### Bind Pose

Set Motion Panel/Layer Manager/"Setup/Animation Mode" Toggle to ```Red```
Select only the bones and the mesh you wish to export
Export naming: ModelName.FBX

###### Animation

Set Motion Panel/Layer Manager/"Setup/Animation Mode" to ```Green```
Select ONLY the bones required in your hierarchy (These should match the exact same bones used for Bind Pose), don't include the mesh.
Export naming: ModelName@AnimationName.FBX
The @ symbol is a special Unity naming convention allowing the animation to be bound to the Human.fbx in the Unity editor

#### Importing from 3ds Max into Unity

If importing only animation or bones from a FBX: 

* Set ```Preserve Hierarchy Model``` import option to ```True```
* Set ```Rig > Avatar Definition``` to ```Copy From Other Avatar```

MaxListener Window, set width and height of selected bones, maybe objects too?
$.width = 0.01
$.height = 0.01

**[⬆ Back to Top](#table-of-contents)**

<a name="textures"></a>
### 4.2 Textures

* Textures follow the [naming convention](#anc-textures) found above. 
* They are a power of two (For example, 512 x 512 or 256 x 1024).
* Use Texture Atlases wherever possible.
* 3D software should point to the Unity project textures for consistency when you save or export.
* It is better to resize the texture in Photoshop then to use Unity’s compression options when the in game texture resolution is already known. This reduces the file size and import time of the texture into Unity.
* When working with a high-resolution source PSD outside your Unity project use the same name for both the high-resolution and the imported Unity file. This allows quick iteration when swapping between the 2 textures.

More information for importing textures can be found here: [https://docs.unity3d.com/Manual/ImportingTextures.html](https://docs.unity3d.com/Manual/ImportingTextures.html)

Textures requiring the use of a Alpha channel should follow this guide: [https://docs.unity3d.com/Manual/HOWTO-alphamaps.html](https://docs.unity3d.com/Manual/HOWTO-alphamaps.html)

##### Texture File Format

All textures should be of the .PSD format. No layers should be included and only one Alpha channel in the imported file.

**[⬆ Back to Top](#table-of-contents)**

<a name="audio"></a>
### 4.3 Audio

Only import uncompressed audio files in to Unity using WAV or AIFF formats.

Great guide on [Unity Audio Import Optimization](https://www.gamasutra.com/blogs/ZanderHulme/20190107/333794/Unity_Audio_Import_Optimisation__getting_more_BAM_for_your_RAM.php)

**[⬆ Back to Top](#table-of-contents)**

<a name="sprite"></a>
### 4.4 Sprite

All the UI elements should be separated as a panel into sprite atlases.

For example, all the sprites associated with the Inventory panel must be in the Inventory Panel Atlas(Tex_UIAtlas_Inventory). The atlas must include all the sprites that belong to the panel. That means every single sprite in that atlas will be shown on that specific panel, not somewhere else. There are some exceptions, in the inventory example, we can assume there will be hundreds of items. So, we can split items in different atlases but, we need to split them by types. For example, Tex_UIAtlas_ItemAtlas_Food_01, Tex_UIAtlas_ItemAtlas_Food_02, Tex_UIAtlas_ItemAtlas_Food_03

**[⬆ Back to Top](#table-of-contents)**


## 5. How to use Git

This section describes how we are going to use git and repos. First of all we are going to follow [Conventional Commits specification](https://www.conventionalcommits.org/tr/v1.0.0/#%C3%B6zet).

Other than that, we are going to use GitHub. We may decide to use PlasticSCM later if we decide to use Unity's Cloud building solution. There is another rule about committing changes that are not included in the Conventional Commits specifications; we will going to use the type "imp"(import) to push our asset import commits. For example;

```imp: Atmospheric House asset part 1``` 
```imp: Atmospheric House asset part 2``` 


That way, we can split the asset imports into parts and ensure everyone knows we just imported an asset to the project. Also, because of the rules of git, we can't push too many objects at once. With this method, we can split the importing process into parts to walk around the limits(some sort of).

Every developer will have their own branches; they can manage their commits and work in that branch till it’s ready to push it to the related branch, it can be the main branch, or it can be a feature branch. In that way, developers can push their changes to the git and keep working on another PC, or in the end, they can save their progress to the server to be safe from any loss.
