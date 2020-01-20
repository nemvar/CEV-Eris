# General
* Where possible, large projects should be broken up into several smaller pull requests, and/or done in phases over time.
* Pull requests should not contain commented code except TODOs and explanation comments.
* Pull requests should not contain any debug output, variables or procs, unless these are of value to admins/coders for live debugging.
* Pull requests should not contain changes that do not relate with functionality described in commit messages.
* If pull request relates with existing github issue, it should be specified in commit message, for example, "Fix broken floor sprites, close #23" (see https://help.github.com/articles/closing-issues-via-commit-messages/ for additional info).
* If pull request contains map files changes, it should be previously proccessed by mapmerger tool (see /tool/mapmerger/install.txt for additional info). Pull request description should contain screenshots of map changes if it's not obvious from map files diff.
* If pull request contains icon files changes, it should be previously proccessed by icon merger tool (see /tool/dmitool/merging.txt for additional info). Pull request description should contain screenshots of changed icon files.
* If you have the permissions, please set appropriate labels on your PRs. Including, at the very least, Ready for Review to indicate that its done.

# Advice for new recruits
Start small! Make your first couple of PRs focus on bugfixes or small balance tweaks until you get used to the system. The repo is littered with failed projects from people who got overambitious and burned out.

Seek input before starting work on significant features. Your proposal may conflict with existing plans and need modified. Getting the approval of maintainers, and especially the head developer, is important for things which may affect game balance.

Be flexible. Very few submissions are accepted as-is, almost every PR will have some required modifications during the review process, sometimes to how the code works, or often to balance out mechanics. 

Finish what you start. A project is only done when it's merged, not just when the PR is up. After submitting something, try to make some time to be available over the next week or so to fix any requested changes after its reviewed. We won't merge unfinished work.

Ask for help whenever you need it. No man is an island. Don't try to struggle alone, nobody will judge you for asking for help with even silly things.


# Changelog Entries
Any pull requests which add or change user-visible features should have a changelog written up. See example.yml in the html/changelogs directory. Make a copy of it, insert your own name, and write about what you've changed. Include it with your pull request. Not everything needs a changelog, only features that players will notice or care about. Minor bugfixes or code refactors can usually go without.


# Coding Policies
Eris has an unusual top-down development style, with future features largely planned out.
To avoid conflicts, it is strongly recommended to discuss any proposed changes in the discord, and get the approval of the development team, before starting work on something that may ultimately be rejected. We can work with your ideas and help fit them into the broader vision.

When making adjustments to game balance, changes should be explained, and generally made in small steps unless there's an egregious problem. 15-25% at a time is the recommended change for balancing values.

When working on large projects, try to make the resulting pull requests as small as feasible. Split large projects into multiple smaller phases if possible. We strongly encourage iterative development, and it's perfectly fine to implement a large feature in many PRs over several months.

Try to comment your code well, there's rarely such a thing as overexplaining. comments are especially important when writing large new features, or using things in unexpected ways.

Values which affect game balance, such as movespeeds, health values and weapon damage, should not be written in or read from config files. Whenever working on an area where such values already exist in config, phase them out and use defines or global variables instead.

When designing new systems and features, try not to create an undue burden for future coders who will have to maintain your work.

The following features or systems are deprecated and should not be used if at all possible. 
Datacore: Use modular records instead
/obj/item/device/pda, and PDA cartridges: Use modular PDAs instead.
Single Function computer consoles: Use modular computers instead.
Direct html browse calls: Use NanoUI instead.

Avoid "Cargo Cult Programming", the ritual of things you don't understand. Try your best to understand the function of codeblocks you copy and paste.


# Code style
Proc defines should contain full type path.

***Good:***
```
/obj/item/pistol/proc/fire()

/obj/item/pistol/proc/reload()
```
***Bad:***

```
/obj/item/pistol
    proc
	fire()

    proc/reload()
```
***
If, else, for, return, continue and break expressions should not be inline.

***Good:***
```
if(condition)
    foo()
```
```
for(var/object in objects)
    foo(object)
```
***Bad:***
```
if(condition) foo()
```
```
for(var/object in objects) foo(object)
```
***

Spaces are needed between function agruments (declaration and definition). Spaces are needed between the binary operator and arguments. Space is not needed when the operation is unary. Spaces are not needed near brackets. Spaces are needed around assignment operator.

***Good:***
```
/obj/item/pistol/fire(var/user, var/target)
    if(can_fire() && target)
        ammo--
        var/corpse = target
```
***Bad:***
```
/obj/item/pistol/fire(var/user,var/target)
    if ( can_fire()&&target )
        ammo --
        var/corpset=target
```
***


Don't have unnecessary return calls or return meaningless data.
If there's nothing after a return, and its not returning a specific value, you don't need a return at all.
The . var stores the return of a function and will be returned even without a specific return call.
***Good:***
```
/proc/do_thing()
	do_thing
	return result_of_doing_thing
	
/proc/do_thing()
	do_thing
	. = result_of_doing_thing
	
/proc/do_thing()
	do_thing
	do_other_thing
```
***Bad:***
```
/proc/do_thing()
	do_thing
	. = result_of_doing_thing
	return
	
/proc/do_thing()
	do_thing
	do_other_thing
	return
```
***


Boolean variables and return values should use TRUE and FALSE constans instead of 1 and 0.
***Good:***
```
/obj/item/pistol/
	var/broken = FALSE

/obj/item/pistol/proc/can_fire()
	return TRUE
```
***Bad:***
```
/obj/item/pistol/
	var/broken = 0

/obj/item/pistol/proc/can_fire()
	return 1
```
***

Using colon operator (:) for object property and procs access is generally inadvisable.

***Good:***
```
var/obj = new obj()
var/count = obj.count
```
***Bad:***
```
var/obj = new obj()
if(hasvar(obj, "count"))
	var/count = obj:count
```
***

Colorized text outputs should use `to_chat(target, text)` and html tags instead of `<<` and magic color symbols. Make use of our span defines when possible.

***Good:***
```
to_chat(player, SPAN_NOTICE("Everything is OK."))
to_chat(player, SPAN_WARNING("There's something wrong..."))
to_chat(player, SPAN_DANGER("Everything is fucked up!"))
```
***Bad:***
```
player << "\blue Everything is OK."
player << "\red \bold Everything is fucked up!"
```
***

del() usage is deprecated, use qdel() instead.

***Good:***
```
qdel(src)
```
***Bad:***
```
del(src)
```
***



# Naming
Avoid short names for class variables. No acronyms or abbreviations.
These are fine to use for local variables within a proc though.

***Good:***
```
/obj/proximity_sensor/update_sprites()
var/count = 0
```
***Bad:***
```
/obj/prox_sensor/upd_sprites()
var/c = 1
```
***


Name your proc parameters properly to prevent name conflicts. If in doubt, use the prefix _ to clearly indicate an input parameter.
Do not use src.var if it can be helped.

***Good:***
```
/obj/set_name(var/newname)
	name = newname
	
/obj/set_name(var/_name)
	name = _name
```
***Bad:***
```
/obj/set_name(var/name)
	name = name
	
/obj/set_name(var/name)
	src.name = name
```
***

Variables, types and methods should be named in "snake case". Constant values should be named in uppercase. 

***Good:***
```
proc/redraw_icons()
#define SHIP_NAME "Eris"
```
***Bad:***
```
proc/Reload_gun()
var/brigArea
```
***

# Dream Maker Quirks/Tricks
Like all languages, Dream Maker has its quirks, some of them are beneficial to us, like these

#### In-To for-loops
```for(var/i = 1, i <= some_value, i++)``` is a fairly standard way to write an incremental for loop in most languages (especially those in the C family), but DM's ```for(var/i in 1 to some_value)``` syntax is oddly faster than its implementation of the former syntax; where possible, it's advised to use DM's syntax. (Note, the ```to``` keyword is inclusive, so it automatically defaults to replacing ```<=```; if you want ```<``` then you should write it as ```1 to some_value-1```).

HOWEVER, if either ```some_value``` or ```i``` changes within the body of the for (underneath the ```for(...)``` header) or if you are looping over a list AND changing the length of the list then you can NOT use this type of for-loop!

#### for(var/A in list) VS for(var/i in 1 to list.len)
The former is faster than the latter, as shown by the following profile results:
https://file.house/zy7H.png
Code used for the test in a readable format:
https://pastebin.com/w50uERkG
***

#### Istypeless for loops
A name for a differing syntax for writing for-each style loops in DM. It's NOT DM's standard syntax, hence why this is considered a quirk. Take a look at this:
```DM
var/list/bag_of_items = list(sword, apple, coinpouch, sword, sword)
var/obj/item/sword/best_sword
for(var/obj/item/sword/S in bag_of_items)
	if(!best_sword || S.damage > best_sword.damage)
		best_sword = S
```
The above is a simple proc for checking all swords in a container and returning the one with the highest damage, and it uses DM's standard syntax for a for-loop by specifying a type in the variable of the for's header that DM interprets as a type to filter by. It performs this filter using ```istype()``` (or some internal-magic similar to ```istype()``` - this is BYOND, after all). This is fine in its current state for ```bag_of_items```, but if ```bag_of_items``` contained ONLY swords, or only SUBTYPES of swords, then the above is inefficient. For example:
```DM
var/list/bag_of_swords = list(sword, sword, sword, sword)
var/obj/item/sword/best_sword
for(var/obj/item/sword/S in bag_of_swords)
	if(!best_sword || S.damage > best_sword.damage)
		best_sword = S
```
specifies a type for DM to filter by. 

With the previous example that's perfectly fine, we only want swords, but here the bag only contains swords? Is DM still going to try to filter because we gave it a type to filter by? YES, and here comes the inefficiency. Wherever a list (or other container, such as an atom (in which case you're technically accessing their special contents list, but that's irrelevant)) contains datums of the same datatype or subtypes of the datatype you require for your loop's body,
you can circumvent DM's filtering and automatic ```istype()``` checks by writing the loop as such:
```DM
var/list/bag_of_swords = list(sword, sword, sword, sword)
var/obj/item/sword/best_sword
for(var/s in bag_of_swords)
	var/obj/item/sword/S = s
	if(!best_sword || S.damage > best_sword.damage)
		best_sword = S
```
Of course, if the list contains data of a mixed type then the above optimisation is DANGEROUS, as it will blindly typecast all data in the list as the specified type, even if it isn't really that type, causing runtime errors.
***

#### Dot variable
Like other languages in the C family, DM has a ```.``` or "Dot" operator, used for accessing variables/members/functions of an object instance.
eg:
```DM
var/mob/living/carbon/human/H = YOU_THE_READER
H.gib()
```
However, DM also has a dot variable, accessed just as ```.``` on its own, defaulting to a value of null. Now, what's special about the dot operator is that it is automatically returned (as in the ```return``` statement) at the end of a proc, provided the proc does not already manually return (```return count``` for example.) Why is this special?

With ```.``` being everpresent in every proc, can we use it as a temporary variable? Of course we can! However, the ```.``` operator cannot replace a typecasted variable - it can hold data any other var in DM can, it just can't be accessed as one, although the ```.``` operator is compatible with a few operators that look weird but work perfectly fine, such as: ```.++``` for incrementing ```.'s``` value, or ```.[1]``` for accessing the first element of ```.```, provided that it's a list.
***

#### Startup/Runtime tradeoffs with lists and the "hidden" init proc
First, read the comments in [this BYOND thread](http://www.byond.com/forum/?post=2086980&page=2#comment19776775), starting where the link takes you.

There are two key points here:

1) Defining a list in the variable's definition calls a hidden proc - init. If you have to define a list at startup, do so in New() (or preferably Initialize()) and avoid the overhead of a second call (Init() and then New())

2) It also consumes more memory to the point where the list is actually required, even if the object in question may never use it!
***
