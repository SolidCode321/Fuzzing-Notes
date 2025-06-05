#1st
## Step 1: Setup ClamAV in the local environment

Run:
````
brew install clamav
sudo freshclam
````

Verify:
````
clamscan --version
````

If encountering error in `sudo freshclam`.
It is probably due to config file being missing, we can fix this by simply adding config files for clamav.

Copy the config files with this command:
```
sudo cp /opt/homebrew/etc/clamav/freshclam.conf.sample /opt/homebrew/etc/clamav/freshclam.conf
```

Edit the config file with nano.
```
sudo nano /opt/homebrew/etc/clamav/freshclam.conf
```

and comment the line below
```
Example
```

if modifying the config file doesn't work due to
```
ERROR: Can't create freshclam.dat in /opt/homebrew/var/lib/clamav

Hint: The database directory must be writable for UID 82 or GID 82

ERROR: Failed to save freshclam.dat!

WARNING: Failed to create a new freshclam.dat!

ERROR: initialize: libfreshclam init failed.

ERROR: Initialization error!
```

Simple, now you just have to give permission to your pc to write to the config file
```
sudo chown -R $(whoami) /opt/homebrew/var/lib/clamav
```

```
freshclam
```


## Step 2: Setting up the python environment

#### Prerequisites
1. Python should be installed
```
brew install python3
```
2. pip should be functional


#### Setup virtual environment
```
python3 -m venv fuzz-env
source fuzz-env/bin/activate
```

#### Install the required libraries
```
pip install --upgrade pip
pip install fuzzingbook notebook
```

