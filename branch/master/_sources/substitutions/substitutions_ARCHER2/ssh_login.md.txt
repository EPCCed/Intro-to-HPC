## Connecting to ARCHER2

### Sign up for an account on ARCHER2 through SAFE

1. [Login to SAFE](https://safe.epcc.ed.ac.uk)
2. Go to the Menu "Login accounts" and select "Request login account"
3. Choose the `TODO` project “Choose Project for Machine Account” box and click "Next"
4. On the next page, the ARCHER2 system should be selected. Click "Next"
5. Enter the username you would prefer to use on ARCHER2. Every username must be unique, so if your chosen name is taken, you will need to choose another

Now you have to wait for the course organiser to accept your request to register. When this has happened, your account will be created on ARCHER2.
Once this has been done, you should be sent an email. _If you have not received an email but believe that your account should have been activated, check your account status in SAFE which will also show when the account has been activated._ You can then pick up your one shot initial password for ARCHER2 from your SAFE account.


### Generate an SSH key pair and upload it to SAFE

In addition to your password, you will need an SSH key pair to access ARCHER2. There is useful guidance on how
to generate SSH key pairs in [the ARCHER2 documentation](https://docs.archer2.ac.uk/user-guide/connecting/#ssh-key-pairs).

Once you have generated your key pair, you need to add the public part to your ARCHER2 account in SAFE:

1. [Login to SAFE](https://safe.epcc.ed.ac.uk)
2. Go to the Menu “Login accounts” and select the ARCHER2 account you want to add the SSH key to
3. On the subsequent Login account details page click the “Add Credential” button
4. Select “SSH public key” as the Credential Type and click “Next”
5. Either copy and paste the public part of your SSH key into the “SSH Public key” box or use the button to select the public key file on your computer.
6. Click “Add” to associate the public SSH key part with your account

The public SSH key part will now be added to your login account on the ARCHER2 system.


### Log into ARCHER2

You should now be able to log into ARCHER2 by following the [login instructions in the ARCHER2 documentation](https://docs.archer2.ac.uk/user-guide/connecting/#ssh-clients).

E.g:

```bash
ssh username@login.archer2.ac.uk
```


`````{note}

If you are using ARCHER2 before you download the files you should move to the ``/work`` filesystem:

```bash
cd /work/[project code]/[group code]/[username]
```

The `/work` filesystem is a high performance parallel file system that can be accessed by both the frontend login nodes and the compute nodes. All jobs on ARCHER2 should be run from the `/work` file system. ARCHER2 compute nodes cannot access the `/home` file system at all. Any jobs attempting to use `/home` will fail with an error. 

For more information the ARCHER2 documentation: [https://docs.archer2.ac.uk/user-guide/io/#using-the-archer2-file-systems](https://docs.archer2.ac.uk/user-guide/io/#using-the-archer2-file-systems)

``````