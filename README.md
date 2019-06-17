# summon-provider-ccp
CyberArk CCP provider for [Summon](https://github.com/cyberark/summon).

---

### **Status**: Alpha

#### **Warning: Naming and APIs are still subject to breaking changes! Do not use in production**

---
## Install

Pre-built binaries and packages are available from GitHub releases
[here](https://github.com/AndrewCopeland/summon-provider-ccp/releases).

### Homebrew

Currently not supported

### Linux (Debian and Red Hat flavors)

Currently not supported

### Auto Install

Currently not supported


### Manual Install
Otherwise, download the [latest release](https://github.com/AndrewCopeland/summon-provider-ccp/releases) and extract it to the directory `/usr/local/lib/summon`.

## Usage in isolation

Give summon-provider-ccp a variable name and it will fetch it for you and print the value to stdout.

```sh-session
$ summon-provider-ccp AppID=myApplication&Safe=appSafe&Object=the-identifying-name-of-the-object/Password
my-application-password
```

### Flags

```
Usage of summon-provider-ccp:
  -h, --help
	show help (default: false)
  -v, --verbose
	be verbose (default: false)
```

## Usage as a provider for Summon

[Summon](https://github.com/cyberark/summon/) is a command-line tool that reads a file in secrets.yml format and injects secrets as environment variables into any process. Once the process exits, the secrets are gone.

*Example*

As an example let's use the `env` command:

Following installation, define your keys in a `secrets.yml` file

```yml
AWS_ACCESS_KEY_ID: !var AppID=myApplication&Safe=appSafe&Object=my-app-aws-access-key/AWSAccessKeyID
AWS_SECRET_ACCESS_KEY: !var AppID=myApplication&Safe=appSafe&Object=my-app-aws-access-key/Content
```

By default, summon will look for `secrets.yml` in the directory it is called from and export the secret values to the environment of the command it wraps.

Wrap the `env` in summon:

```sh
$ summon --provider summon-provider-ccp env
...
AWS_ACCESS_KEY_ID=AKIAJS34242K1123J3K43
AWS_SECRET_ACCESS_KEY=A23MSKSKSJASHDIWM
...
```

`summon` resolves the entries in secrets.yml with the conjur provider and makes the secret values available to the environment of the command `env`.

## Configuration

* Set the Environment Variable:
  * `CYBERARK_CCP_URL`: The url to the CCP server

---
