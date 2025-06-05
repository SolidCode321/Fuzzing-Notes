#1st 
#### Basic Usage
for single file
```
clamscan file.txt
```

or for a directory(recursively scans all files in directory)
```
clamscan /path/to/directory
```

#### Useful Flags
- Show only the infected files and delete them if found.
```
clamscan --infected --remove file.txt
```

- Show only infected result (no summary block at the end) - useful for parsing.
```
clamscan --infected ==no-summary smaple.txt
```

- Recursively scan folder and beep if virus is found (just for fun)
```
clamscan --recursive --bell ./samples/
```

#### Control output and Logging
- log scan output to a file
```
clamscan --log=logs/scan.log fuzzed_sample.txt
```

- Force all output to stdout(good for capturing in subprocess in python).
```
clamscan --stdout sample.txt
```

- verbose mode, shows everything being scanned.
```
clamscan -v sample.txt
```

#### Advanced/Batch Scanning
- Scan entire system except /proc.
```
clamscan --exclude-dir="^/proc" --recursive /
```

- Only scan .pdf files in a folder.
```
clamscan --include="\.pdf$" --recursive ./samples/
```

#### Performance/Limits
- Skip files larger than 5 MB (default is 25 MB).
```
clamscan --max-filesize=5M sample.pdf
```

- Total amount of data to scan from a file.
```
clamscan --max-scansize=20M sample/
```


### Update Signatures(Freshclam)
- Updates ClamAV's virus definitions.
```
sudo freshclam
```
