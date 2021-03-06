lsec2 [![Build Status](https://travis-ci.org/goldeneggg/lsec2.svg?branch=master)](https://travis-ci.org/goldeneggg/lsec2) [![Go Report Card](https://goreportcard.com/badge/github.com/goldeneggg/lsec2)](https://goreportcard.com/report/github.com/goldeneggg/lsec2) [![MIT License](http://img.shields.io/badge/license-MIT-lightgrey.svg)](https://github.com/goldeneggg/lsec2/blob/master/LICENSE)
==========
List view of aws ec2 instances

## Install

### Using Homebrew for OS X

```sh
$ brew tap goldeneggg/lsec2
$ brew install lsec2
```

### Or `go get`

```sh
$ go get -u github.com/goldeneggg/lsec2
```

### Or direct download

Download from [latest release](https://github.com/goldeneggg/lsec2/releases/latest)

## Configuration

### Set environment variables

2 variables are required

```sh
$ export AWS_ACCESS_KEY_ID=YOUR_ACCESS_KEY_ID
$ export AWS_SECRET_ACCESS_KEY=YOUR_SECRET_ACCESS_KEY
```

### Or create `~/.aws/credentials` file
Write your credential information in `~/.aws/credentials` file

* Write a `[default]` section
* Write `aws_access_key_id` in `[default]` section
* Write `aws_secret_access_key` in `[default]` section

```ini
[default]
aws_access_key_id = YOUR_ACCESS_KEY_ID
aws_secret_access_key = YOUR_SECRET_ACCESS_KEY
```

More information: [session \- Amazon Web Services \- Go SDK](http://docs.aws.amazon.com/sdk-for-go/api/aws/session/)

## Usage

```sh
# print all instances info by list view
$ lsec2
```

Result contains informations of instances as follows

* instance id
* private ip address
* public ip address
* instance type
* instance state
* tags

Show more detail information by typing `lsec2 -h`

### Assign region

You can use 3 patterns

* assign by `--region` option (top priority)

```sh
$ lsec2 --region ap-northeast-1
```

* set `AWS_REGION` environment

```sh
$ export AWS_REGION=ap-northeast-1
```

* set `AWS_SDK_LOAD_CONFIG` environment, and write `region` key in `~/.aws/config` file

```sh
$ export AWS_SDK_LOAD_CONFIG=true

$ vi ~/.aws/config
```
```ini
[default]
region = ap-northeast-1
```

Show more information from public AWS documents

* [SDK Configuration — Developer Guide](https://docs.aws.amazon.com/sdk-for-go/v1/developerguide/configuring-sdk.html)
* [Sessions — Developer Guide](http://docs.aws.amazon.com/sdk-for-go/v1/developerguide/sessions.html)

### Filter by tag

```sh
# filterd by a tag key-value separated by "="
$ lsec2 TagName1=tagvalue1

# filterd by some tags
$ lsec2 TagName1=tagvalue1 TagNameN=tagvalueN

# filterd by a tag multiple values separated by comma
$ lsec2 TagName1=tagvalue11,tagvalue12,tagvalue1N
```

### Options

```sh
# with header
$ lsec2 -H

# print only private IP address
$ lsec2 -p

# print state with color
# ("running" is green, "stopped" is red, and others are yellow)
$ lsec2 -c
```

## Tips

### With peco
[peco](https://github.com/peco/peco) is a very useful interactive filtering tool.

* Example: print instances => select instance => SSH to selected instance

```sh
# add function to your .bashrc or .bash_profile or other shell dotfile
$ vi YOUR_DOTFILE

lssh () {
  IP=$(lsec2 $@ | peco | awk -F "\t" '{print $2}')
  if [ $? -eq 0 -a "${IP}" != "" ]
  then
      ssh ${IP}
  fi
}


# load dotfile
$ source YOUR_DOTFILE

# shortcut "lsec2 OPTIONS TAG_FILTERS" => "ssh PRIVATE_IP"
$ lssh TagName1=tagvalue1
```

### With gat
[gat](https://github.com/goldeneggg/gat) is a file posting tool to various services like `cat` command.

* Example: print instances => share your slack

```sh
# shortcut "lsec2 OPTIONS TAG_FILTERS" => copy this results to your slack channel
$ lsec2 TagName1=tagvalue1 | gat slack
```

## Contribute
Please follow [Contributor's Guide](CONTRIBUTING.md)

## Contact

Bugs: [issues](https://github.com/goldeneggg/lsec2/issues)


## ChangeLog
[CHANGELOG](CHANGELOG.md) file for details.


## License

[LICENSE](LICENSE) file for details.

## Special Thanks
[@sugitak](https://github.com/sugitak)
