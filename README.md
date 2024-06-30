# get-off-the-table-2024

Based on Mark Robbins' work on the subject of coding emails without using tables, I tried to take the research a little further, adding some elements. 

The major drawback of Outlook is that it does not recognize certain CSS properties, especially when they are applied to a div. To overcome this problem we therefore use certain mso properties specific to Outlook which will allow us to define the size of the blocks and their positioning.

## Using mso properties to create a "get off the table" email

In this template, I use : 

-the mso-faux absolute technique to create absolute positioning, 
-br clear="all" to separate elements vertically,
-multiple columns
-joint use of margin, padding and border to create internal margins for blocks

1-For the absolute positioning of blocks, and the transformation of divs into tables in Outlook, I use frames here, with :

-mso-element-wrap:no-wrap-beside, 
-mso-left-element:center.


2-To set the size of frames in Outlook and turn everything into tables, I use :

-mso-element-frame-width

3- for blocs positionning in column, I use:

-mso-element-frame-hspace

### 3-Columns:

The first reflex would be to center the entire email using 
the mso-element-left:center property; This works great for single column emails.

But by applying the mso-element-wrap:no-wrap-beside properties to the blocks, they are floating and therefore would end up centered on each other.


we therefore start by applying the mso-element-wrap:no-wrap-beside property to our blocks. These are found at the left end of the email.

Then, we can reposition the blocks/columns horizontally using the mso-element-frame-hspace property, starting from the position: left: 0. 

For examle, if the first block/column is 300px wide, we therefore indicate mso-element-frame-hspace: 0. Then for the second mso-element-frame-hspace:300px (or more if we want some gutter between colums) and so on.

At the end, we can center everything with an enclosing div which we also center with the mso-element-frame-hspace property.

Visually it can work, as in the example template, but unfortunately the centering cannot be perfect: given the number of versions of Outlook, we cannot know the width of the viewing window for each case.

This approximate centering has also the disadvantage of being broken at 120 or 200 DPI. 

I have not yet found a solution to precisely and automatically center columns

### 4-Block margins

To add an internal margin to blocks.

With these frames, in Outlook the divs are not recognized as such, and all the linked style is replaced or repositioned in other tags.
The only way I found is to add padding to the enclosing div for all mail clients except Outlook
And for Outlook:

HTML margin on p tag for left and right margins. (in Outlook this adds left and right margins on the enclosing div  
Mso border and mso padding together on the p tag: (This is the only way to make the margins work here: border and padding are kept on the p tag, but this will also add the padding on the enclosing div, without that there is any visible effect.

5-br
As we can read it on the doc, Word inserts <br clear=ALL> after the Div element. This is necessary to turn off text wrapping in the browser display.without that, the frames are all nested inside each other and the layout becomes very complicated. A simple br and that solves the problem


References : 
https://github.com/M-J-Robbins/get-off-the-table
https://stigmortenmyre.no/mso/html/concepts/ofconstyletable.htm
