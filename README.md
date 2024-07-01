# Get off the table email in 2024

Based on Mark Robbins' incredible work on the subject of coding emails without using tables, I tried at my level to push the research to find solutions to the problems mentioned on his [issue page](https://github.com/M-J-Robbins/get-off-the-table/issues)

The major drawback of Outlook is that it does not recognize certain CSS properties, especially when they are applied to a div. To overcome this problem we therefore use certain mso properties specific to Outlook which will allow us to define the size of the blocks and their positioning.

## Using mso properties to create a "get off the table" email

In this template, I use : 

- the mso-faux absolute technique to create absolute positioning, 
- br clear="all" to separate elements vertically,
- multiple columns
- joint use of margin, padding and border to create internal margins for blocks

1-For the absolute positioning of blocks, and the transformation of divs into tables in Outlook, I use frames here, with :

- mso-element-wrap:no-wrap-beside, 
- mso-left-element:center.


2-To set the size of frames in Outlook and turn everything into tables, I use :

- mso-element-frame-width

3- for blocs positionning in column, I use:

- mso-element-frame-hspace

### 3-Columns:

#### Fist example : file! index.html

The first reflex would be to center the entire email using :

the mso-element-left:center property; This works great for single column emails.

But if we want to create a columnar layout, by applying the mso-element-wrap:no-wrap-beside properties to the blocks, they float and eventually end up centered on each other.


We therefore start by applying the mso-element-wrap:no-wrap-beside property to our blocks. These are found at the left end of the email.

Then, we can reposition the blocks/columns horizontally using the mso-element-frame-hspace property, starting from the position: left: 0. 

For examle, if the first block/column is 300px wide, we therefore indicate mso-element-frame-hspace: 0. Then for the second mso-element-frame-hspace:300px (or more if we want some gutter between colums) and so on.

At the end, we can center everything with an enclosing div which we also center with the mso-element-frame-hspace property.

Visually it can work, as in the example template, but unfortunately the centering cannot be perfect: given the number of versions of Outlook, we cannot know the width of the viewing window for each case.

This approximate centering has also the disadvantage of being broken at 120 or 200 DPI. 

I have not yet found a solution to precisely and automatically center columns

#### second example : file:index-2.html

Another solution to make columns work in Outlook, is to use the mso-element-wrap:around properties.

If we have for example 2 or more columns
- define the width of the blocks using : mso-element-frame-width:xxxpx;
- Set mso-element-wrap:around on the blocks, the last one doesn't need it.
- for the gutter : mso-element-frame-hspace:xxxpx adds space on the left and right side, so you have to choose on which block you add it

  In the case of multiple rows containing multiple columns, you can wrap each row with a mso-element-wrap:no-wrap-beside.

  
Thanks to the mso-element-wrap:around property, the elements will stick one after the other, thus creating columns. We therefore no longer need to position them relative to a left:0 position which is much more practical

The problem of global column centering is still not resolved here.


### 4-Block margins

To add an internal margin to blocks.

With these frames, in Outlook the divs are not recognized as such, and all the linked style is replaced or repositioned in other tags.
The only way I found is to add padding to the enclosing div for all mail clients except Outlook
And for Outlook:

HTML margin on p tag for left and right margins. (in Outlook this adds left and right margins on the enclosing div  
Mso border and mso padding together on the p tag: (This is the only way to make the margins work here: border and padding are kept on the p tag, but this will also add the padding on the enclosing div, without that there is any visible effect.

### 5-br clear="all"

As we can read it on the doc, Word inserts <br clear=ALL> after the Div element. This is necessary to turn off text wrapping in the browser display.without that, the frames are all nested inside each other and the layout becomes very complicated. A simple br and that solves the problem.


In one of his exchanges on [his page](https://github.com/M-J-Robbins/get-off-the-table/issues/3), I see that Mark and heyitstowler had also noted this code, which can be found in the Outlook rendered code:

```
<br style="mso-break-type:section-break">
```


#### References : 

https://github.com/M-J-Robbins/get-off-the-table

https://stigmortenmyre.no/mso/html/concepts/ofconstyletable.htm
