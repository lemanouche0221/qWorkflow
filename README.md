#qWorkflow Library

##A. Introduction
This is an AppleScript library for creating workflows with Alfred 2. This library provides an object-oriented class with functions for working with plist settings files, reading and writing data to files, generating Alfred feedback results, requesting remote data, and more (make sure you read the [FULL DOCUMENTATION](https://github.com/qlassiqa/alfred-workflow/blob/master/documentation/Documentation.md) to get a grip on how to properly use this library).

It was originally created by [David Ferguson using PHP](https://github.com/jdfwarrior/Workflows), and was entirely rewritten by me using AppleScript to provide the same functionality to all my fellow AppleScript lovers.

So you may be asking yourself: 

* ***why on Earth would I use AppleScript when I already have PHP, Python, Ruby, Bash, etc.?*** - yes, it's true, Alfred can be scripted using all those languages, but ask yourself this: **are you able to control MacOS and its Apps using those languages?** I'm afraid not, and this is where AppleScript comes to help. 

* ***but isn't it simpler to use my PHP / Python / etc. skills and combine them with AppleScript inside Alfred?*** Actually no, it isn't simpler - I've tried it, and it becomes really messy, not to mention that Alfred's workflow system doesn't allow that much mixing.

**NOTE:** the `compiled source` folder contains the ready-to-use library script (the file inside this folder should be put inside your Alfred workflow's folder); the `uncompiled source` folder contains the plain .applescript file that you can view online, and it contains fully commented code to better understand what I did there.

##B. Features
There are a lot of things you can do with this library to make your life a lot easier when creating & programming your Alfred Workflows, so here's a list of the most important features (the list will grow while I improve the library):

* **object-oriented approach** to write less & more readable code
* internal **workflow introspection** (finding the bundle ID, cache & storage paths)
* generate Alfred-compatible **XML feedback** with ease
* saving & retrieving **workflow-related settings**
* **remote data requests**
* sending notifications through the **Notification Center** (thanks to [Daji-Djan](https://github.com/Daij-Djan/DDMountainNotifier))
* various **internal utilities that improve AppleScript** (string and date manipulation, file system utilities)

##C. Known Limitations
Now, because AppleScript is a bit limited in terms of capabilities, some functionality isn't available right now, but I will try to improve this library further.

* **no JSON support <u>yet</u>** - AppleScript doesn't know anything about JSON, but I'm already planning a JSON parser for AppleScript

* **bigger file size** - since AppleScript requires extra coding for text manipulation and object handling, the file size is a bit large compared to the PHP equivalent, and it will probably increase as I add new features to it

* **strict syntax for plist records** - it's known that AppleScript's records are a bit clumsy since they lack so many features, that's why when saving a list of records as a PList settings file you should adhere to the following strict record notation: 
 
  ```
	{ 
	  {theKey:"someKeyName", theValue: "textValue"}, 
	  {theKey:"mynum", theValue: 2},
	  {theKey: "booltest", theValue: false},
	  {theKey:"na", theValue: missing value} 
	}
	```

##D. Initialization
Before you write any code, it's imperative that you copy the `q_workflow.scpt` library file. If you plan to use the NotificationCenter methods to trigger notifications, then it's vital that you copy the `bin` folder to your Workflow folder as well since it contains a utility that interfaces with MacOS. Note that trying to send notifications without having the bin folder in your Workflow folder will produce no result (and yes, the utility has to stay inside the bin folder for notifications to work).

```
set workflowFolder to do shell script "pwd"
set wlib to load script POSIX file (workflowFolder & "/q_workflow.scpt")
set wf to wlib's new_workflow()
```

or by specifying a bundle name:

```
...
set wf to wlib's new_workflow_with_bundle("com.mycompany.mybundlename")
```

**Explanations:**
* the first line determines the Alfred workflow's bundle path because this is where the "q_workflow.scpt" library should be placed in order to work

* the second line loads the library from the bundle's path assuming that you already placed it there

* the last line creates a new script object (the equivalent of a class in other languages) with all the required functionality

* since AppleScript doesn't support optional parameters, there are 2 constructors: `new_workflow()` with no parameters, which creates a new class that automatically fetches the bundle name from Alfred, and `new_workflow_with_bundle(<name>)`, which takes 1 parameter with the desired bundle name if none was specified in Alfred.

##E. Methods
For more info, tips and examples on how to use the following methods, please consult the accompanying documentation (again, it is vital that you look at the [FULL DOCUMENTATION](https://github.com/qlassiqa/alfred-workflow/blob/master/documentation/Documentation.md) to get a grip on how to properly use this library).

This library provides 2 categories of methods, namely **workflow methods** and **utility methods**. Workflow methods can be used only after creating a new workflow class (these are also known as instance methods), and provide basic handlers to deal with Alfred Workflows. Utility methods, on the other hand, contain handlers that are used internally by the workflow methods, as well as useful handlers for regular use that enhance AppleScript's capabilities (these include string and date manipulation, file system checks, sending notification, etc.)

#### Workflow Methods
1. get\_bundle()
2. get\_data()
3. get\_cache()
4. get\_path()
5. get\_home()
6. set\_value(key, value, plistfile)
7. set\_values(listofrecords, plistfile)
8. get\_value(key, plistfile)
9. request(url)
10. mdfind(query)
11. write\_file(textorlist, cachefile)
12. read\_file(cachefile)
13. add\_result with(out) isValid given theUid, theArg, theTitle, theSubtitle, theAutocomplete, theIcon, theType
14. get\_results()
15. to\_xml(listofrecords)

#### Utility Methods
1. q\_trim(text)
2. q\_join(list, delimiter or string of delimiters)
3. q\_split(text, delimiter or string of delimiters or list of delimiters)
4. q\_is\_empty(string or list)
5. q\_file\_exists(file path)
6. q\_folder\_exists(folder path)
7. q\_path\_exists(file or folder path)
8. q\_clean\_list(list)
9. q\_encode(text)
10. q\_date\_to\_unixdate(date)
11. q\_unixdate\_to\_date(text)
12. q\_date\_to\_timestamp(date)
13. q\_timestamp\_to\_date(text)
14. q\_send\_notification(message, details, extra)
15. q\_notify()
16. q\_encode\_url(str)
17. q\_decode\_url(str)

##F. Licensing
The library and all its components are free to use, copy and modify, and are provided "AS IS", without warranty of any kind. However, I will greatly appreciate it if you'd give me credit and mention me in your works or anywhere you use this library.