# Installing-Ruby-On-M1-Macs
So, you’ve got an M1 Mac and you’re ready to roll with Ruby. You might have hit a few bumps along the way, but worry not - we’ve all been there. This guide is all about getting you through the rough patches and straight to coding bliss on MacOS Sonoma.


# Introduction
Running Ruby on an M1 Mac can sometimes be a tricky affair, especially with the various dependencies and configurations needed for a successful installation. This guide is tailored to help you navigate through the installation process on MacOS Sonoma, highlighting common issues and providing straightforward solutions to get Ruby up and running on your machine.

## Common Installation Errors
You might have encountered errors similar to the following during your installation attempts:

```ruby-build: using readline from homebrew
ruby-build: using gmp from homebrew

BUILD FAILED (macOS 14.0 using ruby-build 20231009)
compiling ossl_x509name.c
compiling ossl_x509req.c
compiling ossl_x509revoked.c
compiling ossl_x509store.c
linking shared-object openssl.bundle
ld: warning: ignoring duplicate libraries: '-lruby.3.2'
1 warning generated.
linking shared-object ripper.bundle
ld: warning: ignoring duplicate libraries: '-lruby.3.2'
make: *** [build-ext] Error 2 ```
 
Seeing this on your terminal? You’re not alone. Let’s smash those errors together.

## Prerequisites
I’m guessing you’ve got Homebrew somewhere on your machine. If not, no biggie - we’ll get to that. If it's already there, run the following command with an expected error:
```brew upgrade```

```
Error: Cannot install in Homebrew on ARM processor in Intel default prefix (/usr/local)!
...```

This error is a common pitfall and a major reason for installation failures.

## Step 1: Update Xcode
First things first, let's make sure XCode is up-to-date
```xcode-select --install```

## Step 2: Install Homebrew
1. Navigate to [Homebrew's website](https://brew.sh/)	
2. Click on the highlighted option for M1 Macs, which should direct you to their GitHub page.
![homebrew](https://github.com/danielfrance/Installing-Ruby-On-M1-Macs/assets/4622440/6c3b33a1-7ed7-4308-b8b5-cc642b85457d)
4. Under assets, choose your preferred installer. The Homebrew-****.pkg option is recommended for its simplicity.
5. Run the installer and disregard the pop-up command about the Homebrew path for now. We will address this in the following steps.
6. Run `brew doctor` and get rid of the files it warns about.

## Step 3: Install [rbenv](https://github.com/rbenv/rbenv) and [ruby-build](https://github.com/rbenv/ruby-build/wiki#suggested-build-environment)
Now, install rbenv and other necessary tools.  `rbenv` is a Ruby version manager that allows us to run different versions of Ruby locally, globally, and at the application specific level.  `ruby-build` is a tool that downloads and compiles various versions of Ruby and helps us with dependency management

```
brew install rbenv ruby-build rbenv-gemset rbenv-vars
```

## The Next Steps Are Important!! Don't gloss over them

### For Ruby 2.x - 3.0:
```
brew install openssl@1.1 readline libyaml gmp
export RUBY_CONFIGURE_OPTS="--with-openssl-dir=$(brew --prefix openssl@1.1)"
```

### For Ruby 3.1 and above (requires OpenSSL 3):

```
brew install openssl@3 readline libyaml gmp
export RUBY_CONFIGURE_OPTS="--with-openssl-dir=$(brew --prefix openssl@3)"
```

### For Ruby 3.2 and above, you will need the Rust compiler to enable YJIT:
```
brew install rust 
```

## Step 4: Configure Shell Environment
Open your `~/.zshrc` file and add:

```
export PATH=/opt/homebrew/bin:$PATH
eval "$(rbenv init -)" 
```

We’re doing this because Homebrew’s got a new home in /opt/homebrew, and we need to let `rbenv` know where to look. Your `~/.zshrc` should look something like this:


```
export PATH=/opt/homebrew/bin:$PATH

# For Ruby 2.x - 3.0
export RUBY_CONFIGURE_OPTS="--with-openssl-dir=$(brew --prefix openssl@1.1)"
# ************* OR **************
# For Ruby 3.1 and above
export RUBY_CONFIGURE_OPTS="--with-openssl-dir=$(brew --prefix openssl@3)"
eval "$(rbenv init -zsh)" 
```

After updating, run `source ~/.zshrc` to reload your file and ensure the changes take place.  You can also run `brew config` and verify homebrew is using the freshly installed version on the `HOMEBREW_PREFIX` line.

### Step 5: Initialize rbenv
Run:
```
rbenv init 
``` 

You should follow the instructions that are logged. I've only seen to add `eval "$(rbenv init -)" ` to your `~/.zshrc` file (which we did in the step above) but if you see other instructions, please comment below. Then close and reopen your terminal window. Run the following to check your setup:

```
curl -fsSL https://github.com/rbenv/rbenv-installer/raw/main/bin/rbenv-doctor | bash 
```

If `rbenv` points to an old version of Homebrew like the image below, clean it up:

![2](https://github.com/danielfrance/Installing-Ruby-On-M1-Macs/assets/4622440/da7bcf39-1f1d-4c98-b0f2-59ce07de6300)

```
rm -rf /usr/local/bin/rbenv*
```

Run the rbenv-doctor script again to ensure everything is set up properly.

### Step 6: Install Ruby Versions
Install Ruby 3.1.2:
```
rbenv install 3.1.2
```
You should see an output like below:
![3](https://github.com/danielfrance/Installing-Ruby-On-M1-Macs/assets/4622440/fa46fca3-940d-4984-b660-4c8aa2ad9ee8)

For Ruby 3.2.2:
```
rbenv install 3.2.2
```

### Step 7: Set Ruby Version
Create a new directory and navigate to it in the terminal. Set the local Ruby version:

```
rbenv local 3.1.2
```

There is no output for the command above but you should see a .ruby-version file in the directory and can verify the Ruby version with `ruby -v`.
![4](https://github.com/danielfrance/Installing-Ruby-On-M1-Macs/assets/4622440/3343b3f8-6bbf-4e6a-94a1-84fd47903b81)

And as you can see, I have set different local Ruby versions on each directory 
![5](https://github.com/danielfrance/Installing-Ruby-On-M1-Macs/assets/4622440/0698617b-4bb2-4bbe-b1b6-ba21c1e56cbe)

To set a global Ruby version:
```
rbenv global <version>
```

### Conclusion
I mainly wrote it for myself in case I run into this again but hopefully someone gets something out of this.
