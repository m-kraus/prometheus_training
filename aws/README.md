# Lab sessions on AWS

## Preparations on AWS

- Create a key pair named "prometheus_training" and place the provided key within this directory and do a ```chmod 400 prometheus_training.pem```

- Create a yault with your AWS secrets using ```ansible-vault create aws_keys.yml```. Once open enter something like

  ```
  aws_access_key: MY_ACCESS_KEY
  aws_secret_key: MY_SECRET_KEY
  ```

## Deployment

- Rebuild index page from README: ```pandoc -s -c pandoc.css ../README.md -o index.html``` (obviously you'll need ```pandoc``` for this task to be installed on your machine)

- Run a playbook ```ansible-playbook --ask-vault-pass aws_spinup.yml``` (obviously you'll need ```ansible``` to be installed on your machine)

- To reprovision already spun up EC2 instances run ```ansible-playbook --ask-vault-pass aws_reprovision.yml```

- To stop, start or terminate your instances run ```ansible-playbook --ask-vault-pass aws_stop.yml``` ```ansible-playbook --ask-vault-pass aws_start.yml``` or ```ansible-playbook --ask-vault-pass aws_terminate.yml```

## Usage

The training files are located in the ```ec2-user```s home directory in ```~/prometheus_training```

```tmux``` is installed on the EC2 instances to ease ssh access. Some useful commands are:

- ```tmux``` creates a new session

- ```tmux a``` (re-)attaches to a running session

- Once inside tmux ```ctrl-b %``` creates a vertical split, ```ctrl-b "``` creates a horizontal split

- ```ctrl-b o``` swap panes, you can also use ```crtl-b ARROW_KEYS```