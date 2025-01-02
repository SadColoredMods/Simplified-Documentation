# Item System

## How Does the Item System Work?
The item system is split into 2 sections:

- ItemManager
- BaseItem

The ItemManager manages the players inventory and item data. 
BaseItem is an inhertiable class, which items use for functionality.

## Creating an Item
There's 2 steps to creating an item
### Step 1 - Writing Code
Create a new class and name it whatever you want. 
You should inherit from BaseItem.
Right now, your code should look like this:
```cs title="Item.cs" hl_lines="4"
using System.Collections;
using UnityEngine;

public class Item : BaseItem
{
    void Start()
    {
    }

    void Update()
    {
    }
}
```

Here, you can inherit from many different methods such as:
```cs
public virtual void OnUse() { }
public virtual void OnSelect() { }
public virtual void OnDeselect() { }
public virtual void OnPickup() { }
```

When you're inheriting from ```base.OnUse()```, you can modify ```base.DontRemoveItem```, this will not remove the item after usage if set to true. It defaults to false if not specified. **DONT USE THIS FOR ITEM USE COUNTS, AS THAT'S PREHANDLED BY SIMPLIFIED**. Instead, use it for situations like when the player has used the quarter, but theres no vending machine infront of them, so the item doesnt get removed.

It is automatically reverted back to false after the method has ended.

```cs title="Item_OnUse" hl_lines="6 7 8 9"
using System.Collections;
using UnityEngine;

public class Item : BaseItem
{
    public override void OnUse()
    {
        Debug.Log("use :o");
    }
}
```

Another method, ```SendRay(string Tag, out RaycastHit RayHit, double Range = 10)```, will return whether or not it hit a GameObject with the specified tag in argument 1 and the raycastHit object.

This can be wrapped in an if statement like:
```cs title="Item_SendRayAndDontRemove.cs" hl_lines="8 9 10 11 12 13 14 15"
using System.Collections;
using UnityEngine;

public class Item : BaseItem
{
    public override void OnUse()
    {
        if (SendRay("Door", out RaycastHit Ray))
        {
            Debug.Log($"found {Ray.transform.gameObject.name} :D");
        }
        else
        {
            DontRemoveItem = true;
        }
    }
}
```

### Step 2 - Creating the Object
Go to your School scene, and open up the ItemManager gameobject to reveal all of its children.
Create a new empty gameobject and name it the name of your script, now attach the script to it.
Fill out all details like:

- [Name](https://github.com/SadColoredMods/Baldi-Simplified-Open-Source-OVERHAULED/blob/main/Assets/Scripts/ItemStuff/BaseItem.cs "what the player will see it called")
- [Big Sprite](https://github.com/SadColoredMods/Baldi-Simplified-Open-Source-OVERHAULED/blob/main/Assets/Scripts/ItemStuff/BaseItem.cs "what item will look like in the world")
- [Small Sprite](https://github.com/SadColoredMods/Baldi-Simplified-Open-Source-OVERHAULED/blob/main/Assets/Scripts/ItemStuff/BaseItem.cs "what item will look like in the HUD")
- [Uses](https://github.com/SadColoredMods/Baldi-Simplified-Open-Source-OVERHAULED/blob/main/Assets/Scripts/ItemStuff/BaseItem.cs "how many times the item can be used")

### Step 3 - Assigning to a Pickup Script
Now your item should be ready to be used, but you need to assign it to a pickup script, to do this, simply note down your items ID.
To get the Item ID, it's simply how many items are above it + 1
So for example, BSODA has an ID of 4, whilst the quarter has an ID of 5 because it is one below the BSODA.

Here's a list of every items ID:

- Nothing - 0
- Zesty Bar - 1
- Swinging Door Lock - 2
- Principals Keys - 3
- BSODA - 4
- Quarter - 5
- Tape - 6
- Alarm Clock - 7
- WD-NS - 8
- Safety Scissors - 9
- Boots - 10

Once you've gotten your ID, simply go to the pickup script of your choice, and change the ```Item ID``` field, to your items
You'll eventually learn them all :D

## Item Manager Methods
If you're a little bit of a 🤓 you can read ahead to learn about the APIs the item manager has

```Instance``` its just a singleton, bro.

```UpdateItemUI``` force an update of the item HUD

```GetItem(string name)``` returns a [BaseItem](https://github.com/SadColoredMods/Baldi-Simplified-Open-Source-OVERHAULED/blob/main/Assets/Scripts/ItemStuff/BaseItem.cs) from the given name

```GetItem(int id)``` returns a [BaseItem](https://github.com/SadColoredMods/Baldi-Simplified-Open-Source-OVERHAULED/blob/main/Assets/Scripts/ItemStuff/BaseItem.cs) from the given id

```AddItem(BaseItem item)``` adds an item to the item index. its id will be the next available free id

```RemoveItem(string name)``` removes an item from the item index based on the given name. might break tho i didnt test lmao

```RemoveItem(BaseItem item)``` removes the item from the item index. might break tho i didnt test lmao

```GetSelectedItem()``` returns the currently selected item id

```GetSelectedItemObject()``` returns the selected item object / instance, and creates one if it doesnt exist

```CollectItem(int ItemID, BaseItem instance == null)``` collects an item, if instance is null a new instance is automaticlzly created, if its present it will be the instance of the item

```ReplaceCurrentItem(int ItemID)``` yeah it ust replaces the current item. its just for the vending machine yeah idk

## How Does This Work?
great question. figure it out yourself then submit a pr pwetty pwease