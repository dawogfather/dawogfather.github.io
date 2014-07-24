---
published: false
---

## Sublime Text Editor
By far the best text editor I have ever used is sublime text. 

One of the best things about it is that it is available for free (you can and should buy it if you like it http://www.sublimetext.com) and it works on almost all operating systems. 

One of the many many cool features it has is the ability to create and define it's own set of "Build" systems. These can work based on the type of file that is open in sublimetext itself.  

## Madcow Test Automation
I helped to create an open source DSL based testing tool called Madcow. It relies on Webtest and java style .properties files to test webpages. There is also currently a second iteration of it which utilises Selenium Webdriver instead which is under continual development. Check it out here http://madcow.4impact.net.au or on github http://github.com/4impact/madcow. 

For those of you still stuck using Madcow Classic however the following little build system script for Sublime may come in handy. It attempts to run the currently open madcow test against the "local" environment. It was made and tested on windows but it should work on both Windows and unix based machines. 

{%highlight}
{
    "cmd": ["runMadcow.sh", "-u", "local", "-t", "$file_base_name"],
    "file_regex": "^[ ]*File \"(...*?)\", line ([0-9]*)",
    "selector": "source.java-props",
    "working_dir": "${project_path:${folder}}",

    "windows":
    {
		"cmd": ["runMadcow.bat", "-u", "local", "-t", "$file_base_name"],
    }
}
{%endhighlight}

You can add it to Sublime's Build System by simply clicking... 
Tools -> Build System -> New Build System 
pasting the above code and then doing 
File -> Save As -> RunMadcowLocal.sublime-build

Then everytime you open a madcow test all you need to do is click F7 or Ctrl B to run it locally. 