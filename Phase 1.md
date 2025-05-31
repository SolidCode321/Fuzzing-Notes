1st phase involve mainly setting up things and installing everything involved.
We will also understand some basic concepts:
- **What is Fuzzing?**
	**Fuzzing** is a software testing technique where you:
	- Automatically generate lots of **unexpected, malformed, or random inputs**
	- Feed them to a program (like an antivirus)
	- Observe for **crashes**, **hangs**, **unexpected behavior**, or **interesting outputs**
	In our case: We’ll fuzz ClamAV by giving it weird files and watch how it reacts.
- Types of Fuzzing?
	Fuzzing is mainly of three types Black-box, White-box and Grey-box, out of which we will only require Black-box fuzzing. But here are their meanings:
	1. **Black-box**: You don't know the internals of the program thus are just trying to test the input and output relations.
	2. **White-box**: You know the code, and use the program structure to guide the fuzzing.
	3. **Grey-box**:You get feedback form the execution.
- File format fuzzing?
	We will mainly fuzz file formats like .pdf, .docx, .exe, .zip, .jpg, etc.
	In short we will be generating syntactically valid but weird files.
- Fuzzing workflow Overview
	`[Input Generator] → [Save to File] → [Run clamscan] → [Check Results]`
	Basically the steps that we will code are as follows:
	1. Generate a random(or structured) file content
	2. Save it as a file
	3. Run clamscan on it
		When you run a clamscan with the command
		`clamscan testfile.pdf`
		we may get 
			0: Clean
			1: Virus/malware detected
			2: Error (Parse error or crash etc.) -> interesting for fuzzing
	4. Observe: Did it crash? Did it detect anything?
		And also why we detected it has to be studied.


### Requirements
Then given below are the tools that we will be using throughout the project.
- clamscan: This is a clamav scanner
- fuzzingbook: python fuzzing utilities
- subprocess: To run clamscan
- tempfile: To save fuzzed files safely
- os, shutil: For file management


## Conditions for knowing that the fuzzing is working
- ClamAV crashes either hangs or crashes
- ClamAV throws unexpected errors
- You detect malware when there's no malware
- You generate files that evade detection or cause exceptions