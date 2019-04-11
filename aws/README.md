1. Create a key pair named "prometheus_training" and place the provate key within this directory, do ```chmod 400 prometheus_training.pem```

2. Create a yault with your AWS secrets using ```ansible-vault create aws_keys.yml```. Once open enter something like

```
aws_access_key: MY_ACCESS_KEY
aws_secret_key: MY_SECRET_KEY
```
3. Rebuild index page from README: ```pandoc -s -c pandoc.css ../README.md -o index.html```

4. Run a playbook ```ansible-playbook --ask-vault-pass aws_spinup.yml```