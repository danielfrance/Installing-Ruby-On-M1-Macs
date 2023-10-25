# Installing-Ruby-On-M1-Macs
A quick installation guide for navigating installation of Ruby on an M1 Mac

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
 
If this resonates with your experience, you are in the right place.

## Prerequisites
For the purpose of this tutorial, we assume that you have Homebrew installed. If not, no worries, we will cover that as well. Start by upgrading Homebrew:

```brew upgrade```

This command might return an error like:
```
Error: Cannot install in Homebrew on ARM processor in Intel default prefix (/usr/local)!
...```

This error is a common pitfall and a major reason for installation failures.

## Step 1: Update Xcode
Ensure Xcode is up to date:

```xcode-select --install```

## Step 2: Install Homebrew
1. Navigate to [Homebrew's website](brew.sh)	
2. Click on the highlighted option for M1 Macs, which should direct you to their GitHub page.
![homebrew](https://github.com/danielfrance/Installing-Ruby-On-M1-Macs/assets/4622440/6c3b33a1-7ed7-4308-b8b5-cc642b85457d)
4. Under assets, choose your preferred installer. The Homebrew-****.pkg option is recommended for its simplicity.
5. Run the installer and disregard the pop-up command about the Homebrew path for now. We will address this in the next steps.

## Step 3: Install [rbenv](https://github.com/rbenv/rbenv) and [ruby-build](https://github.com/rbenv/ruby-build/wiki#suggested-build-environment)
Now, install rbenv and other necessary tools:

```brew install rbenv ruby-build rbenv-gemset rbenv-vars```
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

This adjusts the Homebrew path for M1 Macs and configures the shell for rbenv. Ensure your `~/.zshrc` includes:

```
export PATH=/opt/homebrew/bin:$PATH

# For Ruby 2.x - 3.0
export RUBY_CONFIGURE_OPTS="--with-openssl-dir=$(brew --prefix openssl@1.1)"
# ************* OR **************
# For Ruby 3.1 and above
export RUBY_CONFIGURE_OPTS="--with-openssl-dir=$(brew --prefix openssl@3)"
eval "$(rbenv init -zsh)" 
```

After updating, run `source ~/.zshrc`

### Step 5: Initialize rbenv
Run:
```
rbenv init 
``` 

Then close and reopen your terminal window. Run the following to check your setup:

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
This guide is designed to streamline the Ruby installation process on M1 Macs, ensuring you have a smooth experience. Feel free to reach out with questions or updates. Happy coding!
