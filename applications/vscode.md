# [Visual Studio Code](https://code.visualstudio.com/)

## Remote Keep asking for passphrase of SSH key

[Setting up the SSH Agent](https://code.visualstudio.com/docs/remote/troubleshooting#_setting-up-the-ssh-agent)

{% tabs %}
{% tab title="Windows" %}

To enable SSH Agent automatically on Windows, start a local Administrator PowerShell and run the following commands:
**Make sure you're running as an Administrator**

```bash
Set-Service ssh-agent -StartupType Automatic
Start-Service ssh-agent
Get-Service ssh-agent
```

Now the agent will be started automatically on login.

{% endtab %}

{% tab title="Linux" %}

To start the SSH Agent in the background, run:

```bash
eval "$(ssh-agent -s)"
```

To start the SSH Agent automatically on login, add these lines to your ~/.bash_profile:

```bash
if [ -z "$SSH_AUTH_SOCK" ]; then
   # Check for a currently running instance of the agent
   RUNNING_AGENT="`ps -ax | grep 'ssh-agent -s' | grep -v grep | wc -l | tr -d '[:space:]'`"
   if [ "$RUNNING_AGENT" = "0" ]; then
        # Launch a new instance of the agent
        ssh-agent -s &> .ssh/ssh-agent
   fi
   eval `cat .ssh/ssh-agent`
fi
```

{% endtab %}

{% tab title="Mac" %}

The agent should be running by default on macOS.

{% endtab %}
{% endtabs %}
