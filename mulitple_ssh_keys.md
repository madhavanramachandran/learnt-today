- Create a new SSH key for the user with email_id as identifier
 > ```ssh-keygen -t rsa -C "maddy.srv@gmail.com"```

- Add the newly generated key to the ssh agent:
 >```ssh-add ~/.ssh/{{file_name}}```
 This is the file name of the private key.

- create a config file ~/.ssh folder and add the identiy
```
#maddy.srv
Host github-maddy
	HostName github.com
	IdentityFile ~/.ssh/maddy_srv
	IdentitiesOnly yes
```

- Validate the connection:
 ```ssh -T git@github-maddy```

- To set the remote git url in my local repo
  > `git remote set-url origin git@github-maddy:madhavanramachandran/text_files.git`
  - Then we can able to push the files to this repo without any more prompts.
#


References:

- https://gist.github.com/developius/c81f021eb5c5916013dc
- https://gist.github.com/jexchan/2351996
- https://gist.github.com/oanhnn/80a89405ab9023894df7
- https://help.github.com/en/articles/error-permission-denied-publickey