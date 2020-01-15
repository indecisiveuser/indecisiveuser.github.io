---
layout: post
Title: Final Project Daily Log
---
Welcome to my Final Project Daily Log, in which I attempt to explain my creation of a shuffle parameter in scikit-learn's cross_val_score function.<br><br>

First, for some background:<br>
When performing regressions using train/test split or cross validation, shuffling your data is always a good idea. Why? Let's consider the case in which the data are sorted. Without shuffling, our training dataset would be very different from our testing dataset, which would really mess up our results. However, shuffling takes care of this! <br><br>

Now, there are multiple ways to shuffle your data before splitting. The (very) easy way is to just say shuffle=True on models that have shuffle methods. With functions that do not have shuffle methods (like cross_val_score) it takes a bit (albiet not that much) more work. In my experience, I have to create a dataframe with the data, sort it using the built-in pandas .sample function, and then separate it out into an input dataframe and a response variable. But who wants to do extra work when they can just type shuffle=True? No one, that's who.<br><br>


Below is a log of my daily progress, including:

1.) What I worked on today

2.) What I learned

3.) Any road blocks or questions that I need to get answered

4.) What my goals are for tomorrow

**January 7th**<br>

Today I tried to locally install scikit-learn. TL;DR: it was a mess, but I finally did it!<br><br>

Finding the instructions were easy enough: I just had to visit the scikit-learn GitHub page and then click on the page for 'contributers.' For reference, [here](https://scikit-learn.org/stable/developers/contributing.html) are the instructions that I followed.<br><br>

It looked easy enough. I already had a GitHub account, and was familiar with 'forking' projects. Plus, I thought I would only have to write 8 or so lines of code to complete the download (I was really wrong). So, I tried the first line. It didn't work.<br><br>

<img src="/images/jan7-1.png" width="800"/><br><br>

Since the repository definitely exists, I turned to the other error about the publickey. Apparently, in order to access the repo, I needed to create my own (and thus private) key. These keys are known as Secure Shell (or SSH) and I had never heard of them before this instance. Yipee. Anyways, I managed to find the solution to this problem on the GitHub website, which has nice step-by-step instructions on [generating your own SSH key](https://help.github.com/en/github/authenticating-to-github/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent) and [adding it to your GitHub account](https://help.github.com/en/github/authenticating-to-github/adding-a-new-ssh-key-to-your-github-account). Adding it to my GitHub account was very straightforward. I checked to see if GitHub recognises my new SSH key...<br><br>

<img src="/images/jan7-2.png" width="800"/><br><br>

Nope. But when I open up a new Terminal window: <br><br>

<img src="/images/jan7-3.png" width="800"/><br><br>

Success! The next few lines on the scikit-learn instructions worked without error until I tried to install sci-kit learn. In a sea of red errors, here is what I found:<br><br>

<img src="/images/jan7-4.png" width="800"/><br><br>

Apparently, I needed to install gcc, also known as the GNU Compiler Collection. To do so, I first tried to brew install it, which gave me an error that gcc needed to be installed as a binary package and built from the source. However, it also told me how to install command line tools (xcode), which I promptly did. Finally, I found via Google an [old instruction manual](http://www.cs.millersville.edu/~gzoppetti/InstallingGccMac.html) for downloading gcc on a mac. These instructions worked for the most part. Since I had already downloaded xcode, I first downloaded the correct version of MacPorts using the .pkg installer and then sudo port installed the most recent version of gcc (gcc9 in my case). Then I was finally able to install the developer version of sci-kit learn. Yay!<br><br>

<img src="/images/jan7-5.png" width="800"/><br><br>

For brevity, a compilation of the instructions that worked for me:

1) Create an account on GitHub if you do not already have one.

2) Fork the project repository: click on the ‘Fork’ button near the top of the page. This creates a copy of the code under your account on the GitHub user account.

3) Paste the text below, substituting in your GitHub email address.

$ ssh-keygen -t rsa -b 4096 -C "your_email@example.com"
This creates a new ssh key, using the provided email as a label.

4) When you're prompted to "Enter a file in which to save the key," press Enter. This accepts the default file location.

> Enter a file in which to save the key (/Users/you/.ssh/id_rsa): [Press enter]

5) At the prompt, type a secure passphrase.

> Enter passphrase (empty for no passphrase): [Type a passphrase]
> Enter same passphrase again: [Type passphrase again]

6) Start the ssh-agent in the background.

$ eval "$(ssh-agent -s)"
> Agent pid *****

7) For macOS Sierra 10.12.2 or later: need to modify your ~/.ssh/config file to automatically load keys into the ssh-agent and store passphrases in the keychain.

Open a new Terminal window

$ cd ~/.ssh/config

8) touch config

9) vim config

10) press i (should go into Insert Mode)

11) Copy and paste

Host *
  AddKeysToAgent yes
  UseKeychain yes
  IdentityFile ~/.ssh/id_rsa


into the file.

12) Press esc and then type :wq and then press enter


13) Add your SSH private key to the ssh-agent and store your passphrase in the keychain. If you created your key with a different name, or if you are adding an existing key that has a different name, replace id_rsa in the command with the name of your private key file.

$ ssh-add -K ~/.ssh/id_rsa

14) Copy the SSH key to your clipboard.

If your SSH key file has a different name than the example code, modify the filename to match your current setup. When copying your key, don't add any newlines or whitespace.

$ pbcopy < ~/.ssh/id_rsa.pub

15) IN GITHUB: In the upper-right corner of any page, click your profile photo, then click Settings.

16) In the user settings sidebar, click SSH and GPG keys.

17) Click New SSH key or Add SSH key.

18) In the "Title" field, add a descriptive label for the new key. For example, if you're using a personal Mac, you might call this key "Personal MacBook Air".

19) Paste your key into the "Key" field.

20) Click Add SSH key.

If prompted, confirm your GitHub password.

21) Clone your fork of the scikit-learn repo from your GitHub account to your local disk:

$ git clone git@github.com:YourLogin/scikit-learn.git
$ cd scikit-learn

22) Install the development dependencies:

$ pip install cython pytest pytest-cov flake8

23) Install x-code (Note: This will take a while)

$ xcode-select --install

24) IF YOU HAVE A MAC: Download and click through the package installer of the correct version of MacPorts[here](https://www.macports.org/install.php) (Note: this will also take a while, ~ 1 hour)

25) Open a *new* Terminal window

$ sudo port install gcc9

Note: Only gcc9 and above support the version of xcode you downloaded in step 23. Also, gcc9 is the most recent release as of 01/2020. Check out the [gcc docs](https://gcc.gnu.org/) for more information.

26) Install scikit-learn in editable mode:

$ pip install --editable .

27) Add the upstream remote. This saves a reference to the main scikit-learn repository, which you can use to keep your repository synchronized with the latest changes:

$ git remote add upstream https://github.com/scikit-learn/scikit-learn.git

28) You should now have a working installation of scikit-learn, and your git repository properly configured. The next steps now describe the process of modifying code and submitting a PR:

Synchronize your master branch with the upstream master branch:

$ git checkout master
$ git pull upstream master
Create a feature branch to hold your development changes:

$ git checkout -b my_feature
and start making changes. Always use a feature branch. It’s good practice to never work on the master branch!

Tomorrow, I'm excited to explore sci-kit learn!

**January 8th**<br>

Today, I mostly read the scikit-learn docs with a focus on the ShuffleSplit class, the StratifiedShuffleSplit class, and the cross validation function. While doing so, I attempted to find a way add a shuffle method to the cross_validation function. The place I am most excited about is the check_cv function located in \_split.py located under model.selection. Here is a screenshot of the scikit-learn code:<br><br>

<img src="/images/jan8.png" width="800"/><br><br>

If I can pass a shuffle parameter into this function, it can be passed into the Kfold/StratifiedKfold cross validator, _which both already have shuffle methods_.
However, I'm a bit confused regarding the old vs new style cv objects. What is the difference?

Tomorrow, I'm planning to start coding :)

**January 9th**

Today, I started working on the check_cv class by adding a shuffle parameter. When the value set for shuffle is a boolean, I believe my modifications will work. However, I'm not so sure about the exceptions (eg when shuffle is not a boolean or when cv is not a type that will work). I will have to start testing tomorrow...Also, I'm not convinced that check_cv is the only place cross validates the data, so tomorrow I'm going to look at the cross_validation function a bit more closely to try to find any additional ways that I can add a shuffle method there.

**January 10th**

Talking to Lauren about my plans today was very useful to me. I completely agree with her comments that before trying to add a shuffle method, I should probably start contributing in other, less noticable ways to build trust and also gain a greater understanding of sci-kit learn and open-source contributing in general. Therefore, I have taken a look at [this link](https://github.com/scikit-learn/scikit-learn/issues?q=is%3Aopen+is%3Aissue+label%3A%22good+first+issue%22) which lists the issues that are good for first-time contributers. I think I may try to 'claim' one Monday (the 13th) and begin. I just hope that I will have something to show for the presentation during finals....

**January 13th**

Today, I started work on fixing some documentation issues mentioned in the 'good first issue' category of sci-kit learn's issues tab. The link to this issues is [here](https://github.com/scikit-learn/scikit-learn/issues/15761). I worked on the \_validation under model_selection to streamline documentation, specifically taking away 'optional' in the docs. I am hoping to finish this up tomorrow, and hopefully submit a Pull Request.<br><br>

However, I do have some questions:<br>
1) If the base function has y and a default value of None, should all functions in the same vein have the same? Right now, some have y=None and some only have y, but the docs still said they were 'optional'<br>
2) If a function doesn't have a parameters section, should I add one?<br>
3) How do I deal with 'optional' when it is in reference to the possible outputs of a function?

**January 15th**

**January 16th**

**January 17th**