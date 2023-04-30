# ANTLR For Windows

---

## Introduction

The purpose of this readme is to summarize my process in configuring ANTLR in Windows.
Navigate to the ANTLR downloads page:

https://www.antlr.org/download.html

And scroll to the bottom until you see:

ANTLR tool and Java Target

Complete ANTLR 4.12.0 Java binaries jar. Complete ANTLR 4.12.0 tool, Java runtime and ST 4.0.8, which lets you run the tool and the generated code. 

Download this complete binary, and proceed to the next step.

---

## Preparing Windows Environment

In order to get things working easily, what we want to do is make sure that Java is installed, and the java -version command returns a valid result in Powershell.

Once we've got that setup, we move our new .jar file we got from ANTLR and move it to some folder inside the folder you keep your SDKs. I named mine Javalib, and so the full path is: sdk\Javalib\antlr*.jar


Next you will want to create a folder in the same sdk folder called antlr4.

This is going to be where we put our script which runs the java command for us.

### Add CLASSPATH and Append PATH environment variables

	Open the Start menu and search for "environment variables".
    Click on "Edit the system environment variables".
    In the System Properties window, click on the "Environment Variables" button at the bottom.
    Under "User Variables", click the "New" button.
    Enter "CLASSPATH" for the variable name and the path you just established for the .jar file in the "Variable value" field. (including the full filename for the jar file)
    Click "OK" to save the variable. 


Now we are ready to append the path variable

	Open the Start menu and search for "environment variables".
    Click on "Edit the system environment variables".
    In the System Properties window, click on the "Environment Variables" button at the bottom.
    Under "User Variables", locate the PATH variable, and click it ensuring it's highlighted.
    Click the button "Edit..."
    Once the new window pops up, click the button "New"
    Add the path to your newly created antlr4 folder for example: C:\sdk\antlr4
    Click "OK" to save the variable. 


---

## Creating a Powershell Script

We have a java command that we want to run for antlr and for grun.

We simply do the following to create the prompt and run it in powershell.

To do so, create a file called antlr4.ps1:

	
	$ANTLR_CMD = "java org.antlr.v4.Tool"

	# Combine the ANTLR command and the arguments into a single string
	$command = "$ANTLR_CMD $($args -join ' ')"

	# Execute the command
	Invoke-Expression $command


(Note the name of the powershell variable doesn't matter)

Now repeat the process with a file called grun.ps1, with this updated script:

	
	$ANTLR_CMD = "java org.antlr.v4.gui.TestRig"
	Invoke-Expression $ANTLR_CMD
	# Combine the ANTLR command and the arguments into a single string
	$command = "$ANTLR_CMD $($args -join ' ')"
	
	# Execute the command
	Invoke-Expression $command


---

## Running the Tools


Now from within powershell, you should be able to simply do:
	
	>antlr4

Which should give:

	ANTLR Parser Generator  Version 4.12.0
	 -o ___              specify output directory where all output is generated
	 -lib ___            specify location of grammars, tokens files
	 -atn                generate rule augmented transition network diagrams
	 -encoding ___       specify grammar file encoding; e.g., euc-jp
	 -message-format ___ specify output style for messages in antlr, gnu, vs2005
	 -long-messages      show exception details when available for errors and warnings
	 -listener           generate parse tree listener (default)
	 -no-listener        don't generate parse tree listener
	 -visitor            generate parse tree visitor
	 -no-visitor         don't generate parse tree visitor (default)
	 -package ___        specify a package/namespace for the generated code
	 -depend             generate file dependencies
	 -D<option>=value    set/override a grammar-level option
	 -Werror             treat warnings as errors
	 -XdbgST             launch StringTemplate visualizer on generated code
	 -XdbgSTWait         wait for STViz to close before continuing
	 -Xforce-atn         use the ATN simulator for all predictions
	 -Xlog               dump lots of logging info to antlr-timestamp.log
	 -Xexact-output-dir  all output goes into -o dir regardless of paths/package



Similarly, with grun we do:

	>grun

And we get:


	java org.antlr.v4.gui.TestRig GrammarName startRuleName
	  [-tokens] [-tree] [-gui] [-ps file.ps] [-encoding encodingname]
	  [-trace] [-diagnostics] [-SLL]
	  [input-filename(s)]
	Use startRuleName='tokens' if GrammarName is a lexer grammar.
	Omitting input-filename makes rig read from stdin.


We should now be able to generate from our antlr grammar files!!!

--- 

## Allowing PowerShell scripts to run:

You may also need to change the PowerShell script execution policy using the Windows Settings app. Here's how:

Open the Windows Settings app by clicking on the Start menu and selecting the gear icon, or by pressing the Windows key + I.

Search for "Developer Settings"

Under the "Use developer features" section, select the "Developer mode" radio button.

Click on "Yes" when prompted to confirm that you want to enable developer mode.

Open a PowerShell console with administrator privileges.

Check the current execution policy by running the following command:

	Get-ExecutionPolicy

This will return the current script execution policy for your system.

If the execution policy is set to "Restricted", you will need to change it to "RemoteSigned" or "Unrestricted". To set the execution policy to "RemoteSigned", run the following command:

	Set-ExecutionPolicy RemoteSigned

Note, this could potentially put your computer at risk.

---