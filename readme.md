# Coding rules and ideas

This article contains ideas and rules for setting up a project, from structure to naming conventions for scripts and assets.

## Table of Contents

> 1. [Project Structure](#project-structure)
> 1. [Scripts](#scripts)
> 1. [Asset Naming Conventions](#asset-naming-conventions)
> 1. [Asset Workflows](#asset-workflows)
> 1. [Version Control / Git](#version-control)


## 1. Project Structure
The directory structure style of a project should be considered law. Asset naming conventions and content directory structure go hand in hand, and violating either causes unneeded chaos.

```
Assets
    _Developers (Use a `_` to keep this folder at the top)
        DeveloperName
            (Work in progress assets)
    _ProjectName (Use a `_` to keep this folder at the top)
	    +Art
		    - 3D
			    -- Animation
				    ---- Character
					    ++++ Arthas
					    ++++ Humanoids
					    ++++ Survivals (can be more spesific)
			    -- Models
				    --- Guns
					    ---- Kriss Vector
						    +++++ Textures
						    +++++ Prefab
						    ----- etc...
					    ++++ M4
					    ++++ AK47
		    - 2D
			    -- UI
				    --- Ingame
					    ---- Tex_Atlas_CharacterCustomization.png    
					    (If there are so many atlases, there can be more folders like Trade, Arena, etc.)
				    +++ Lobby				    
		    - VFX
			    -- Ingame(Category) (If there are so many prefabs in different categories if the total of 5-10 VFXes, there is no need to create this folder)
				    --- Guns
					    ---- Kriss Vector
						    +++++ Prefabs
						    +++++ Textures
						    +++++ Materials
				-- Lobby
					+++ Prefabs
					+++ Materials
					+++ Textures
				-- Common
					--- Same as above.
			- Materials
			- Textures
		+ Audio (this folder has the same folder rules above)
			- SFX
				-- Ingame
					--- Characters
						++++ Male
						++++ Female
						++++ (Or more spesific like Arthas, Illidan)
				-- Lobby
			- Music (If there are so many prefabs in different categories, if there is a total of 5-10 of music, there is no need to create sub-category folders)
				-- Ingame 
				-- Lobby
		+ Code
			- ECS/DOTS
				-- Authoring
				-- Data
				-- System
					+++ Character (sub-folder)
					+++ Minion (sub-folder)
					+++ Zombie (sub-folder)
					--- ... .cs
					--- ... .cs
			- Scripts
				-- Abilities
				-- Data
				-- Dump
				-- Interfaces
				-- Managers
				-- SO (scriptable objects)
				-- UI
					--- Data
					--- Groups
					--- Prefabs
		+ Editor
		+ Prefabs
			- Entities
			- Managers
			- UI
			- Zombies/Characters (If there are so many objects, they need to be split. for example; Characters -> Zombies or Characters -> Female -> Zombies)
		+ Scenes (If there aren't so many scenes, there is no need to be split. The Scenes folder will be enough)
			- Subscenes
			- Levels, etc.
		+ Shaders
	+ScriptableObjects(as data instance, not script)
    +Plugins(ThirdPartySDK, just the tools(not visual assets))
    +Imports(This folder is for bulk visual asset(not tools) importing)
    +Resources (just the objects need to load with Resources Load. This folder has the same folder rules above)
    +DLCs/Mini-Games/extras
```




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


### 1.1 Folder Names
These are common rules for naming any folder in the content structure.

#### Always Use [PascalCase](#terms-cases)
PascalCase refers to starting a name with a capital letter and then starting every following word with a capital letter instead of using spaces. For example, `DesertEagle`, `RocketPistol`, and `ASeriesOfWords`.

#### Never Use Spaces
Never use spaces. Spaces can cause various engineering tools and batch processes to fail. Ideally, your project's root also contains no spaces and is located somewhere such as `D:\Project` instead of `C:\Users\My Name\My Documents\Unity Projects`.

#### Never Use Unicode Characters And Other Symbols
If one of your game characters is named 'Zoë', its folder name should be `Zoe`. Unicode characters can be worse than [Spaces](#1.1.2) for engineering tools, and some parts applications don't support Unicode characters in paths either.

Related to this, if your project and your computer's user name have a Unicode character (i.e., your name is `Zoë`), any project located in your `My Documents` folder will suffer from this issue. Simply moving your project to something like `D:\Project` will often fix these mysterious issues.

Using other characters outside `a-z`, `A-Z`, and `0-9`, such as `@`, `-`, `_`, `,`, `*`, and `#`, can also lead to unexpected and difficult-to-track issues on other platforms, source control, and weaker engineering tools. 

### 1.2 Use A Top Level Folder For Project Specific Assets
All of a project's assets should exist in a folder named after the project. For example, if your project is named 'Generic Shooter', _all_ of it's content should exist in `Assets/GenericShooter`.

> The `Developers` folder is not for assets your project relies on and, therefore, is not project-specific. See [Developer Folders](#use-developers-folder-for-local-testing) for details about this.

There are multiple reasons for this approach.

#### No Global Assets
When assets are allowed to exist outside of a project folder, enforcing a strict structure layout often becomes much more complicated, as assets not in a folder encourage the bad behavior of not having to organize them.

Every asset should have a purpose; otherwise, it does not belong to a project. If an asset is an experimental test the project shouldn't use, it should be put in a [`Developer`](#use-developers-folder-for-local-testing) folder.

#### Samples, Templates, and 3rd Party Content Are Risk-Free
Suppose a team member decides to add sample content, template files, or assets they bought from a 3rd party. In that case, it is guaranteed that these new assets will not interfere with the project in any way unless your project's top-level folder is uniquely named.

You can not fully trust third-party content to conform to the top-level folder rule. Many assets have the majority of their content in a top-level folder but possibly modified Unity sample content and level files polluting the global `Assets` folder.

We should also use Assembly definitions for third-party content, scripts, and plugins. If those third-party assets don't have Assembly definitions, the developer who imports them is responsible for creating one.

#### DLC, Mini-Games, and Patches Are Easily Maintained
DLC or multiple sub-projects associated with the project that may either be migrated out or not cooked in a build, assets relating to these projects should have their own separate top-level content folder. This makes cooking DLC separate from main project content far more manageable. Sub-projects can also be migrated in and out with minimal effort. Suppose you need to change a material of an asset or add some particular asset override behavior in a patch. In that case, you can quickly put these changes in a patch folder and work safely without the chance of breaking the core project.


### 1.3 Use Developers Folder For Local Testing
During a project's development, it is very common for team members to have a sort of 'sandbox' where they can experiment freely without risking the core project. Because this work may be ongoing, these team members may wish to put their assets on a project's source control server. Not all teams require the use of Developer folders, but those that use them often run into a common problem with assets submitted to source control.

It is very easy for a team member to accidentally use assets that are not ready for use, which will cause issues once those assets are removed. For example, an artist may be iterating on a modular set of static meshes and still working on correcting their sizing and grid snapping. If a world builder sees these assets in the main project folder, they might use them all over a level, not knowing they could be subject to incredible change and/or removal. This causes massive amounts of re-working by everyone on the team to resolve the issue.

If these modular assets had been placed in a Developer folder, the world builder would never have had a reason to use them, and the whole issue would never have happened.

Once the assets are ready for use, an artist simply has to move the assets into the project-specific folder. This is essentially 'promoting' the assets from experimental to production.

### 1.4 All Scene Files Belong In A Folder Called Scenes
Scene files are incredibly special, and it is common for every project to have its own map naming system, especially if it works with sub-scenes or streaming scenes. No matter what system of map organization is in place for the specific project, all scenes should belong in `Assets/ProjectNameName/Scenes`.

Being able to tell someone to open a specific map without explaining where it is is a great time saver and general 'quality of life' improvement. It is common for scenes to be within sub-folders of `Scenes`, such as `Scenes/Campaign1/` or `Scenes/Arenas`, but the most important thing here is that they all exist within `Assets/ProjectNameName/Scenes`.

This also simplifies the job of cooking for engineers. Wrangling scenes for a build process can be extremely frustrating if they have to dig through arbitrary folders for them. If a team's scenes are all in one place, it is much harder to accidentally not cook a map in a build. It also simplifies lighting build scripts and QA processes.

### 1.5 Ownership
Define ownership of zone/assets/features. Some assets, like scenes or prefabs, do not handle simultaneous changes by multiple people very well, creating conflict. Having a single person who can change (or give the right to change) a given asset helps to avoid that problem. Always work on your developer folder, which needs to have its own Assembly Definition. Create a variant of the scenes for yourself to work on without changing the original one. While working on a general system/service, stay in contact with your team and make sure no one else is doing the same thing on that system/service.

### 1.6 Material Library

Suppose there are master materials, layered materials, or any reusable materials or textures that do not belong to any subset of assets. In that case, these assets should be located in `Assets/ProjectName/Art/Materials`.

This way, all 'global' materials will have a place to live and will be easily located.

Any testing or debug materials should be within `Materials/Debug`. This allows debug materials to be easily stripped from a project before shipping and makes it incredibly apparent if production assets are using them if reference errors are shown.

### 1.7 Scene Structure
Next to the project's Hierarchy, there's also scene hierarchy. As before, we'll present you with a template. You can adjust it to your needs. Use named empty game objects as scene folders. Be careful with it; do not use dynamic objects in a folder object. It will cause performance issues. Dynamic objects should be parent-free in the scene. Use the names exactly as they are here.
```
---> This is a special tag for the Hierarchy2 plugin. It makes the folder objects look a bit better in the Hierarchy. 
There is no need to use it if the project doesn't have that plugin.
```

```
--->SubScenes (it's not a folder object, it's just a brace to make the scene more organized)
--->Managers
	-- All the managers/singletons
--->Scene
	++ Cameras
	++ World
		+++ Lights
		+++ Terrains
		+++ Props
		+++ Volumes
		+++ Static objects, like Houses or zones
	++ Gameplay
		+++ Actors
		+++ Items
--->UI
--->Dynamic  (it's not a folder object, it's just a brace to make the scene more organized)
```

An example is in the screenshot: Dynamic and Subscene objects are just pins, not folder objects, used to separate/organize objects.

[Example screenshot](https://prnt.sc/fdf_tb2mY-zz)

 - All empty objects should be located at 0,0,0 with default rotation and scale.
 - Do not populate the "--->Dynamic" object when instantiating an object in runtime.

**[⬆ Back to Top](#table-of-contents)**


## 2. Scripts

This section will focus on C# classes and their internals. When possible, style rules conform to Microsoft's C# standard. The most important rule in this section is all the names can be long, but **must be unambiguous and descriptive.**


### Sections
> 2.1 [Class Organization](#classorganization)

> 2.2 [Compiling](#compiling)

> 2.3 [Variables](#variables)

> 2.4 [Functions](#functions)

<a name="classorganization"></a>
### 2.1 Class Organization

 1. Source files should contain only one public type, although multiple internal classes are allowed. 
 2. Source files should be given the name of the public class in the file.        
      
Class members should be alphabetized and grouped into sections:

* `Enums, Structs, Constructors, and etc. (smaller to bigger)`
* `Constant Fields`
* `Static Fields`
* `Events, Delegates, Actions`
* `Properties`
* `Fields`
* `LifeCycle Methods` (Awake, OnEnable, OnDisable, OnDestroy), Use them in execution order.
* `Public Methods` (Methods that return values must be grouped together in the related scope)
* `Private Methods`
* `Public Listeners`
* `Private Listeners`

Within each of these groups, order by access:

* `public`
* `internal`
* `protected`
* `private`

```
public class temp : MonoBehaviour
{
    /* ------------------------------------------ */
    
	/// <summary>  
    /// Public and Serializable variables should have a summary and tooltip
    /// </summary>
    [Tooltip("Public variables set in the Inspector, should have a Tooltip")]
    public const string ConnectionString = "databaseConnectionLink";
    public static string ConnectionString = "databaseConnectionLink";
    
    //Public & Serializefield variables are in the same category
    #region Public-Serializable-Variables

	public delegate void DayChangedEventHandler(object sender, DateTime e);
    public event DayChangedEventHandler DayChangedEvent;  
    
    public string ConnectionString { get; private set; }
    [Serializefield] string ConnectionString = "databaseConnectionLink";

    #endregion

    /* ------------------------------------------ */

    #region Internal-Variables

    internal const string ConnectionString = "databaseConnectionLink";
    internal static string ConnectionString = "databaseConnectionLink";
    
	internal delegate void DayChangedEventHandler(object sender, DateTime e);
    internal event DayChangedEventHandler DayChangedEvent;
    
    internal string ConnectionString = "databaseConnectionLink";
    internal string ConnectionString { get; set; }    

    #endregion

    /* ------------------------------------------ */

    #region Protected-Variables

    protected const string _connectionString = "databaseConnectionLink";
    protected static string _connectionString = "databaseConnectionLink";
    
    protected delegate void _dayChangedEventHandler(object sender, DateTime e);
    protected event _dayChangedEventHandler DayChangedEvent;
    
    protected string _connectionString = "databaseConnectionLink";
    protected string _connectionString { get; set; }
    
    #endregion

    /* ------------------------------------------ */

    #region Private-Variables

    private const string _connectionString = "databaseConnectionLink";
    private static string _connectionString = "databaseConnectionLink";
    
    private delegate void _dayChangedEventHandler(object sender, DateTime e);
    private event _dayChangedEventHandler DayChangedEvent;
    
    private string _connectionString = "databaseConnectionLink";
    private string _connectionString { get; set; }
    
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

}
```


#### All Public Functions Should Have A Summary

Any function with an access modifier of Public should have its summary filled out.

```
/// <summary>
/// This method is responsible for spawning projectile and calculating the next fire time.
/// </summary>
public void Fire()
{
	// Some logic here (with their short comments, if it's complex).
}
```


#### Commenting
Comments should be used to describe intention, algorithmic overview, and/or logical flow.
It would be ideal if someone other than the author could understand a function’s intended behavior and general operation by reading the comments alone.

While there are no minimum comment requirements, and certainly some very small routines need no commenting at all, it is hoped that most routines will have comments reflecting the programmer’s intent and approach.

##### Comment Style
Place the comment on a separate line, not at the end of a line of code.

Begin comment text with an uppercase letter.

End comment text with a period.

Insert one space between the comment delimiter (//) and the comment text, as shown in the following example.

The // (two slashes) style of comment tags should be used in most situations. Wherever possible, place comments above the code instead of beside it. Here is an example:
```
        // Sample comment above a variable.
        private int _myInt = 5;
```

#### Regions
The `#region` directive enables you to collapse and hide sections of code in C# files. The ability to hide code selectively makes your files more manageable and easier to read. Use it to organize the code/logic into categories such as Back-end, Front-end, LifeCycle, Public, and Private Methods.

```
#region "This is the code to be collapsed"
    private System.ComponentModel.Container Container
#endregion
```

#### Spacing
Do use a single space after a comma between function arguments.

Example: `Console.In.Read(myChar, 0, 1);`
* Do not use a space after the parenthesis and function arguments.
* Do not use spaces between a function name and parenthesis.
* Do not use spaces inside brackets.

#### Curly Brackets
One-line operations in loops or condition checks such as if, while, for, or foreach must be brackets-free. If you know there will be more code later on (at most, there will be a few commits in the same feature update, not two later), it may be acceptable, but it's not in most cases. Keep the code clean as much as possible.

**Bad**

```
if (IsServer)
{  
	StartServer();  
}
```

**Good**

```
if (IsServer)
	StartServer();  
```

<a name="compiling"></a>
### 2.2 Compiling
All scripts should compile with zero warnings and zero errors. You should fix script warnings and errors immediately, as they can quickly cascade into very scary, unexpected behavior.

Do *not* submit broken scripts to the feature's branch. If you must store them on source control, shelve them or submit them to your private branch if you have one.



<a name="variables"></a>
### 2.3 Variables
The words `variable` and `property` may be used interchangeably.

#### Variable Naming
##### Turk's notation
I want to start with the notation that I designed. It looks like Hungarian notation because both notations have similar intentions. Let's see an example first, then talk about it more. 
```
// Correct
private int _counter;
private string _name;
 
// Avoid
private int iCounter;
private string strName;

// Correct
[SerializeField] private Text Txt_Nickname;
[SerializeField] private Button Btn_Play;
 
// Avoid
(SerializeField also follows Public access level naming conventions because they can be changed/referenced outside the scope.)
[SerializeField] private Text TNickname || TextNickname || T_Nickname || Text_Nickname;
[SerializeField] private Button ButtonPlay;
```
In the example, you see the Hungarian notation as Avoid example here. The main crossroads is that both notations do not give prefixes everywhere. If the property is a primitive data type such as int, string, or float, we do not give it prefixes or suffixes. 

In most cases, Functions/Methods, Properties, and Variables are excluded, but in some cases, we may use the notation for naming variables or methods. Anything can be split into parts by its core, such as a folder, data type, job type, owner, etc. Other than that, any type of data, system, asset, or folder structure is subject to this notation if it can be split as explained above. Let's check another example here;

**Bad**

```
// You can't store this struct in the class because of the name duplication. 
// You have to think more here about naming. So, you have two problems here. 
// One, you can't place things in the right places.
// Second, you have to spend more time on naming.
public struct PlayerData
{
	public string Str_Nickname;
	public int Int_Score;
	public Sprite ProfilePicture; // <-- Pay attention here.
}

public class Player
{
	public PlayerData PlayerData;
	// OR
	public PlayerData Data_Player;

	public Image ProfilePicture; // <-- Also, pay attention here. Name duplication. This doesn't occur in our notation.
	
	private PlayerData[] OpponentsData;
}
```

**Good**

```
public class Player
{
	public struct Data
	{
		public string Nickname;
		public int Score;

		public Sprite Spr_ProfilePicture; // <-- Pay attention here.
	}

	public Data Data;
	public Image Img_ProfilePicture;  // <-- There is no name duplication here, as we expect.
	
	private Data_Player[] _data_Opponents;
}
```
In the code above, we saw two different approach which occur problems to us, that's the reason I designed this notation. First, what if we don't use the notation and second what if we use it in wrong way.

The idea behind this notation is to reduce the effort lost in naming classes, properties, and any type of data and recalling those names while coding, most of which are written by other developers. This notation makes the developer use primitive names instead of giving some nicknames that make sense to them. Of course, no one can stop a developer with bad intentions, but this makes the developer focus on the logic rather than the file and data names(after the learning curve).

More information about deciding prefixes and suffixes [check here](#asset-naming-conventions).


##### Naming Conventions

Naming convention rules by access modifiers:

* `public` -> `UpperCamelCase` -> `ConnectionString`

* `internal` -> `UpperCamelCase` -> `ConnectionString`

* `protected` -> `"_" + lowerCamelCase` -> `_connectionString`

* `private` -> `"_" + lowerCamelCase` -> `_connectionString`

* `local` -> `lowerCamelCase` -> `connectionString`

##### Considered Context

All variable names must not be redundant with their context, as all variable references in the class will always have context.

##### Considered Context Examples:

Consider a Class called `PlayerCharacter`.

**Bad**

* `PlayerScore`

* `PlayerKills`

* `MyTargetPlayer`

* `MyCharacterName`

* `CharacterSkills`

* `ChosenCharacterSkin`

* `MovSpeed`

All of these variables are named redundantly. It is implied that each variable is representative of the `PlayerCharacter` it belongs to because it is `PlayerCharacter` that defines these variables.

**Good**

* `Score`

* `Kills`

* `Target` || `CurrentTarget`

* `Name`

* `Skills`

* `Skin` || `CurrentSkin`

* `MovementSpeed`

#### Variable Access Level

In C#, variables have a concept of access level. Public means any code outside the class can access the variable. Protected means only the class and any child classes can access this variable internally. Private means only this class and no child classes can access this variable. In the ideal case, we won't let any data public access, so keep this in mind while designing any architecture and make sure to keep the data private, internal, and protected as much as possible. Give the public get/read access to the data with wrappers, properties, and methods.

**Variables should only be made public if necessary. Prefer using the attribute `[SerializeField]` instead of making a variable public.**

##### Local Variables

Define the variable with the type instead of using var where possible.

**Bad**

```

var var1 = "Some string.";

var var2 = 27;

var var3 = ExampleClass.ResultSoFar();

for (var i = 0; i < bountyHunterFleets.Length; ++i) {}

```

**Good**

```

string var1 = "Some string.";

int var2 = 27;

int var3 = ExampleClass.ResultSoFar();

for (int i = 0; i < bountyHunterFleets.Length; ++i) {}

```

<a name="publicvariables"></a>

##### Public Variables

The value of a class will not be changed by any class and will not be assigned. If the data needs to be updated, the class will do it, and public data will only be used for reference. The changes will be made using the class's methods. Each class is responsible for updating its data and references. For example;

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

#### Variables accessible in the Editor

##### Tooltips

All Serializable variables should have a description in their `[Tooltip]` fields that explains how changing this value affects the script's behavior.

##### Variable Slider And Value Ranges

All Serializable variables should use slider and value ranges if there is a value that a variable should _not_ be set to.

**Example:** A script that generates fence posts might have an editable variable named `PostsCount` and a value of -1 would not make any sense. Use the range fields `[Range(min, max)]` to mark 0 as a minimum.

Suppose an editable variable is used in a Construction Script. In that case, a reasonable Slider Range should be defined so that someone can not accidentally assign it a large value that could crash the editor.

A Value Range only needs to be defined if the bounds of a value are known. While a Slider Range prevents accidental large number inputs, an undefined Value Range allows a user to specify a value outside the Slider Range that may be considered 'dangerous' but still valid.


#### Variable Types

##### Booleans

###### Boolean Prefix
All booleans should be named in PascalCase but prefixed with a verb.

Example: Use `isDead` and `hasItem`, **not** `Dead` and `Item`.

###### Boolean Names
All booleans should be named as descriptive adjectives to represent general information when possible.

###### Boolean Complex States
Do not use booleans to represent complex and/or dependent states. This makes state adding and removing complex and no longer easily readable. Use an enumeration instead.

Example: When defining a weapon, do **not** use `isReloading` and `isEquipping` if a weapon can't be both reloading and equipping. Define an enumeration named `WeaponState` and use a variable with this type called `WeaponState` instead. This makes it far easier to add new states to weapons.

##### Enums
Enums use PascalCase with the "Enum" prefix and singular names for enums and their values. Exception: bit field enums should be plural. Enums can be placed outside the class space to provide global access. Enums always have an invalid value, such as "None" or "Invalid".

Example: 
```
public class Weapon
{
	public enum Enum_WeaponType
	{
		None,
	    Knife,
	    Gun
	}

	// Enum can have multiple values
	[Flags]
	public enum Enum_Dockings
	{
		None = 0,
		Top = 1,
	}
	
	public Enum_WeaponType Type
}
```

##### Arrays
Arrays follow the same naming rules as above but should be named as a plural noun.

Example: Use `Targets`, `Hats`, and `EnemyPlayers`, not `TargetList`, `HatArray`, `EnemyPlayerArray`.

##### Interfaces
Interfaces are led with a capital `I` followed by PascalCase.

Example: ```public interface ICanEat { }```


<a name="functions"></a>
### 2.4 Functions, Events, and Event Dispatchers
This section describes how you should author functions, events, and event dispatchers. Everything that applies to functions also applies to events unless otherwise noted.

#### Function Naming
The naming of functions, events, and event dispatchers is critically important. Based on the name alone, certain assumptions can be made about functions. For example:

* Is it a pure function?
* Is it fetching state information?
* Is it a handler?
* What is its purpose?

These questions and more can all be answered when functions are named appropriately. 

##### Local Functions/Methods
There is another rule for Local functions/methods: every local method, meaning the methods introduced in a method (method-in-method) must have a "Local_" prefix. For example;
```
public void Eat()
{
	if(CanEat())
	// Eat

	bool Local_CanEat()
	{
		...
		return true;
	}
}

// OR

public void Shoot()
{	
	...
	// Logic
	Local_SaveAmmo();

	void Local_SaveAmmo()
	{
		...
	}
}
```


<a name="function-verbrule"></a>
#### All Functions Should Be Verbs
All functions and events perform some action, whether getting info, calculating data, or causing something to explode. Therefore, all functions should start with verbs. They should be worded in the present tense whenever possible. They should also have some context as to what they are doing.

Good examples:

* `Fire` - This is a good example in a Weapon class, as it has context. Bad if in a Barrel / Grass / any ambiguous class.
* `Jump` - Good example if in a Character class, otherwise, needs context.
* `Explode`
* `ReceiveMessage`
* `SortTargets`
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
* `Color` - Verb with no context or ambiguous noun.


#### Functions Returning Bool Should Ask Questions
When writing a function that does not change the state of or modify any object and is purely for getting information, state, or computing a yes/no value, it should ask a question. This should also follow [the verb rule](#all-functions-should-be-verbs).

This is extremely important because if a question is not asked, it may be assumed that the function performs an action and is returning whether that action succeeded.

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

* `Fire` - Is it on fire? Will fire? Do fire?
* `OnFire` - Can be confused with event dispatcher for firing.
* `Dead` - Is dead? Will deaden?
* `Visibility` - Is it visible? Set visibility? A description of flying conditions?

#### Functions Responsible For Actions of UI Elements
When using regular functions for the UI elements, it becomes harder to handle them in debugging. So, that's why we are using some special tags for them. In that way, we assume that the function references are made by hand by the inspector. That's why we should carefully inspect that function and expect some independent triggers besides the references we found on the IDE.

Example for Buttons:

* `FunBuy` ->  `FunX`

Example for Inputfields:

* `OnInputNicknameValueChanged` -> `OnInputXValueChanged`

Example for Dropdowns:

* `OnDrpNicknameValueChanged` -> `OnDrpXValueChanged`

Example for Toggles:

* `OnTgNicknameValueChanged` -> `OnTgXValueChanged`


#### Event Handlers and Dispatchers Should Start With `On`
Any function that handles an event or dispatches an event should start with `On` and continue to follow [the verb rule](#all-functions-should-be-verbs).

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

Any kind of finding method is forbidden; stay away from it. Make sure you assign all dependencies by hand in the inspector or with constructor methods for non-monobehaviour classes. You can use the ContextMenu attribute if you want. ContextMenu methods must be placed in the region called "Helpers".


#### Use "For" Instead of "Foreach"

Avoid using `foreach` for loops; it's more expensive than `for` and may create garbage.

#### Language Integrated Query (LINQ)

Linq is a powerful way to create queries but can be expensive. Be careful with Linq; don't use it in loops or repetitive methods.

**[⬆ Back to Top](#table-of-contents)**


### 2.5 Avoid Events(Delegates), Adopt Observer Pattern
Avoid using any type of delegate wherever possible; use observer patterns as simple or complex solutions. An observer pattern acts like a delegate but uses a different approach, which gives you a performance boost and flexibility. In the observer approach, you call a method instead of triggering a delegate, which is faster ~10 times. How much faster is up to the developer. If the developer implements too many functions around it, it may act worse, so be careful what you build around it.

### 2.6 Debugging
All debugging should be strapped/wrapped with a tool and not leave the development stage. The main branch builds should not contain any data that may be sensitive data debugging related to security reasons.

You can use priority-based logging. That way, you can leave the high-level logs out of the live version builds to prevent hackers and people with bad intentions from having more information about your data and product.

### 2.7 Profiling
#### How and Why to Use ProfilerMarkers
ProfilerMarker is a powerful tool that allows you to measure various methods, but it also introduces security risks. Therefore, ensure that you strip it from main/live builds. ProfilerMarker is a more efficient alternative to Deep Profiler. Deep Profiler can add significant overhead to debugging because it operates within the entire project scope. On the other hand, with ProfilerMarker, you can selectively "deep profile" only where necessary.

### 2.8 Coroutines, Tasks, Async Operations
We'll briefly discuss Coroutines and async operations here. As we all know, game development loops and async operations are often used. Of course, we will use them in projects, but instead of Coroutines, we'll use Tasks for loops and async operations. 

#### 2.8.1 UniTask

UniTask is a powerful tool developed for Unity to maximize the benefits of C#'s Tasks. Although garbage-free, if it's misused, it can create garbage, like almost anything in the language. So, be careful if you try to pull up a dark magic with it. :)

You can multi-thread programming with this library if you know what you're doing. Keep in mind that you can't use managed codes in a multi-thread environment.

Each class should have its CancellationToken sources, and there can be more than one if needed. They must also have a cancellation method that can be accessed from the public access level and must be inherited from a base class or an interface.

**[⬆ Back to Top](#table-of-contents)**

### 2.9 Principles and Patterns
If you are reading this TDD, I'm sure you know the fundamental principles. In case you don't, let's discuss them briefly.

#### 2.9.1 SOLID Principles
SOLID is an acronym for the first five object-oriented design (OOD) principles by Robert C. Martin (also known as Uncle Bob).

 - **Single Responsibility**: A class or module should have **only one responsibility**. It should handle only one piece of functionality, and changes in the functionality should affect only that class. This makes the code easier to understand and maintain.
-   **Open-Closed principle**: Classes should be **open for extension but closed for modification**. Instead of changing existing classes to add new behavior, extend them to implement the new functionality. This minimizes the risk of breaking existing code.
-   **Liskov substitution principle**: Subclasses should be **replaceable for their base classes** without altering the correctness of the program. A subclass must retain the behavior and properties of its superclass, ensuring compatibility.
-   **Interface segregation**: A class should not be forced to depend on interfaces it does not use. Instead, create **smaller, more specific interfaces**. This ensures that classes are only dependent on the functionalities they need.
-   **Dependency Inversion**: High-level modules should not depend on low-level modules. Both should depend on abstractions. This makes the code more modular and flexible, allowing for easier updates and changes in dependencies.

#### 2.9.2 DRY Principles
DRY is also an acronym for "Don't Repeat Yourself". As the title suggests, the goal is to avoid code duplication. If more than one system uses the same code, we can create a related Utils class and move the code to it. If the code is in one class but needs to be run in multiple methods, we can move it to a separate method to use anywhere in the class.

#### 2.9.3 KISS Principles
Like the other two principles, KISS is also an acronym for "Keep It Simple, Stupid!" As the title suggests, the goal is to keep the code simple. Albert Einstein once said, “If you can’t explain it, you don’t understand it well enough.” Keep it simple; separate the logic until it is simple enough.


### 2.10 Dependency Injection
I almost hear you saying, "What about dependency injection?". Nope, we don't do that here. 
Joke aside, you don't need any DI frameworks if your architecture is valid. So, we do not welcome DI.

### 2.11 Unit Test
Every code used in a loop or continuous usage should have unit tests. The developer must be sure that the code or logic has good performance and zero garbage. Leads should be able to test and see those algorithms unit test when they want because they might want to know the performance of those codes at any time, especially before accepting the PR.

### 2.12 Dictionary and HashMaps
You should be careful while using hashmaps to avoid garbages and boxing. For example, using Enums as a Key for a container creates garbage because of object boxing, so be careful. Another example of false implementation to this is the structs and custom data types; if you are going to use custom data as a Key, you should implement an IEquatable interface to the data to avoid performance costs.

### 2.13 Avoid List, LinkedList
Avoid List, LinkedList, and similar containers as much as possible. Adopt and learn how to write "Cache-Friendly Code". The nature of List and similar containers is that they store the data split in memory, and that's not what we want to get most of the performance. So, use Array and other similar containers that suit your needs. This is not a tutorial, so I won't go into full detail about how to use Arrays and create similar containers here.

It's not forbidden, so if you need to use it, go for it. However, if you have any information about the data, do not forget to initialize it with a capacity.

**[⬆ Back to Top](#table-of-contents)**

### 2.14 Namespace
We'll use namespaces, and classes without a namespace will not be able to pass a PR. Namespace naming should follow the [project structure](#project-structure). For example;

```
namespace _Project_X.Code.ECS.Server.Systems {}
namespace _Project_X.Code.ECS.Common.Systems {}
namespace _Project_X.Code.Scripts.Managers {}
namespace _Project_X.Code.Scripts.Abilities {}
namespace _Project_X.Code.Scripts.Components {}
namespace _Project_X.Code.Scripts.Data {}
```
### 2.15 Assembly Definition
We'll use Assembly Definitions for both the game system and the plugins that we imported. There will be assets that don't have Assembly Definitions, but they should. [Check here](#samples-templates-and-3rd-party-content-are-risk-free) what to do in that situation. For our structure, we will have three Assembly Definitions for ECS, Scripts, and Editor scripts, if we have them.

### 2.16 Unsafe code
Unsafe codes are forbidden because the developers who wrote them must know what they are doing. In most cases, unsafe codes cause trouble. So, if you are going to use them, they must be supervised. The leads must investigate every change and implementation closely.


## 3. Asset Naming Conventions
Naming conventions should be treated as law. A project that conforms to a naming convention can manage, search, parse, and maintain its assets with incredible ease.

Most things are prefixed with the prefix generally being an acronym of the asset type followed by an underscore.

**Assets use [PascalCase](#cases)**

<a name="base-asset-name"></a>
<a name="3.1"></a>
### 3.1 Base Asset Name
`Prefix_BaseAssetName_Variant_Suffix`

All assets should have a _Base Asset Name_. A Base Asset Name represents a logical grouping of related assets. Any asset part of this logical group should follow the standard of  `Prefix_BaseAssetName_Variant_Suffix`.

Keeping the pattern `Prefix_BaseAssetName_Variant_Suffix` in mind and using common sense is generally enough to warrant good asset names. Here are some detailed rules regarding each element.

`Prefix` and `Suffix` are to be determined by the asset type through the following [Asset Name Modifiers](#asset-name-modifiers) tables.

`BaseAssetName` should be determined by a short and easily recognizable name related to the context of this group of assets. For example, if you had a character named Bob, all of Bob's assets would have the `BaseAssetName` of `Bob`.

For unique and specific variations of assets, `Variant` is a short and easily recognizable name representing a logical grouping of assets that are a subset of an asset's base name. For example, if Bob had multiple skins, these skins should still use `Bob` as the `BaseAssetName` but include a recognizable `Variant`. An 'Evil' skin would be referred to as `Bob_Evil` and a 'Retro' skin would be referred to as `Bob_Retro`.

For unique but generic variations of assets, `Variant` is a two-digit number starting at `01`. For example, if an environment artist generates nondescript rocks, they would be named `Rock_01`, `Rock_02`, `Rock_03`, etc. Except for rare exceptions, you should never require a three-digit variant number. If you have over 100 assets, you should consider organizing them with different base names or using multiple variant names.

Depending on how your asset variants are made, you can chain together variant names. For example, if you are creating flooring assets for an Arch Viz project, you should use the base name `Flooring` with chained variants such as `Flooring_Marble_01`, `Flooring_Maple_01`, `Flooring_Tile_Squares_01`.

If the file type consists of a single word, you need to use the first three letters of it like in the examples;
*(Three letters rule may extend for some cases, such as "Pref" for prefabs.)*

| Asset Type               | Asset Name   |
| ------------------------ | ------------ |
| Audio            | Aud_M4_Shoot       |
| Material                 | Mat_Bob_Standard_Head        |
| Texture (Diffuse/Albedo) | Tex_Bob_Standard_D      |
| Texture (Normal)         | Tex_Bob_Standard_N      |
| Texture (Evil Diffuse)   | Tex_Bob_Evil_D |


If it consists of multiple words, you need to use its first letters. For example;


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
<a name="3.2"></a>
### 3.2 Asset Name Modifiers

When naming an asset, use these tables to determine the prefix and suffix to use with an asset's [Base Asset Name](#base-asset-name).

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
| Prefab                  | Pref_       |            |                                  |
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

| Asset Type    | Prefix | Suffix      | Notes                                   |
| ------------- | ------ | ----------- | --------------------------------------- |
| Mesh          | Mes_       | _LodX* | Only use LOD suffix if model uses LOD's |
| Mesh Collider | MC_       |    |                                         |

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
| Prefab         | Pref_       |            |                                  |
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
| Texture (Diffuse/Albedo/Base Color)| Tex_ | _Albedo      |                                  |
| Texture (Normal)        | Tex_         | _Normal         |                                  |
| Texture (Roughness)     | Tex_         | _Roughness         |                                  |
| Texture (Alpha/Opacity) | Tex_         | _Alpha         |                                  |
| Texture (Ambient Occlusion) | Tex_     | _AO      |                                  |
| Texture (Bump)          | Tex_         | _Bump         |                                  |
| Texture (Emissive)      | Tex_         | _Emissive         |                                  |
| Texture (Mask)          | Tex_         | _Mask         |                                  |
| Texture (Specular)      | Tex_         | _Specular         |                                  |
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
| Texture (Sprite atlas) | Tex_     |    |       |

<a name="anc-effects"></a>
#### 3.2.11 Effects
| Asset Type      | Prefix | Suffix | Notes |
| --------------- | ------ | ------ | ----- |
| Particle System | PS_    |        |       |
**[⬆ Back to Top](#table-of-contents)**


<a name="assetworkflows"></a>

## 4. Asset Workflows

This section describes best practices for creating and importing assets usable in Unity.

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
* Avoid super long thin triangles (Speeds up tile-based renderers & helps with proper GI baking)
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

Bind normal bones to the CAT rig for use in skinning and exporting.

###### Bind Pose

Set Motion Panel/Layer Manager/"Setup/Animation Mode" Toggle to ```Red```
Select only the bones and the mesh you wish to export
Export naming: ModelName.FBX

###### Animation

Set Motion Panel/Layer Manager/"Setup/Animation Mode" to ```Green```
Select ONLY the bones required in your hierarchy (These should match the exact same bones used for Bind Pose), don't include the mesh.
Export naming: ModelName@AnimationName.FBX
The @ symbol is a special Unity naming convention allowing the animation to be bound to the Human.fbx in the Unity editor.

#### Importing from 3ds Max into Unity

If importing only animation or bones from a FBX: 

* Set ```Preserve Hierarchy Model``` import option to ```True```
* Set ```Rig > Avatar Definition``` to ```Copy From Other Avatar```

MaxListener Window, set the width and height of selected bones, and maybe objects too.
$.width = 0.01
$.height = 0.01

**[⬆ Back to Top](#table-of-contents)**


### 4.2 Textures

* Textures follow the [naming convention](#anc-textures) found above. 
* They are a power of four (512 x 512, 256 x 1024).
* Use Texture Atlases wherever possible.
* 3D software should point to the Unity project textures for consistency when you save or export.
* It is better to resize the texture in Photoshop than to use Unity’s compression options when the in-game texture resolution is already known. This reduces the file size and import time of the texture into Unity.
* When working with a high-resolution source PSD outside your Unity project, use the same name for both the high-resolution and the imported Unity files. This allows quick iteration when swapping between the two textures.
* Limitations: High-priority objects will have 2k textures max.

More information for importing textures can be found here: [https://docs.unity3d.com/Manual/ImportingTextures.html](https://docs.unity3d.com/Manual/ImportingTextures.html)

Textures requiring the use of an Alpha channel should follow this guide: [https://docs.unity3d.com/Manual/HOWTO-alphamaps.html](https://docs.unity3d.com/Manual/HOWTO-alphamaps.html)

##### Texture File Format

All textures should be in JPG/JPEG format unless they have an Alpha channel, in which case, the PNG format must be used.

**[⬆ Back to Top](#table-of-contents)**

<a name="audio"></a>
### 4.3 Audio

Only import uncompressed audio files into Unity using WAV or AIFF formats.

Great guide on [Unity Audio Import Optimization](https://yasirkula.com/2020/03/31/unity-audioclip-import-ayarlari/)

**[⬆ Back to Top](#table-of-contents)**

<a name="sprite"></a>
### 4.4 Sprite

All the UI elements should be separated as a panel into sprite atlases.

For example, all the sprites associated with the Inventory panel must be in the Inventory Panel Atlas(Tex_Atlas_UI_Inventory). The atlas must include all the sprites that belong to the panel. That means every sprite in that atlas will be shown on that specific panel, not elsewhere. There are some exceptions; we can assume there will be hundreds of items in the inventory example. We can split items into different atlases but must split them by type. For example, Tex_Atlas_UI_Item_Food_01, Tex_Atlas_UI_Item_Food_02, Tex_Atlas_UI_Item_Food_03

**[⬆ Back to Top](#table-of-contents)**


## 5. Version Control
This section describes how we are going to use git and repos. 

### 5.1 Base Rules
First of all, we are going to follow [Conventional Commits specification](https://www.conventionalcommits.org/tr/v1.0.0/#%C3%B6zet).

Another rule about committing changes is not included in the Conventional Commits specifications; we will use the type "imp"(import) to push our asset import commits. For example;

```imp: Atmospheric House asset part 1``` 
```imp: Atmospheric House asset part 2``` 


That way, we can split the asset imports into parts and ensure everyone knows we just imported an asset to the project. Also, because of git limitations, we can't push too many objects at once. With this method, we can split the importing process into parts to walk around the limits(some sort of).

### 5.2 Branches
Every developer will have their branches when needed related to features, hotfixes, and main branches such as "Development" and "Main/Product"; they can manage their commits and work in that branch till it’s ready to push it to the related branch, it can be the main branch, or it can be a feature branch. In that way, developers can push their changes to the git and keep working on another PC, or in the end, they can save their progress to the server to be safe from any loss. Here is the expected branch architecture;

 - **Main**: This is the branch that represents the live version of the product. No one can send commits directly to this branch; every commit must be tested in the Development branch before it goes into the Main branch.
 - **Development**: This branch is the most updated product version, containing untested features and fixes. Every commit goes into the Development branch at the end and gets tested by the QA teams.
- **Feature_XYZ (sub-branch)**: These branches are for developing new features for the product. Only leads can open a PR to merge these branches into the Development branch. Developers must create their branches and send their commits through a PR to the related sub-branches.
 - **HotFix_XYZ (sub-branch)**: These branches have the same rules as Feature branches.
 - **Feature_XYZ_DevX (dev-branch)**
 - **Feature_YZ_DevY (dev-branch)**
 - **HotFix_XYZ_DevZ (dev-branch)**

Each feature and hotfix will have its branch until it is accepted into the development branch. Developers will have their branches on top of those branches, and they will open a PR to send an update. Each dev branch will be closed after the merging of the related sub-branch. A dev branch created from one of the main branches will stay until the developer moves on to another project.

### 5.3 Pull Requests
Before sending a PR to any branch, the developer must pull the source/target branch's changes and make sure s/he fixed any merge conflicts. Leads may and will ask the developers to resolve the merge conflicts anytime they are ready to inspect or accept the particular PR.

If the PR is related to a bug fix, the title should have the ID of the issue/to-do from the project management tools given. 

**The developer must profile the feature and make sure it's garbage-free before opening a PR.**

### 5.4 Commits
The commits' titles and descriptions must include short, descriptive information about the feature or fix. If the commit is related to an issue or to-do, the title must have the ID (given by the project management tool).

### 5.5 Interface / Software
The developers are free to use bash if they choose to and have experience with it. But we'll use Fork as our main interface to git as a team.
	
**[⬆ Back to Top](#table-of-contents)**
