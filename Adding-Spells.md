This page explains how to add spells to your spell pack.

## Prerequisites
- An addon mod ready to add spells to. You should have completed everything in the [[developing addons]] tutorial at this point.
- An idea for a spell! The only limits here are your imagination and your programming skill, though the latter can always be improved with practice - and there's a whole modding community out there that can help you out if you get stuck.

## Writing the spell class

First of all, you'll need to write a new class for your spell, where you can put all of the code that makes it work. Create a new class and have it extend `Spell`. The `Spell` class can be found in the package `electroblob.wizardry.spell` and is the base class for all spells. I've taken the time to write full Javadoc comments for the methods and fields in this class, so I recommend you read them carefully!

In this class, create a new constructor with no arguments, and inside it, call the super constructor in the `Spell` class. There are **two** super constructors in `Spell`, and you need to make sure you use the one which takes an additional `modID` argument. You'll then need to pass some values into the super constructor which define basic information about your spell, such as tier, element and mana cost; have a look at the example below for the. Most importantly, you'll need to pass in the **mod ID** of your addon mod, so that wizardry knows which mod the spell is from and where to look for your spell icon.

> Depending on what you did when you created the class, your IDE might have added two constructors for you. If this happens, delete the constructor **without** the `modID` argument (it'll be the shorter one), remove all of the arguments from the remaining constructor, and then pass in the values as before.

The next thing you'll need to do is override the _first_ `cast(...)` method in `Spell`. This is the method that makes your spell work, and it gets called whenever a _player_ casts the spell (see below for an explanation of how to get wizards and other entities to cast your spell). Write the code that makes your spell work in this method. Your spell could do any number of different things, so use your imagination! Pretty much anything is possible with Forge these days. You might find it useful to look through some of wizardry's own spell classes for examples.

> There is a class in `electroblob.wizardry.util` called `WizardryUtilities` that you might find useful - I wrote it as a place to put various methods I needed for the mod, some of which might come in handy to you as well.

## Registering your spell

To get your spell to appear in game, you'll need to register it in a similar way to blocks and items. Like blocks and items, each spell has a single instance. In Forge 1.10 and higher, this is done using **registry events**. To register your spell using registry events, make an event handler (or open up an existing one) and make a new method that subscribes to `RegistryEvent.Register<Spell>` (see the Forge documentation on [events](https://mcforge.readthedocs.io/en/latest/events/intro/) if you don't know how). Inside that method, create a new instance of your spell class and register it using `event.getRegistry().register(spell)`. The method should now look something like this:

```java
@SubscribeEvent
public static void register(RegistryEvent.Register<Spell> event){

    event.getRegistry().register(new YourSpell());

}
```

> The older `GameRegistry.register(...)` methods still exist in Forge, but because of how wizardry processes the registered spells, these **will not work** for spells - you must use registry events.

That's it for the required stuff! If you load up the game, you should now see a spell book and (if your spell isn't continuous) a scroll for your spell in the Spells tab of the creative mode inventory, and with any luck you'll be able to cast your spell in-game.

## Making your spell look nice

As you probably know, all spells in wizardry have an icon which appears in their spell book and on the spell HUD. You'll need to draw an icon for your spell, which should be a pictorial representation of your spell. I suggest starting with the blank icon, which can be found in `assets/wizardry/textures/spells/none.png`. Edit this with an image editing program (I use GIMP) and save your finished icon as `[your spell name].png` in `resources/[your mod ID]/assets/textures/spells`, where [your spell name] is the unlocalised name you gave your spell in its constructor.

You will need to add **localisations** for the spell to give it a nice, readable name and description. These should be put in **your lang file**, located in `assets/[your mod ID]/lang/en_US.lang`. The syntax is `spell.[unlocalised name]` for the spell name and `spell.[unlocalised name].desc` for the description.

## Further information

That covers the basics of adding your own spells to wizardry, but it's likely that at some point you'll want to do something that requires more than just a spell class. Fortunately, wizardry already has some classes that can help you:

- If you want to add custom entities for your spell, there are some base classes you can use for that. See [[Adding Entities]] for more details.
- You might also want to add particle effects to your spells. Vanilla Minecraft has a fair few itself, but there are also some that are added by wizardry. To spawn these, use `Wizardry.proxy.spawnParticle(...)`, and pass the `WizardryParticleType` that you want to spawn into the method parameters.
- `WandHelper` in `electroblob.wizardry` might also come in handy if you plan to interact with wand data at all.