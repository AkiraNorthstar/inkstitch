---
title: "Workflow"
permalink: /docs/workflow/
excerpt: ""
last_modified_at: 2018-04-14
toc: true
---

## Basic Usage

**Tipp:** If you are new to Inkscape, have a look at their [Basic Tutorial](https://inkscape.org/en/doc/tutorials/basic/tutorial-basic.html).
{: .notice--info }

Try the following steps in order to test the extension and to learn about basic functionality.

### Step 1 - Draw an Object

Create an object, e.g. a circle and make sure it has both a stroke and a fill.

![Fill and Stroke](https://edutechwiki.unige.ch/mediawiki/images/thumb/8/86/SVG-yellow-circle-stroke-fill.png/300px-SVG-yellow-circle-stroke-fill.png)

### Step 2 - Convert to Path
Transform **all objects** you want to stitch to paths:

* Select all objects (`Ctrl+A`)
* `Path -> Object to Path` or `Ctrl+Alt+C`.<br>

**Info:** Objects that are not of "path" type, are ignored by InkStitch.
{: .notice--info }

### Step 3 - Parametrize SVG Path for Embroidery

* Select at least one object.
* Open `Extensions -> Embroider -> Params` and play with them.
* For now, just accept the defaults or close without saving.

### Step 4 - Create the Embroidery File

You can either create a stitch file for a selection of objects or for all path objects.
To create an embroidery file for the whole design:

* Click into some empty space (in order to deselect)
* Run `Extensions > Embroidery > Embroider...`
* Select the right file format for your machine
* Type a directory name where you want to save your output files. E.g. `C:\Users\%USERNAME%\Documents` on Windows. Ink/Stitch will remember this information.

### Step 5 - Inspect in Inkscape

The circle you made will disappear and be replaced with some stripes and zig-zags. Ink/Stitch has hidden all of your layers and created a new one called `Stitch Plan`, in which it has placed a visual representation of the stitch plan it created. It has interpreted your shape as two instructions: Fill and Stroke. Fill is implemented using fill stitching, and Stroke is implemented by running satin stitching along the outline.

Select the horizontal lines using the `Edit Paths by Nodes` tool. Zoom in a bit and you'll see that the lines are actually made up of lots of points. Each point represents one stitch - one needle penetration and interlocking of the top thread with the bobbin thread. Notice how the points all line up nicely in diagonals. This will give the fill stitching a nice, orderly visual appearance.

Now look at the zig-zags. These are the satin stitches. Note that the corners look pretty ugly. This is because satin stitches generated from a shape's stroke are pretty rudimentary and aren't implemented intelligently. *You can exert much greater control over satin stitching using a **Satin Column**, described later.*

![Stitch Plan](https://edutechwiki.unige.ch/mediawiki/images/thumb/6/6a/Inkstitch-stitch-plan.png/800px-Inkstitch-stitch-plan.png)

The stitching preview you're looking at just now isn't intended to be permanent. Immediately undo it with `Ctrl-Z` after you've looked at the stitches. The actual work that does, is to output a design file.

### Step 6 - Stitching Out the Design
Where'd the design file go? One of the parameters you were able to specify in the filter settings dialog was the output directory. By default, the directory used is the place where you installed the extension's Python files.

Ink/Stich will create a file named `something.___`, where `something` is the name of your svg file (e.g. `something.svg`) and `___` is the proper extension for the output format you select. If `something.___` already exists, it will be renamed to `something.___.1`, and `something.___.1` will be renamed to `something.___.2`, etc, up to 5 backup copies. When you've got the design the way you like it, save off a copy of `something.___` and feed your machine.

## Workflow

### Step 1: Sketch design or use an image

First, I get an idea for what I want my finished product to look like. If I'm basing my design off an existing picture or graphic, I load it into Inkscape in its own layer. Some graphics are amenable to Inkscape's auto-tracing feature, especially if I simplify the image in GIMP first.

After auto-tracing, I clean up the vector shapes, using "Simplify" and deleting nodes by hand when possible. My goal is to use as few Bezier curves as reasonably possible to represent the image.

If I need to trace an image by hand, I usually use the freehand drawing tool. This tool creates paths with a lot of Bezier nodes, so again, I'll simplify the curves as much as possible.

Working with an existing SVG image can save a ton of time, so consider using Google image search with the filter set to SVG.

For text, choose a font carefully. It's quite hard to make satin look good when it's 1mm wide or narrower. Sans-serif fonts tend to be the easiest. For text smaller than 4mm tall, you'll have a very difficult time making lowercase letters look good, so consider block-caps. Cursive/script fonts can work well, but it's not going to be as easy as you think. I find that I spend the most time on text by far.


### Step 2: Plan stitch path and color changes

At this point, you'll have a vector graphic representation of your image. The next thing to do is to convert your vectors into the kind that **Embroider** understands and put them in the right order.

When you're designing for embroidery machines that can't cut the thread mid-sew or switch colors automatically, you're going to want to optimize your stitch path to reduce or hide jump stitches and make minimal color changes. I also try to avoid stitching over jump stitches when possible, because it's a total pain to trim them by hand when you do.

The order of stitching also affects how the fabric pulls and pushes. Each stitch will distort the fabric, and you'll need to take this into account and compensate accordingly. Look for articles on machine embroidery distortion for more info on this.

### Step 3: create the embroidery vectors

I make heavy use of layers and groups at this point. If I've traced an image, I'll leave it as the lowest layer and set it invisible in the Layers or Objects palette. Any layer, group, or vector shape that is set invisible will be ignored by **Embroider**.

I keep my initial traced vectors in their own layer and use them as a reference when designing embroidery vectors. I copy and paste then as necessary into a higher layer and work with the copies.

I use only AutoFill and Satin Columns in my designs. I begin converting filled areas to AutoFill regions. Each time I create an AutoFill shape, I set its parameters using **Params**. Then I select it and run **Embroider**, which will cause it to show a stitch plan for just the selected object(s).

I examine the resulting stitch plan using the node editor tool. Each vertex is a single stitch; the needle will penetrate the fabric and interlock with the bobbin thread at this point. Once I'm done examining the stitch plan, I Undo the **Embroider** operation to remove the stitch plan and make my vectors visible again. Then I make any changes necessary, re-run **Embroider**, and repeat until it looks right.

At this point, I save my SVG file. If Inkscape is starting to become sluggish (due to the memory leak described above), I'll restart it before continuing.

Next, I work on Satins. Remember that a Satin Column is defined by two lines that run along the edges of the column. It's usually a good idea to run satin along the outside border of a fill region. Inkscape makes this easy. I copy and paste the shape from the traced vectors, then disable Fill and enable Stroke. I set the stroke width to my desired satin width. Finally, I use the "Stroke to Path" option to convert just the stroke into its own path.

At this point, it's necessary to cut the paths so that they aren't a continuous loop. The cut will also tell **Embroider** where to start stitching from. Add a point at the desired cut location by double-clicking on the path with the Node Editor tool active. Cut at this point by selecting at and pressing shift+b. Repeat for the second path.

Now you've got an object made of two paths. They need to be going on the same direction for Satin Column to work. You can tell what direction the path goes in by enabling direction indicators in Inkscape's preferences. To reverse one of the paths, select one of its points and choose "Reverse Path". I bind this to ctrl+r in Inkscape's preferences.

By now, it's likely that the two paths in the object have an unequal number of nodes. As described above, a Satin Column is made of consecutive pairs of points on the two paths. You have a couple of techniques available to make your nodes line up in pairs:

* simplify the shape (control+L)
* delete extra nodes
* add more nodes
* joining nodes

I usually Simplify first if necessary to reduce the path to a manageable number of nodes. Remember that machine embroidery is fairly imprecise and your final product will not have the incredibly fine details that you see on your screen, so simplifying can often be acceptable even if it changes the path.

Next, I _try_ to delete nodes or join. Inkscape will attempt to manipulate the neighboring nodes as necessary to keep the path the same, but I find that it's not particularly good at this. Instead of deleting a troublesome point, it can often work better to simply add a matching point on the other path.

Finally, I run *Embroider* with the path selected and examine the output. I add, remove, and adjust points as necessary to make my satin look nice.

You may find that you get the dreaded error, "object <name> has two paths with an unequal number of points". This can be confusing because it may _look_ like your paths have the same number of points. Usually this is because of duplicate points: multiple points with the exact same coordinates or very similar coordinates. To find them, I drag-select each point in turn, examining the bottom status bar. It will tell me how many points I've selected. If it looks like I've selected just one but Inkscape says I've selected 3, then it's likely that I've found my culprit.

Often just pressing shift+J (join nodes) will eliminate the extra points without modifying the shape. Sometimes it won't, and in cases like that, I've had luck with adding extra points on either side of the "cluster", then delete the "cluster". The extra points anchor the shape and disallow Inkscape from messing it up. Then it may be possible to delete the added points, or I just add points to the other path to match with them.

### Step 4: Ordering

Once I've created all of my vectors and test-embroidered them individually, it's time to put everything in the right order. This is where the Objects tool from the latest development version of Inkscape (described above) comes in useful. Because my embroidery machine can neither trim threads nor switch colors mid-sew, I optimize my order to minimize color changes and reduce or hide jump-stitches.

*Embroider* will stitch objects in exactly the order they appear in your SVG document, from lowest to highest in stacking order. If the distance between two objects is long, *Embroider* will add a jump-stitch between them automatically. It uses the color of the object to determine thread color, so changes in color from one object to the next will result in a thread-change instruction being added to the embroidery output file.

Inkscape gives you the ability to raise and lower objects in the stacking order using the PageUp and PageDown keys. However, it seems to be unwilling to give the user complete control over the stacking order. I think this has to do with whether objects overlap: if A and B don't overlap, Inkscape doesn't care how they're stacked in the SVG file and it sometimes won't let you reorder them.

To solve this, I created the *Reorder* extension. To use it, hold down the shift key and select objects one at a time in the order you'd like them to appear in the SVG file and run *Reorder*. *Reorder* will remove the selected objects from the SVG XML tree and then re-add them at the location of the first-selected object, in the order you selected them.

You can also manually manipulate the underlying SVG XML structure by using Inkscape's XML Editor pane. Its "Raise" and "Lower" buttons directly manipulate the order of XML tags in the SVG file and are not subject to the same limitations as PageUp and PageDown. Note that the ordering of XML tags in the XML Editor tool is the _reverse_ of the order of objects in the Objects tool.

### Step 5: Render to a file format supported by your machine

Once I've got everything in the right order, I deselect all objects and run *Embroider* again. This will embroider all visible objects in the document. In the extension settings, select a file format supported by your machine. Most machines can support DST, and some Brother machines prefer PES.

*Embroider* will create a file in the specified output directory named after your SVG file, but with the extension changed to `.DST`, `.PES`, or whatever format you selected. It will back up any existing file there, storing up to 5 old copies of each file.

### Step 6: Convert to PES and upload

Transfer the design to your machine in whatever manner is appropriate. My machine exposes itself as a (tiny) USB thumb drive, so I can upload directly.

### Step 7: Test-sew

I've never gotten an embroidery file correct on the first try. There's always room for improvement! To test out my design, I prepare a test piece of fabric that matches my final fabric as closely as possible. I use the same stabilizer and the exact same fabric if possible. For t-shirts, I try to find a similar fabric (usually knit). Knits need a lot of stabilization.

I sew out the design, watching the machine to make sure that there aren't any surprises. I'm watching for gaps that indicate that the fabric has been distorted. I'm also watching for areas where stitches are piling up too closely and the machine is having trouble sewing, which indicates that the stitch density is too high. Finally, of course, I'm watching in giddy anticipation to see my design for the first time! :)

### Step 8+: iterate

Then I go back and tweak my design. Hopefully it only takes a few tries to get it how I want it. Once I'm done, I copy the final embroidery file from my output directory, just to avoid accidentally overwriting it in the future.