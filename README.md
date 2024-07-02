# Get off the table email in 2024

Based on Mark Robbins' incredible work on the subject of coding emails without using tables, I tried at my level to push the research to find solutions to the problems mentioned on his [issue page](https://github.com/M-J-Robbins/get-off-the-table/issues)

The major drawback of Outlook is that it does not recognize certain CSS properties, especially when they are applied to a div. Whatever tag is used, it is most of the time transformed into a simple paragraph in Outlook.

To overcome this problem we therefore use certain mso properties, specific to Outlook, which will allow us to define the size of the blocks and their positioning, while using simple divs.

In this template, I will use : 

- the mso-faux absolute technique to create absolute positioning, 
- br clear="all" to separate elements vertically. When the frame does not allow text to wrap around it , Word inserts <br clear=ALL> after the Div element. This is necessary to turn off text wrapping in the browser display. In the outlook output code, we can also see `style="mso-break-type:section-break"` which is used to create sections in word.
- multiple columns
- joint use of margin, padding and border to create internal margins for blocks


## Using Word frames to create a "get off the table" email

As written in the documentation :

Word allows the creation of floating frames in Web documents, even though there are currently no HTML elements to support this functionality. Word stores the frame or drop cap settings in the HTML file using styles and emulates the appearance of the frame or drop cap in the browser using HTML tables. When saving as HTML, Word inserts the contents of the frame into a single-cell Table element for appropriate browser display. This Table element is contained in a Div element that stores most of the frame settings. 
Word uses the following styles to save frame settings to HTML: 

### the mso-properties

- mso-element:frame
- mso-element-wrap
- mso-element-left
- mso-element-top
- mso-element-frame-width
- mso-element-frame-height
- mso-element-frame-hspace
- mso-element-frame-vspace
- mso-element-anchor-horizontal
- mso-element-anchor-vertical
- mso-element-anchor-lock

To create an email template, we will only need a few properties, notably those which allow us to transform divs into tables and to position it.

- mso-element-wrap:no-wrap-beside or around
- mso-left-element:center.
- mso-element-frame-width
- mso-element-frame-hspace

#### Mso-element-left

mso-left-element is used for positionning. It takes several values: `center,<length>,inside,left,outside,right`.
Among those different values, the center value allows the creation of an array.

So used alone, this code :
```
<div style="mso-element-left:center">hello</div>
```
Becomes, in Outlook :
```
<div style="mso-element:frame;mso-element-wrap:auto;mso-element-anchor-horizontal:
column;mso-element-left:center;mso-height-rule:exactly">
<table cellspacing="0" cellpadding="0" hspace="0" vspace="0" align="center">
 <tbody><tr>
  <td valign="top" align="left" style="padding-top:0cm;padding-right:0cm;
  padding-bottom:0cm;padding-left:0cm">
  <p class="MsoNormal" style="background:purple;mso-element:frame;mso-element-wrap:
  auto;mso-element-anchor-horizontal:column;mso-element-left:center;mso-height-rule:
  exactly"><span style="color:white;mso-color-alt:windowtext">hello </span><o:p></o:p></p>
  </td>
 </tr>
</tbody></table>
</div>
```

#### Mso-element-wrap

The mso-element-wrap sets the wrapping of the mso-element, and works like CSS float. It takes the following values : around,auto,none,no-wrap-beside.

around works like float, other elements will wrap around it
auto Sets to around. It's the default value and is similar to around. Both can be used to create columns.
none allows absolute positionning and can be used to create faux absolute layout that works in Windows 10 &11 Mails.
no-wrap-beside puts the element on it’s own line, like a block element.

So use alone, this code :
```
<div style="background:purple;mso-element-wrap:no-wrap-beside">hello</div>
```

In Outlook, becomes :

```
<div style="mso-element:frame;mso-element-wrap:no-wrap-beside;mso-element-anchor-horizontal:
column;mso-height-rule:exactly">
<table cellspacing="0" cellpadding="0" hspace="0" vspace="0">
 <tbody><tr>
  <td valign="top" align="left" style="padding-top:0cm;padding-right:0cm;
  padding-bottom:0cm;padding-left:0cm">
  <p class="MsoNormal" style="background:purple;mso-element:frame;mso-element-wrap:
  no-wrap-beside;mso-element-anchor-horizontal:column;mso-height-rule:exactly"><span style="color:white;mso-color-alt:windowtext"hello</span><o:p></o:p></p>
  </td>
 </tr>
</tbody></table>
</div>
```

#### Mso-element-frame-width

mso-element-frame-width is used to set the width of the mso-element.

So used alone, this code :
```
<div style="mso-element-frame-width:200px">hello</div>
```

becomes

```
<div style="mso-element:frame;mso-element-frame-width:150.0pt;mso-element-wrap:
auto;mso-element-anchor-horizontal:column;mso-height-rule:exactly">
<table cellspacing="0" cellpadding="0" hspace="0" vspace="0" width="200">
 <tbody><tr>
  <td valign="top" align="left" style="padding-top:0cm;padding-right:0cm;
  padding-bottom:0cm;padding-left:0cm">
  <p class="MsoNormal" style="background:purple;mso-element:frame;mso-element-frame-width:
  150.0pt;mso-element-wrap:auto;mso-element-anchor-horizontal:column;
  mso-height-rule:exactly"><span style="color:white;mso-color-alt:windowtext">hello
  </span><o:p></o:p></p>
  </td>
 </tr>
</tbody></table>
</div>
```

#### mso-element-frame-hspace

mso-element-frame-hspace is used to create a horizontal space to the left and right of the frames, which will be used to create margins between the columns. We can also use mso-para-margin-left which is the equivalent of margin-left. 

## Notes on mso-properties

We see here that Outlook takes the code inserted into the div and automatically adds certain other properties:
Outlook recognizes that we are using frames so inserts `mso-element:frame`; Without declaring the `mso-element-wrap` it automatically inserts it with `auto` as default. The two properties `mso-element-anchor-horizontal` and `mso-height-rule` are systematically added.

We also notice that these value properties are duplicated in a paragraph tag.

If we surround the text with a paragraph tag with style, it will take the style and also add the style of the frame. We notice this while it no longer indicates the width.

```
<p style="margin:0cm;background:purple;mso-element:frame;mso-element-wrap:around;mso-element-anchor-horizontal:column;mso-height-rule:exactly"><span style="color:white;mso-color-alt:windowtext">hello</span><o:p></o:p></p>
```

From these elements of knowledge, we will therefore be able to create a template with columns


### How to create Columns with frames ?

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

Here is the code to create a two-column layout

```
 <div style="mso-element-wrap:no-wrap-beside;mso-element-frame-hspace:160pt">
    <div style="display:inline-block; margin: 0 -1px; width:100%; min-width:200px; max-width:330px; mso-width:300px;vertical-align:top;mso-element-left:0px;mso-element-frame-width:300px;">
       <img src="" style="">  
      <div style="font-family: sans-serif; font-size: 15px; line-height: 20px; color: #555555; padding-top: 10px;">
        <p style="margin: 0;">hello</p>
      </div>
    </div>
    <div style="display:inline-block; margin: 0 -1px; width:100%; min-width:200px; max-width:330px; vertical-align:top;mso-element-left:320px;mso-element-frame-width:300px">
        <img src="" style="">
      <div style="font-family: sans-serif; font-size: 15px; line-height: 20px; color: #555555; padding-top: 10px;">
        <p style="margin: 0;">hello</p>
      </div>
    </div>
  </div>
```

And in Outlook, the output code becomes : 

```
<div style="mso-element:frame;mso-element-frame-width:225.0pt;mso-element-frame-hspace:
160.0pt;mso-element-wrap:no-wrap-beside;mso-element-anchor-horizontal:column;
mso-element-left:.05pt;mso-height-rule:exactly">
<table cellspacing="0" cellpadding="0" hspace="0" vspace="0" width="513">
 <tbody><tr>
  <td valign="top" align="left" style="padding-top:0cm;padding-right:160.0pt;
  padding-bottom:0cm;padding-left:160.0pt">
  <p class="MsoNormal" align="center" style="text-align:center;background:#F2F2F2;
  vertical-align:top;mso-element:frame;mso-element-frame-width:225.0pt;
  mso-element-frame-hspace:160.0pt;mso-element-wrap:no-wrap-beside;mso-element-anchor-horizontal:
  column;mso-element-left:.05pt;mso-height-rule:exactly"><span lang="EN" style="mso-fareast-font-family:&quot;Times New Roman&quot;;color:black;mso-color-alt:
  windowtext;mso-ansi-language:EN"><img width="310" id="_x0000_i1025" src="https://40daystemp3.testi.at/placeholder/310" style="background-color:
  #DDDDDD;color:#555555;font-family:sans-serif;font-size:15px;height:auto;
  line-height:20px;max-width:310px;width:100%" border="0"></span><span lang="EN" style="mso-fareast-font-family:&quot;Times New Roman&quot;;mso-ansi-language:EN"><o:p></o:p></span></p>
  <p align="center" style="margin:0cm;text-align:center;line-height:15.0pt;
  background:#F2F2F2;vertical-align:top;mso-element:frame;mso-element-frame-width:
  225.0pt;mso-element-frame-hspace:160.0pt;mso-element-wrap:no-wrap-beside;
  mso-element-anchor-horizontal:column;mso-element-left:.05pt;mso-height-rule:
  exactly"><span lang="EN" style="font-size:11.5pt;color:#555555;mso-ansi-language:
  EN">hello<o:p></o:p></span></p>
  </td>
 </tr>
</tbody></table>
</div>

<div style="mso-element:frame;mso-element-frame-width:225.0pt;mso-element-frame-hspace:
160.0pt;mso-element-wrap:no-wrap-beside;mso-element-anchor-horizontal:column;
mso-element-left:240.0pt;mso-height-rule:exactly">
<table cellspacing="0" cellpadding="0" hspace="0" vspace="0" width="513">
 <tbody><tr>
  <td valign="top" align="left" style="padding-top:0cm;padding-right:160.0pt;
  padding-bottom:0cm;padding-left:160.0pt">
  <p class="MsoNormal" align="center" style="text-align:center;background:#F2F2F2;
  vertical-align:top;mso-element:frame;mso-element-frame-width:225.0pt;
  mso-element-frame-hspace:160.0pt;mso-element-wrap:no-wrap-beside;mso-element-anchor-horizontal:
  column;mso-element-left:240.0pt;mso-height-rule:exactly"><span lang="EN" style="mso-fareast-font-family:&quot;Times New Roman&quot;;color:black;mso-color-alt:
  windowtext;mso-ansi-language:EN">
<img width="310" id="_x0000_i1026" src="https://40daystemp3.testi.at/placeholder/310" style="background-color:
  #DDDDDD;color:#555555;font-family:sans-serif;font-size:15px;height:auto;
  line-height:20px;max-width:310px;width:100%" border="0"></span><span lang="EN" style="mso-fareast-font-family:&quot;Times New Roman&quot;;mso-ansi-language:EN"><o:p></o:p></span></p>
  <p align="center" style="margin:0cm;text-align:center;line-height:15.0pt;
  background:#F2F2F2;vertical-align:top;mso-element:frame;mso-element-frame-width:
  225.0pt;mso-element-frame-hspace:160.0pt;mso-element-wrap:no-wrap-beside;
  mso-element-anchor-horizontal:column;mso-element-left:240.0pt;mso-height-rule:
  exactly"><span lang="EN" style="font-size:11.5pt;color:#555555;mso-ansi-language:
  EN">hello<o:p></o:p></span></p>
  </td>
 </tr>
</tbody></table>
</div>
```


#### second example : file:index-2.html

Another solution to make columns work in Outlook, is to use the mso-element-wrap:around/auto property.

If we have for example 2 or more columns:
- define the width of the blocks using : mso-element-frame-width:xxxpx;
- Set mso-element-wrap:around on the blocks, the last one doesn't need it.
- for the gutter : mso-element-frame-hspace:xxxpx adds space on the left and right side, so you have to choose on which block you add it. You can also use mso-para-margin-left:xxpx.

  In the case of multiple rows containing multiple columns, you can wrap each row with a mso-element-wrap:no-wrap-beside.

  
Thanks to the mso-element-wrap:around or auto property, the elements will stick one after the other, thus creating columns. We therefore no longer need to position them relative to a left:0 position which is much more practical

The problem of global column centering is still not resolved here. 

Here is the code used to create columns, with mso-element-wrap:around

```
<div style="mso-element-wrap:no-wrap-beside">
    <div style="display:inline-block; margin: 0 -1px; width:100%; min-width:200px; max-width:330px; mso-width:300px;vertical-align:top;mso-element-wrap:around;mso-element-frame-width:300px;">      
        <img src="https://40daystemp3.testi.at/placeholder/310" width="310" height="" border="0" alt="" style="width: 100%; max-width: 310px; height: auto; background: #dddddd; font-family: sans-serif; font-size: 15px; line-height: 20px; color: #555555;" >
      <div style="font-family: sans-serif; font-size: 15px; line-height: 20px; color: #555555; padding-top: 10px;">
        <p style="margin: 0;">Maecenas sed ante pellentesque, posuere leo id, eleifend dolor. Class aptent taciti sociosqu ad litora torquent per conubia nostra, per inceptos himenaeos.</p>
      </div>
    </div>
    <div style="display:inline-block; margin: 0 -1px; width:100%; min-width:200px; max-width:330px; vertical-align:top;mso-element-frame-width:300px;mso-element-frame-hspace:20px">
      <img src="https://40daystemp3.testi.at/placeholder/310" width="310" height="" border="0" alt="" style="width: 100%; max-width: 310px; height: auto; background: #dddddd; font-family: sans-serif; font-size: 15px; line-height: 20px; color: #555555;" >
      <div style="font-family: sans-serif; font-size: 15px; line-height: 20px; color: #555555; padding-top: 10px;">
        <p style="margin: 0;">Maecenas sed ante pellentesque, posuere leo id, eleifend dolor. Class aptent taciti sociosqu ad litora torquent per conubia nostra, per inceptos himenaeos.</p>
      </div>
    </div>
  </div>
```

And in Outlook, the output code becomes : 

```
<div style="mso-element:frame;mso-element-frame-width:225.0pt;mso-element-wrap:
around;mso-element-anchor-horizontal:column;mso-height-rule:exactly">
<table cellspacing="0" cellpadding="0" hspace="0" vspace="0" width="300" align="left">
 <tbody><tr>
  <td valign="top" align="left" style="padding-top:0in;padding-right:0in;
  padding-bottom:0in;padding-left:0in">
  <p class="MsoNormal" style="vertical-align:top;mso-element:frame;mso-element-frame-width:
  225.0pt;mso-element-wrap:around;mso-element-anchor-horizontal:column;
  mso-height-rule:exactly"><img width="310" id="_x0000_i1025" src="https://40daystemp3.testi.at/placeholder/310" style="background-color:
  #DDDDDD;color:#555555;font-family:sans-serif;font-size:15px;height:auto;
  line-height:20px;max-width:310px;width:100%" border="0"><o:p></o:p></p>
  <p style="margin:0in;line-height:15.0pt;vertical-align:top;mso-element:frame;
  mso-element-frame-width:225.0pt;mso-element-wrap:around;mso-element-anchor-horizontal:
  column;mso-height-rule:exactly"><span style="font-size:11.5pt;font-family:
  &quot;Arial&quot;,sans-serif;color:#555555">hello<o:p></o:p></span></p>
  </td>
 </tr>
</tbody></table>
</div>

<div style="mso-element:frame;mso-element-frame-width:225.0pt;mso-element-frame-hspace:
15.0pt;mso-element-wrap:no-wrap-beside;mso-element-anchor-horizontal:column;
mso-height-rule:exactly">
<table cellspacing="0" cellpadding="0" hspace="0" vspace="0" width="320">
 <tbody><tr>
  <td valign="top" align="left" style="padding-top:0in;padding-right:15.0pt;
  padding-bottom:0in;padding-left:15.0pt">
  <p class="MsoNormal" style="vertical-align:top;mso-element:frame;mso-element-frame-width:
  225.0pt;mso-element-frame-hspace:15.0pt;mso-element-wrap:no-wrap-beside;
  mso-element-anchor-horizontal:column;mso-height-rule:exactly"><img width="310" id="_x0000_i1026" src="https://40daystemp3.testi.at/placeholder/310" style="background-color:#DDDDDD;color:#555555;font-family:sans-serif;
  font-size:15px;height:auto;line-height:20px;max-width:310px;width:100%" border="0"><o:p></o:p></p>
  <p style="margin:0in;line-height:15.0pt;vertical-align:top;mso-element:frame;
  mso-element-frame-width:225.0pt;mso-element-frame-hspace:15.0pt;mso-element-wrap:
  no-wrap-beside;mso-element-anchor-horizontal:column;mso-height-rule:exactly"><span style="font-size:11.5pt;font-family:&quot;Arial&quot;,sans-serif;color:#555555">hello<o:p></o:p></span></p>
  </td>
 </tr>
</tbody></table></div>
```

As you can see, mso-element-wrap:around adds the align="left" property to the table, and there is no way to override or replace this "left" value. We cannot therefore refocus the blocks which end up on the left of the window. We can't push them to the right either with mso-para-margin-left or mso-element-frame-hspace, this completely breaks the rendering. To date I have not found any solution to this problem.


## How to create frame inner margins ?

To add an internal margin to blocks.

With these frames, in Outlook the divs are not recognized as such, and all the linked style is replaced or repositioned in other tags.
The only way I found is to add padding to the enclosing div for all mail clients except Outlook
And for Outlook:

HTML margin on p tag for left and right margins. (in Outlook this adds left and right margins on the enclosing div  
Mso border and mso padding together on the p tag: (This is the only way to make the margins work here: border and padding are kept on the p tag, but this will also add the padding on the enclosing div, without that there is any visible effect.

## separate frames vertically ?

As we can read it on the doc, Word inserts <br clear=ALL> after the Div element. This is necessary to turn off text wrapping in the browser display.without that, the frames are all nested inside each other and the layout becomes very complicated. A simple br and that solves the problem.

```
<br clear="all">
```

In one of his exchanges on [his page](https://github.com/M-J-Robbins/get-off-the-table/issues/3), I see that Mark and heyitstowler had also noted this code, which can be found in the Outlook rendered code:

```
<br style="mso-break-type:section-break">
```


I continue my research, just to try to get to the end, knowing that it will certainly lead nowhere :). The release of One Outlook is coming in a few years, and we will normally no longer need to find such hacks to code on Outlook


#### References : 

https://github.com/M-J-Robbins/get-off-the-table

https://stigmortenmyre.no/mso/html/concepts/ofconstyletable.htm
