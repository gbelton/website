---
title: Using Prompts with SAS Enterprise Guide
author: ''
date: '2019-01-07'
slug: sas-prompts
categories: []
tags: []
image:
  caption: ''
  focal_point: ''
---
In my work environment, we have users who are not SAS programmers but need to get data from our SAS server. One way of dealing with this is to give them a program with comments like: 

```sas 
/* change this line to indicate which variables you want to include */
```
This only works if the user has at least a minimal knowledge of SAS programming. What actually happens in my office is that someone on the data team gets an email asking us to run the program for the user. A better option is to use SAS Prompts to give the user the ability to customize the program without needing to edit the program. Here’s how to do this using SAS Enterprise Guide. 

Prompts are a way to create macro variables and insert them into the program at runtime. As with any SAS program that uses macros, the first step is to create the program without macros. After you know the program works as intended, you can then “macro-fy” the program.

We’ll work with this sample program using Fisher’s Iris Data from the sashelp library. First, we will subset the data to use only one species of iris, then we’ll calculate some descriptive statistics for that species:

```sas
data work.iris;
      set sashelp.iris;
      where species = "Setosa";
run;
title "Fisher's Iris Data, Species = Setosa";
footnote "Run by Gerald";
proc means data=work.iris MEAN MEDIAN STD MAXDEC=2; 
run;
```  

The output looks like this:
![](/post/sas-prompts/snip1.jpg)

That's the output we were going for, now lets create some prompts and incorporate them into the program. We'll start with a simple one that lets the user enter their first name, and change the footnote to use the entered name.

In Enterprise Guide, navigate to the prompt manager. It's the fourth icon from the left in this group:

![](/post/sas-prompts/promptmanager.jpg)

Click on the "Add" button and this will pop up:

![](/post/sas-prompts/promptbox1.jpg)

"Name" is where you give your prompt a name. This has to be a valid SAS Macro Variable name, because the prompt creates a macro variable. Let's call this one "first_name." 

"Displayed Text" is the label that the user will see when they run the program. Put "First Name" into that box.

"Description" is optional. You can write instructions for the user here if you want to. In this case, the displayed "First Name" is probably all the instruction the user needs, so we'll leave the Description blank.

Next are some check boxes. For this example, check "Requires a non-blank value" and "Use prompt value throughout project." 

Now click on the "Prompt Type and Values" tab at the top of the box.  This is where you select the type of prompt, the method for populating the prompt,  etc. It looks like this:

![](/post/sas-prompts/promptbox2.jpg)

For this one we want the Prompt type to be Text, Method to be "User enters values," Single value, and Single Line. Click the OK button when you're done.

Now right-click on the Program icon in Enterprise Guide and select "Properties." Select "Prompts from the menu bar on the left, and click the "Add" button. Select "first_name" and click OK. The result looks like this:

![](/post/sas-prompts/prompts.jpg)

The final step is to incorporate the macro variable "first_name" into our program. In the footnote statement, change "Gerald" to "&first_name".  Now when you run the program, a box will pop up to prompt you for a First Name, and whatever you enter in that box will show up in your footnote. 

Let's add another prompt. This one will prompt for the species name instead of having it hard coded into the program. In the General tab of the Add New Prompt box, enter "species" for Name and "Species Name" for displayed text. Require non-blank values, and Use prompt value throughout project.  In the Prompt Type and Values tab, the prompt type is still "Text," but this time for Method we'll select "User selects values from a static list." Then click "Add" and add the values, like this:

![](/post/sas-prompts/species.jpg)

Then in the program change this:

```sas
where species = "Setosa";
```

to this:
```sas
where species = "&species";
```

Don't forget to right-click on the program icon and select "properties" to add the prompt to your program. 

Now when we run the program it looks like this:

![](/post/sas-prompts/running.jpg)

![](/post/sas-prompts/output.jpg)

For more on this topic, <a href="https://support.sas.com/resources/papers/proceedings17/0818-2017.pdf">click here</a>.

