# Using Packer, create a vagrant virtualbox base ubuntu lts box - README.md with instructions (github repo)

## Intro
This manual is dedicated to create vagrant virtualbox base ubuntu lts box. Tested on Mac OS X.

## Requirements
- Oracle Virtualbox recent version installed
[VirtualBox installation manual](https://www.virtualbox.org/manual/ch01.html#intro-installing)

- Hashicorp packer recent version installed
[Packer installation manual](https://learn.hashicorp.com/tutorials/packer/getting-started-install)

- Hashicorp vagrant recent version installed
[Vagrant installation manual](https://learn.hashicorp.com/tutorials/vagrant/getting-started-install)

- git installed
[Git installation manual](https://git-scm.com/download/mac)

- rbenv installed
[Rbenv installation manual](https://github.com/rbenv/rbenv#installation)

## Preparation 
- Clone git repository. 

```bash
git clone https://github.com/antonakv/packer-ob-1
```

Expected command output looks like this:

```bash
Cloning into 'packer-ob-1'...
remote: Enumerating objects: 12, done.
remote: Counting objects: 100% (12/12), done.
remote: Compressing objects: 100% (12/12), done.
remote: Total 12 (delta 1), reused 3 (delta 0), pack-reused 0
Receiving objects: 100% (12/12), done.
Resolving deltas: 100% (1/1), done.
```

- Change folder to packer_bionic64 

```bash
cd packer_bionic64
```

## Build
- In the same folder you were before run 
- 
```bash
packer build template.json
```

Sample result

```bash
bionic64-vbox: output will be in this color.

==> bionic64-vbox: Retrieving Guest additions
==> bionic64-vbox: Trying /Applications/VirtualBox.app/Contents/MacOS/VBoxGuestAdditions.iso
==> bionic64-vbox: Trying /Applications/VirtualBox.app/Contents/MacOS/VBoxGuestAdditions.iso
==> bionic64-vbox: /Applications/VirtualBox.app/Contents/MacOS/VBoxGuestAdditions.iso => /Applications/VirtualBox.app/Contents/MacOS/VBoxGuestAdditions.iso
==> bionic64-vbox: Retrieving ISO
==> bionic64-vbox: Trying https://cdimage.ubuntu.com/releases/18.04/release/ubuntu-18.04.5-server-amd64.iso
==> bionic64-vbox: Trying https://cdimage.ubuntu.com/releases/18.04/release/ubuntu-18.04.5-server-amd64.iso?checksum=sha256%3A8c5fc24894394035402f66f3824beb7234b757dd2b5531379cb310cedfdf0996
    bionic64-vbox: ubuntu-18.04.5-server-amd64.iso 951.00 MiB / 951.00 MiB [===================================================================] 100.00% 2m17s
==> bionic64-vbox: https://cdimage.ubuntu.com/releases/18.04/release/ubuntu-18.04.5-server-amd64.iso?checksum=sha256%3A8c5fc24894394035402f66f3824beb7234b757dd2b5531379cb310cedfdf0996 => /Users/aakulov/Documents/hashicorp/test/packer-ob-1/packer_cache/a37af95ab12e665ba168128cde2f3662740b21a2.iso
==> bionic64-vbox: Starting HTTP server on port 8072
==> bionic64-vbox: Creating virtual machine...
==> bionic64-vbox: Creating hard drive...
==> bionic64-vbox: Mounting ISOs...
    bionic64-vbox: Mounting boot ISO...

[Skipped some messages]

==> bionic64-vbox (vagrant): Creating a dummy Vagrant box to ensure the host system can create one correctly
==> bionic64-vbox (vagrant): Creating Vagrant box for 'virtualbox' provider
    bionic64-vbox (vagrant): Copying from artifact: output-bionic64-vbox/bionic64-vbox-disk001.vmdk
    bionic64-vbox (vagrant): Copying from artifact: output-bionic64-vbox/bionic64-vbox.ovf
    bionic64-vbox (vagrant): Renaming the OVF to box.ovf...
    bionic64-vbox (vagrant): Compressing: Vagrantfile
    bionic64-vbox (vagrant): Compressing: bionic64-vbox-disk001.vmdk
    bionic64-vbox (vagrant): Compressing: box.ovf
    bionic64-vbox (vagrant): Compressing: metadata.json
Build 'bionic64-vbox' finished after 11 minutes 39 seconds.

==> Wait completed after 11 minutes 39 seconds

==> Builds finished. The artifacts of successful builds are:
--> bionic64-vbox: VM files in directory: output-bionic64-vbox
--> bionic64-vbox: 'virtualbox' provider box: bionic64-vbox.box
```

- Add the built image box to Virtualbox

```bash
vagrant box add --force --name bionic64  bionic64-vbox.box
```

Sample result
```
==> box: Box file was not detected as metadata. Adding it directly...
==> box: Adding box 'bionic64' (v0) for provider: 
    box: Unpacking necessary files from: file:///Users/user/Documents/packer-ob-1/bionic64-vbox.box
==> box: Successfully added box 'bionic64' (v0) for 'virtualbox'!
```

## Test OS image (optional)
Do tests to ensure if Operating system image is functioning as expected

- Install ruby 2.6.6 using rbenv

```bash
rbenv install -l
rbenv install 2.6.6
rbenv local 2.6.6 
```

Sample output

```bash
➜  packer-ob-1 git:(master) rbenv install -l
2.5.8
2.6.6
2.7.2
3.0.0
jruby-9.2.14.0
mruby-2.1.2
rbx-5.0
truffleruby-21.0.0
truffleruby+graalvm-21.0.0

Only latest stable releases for each Ruby implementation are shown.
Use 'rbenv install --list-all / -L' to show all local versions.

➜  packer-ob-1 git:(master) rbenv install 2.6.6
rbenv: /Users/aakulov/.rbenv/versions/2.6.6 already exists
continue with installation? (y/N) y
Downloading ruby-2.6.6.tar.bz2...
-> https://cache.ruby-lang.org/pub/ruby/2.6/ruby-2.6.6.tar.bz2
Installing ruby-2.6.6...
ruby-build: using readline from homebrew
Installed ruby-2.6.6 to /Users/aakulov/.rbenv/versions/2.6.6

➜  packer-ob-1 git:(master) rbenv local 2.6.6 
➜  packer-ob-1 git:(master) 
```

- Install kitchen-test locally
```bash
bundle install --path vendor/bundle
```

Expected result

```
➜  packer-ob-1 git:(master) bundle install --path vendor/bundle
Fetching gem metadata from https://rubygems.org/........
Fetching gem metadata from https://rubygems.org/.
Resolving dependencies....
Fetching concurrent-ruby 1.1.8
Installing concurrent-ruby 1.1.8
Fetching i18n 1.8.9
Installing i18n 1.8.9
Fetching minitest 5.14.4

[Skipped some messages]

Installing kitchen-inspec 2.4.1
Fetching kitchen-vagrant 1.8.0
Installing kitchen-vagrant 1.8.0
Bundle complete! 4 Gemfile dependencies, 180 gems now installed.
Bundled gems are installed into `./vendor/bundle`
```
- Perform tests
```bash
bundle exec kitchen test
```

Expected command output
```bash
-----> Starting Test Kitchen (v2.10.0)
-----> Cleaning up any prior instances of <default-vbox-bionic64>
-----> Destroying <default-vbox-bionic64>...
       Finished destroying <default-vbox-bionic64> (0m0.00s).
-----> Testing <default-vbox-bionic64>
-----> Creating <default-vbox-bionic64>...
       Bringing machine 'default' up with 'virtualbox' provider...
       ==> default: Importing base box 'bionic64'...

[Skipped some messages]

  System Package thin-provisioning-tools
     ✔  is expected to be installed
  System Package vim
     ✔  is expected to be installed
  System Package git
     ✔  is expected to be installed
  System Package jq
     ✔  is expected to be installed
  System Package curl
     ✔  is expected to be installed
  System Package wget
     ✔  is expected to be installed
  System Package language-pack-en
     ✔  is expected to be installed

Test Summary: 7 successful, 0 failures, 0 skipped
       Finished verifying <default-vbox-bionic64> (0m2.20s).
-----> Destroying <default-vbox-bionic64>...
       ==> default: Forcing shutdown of VM...
       ==> default: Destroying VM and associated drives...
       Vagrant instance <default-vbox-bionic64> destroyed.
       Finished destroying <default-vbox-bionic64> (0m5.30s).
       Finished testing <default-vbox-bionic64> (0m49.54s).
-----> Test Kitchen is finished. (0m51.31s)

```
