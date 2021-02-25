# Using Packer, create a vagrant virtualbox base ubuntu lts box - README.md with instructions (github repo)
## Intro
This manual is dedicated to create vagrant virtualbox base ubuntu lts box, start it and make some tests.
## Requirements
* Operating system: Linux / Mac OSX of recent version
* Bash/Zsh interpreter basic usage skills
* Oracle Virtualbox recent version installed
* Hashicorp packer recent version installed
* Hashicorp vagrant recent version installed
* git installed
* rbenv installed
## Preparation 
* Clone git repository. Run using Bash/Zsh interpreter 
```bash
git clone https://github.com/kikitux/packer_bionic64
```
* Change folder to packer_bionic64 
```bash
cd packer_bionic64
```
## Build
* In the same folder you were before run 
```bash
packer build template.json
```
* Add the built image box to Virtualbox 
```bash
vagrant box add --force --name bionic64  bionic64-vbox.box
```
## Test OS image (optional)
Do tests to ensure if Operating system image is functioning as expected
* Install ruby 2.6.6 using rbenv
```bash
rbenv install -l
rbenv install 2.6.6
rbenv local 2.6.6 
```
* Install kitchen-test locally
```bash
bundle install --path vendor/bundle
```
* Perform tests
```bash
bundle exec kitchen test
```
